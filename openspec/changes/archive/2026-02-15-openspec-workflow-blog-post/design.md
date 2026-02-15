## Context

This is a blog post for blog.fuzzwah.com, not a software feature. The blog runs on Jekyll with posts in `_posts/` as markdown with YAML frontmatter. The post documents how Fuzzwah uses OpenSpec with Claude Code's VSCode extension as an end-to-end agentic workflow, and proposes a new idea: auto-generating support skill files on archive.

The target audience is developers using Claude Code who want to level up their agentic workflows. The post should resonate with people who've felt the friction of going from "I have an idea" to "it's built and I can support it."

## Goals / Non-Goals

**Goals:**
- Write a compelling blog post that explains the full OpenSpec + Claude Code workflow
- Walk through the lifecycle: plan mode → opsx:new → opsx:ff → opsx:apply → opsx:archive
- Introduce the new idea of archive-triggered support skill generation
- Follow STYLE_GUIDE.md voice — conversational Australian, enthusiastic, self-deprecating
- Make the reader want to try this workflow themselves

**Non-Goals:**
- Not a tutorial or step-by-step setup guide for OpenSpec
- Not a deep dive into OpenSpec internals or configuration
- Not implementing the support skill generation feature (just exploring the idea)
- Not covering every possible Claude Code feature

## Decisions

1. **Post angle: workflow narrative, not a tutorial.** The post tells the story of how the workflow evolved and why each piece matters, rather than being a dry "how to set up OpenSpec" guide. This keeps the energy up and matches Fuzzwah's voice. A tutorial would feel corporate.

2. **Structure: three acts.** Open with the current workflow (plan mode + CLAUDE.md), then the OpenSpec integration (artifacts as a thinking tool), then the new idea (support skills from archive). This builds naturally from established to speculative.

3. **The new idea gets honest treatment.** Present the support skill concept as genuinely exciting but acknowledge it's still an idea being explored. No overselling, no "this will change everything" — just genuine enthusiasm about the possibility.

4. **Code/config snippets are minimal.** Show a CLAUDE.md snippet and maybe a skill file example, but keep it tight. The post is about the workflow philosophy, not copy-paste setup instructions.

## Risks / Trade-offs

- [Post could feel too niche] → Frame it around the universal experience of managing complexity in AI-assisted coding, not just OpenSpec specifics
- [The support skill idea is unproven] → Be upfront that it's exploratory. Fuzzwah's voice handles speculation well with "I reckon" and "stay tuned"
- [Could come across as promotional for OpenSpec] → Keep the focus on the workflow and the thinking behind it, not on OpenSpec as a product
