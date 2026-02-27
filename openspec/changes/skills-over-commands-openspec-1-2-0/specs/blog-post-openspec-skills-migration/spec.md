## ADDED Requirements

### Requirement: Post covers the migration mechanics
The post SHALL document what `openspec update --force` did: removed 7 old command files (`/opsx:new`, `/opsx:ff`, `/opsx:explore`, `/opsx:apply`, `/opsx:archive`, plus deprecated `/opsx:continue`, `/opsx:sync`, `/opsx:bulk-archive`, `/opsx:verify`, `/opsx:onboard`) and created 4 new skills (`openspec-propose`, `openspec-apply-change`, `openspec-explore`, `openspec-archive-change`) with thin command wrappers remaining.

#### Scenario: Migration mechanics are concrete
- **WHEN** a reader encounters the "What Changed" section
- **THEN** they SHALL understand exactly which files were removed, which skills were created, and what the `--force` flag did — with no ambiguity about the before/after state

### Requirement: Post explains the commands vs skills distinction
The post SHALL explain that in Claude Code, commands are one-shot instructions the agent reads and acts on once, while skills are persistent context modules loaded at session start that the agent draws on across every turn.

#### Scenario: Distinction is illustrated with a concrete example
- **WHEN** the conceptual distinction is introduced
- **THEN** it SHALL be grounded in a specific example from the OpenSpec migration (e.g., `/opsx:apply` as a command vs `openspec-apply-change` as a skill)

### Requirement: Post makes the case for spec-driven development overhead
The post SHALL honestly acknowledge the proposal → design → specs → tasks → implement → archive cycle is heavyweight, then make the affirmative case: the overhead prevents agent drift, creates an audit trail, and forces thinking before building.

#### Scenario: Overhead argument is honest before it is positive
- **WHEN** the spec-driven overhead section is read
- **THEN** the acknowledgment of cost SHALL come before the argument for value — not minimised or hedged

### Requirement: Post uses openspec update as a good AI tooling example
The post SHALL use the `openspec update` experience to illustrate good AI tooling design: one command handles mechanical file changes, the human handles judgment calls (updating CLAUDE.md and memory files with context-aware reference changes).

#### Scenario: Division of labour is clear
- **WHEN** the tooling design section is read
- **THEN** it SHALL be clear what the tool did automatically vs what required human judgment, and why that division is the right one

### Requirement: Post argues for .claude/ as first-class infrastructure
The post SHALL argue that the `.claude/` directory is not a config folder — it is versioned, reviewed project infrastructure that tells collaborators (human and AI) how the project works. Stale references in `.claude/` are a bug, just like stale references in code.

#### Scenario: Infrastructure argument is actionable
- **WHEN** a reader finishes the post
- **THEN** they SHALL have a concrete mental model for treating `.claude/` the way they treat a Makefile or CI config — something that gets reviewed, kept current, and considered part of the project

### Requirement: Post is written in the established blog voice
The post SHALL be written in the author's established voice: conversational, warm, Australian English, self-deprecating where appropriate, direct, no corporate filler, no AI phrases, no emojis.

#### Scenario: Voice is consistent with existing posts
- **WHEN** the post is compared against recent posts (e.g., the StackTUI post, the OpenSpec workflow post)
- **THEN** the tone, vocabulary, sentence structure, and humour SHALL be consistent — same author, same register

### Requirement: Post has correct frontmatter and filename
The post SHALL use the correct Jekyll frontmatter format and be saved at `_posts/2026-02-27-skills-over-commands-openspec-1-2-0.md`.

#### Scenario: Frontmatter is valid
- **WHEN** the post file is created
- **THEN** it SHALL include `title`, `date` (with Sydney timezone offset), `author: Fuzzwah`, and a casual `description` — no extra fields, no SEO stuffing
