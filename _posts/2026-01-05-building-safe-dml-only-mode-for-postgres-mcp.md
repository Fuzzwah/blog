---
title: Building Safe DML Only Mode For Postgres MCP
date: 2026-01-05 10:00:00 +1100
author: Fuzzwah
description: How I added a safety net to postgres-mcp after an AI agent decided to "help" by rewriting my database schema.
---

So I gave an AI agent unrestricted access to my database and it went ahead and restructured my schema. Surprise, surprise. Let me tell you how that little disaster led me to build a whole new access mode for [postgres-mcp](https://github.com/crystaldba/postgres-mcp).

## The Problem: Two Modes Aren't Enough

The postgres-mcp server shipped with two access modes: **UNRESTRICTED** (full database access, great for dev, terrifying for anything else) and **RESTRICTED** (read-only, safe but useless when you actually need to write data). What was missing was the middle ground — let the agent insert records, update statuses, delete old data, but for the love of god don't let it restructure the entire database.

That missing middle ground is what I built: **DML_ONLY mode**.

## The Wake-Up Call: Django vs. Direct Schema Changes

Here's what actually happened. I was working on a Django project where the database schema is managed entirely through Django's migration system. Standard stuff:

```python
# models.py
class User(models.Model):
    name = models.CharField(max_length=100)
    email = models.EmailField()
    # Schema changes happen through migrations
```

```bash
# The proper way to add a field
python manage.py makemigrations
python manage.py migrate
```

Everything was working perfectly until I gave a coding agent access with UNRESTRICTED mode. The agent was helping me with some data updates when it decided to be "helpful" and added a new field directly:

```sql
ALTER TABLE users ADD COLUMN phone_number VARCHAR(20);
```

This completely bypassed Django's migration system. Now I had a database schema that didn't match my models, no migration file tracking the change, no way to reproduce it on other environments, teammates pulling code that expected a field that didn't exist in their local databases, and production deployments that would fail or behave inconsistently. Bloody brilliant.

The agent thought it was helping. Instead, it created a mess that took hours to untangle — manually creating a migration to match the schema change, rolling back the ALTER on dev, applying the proper migration, and documenting what happened so we wouldn't repeat it.

What the agent should have been able to do is INSERT new records, UPDATE existing rows, and DELETE invalid data. What it absolutely should not have been able to do is ALTER table structure, CREATE new tables, or DROP columns. In any framework-managed database environment (Django, Rails, Entity Framework, etc.), schema changes need to flow through the framework's migration system, not through direct DDL. DML_ONLY mode enforces that boundary.

## Planning the Implementation

I started by creating a detailed implementation plan. The key insight was to use pglast, a PostgreSQL SQL parser for Python, to analyse SQL abstract syntax trees (ASTs) before execution.

The requirements were straightforward: allow SELECT, INSERT, UPDATE, DELETE, EXPLAIN, and SHOW. Block CREATE, ALTER, DROP, TRUNCATE, VACUUM, and all other DDL operations.

## Building the DmlOnlySqlDriver

The implementation centred around a new `DmlOnlySqlDriver` class that wraps the existing `SqlDriver`. Rather than reinventing the wheel, I inherited from `SafeSqlDriver`'s battle-tested components — reusing the entire `ALLOWED_FUNCTIONS` list (100+ PostgreSQL functions), extending `ALLOWED_NODE_TYPES` with DML-specific nodes, and mirroring the validation architecture.

Validation happens in two phases. First, statement-level validation:

```python
ALLOWED_STMT_TYPES = {
    SelectStmt, InsertStmt, UpdateStmt, DeleteStmt,
    ExplainStmt, VariableShowStmt
}
```

Then node-level validation, which recursively checks every node in the SQL AST to make sure no disallowed operations sneak through in subqueries or CTEs.

One thing that needed special attention was PostgreSQL's INSERT ... ON CONFLICT (UPSERT). I had to add three additional node types — `OnConflictClause`, `InferClause`, and `IndexElem` — to support queries like:

```sql
INSERT INTO users (id, name, email)
VALUES (1, 'John Doe', 'john@example.com')
ON CONFLICT (id) DO UPDATE SET name = EXCLUDED.name
```

## The Code Review: Learning from My Own Mistakes

After pushing my initial implementation (with all 46 tests passing!), I did a thorough code review of my own work and found 6 critical issues. I reckon this is one of those things where slowing down saves you a world of pain later.

**The RawStmt validation bug** was the scariest one. I was validating the `RawStmt` wrapper node instead of the inner statement:

```python
# WRONG: Validates the wrapper
self._validate_node(stmt)

# RIGHT: Validates the actual statement
if isinstance(stmt, RawStmt):
    self._validate_node(stmt.stmt)  # Validate inner statement
```

**Generic error messages** were another problem. Going from "Error validating query: SELECT * FROM users" to "Statement type CreateStmt not allowed in DML_ONLY mode. Only SELECT, INSERT, UPDATE, DELETE, EXPLAIN, and SHOW are permitted" makes a massive difference for debugging.

**Function validation safety** needed tightening — using `hasattr(node, "funcname")` could crash on unexpected node shapes, so I switched to proper type checking:

```python
# WRONG: Assumes funcname exists
if hasattr(node, "funcname") and node.funcname:
    func_name = ".".join([str(n.sval) for n in node.funcname])

# RIGHT: Check type first
if isinstance(node, FuncCall):
    func_name = ".".join([str(n.sval) for n in node.funcname]).lower()
```

I also changed timeout errors from `ValueError` to `TimeoutError` so callers can distinguish query timeouts from validation failures.

**The WHERE clause debate** was the most interesting one. Initially I allowed UPDATE and DELETE without WHERE clauses, thinking "users might legitimately need to modify all rows." But then I realised: this is the number one cause of accidental data loss.

```sql
-- Whoops, forgot the WHERE clause!
DELETE FROM users  -- Deletes EVERYTHING
```

So I added a critical safety feature:

```python
if isinstance(stmt.stmt, (UpdateStmt, DeleteStmt)):
    if not stmt.stmt.whereClause:
        raise ValueError(
            f"{stmt_type} statements require a WHERE clause in DML_ONLY mode. "
            "This prevents accidental deletion or modification of all rows."
        )
```

Better to block a legitimate edge case than let an agent accidentally nuke your data.

## Testing

The final implementation includes 48 unit tests covering allowed DML operations, blocked DDL operations, complex queries (CTEs, subqueries, JOINs), error handling (SQL injection, invalid syntax, timeouts), and WHERE clause requirements. Plus 7 integration tests for access mode selection. All existing tests continue to pass — no regression introduced. It might seem like overkill, but they caught issues with UPSERT, error messages, and type checking that I would have missed otherwise.

## Using It

You can fire it up with:

```bash
mcp-server-postgres postgres://user:pass@localhost/dbname \
  --access-mode dml_only \
  --query-timeout 30
```

It supports complex queries including CTEs, JOINs, subqueries, and UPSERT. The WHERE clause requirement prevents accidental mass deletions. Specific error messages guide you to fix issues quickly. And it introduces zero breaking changes to existing code.

## What I Learnt

1. **Reuse battle-tested code** — building on top of `SafeSqlDriver` saved weeks of work and countless security bugs
2. **Code review yourself** — taking time to review my own code before submitting revealed 6 critical issues
3. **Fail safe, not silent** — better to block a legitimate edge case than allow accidental data destruction
4. **Test exhaustively** — 48 tests caught issues I would have missed
5. **Error messages matter** — specific error messages turn frustrating debugging sessions into quick fixes

There's potential for future enhancements like an optional flag to allow UPDATE/DELETE without WHERE for legitimate use cases, row-level security integration, query cost estimation, and rate limiting. But for now, DML_ONLY mode sorts out the core problem: giving AI agents the access they need without the access that could destroy your data.

The complete implementation is available on my fork of the [postgres-mcp repository](https://github.com/Fuzzwah/postgres-mcp/tree/feature/dml-only-mode). All 55 tests passing, code review complete, ready for merge. Stay tuned for more on how this fits into my broader agentic coding workflow.
