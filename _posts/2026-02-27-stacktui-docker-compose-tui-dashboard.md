---
title: "StackTUI Under the Hood — Textual, Threading, and Spec-Driven Development"
date: 2026-02-27 10:00:00 +1100
author: Fuzzwah
description: A deeper look at how StackTUI actually works — the Textual layout, the threading model, the TOML config, and the OpenSpec workflow keeping the whole thing from falling apart.
---

The [intro post from last week](/2026/02/19/stacktui-a-tui-dashboard-for-docker-compose/) covered what StackTUI does and how to try it. This one's for the people who came back asking about the how — the Textual architecture, the threading model, what a real config file looks like, and how you actually wire it up to your own project. There's also a proper look at the OpenSpec + Claude Code workflow that was used to build it, which I keep mentioning in passing and should probably explain properly at least once.

## What You're Looking At

When StackTUI opens you get a four-zone layout. At the top, a header bar with your project name, the current git branch, and a timestamp for the last service refresh. Below that, three columns running side-by-side: git operations and quick links on the left, service status in the middle, action buttons on the right. Below that, a full-width log panel streaming output from whichever container or log file you've selected. Footer at the bottom with keyboard shortcuts.

The middle column is where your eyes end up most of the time. Each service gets a row with a coloured status dot — green for healthy, yellow for running but without a health check configured, red for stopped or unhealthy, grey if StackTUI hasn't seen it yet. The whole panel refreshes every 10 seconds. Under the hood that's `docker compose ps --format json` and `docker ps` running together, with the results parsed and deduplicated (both commands return service information but with slightly different shapes, so there's a bit of merging required).

## The Features, Explained Properly

The [intro post](/2026/02/19/stacktui-a-tui-dashboard-for-docker-compose/) listed the features. Here's the actual detail on the ones that need explaining.

### Service Orchestration: Smart Ordering

The stop, start, and restart buttons look simple but there's logic underneath. When you restart services, StackTUI splits them into two groups based on your config: infrastructure services (databases, caches, reverse proxies — things running stock Docker images) and application services (your custom Dockerfiles).

Infrastructure comes up first. Application services are then rebuilt with `--build` before they start. This mirrors what you'd actually do manually after pulling new code — Postgres doesn't need rebuilding, but your Flask app does. Stop operations reverse the order: application services first, infrastructure last.

You declare the split in `dashboard.toml`:

```toml
[services]
infra = ["db", "redis", "nginx"]
app = ["webapp", "worker"]
```

That's the whole configuration. StackTUI uses it to sequence operations correctly for both start and stop.

### Git Integration: The Path Map

The git panel does the expected stuff — shows your current branch, lets you pull, switch branches, switch to specific commits or tags. But the feature that earns its keep day-to-day is the path map.

When you pull or switch branches, StackTUI runs `git diff` against the previous HEAD and checks which files changed. It cross-references those paths against a config section that maps source paths to services:

```toml
[[path_map]]
path = "webapp/"
services = ["webapp"]

[[path_map]]
path = "worker/"
services = ["worker"]

[[path_map]]
path = "shared/"
services = ["webapp", "worker"]
```

After a pull, rather than guessing which services need restarting, StackTUI highlights the affected ones in the service panel. One button restarts just those services, rebuilt from scratch. On a project with six or more services this saves a lot of "did I restart everything that needed it?" anxiety.

The TOML array-of-tables syntax (`[[path_map]]`) means you can add as many entries as you need. Shared libraries, config files that affect multiple services, monorepo layouts where a single change touches several things — all expressible.

### Log Tailing

The log panel at the bottom is the most-used part of the dashboard once it's open. A dropdown at the top of the panel lets you switch between sources: any service defined in your config (streams `docker logs -f` for that container), any log files you've pointed at, or the StackTUI internal log (useful when something's going wrong with the dashboard itself and you're trying to figure out why).

Switching sources is instant. No new terminal tab, no command to type. Pick from the dropdown and the panel starts streaming. It sounds small but it's the thing people mention first when they've been using it for a while.

## The Config File

Everything project-specific lives in `dashboard.toml` in your project root. Here's what a realistic config looks like:

```toml
[project]
name = "myapp"
dev_compose = "docker-compose.dev.yml"
prod_compose = "docker-compose.yml"
dev_url = "http://localhost:8080"
prod_url = "https://myapp.example.com"

[git]
repo_path = "."
remote = "origin"
webhook_secret = "your-webhook-secret-here"

[services]
infra = ["db", "redis", "nginx"]
app = ["webapp", "worker"]

[[log_files]]
name = "nginx access"
path = "/var/log/nginx/access.log"

[[log_files]]
name = "app errors"
path = "/var/log/myapp/error.log"

[[links]]
name = "App"
url = "http://localhost:8080"

[[links]]
name = "Admin"
url = "http://localhost:8080/admin"

[[path_map]]
path = "webapp/"
services = ["webapp"]

[[path_map]]
path = "worker/"
services = ["worker"]

[[path_map]]
path = "shared/"
services = ["webapp", "worker"]
```

The `[project]` section handles dev/prod mode. StackTUI auto-detects the environment by inspecting which containers are currently running, or you can force it with `--dev` or `--prod` flags. Different compose files, different URLs — the dashboard adapts.

The `[[links]]` entries turn up as quick-access buttons in the left panel. Handy for services with web UIs, admin panels, API docs, monitoring dashboards, whatever you need one-click access to.

Per-user settings live in a separate `.stacktui-user.toml` so team members can have their own preferences without touching the shared config. Right now that's just theme — press `T` to cycle through all twelve of them. Yes, twelve. I got a bit carried away. The themes are Nord, Gruvbox, Tokyo Night, Catppuccin, Dracula, and a handful of others. They all look good. No apologies.

## Under the Hood

### One File, ~1545 Lines

The entire application lives in `stacktui/dashboard.py`. This is a deliberate choice and also, objectively, a bit deranged.

The upside: the whole application is readable in a single sitting. There's no module tree to navigate, no hunting for where something is defined, no import chain to trace. You can `grep` for anything and find it immediately. For a tool that people might want to fork or customise, that has real value.

The downside is obvious — 1545 lines is a lot of code to hold in your head, and there's no module boundary forcing concerns to stay separated. I've kept it manageable with careful organisation: clear section headers, consistent naming, and a strict rule that UI code and business logic don't get tangled together. Whether that holds as the feature set grows is something I'll deal with when I get there.

### Textual: Reactive TUI Layout

[Textual](https://textual.textualize.io/) is the Python TUI framework that makes all of this look decent without several hundred lines of `curses` boilerplate. The layout is defined through widget composition — the dashboard is a Textual `App` that combines a `Header`, a three-column horizontal container, a log panel, and a `Footer`. Widgets declare reactive attributes, and when those attributes change, Textual re-renders the affected parts of the screen automatically.

Service status dots are reactive. When the background refresh worker updates service state, each relevant widget's reactive attribute changes, Textual diffs it, and only the dots that actually changed redraw. That's the Textual reactive model doing its thing — what would've been hundreds of lines of manual screen management becomes a handful of attribute assignments.

If you haven't used Textual, the team behind it have built something genuinely excellent. Rich TUIs in Python used to be a nightmare. Textual makes it feel almost like building a web app. The widget system, the CSS-like styling, the reactive attributes — it all just works. It's been one of the best parts of this project.

### Threading: Workers and Exclusive Locks

Any operation that touches Docker, git, or the filesystem runs in a Textual worker thread rather than on the main UI thread. Textual provides a `@work` decorator for this:

```python
@work(exclusive=True, thread=True)
async def restart_services(self, services: list[str]) -> None:
    # runs in a background thread
    # exclusive=True means only one at a time
    ...
```

The `exclusive=True` argument is the important bit. Only one orchestration operation can run at a time — if you click restart while a restart is already running, the second one doesn't start a competing parallel operation, it waits. This prevents the delightful failure mode where you double-click and end up with two Docker operations fighting over the same containers.

Output from workers streams live to the log panel via Textual's message-passing system. When you restart a service, you see the `docker compose` output appearing line-by-line in real time, not a silent spinner that eventually turns green.

Git operations — pull, checkout, fetch — are also threaded workers with the same exclusive locking. You can't accidentally kick off two pulls at once.

### Config Loading

Config is loaded using stdlib `tomllib`, added in Python 3.11. That's why 3.11 is the minimum version requirement — no YAML, no JSON, no third-party parsing library. TOML's array-of-tables syntax is a natural fit for this kind of project config. The `[[path_map]]` and `[[log_files]]` sections parse cleanly into lists of dicts with no special handling required.

## Built with OpenSpec and Claude Code

Here's the bit I find interesting to talk about, even though it has nothing to do with Docker.

StackTUI was built entirely using Claude Code with [OpenSpec](https://github.com/openspec-dev/openspec) managing the development workflow. Every feature went through a structured artifact cycle before any code was written:

1. **Proposal** — what problem are we solving and why does it need solving now
2. **Design** — how we're solving it, what alternatives we considered, what trade-offs we're accepting
3. **Specs** — testable requirements with concrete scenarios (WHEN this happens, THEN the system does that)
4. **Tasks** — a checkbox list of concrete implementation steps

When Claude Code picks up a session to implement a feature, it reads all four artifacts before touching a line of code. It knows why the feature exists, how it's supposed to behave, what design decisions were made, and exactly what done looks like.

The difference between this and ad-hoc AI coding is real. With vibes-driven development the agent has whatever's in the recent conversation and tends toward the simplest thing that satisfies the immediate prompt. With specs in place, it has the threading model, the config structure, the service ordering logic, the edge cases we considered — all the things that were decided in earlier artifacts. It doesn't accidentally work against those decisions because they're documented rather than implied.

On a project with as many moving parts as StackTUI — Textual widgets, Docker API calls, git operations, webhook handling, theme management, config parsing — having something to come back to genuinely mattered. I picked up sessions days apart and the agent slotted back in without me re-explaining the architecture, because the architecture was written down.

I've gone into the full workflow detail in [an earlier post](/2026/02/15/from-vague-idea-to-support-skill-my-openspec-workflow/) if you want the specifics. Short version: it's the first time I've felt like AI-assisted development is producing something I'd actually want to maintain.

## Getting Started

Python 3.11+ required. The only runtime dependency is Textual.

```bash
pip install stacktui
# or
uv add stacktui
```

If you want to try it before wiring it up to your own project, the repo includes a demo environment — five Docker Compose services ready to go:

```bash
git clone https://github.com/fuzzwah/stacktui.git
cd stacktui

# Start the demo services
docker compose -f demo/docker-compose.yml up -d

# Copy the demo config and run
cp dashboard.demo.toml dashboard.toml
stacktui --dev
```

Hit `http://localhost:8080` to generate some nginx and webapp traffic so the logs have something interesting to show. Takes about five minutes from clone to working dashboard.

For your own project, the repo has an [integration guide](https://github.com/fuzzwah/stacktui/blob/main/docs/integration.md) that walks through setting up `dashboard.toml` from scratch. The demo config is well-annotated if you prefer to learn by example — most of what you need to know is in there.

It's MIT licensed, it's on [GitHub](https://github.com/fuzzwah/stacktui), and I'd genuinely love to know if it ends up being useful to you. File issues, send PRs, or drop a comment. There's more to come.
