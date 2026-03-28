---
title: "Conductor: Orchestrating AI Agents Across My Repos"
date: 2026-03-28 10:00:00 +1100
author: Fuzzwah
description: I had six repos and too many terminal tabs. Conductor gave me parallel AI agents in isolated workspaces, and the workflow that came out of it is pretty damn good.
---

There's a point in any project where the number of terminal tabs starts to feel like a personal failing. I'd been running Claude Code sessions across six different repos — switching between terminals, trying to remember which agent was doing what, occasionally discovering that two agents had different ideas about the database schema. It worked, technically. The same way juggling chainsaws works until it doesn't.

I needed a system, not just more terminal tabs.

## The Mess

The platform I'm building has six Git repositories:

- A **Discord bot** (Python, py-cord) that handles user interactions
- A **Django web app** (DRF, Redis) serving the API and admin interface
- A **data collection pipeline** (Python, RabbitMQ) that ingests and processes external data on a schedule
- **Shared database utilities** — common models, migration helpers, query patterns
- An **OAuth service** (Node.js, deployed on Vercel) handling third-party auth
- A **Jekyll documentation site** for the project

All sharing a PostgreSQL database with time-series partitioning. That shared database is the connective tissue — and the thing most likely to cause grief when an agent in one repo writes a query without checking what's actually in the schema.

Each repo is manageable on its own. The problem is coordination. When an agent working on the Discord bot needs to understand a database pattern that was just changed by an agent working on the pipeline, you need something that keeps everyone on the same page. And "me, alt-tabbing between six terminals and hoping for the best" wasn't cutting it.

## Finding Conductor

[Conductor](https://www.conductor.build/) is a Mac app for running teams of coding agents. The pitch is simple: create parallel Claude Code agents in isolated workspaces, see what they're all doing at a glance, then review and merge their changes. Each workspace is an isolated copy and branch of your Git repo — agents can't step on each other's toes because they're literally working in separate directories on separate branches.

What got my attention was the workspace model. When you add a repo to Conductor and create a workspace, it copies your git-tracked files into an isolated environment. The agent works there independently. You can spin up multiple workspaces from the same repo — one per feature, one per bug, whatever you need. Hit `⌘N` and you've got another Claude instance running in its own sandbox.

For my six-repo setup, this was immediately appealing. Instead of six terminal windows with Claude Code sessions that I had to babysit, I could see all my agents in one interface. Which one is thinking, which one is writing code, which one has finished and is waiting for review — all visible without alt-tabbing through a mess of terminals.

## The Workflow That Emerged

Conductor handles the workspace isolation and parallel execution. But the workflow I've built on top of it — that's where it gets interesting.

### A Central Hub Repo

I added a seventh repo to the mix. A central orchestration repo that holds project-wide specs, shared configuration, and planning artifacts. Each of the six component repos has symlinks back to this hub for shared docs and specs:

```
hub/
├── openspec/           # All specs and change tracking
├── shared/
│   ├── database-schema.md
│   ├── deploy-procedures.md
│   └── architecture-decisions.md
└── CLAUDE.md           # Project-wide agent context

discord-bot/
├── src/
├── openspec -> ../hub/openspec
├── shared -> ../hub/shared
└── CLAUDE.md           # Repo-specific agent context
```

When I spec out a change in the hub and commit it to main, every agent in every workspace can see it immediately through their symlinks. No syncing, no copying files around. It's just there.

Conductor's setup scripts made this clean to wire up. Each repo's setup script creates the symlinks and installs dependencies in the isolated workspace. Spin up a new workspace, the script runs, and the agent has everything it needs from the jump.

### Spec-First with OpenSpec

Rather than pointing agents at problems and saying "go fix it," work flows through a deliberate pipeline using OpenSpec:

1. **Explore** — thinking partner mode. No code. Just discussion, investigation, and clarifying what the problem actually is.
2. **Propose** — a formal spec with a design document, task breakdown, and implementation plan. Everything gets written down before anyone writes a line of code.
3. **Apply** — implementation against the spec. The agent works through the tasks with clear guardrails rather than vibes.
4. **Archive** — the change gets filed away. Done and dusted.

The hub repo is where most exploring and proposing happens. I'll have a Conductor workspace open on the hub, working through a spec with one agent, while another workspace on the Django repo is implementing a completely different feature. Parallel work, shared context, no conflicts.

### CLAUDE.md as Institutional Memory

Each repo has its own `CLAUDE.md`. The hub repo's version provides project-wide context: database access patterns, deployment procedures, shell conventions (fish shell everywhere, because I'm that person), coding standards, and architectural decisions.

