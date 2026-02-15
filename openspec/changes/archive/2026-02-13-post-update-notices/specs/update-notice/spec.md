## ADDED Requirements

### Requirement: Post update notice frontmatter schema
A blog post MAY include an `update_notice` object in its YAML frontmatter. The object SHALL contain three required fields:
- `message` (string): A brief explanation of what changed or why the post is outdated
- `url` (string): The relative URL path to the newer post
- `link_text` (string): The clickable text for the link to the newer post

#### Scenario: Post with update_notice frontmatter
- **WHEN** a post includes a valid `update_notice` object with `message`, `url`, and `link_text` fields
- **THEN** the post layout SHALL render a notification banner between the post header and post content

#### Scenario: Post without update_notice frontmatter
- **WHEN** a post does not include `update_notice` in its frontmatter
- **THEN** the post layout SHALL render normally with no notification banner and no empty markup

### Requirement: Update notice rendering
The update notice SHALL be rendered as a `div` with class `update-notice`, containing:
- A bold "Heads up:" label
- The `message` text
- A link to `url` with `link_text` as the anchor text, followed by a right arrow

The notice SHALL appear after the post title and meta line, before the post content body.

#### Scenario: Notice banner content and placement
- **WHEN** a post with `update_notice` frontmatter is rendered
- **THEN** a `.update-notice` div SHALL appear between the `<header>` element and the `{{ content }}` block
- **AND** the div SHALL contain the message text and a link to the specified URL

### Requirement: Update notice styling
The update notice SHALL be styled as a subtle info callout with:
- A left accent border using `$info-color`
- A light background tint in light mode
- A dark-appropriate background in dark mode (via `prefers-color-scheme: dark`)
- Sans-serif font at a slightly smaller size than body text
- Compact padding that doesn't dominate the page

#### Scenario: Light mode appearance
- **WHEN** the user's system is in light mode
- **THEN** the notice SHALL display with a light blue-tinted background and blue left border

#### Scenario: Dark mode appearance
- **WHEN** the user's system is in dark mode (prefers-color-scheme: dark)
- **THEN** the notice SHALL display with a dark semi-transparent blue background and blue left border

### Requirement: Initial posts receive update notices
The following existing posts SHALL receive `update_notice` frontmatter:
- `2026-01-07-my-agentic-coding-stack.md` — linking to the workflow comparison post
- `2026-01-08-adopting-github-spec-kit.md` — linking to the Spec-Kit to OpenSpec switching post

#### Scenario: Agentic coding stack post notice
- **WHEN** a reader views "My Agentic Coding Stack"
- **THEN** they SHALL see a notice indicating the workflow has evolved, with a link to the workflow comparison post

#### Scenario: Spec-Kit adoption post notice
- **WHEN** a reader views "Adopting GitHub Spec-Kit"
- **THEN** they SHALL see a notice indicating the author has switched to OpenSpec, with a link to the switching post
