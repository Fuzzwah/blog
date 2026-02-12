---
title: "Migrating from Spec-Kit to OpenSpec: A Practical Guide"
date: 2026-02-13 10:00:00 +1100
author: Fuzzwah
description: A step-by-step guide for Spec-Kit users ready to migrate to OpenSpec—covering preparation, spec conversion, workflow mapping, and how to preserve the discipline you've built while gaining a living specification system.
---

If you've been using GitHub's Spec-Kit for spec-driven development, you already know the value of structured requirements, prioritized user stories, and measurable success criteria. Spec-Kit taught a lot of us those habits. But after living with it on a production project, I found myself wanting a more fluid workflow—less rigid phase gating, faster planning cycles, and most of all, a way to turn completed specs into living knowledge rather than letting them gather dust. I [wrote about my own switch to OpenSpec](/2026/02/06/switching-from-speckit-to-openspec/) recently, and since then several people have asked for more practical guidance on the actual migration process.

This post is that guide.

## Why the Tooling Layer Matters

Before diving into mechanics, it's worth stepping back to consider why this migration matters at all. Can Bölük's recent post on [The Harness Problem](https://blog.can.ac/2026/02/12/the-harness-problem/) articulated something that resonated deeply:

> "The model is the moat. The harness is the bridge. Burning bridges just means fewer people bother to cross. Treating harnesses as solved, or even inconsequential, is very short-sighted."

The [Hacker News discussion](https://news.ycombinator.com/item?id=46988596) around this post landed on a similar conclusion: 2026 is shaping up to be the year people realize that the infrastructure around AI models matters as much as the models themselves. The tooling layer—the harness—is where leverage lives.

Spec-driven development frameworks are exactly this kind of harness. They shape how effectively your AI assistant understands requirements, maintains context, and translates intent into code. A stalled harness means stalled leverage. When Spec-Kit stopped evolving, it wasn't just missing features—it was an increasingly outdated bridge between your thinking and your AI's output.

That's the real reason to consider migrating. Not because there's anything wrong with Spec-Kit—it's a solid framework that continues to receive updates—but because the harness you choose shapes how effectively you work. If OpenSpec's fluid workflow and archiving system better fit how you think about specs, switching isn't disloyalty to a tool. It's optimizing your bridge.

## Before You Start: What to Preserve

The most important thing about your Spec-Kit workflow isn't Spec-Kit. It's the discipline you've built. Before touching any files, write down the practices you want to carry forward:

- **P1/P2/P3 prioritization** — This mental model works in any framework. Keep using it.
- **Measurable success criteria** — Concrete numeric targets instead of "it works." Non-negotiable.
- **Technology-agnostic requirements** — Describing *what* before *how*. This is a thinking habit, not a tool feature.
- **Edge case documentation** — The practice of asking "what could go wrong?" before writing code.
- **Constitution principles** — Your project-wide development principles. These transcend any single tool.

These are yours. No migration can take them from you, and no framework grants them automatically.

## Step 1: Audit Your Existing Specs

Start by cataloguing what you have. In a typical Spec-Kit project you'll find:

- A **constitution** document capturing project-wide principles
- One or more **spec files** with user stories, requirements, and success criteria
- **Implementation plans** generated from those specs
- Various artifacts from `/specify` and `/speckit.plan` commands

For each spec, note its status:
- **Active**: Currently being implemented
- **Completed**: Feature shipped, spec is historical reference
- **Planned**: Spec written but implementation hasn't started

This inventory tells you what needs careful migration versus what can be batch-converted.

## Step 2: Install and Initialize OpenSpec

```bash
npm install -g @fission-ai/openspec@latest
```

Then, in your project root:

```bash
openspec init
```

This creates the `openspec/` directory structure. Before going further, disable telemetry if that matters to you:

```bash
export OPENSPEC_TELEMETRY=0
```

Add this to your shell profile so it persists.

## Step 3: Migrate Your Constitution

Your Spec-Kit constitution maps naturally to OpenSpec's project-level configuration. The concepts translate directly:

| Spec-Kit Constitution | OpenSpec Equivalent |
|---|---|
| Development principles | Project specs (root-level) |
| Technology constraints | Design constraints |
| Quality standards | Spec validation criteria |
| Architecture decisions | Design documentation |

Create your initial OpenSpec project specs by extracting the principles from your constitution. The format changes, but the content shouldn't—these are your project's DNA.

## Step 4: Convert Specs by Status

### Planned Specs (Not Yet Implemented)

These are the easiest. For each planned spec:

1. Run `/opsx:new` to create a new change folder
2. Transfer your user stories, requirements, and success criteria into the proposal and spec artifacts
3. Use `/opsx:ff` (fast-forward) to generate the remaining planning artifacts

The fast-forward command generates a complete planning package—proposal, specs, design, and tasks—in one pass. Review the output and refine where your original spec had details that the generated artifacts missed.

**Key difference**: Where Spec-Kit enforced rigid phases (spec → implementation plan, in that order), OpenSpec lets you iterate on any artifact at any time. If fast-forward generates a design that reveals a gap in your requirements, just update the spec directly. No phase gates.

### Active Specs (Currently Being Implemented)

These need more care. You're mid-stream, so the goal is continuity, not disruption:

1. Run `/opsx:new` to create the change folder
2. Manually populate the proposal and specs from your existing Spec-Kit artifacts
3. Populate the task list with your remaining work items
4. Mark already-completed tasks as done
5. Continue implementation using `/opsx:apply`

Don't try to fast-forward active specs—you already have the context and the plan. Just map your existing state into OpenSpec's structure and keep going.

### Completed Specs (Historical Reference)

This is where OpenSpec's killer feature—archiving—earns its keep. For completed specs:

1. Create the change folder with `/opsx:new`
2. Populate all artifacts from your Spec-Kit originals
3. Mark all tasks as complete
4. Run `/opsx:archive`

Archiving merges the completed spec's delta into your main specification base. Your project's central specs now reflect what you actually built, not just what you originally planned. Each archived spec enriches the context available to your AI assistant for future work.

If you have many completed specs, `/opsx:bulk-archive` handles them in batch.

**This is the single biggest upgrade over Spec-Kit.** In Spec-Kit, completed specs were inert files. In OpenSpec, they become living knowledge that compounds with every archived change.

## Step 5: Map Your Workflow Commands

If you've internalized Spec-Kit's commands, here's how they translate:

| Spec-Kit Command | OpenSpec Equivalent | Notes |
|---|---|---|
| `/specify` | `/opsx:new` + `/opsx:ff` | OpenSpec separates creation from planning |
| `/speckit.plan` | `/opsx:ff` or manual refinement | Fast-forward generates all artifacts at once |
| *(no equivalent)* | `/opsx:apply` | AI implements planned tasks |
| *(no equivalent)* | `/opsx:archive` | Merges completed work into living specs |

The biggest workflow shift: OpenSpec uses **delta specifications**. Instead of rewriting an entire spec when requirements change, you express modifications as ADDED, MODIFIED, or REMOVED requirements. This keeps diffs focused and reviews meaningful.

## Step 6: Clean Up

Once all your specs are migrated and you've verified the OpenSpec workflow feels right:

1. Remove Spec-Kit's Python dependencies from your project
2. Delete the old Spec-Kit directory structure
3. Update any documentation or onboarding guides that reference Spec-Kit commands
4. Commit the migration as a single, well-documented change

Don't rush this step. Run both systems in parallel for a sprint or two if that gives you confidence. There's no penalty for a gradual transition.

## Common Gotchas

**Don't translate format one-to-one.** Spec-Kit and OpenSpec have different philosophies. Spec-Kit favors comprehensive upfront documents; OpenSpec favors iterative refinement through deltas. Trying to force Spec-Kit's rigid structure into OpenSpec defeats the purpose.

**Archive completed work before starting new work.** The value of archiving compounds—each archived spec improves context for everything that follows. If you migrate a backlog of completed specs, archive them chronologically so the specification base builds up in the right order.

**Expect a mental model shift.** Spec-Kit trained you to think in complete documents with phase gates. OpenSpec asks you to think in changes against an evolving baseline. This feels uncomfortable at first, like switching from waterfall to incremental delivery. Give it a couple of features before judging.

**Don't lose the rigor.** OpenSpec's fluidity is a feature, but it can also be a trap. Without Spec-Kit's rigid checklist enforcing quality, you need to bring that discipline yourself. Keep asking: Are my success criteria measurable? Are my priorities honest? Have I documented edge cases?

## The Bottom Line

I've just completed this migration on a production project with a substantial spec backlog. The process took about a day for the mechanical conversion, plus another day of refinement as I got comfortable with the new workflow. The specs themselves—the thinking, the priorities, the success criteria—all carried over intact.

The real payoff came after archiving. Starting a new feature with a specification base that actually reflects the current state of the system, rather than a collection of historical documents, is a qualitatively different experience. Your AI assistant doesn't need to piece together context from scattered specs and outdated plans. It reads the living specification and understands where the system is right now.

That's the harness working for you instead of against you.

---

*For more context on my initial adoption of spec-driven development, see [Adopting GitHub Spec-Kit](/2026/01/08/adopting-github-spec-kit/). For the detailed comparison that led to my own switch, see [Switching from Spec-Kit to OpenSpec](/2026/02/06/switching-from-speckit-to-openspec/).*