Each component repo's `CLAUDE.md` adds repo-specific details on top. The Discord bot's version covers py-cord patterns. The Django app's covers DRF serialiser conventions and Redis caching. And so on.

This is the mechanism that keeps multiple independent agents aligned without me standing over their shoulders. Every agent session is effectively day one for that agent — `CLAUDE.md` is the onboarding doc that gets read every time. A rough sketch of what the hub's version covers:

- **Database rules**: Always check `shared/database-schema.md` before writing queries. Time-series tables use date-based partitioning. Never modify partition boundaries without updating the schema doc.
- **Deployment**: Each service runs via systemd. Deployment order matters — migrations first, then API, then consumers, then bot.
- **Shell conventions**: Fish shell everywhere. Don't generate bash scripts.
- **Cross-repo coordination**: If you change a shared model, note it in the hub. If you're unsure about a database pattern, explore first.

## What's Working Well

After a few months of running this setup, here's what I'm genuinely happy with:

- **Parallel agent work is the big win.** Speccing out a feature in one Conductor workspace while another implements something unrelated — both productive at the same time, neither blocked by the other. Conductor's interface makes it easy to keep track of who's doing what.
- **Isolated workspaces prevent chaos.** Before Conductor, I had occasional disasters where two Claude Code sessions in the same repo would make conflicting changes. Isolated branches and directories solved that completely.
- **The diff viewer is great for review.** When an agent finishes work, Conductor shows exactly what changed and recommends the steps to merge. It's a much better review experience than digging through `git diff` output in a terminal.
- **Checkpoints as safety nets.** Conductor snapshots the workspace state before each agent turn. If an agent goes off the rails, I can revert to any previous checkpoint without losing the conversation context. I've used this more than I'd like to admit.
- **Agents that understand deployment are weirdly useful.** Because `CLAUDE.md` includes the full deployment pipeline — systemd services, nginx config, the lot — agents provide deploy commands alongside code changes. "Here's the migration, here's the code, and here's the systemctl commands to roll it out."
- **Agents surfacing CLAUDE.md inconsistencies.** A bonus I didn't expect. Because agents actually read `CLAUDE.md` carefully, they occasionally flag things that don't match reality. Free documentation auditing.

## What's Still Tricky

Not all sunshine and rainbows:

- **Keeping CLAUDE.md accurate is ongoing work.** The project evolves, services get renamed, deployment procedures change. If `CLAUDE.md` falls behind, agents start confidently doing the wrong thing — which is arguably worse than no instructions at all. The agents themselves often catch the drift, but it's still on me to make corrections.
- **Balancing context vs conciseness is an art.** Too much context in `CLAUDE.md` and agents get overwhelmed or slow. Too little and they make avoidable mistakes. I'm constantly tweaking the balance. There's no formula, just iteration.
- **Explore mode requires discipline.** When you've got an agent ready to code and Conductor makes it so easy to just spin up another workspace, the temptation to skip the thinking phase is strong. I still skip it sometimes for small changes. I still regret it sometimes when those "small changes" turn out to have hidden complexity.
- **Setup scripts need maintenance.** As repos evolve and dependencies change, the Conductor setup scripts need updating too. It's not a lot of work, but it's one more thing to remember.

## Where This Is Heading

The combination of Conductor for workspace management and OpenSpec for structured workflow has been genuinely good. Conductor solved the mechanical problem — running multiple agents without losing track of them — and the spec-first workflow solved the strategic problem of keeping agents aligned and thoughtful rather than just fast.

If you're working with AI agents on anything bigger than a single repo, I reckon the setup is worth trying. Conductor handles the orchestration, `CLAUDE.md` files handle the context, and a structured spec workflow keeps everyone honest. The specific tools matter less than the principles: isolated workspaces, shared context, and thinking before coding.

Still evolving, but it's working well enough that I'm building real things with it every day. Stay tuned.
