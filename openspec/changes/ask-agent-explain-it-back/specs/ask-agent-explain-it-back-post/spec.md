## ADDED Requirements

### Requirement: Blog post file with correct frontmatter
The post SHALL be created as `_posts/2026-03-28-ask-the-agent-to-explain-it-back.md` with valid YAML frontmatter including title, date (Australia/Sydney timezone), author (Fuzzwah), and a casual one-sentence description.

#### Scenario: Post file exists with valid frontmatter
- **WHEN** the post file is created
- **THEN** it SHALL contain frontmatter with title, date in `+1100` or `+1000` timezone, author set to "Fuzzwah", and a concise description

### Requirement: Post explains the setup
The post SHALL establish the context: working with multiple AI coding agents in parallel, each bootstrapped with a CLAUDE.md file that serves as institutional knowledge (project structure, conventions, deployment, architecture).

#### Scenario: Reader understands the CLAUDE.md setup
- **WHEN** a reader finishes the opening section
- **THEN** they SHALL understand that CLAUDE.md files are loaded as context for every new agent conversation across multiple repos

### Requirement: Post describes the core technique
The post SHALL explain the technique of spinning up a fresh agent and asking it to explain the project's workflow back to the author, rather than diving straight into a task.

#### Scenario: Technique is clearly described
- **WHEN** a reader reaches the technique section
- **THEN** they SHALL understand the specific action: start a new agent session, ask it to explain the project back to you, and use its response to evaluate your context docs

### Requirement: Post covers what the technique surfaces
The post SHALL describe the four categories of issues this technique reveals: stale information, inconsistencies, implicit knowledge gaps, and ambiguities. Each SHALL include a concrete example or illustration.

#### Scenario: Fish/bash inconsistency example
- **WHEN** the inconsistencies category is discussed
- **THEN** the post SHALL include the specific example of documenting fish shell conventions while showing bash syntax elsewhere

#### Scenario: All four categories covered
- **WHEN** a reader finishes the "what it surfaces" section
- **THEN** they SHALL have encountered stale information, inconsistencies, implicit knowledge, and ambiguities as distinct categories

### Requirement: Post explains why fresh agents are effective test readers
The post SHALL articulate why a fresh agent is uniquely suited for this: no prior context, no memory of past conversations, no ability to fill in gaps from experience. If the agent gets it wrong, the docs need fixing.

#### Scenario: Fresh agent advantage is clear
- **WHEN** a reader finishes the "why this works" section
- **THEN** they SHALL understand that a fresh agent's limitations (no memory, no gap-filling) are precisely what makes it a good documentation auditor

### Requirement: Post describes the feedback loop
The post SHALL explain how the conversation becomes collaborative editing: agent explains, author corrects, agent updates the CLAUDE.md in the same session. The context improves with each pass.

#### Scenario: Feedback loop is actionable
- **WHEN** a reader finishes the feedback loop section
- **THEN** they SHALL understand the complete cycle: explanation → correction → immediate doc update in the same session

### Requirement: Post includes specific prompts to try
The post SHALL provide at least four specific prompts readers can use, along with what each one tests. These SHALL be presented in a scannable format (list or similar).

#### Scenario: Prompts are provided
- **WHEN** a reader looks for actionable prompts
- **THEN** they SHALL find at least four specific example prompts with descriptions of what each one validates

### Requirement: Post conveys the meta-insight
The post SHALL communicate the core insight: agent output quality is directly proportional to context quality, and using agents as context reviewers creates a virtuous cycle where every fix improves all future agent conversations.

#### Scenario: Meta-insight resonates
- **WHEN** a reader finishes the post
- **THEN** they SHALL understand the compounding value: each context improvement benefits every future agent session across every conversation

### Requirement: Post follows STYLE_GUIDE.md voice
The post SHALL be written in Fuzzwah's conversational Australian voice: enthusiastic, self-deprecating, direct, using Australian English spelling, no emojis, no corporate language, no AI filler phrases.

#### Scenario: Voice matches style guide
- **WHEN** the post is reviewed against STYLE_GUIDE.md
- **THEN** it SHALL use Australian English spelling (-ise, arse, favourite, colour), conversational tone, and avoid corporate language, excessive hedging, and generic AI filler

### Requirement: Post opens with a hook and closes looking forward
The post SHALL open with a hook (quip, observation, or punchy opener) per STYLE_GUIDE.md and close with a forward-looking statement.

#### Scenario: Opening hooks the reader
- **WHEN** a reader starts the post
- **THEN** the first sentence or paragraph SHALL be engaging and conversational, not a dry summary

#### Scenario: Closing looks forward
- **WHEN** a reader finishes the post
- **THEN** the final section SHALL look forward or invite further exploration of the technique
