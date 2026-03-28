---
title: "Ask the Agent to Explain It Back"
date: 2026-03-28 14:00:00 +1100
author: Fuzzwah
description: I've been using fresh AI agents as test readers for my project context docs, and it's turned into one of my favourite lightweight practices.
---

Here's a thing I keep doing that I didn't plan to turn into a habit: every now and then, instead of giving an AI agent a task, I ask it to explain my project back to me.

That sounds like a waste of a perfectly good agent session. It's not. It's become one of the most useful things I do.

## The Setup

I run multiple AI coding agents in parallel across different repos. Each one is bootstrapped with a CLAUDE.md file — a markdown document that gets loaded automatically at the start of every conversation. It's the agent's institutional knowledge. Project structure, coding conventions, deployment procedures, database patterns, shell environment details, architectural decisions. Everything an agent needs to know before it touches a single line of code.

Think of it like onboarding documentation, except the new hire is an AI that reads it fresh every single time. No memory of last session. No "oh yeah, I remember we changed that." Every conversation starts from scratch with whatever's in that file.

When you're running agents across six repos and spinning up dozens of sessions a day, the quality of that context file matters enormously. A stale instruction in CLAUDE.md doesn't just cause one bad conversation — it causes the same mistake in every conversation until someone catches it.

## The Technique

Instead of diving straight into a task, I occasionally spin up a fresh agent and ask it something like: "Explain our project workflow back to me."

That's it. No trick to it. The agent reads its CLAUDE.md context, pokes around the codebase, and gives me its best understanding of how everything works.

And then I listen. Really listen. Because the gaps and mistakes in its explanation are a direct mirror of the gaps and mistakes in my documentation.

## What It Surfaces

I keep finding the same categories of issues every time I do this.

### Stale Information

Procedures that have changed but the docs haven't caught up. The deploy process got simplified three weeks ago but CLAUDE.md still describes the old manual steps. The test command switched from `pytest` to a custom script but nobody updated the conventions section. That sort of thing.

You'd think you'd notice this stuff yourself, but you don't. You know the current process, so when you scan the docs, your brain autocorrects the stale bits. The agent can't do that. It takes the docs at face value, and when it confidently explains a process that hasn't existed for a month, you notice.

### Inconsistencies

This one's my favourite. I had a CLAUDE.md that documented fish shell conventions in one section — how we use fish as our default shell, the syntax patterns to follow — and then showed bash-style commands in the examples further down. The commands happened to work in both shells, so nobody noticed the mismatch. It wasn't causing errors, just sending mixed signals about which shell we actually use.

A fresh agent laid it all out side by side: "Your shell conventions section says fish, but these examples use bash syntax." Dead obvious once someone points it out. Invisible when you're too close to it.

### Implicit Knowledge

The stuff that lives in your head but never made it into the docs. You know that the staging database gets refreshed every Sunday night so you shouldn't trust Monday morning data. You know that one particular API endpoint is rate-limited differently from the rest. You know that the `utils/` directory is a graveyard of abandoned experiments and the agent should ignore most of what's in there.

But you never wrote any of that down. So every fresh agent session is operating without it. When the agent explains the project and doesn't mention any of these things, that's your signal. If it's not in the agent's explanation, it's not in the docs, which means no agent has ever known it.

### Ambiguities

Instructions that make perfect sense to you but could be read differently by something encountering them cold. "Follow the standard deployment process" — standard according to whom? "Use the existing patterns" — which patterns, there are three different approaches in the codebase? "Check with the team before merging" — the agent is the team, mate.

An agent interpreting these instructions literally will either guess (badly) or stall. Either way, the ambiguity is costing you.

## Why This Works

A fresh agent is the perfect test reader for your documentation. It genuinely has no prior context. No memory of past conversations. No ability to fill in gaps from experience. It can't think "oh, they probably mean X" the way a colleague would. It reads what's there, and only what's there.

If the agent's explanation is wrong, that's a signal that your context docs are wrong. If it's incomplete, the docs are incomplete. If it gets confused by something, every future agent session will get confused by the same thing.

It's like having a new team member start every single day and watching where they stumble. Except this team member reads fast and doesn't mind being asked to do it again tomorrow.

## The Feedback Loop

Here's where it gets properly useful. The conversation doesn't stop at "the agent explained it wrong." It becomes collaborative editing, right there in the same session.

The agent explains something. I correct it: "Actually, production uses fish too — that's why those bash-style commands work but they're inconsistent with our conventions." The agent acknowledges the correction and I ask it to update the CLAUDE.md right there. New instruction, better wording, the inconsistency fixed.

One conversation. The context file is now better for every future session.

Sometimes the corrections cascade. Fixing one section reveals that another section references the old version. Or adding a missing piece of implicit knowledge makes you realise there are three other things you also never wrote down. The agent and I go back and forth, each pass making the document a little more complete and a little more accurate.

It's not a formal review process. It's more like rubber duck debugging for your documentation, except the duck talks back and can make the edits for you.

## What to Ask For

If you want to try this, here are the prompts I've found most useful:

- **"Explain our workflow to me"** — tests whether the high-level process is documented clearly enough for an agent to follow. If it can't describe the workflow, it can't follow it.
- **"How does deployment work?"** — catches stale deploy procedures fast. Deployment steps change more often than people update the docs.
- **"What should a new agent know before touching the database?"** — validates that access patterns, gotchas, and schema docs are all discoverable from the context. Database mistakes are the expensive ones.
- **"Walk me through how you'd add a new feature"** — tests whether your spec-first workflow, branching conventions, and coding patterns are clear enough to follow without hand-holding. This one usually surfaces the most gaps.
- **"What conventions should you follow when writing code in this project?"** — catches inconsistencies between documented conventions and actual codebase patterns. If the agent describes conventions that don't match what's in the code, something needs updating.

You don't need to do all of these at once. Pick one, see what comes back, fix what needs fixing. The whole thing takes maybe fifteen minutes and the payoff compounds.

## The Bigger Picture

Here's the thing that made this click for me: the quality of your AI agent's work is directly proportional to the quality of the context you give it. That sounds obvious when you say it out loud, but the implication is that improving your context docs is one of the most impactful things you can do. Every fix benefits every agent, in every conversation, going forward.

And using the agents themselves as reviewers creates a virtuous cycle. The agent finds a gap, you fix it, the next agent is better because of the fix, and when that agent eventually reviews the docs, it catches something else. Each pass tightens the loop.

I've started thinking of my CLAUDE.md files less as static documentation and more as living context that gets refined through use. The agents are both the consumers and the quality assurance. Every inconsistency caught is one fewer mistake that'll get repeated across dozens of future sessions.

It's a small practice. No tooling required, no process to set up, no overhead beyond occasionally asking "explain this back to me" instead of "go build this thing." But the compound effect of consistently better context has been one of the bigger improvements to my agentic workflow. Worth a try next time you spin up a fresh session.
