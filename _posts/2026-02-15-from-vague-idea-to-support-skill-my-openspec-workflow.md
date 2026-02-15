---
title: "From Vague Idea to Support Skill: My OpenSpec Workflow"
date: 2026-02-15 14:00:00 +1100
author: Fuzzwah
description: How I wired up Claude Code and OpenSpec so a half-baked thought turns into a shipped feature with its own support toolkit.
---

I've been banging on about spec-driven development for a while now — from [messy PRDs](/2026/01/08/adopting-github-spec-kit/) to [Spec-Kit](/2026/02/06/switching-from-speckit-to-openspec/) to [OpenSpec](/2026/02/13/migrating-from-speckit-to-openspec-a-practical-guide/). Each step got me closer to something that actually sticks. But the workflow I've landed on now? It's honestly the first time I've felt like the whole process from "I reckon we need X" to "it's built, documented, and supportable" is one smooth ride rather than a series of disconnected sprints.

So let me walk you through it.

## The Starting Point: Plan Mode With Opus

If you've read my earlier posts you'll know I'm a big fan of [plan mode](/2026/01/17/choosing-your-agentic-coding-workflow/) — use Opus to think, Sonnet to build. That hasn't changed. What has changed is how tightly I've wired it into OpenSpec.

My workflow starts in the Claude Code extension in VS Code. I fire up a new session with Opus selected and I'm immediately in plan mode. The prompt is deliberately casual and concise — I'm not writing requirements, I'm having a conversation:

```
Consider a system where users can submit feedback through a widget that appears on any page of the app. It should capture the current URL and a screenshot automatically.
```

That's it. No user stories, no acceptance criteria, no formal structure. Just the kernel of an idea expressed the way you'd describe it to a colleague. Opus takes that and explores the codebase, asks clarifying questions if it needs to, and builds out a proper plan — architecture considerations, files that'll need changing, potential gotchas.

The beauty of this is that I'm thinking out loud with something that can actually interrogate the codebase while I do it. I describe what I want, Opus shows me what it'd take to build it, and we go back and forth until the plan feels right.

## The CLAUDE.md Trick

Here's where it gets properly interesting. In my `CLAUDE.md` file I've got two instructions that wire everything together:

```markdown
## Agent Behavior

- **Always start in plan mode.** When a fresh session begins with a task or prompt, enter plan mode first. Explore the codebase, understand what's needed, and present a plan before writing any code or content.
- **After a plan is approved, start an OpenSpec change.** Once the user approves your plan, immediately run `/opsx:new` to create a tracked change before implementing anything. The plan you just made is the input for the proposal.
```

So when I approve the plan and the agent exits plan mode, it doesn't just start hacking away at code. It creates an OpenSpec change. The plan it just spent time developing becomes the raw material for the proposal artifact. No copy-pasting, no context switching, no "now take that plan and turn it into a spec" — it just flows.

This is the bit that makes me unreasonably happy. The thinking work doesn't evaporate when you move from planning to building. It gets captured, structured, and becomes the foundation for everything that follows.

## OpenSpec as a Thinking Tool

Once the change is created, I run `/opsx:ff` to fast-forward through the artifact generation. OpenSpec's spec-driven schema produces four artifacts:

1. **Proposal** — the why. What problem are we solving and what's changing?
2. **Design** — the how. Architecture decisions, trade-offs, what we're explicitly not doing
3. **Specs** — the what. Testable requirements with concrete scenarios
4. **Tasks** — the work. A checkbox list of implementation steps

Each artifact builds on the previous ones, and the agent reads them all before creating the next. So the thinking compounds — the proposal informs the design, the design and specs together shape the tasks.

I'll be honest, when I first started with spec-driven development it felt like ceremony for ceremony's sake. Writing specs before code? Documenting trade-offs? My eyes would've glazed over reading that a year ago. But having watched Claude generate genuinely useful artifacts from a casual conversation — artifacts that catch edge cases I hadn't considered, that force decisions I was avoiding — I'm a proper convert.

The artifacts aren't bureaucracy. They're the agent thinking rigorously about your problem before touching a single line of code. And when implementation starts with `/opsx:apply`, the agent has all that context. It knows what it's building, why it's building it that way, and what "done" looks like. The code quality difference is noticeable.

## The Missing Piece: What Happens After Archive?

Right. So here's the new idea I've been chewing on, and I reckon it could be decent one.

The current lifecycle looks like this: plan → spec → build → archive. When you're done, you run `/opsx:archive` and the change gets filed away. Your delta specs merge into the main specs, your tasks get ticked off, everything's tidy. Nice.

But then three weeks later someone reports a bug in the feature you just built. You open a new Claude Code session, describe the issue, and... the agent has no idea about the design decisions you made, the edge cases you considered, the trade-offs you explicitly chose. All that rich context from the artifacts? Gone. Archived. The agent starts from scratch, reading code and trying to reverse-engineer intent from implementation.

What if archiving a change also generated a support skill?

## Support Skills: Closing the Loop

The idea is dead simple. When you run `/opsx:archive`, alongside the normal archiving process, it generates a Claude Code skill file — a `.md` file in your `.claude/skills/` directory that's specifically designed for debugging and supporting that feature.

Think about what the agent already knows at archive time. It has the proposal (why we built this), the design (how we built it and what alternatives we rejected), the specs (what it should do, with concrete scenarios), and the tasks (what was actually implemented). That's exactly the context you need when something goes wrong.

A generated support skill might look something like this:

```markdown
# Support: Feedback Widget

## What This Feature Does
Captures user feedback from any page via an embedded widget.
Automatically includes the current URL and a browser screenshot.
Feedback is stored in the feedback table and triggers a Slack
notification to the #product-feedback channel.

## Key Design Decisions
- Screenshot capture uses html2canvas (client-side, no server
  rendering) — chosen for privacy and simplicity over accuracy
- Widget loads lazily to avoid impacting page performance
- Feedback submissions are rate-limited to 5 per user per hour

## Common Issues
- If screenshots appear blank, check that the page doesn't use
  cross-origin iframes (html2canvas limitation)
- Rate limiting is per-user via session token, not IP-based
- The Slack webhook URL is in environment config, not hardcoded

## Key Files
- Frontend: src/components/FeedbackWidget.tsx
- API endpoint: src/api/feedback.ts
- Database migration: migrations/024_feedback_table.sql

## Specs Reference
[Links to the archived spec scenarios for this feature]
```

When someone reports a bug, you could say "debug the feedback widget" and the agent would have immediate context about what the feature does, how it was designed, known limitations, and where to look. No archaeology required.

## The Full Lifecycle

What excites me about this is that it completes the circle:

1. **Plan** — "consider a system where users can submit feedback..."
2. **Spec** — OpenSpec artifacts capture the thinking
3. **Build** — `/opsx:apply` implements from the specs
4. **Archive** — change gets filed, specs merge
5. **Support** — auto-generated skill file preserves operational context

Each stage feeds the next. The vague idea becomes a plan, the plan becomes specs, the specs become code, and the code gets a companion skill that makes it supportable. Nothing falls through the cracks.