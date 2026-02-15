## Why

The blog needs a post documenting the OpenSpec + Claude Code workflow that Fuzzwah has developed — how he goes from a vague idea to a shipped feature using plan mode, CLAUDE.md configuration, and OpenSpec artifacts. This also introduces a new idea: auto-generating support/debug skill files when archiving a change, completing the full lifecycle from ideation to ongoing support.

## What Changes

- New blog post in `_posts/` covering the OpenSpec workflow with Claude Code's VSCode extension
- Post walks through the full workflow: plan mode → opsx:new → opsx:ff → opsx:apply → opsx:archive
- Introduces the concept of extending `/opsx:archive` to auto-generate a support skill file for debugging/maintaining the feature just built
- Written in Fuzzwah's conversational Australian voice per STYLE_GUIDE.md

## Capabilities

### New Capabilities
- `blog-post-openspec-workflow`: A blog post about the OpenSpec + Claude Code workflow, covering plan mode usage, CLAUDE.md configuration for automatic OpenSpec triggering, and the new idea of archive-triggered support skill generation

### Modified Capabilities
<!-- No existing spec-level requirements are changing -->

## Impact

- New file in `_posts/` directory (blog post markdown)
- No code changes, no API changes, no dependency changes
- Content only — a new blog post
