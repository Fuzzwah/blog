### Requirement: Blog post file exists with correct format
The blog post SHALL be created as a markdown file in `_posts/` with the filename format `YYYY-MM-DD-slug-title.md` and SHALL include valid YAML frontmatter with title, date (Australia/Sydney timezone), author (Fuzzwah), and description fields.

#### Scenario: Post file created with correct frontmatter
- **WHEN** the blog post is written
- **THEN** a file exists at `_posts/2026-02-15-openspec-workflow-claude-code.md` with complete YAML frontmatter including title, date with +1100 timezone, author set to "Fuzzwah", and a casual one-sentence description

### Requirement: Post covers the current workflow
The blog post SHALL describe the workflow of using Claude Code's VSCode extension with the Opus model in plan mode, prompting with concise feature descriptions, and using CLAUDE.md to automatically trigger OpenSpec changes when exiting plan mode.

#### Scenario: Workflow description is present
- **WHEN** a reader reads the post
- **THEN** they understand the flow: start in plan mode → prompt with concise description → CLAUDE.md triggers opsx:new on exit → opsx:ff generates artifacts → opsx:apply implements

### Requirement: Post introduces support skill generation concept
The blog post SHALL introduce and explore the idea of extending `/opsx:archive` to auto-generate a skill file for debugging and supporting the feature that was just built, completing the lifecycle from ideation to ongoing support.

#### Scenario: Support skill concept is explained
- **WHEN** a reader reads the post
- **THEN** they understand the proposed extension: archiving a change would generate a Claude Code skill file containing context about the feature, how it works, and how to debug it

### Requirement: Post follows STYLE_GUIDE.md voice
The blog post SHALL be written in Fuzzwah's conversational Australian voice: warm, enthusiastic, self-deprecating, using Australian English spelling, with no emojis, no corporate language, and no AI filler phrases.

#### Scenario: Voice matches style guide
- **WHEN** the post is reviewed against STYLE_GUIDE.md
- **THEN** it uses Australian English (organise, realise, colour), casual tone, genuine enthusiasm, self-deprecating humour, and avoids corporate language and AI filler

### Requirement: Post structure follows style guide patterns
The blog post SHALL open with a hook (not a dry summary), flow through topics with energy, and close looking forward. Technical sections SHALL use lists for workflows and prose for narrative.

#### Scenario: Post opens with a hook
- **WHEN** a reader starts the post
- **THEN** the opening line is engaging — a quip, observation, or punchy statement that draws them in

#### Scenario: Post closes looking forward
- **WHEN** a reader reaches the end
- **THEN** the closing teases what's coming next or invites the reader to try the workflow themselves
