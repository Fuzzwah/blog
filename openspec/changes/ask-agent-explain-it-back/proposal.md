## Why

Blog posts about practical AI coding techniques are the core content of this blog. This post covers a technique the author actively uses — asking fresh AI agents to explain a project back to him as a way to audit and improve the shared CLAUDE.md context that all agents operate from. It's a timely, practical topic that fills a gap: most writing about AI agents focuses on what they build, not how to maintain the context that makes them effective.

## What Changes

- New blog post in `_posts/` covering the "ask the agent to explain it back" technique
- Post explains the setup (CLAUDE.md as institutional knowledge), the technique (fresh agent as test reader), what it surfaces (stale info, inconsistencies, implicit knowledge, ambiguities), the feedback loop, specific prompts to try, and the meta-insight about context quality driving agent quality
- Written in Fuzzwah's conversational Australian voice per STYLE_GUIDE.md

## Capabilities

### New Capabilities
- `ask-agent-explain-it-back-post`: Blog post about using fresh AI agents to audit and improve CLAUDE.md context files. Covers the technique, what it surfaces, the feedback loop, and practical prompts.

### Modified Capabilities

None.

## Impact

- New markdown file in `_posts/` directory
- No code changes, no dependency changes
- Published content on blog.fuzzwah.com after next deploy
