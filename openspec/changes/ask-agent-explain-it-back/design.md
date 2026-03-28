## Context

The blog publishes posts about agentic coding workflows, tools, and practical techniques. Posts are written in Fuzzwah's conversational Australian voice (defined in STYLE_GUIDE.md), live in `_posts/` as dated markdown files with YAML frontmatter, and are built by Jekyll.

This post covers a specific technique: using fresh AI agent sessions to audit the CLAUDE.md context files that bootstrap all agent conversations. The author runs multiple agents in parallel across repos, so context quality has an outsized impact on consistency and correctness.

## Goals / Non-Goals

**Goals:**
- Write a blog post that clearly explains the "ask the agent to explain it back" technique
- Make the technique immediately actionable — readers should be able to try it after reading
- Match Fuzzwah's voice: conversational, Australian, enthusiastic, self-deprecating, direct
- Structure the post for both narrative flow and practical reference (specific prompts to try)

**Non-Goals:**
- Not a tutorial on setting up CLAUDE.md from scratch — assumes the reader already uses context files
- Not a deep dive into Claude Code features or configuration
- Not a comparison of different AI coding tools
- Not a formal methodology or framework — this is a lightweight habit

## Decisions

**Post structure: narrative arc with embedded practical sections**
The post follows a natural flow: setup → technique → what it finds → why it works → the feedback loop → specific prompts → meta-insight. This mirrors how you'd explain it to someone at the pub. The "specific prompts" section uses a list format since those are reference items readers will come back to. Rationale: pure narrative would bury the actionable bits; pure listicle would lose the conversational voice.

**Angle: the agent as test reader, not the agent as editor**
The key framing is that a fresh agent is like a new hire reading your docs for the first time — it reveals what's missing, stale, or ambiguous. The editing that follows is a natural consequence, not the main point. Rationale: "use AI to edit your docs" is generic advice; "use AI's fresh-eyes perspective to audit your docs" is a specific, novel technique.

**Tone: practical first-person, aimed at practitioners**
Not beginner-friendly explainer, not thought-leadership thinkpiece. Written for devs who are already running AI agents and want to squeeze more consistency out of them. Rationale: matches the blog's audience and the author's voice.

**Real examples over hypotheticals**
Use the fish/bash inconsistency as the anchor example — it's specific, relatable, and demonstrates how subtle these issues can be. Rationale: concrete examples land better than abstract descriptions.

## Risks / Trade-offs

- **Too niche**: Readers who don't use CLAUDE.md or similar context files may not connect. Mitigation: frame the technique generically enough that it applies to any agent context mechanism.
- **Reads as obvious**: "Read your own docs" isn't groundbreaking. Mitigation: emphasise that the agent brings a genuinely different perspective — zero prior context, literal interpretation, no gap-filling. That's the insight.
- **Too long**: The topic has natural depth but the post should stay punchy. Mitigation: keep sections tight, let the prose do the work without over-explaining.
