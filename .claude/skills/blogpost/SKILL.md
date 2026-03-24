---
name: blogpost
description: Use this skill when the user wants to write a blog post, create a blog post, make a new blog post, or uses the /blogpost command. Trigger phrases include "write a blog post", "create a blog post", "new blog post", "/blogpost".
license: MIT
metadata:
  author: Fuzzwah
  version: "1.0"
---

# Blog Post Writing Skill

Automated workflow for creating blog posts following Fuzzwah's writing style and the OpenSpec process.

## Workflow Steps

### 1. Capture Topic

- If the user provided a topic description with the command, use it directly
- If not, use `AskUserQuestion` to ask: "What's the blog post topic? Provide a brief description of what you want to write about."

### 2. Read Style Guide

- **CRITICAL**: Read `/Users/fuzzwah/Projects/blog/STYLE_GUIDE.md` before proceeding
- Internalize the key style points:
  - **Voice**: Conversational, warm, unpretentious Australian mate at the pub
  - **Language**: Australian English (organise, realise, favourite, colour, arse)
  - **Tone**: Enthusiastic, self-deprecating, direct and honest
  - **No emojis**, no corporate language, no AI filler phrases
  - **Structure**: Hook opening, bouncing between topics, forward-looking close
  - **Format**: Prose for narrative, lists for workflows/instructions

### 3. Start OpenSpec Change

- Invoke the `/opsx:new` skill with the topic description
- Frame the request as: "This is a blog post about [topic]. When writing, reference STYLE_GUIDE.md for Fuzzwah's conversational Australian voice and tone."
- Provide any additional context the user shared (references, links, key points)

### 4. Fast-Forward Artifacts

- Invoke the `/opsx:ff` skill to generate all artifacts (proposal, specs, design, tasks)
- Ensure the artifacts capture:
  - **Proposal**: Topic overview, key points to cover, why this post matters, target reader
  - **Specs**: Detailed outline with section headings, technical details, reference materials to cite
  - **Design**: Post structure, tone decisions for this specific topic, unique angle that makes this post stand out
  - **Tasks**: Concrete writing steps — draft each section, add code examples, write intro hook, write closing, review against style guide

### 5. Apply Implementation

- Invoke the `/opsx:apply` skill to write the actual blog post
- Ensure the post follows the required format:
  - **Filename**: `_posts/YYYY-MM-DD-slug-title.md`
  - **Frontmatter**:
    ```yaml
    ---
    title: Post Title Here
    date: YYYY-MM-DD HH:MM:SS +1100
    author: Fuzzwah
    description: One casual sentence describing the post.
    ---
    ```
  - **Timezone**: Australia/Sydney (+1100 AEDT or +1000 AEST)
  - **Content**: Following STYLE_GUIDE.md voice (conversational, Australian, enthusiastic, self-deprecating)

### 6. Verify Post

- Confirm the post file exists in `_posts/` directory
- Verify frontmatter is complete and properly formatted
- Check that the writing matches the style guide tone and voice
- Report completion to the user with the filename and a brief summary

## Key Constraints

### Voice and Style (from STYLE_GUIDE.md)

- **Conversational and warm**: Write like catching up with mates at a barbecue
- **Australian English**: Use `-ise` endings, "arse" not "ass", "favourite" not "favorite"
- **Enthusiastic by default**: Even mundane things get energy
- **Self-deprecating but not mopey**: Laugh at mistakes, acknowledge reality with a grin
- **Direct and honest**: Say what you think, don't hedge

### Sentence Structure

- Mix short punchy sentences with longer flowing ones
- Fragments are fine: "Super excited." / "Not happy Jan."
- Rhetorical questions pop up naturally
- Prefer prose over lists for narrative sections
- Use lists for workflows, setup instructions, sequential steps

### Things to Avoid

- Corporate language ("leverage", "synergy", "utilize", "facilitate")
- Excessive hedging ("I think perhaps it might be...")
- Generic AI filler ("In today's fast-paced world...", "Let's dive in!", "Without further ado...")
- Clickbait phrasing ("You won't believe...", "Here's why...")
- Emojis (occasional `:)` smiley is fine)

### Post Format Requirements

- **Date/time**: Current date in Australia/Sydney timezone
- **Description**: Casual one-sentence summary, not SEO keyword stuffing
- **Filename slug**: Lowercase with hyphens, descriptive of the topic

## Error Handling

- If the topic is unclear or too vague, use `AskUserQuestion` to clarify before starting OpenSpec
- If OpenSpec commands fail, report the error clearly and stop the workflow
- If `STYLE_GUIDE.md` is missing or unreadable, warn the user but continue (use general conversational Australian voice)
- If the user wants to modify the post after creation, suggest using `/opsx:continue` to iterate

## Integration with Existing Workflow

This skill wraps the existing OpenSpec blog post workflow defined in `CLAUDE.md`:
1. Normally: Plan mode → `/opsx:new` → `/opsx:ff` → `/opsx:apply`
2. With this skill: `/blogpost [topic]` → automatic execution of all steps

The skill maintains compatibility with the existing CLAUDE.md instructions while providing a streamlined single-command interface.

## Examples

### Example 1: Basic usage with topic
```
User: /blogpost how I use Claude Code skills to streamline my blog writing
```

The skill will:
1. Capture the topic: "how I use Claude Code skills to streamline my blog writing"
2. Read STYLE_GUIDE.md
3. Run `/opsx:new` with context about the topic and style requirements
4. Run `/opsx:ff` to generate artifacts
5. Run `/opsx:apply` to write the post
6. Report completion

### Example 2: Usage without topic
```
User: /blogpost
```

The skill will:
1. Ask: "What's the blog post topic? Provide a brief description of what you want to write about."
2. Wait for user response
3. Proceed with steps 2-6 from Example 1

### Example 3: Natural language trigger
```
User: I want to write a blog post about my experience with agentic coding workflows
```

The skill auto-triggers based on the phrase "write a blog post" and:
1. Extracts topic: "my experience with agentic coding workflows"
2. Proceeds with workflow steps 2-6

## Reference Documentation

See `references/workflow.md` for:
- Detailed examples of good topic descriptions
- Sample frontmatter and post structure
- Common style guide violations and how to fix them
- OpenSpec artifact patterns for blog posts
