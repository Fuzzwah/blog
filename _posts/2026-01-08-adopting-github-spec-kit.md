---
title: Adopting GitHub Spec-Kit
date: 2026-01-08 10:00:00 +1100
author: Fuzzwah
description: How converting my messy PRDs to GitHub Spec-Kit specs cleaned up my requirements game.
update_notice:
  message: "I've since switched from Spec-Kit to OpenSpec."
  url: /2026/02/06/switching-from-speckit-to-openspec/
  link_text: "Read about why I switched"
---

Three weeks ago I found myself staring at three different "Product Requirements Documents" that were supposed to guide my development work. Each one written in a different style, with different levels of detail, and — most frustratingly — each left critical questions unanswered.

The first was essentially a wall of implementation details. It told me *how* to build the feature (database schemas, caching strategies, partitioning approaches) but buried the *why* so deep I had three different interpretations of what problem I was actually solving. The second was the opposite — eight pages of ambitious vision with phrases like "engaging experience" and "viral growth potential" but zero clarity on what constituted MVP versus nice-to-have. The third fell somewhere in between: decent problem statement, but when I asked "how will we know when this is done?" I got crickets.

Sound familiar?

## The Discovery

I'd been aware of [GitHub's Spec Kit](https://github.com/github/spec-kit) for a while — bookmarked it months ago, probably skimmed the README once. But between actually building features and fighting fires, I never gave it serious attention. Looking at those three conflicting PRDs, I finally decided to invest the time to understand what Spec Kit was actually offering. Not just another template to ignore, but a framework for thinking about requirements differently.

Three things immediately clicked:

1. **Clear separation of concerns** — specs describe *what* I'm building (technology-agnostic requirements), while implementation plans handle *how* (technology-specific solutions)
2. **Forced prioritisation** — the P1/P2/P3 system for user stories makes you confront what's actually MVP versus nice-to-have
3. **Measurable success** — every spec requires concrete success criteria, no more "it works" handwaves

The `/specify` AI command that generates specs from conversation was intriguing, but I decided to start by converting my existing PRDs manually. I reckoned this would force me to understand the framework properly before letting the AI do the heavy lifting.

## The Migration

### The Constitution

Before writing any specs, Spec Kit requires a "constitution" — a document that captures your project's core development principles. This felt like busywork at first, but it forced me to articulate things I'd been doing implicitly:

- How I manage my distributed architecture
- Database access patterns and query optimisation principles
- Non-interactive automation requirements (scripts must run unattended)
- Pre-commit validation standards
- Environment management discipline

Writing this down created a reference point for future decisions and honestly helped clarify my own thinking about the principles guiding the project. Not bad for something I nearly skipped.

### Converting the Implementation-Heavy PRD

I tackled my messiest PRD first — the one that was all implementation details with buried requirements.

1. Created the spec directory following Spec Kit conventions
2. Copied the Spec Kit template
3. Started with the user stories section

Here's where it got interesting. Extracting user stories forced me to ask "who benefits from this and how?" and I discovered I'd been conflating three distinct user personas with different needs that my original PRD had mushed together. Going through each implementation detail and asking "what requirement does this satisfy?" revealed gaps I hadn't noticed — I had detailed schemas but hadn't specified critical constraints like data retention periods or performance targets.

I ended up with 4 user stories (prioritised P1-P2), 34 functional requirements organised into logical tiers, 17 measurable success criteria with specific numeric targets, and 8 edge cases I hadn't considered. The immediate win: when I returned to implementation later, I could reference specific success criteria instead of trying to remember what I'd meant. Bloody useful.

### Converting the Vision-Heavy PRD

The second PRD was all vision, zero specifics. Converting it revealed exactly why projects like this become scope creep nightmares.

Eight pages of "wouldn't it be cool if..." features with no clear priority. The P1/P2/P3 system forced brutal honesty — P1 for core functionality that must ship, P2 for important but not blocking MVP, P3 for nice-to-have future iterations. I cut two-thirds of the original "requirements" to P3. This hurt at first — I really wanted those features! — but it clarified what I could actually deliver in a reasonable timeframe. Cut scope by 40% while maintaining the core value proposition. That's a win I'll take.

The original PRD also had vague aspirational language where the spec required specifics — concrete numeric targets for conversion rates, retention metrics, and business outcomes. Now I'd actually know if the feature succeeded.

### Converting the Ambiguous PRD

The third PRD had a good problem statement but vague completion criteria. The classic "how will we know when this is done?" problem.

The Spec Kit answer: I defined 16 specific success criteria including detection rates, reliability targets, and time-bound correction windows. The template also prompted me to think through 12 scenarios I hadn't considered — API failures, edge conditions, data inconsistencies, and timing issues. Each edge case got a documented resolution approach.

I started implementation the next day with zero unanswered questions. That never happens.

## The Results

After converting three PRDs to specs over two weeks, the difference was night and day:

- **Scope creep replaced with scope clarity** — the P1/P2/P3 system creates a clear understanding of MVP vs. future work, instead of everything feeling equally important and equally urgent
- **"It works" replaced with measurable success** — concrete criteria with numeric targets instead of vague definitions of done
- **Tech-first thinking replaced with requirements-first** — I understand *what* before debating *how*, instead of designing databases before understanding requirements
- **Each user story independently testable** — no more tangled features that can't be validated separately

## Going Forward

My new workflow looks like this:

1. **Problem discussion** — rough notes on what I'm trying to solve
2. **Use `/specify` command** — AI generates initial spec from conversation
3. **Review and refine** — refine user stories, prioritise, define success criteria
4. **Quality validation** — run through the Spec Kit checklist (no `[NEEDS CLARIFICATION]` markers allowed)
5. **Commit to feature branch** — spec lives alongside code in version control
6. **Use `/speckit.plan`** — generate implementation plan when ready to build
7. **Reference during development** — specs are living documents, not write-once artefacts

The key shift: **specs are now the source of truth**, not the code or the PRD or the chat conversation.

## What I've Learnt

1. **Technology-agnostic requirements are liberating** — my original PRDs specified exact technologies and implementation approaches. The specs say "handle large datasets efficiently" and "respond quickly." This lets me propose better solutions if they exist.
2. **Measurable success criteria eliminate ambiguity** — changing "improve performance" to "reduce data volume by 90%" gave me a clear target. If I hit 85%, I know I need to do more work. If I hit 95%, I know I exceeded the requirement.
3. **Prioritisation forces hard choices** — the P1/P2/P3 system made me confront reality: I can't build everything immediately. This eliminated scope creep and made planning realistic.
4. **Edge cases prevent rework** — documenting edge cases upfront (like "what if external dependency is unavailable?") meant I designed resilient systems from the start instead of patching failures in production.
5. **Manual conversion builds understanding** — I could have used `/specify` to generate specs from my PRDs, but manually converting the first three taught me *why* each section matters. Now when I use the AI tool, I know what to look for in the output.

I've got more PRDs waiting to be converted, but more importantly I'm writing new specs directly instead of starting with PRDs. The framework gives me a structured way to think about what I'm building. When I consider a new feature, I ask "what's the user story?", "how will I measure success?", and "is this P1, P2, or P3?" I reckon that's the real win — not just better documentation, but better thinking about what I'm building and why. Stay tuned.
