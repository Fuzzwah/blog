## Context

StackTUI is an existing open-source project (https://github.com/fuzzwah/stacktui) — a Python TUI dashboard built on Textual for managing Docker Compose stacks. The blog post is the deliverable here, not the tool itself. The post needs to work for two audiences simultaneously: developers who want to know if StackTUI solves their problem, and technically curious readers who want to understand how it's built.

The blog already has a post about StackTUI (the short original intro post from when it launched). This new post is a deeper technical dive — covering features, architecture, config, and the OpenSpec development workflow that underpins it.

## Goals / Non-Goals

**Goals:**
- Write a single comprehensive blog post that covers StackTUI in enough depth to be genuinely useful
- Lead with the problem (terminal window sprawl when managing Docker Compose stacks), not the solution
- Walk through the key features with enough concrete detail that readers understand what they're getting
- Explain the Textual architecture and single-file design without turning into documentation
- Cover the TOML config system so readers know how to adapt it to their own projects
- Include the OpenSpec + Claude Code development workflow as a meta angle — it's interesting and relevant to the blog's existing themes
- End with a clear quick-start path so readers can try it immediately

**Non-Goals:**
- Full API documentation or exhaustive feature reference — that lives in the README
- Comparing StackTUI to every other Docker management tool on the market
- A step-by-step installation tutorial (link to the README instead)
- Covering the demo environment in exhaustive detail — use it to illustrate, not document

## Decisions

### Post angle: problem-first, architecture-honest
The post opens with the real pain — juggling terminal windows when something goes wrong in prod at midnight. This is more compelling than "StackTUI is a cool TUI dashboard." The architecture section is honest about tradeoffs (single-file design, threading model, Textual dependency) rather than just celebrating decisions. Readers who've built TUI tools will appreciate this.

### Structure: narrative sections flow as prose, instructional content uses lists
Per the style guide, the problem intro and architecture sections use flowing paragraphs. Feature walkthrough, config examples, and quick-start use lists and code blocks. This matches the blog's technical writing conventions.

### OpenSpec workflow as a section, not a footnote
The development workflow angle is interesting enough to warrant its own section — it connects to the blog's broader theme of AI-assisted development and gives the post a meta layer that makes it more than just a tool writeup. Keep it concise though; this isn't the main event.

### Tone: technical but warm, not documentation mode
Target the reader who'd be at home on the Textual Discord — someone who's built things in Python, manages Docker stacks, and appreciates when someone explains architectural decisions honestly rather than just marketing at them.

### Date and filename
Post date: 2026-02-27. Filename: `2026-02-27-stacktui-docker-compose-tui-dashboard.md`.

## Risks / Trade-offs

- [Risk: Post becomes too long] → Keep each section tight. The architecture section in particular could balloon — constrain it to the most interesting decisions (threading model, single-file tradeoff, Textual reactive layout).
- [Risk: Tone drifts into documentation] → After each section, check: does this read like a README or a blog post? Keep the voice present throughout.
- [Risk: OpenSpec section overshadows the main topic] → One section, concise, focused on what's interesting (spec-driven AI dev, not a tutorial on OpenSpec).
