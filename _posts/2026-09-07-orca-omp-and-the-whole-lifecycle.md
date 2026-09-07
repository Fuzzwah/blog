---
title: "Orca, omp and the Whole Lifecycle: My New Agentic Dev Environment"
date: 2026-09-07 08:00:00 +1000
author: Fuzzwah
description: Five months, one rug pull and a rebuilt setup — Orca running a fleet of agents on a headless box, omp as the engine, and every change tracked from spec to board.
---

Five months between posts. In my defence, the whole setup got rebuilt in that time, and I wanted to write about the thing I landed on rather than the thing I was excited about for a fortnight.

The last post ended with a line I was pretty proud of: "One machine. One brain." That was the [split-brain](/2026/04/02/split-brain-coding-agent/) conduit setup, and it genuinely was a good answer to the problem I'd been having. Sounded like a conclusion. Turns out it was more of a stopping point — the kind you hit when you've solved the problem in front of you and can't yet see the better one sitting behind it.

## The Rug Pull

conduit was a wrapper around the agent CLIs, driving sessions through Claude Code's headless `-p` mode. It worked a treat — right up until Anthropic pulled `claude -p` out of what a subscription covers. Just like that, the thing my whole workflow stood on wasn't a thing anymore.

I'm not going to pretend that's an unreasonable call on their part; subscription economics are their business. But it was a hell of a lesson in single-vendor dependence. If your whole harness is built on one vendor's CLI quirks, you don't have a workflow. You have a lease, with a moveable expiry date.

## The Rebuild

So the rebuild started from a different question. Not "which agent should I standardise on", but "what's the environment around the agents, and does it survive any of them leaving?"

[Orca](https://github.com/stablyai/orca) — the ADE from stablyai — is the answer I landed on. It runs any coding agent side by side: Claude Code, Codex, OpenCode, DeepSeek's TUI, pi, omp, whatever I feel like pointing at a problem. Each one gets its own isolated git worktree, they're all tracked in the one place, and they run on my own subscriptions and API keys. Agent-agnostic, by design.

That changes the risk profile completely. If a vendor does another rug pull, an agent engine gets swapped out and nothing around it moves — the worktrees, the sessions, the review flow, the history. The environment is the investment now, not the model vendor.

## The Headless Box

Orca runs as a server, and mine runs headless on the little Linux box that lives on my desk and never goes to sleep. My MacBooks don't run agents anymore. They don't even SSH in to run agents. They just pair with the Orca server over Tailscale, and the whole fleet is on screen, same as if it were sitting in front of me.

When I'm not at a desk, the phone does the job — the mobile companion pings me when an agent finishes, and I can send follow-ups from wherever I am. Agents keep working when I close the laptop. The "which machine am I on" question from the split-brain post isn't answered anymore, it's dissolved: there's no machine-side state to worry about, because the answer is always "the box". The brain has a body now, and the body is a headless box I point at from anywhere.

The multi-repo pattern I wrote about in the [Conductor post](/2026/03/28/conductor-orchestrating-ai-agents-across-repos/) survived the move and got sharper. The meta repo's setup hooks give every agent session its own git worktrees of each sub-repo — the Discord bot, the Django site, the data collectors — so a fleet of agents can chew on different features without ever stepping on each other's toes.

## The Engine

The engine I spend most of my time in these days is [omp](https://omp.sh) — oh-my-pi, which bills itself as the coding agent with the IDE wired in. It's one of the engines Orca will happily run, and it's become my default for day-to-day work.

The genuinely interesting bit isn't the CLI though — it's the model economics, which have come full circle to a philosophy I wrote about back in [January](/2026/01/07/my-agentic-coding-stack/). Back then I was manually switching models: Claude for building features, GPT for review. Now the pipeline does the switching for me. Day-to-day tasks run on a fast, cheap model — DeepSeek's v4 flash at the moment — and the strong models, codex and sol, are on tap for the parts that matter. Fast model drafts, strong model checks the important bits, and I don't have to think about which brain is doing what.

## The Lifecycle

The other half of the rebuild is the process around the agents, and it's all built on [OpenSpec](https://github.com/openspec-dev/openspec), which regular readers have watched evolve with this blog from the start. The spec-first discipline from the Conductor days is still the backbone — every non-trivial change gets a proposal, a design, delta specs and a task list — but it's matured into a proper lifecycle with skills: explore, propose, apply, archive.

## The Review Gate

The bit I'm most chuffed about is the review gate, because it fixes the oldest weakness in agentic coding: the model that wrote the plan being the only set of eyes on it.

Every proposal gets reviewed by a dedicated review agent bound to a strong model — deliberately separate from the fast model that wrote the artifacts. It checks that the scope and acceptance criteria hold up, that the delta specs are verifiable and consistent with what they claim to change, that the design is feasible, that the tasks are actionable and traceable to the requirements, and that the whole change hangs together. Findings get applied straight to the artifacts, and only the artifacts — the review never touches code. Once it passes, the change lands on main, and because the specs are symlinked into every sub-repo, every agent in every worktree sees them from the next session on.

## The Board and the Issues

Then the change gets a body. Every active change becomes a GitHub issue in the meta repo — the title is the change name, and the body carries the summary, the current status, a link to the change folder, and the full task list as checkboxes mirroring the change's `tasks.md`. The issue goes on the [Backlog board](https://github.com/orgs/iracingreports/projects/5), and from then on the card is the change: Not started, In progress, Ready to archive, Deferred, Done.

Progress renders from those issue checkboxes, which is why every finished task gets ticked twice — once in the change's `tasks.md`, once in the issue body. Drift makes the board lie, so the rule is simple: no drift.

When a change's done, it gets archived: issue closed, card to Done, and the change folder filed away. Deferred is the honest shelf for the ones that die halfway, and they sit there rather than pretending to be finished.

## The Shape of It

End to end, it's one pipeline:

1. **Explore** — thinking-partner session, no code
2. **Propose** — full artifacts: proposal, design, specs, tasks
3. **Review** — strong model, independent, findings applied to the artifacts
4. **Land** — change goes to main, every agent can see it
5. **Track** — issue raised, card on the board
6. **Apply** — tasks worked through, ticked off in both places
7. **Archive** — issue closed, card Done

And here's the bit I keep grinning about: this very post is going through that pipeline. The blog repo has its own `openspec/changes` folder, and this post is an OpenSpec change with its own proposal, design, specs and task list — all drafted before a word of the post was written. If I'm going to argue the process is the product, the post about it should eat its own dog food.

## Where It's Heading

Right now the pipeline's biggest passenger is the reports rollout on the iracingreports platform — 21 report types working their way through the same explore → propose → review → card → implement → archive loop.

Conductor gave me the fleet view. conduit gave me one brain on one box. Orca and omp gave me a headless fleet I can steer from my phone, and OpenSpec plus the board gave it a backbone. For the first time the whole thing feels like a system rather than a collection of clever hacks held together by enthusiasm. The hacks were fun. The system is better.

Stay tuned.
