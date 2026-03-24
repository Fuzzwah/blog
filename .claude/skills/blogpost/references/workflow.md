# Blog Post Workflow Reference

Detailed reference for the `/blogpost` skill and how it integrates with OpenSpec and the style guide.

## Overview

The `/blogpost` skill automates the complete blog post creation workflow:
1. Topic capture
2. Style guide reference
3. OpenSpec change creation
4. Artifact generation
5. Post implementation
6. Verification

This document provides examples, patterns, and troubleshooting guidance.

---

## Good Topic Descriptions

The topic description you provide should be concise but include enough detail for the agent to understand what you want to write about.

### ✅ Good Examples

```
/blogpost how I use Claude Code skills to streamline my blog writing workflow

/blogpost my experience moving from manual code reviews to agentic workflows

/blogpost setting up a Jekyll blog with GitHub Pages and custom domains

/blogpost the best games I played in 2025 and why they kicked arse

/blogpost weekend trip to the Sunshine Coast and a close encounter with a jellyfish
```

**Why these work:**
- Clear topic focus
- Enough context to understand the angle
- Natural phrasing (how you'd describe it to a mate)

### ❌ Poor Examples

```
/blogpost blog post

/blogpost write something

/blogpost tech
```

**Why these don't work:**
- Too vague, no clear topic
- No context or direction
- Agent will need to ask follow-up questions

---

## Post Frontmatter Structure

Every blog post requires YAML frontmatter at the top of the file. Here's the required structure:

```yaml
---
title: Your Post Title Here
date: 2026-02-15 18:30:00 +1100
author: Fuzzwah
description: A casual one-sentence summary that sounds like you're telling a mate what the post is about.
---
```

### Frontmatter Field Details

**title:**
- Natural, conversational phrasing
- Not clickbait ("How I X" not "You Won't Believe How I X")
- Can include casual language ("kick arse", "bloody good")

**date:**
- Format: `YYYY-MM-DD HH:MM:SS +ZZZZ`
- Timezone: Australia/Sydney
  - `+1100` during AEDT (Daylight Saving Time, October-April)
  - `+1000` during AEST (Standard Time, April-October)
- Use current date/time when creating the post

**author:**
- Always `Fuzzwah`

**description:**
- Single sentence (can be a long sentence)
- Casual and conversational
- Not SEO keyword stuffing
- Should give a genuine preview of what's in the post

### Example Frontmatter Variations

**Technical post:**
```yaml
---
title: Setting Up Claude Code Skills for Blog Automation
date: 2026-02-15 18:30:00 +1100
author: Fuzzwah
description: How I created a custom skill to streamline writing blog posts with Claude Code, OpenSpec, and a style guide that keeps my voice consistent.
---
```

**Personal/lifestyle post:**
```yaml
---
title: Sunshine Coast Adventure and a Jellyfish Encounter
date: 2026-02-15 18:30:00 +1100
author: Fuzzwah
description: Headed up the coast for a weekend of sun, surf, and a close call with a bluebottle that nearly ruined the whole damn trip.
---
```

**Gaming post:**
```yaml
---
title: November's Gaming Releases Are Absolutely Stacked
date: 2026-02-15 18:30:00 +1100
author: Fuzzwah
description: The gaming universe is hectic right now and November is packed with big name releases that are going to destroy my free time.
---
```

---

## Filename Pattern

Blog posts are saved in `_posts/` with the pattern:

```
_posts/YYYY-MM-DD-slug-title.md
```

### Slug Creation Rules

- Lowercase only
- Words separated by hyphens (`-`)
- No special characters or punctuation
- Descriptive enough to understand the topic

**Examples:**
```
_posts/2026-02-15-claude-code-skills-blog-automation.md
_posts/2026-02-15-sunshine-coast-jellyfish-encounter.md
_posts/2026-02-15-november-gaming-releases-stacked.md
```

---

## Style Guide Common Violations and Fixes

### ❌ Violation: AI Filler Phrases

**Wrong:**
> "In today's fast-paced world of software development, Let's dive into how Claude Code can help you level up your workflow!"

**Fixed (Fuzzy's voice):**
> "Claude Code has been an absolute game changer for how I write code, and I reckon it's worth sharing how I've set it up to handle the repetitive stuff."

---

### ❌ Violation: Corporate Language

**Wrong:**
> "By leveraging agentic workflows, we can facilitate more efficient development processes and implement solutions that deliver value to stakeholders."

**Fixed (Fuzzy's voice):**
> "Agentic workflows let me get more done with less stuffing around. The agent sorts out the boring bits while I focus on the interesting problems."

---

### ❌ Violation: Over-Hedging

**Wrong:**
> "I think perhaps it might be worth considering that you could potentially try using skills to maybe improve your workflow."

**Fixed (Fuzzy's voice):**
> "Skills will absolutely improve your workflow. Give them a crack and you'll see what I mean."

---

### ❌ Violation: Generic Blog Tone

**Wrong:**
> "This weekend I had a great time at the beach! The weather was perfect and we really enjoyed ourselves. Here are 5 tips for your next beach trip..."

**Fixed (Fuzzy's voice):**
> "Sunday we headed up the Sunshine Coast, eager for a day at the beach. After a bit of a splash in the ocean and some sun baking we escaped the incredible heat, buying fish and chips and watching the world go by."

---

### ❌ Violation: American English

**Wrong:**
> "I need to organize my favorite colors and realize how awesome this customization is."

**Fixed (Australian English):**
> "I need to organise my favourite colours and realise how bloody good this customisation is."

---

## OpenSpec Artifact Structure for Blog Posts

When the `/blogpost` skill runs `/opsx:ff`, it generates several artifacts. Here's what should be in each:

### Proposal Artifact

**Purpose:** High-level overview of the blog post concept

**Should include:**
- Topic summary (2-3 sentences)
- Key points to cover (bullet list)
- Target reader (who is this for?)
- Why this post matters (what value does it provide?)

**Example:**
```markdown
## Proposal: Blog Post Automation with Claude Code Skills

This post explains how I created a custom `/blogpost` skill to streamline blog writing. The key points:

- Why manual OpenSpec workflow was tedious
- How skills work in Claude Code
- Building a custom skill from scratch
- Integration with style guide for voice consistency

Target reader: Anyone using Claude Code who writes regularly and wants to automate repetitive workflows.

Why it matters: Shows a practical example of custom skill creation and demonstrates how to maintain writing voice consistency with AI assistance.
```

### Specs Artifact

**Purpose:** Detailed outline and technical specifications

**Should include:**
- Section-by-section outline with headers
- Technical details to cover in each section
- Code examples or commands to include
- Reference materials and links to cite
- Any specific style/tone notes for this post

**Example:**
```markdown
## Specs: Blog Post Automation

### Section 1: The Problem
- Manual workflow: plan mode → /opsx:new → /opsx:ff → /opsx:apply
- Easy to forget style guide reference
- Repetitive setup for each post

### Section 2: How Claude Code Skills Work
- Skill directory structure (.claude/skills/name/)
- SKILL.md format with YAML frontmatter
- Trigger phrases in description field
- Integration with other skills

### Section 3: Building the /blogpost Skill
- Created directory structure
- Wrote SKILL.md with workflow steps
- Set up style guide auto-loading
- Chained OpenSpec commands

### Section 4: Using the Skill
- Command syntax: /blogpost [topic]
- What happens behind the scenes
- Example invocations

### Code/command examples to include:
- mkdir -p .claude/skills/blogpost/references
- YAML frontmatter structure
- Skill invocation examples

### References:
- Claude Code skills documentation
- OpenSpec workflow docs
- Link to STYLE_GUIDE.md in the repo
```

### Design Artifact

**Purpose:** Structural and tonal decisions for this specific post

**Should include:**
- Post structure (opening hook, sections, closing)
- Tone decisions (how technical? how casual? how much self-deprecation?)
- Unique angle (what makes this post different from generic coverage?)
- Formatting choices (where to use lists vs prose)

**Example:**
```markdown
## Design: Blog Post Automation

### Opening Hook
Start with the pain point: "Writing blog posts through the full OpenSpec workflow is brilliant for quality, but bloody tedious when you're doing it regularly."

### Structure
1. Hook: The tedium of manual workflow
2. Discovery: How skills can automate it
3. Implementation: Building the custom skill
4. Result: One command to rule them all
5. Close: What's next (other skills to build)

### Tone
- Technical but accessible
- Self-aware about over-engineering (but doing it anyway)
- Genuine enthusiasm about automation
- Include some self-deprecation about "automating blog posts about automation"

### Unique Angle
This isn't a generic "how to use Claude Code" post. It's a specific example of:
- Identifying a personal pain point
- Building a custom solution
- Meta commentary on using AI to write about AI tools

### Formatting
- Prose for storytelling sections (the problem, the discovery)
- Lists for technical sections (skill structure, workflow steps)
- Code blocks for examples (directory commands, YAML)
```

### Tasks Artifact

**Purpose:** Concrete implementation checklist

**Should include:**
- Step-by-step writing tasks
- Each section broken into actionable items
- Review and verification steps

**Example:**
```markdown
## Tasks: Blog Post Automation

- [ ] Write opening hook about manual workflow tedium
- [ ] Draft Section 1: The Problem (explain current workflow)
- [ ] Draft Section 2: Skills Discovery (how I learned about them)
- [ ] Draft Section 3: Building the Skill (implementation details)
- [ ] Draft Section 4: Using the Skill (examples and demo)
- [ ] Add code examples (mkdir commands, YAML structure)
- [ ] Write closing section (what's next, other automation ideas)
- [ ] Review against STYLE_GUIDE.md:
  - [ ] Conversational Australian voice
  - [ ] No AI filler phrases
  - [ ] Self-deprecating humor present
  - [ ] Prose for narrative, lists for technical
- [ ] Verify frontmatter is correct
- [ ] Verify filename matches pattern
```

---

## Troubleshooting

### Skill doesn't trigger

**Symptoms:** You type `/blogpost` and nothing happens

**Solutions:**
1. Check that SKILL.md exists at `.claude/skills/blogpost/SKILL.md`
2. Verify the YAML frontmatter is valid (no syntax errors)
3. Restart Claude Code to reload skills
4. Try the full skill name: `/blogpost`

### Style guide not being followed

**Symptoms:** Post doesn't match Fuzzy's voice, uses AI filler, or American English

**Solutions:**
1. Check that STYLE_GUIDE.md is being read (should see it in the workflow)
2. Explicitly remind during `/opsx:new`: "Remember to reference STYLE_GUIDE.md for Fuzzy's conversational Australian voice"
3. Review and edit the post manually, then run `/opsx:continue` to regenerate

### OpenSpec commands fail

**Symptoms:** `/opsx:new`, `/opsx:ff`, or `/opsx:apply` errors out

**Solutions:**
1. Check that OpenSpec is properly initialized (`openspec status`)
2. Verify no other OpenSpec change is in progress
3. Check permissions in `.claude/settings.local.json`
4. Review error message and adjust accordingly

### Post frontmatter is wrong

**Symptoms:** Missing fields, wrong timezone, incorrect format

**Solutions:**
1. Verify the tasks artifact includes frontmatter checklist
2. Manually edit after creation using the Edit tool
3. Reference this workflow.md for correct frontmatter structure

---

## Advanced Usage Patterns

### Writing Multiple Posts

You can use `/blogpost` multiple times in a session, but each will create a separate OpenSpec change. After finishing one post:

1. Run `/opsx:archive` to archive the completed change
2. Run `/blogpost [new topic]` to start the next one

### Iterating on a Post

If you want to revise or extend a post after creation:

1. Run `/opsx:continue` to add more tasks
2. Run `/opsx:apply` to implement the changes
3. Archive when satisfied

### Reusing Content

If you have reference material, code snippets, or quotes to include:

1. Provide them with the initial `/blogpost` command
2. Example: `/blogpost how I set up my shell environment [paste config here]`
3. The agent will incorporate them into the specs and use them in the post

---

## Integration with Existing Workflow

The `/blogpost` skill doesn't replace the OpenSpec workflow—it wraps it. You can still:

- Use `/opsx:new` manually for full control
- Use `/opsx:explore` to think through ideas first
- Use `/opsx:continue` to add more artifacts mid-workflow
- Use `/opsx:verify` before archiving to check everything

The skill is a convenience layer for the common case: "I have a topic, write a blog post about it in my voice."

---

## Example Full Workflow

Here's what happens when you run `/blogpost`:

```
User: /blogpost using agentic workflows to ship features faster

Agent:
1. [Reads STYLE_GUIDE.md]
2. [Invokes /opsx:new with topic and style context]
   → Creates proposal artifact
3. [Invokes /opsx:ff]
   → Creates specs artifact
   → Creates design artifact
   → Creates tasks artifact
4. [Invokes /opsx:apply]
   → Writes _posts/2026-02-15-agentic-workflows-ship-faster.md
   → Frontmatter with correct timezone
   → Content in Fuzzy's voice
5. [Verifies post exists and matches requirements]
6. [Reports completion to user]

User sees:
"✅ Blog post created: _posts/2026-02-15-agentic-workflows-ship-faster.md

The post covers your experience with agentic workflows, how they've changed your development speed, and practical examples from your workflow. Written in your conversational Australian voice with the usual self-deprecating humor about probably over-engineering things.

Ready to review and publish!"
```

---

## Maintenance Notes

### When to Update This Skill

Update the `/blogpost` skill when:

- STYLE_GUIDE.md changes significantly
- OpenSpec workflow changes (new commands, different artifact structure)
- Blog post frontmatter requirements change
- You identify common issues or missing features

### Where to Update

- SKILL.md: Core workflow and logic
- workflow.md (this file): Examples, troubleshooting, patterns
- Add new reference files as needed for specific use cases

### Testing After Updates

After updating the skill, test:

1. Basic invocation: `/blogpost test topic`
2. No-topic invocation: `/blogpost`
3. Verify style guide is read
4. Verify OpenSpec commands execute
5. Verify post format is correct
6. Verify voice matches style guide
