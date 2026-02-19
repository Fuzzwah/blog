---
title: "StackTUI — A TUI Dashboard for Your Docker Compose Projects"
date: 2026-02-19 10:00:00 +1100
author: Fuzzwah
description: I open-sourced a terminal dashboard for managing Docker Compose stacks, and I reckon it's pretty bloody useful.
---

If you've ever found yourself with six terminal tabs open — one running `docker compose ps`, another tailing logs, a third where you're about to type `docker compose restart webapp` for the fourth time today — then you know the specific flavour of annoyance that led me to build this thing. I got properly sick of it. The constant tab-switching, the "wait, which terminal was I watching logs in?", the moment you realise you restarted the wrong service because you were in the wrong tab. A world of pain.

So I built [StackTUI](https://github.com/Fuzzwah/stacktui), a TUI dashboard for Docker Compose projects. And I've just open-sourced it.

## What StackTUI Does

The pitch is simple: one terminal, full picture of your stack. You open it up and you can see which services are healthy, which ones have fallen over, what the logs are doing, and what branch your code is on. You can restart a misbehaving service without opening a new tab. You can tail logs from any container or log file and switch between them with a dropdown. You can pull the latest code and see which services were affected by the changes.

It's a cockpit for your Compose stack. Everything you'd normally spread across half a dozen terminal windows, crammed into one TUI that actually looks decent.

## The Features

- **Service monitoring** — live health indicators with colour-coded status dots, refreshed every 10 seconds
- **Log tailing** — stream container logs or local log files in real time, switchable via dropdown
- **Service orchestration** — stop, start, and restart selected services with smart ordering (infra first, then app services)
- **Git integration** — pull latest code, switch branches, view commit history, and auto-detect which services are affected by changes
- **Webhook notifications** — banner alerts when GitHub pushes new commits to your current branch
- **12 themes** — yes, twelve. Press `T` to cycle through them. This is perhaps evidence that I got a bit carried away, but they look good so I'm not apologising
- **TOML configuration** — one config file adapts the dashboard to any Compose project
- **Dev and prod modes** — auto-detects your environment or use `--dev`/`--prod` flags
- **Native process detection** — monitors non-Docker processes via `pgrep` patterns in dev mode
- **Self-update** — pulls latest code on startup and offers a reload button when the dashboard script changes

## Try It in Five Minutes

I've bundled a complete demo environment so you can kick the tyres without wiring up your own project first. Five services — a Flask webapp, a background worker, nginx, PostgreSQL, and Redis — all ready to go.

```bash
# Clone the repo
git clone https://github.com/fuzzwah/stacktui.git
cd stacktui

# Start the demo services
docker compose -f demo/docker-compose.yml up -d

# Install dependencies and run with the demo config
uv sync
cp dashboard.demo.toml dashboard.toml
uv run python dashboard.py --dev
```

That's it. You'll have a fully working dashboard with services to monitor, logs to tail, and buttons to press. Hit `http://localhost:8080` to generate some nginx and webapp traffic so the logs have something interesting in them.

## Using It With Your Own Project

Getting StackTUI running against your own Compose stack takes about ten minutes. Copy `dashboard.toml.example` into your project root, rename it to `dashboard.toml`, and start filling in the sections — your project name, paths to your compose files, which services are "app" services versus infrastructure, where your log files live, and any useful URLs you want quick access to.

The demo config is a solid reference. Most of the sections are self-explanatory, and I've annotated the example file so you're not guessing at what each field does. Copy `dashboard.py` into your project and you're off.

## Built With Claude Code and OpenSpec

If you've been following this blog you'll know I've been deep in the [agentic coding](/2026/01/17/choosing-your-agentic-coding-workflow/) and [spec-driven development](/2026/02/06/switching-from-speckit-to-openspec/) rabbit hole for a while now. StackTUI is the project that came out of it. The whole thing was built using Claude Code with [OpenSpec](https://github.com/openspec-dev/openspec) handling the spec-driven workflow.

Having proper specs made a genuine difference on a project this size. There are a lot of moving parts — Textual widgets, Docker API calls, git operations, webhook handling, theme management, config parsing — and it'd have been easy for things to get tangled without something to come back to. Each feature went through the proposal-design-specs-tasks cycle, and when I picked up a session days later, the agent had all the context it needed to keep building without me re-explaining everything.

It's a bit meta, I know. The last few posts have been about the workflow, and this is the thing that actually came out of it. But I reckon that's the best endorsement — the workflow produced something real and useful, not just more posts about workflows.

## What I've Learnt

1. **Textual is genuinely excellent.** The team behind it have built something special. Rich TUIs in Python used to be a nightmare of curses and manual layout. Textual makes it feel almost like building a web app. The widget system, the CSS-like styling, the reactive attributes — it all just works.

2. **Docker's API surface is larger than it looks.** Health checks have edge cases. Container states aren't as simple as "running" or "stopped". Compose service ordering gets interesting when you need to restart infrastructure before app services. I spent more time on these details than I expected.

3. **Git-aware service detection was trickier than expected.** The idea is simple — look at what files changed and figure out which services are affected. The implementation required mapping source file paths to services, which sounds straightforward until you factor in shared libraries, config files that affect everything, and monorepo-style layouts. Worth doing properly though. Being able to select "changed services" and restart just those is a genuine time-saver.

4. **Building dev/prod modes from the start was the right call.** I nearly left it as a "nice to have" but I'm glad I didn't. The dashboard behaves differently in development versus production — different compose files, different URLs, native process monitoring only in dev — and baking that distinction in early saved a lot of painful refactoring later.

I've been using StackTUI daily on my own projects and it's become one of those tools I forget isn't just part of how Docker works. It's MIT licensed, it's on [GitHub](https://github.com/Fuzzwah/stacktui), and I'd genuinely love for other people to get some use out of it. Go have a play, file issues if something's broken, and let me know what you reckon.
