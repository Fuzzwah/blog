## ADDED Requirements

### Requirement: Blog post file with correct frontmatter
The post SHALL be created at `_posts/2026-02-19-stacktui-a-tui-dashboard-for-docker-compose.md` with frontmatter containing title, date (2026-02-19 10:00:00 +1100), author (Fuzzwah), and a casual one-sentence description.

#### Scenario: Frontmatter is complete and correctly formatted
- **WHEN** the post file is created
- **THEN** it SHALL contain valid YAML frontmatter with title, date in Australia/Sydney timezone, author set to "Fuzzwah", and a conversational description

### Requirement: Opening hook establishes the problem
The post SHALL open with a personal, punchy hook about the frustration of managing Docker Compose projects from multiple terminal tabs/commands. No heading on the opening — it leads directly into the narrative.

#### Scenario: Reader encounters the opening
- **WHEN** a reader starts the post
- **THEN** they SHALL encounter a relatable problem statement about Docker Compose terminal sprawl, followed by the announcement that StackTUI is now open source

### Requirement: "What StackTUI Does" section provides human overview
The post SHALL include a section titled "What StackTUI Does" that describes the tool in human terms — what the experience feels like, not a technical specification.

#### Scenario: Reader wants to understand the tool quickly
- **WHEN** a reader finishes this section
- **THEN** they SHALL understand that StackTUI is a single-terminal dashboard for monitoring, managing, and debugging Docker Compose stacks

### Requirement: Features section uses bullet list format
The post SHALL include a "Features" section with 8-10 bullet points covering: service monitoring, log tailing, service orchestration, git integration, webhook notifications, themes, TOML config, and dev/prod modes.

#### Scenario: Features are scannable
- **WHEN** a reader scans the features section
- **THEN** each feature SHALL be a single casual bullet point, not documentation-style prose

### Requirement: Demo section with quick-start code block
The post SHALL include a section about the demo environment with a code block showing the commands to clone, start demo services, install dependencies, and run the dashboard.

#### Scenario: Reader wants to try StackTUI without their own project
- **WHEN** a reader follows the demo instructions
- **THEN** they SHALL have a working StackTUI instance with 5 demo services (webapp, worker, nginx, db, redis)

### Requirement: "Using It With Your Own Project" section
The post SHALL include a brief section on how to configure StackTUI for an existing Docker Compose project via the TOML config file.

#### Scenario: Reader has their own Compose project
- **WHEN** a reader finishes this section
- **THEN** they SHALL understand that wiring up StackTUI involves copying a config file and editing it, with the demo config as a reference

### Requirement: "Built With Claude Code and OpenSpec" section
The post SHALL include a section connecting the build process to the blog's ongoing themes about agentic coding and spec-driven development. This SHALL be framed as part of the story, not a product pitch.

#### Scenario: Section connects to blog themes
- **WHEN** a reader finishes this section
- **THEN** they SHALL understand that StackTUI was built using Claude Code with OpenSpec managing the spec-driven workflow, and that the recent blog posts about these tools culminated in this project

### Requirement: "What I've Learnt" section with specific lessons
The post SHALL include a "What I've Learnt" section with numbered lessons specific to the StackTUI build experience.

#### Scenario: Lessons are concrete and specific
- **WHEN** a reader reads the lessons
- **THEN** each lesson SHALL reference a specific aspect of the build (Textual framework, Docker API, git integration, dev/prod modes)

### Requirement: Forward-looking close
The post SHALL close with a brief, casual paragraph pointing forward — inviting readers to try StackTUI, file issues, or contribute. SHALL link to the GitHub repo.

#### Scenario: Post ends with invitation
- **WHEN** a reader finishes the post
- **THEN** they SHALL see a call to action to try the project and a link to the GitHub repo

### Requirement: Voice matches STYLE_GUIDE.md
The post SHALL use Fuzzwah's conversational Australian voice throughout — enthusiastic, self-deprecating, direct. Australian English spelling. No emojis, no corporate language, no AI filler phrases.

#### Scenario: Voice consistency check
- **WHEN** the post is reviewed against STYLE_GUIDE.md
- **THEN** it SHALL use Australian English (organise, realise, colour), include self-deprecating humour, avoid corporate language and AI filler, and maintain an enthusiastic but genuine tone
