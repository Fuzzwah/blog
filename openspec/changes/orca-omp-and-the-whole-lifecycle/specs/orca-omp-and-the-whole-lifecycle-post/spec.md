## ADDED Requirements

### Requirement: Blog post file with correct frontmatter
The post SHALL be created as `_posts/2026-09-07-orca-omp-and-the-whole-lifecycle.md` with valid YAML frontmatter including title, date (Australia/Sydney timezone, `+1000` AEST for September), author (Fuzzwah), and a casual one-sentence description.

#### Scenario: Post file exists with valid frontmatter
- **WHEN** the post file is created
- **THEN** it SHALL contain frontmatter with title, date in `+1000` timezone, author set to "Fuzzwah", and a concise description

### Requirement: Post opens by calling back to the split-brain post
The post SHALL open with a hook that references the previous post's ending ("One machine. One brain.") and establishes that the setup has since been rebuilt.

#### Scenario: Opening hooks the reader and establishes continuity
- **WHEN** a reader starts the post
- **THEN** the opening SHALL reference the split-brain post and frame the conduit setup as superseded, not concluded

### Requirement: Post explains what broke the previous setup
The post SHALL explain that the conduit setup wrapped agent CLIs and relied on `claude -p`, and that Anthropic removed `claude -p` from what the subscription covers — and SHALL draw the single-vendor-dependence lesson without trashing conduit.

#### Scenario: The breakage and lesson are clear
- **WHEN** a reader finishes the opening sections
- **THEN** they SHALL understand why the conduit setup stopped working and why the author frames dependence on one vendor's CLI as the underlying mistake

### Requirement: Post introduces Orca accurately
The post SHALL describe Orca (stablyai) as an agent-agnostic ADE that runs multiple coding agents side by side — Claude Code, Codex, OpenCode, DeepSeek TUI, pi, omp — each in its own isolated git worktree, tracked in one place, using the user's own subscriptions/keys.

#### Scenario: Orca's model is described correctly
- **WHEN** a reader finishes the Orca section
- **THEN** they SHALL understand that Orca's core property is agent-agnosticism: engines are interchangeable without disturbing the worktrees, sessions, or history around them

### Requirement: Post describes the headless remote deployment
The post SHALL describe Orca running as a headless server on the always-on Linux box, with the author working entirely remotely: MacBooks pairing to the Orca server over Tailscale and the phone companion used for notifications and follow-ups. It SHALL tie this back to the split-brain problem.

#### Scenario: Remote access pattern is described
- **WHEN** a reader finishes the deployment section
- **THEN** they SHALL understand that no agents run on the laptops, all sessions live on the box, and the "which machine" question is gone

### Requirement: Post explains omp's place and model-tier economics
The post SHALL introduce omp (oh-my-pi) as the agent engine for day-to-day work and explain the two-tier model approach: a fast cheap default model (deepseek v4 flash) for routine tasks, with strong models (codex/sol) on tap for the review gate — framed as the evolution of the January stack post's manual two-model approach.

#### Scenario: Model tiers are explained
- **WHEN** a reader finishes the engine section
- **THEN** they SHALL understand that routine work runs on a cheap fast model and the strong model is reserved for the parts that matter, automatically rather than by manual switching

### Requirement: Post walks the full change lifecycle
The post SHALL describe the OpenSpec pipeline (explore → propose → apply → archive), the mandatory strong-model review gate after propose (what it checks, that it only touches planning artifacts), and the tracking layer: every change becomes a GitHub issue whose body mirrors `tasks.md` as checkboxes, linked as a card on the Backlog board with the Status lifecycle (`Not started` → `In progress` → `Ready to archive` → `Deferred` → `Done`), tasks ticked in both places, archive closing the issue and moving the card to Done.

#### Scenario: Lifecycle is concretely walkable
- **WHEN** a reader finishes the pipeline sections
- **THEN** they SHALL be able to describe the end-to-end path of a change: idea → explore → propose → strong-model review → land on main → issue raised and carded → apply with tasks ticked twice → archive closes the issue

### Requirement: Post lands the meta point about itself
The post SHALL note that it is itself going through this pipeline — the blog repo has its own `openspec/changes/` directory and the post is an OpenSpec change being drafted against its artifacts.

#### Scenario: Meta point lands
- **WHEN** a reader finishes the post
- **THEN** they SHALL understand the author runs even blog posts through the same change discipline

### Requirement: Post closes looking forward
The post SHALL close with a forward-looking statement — the pipeline as the thing carrying current work (e.g. the reports rollout) and what the setup feels like as a whole system.

#### Scenario: Closing looks forward
- **WHEN** a reader finishes the post
- **THEN** the final section SHALL look forward rather than merely summarise

### Requirement: Superseded posts get update notices
The posts `_posts/2026-04-02-split-brain-coding-agent.md` and `_posts/2026-03-28-conductor-orchestrating-ai-agents-across-repos.md` SHALL carry `update_notice` frontmatter pointing to the new post, so readers of the old setups find the current one.

#### Scenario: Notices point to the new post
- **WHEN** the superseded posts are rendered
- **THEN** each SHALL show an update notice linking to the new post

### Requirement: Post follows STYLE_GUIDE.md voice
The post SHALL be written in Fuzzwah's conversational Australian voice: enthusiastic, direct, self-deprecating where apt, Australian English spelling, no emojis, no corporate language, no AI filler, hooks and forward closes per the style guide.

#### Scenario: Voice matches style guide
- **WHEN** the post is reviewed against STYLE_GUIDE.md
- **THEN** it SHALL use Australian English spelling, conversational tone, and avoid corporate language, excessive hedging, clickbait phrasing, and generic AI filler
