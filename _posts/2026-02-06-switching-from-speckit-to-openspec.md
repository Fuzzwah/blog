---
title: Switching from Spec-Kit to OpenSpec
date: 2026-02-06 10:00:00 +1100
author: Fuzzwah
description: Why I moved from GitHub's Spec-Kit to OpenSpec—comparing the two frameworks on maintenance health, workflow flexibility, and how OpenSpec's archive system preserves knowledge from completed work.
---

A month ago I wrote about [adopting GitHub's Spec-Kit](/2026/01/08/adopting-github-spec-kit/) and how converting my messy PRDs into structured specs cut scope by 40% and eliminated ambiguity. The framework genuinely changed how I think about requirements. I was hooked on spec-driven development.

Then the warning signs started.

## The Spec-Kit Maintenance Problem

I'd been watching the [Spec-Kit GitHub repository](https://github.com/github/spec-kit) for updates - new features, bug fixes, community activity. What I saw instead was silence. A [community discussion](https://github.com/github/spec-kit/discussions/1482) captured what many of us were feeling: 22 pull requests sitting open with zero commits merged in an entire month.

The sole official response from a GitHub maintainer? "Expect to see a flurry of activity soon." No timeline, no roadmap, no acknowledgment of the growing backlog.

The community wasn't reassured. Users started openly discussing alternatives. Some speculated that GitHub was folding spec-driven development directly into Copilot or VSCode, making the standalone tool redundant. Others flagged real usability issues - token efficiency problems, context that didn't persist between AI sessions - with no fixes in sight.

This put me in an uncomfortable position. I'd just rebuilt my entire requirements workflow around Spec-Kit. My specs, my constitution, my `/specify` and `/speckit.plan` commands - all of it depended on a tool whose future was genuinely uncertain.

The question became: do I bet my workflow on a tool that might be abandoned?

## Discovering OpenSpec

The same discussion that raised the alarm also pointed me toward [OpenSpec](https://github.com/Fission-AI/OpenSpec). With over 22,000 GitHub stars, 523 commits from 45+ contributors, and regular releases, the contrast with Spec-Kit's stalled repository was stark.

My first impression was that OpenSpec felt lighter. Where Spec-Kit required a Python environment, OpenSpec is a single npm install. Where Spec-Kit enforced rigid phase gates between spec creation and implementation planning, OpenSpec embraces fluidity - you can iterate on any artifact at any time without the framework fighting you.

But the real draw wasn't just "actively maintained." It was a fundamentally different approach to what happens after you finish building something.

## Comparing the Two Frameworks

### Where Spec-Kit Excels

Credit where it's due - Spec-Kit taught me things that carry forward regardless of tooling:

- **P1/P2/P3 prioritization** forces brutal honesty about what's actually MVP. I still use this mental model even outside of any tool.
- **Measurable success criteria** with concrete numeric targets eliminated the "it works" handwave from my vocabulary.
- **The constitution concept** - documenting project-wide principles before writing any specs - was genuinely valuable for clarifying my own thinking.
- **Forced rigor** through its quality checklist meant no spec shipped with `[NEEDS CLARIFICATION]` markers.

These are good ideas. They're also ideas that aren't exclusive to Spec-Kit.

### Where Spec-Kit Falls Short

Beyond the maintenance concerns:

- **Heavyweight setup**: Python environment, specific directory conventions, rigid template structure.
- **Phase-locked workflow**: You write a spec, then you write an implementation plan, in that order. Need to revise the spec after starting implementation? The framework doesn't accommodate that naturally.
- **No lifecycle management**: Once a spec is written and implemented, it just... sits there. There's no built-in concept of archiving completed work or evolving your specification base over time.
- **Static knowledge**: Completed specs don't feed back into your project's understanding. Each new feature starts from scratch context-wise.

### Where OpenSpec Shines

- **Active maintenance**: Regular commits, responsive maintainers, growing community. You're not shouting into a void when you hit an issue.
- **Lightweight installation**: `npm install -g @fission-ai/openspec@latest` and `openspec init`. Done.
- **Fluid workflow**: No rigid phase gates. The built-in schema guides you through proposal → specs → design → tasks, but you can jump between artifacts as your understanding evolves.
- **Delta specifications**: Instead of rewriting entire specs when something changes, you express modifications as ADDED, MODIFIED, or REMOVED requirements. This is cleaner, reduces conflicts during parallel work, and makes reviews focused on what actually changed.
- **Multi-tool support**: Works with 20+ AI assistants and editors, not just GitHub's ecosystem.
- **And the big one: archiving.** More on this below.

### Where OpenSpec Has Gaps

- **Newer ecosystem**: Less battle-tested than Spec-Kit in large-scale or enterprise contexts. The community is growing fast, but it's still younger.
- **Different mental model**: If you're coming from Spec-Kit, the shift from "write a complete spec document" to "express changes as deltas against a living specification" takes some adjustment.
- **Telemetry by default**: Anonymous usage stats are collected out of the box. You can opt out with `OPENSPEC_TELEMETRY=0`, but it's worth knowing.

## The Killer Feature: Archiving Completed Work

This is what pushed me from "interesting alternative" to "I'm switching."

In Spec-Kit, when you finish building a feature, nothing happens to the spec. It sits in your repository as a historical artifact - useful if someone digs it up, but not actively contributing to your project's evolving understanding.

OpenSpec treats completed work differently. When you run `/opsx:archive`:

1. **Validation**: It checks that all artifacts exist (proposal, specs, design, tasks) and confirms task completion.
2. **Spec sync**: Delta specifications merge into your main specs. Your central specification repository now reflects the feature you just built - it becomes the evolved source of truth.
3. **Timestamped archival**: The change folder moves from `openspec/changes/[name]/` to `openspec/changes/archive/[date]-[name]/`, preserving the complete artifact chain.
4. **Conflict resolution**: When archiving multiple parallel changes, the system detects overlapping specifications and resolves them based on implementation order.

The result: your specifications compound over time. Each completed feature enriches the specification base. When you start a new change, your AI assistant has accurate, up-to-date context about how the system actually works - not how it was originally planned to work six months ago.

This is the difference between specs as documents and specs as a living knowledge system.

For bulk operations, `/opsx:bulk-archive` handles multiple completed changes simultaneously, which is valuable when you've been heads-down on parallel work streams and need to reconcile everything at once.

## The New Workflow

My workflow has evolved from the Spec-Kit process I described last month:

1. **`/opsx:new`** → Create a new feature branch with dedicated folder structure
2. **`/opsx:ff`** → Fast-forward through planning: generates proposal, specs, design, and tasks in one pass
3. **Iterate freely** → Refine any artifact as understanding deepens. No phase gates.
4. **`/opsx:apply`** → AI implements the planned tasks
5. **`/opsx:archive`** → Completed work merges into the living specification, folder moves to archive

The fast-forward command (`/opsx:ff`) deserves special mention. Where Spec-Kit required me to build up each artifact sequentially, fast-forward generates a complete planning package in one step. I can then refine whichever parts need attention rather than grinding through a linear process.

## Lessons Learned

### Spec-Kit Was Valuable

I don't regret the Spec-Kit detour. It taught me the discipline of spec-driven development: writing requirements before code, defining measurable success criteria, prioritizing ruthlessly. Those habits persist regardless of tooling.

### Tools Need Active Stewards

A framework is only as reliable as its maintenance. When you build workflows around a tool, you're implicitly trusting that the maintainers will fix bugs, ship improvements, and respond to the community. Spec-Kit's silence broke that trust.

### Living Specs Beat Static Docs

The biggest conceptual shift from Spec-Kit to OpenSpec isn't the commands or the directory structure - it's the idea that specifications should evolve as your system evolves. Archiving isn't just cleanup; it's knowledge management.

### The Discipline Matters More Than The Tool

P1/P2/P3 prioritization, measurable success criteria, edge case documentation, technology-agnostic requirements - these concepts work in any framework. If OpenSpec disappeared tomorrow, I'd carry these practices to whatever came next. The specific tool matters less than the habit of thinking rigorously about what you're building before you build it.

---

*If you're currently using Spec-Kit and feeling the same uncertainty, take a look at [OpenSpec](https://github.com/Fission-AI/OpenSpec). The migration is straightforward, and the archive workflow alone is worth the switch. But more importantly - if you haven't started with spec-driven development at all, start now. The tool you choose matters far less than the decision to stop coding before you've defined what success looks like.*
