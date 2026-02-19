## Context

The blog has published 8 posts about agentic coding tools and workflows (Claude Code, MCP servers, OpenSpec). StackTUI is a Python/Textual TUI dashboard for Docker Compose projects that was built using these tools. Open-sourcing it provides a natural blog post that shows a concrete outcome of the workflows discussed in prior posts.

The project README at https://github.com/Fuzzwah/stacktui provides comprehensive feature documentation and demo setup instructions.

## Goals / Non-Goals

**Goals:**
- Announce StackTUI as open source with genuine enthusiasm
- Give readers a clear picture of what it does and why it exists
- Provide a low-friction path to try it (demo environment)
- Connect naturally to the blog's Claude Code + OpenSpec themes
- Follow STYLE_GUIDE.md voice (conversational Australian, self-deprecating, direct)

**Non-Goals:**
- Not a full configuration tutorial or documentation
- Not a deep technical dive into Textual framework internals
- Not a pitch for OpenSpec — it's part of the story, not the focus
- Not a changelog or version announcement

## Decisions

**Narrative arc: "problem → built a thing → here it is → go try it"**
This keeps the post grounded in personal experience rather than reading like a product launch. The problem (too many terminal tabs managing Docker Compose) is relatable. The solution is the announcement.

**~1200 words, 7 sections + hook/close**
Matches the length and density of recent posts. Enough to cover features properly without dragging.

**Feature list as bullet points, not prose**
The style guide explicitly says lists for feature sets. Keep bullet copy casual, not documentation-speak.

**Demo environment as its own section**
The demo is the easiest way to try StackTUI without committing to wiring up your own project. Deserves prominent placement with a code block.

**"Built With Claude Code and OpenSpec" as a dedicated section**
This connects to the blog's themes but stays as a story beat, not a sales pitch. Acknowledges the meta angle: "the last few posts were about the workflow, this is the thing that came out of it."

**"What I've Learnt" section included**
This is an established pattern in recent posts. Keep the lessons specific to StackTUI (Textual framework, Docker API edge cases, git integration complexity).

## Risks / Trade-offs

**Risk: Post reads like a press release** → Mitigation: Lead with personal frustration, use self-deprecating humour about scope creep (12 themes), keep the "go have a play" energy throughout.

**Risk: Feature section becomes a wall of bullets** → Mitigation: Cap at 8-10 bullets, keep each to one line, group logically.

**Risk: OpenSpec section feels forced** → Mitigation: Frame as "how I built it" not "why you should use OpenSpec." One paragraph of genuine reflection, not advocacy.
