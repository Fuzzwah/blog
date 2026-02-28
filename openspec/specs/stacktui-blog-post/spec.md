## Requirements

### Requirement: Blog post introduces StackTUI with a problem-first hook
The post SHALL open by establishing the problem — managing a Docker Compose stack means juggling multiple terminal windows for logs, status checks, restarts, and git operations. The hook SHALL be concrete and relatable (e.g., fixing a prod issue at midnight across four terminal windows).

#### Scenario: Reader immediately understands the problem being solved
- **WHEN** a reader opens the post
- **THEN** they understand within the first two paragraphs what problem StackTUI solves and why that problem is worth solving

### Requirement: Post covers the core features with concrete detail
The post SHALL cover these features with enough specificity that readers know what they're getting:
- Live service health monitoring (10-second refresh, coloured status dots)
- Real-time log tailing (switchable between container logs and local log files)
- Service orchestration with smart ordering (infra first, app services rebuilt with `--build`)
- Git integration (pull, branch/ref switching, auto-detect affected services via `[[path_map]]`)
- Webhook notifications (GitHub push event banner alerts)
- Dev/prod mode auto-detection
- 12 themes with `T` key cycling, persistent per user
- Self-update on startup

#### Scenario: Reader can identify whether StackTUI meets their needs
- **WHEN** a reader scans the features section
- **THEN** they can determine whether the tool addresses their specific workflow without reading the full README

### Requirement: Post explains the TOML config system
The post SHALL explain how `dashboard.toml` adapts StackTUI to any Docker Compose project. It SHALL include a representative config snippet showing the key sections (project name, services, git config, path_map).

#### Scenario: Reader understands how to configure StackTUI for their own project
- **WHEN** a reader reaches the config section
- **THEN** they understand the structure of `dashboard.toml` and which sections are required vs optional

### Requirement: Post covers the Textual architecture honestly
The post SHALL explain the technical architecture with enough depth to satisfy a technically curious reader:
- Single-file design (~1545 lines, `stacktui/dashboard.py`) and the tradeoff involved
- Textual reactive TUI layout (Header → 3-column top pane → log panel → Footer)
- Docker queries via `docker compose ps --format json`, parsed and deduplicated
- `@work(exclusive=True, thread=True)` workers for orchestration and git operations
- Config via stdlib `tomllib`, per-user theme in `.stacktui-user.toml`

#### Scenario: A Python developer understands the implementation approach
- **WHEN** a reader with Python experience reads the architecture section
- **THEN** they understand how Textual's reactive model is used, why workers prevent concurrent ops, and what the single-file tradeoff means in practice

### Requirement: Post includes the OpenSpec + Claude Code development workflow angle
The post SHALL include a section explaining that StackTUI is built using a spec-driven AI development workflow. It SHALL cover: the artifact flow (proposal → design → delta specs → task list → implementation), how Claude Code reads specs and task lists to write code, and why this keeps the AI grounded in documented requirements.

#### Scenario: Reader understands the meta-story of how StackTUI is built
- **WHEN** a reader interested in AI-assisted development reads this section
- **THEN** they understand the OpenSpec workflow and how it differs from ad-hoc AI coding

### Requirement: Post ends with a clear quick-start path
The post SHALL close with installation instructions (`pip install stacktui` or `uv add stacktui`), the demo environment for hands-on trial, and a link to the GitHub repo and integration guide.

#### Scenario: Reader can get StackTUI running within 5 minutes
- **WHEN** a motivated reader reaches the end of the post
- **THEN** they have everything they need to install StackTUI and run the demo environment

### Requirement: Post uses correct frontmatter and filename conventions
The post SHALL use the filename format `YYYY-MM-DD-slug.md` in `_posts/` and include valid YAML frontmatter with `title`, `date` (with Australia/Sydney timezone), `author: Fuzzwah`, and a casual `description`.

#### Scenario: Post renders correctly on the Jekyll site
- **WHEN** the post file is added to `_posts/`
- **THEN** Jekyll builds without errors and the post appears with correct metadata
