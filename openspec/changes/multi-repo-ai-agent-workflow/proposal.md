## Why

The blog has covered OpenSpec, Claude Code skills, and spec-driven development in recent posts. This post zooms out to the bigger picture: how do you actually orchestrate multiple AI coding agents across a multi-repo platform? It's the natural next step — from "here's a workflow tool" to "here's how I use it at scale across a real project with six repos."

Developers experimenting with AI-assisted development are mostly seeing single-repo demos. This post shares practical, honest experience running agents in parallel across a platform with real architectural constraints (shared database, multiple languages, deployment pipelines).

## What Changes

- New blog post covering the architecture and tooling patterns behind a multi-repo AI agent workflow
- Post covers: the problem (multi-component platform), conductor workspaces with symlinks, OpenSpec spec-first pipeline, CLAUDE.md as institutional memory, what works, what's tricky
- Written in Fuzzwah's conversational Australian voice per STYLE_GUIDE.md
- ~1800-2200 words, following established post structure (hook → sections → honest assessment → forward-looking close)

## Capabilities

### New Capabilities

- `blog-post-multi-repo-workflow`: Blog post in `_posts/` covering conductor workspace architecture, spec-first workflow with OpenSpec, CLAUDE.md as shared context, and practical lessons from running parallel AI agents across repos

### Modified Capabilities

None.

## Impact

- New file: `_posts/2026-03-26-conductor-workspaces-orchestrating-ai-agents-across-repos.md`
- No changes to existing files, layouts, or styles
