## 1. Setup

- [x] 1.1 Create post file `_posts/2026-03-28-conductor-workspaces-orchestrating-ai-agents-across-repos.md` with correct YAML frontmatter (title, date 2026-03-28 10:00:00 +1100, author Fuzzwah, casual description)

## 2. Write Post Content

- [x] 2.1 Write opening hook — the reality of managing six repos, too many terminal tabs, the moment you realise you need a system not just tools
- [x] 2.2 Write "The Platform" section — brief sketch of the components (Discord bot, Django app, data pipeline, shared DB utils, OAuth service, docs site), emphasise the shared PostgreSQL database with time-series partitioning as the connective tissue, keep it generic (no product name)
- [x] 2.3 Write "Conductor Workspaces" section — explain the central orchestration repo, how each sub-repo gets its own workspace with an AI agent, symlinks back to the central repo for shared specs and config, include a directory structure example
- [x] 2.4 Write "Spec-First, Not Code-First" section — explain the OpenSpec pipeline (explore → propose → apply → archive), why explore mode matters (prevents agents rushing into code), how specs committed to main are immediately visible to all agents via symlinks, reference prior blog posts for OpenSpec details
- [x] 2.5 Write "CLAUDE.md as Institutional Memory" section — repo-specific vs project-wide CLAUDE.md, what goes in each (database access patterns, deployment procedures, shell conventions, coding standards), how this keeps independent agents aligned without human supervision, include a snippet example showing what a CLAUDE.md entry looks like
- [x] 2.6 Write "What Actually Works" section — bullet list: parallel agent work across repos, symlinked specs visible everywhere, agents providing deploy commands alongside code, shared database schema doc, agents surfacing CLAUDE.md inconsistencies
- [x] 2.7 Write "What's Still Tricky" section — bullet list with brief explanation: keeping CLAUDE.md accurate, balancing context vs conciseness, discipline around explore mode
- [x] 2.8 Write closing paragraph — this is a workflow that evolves, invite readers to try the patterns, forward-looking (what's next for the workflow), casual sign-off

## 3. Review

- [x] 3.1 Review entire post against STYLE_GUIDE.md — check for Australian English, conversational voice, no AI filler, no corporate language, self-deprecating humour present, technical sections use good structure (headers, lists where appropriate, prose for narrative)
- [x] 3.2 Verify frontmatter is complete and filename matches convention
