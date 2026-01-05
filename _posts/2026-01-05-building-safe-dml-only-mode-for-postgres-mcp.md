# Building a Safe Data Manipulation Layer for postgres-mcp: The DML_ONLY Mode Story

## The Problem: A Missing Middle Ground

When I started working with [postgres-mcp](https://github.com/crystaldba/postgres-mcp), I quickly realized there was a gap in the access control system. The server offered two modes:

- **UNRESTRICTED**: Full database access - perfect for development, but too dangerous for production agents
- **RESTRICTED**: Read-only mode - safe, but useless when you need to write data

But what if you need an AI agent that can insert records, update statuses, or delete outdated data—without giving it the keys to restructure your entire database schema?

That's the use case that led me to build **DML_ONLY mode**: a new access level that allows data manipulation while blocking schema changes.

## The Journey Begins: Planning the Implementation

I started by creating a detailed implementation plan in `DML_ONLY_MODE_IMPLEMENTATION.md`. The key insight was to leverage pglast, a PostgreSQL SQL parser for Python, to analyze SQL abstract syntax trees (ASTs) before execution.

The requirements were clear:
- ✅ Allow: SELECT, INSERT, UPDATE, DELETE, EXPLAIN, SHOW
- ❌ Block: CREATE, ALTER, DROP, TRUNCATE, VACUUM, and other DDL operations

## Building the DmlOnlySqlDriver

The implementation centered around a new `DmlOnlySqlDriver` class that wraps the existing `SqlDriver`. Here's the approach:

### 1. Reuse Smart Patterns

Rather than reinventing the wheel, I inherited from `SafeSqlDriver`'s battle-tested components:
- Reused the entire `ALLOWED_FUNCTIONS` list (100+ PostgreSQL functions)
- Extended `ALLOWED_NODE_TYPES` with DML-specific nodes
- Mirrored the validation architecture

### 2. Two-Level Validation

The validation happens in two phases:

**Statement-Level Validation**:
```python
ALLOWED_STMT_TYPES = {
    SelectStmt, InsertStmt, UpdateStmt, DeleteStmt,
    ExplainStmt, VariableShowStmt
}
```

**Node-Level Validation**:
Recursively validates every node in the SQL AST to ensure no disallowed operations sneak through in subqueries or CTEs.

### 3. Handle UPSERT Operations

PostgreSQL's INSERT ... ON CONFLICT (UPSERT) required special attention. I had to add three additional node types:
- `OnConflictClause` - For the ON CONFLICT clause itself
- `InferClause` - For conflict target specification
- `IndexElem` - For index element specification in conflict targets

This allowed queries like:
```sql
INSERT INTO users (id, name, email)
VALUES (1, 'John Doe', 'john@example.com')
ON CONFLICT (id) DO UPDATE SET name = EXCLUDED.name
```

## The Code Review: Learning from Mistakes

After pushing my initial implementation (with all 46 tests passing!), I did a thorough code review and found 6 critical issues:

### Issue #1: The RawStmt Validation Bug 🐛

**The Problem**: I was validating the `RawStmt` wrapper node instead of the inner statement.

```python
# WRONG: Validates the wrapper
self._validate_node(stmt)

# RIGHT: Validates the actual statement
if isinstance(stmt, RawStmt):
    self._validate_node(stmt.stmt)  # Validate inner statement
```

### Issue #2: Generic Error Messages 😕

**Before**:
```
Error validating query: SELECT * FROM users
```

**After**:
```
Statement type CreateStmt not allowed in DML_ONLY mode. 
Only SELECT, INSERT, UPDATE, DELETE, EXPLAIN, and SHOW are permitted.
```

Much better for debugging!

### Issue #3: Function Validation Safety

**The Problem**: Using `hasattr(node, "funcname")` could crash on unexpected node shapes.

**The Fix**: Use proper type checking before accessing attributes:
```python
# WRONG: Assumes funcname exists
if hasattr(node, "funcname") and node.funcname:
    func_name = ".".join([str(n.sval) for n in node.funcname])

# RIGHT: Check type first
if isinstance(node, FuncCall):
    func_name = ".".join([str(n.sval) for n in node.funcname]).lower()
```

### Issue #4: Timeout Error Types

Changed timeout errors from `ValueError` to `TimeoutError` so callers can distinguish query timeouts from validation failures.

### Issue #5: The WHERE Clause Debate 🤔

Initially, I allowed UPDATE and DELETE without WHERE clauses, thinking "users might legitimately need to modify all rows."

But then I realized: **This is the #1 cause of accidental data loss.**

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

Now users get a clear error instead of accidentally nuking their database.

## Testing: Comprehensive Coverage

The final implementation includes **48 unit tests**:

- **13 tests** for allowed DML operations (INSERT, UPDATE, DELETE, UPSERT)
- **18 tests** for blocked DDL operations (CREATE, ALTER, DROP, etc.)
- **6 tests** for complex queries (CTEs, subqueries, JOINs)
- **9 tests** for error handling (SQL injection, invalid syntax, timeouts)
- **2 tests** for WHERE clause requirement

Plus **7 integration tests** for access mode selection.

All existing tests continue to pass—no regression introduced.

## Real-World Examples

Here's what DML_ONLY mode enables:

### ✅ Data Migration Scripts
```sql
-- Import data from CSV
INSERT INTO users (name, email, created_at)
SELECT name, email, NOW() FROM staging_users;

-- Clean up staging
DELETE FROM staging_users WHERE imported = true;
```

### ✅ Application Agents
```sql
-- Update order status
UPDATE orders 
SET status = 'shipped', shipped_at = NOW()
WHERE order_id = 12345;

-- Insert audit log
INSERT INTO audit_logs (action, user_id, timestamp)
VALUES ('ORDER_SHIPPED', 42, NOW());
```

### ❌ Can't Break Things
```sql
-- These are blocked:
DROP TABLE users;           -- ❌ DDL blocked
TRUNCATE TABLE orders;      -- ❌ DDL blocked
CREATE INDEX idx_email;     -- ❌ DDL blocked
ALTER TABLE users ADD...;   -- ❌ DDL blocked

-- These require WHERE:
DELETE FROM users;          -- ❌ Missing WHERE
UPDATE orders SET status;   -- ❌ Missing WHERE
```

## Key Takeaways

1. **Reuse battle-tested code**: Building on top of `SafeSqlDriver` saved weeks of work and countless security bugs.

2. **Code review yourself**: Taking time to review my own code before submitting revealed 6 critical issues.

3. **Fail safe, not silent**: Better to block a legitimate edge case than allow accidental data destruction.

4. **Test exhaustively**: 48 tests might seem like overkill, but they caught issues with UPSERT, error messages, and type checking.

5. **Error messages matter**: Specific error messages turn frustrating debugging sessions into quick fixes.

## The Result

The DML_ONLY mode is now ready for production use. It provides:

- **Safety**: WHERE clause requirements prevent accidental mass deletions
- **Clarity**: Specific error messages guide users to fix issues
- **Flexibility**: Supports complex queries (CTEs, JOINs, subqueries, UPSERT)
- **Performance**: Optional timeout configuration for long-running queries
- **Compatibility**: Zero breaking changes to existing code

You can use it today:

```bash
mcp-server-postgres postgres://user:pass@localhost/dbname \
  --access-mode dml_only \
  --query-timeout 30
```

## What's Next?

Potential future enhancements:

- Optional flag to allow UPDATE/DELETE without WHERE for legitimate use cases
- Row-level security integration
- Query cost estimation to prevent expensive operations
- Rate limiting for DML operations

But for now, DML_ONLY mode solves the core problem: giving AI agents and automated scripts the access they need, without the access that could destroy your data.

---

*The complete implementation is available my fork the [postgres-mcp repository](https://github.com/Fuzzwah/postgres-mcp/tree/feature/dml-only-mode). All 55 tests passing, code review complete, ready for merge.*
