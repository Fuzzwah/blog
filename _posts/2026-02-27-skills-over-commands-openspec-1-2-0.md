---
title: "Skills Over Commands: What OpenSpec 1.2.0 Taught Me About Claude Code"
date: 2026-02-27 10:00:00 +1100
author: Fuzzwah
description: Ran openspec update --force on one of my projects, watched it cleanly handle the migration, and got to thinking about what commands vs skills in Claude Code actually means.
---

I ran `openspec update --force` on one of my projects last week and stood there for a moment watching the output tick by. Removed `/opsx:new`. Removed `/opsx:ff`. Removed `/opsx:explore`. Removed `/opsx:apply`. Removed `/opsx:archive`. Plus five deprecated ones that had been hanging around looking sorry for themselves. Then: created `openspec-propose`. Created `openspec-apply-change`. Created `openspec-explore`. Created `openspec-archive-change`.

Seven files out. Four files in. About three seconds.

That's how tooling upgrades should work. And standing there thinking about *why* it worked that cleanly sent me down a bit of a rabbit hole about how Claude Code actually organises the instructions you give it — which, it turns out, is worth understanding if you're building serious projects with it.

## What Changed

OpenSpec 1.2.0 moved its core workflows from Claude Code commands to skills. The old system had a bunch of command files in `.claude/commands/opsx/`: `/opsx:new` for creating a change, `/opsx:ff` (fast-forward) for generating all the artifacts in one go, plus `/opsx:explore`, `/opsx:apply`, and `/opsx:archive` for the rest of the lifecycle. Alongside those were a handful of deprecated commands — `/opsx:continue`, `/opsx:sync`, `/opsx:bulk-archive`, `/opsx:verify`, `/opsx:onboard` — that had accumulated over time and were just taking up space.

After the update: four skills (`openspec-propose`, `openspec-apply-change`, `openspec-explore`, `openspec-archive-change`) with thin command wrappers remaining for the ones you actually invoke. The `/opsx:new` + `/opsx:ff` two-step collapsed into a single `/opsx:propose`. Cleaner, less surface area, better integrated.

The `--force` flag told the update tool to overwrite the existing files rather than bail out on finding they already existed. For this kind of migration that was the right call — I hadn't made custom modifications to those command files, so there was nothing to preserve.

## Commands vs Skills: What the Distinction Actually Means

Here's the bit worth understanding, because it affects how you think about your whole `.claude/` setup.

In Claude Code, a **command** is essentially a one-shot prompt injection. When you invoke `/opsx:apply`, the agent reads that command file, follows its instructions for the current turn, and that's it. Each invocation is fresh. The command exists to tell the agent what to do right now.

A **skill**, on the other hand, is persistent context. Skill files live in `.claude/skills/` and they're loaded as part of the agent's operating context for the session. The agent doesn't just read a skill when you invoke it — it carries that context across the whole conversation. A skill gives the agent durable understanding of *how* to work, not just instructions for a single task.

The practical difference: with `/opsx:apply` as a command, the agent read the OpenSpec apply workflow instructions at the moment I invoked it and acted on them for that turn. With `openspec-apply-change` as a skill, the agent has the full OpenSpec methodology loaded as context throughout the session. It knows how OpenSpec works, what the artifacts mean, how to interpret the tasks file — not because I just told it, but because that understanding is baked into how it's operating.

It's a bit like the difference between reading a recipe once before you start cooking versus having actually spent time in a kitchen. One gives you instructions to follow. The other builds something that carries across everything you do.

## The Manual Part: .claude/ Is Infrastructure

`openspec update --force` handled all the file operations. But it couldn't do everything. The remaining work was mine: updating `CLAUDE.md` and the project's memory files to replace stale references.

The old workflow instructions in `CLAUDE.md` referred to `/opsx:new` and `/opsx:ff` as separate steps. After 1.2.0 those are replaced by `/opsx:propose`. Same story in the memory files — anywhere I'd noted the workflow for the agent's benefit, those references needed updating. Not complicated, but it required reading the context and making a judgement call about what each reference meant and whether the new workflow covered the same ground.

