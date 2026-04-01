## Why

Working across multiple machines with agentic coding tools creates a subtle but genuinely painful problem: the agent's state diverges. Sessions are scattered, memory files accumulate differently on each machine, and the agent starts behaving inconsistently depending on where you're sitting. This post covers a real problem the author hit, what he tried first (Conductor), and what actually fixed it (conduit on a dedicated homelab box). It's practical, honest, and directly useful to anyone running Claude Code across more than one machine.

## What Changes

- New blog post in `_posts/` covering the "split-brain coding agent" problem and how conduit solved it
- Post explains the problem (scattered sessions, diverging memory/harness state across two MacBooks), what was tried first (Conductor — wrong fit for a multi-repo setup), what worked (conduit on a ThinkCentre P330), and the fork improvements added
- Written in Fuzzwah's conversational Australian voice per STYLE_GUIDE.md
- 400–600 words, "here's what I did and why" framing — not a tutorial

## Capabilities

### New Capabilities
- `split-brain-coding-agent-post`: Blog post about solving the split-brain coding agent problem with conduit. Covers the problem, what didn't work, what did, and honest tradeoffs.

### Modified Capabilities

None.

## Impact

- New markdown file in `_posts/` directory
- No code changes, no dependency changes
- Published content on blog.fuzzwah.com after next deploy
