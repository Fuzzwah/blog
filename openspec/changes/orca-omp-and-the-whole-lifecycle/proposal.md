## Why

The author's agentic development environment has changed completely since the last post (April 2026). The conduit-on-a-box setup from "The Split-Brain Coding Agent Problem" was built on a wrapper around agent CLIs that relied on `claude -p`, which Anthropic pulled out of the subscription — a textbook single-vendor dependence failure. The replacement is a fundamentally different setup: Orca (an agent-agnostic ADE) running headless on a Linux box as a remote server, a fleet of interchangeable agent engines (Claude Code, Codex, OpenCode, DeepSeek TUI, pi, omp) each in its own git worktree, omp as the primary engine, and an OpenSpec + GitHub Projects pipeline that tracks every change from proposal through strong-model review to a board card with an issue. This post is the overdue "here's my new environment" update the blog's arc has been building toward, and it lands the blog's recurring themes (vendor independence, spec-first discipline, model-tier economics) in one place.

## What Changes

- New blog post in `_posts/` covering the author's current agentic development environment
- Post explains what broke (conduit's dependence on `claude -p`, Anthropic pulling it from the subscription), the rebuild on Orca (agent-agnostic, isolated git worktrees per agent, remote headless server on a Linux box reached over Tailscale from MacBooks and phone), omp as the engine with cheap fast default models and strong models on tap, the OpenSpec lifecycle (explore → propose → apply → archive), the mandatory strong-model review gate, and the GitHub Issues + Backlog board tracking layer (card = issue, task checkboxes mirroring `tasks.md`, Status lifecycle)
- Written in Fuzzwah's conversational Australian voice per STYLE_GUIDE.md
- Roughly 1,100–1,300 words, structured with headers and a closing end-to-end lifecycle list
- Adds `update_notice` frontmatter to the superseded split-brain and Conductor posts pointing to the new post

## Capabilities

### New Capabilities
- `orca-omp-and-the-whole-lifecycle-post`: Blog post about the author's current agentic dev environment — Orca on a headless box, omp as engine, and the OpenSpec → review → issue → board pipeline. Covers the `claude -p` rug pull that killed the previous setup, why agent-agnostic won, the fully remote access pattern, and the whole change lifecycle end to end.

### Modified Capabilities

None.

## Impact

- New markdown file in `_posts/` directory
- `update_notice` frontmatter edits to two existing posts (`2026-04-02-split-brain-coding-agent.md`, `2026-03-28-conductor-orchestrating-ai-agents-across-repos.md`)
- OpenSpec change artifacts under `openspec/changes/orca-omp-and-the-whole-lifecycle/`
- No code changes, no dependency changes
- Published content on blog.fuzzwah.com after next deploy