This is exactly the right division. The tool is good at mechanical file operations — it knew precisely which files to remove and create, and it did that without any fuss. The human is good at understanding what notes actually mean in context and updating them accurately. If the tool had tried to rewrite CLAUDE.md automatically, it'd need to understand prose, interpret intent, and make editorial decisions. That's not its job.

The deeper point: your `.claude/` directory is project infrastructure. Not a config folder you wire up once and forget. It's the thing that tells your agent — and future you, and anyone else working in the codebase — how this project operates. If you wouldn't leave dead references in your source code, don't leave them in your agent instructions. Stale commands in CLAUDE.md are a bug. They send your agent off doing things that don't work the way you expect, or don't work at all.

Review it like you review code. Keep it current alongside your dependencies. Version it. It's a first-class part of the project.

## Why the Spec-Driven Overhead Is Worth It

Right, while I'm here — since this is fundamentally a post about OpenSpec.

I know what you're thinking about the proposal → design → specs → tasks cycle, because I used to think it too. It's heavy. If I want to add a field to a form, should I really be writing a design document? The honest answer is: sometimes no, sometimes yes, and developing the judgment for which is which is part of the discipline.

For small changes in a well-understood system you can probably skip the full cycle. But for anything non-trivial — a new feature, a change that touches multiple systems, anything involving architecture decisions — the overhead is load-bearing.

Here's what it actually does: it stops your agent from drifting. Without a spec, an AI coding agent works from its best interpretation of what you want based on context it's built up during the session. Sessions end. Context resets. The agent starts fresh and rebuilds its interpretation from scratch. With a spec, the intent is captured in writing. The agent reads the tasks file, sees the design, and knows what "done" looks like without you re-explaining everything.

The spec is also a design review you can't skip. I've caught myself, mid-proposal, realising I'm about to build something with a flaw I hadn't spotted in the planning conversation. Writing it down forces a kind of rigour that talking through it doesn't. The artifact isn't bureaucracy — it's the agent thinking carefully about your problem before touching anything, and that thinking preserved somewhere you can come back to.

Three weeks later when something breaks, the archived change tells you why it was built the way it was. That's worth more than it sounds.

## openspec update as a Benchmark

I want to loop back to `openspec update --force` for a moment, because I think it's worth calling out as a concrete example of good AI tooling design.

The tool had a clear job: migrate from the old command structure to the new skills structure. It knew which files to remove and which to create. It executed that job cleanly, left me a summary of what it had done, and got out of the way. No manual file copying, no hunting through docs for the new directory structure, no figuring out the diff myself.

But it didn't try to be clever about the parts that required judgment. It didn't attempt to update `CLAUDE.md`. It didn't rewrite memory files. Those are editorial decisions that require understanding context, and it correctly left those to me.

That's the model: automate the mechanical work completely, hand off the judgment calls explicitly. A tool that tries to do too much gets the judgment calls wrong and creates a mess you have to clean up anyway. A tool that does too little makes you do work that doesn't need human attention. Getting that boundary right is the difference between a tool you trust and one you're always second-guessing.

## Your .claude/ Directory Is the New Makefile

The broader principle I keep coming back to: `.claude/` has gradually become one of the most important parts of my projects. Not in terms of code — there's no logic in there — but in terms of what it tells collaborators (human and AI) about how the project works.

`CLAUDE.md` is the orientation document. Skill files are the institutional knowledge. Memory files are the running notes. Together they encode how this project operates, what workflows it uses, and what an agent coming in fresh needs to know to be useful rather than running off in the wrong direction.

That's what a `Makefile` used to do. It was the entry point. You'd look at the Makefile to understand what the project knew how to do. `.claude/` is doing that job now, except the consumer is an AI agent and the "commands" are richer context rather than shell recipes.

The skills system in OpenSpec 1.2.0 is a small change, technically. Seven files out, four in, a bit of manual doc cleanup. But it's a concrete example of this pattern playing out well — tooling that understands how to slot into your `.claude/` setup, that migrates cleanly when it needs to change, and that makes your agent smarter in ways that persist across sessions rather than evaporating when the conversation ends.

That's the direction I want all my tooling to move in. Felt worth saying.
