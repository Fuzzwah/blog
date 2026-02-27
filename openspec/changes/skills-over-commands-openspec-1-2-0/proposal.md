## Why

OpenSpec 1.2.0 introduced a skills system that changes how the core workflows integrate with Claude Code. The old command-based approach treated each workflow invocation as a one-shot instruction; the new skills system provides persistent context modules the agent draws on across an entire session. This post documents a real migration and explores what that architectural distinction means in practice.

## What Changes

- Blog post covering the OpenSpec 1.2.0 skills migration on a production Python monorepo
- Documents the migration path: `openspec update --force` automated the file changes; manual cleanup updated CLAUDE.md and project memory files
- Explains the commands vs skills distinction in Claude Code and why it matters
- Makes the case for spec-driven development with AI coding tools
- Argues for treating `.claude/` as first-class project infrastructure

## Capabilities

### New Capabilities

- `blog-post-openspec-skills-migration`: A blog post covering the OpenSpec 1.2.0 skills system migration — what changed, what it means, and why the broader pattern of `.claude/` as infrastructure matters

### Modified Capabilities

## Impact

- New file: `_posts/2026-02-27-skills-over-commands-openspec-1-2-0.md`
- No changes to site structure, templates, or configuration
