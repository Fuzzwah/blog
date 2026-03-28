## Context

The blog has published posts about Claude Code, OpenSpec, and spec-driven development. This post takes the perspective one level up — how the author orchestrates AI agents working independently across multiple Git repositories on a real platform. The platform has a Discord bot (Python/py-cord), a Django web app (DRF/Redis), a data pipeline (Python/RabbitMQ), shared database utilities, supporting services (Node.js OAuth, Jekyll docs site), all sharing a PostgreSQL database with time-series partitioning.

The author isn't naming the product — the focus is purely on the development workflow patterns that other developers can adapt.

## Goals / Non-Goals

**Goals:**
- Share a practical, replicable architecture for multi-repo AI agent workflows
- Explain the conductor workspace pattern (central repo + symlinks) clearly enough that someone could set it up
- Show how OpenSpec's explore → propose → apply → archive pipeline prevents agents from rushing into code
- Demonstrate CLAUDE.md as the mechanism for institutional memory across agents
- Be honest about what's tricky — not a sales pitch
- Follow STYLE_GUIDE.md voice (conversational Australian, self-deprecating, direct)

**Non-Goals:**
- Not revealing or promoting the specific product being built
- Not a tutorial — readers should understand the patterns, not follow step-by-step setup
- Not an OpenSpec deep-dive (that's been covered in prior posts)
- Not a Claude Code feature walkthrough
- Not benchmarking AI agent performance or comparing tools

## Decisions

**Narrative arc: "here's what I'm juggling → here's how I've organised it → here's what I've learnt"**
Start with the complexity of the platform to establish why you need this workflow, then walk through the solution layers, close with honest lessons. The reader should feel like they're getting a tour of a working setup, not a pitch.

**~1800-2200 words, 6 main sections + hook/close**
Longer than the StackTUI announcement because there's more conceptual ground to cover. Each section needs to land its own idea without depending too heavily on the others.

**Section structure:**
1. Hook — the mess that motivated the workflow
2. The Platform (brief) — just enough to establish the multi-repo complexity, no product reveal
3. Conductor Workspaces — the central repo + symlink architecture
4. Spec-First with OpenSpec — the explore/propose/apply/archive pipeline
5. CLAUDE.md as Institutional Memory — how context stays consistent across agents
6. What Actually Works / What's Tricky — honest assessment, grouped together
7. Close — forward-looking, invite readers to try the patterns

**Describe the platform components generically**
Use the tech stack names (Django, py-cord, RabbitMQ etc.) but don't name the actual product or service. The point is the architecture pattern, not the product.

**Lists for the "what works/what's tricky" section**
These are concrete observations, not narrative. Lists make them scannable. Keep list items casual per style guide.

**Prose for the conceptual sections (conductor workspaces, OpenSpec, CLAUDE.md)**
These need explanation and context. A directory tree or code snippet can break up the prose where it helps.

## Risks / Trade-offs

**Risk: Post reads like documentation for OpenSpec** → Mitigation: Keep OpenSpec to one section, frame it as "how I use it" not "how it works." Link to prior posts for details.

**Risk: Too abstract without the product context** → Mitigation: Use concrete examples (symlink paths, CLAUDE.md snippets, directory structures) to ground the patterns in reality.

**Risk: "What's tricky" section sounds like complaining** → Mitigation: Frame challenges as interesting problems, not frustrations. Self-deprecating humour helps.

**Risk: Readers think this only works with Claude Code** → Mitigation: Note that the patterns (central specs, symlinks, structured workflow) are tool-agnostic even though this implementation uses Claude Code.
