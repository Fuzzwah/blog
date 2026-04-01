---
title: The Split-Brain Coding Agent Problem
date: 2026-04-02 12:00:00 +1100
author: Fuzzwah
description: How I stopped my coding agent from losing its mind every time I switched MacBooks.
---

For a while there, my Claude Code setup was doing my head in.

I've got two MacBooks — one at my desk, one on the couch — and I was bouncing between them depending on where I felt like working. Fine in theory. In practice, it meant agent sessions were scattered across both machines in various states of completion. Half-finished features on one, a separate chain of thought on the other. Merging the work back together was tedious, and that's the optimistic framing.

The deeper problem was the harness state. Claude Code writes memory files to the local filesystem, and those files never get committed to repos. So each machine ended up with its own idea of what was going on — different memories, different context, different accumulated session history. The agent would behave differently depending on which MacBook I was sitting at. Same codebase, same me, different brain. Frustrating doesn't quite cover it.

I had a look at [Conductor](https://github.com/conduit-cli/conductor) first, since I'd already written about it and knew the general vibe. The pitch is solid — structured workspace management for AI agents. But it really wants a monorepo, and my setup is a "one workspace repo wrapping sub-repos for each service" kind of deal: website, Discord bot, data processing, docs. I couldn't be bothered restructuring everything to fit that model. And honestly, even if I had, it still wouldn't have fixed the core problem. The sessions would still be split across two machines.

What actually sorted it was stumbling onto [conduit](https://github.com/conduit-cli/conduit) — a TUI and web UI for managing Claude Code sessions, backed by a SQLite database. I set it up on my ThinkCentre P330, a little box that lives on my desk and runs 24/7. Now when I want to code, I either SSH in from whichever MacBook I'm on and pick up a session right where I left off, or I open the web interface and do the same thing. One machine, one harness, one memory state. The agent has a single, consistent brain regardless of where I'm sitting. Pretty rad.

It wasn't perfect straight out of the box, though. A few things annoyed me enough that I forked it — described the issues to Claude and had it fix them. The fork lives at [github.com/Fuzzwah/conduit](https://github.com/Fuzzwah/conduit) if you want to have a look. The main things I added:

- **Multiline paste support in the TUI** — the upstream version would mangle anything with newlines
- **Copy code block to clipboard with `y` in scrolling mode** — auto-dedents the content and pipes it through OSC 52, so it actually lands on my Mac's clipboard even over SSH
- **Theme-aware syntax highlighting** — small thing, but it matters when you're staring at it all day

The OSC 52 one is the bit I'm most pleased with. Getting clipboard passthrough working over SSH is one of those things that sounds like it should be trivial and absolutely isn't. Having it just work when I hit `y` over a code block is genuinely nice.

The honest tradeoff is that I'm now dependent on the ThinkCentre being up. If that box goes down, my sessions go with it. In practice, that's not a real concern — it's a dedicated homelab machine that doesn't get shut down — but it's worth naming. You're trading local autonomy for centralised consistency. For my setup, that's a trade I'm very happy with.

One machine. One brain. No more split-personality agent.
