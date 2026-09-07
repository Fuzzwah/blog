## Context

The blog publishes posts about agentic coding workflows, tools, and practical techniques. Posts are written in Fuzzwah's conversational Australian voice (defined in STYLE_GUIDE.md), live in `_posts/` as dated markdown files with YAML frontmatter, and are built by Jekyll.

The blog's arc to date: VS Code + Copilot (Jan) → OpenSpec adoption (Feb) → Conductor for parallel agents (Mar) → conduit on a homelab box to fix split-brain sessions (Apr). The last post ended "One machine. One brain." Since then the author rebuilt the environment on Orca + omp with an OpenSpec → review → issue → board pipeline, and this post is the status update that arc demands. It is the direct successor to the split-brain post and an evolution of the Conductor workflow post.

## Goals / Non-Goals

**Goals:**
- Explain what broke the conduit setup (dependence on `claude -p`; Anthropic removing it from the subscription) and the lesson about single-vendor dependence
- Introduce Orca accurately: agent-agnostic ADE running any coding agent side by side, each in its own git worktree, tracked in one place
- Describe the deployment: Orca running as a headless server on the Linux box, accessed entirely remotely over Tailscale from the MacBooks, with the phone as a steering companion
- Explain omp's place and the model-tier economics (fast cheap default model for routine work, strong model on tap for review)
- Walk the full change lifecycle: OpenSpec skills, the mandatory strong-model review gate, GitHub issue per change, Backlog board card with Status lifecycle, tasks ticked in both `tasks.md` and the issue body, archive closes the loop
- Land the meta point: this post itself is going through the same OpenSpec pipeline in the blog repo
- Add update notices to the superseded split-brain and Conductor posts
- Match Fuzzwah's voice: conversational, direct, mildly opinionated, Australian

**Non-Goals:**
- Not a setup tutorial for Orca or omp — no install steps
- Not a comprehensive tool comparison
- Not a deep dive into OpenSpec mechanics — the Feb posts covered that
- Not a post re-litigating what conduit was — the previous post did that; here it is one scene in the story

## Decisions

**Post structure: rug pull → rebuild → remote box → engine → lifecycle → review gate → board/issues → end-to-end shape → forward close**
The natural drama is the breakage: a setup the author wrote about enthusiastically was killed by a vendor decision. Open on that, land the rebuild, then spend the second half on the pipeline that makes the environment more than a pile of tools. Rationale: the breakage justifies the rebuild, and the pipeline is the genuinely new thing worth reading about.

**Angle: "don't bet on one agent, bet on the environment around agents"**
The through-line is vendor independence at the engine layer and discipline at the process layer. Orca makes engines interchangeable; OpenSpec + the board makes work auditable. Rationale: it connects every section and gives readers something to steal regardless of which tools they use.

**Callback continuity: use the split-brain post's "One machine. One brain." as the hook**
The new post opens by calling that line a stopping point, not a conclusion. Then the split-brain question is answered harder: the brain now lives in a headless body on a box, pointed at from anywhere. Rationale: rewards regular readers, keeps the blog's story coherent, and the update notices formalise the succession.

**Model economics: frame as the grown-up version of the January post's two-model approach**
The Jan stack post manually switched models (Claude for building, GPT for review). Now the pipeline does it automatically: fast model (deepseek v4 flash) for day-to-day work, strong model (codex/sol) bound to the review gate. Rationale: continuity that shows evolution, not just replacement.

**Review gate and board sections: concrete and specific, not abstract**
Name what the review agent actually checks (scope, verifiable/consistent specs, feasible design, actionable traceable tasks, coherence), what it may touch (planning artifacts only, never code), and exactly how the board works (card = linked issue, body mirrors tasks.md as checkboxes, Status lifecycle, tick both or the board lies). Rationale: this is the least-covered, most useful material; specifics are what make it useful.

**Links: inline, natural**
Link the previous posts (split-brain, Conductor, Jan stack post), stablyai/orca, onorca.dev, omp.sh / oh-my-pi, openspec-dev/openspec, and the Backlog board. In context, not as a trailing list. Rationale: style guide convention.

**Voice: wry about the rug pull, genuinely enthusiastic about the rebuild**
The `claude -p` removal gets a fair-but-cheeky treatment ("you don't have a workflow, you have a lease"). No grievance list. The pipeline sections carry real excitement — this is the stuff the author is chuffed about. Rationale: style guide's "acknowledge problems briefly, find the silver lining" rule.

## Risks / Trade-offs

- **Too long**: this post covers more ground than recent ones. Mitigate: keep each section tight, one idea per section, let the closing list do the summarising. Target ~1,100–1,300 words.
- **Tool-dump feel**: five tools and a process could read like a shopping list. Mitigate: every section is tied to the angle — the environment around agents — and to what changed versus previous posts.
- **Conduit shade**: the previous post was positive about conduit. Mitigate: blame the dependence, not the tool; it was great until the thing it wrapped moved.
- **Over-claiming on Orca/omp internals**: the author runs these tools but isn't their maintainer. Mitigate: describe behaviour the author actually uses (headless server over Tailscale, worktrees, mobile companion, engine choice), not internals he hasn't touched.
