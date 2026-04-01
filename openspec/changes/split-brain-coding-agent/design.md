## Context

The blog publishes posts about agentic coding workflows, tools, and practical techniques. Posts are written in Fuzzwah's conversational Australian voice (defined in STYLE_GUIDE.md), live in `_posts/` as dated markdown files with YAML frontmatter, and are built by Jekyll.

This post covers a specific, relatable infrastructure problem: using Claude Code across multiple machines causes session scatter and harness divergence. The solution is conduit — a TUI/web UI for managing Claude Code sessions backed by SQLite, running on a dedicated always-on homelab box.

## Goals / Non-Goals

**Goals:**
- Write a post that clearly explains the problem and why it's annoying
- Convey why Conductor wasn't the right fit without being dismissive of it
- Show how conduit solves the core problem — one machine, one memory state, one harness
- Mention the fork naturally — not as a brag, just as "I fixed a few things that bugged me"
- End with an honest take on the single-machine dependency tradeoff
- Match Fuzzwah's voice: conversational, direct, mildly opinionated, Australian

**Non-Goals:**
- Not a setup tutorial for conduit — no step-by-step instructions
- Not a comprehensive comparison of session management tools
- Not a deep dive into Claude Code internals
- Not a post about Conductor — mention it briefly, move on

## Decisions

**Post structure: problem → failed attempt → solution → improvements → honest tradeoff**
Natural narrative arc that mirrors how you'd tell this story to a mate. Start with the pain point, acknowledge what you tried that didn't fit, land on what worked, show you actually engaged with the tool by forking it, then be honest about the one gotcha. Rationale: this structure is satisfying to read and doesn't bury the lede.

**Angle: "one machine, one brain" as the core insight**
The key framing is that split state across machines creates a kind of split-brain problem — the agent has different memories and context depending on where you're sitting. The fix is boring infrastructure: centralise everything on one box. Rationale: this framing makes the post memorable and gives it a title that earns a click without being clickbait.

**Tone: wry and practical, not complaining**
The problem is genuinely annoying but the post shouldn't read as a grievance list. Describe the pain, show you solved it, move on. Self-deprecating where appropriate ("I couldn't be bothered restructuring everything"). Rationale: matches the style guide's "default to enthusiastic, acknowledge problems briefly then find the silver lining" rule.

**Fork section: brief, specific, matter-of-fact**
List the three improvements without over-explaining them. The OSC 52 clipboard trick is the most technically interesting — give it a sentence. Don't pad this section. Rationale: the post isn't about the fork, it's about the workflow problem and solution. The fork is evidence of genuine engagement, not the main event.

**Tradeoff close: honest, not apologetic**
End by acknowledging you're now dependent on one machine being up, then immediately diffuse it — it's a ThinkCentre that lives on my desk and runs 24/7, so in practice this isn't a problem. Don't over-engineer the hedge. Rationale: readers will notice the obvious tradeoff; address it directly and move on rather than pretending it doesn't exist.

**Links: inline, natural**
conduit-cli/conduit and github.com/Fuzzwah/conduit should appear in context, not in a trailing reference list. Conductor gets a mention but doesn't need a link unless it flows naturally. Rationale: style guide says to include links naturally in context.

## Risks / Trade-offs

- **Too short if the fork section is sparse**: Three bullet points don't fill much space. Mitigate by writing more prose around each one, or by letting the problem section breathe a bit more.
- **Reads as a product placement for conduit**: The post is genuinely about the problem and the solution happened to be conduit. Keep the framing problem-first. The fork section shows it's not an uncritical endorsement.
- **Conductor mention comes across as dismissive**: Be specific about why it didn't fit (monorepo assumption, multi-machine problem remains) rather than just "it wasn't for me". Rationale: specificity is more useful and reads as fair, not snarky.
