## 1. Setup

- [x] 1.1 Create OpenSpec change artifacts (`proposal.md`, `design.md`, `specs/`, `tasks.md`) under `openspec/changes/orca-omp-and-the-whole-lifecycle/`
- [x] 1.2 Create blog post file `_posts/2026-09-07-orca-omp-and-the-whole-lifecycle.md` with valid YAML frontmatter (title, date with +1000 AEST timezone, author, description)

## 2. Draft Post Content

- [x] 2.1 Write opening hook — callback to the split-brain post's "One machine. One brain." ending, framing it as a stopping point
- [x] 2.2 Write the rug-pull section — conduit wrapped agent CLIs and relied on `claude -p`; Anthropic removed it from the subscription; the single-vendor-dependence lesson
- [x] 2.3 Write the Orca section — agent-agnostic ADE, engines side by side in isolated git worktrees, tracked in one place, own subscriptions
- [x] 2.4 Write the headless box section — Orca as a remote server on the Linux box, MacBooks pairing over Tailscale, phone companion, the split-brain question answered harder
- [x] 2.5 Write the engine section — omp (oh-my-pi) and the two-tier model economics (fast cheap default, strong model on the review gate), as evolution of the Jan stack post
- [x] 2.6 Write the lifecycle section — OpenSpec explore → propose → apply → archive
- [x] 2.7 Write the review gate section — strong-model reviewer, what it checks, planning artifacts only
- [x] 2.8 Write the board and issues section — issue per change, body mirrors tasks.md, card on the Backlog board, Status lifecycle, tick both or the board lies
- [x] 2.9 Write the end-to-end lifecycle list and the meta point — this post is itself an OpenSpec change
- [x] 2.10 Write the closing — pipeline as the system, reports rollout, looking forward

## 3. Review

- [x] 3.1 Review full post against STYLE_GUIDE.md — check Australian English spelling, conversational voice, no corporate language, no AI filler, no emojis
- [x] 3.2 Verify factual claims against the environment — Orca/omp descriptions, review gate behaviour, board mechanics, links
- [x] 3.3 Verify frontmatter is complete and correctly formatted
- [x] 3.4 Read the post end-to-end for flow, pacing, and tone consistency
- [x] 3.5 Build the site locally with Jekyll and confirm the post renders and links resolve

## 4. Update Notices

- [x] 4.1 Add `update_notice` frontmatter to `_posts/2026-04-02-split-brain-coding-agent.md` pointing to the new post
- [x] 4.2 Update `update_notice` frontmatter on `_posts/2026-03-28-conductor-orchestrating-ai-agents-across-repos.md` to point to the new post

## 5. Publish

- [x] 5.1 Create feature branch, commit the post and artifacts, push
- [x] 5.2 Open a pull request against `main`
