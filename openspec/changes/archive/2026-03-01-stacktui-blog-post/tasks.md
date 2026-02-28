## 1. Setup

- [x] 1.1 Read STYLE_GUIDE.md to calibrate voice before writing
- [x] 1.2 Review existing blog posts for tone reference
- [x] 1.3 Create post file `_posts/2026-02-27-stacktui-docker-compose-tui-dashboard.md` with correct frontmatter

## 2. Opening and Problem Section

- [x] 2.1 Write the opening hook — concrete scenario of managing a Docker Compose stack across multiple terminal windows (prod issue at midnight, four windows open)
- [x] 2.2 Introduce StackTUI as the solution in one or two tight paragraphs — what it is, where it runs, what it gives you

## 3. Features Walkthrough

- [x] 3.1 Write the service monitoring section — live health indicators, 10-second refresh, coloured status dots
- [x] 3.2 Write the log tailing section — switchable dropdown between container logs and local log files
- [x] 3.3 Write the service orchestration section — smart ordering (infra first, app services rebuilt with `--build`), stop/start/restart
- [x] 3.4 Write the git integration section — pull, branch/ref switching, `[[path_map]]` for detecting affected services
- [x] 3.5 Write the remaining features — webhook notifications, dev/prod auto-detection, 12 themes with `T` key, self-update

## 4. TOML Config Section

- [x] 4.1 Write the config section explaining how `dashboard.toml` adapts the dashboard to any project
- [x] 4.2 Include a representative config snippet showing the key sections

## 5. Architecture Section

- [x] 5.1 Write the architecture section: single-file design tradeoff, Textual reactive layout (Header → 3-column → log → Footer)
- [x] 5.2 Cover Docker queries (`docker compose ps --format json`) and deduplication approach
- [x] 5.3 Cover the `@work(exclusive=True, thread=True)` workers — why this prevents concurrent ops and how output streams live to the log panel
- [x] 5.4 Cover config loading (stdlib `tomllib`, per-user `.stacktui-user.toml`)

## 6. OpenSpec + Claude Code Workflow Section

- [x] 6.1 Write the development workflow section — the artifact flow (proposal → design → delta specs → task list → implementation)
- [x] 6.2 Explain how Claude Code reads specs and task lists to write code, and why specs keep the AI grounded

## 7. Quick Start and Closing

- [x] 7.1 Write the quick-start section — installation (`pip install stacktui` / `uv add stacktui`), demo environment for hands-on trial, link to GitHub and integration guide
- [x] 7.2 Write the closing — looking forward, what's next for StackTUI

## 8. Review

- [x] 8.1 Read the full post aloud — check voice stays warm and Australian throughout, no documentation drift
- [x] 8.2 Check spelling: Australian English (`organise`, `realise`, `colour`, `favourite`)
- [x] 8.3 Verify no banned phrases (`leverage`, `dive in`, `In today's fast-paced world`, etc.)
- [x] 8.4 Confirm frontmatter is correct (date, timezone, author, description)
