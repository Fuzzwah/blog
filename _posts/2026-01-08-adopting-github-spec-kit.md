---
title: Adopting GitHub Spec-Kit
date: 2026-01-08
---

Three weeks ago, I found myself staring at three different "Product Requirements Documents" that were supposed to guide my development work. Each was written in a different style, with different levels of detail, and - most frustratingly - each left critical questions unanswered.

The first document was essentially a wall of implementation details. It told me *how* to build the feature (database schemas, caching strategies, partitioning approaches) but buried the *why* so deep that I had three different interpretations of what problem I was actually solving.

The second was the opposite problem - eight pages of ambitious vision with phrases like "engaging experience" and "viral growth potential" but zero clarity on what constituted a minimum viable product versus nice-to-have features.

The third fell somewhere in between: decent problem statement, but when I asked "how will we know when this is done?" I got crickets.

Sound familiar?

## The Discovery

I'd been aware of [GitHub's Spec Kit](https://github.com/github/spec-kit) for a while - bookmarked it months ago, probably skimmed the README once. But between actually building features and fighting fires, I never gave it serious attention.

Looking at those three conflicting PRDs, I finally decided to invest the time to understand what Spec Kit was actually offering. Not just another template to ignore, but a framework for thinking about requirements differently.

Three things immediately clicked:

1. **Clear separation of concerns**: Specs describe *what* I'm building (technology-agnostic requirements), while implementation plans handle *how* (technology-specific solutions)
2. **Forced prioritization**: The P1/P2/P3 system for user stories makes you confront what's actually MVP versus nice-to-have
3. **Measurable success**: Every spec requires concrete success criteria - no more "it works" handwaves

The `/specify` AI command that generates specs from conversation was intriguing, but I decided to start by converting my existing PRDs manually. This would force me to understand the framework deeply.

## The Migration: A Step-by-Step Walkthrough

### Step 1: Create the Constitution

Before writing any specs, Spec Kit requires a "constitution" - a document that captures your project's core development principles. This felt like busywork at first, but it forced me to articulate things I'd been doing implicitly:

- How I manage my distributed architecture
- My database access patterns and query optimization principles
- Non-interactive automation requirements (scripts must run unattended)
- Pre-commit validation standards
- Environment management discipline

Writing this down created a reference point for future decisions and helped clarify my own thinking about the principles guiding the project.

### Step 2: Convert the Implementation-Heavy PRD

I tackled my messiest PRD first - the one that was all implementation details with buried requirements.

**The Process**:
1. Created the spec directory following Spec Kit conventions
2. Copied the Spec Kit template
3. Started with the user stories section

**The Revelation**: Extracting user stories forced me to ask "who benefits from this and how?" I discovered I'd been conflating three distinct user personas with different needs that my original PRD had mushed together.

**The Requirements Extraction**: I went through each implementation detail in the PRD and asked "what requirement does this satisfy?" This revealed gaps - I had detailed schemas but hadn't specified critical constraints like data retention periods or performance targets.

I ended up with:
- 4 user stories (prioritized P1-P2)
- 34 functional requirements organized into logical tiers
- 17 measurable success criteria (with specific numeric targets)
- 8 edge cases we hadn't considered

**Immediate win**: When I returned to implementation later, I could reference specific success criteria instead of trying to remember what I'd meant.

### Step 3: Convert the Vision-Heavy PRD

The second PRD was all vision, zero specifics. Converting it revealed why projects like this become scope creep nightmares.

**The Challenge**: Eight pages of "wouldn't it be cool if..." features with no clear priority.

**The Spec Kit Solution**: The P1/P2/P3 system forced brutal honesty:
- P1: Core functionality that must ship
- P2: Important but not blocking MVP  
- P3: Nice-to-have for future iterations

I cut two-thirds of the original "requirements" to P3. This hurt at first - I really wanted those features! - but it clarified what I could actually deliver in a reasonable timeframe.

**Measurable Success Criteria**: The original PRD had vague aspirational language. The spec required specifics with concrete numeric targets for conversion rates, retention metrics, and business outcomes.

Now I'd know if the feature succeeded, and could adjust tactics if metrics lagged.

**Immediate win**: Cut scope by 40% while maintaining the core value proposition

### Step 4: Convert the Ambiguous PRD

The third PRD had a good problem statement but vague completion criteria.

**The Original Question**: "How will we know when this is done?"

**The Spec Kit Answer**: I defined 16 specific success criteria including detection rates, reliability targets, and time-bound correction windows.

**Edge Cases**: The template prompted me to think through 12 scenarios I hadn't considered - API failures, edge conditions, data inconsistencies, and timing issues. Each edge case got a documented resolution approach.

**Immediate win**: I started implementation the next day with zero unanswered questions

## The Results

After converting three PRDs to specs over two weeks, here's what changed:

### Before Spec Kit
- **Scope creep**: Features ballooned as I discovered "oh I should also..."
- **Ambiguous completion**: "It works" was my definition of done
- **Priority confusion**: Everything felt equally important (and equally urgent)
- **Tech-first thinking**: I designed databases before understanding requirements

### After Spec Kit
- **Scope clarity**: P1/P2/P3 system creates clear understanding of MVP vs. future
- **Measurable success**: Concrete criteria with numeric targets
- **Independent testability**: Each user story can be validated separately
- **Requirements-first**: I understand *what* before debating *how*

### Concrete Improvements

**First Spec**: Identified that I was capturing far more data than needed. Now I know exactly what's essential versus noise, with clear tiering of criticality.

**Second Spec**: Ruthless prioritization cut scope by 40% while preserving core value. I now have a shippable MVP instead of an aspirational wishlist.

**Third Spec**: 16 measurable success criteria and 12 edge cases documented before writing a line of code. I started implementation with zero unanswered questions.

## Going Forward

My new workflow:

1. **Problem discussion** → rough notes on what I'm trying to solve
2. **Use `/specify` command** → AI generates initial spec from conversation
3. **Review and refine** → refine user stories, prioritize, define success criteria
4. **Quality validation** → run through Spec Kit checklist (no [NEEDS CLARIFICATION] markers allowed)
5. **Commit to feature branch** → spec lives alongside code in version control
6. **Use `/speckit.plan`** → generate implementation plan when ready to build
7. **Reference during development** → specs are living documents, not write-once artifacts

The key shift: **specs are now the source of truth**, not the code or the PRD or the chat conversation.

## Lessons Learned

### 1. Technology-Agnostic Requirements Are Liberating

My original PRDs specified exact technologies and implementation approaches. The specs say "handle large datasets efficiently" and "respond quickly." This lets me propose better solutions if they exist.

### 2. Measurable Success Criteria Eliminate Ambiguity

Changing "improve performance" to "reduce data volume by 90%" gave me a clear target. If I hit 85%, I know I need to do more work. If I hit 95%, I know I exceeded the requirement.

### 3. Prioritization Forces Hard Choices

The P1/P2/P3 system made me confront reality: I can't build everything immediately. This eliminated scope creep and made planning realistic.

### 4. Edge Cases Prevent Rework

Documenting edge cases upfront (like "what if external dependency is unavailable?") meant I designed resilient systems from the start instead of patching failures in production.

### 5. Manual Conversion Builds Understanding

I could have used `/specify` to generate specs from my PRDs, but manually converting the first three taught me *why* each section matters. Now when I use the AI tool, I know what to look for in the output.

## What's Next

I have more PRDs waiting to be converted. But more importantly, I'm writing new specs directly instead of starting with PRDs:

- Using `/specify` to generate initial specs from my planning sessions
- Refining them through the quality checklist
- Using `/speckit.plan` to generate implementation plans when ready to build

The framework gives me a structured way to think about what I'm building. When I consider a new feature, I ask:
- "What's the user story?"
- "How will I measure success?"  
- "Is this P1, P2, or P3?"

That's the real win - not just better documentation, but better thinking about what I'm building and why.

---

*Ready to try Spec Kit yourself? Start with your messiest PRD and convert it manually. The friction you feel during conversion reveals exactly where your requirements are unclear. That's the value.*
