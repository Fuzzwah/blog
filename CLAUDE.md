# Fuzzwah's Blog

Jekyll blog hosted at blog.fuzzwah.com. Posts about agentic coding, tools, and workflows.

## Agent Behavior

- **Always start in plan mode.** When a fresh session begins with a task or prompt, enter plan mode first. Explore the codebase, understand what's needed, and present a plan before writing any code or content.
- **After a plan is approved, start an OpenSpec change.** Once the user approves your plan, immediately run `/opsx:propose` to create a tracked change with all artifacts (proposal, specs, design, tasks) generated in one step. Then use `/opsx:apply` to implement.

## Project Structure

- `_posts/` — Blog posts in markdown with YAML frontmatter
- `_layouts/`, `_includes/`, `_sass/` — Jekyll templating and styles
- `STYLE_GUIDE.md` — Writing voice and tone guide (read this before writing any content)
- `openspec/` — OpenSpec change tracking

## Writing Blog Posts

Every blog post goes through OpenSpec. The workflow:

1. **`/opsx:propose`** — Provide a brief topic overview and any reference documents; generates all artifacts (proposal, specs, design, tasks) in one step
2. **`/opsx:apply`** — Write the actual post

### Voice and Style

Read `STYLE_GUIDE.md` before writing any content. Key points:

- Conversational, warm, Australian voice — like telling a mate at the pub
- Australian English (organise, realise, favourite, colour, arse)
- No emojis, no corporate language, no AI filler phrases
- Self-deprecating humour, genuine enthusiasm for the tech
- Open with a hook, close looking forward

### Post Format

Posts live in `_posts/` as `YYYY-MM-DD-slug-title.md` with frontmatter:

```yaml
---
title: Post Title Here
date: YYYY-MM-DD HH:MM:SS +1100
author: Fuzzwah
description: One casual sentence describing the post.
---
```

- Timezone is `Australia/Sydney` (+1100 AEDT or +1000 AEST)
- Description should be casual and concise, not SEO keyword stuffing
- Use prose for narrative sections, lists for workflows/steps/tool comparisons
- Keep code blocks exactly as provided in reference material

### OpenSpec Context for Blog Posts

When generating OpenSpec artifacts for a blog post:

- **Proposal**: Topic overview, key points to cover, why this post matters, target reader
- **Specs**: Detailed outline with section headings, key technical details to include, reference materials and links to cite
- **Design**: Post structure, tone decisions for this specific topic, what angle makes this post unique vs generic coverage
- **Tasks**: Concrete writing steps — draft each section, add code examples, write intro hook, write closing, review against style guide
