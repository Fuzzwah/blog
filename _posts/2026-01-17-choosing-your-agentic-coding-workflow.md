---
title: "Choosing Your Agentic Coding Workflow: VS Code vs Claude Code CLI"
date: 2026-01-17 10:00:00 +1100
author: Fuzzwah
description: When to use Copilot Chat, the Claude Code extension, or the CLI — and why I bounce between all three.
update_notice:
  message: "This covers single-repo workflows. I've since moved to running parallel agents across multiple repos with Conductor."
  url: /2026/03/28/conductor-orchestrating-ai-agents-across-repos/
  link_text: "Read about the multi-repo approach"
---

I reckon one of the most common questions I get asked about agentic coding is "which tool should I use?" And honestly, the answer that took me too long to figure out is: all of them, depending on what you're doing. After months of experimenting I've got some pretty strong opinions about when each tool shines, so let me walk you through it.

## The Contenders

There are three main approaches I bounce between:

- **VS Code + Copilot Chat** — GitHub's native agentic coding experience, deeply integrated into the editor with support for `copilot-instructions.md` files, workspace context awareness, and the ability to watch the agent work in real-time.
- **VS Code + Claude Code Extension** — Brings Claude's capabilities directly into VS Code, combining Anthropic's powerful models with the familiar editor environment.
- **CLI-based Claude Code** — A terminal-based workflow using Claude Code from the command line. Different paradigm entirely, and bloody brilliant for certain tasks.

## For Existing Codebases: VS Code Wins

When you're working with established projects — codebases you know well, with existing patterns and conventions — the VS Code-based approaches (particularly Copilot Chat) have a massive advantage: **visibility and control**.

You can watch the agent work. See which files it's reading, what changes it's considering, and jump in before it heads down the wrong path. This is invaluable when you've got domain knowledge the agent lacks. You can course-correct immediately with things like "No, use the existing `UserService` instead of creating a new one" or "Follow the pattern in `handlers/auth.ts`" and it just works beautifully in that context.

The real superpower here is `copilot-instructions.md`. You can document your codebase's conventions, architectural decisions, and common patterns in `.github/copilot-instructions.md` files. The agent reads these and follows your guidelines, which means more consistent, on-brand code changes. I'll bang on about this more in a sec, because it's genuinely that good.

The other thing worth mentioning is that for codebases you understand deeply, you're not relying on the agent to figure out architecture — you're using it to accelerate implementation of changes you've already mentally designed. The stakes are lower, and the agent's contextual awareness of your workspace structure, open files, and existing code makes the whole thing feel natural.

## For New Projects: Claude Code Shines

When starting from scratch or exploring unfamiliar territory, I consistently reach for **Claude Code** (whether CLI or extension). New projects benefit enormously from thoughtful planning before implementation, and Claude Code's plan mode is specifically designed for this:

1. **Use Opus for Planning** — Start with Claude Opus in plan mode. Give it your requirements and let it think through the architecture, file structure, dependencies, and implementation approach.
2. **Human Review Phase** — This is critical. Review the plan carefully. Ask questions. Request alternatives. Refine the approach. The plan is cheap to modify — implementation is expensive.
3. **Switch to Sonnet for Implementation** — Once you're satisfied with the plan, switch to Claude Sonnet to execute it. Sonnet is faster and more cost-effective while still being highly capable.

This works so well because Opus excels at the high-level architectural reasoning that prevents costly mistakes early on. And without existing code to navigate, you're not really benefiting from VS Code's contextual awareness anyway — Claude Code can build the structure from first principles. Creating multiple files, setting up project structure, configuring build tools — these tasks work better in a more autonomous mode where the agent can work through a plan systematically. The plan mode forces you to think through your design before committing to code, and that front-loaded thinking time pays dividends later.

## My Recommended Workflow

Here's what I actually do in practice:

**Starting a New Project:**
1. Use Claude Code CLI with Opus
2. Generate comprehensive plan
3. Review and refine plan (multiple iterations if needed)
4. Switch to Sonnet
5. Execute plan
6. Review generated code carefully
7. Transition to VS Code for ongoing development

**Working on Existing Projects:**
1. Use VS Code + Copilot Chat
2. Maintain `copilot-instructions.md` files
3. Guide the agent using workspace context
4. Intervene and course-correct as needed
5. Keep changes focused and reviewable

**Large Refactors or Feature Additions:**
1. If the change is well-defined: VS Code + Copilot Chat
2. If the approach needs design work: Claude Code (Opus) for planning
3. Review plan in VS Code
4. Execute in whichever environment feels right

## What I've Learnt

A few things have become clear after living with all three tools:

1. **Always review plans heavily** — whether using Opus or another model, never skip the plan review phase for significant work. Plans are cheap to modify; implemented code is expensive to refactor. Ask questions, request alternatives, and make sure you understand the approach before implementation begins.
2. **Opus for architecture, Sonnet for implementation** — this cost-performance sweet spot works consistently well. Opus thinks through the hard problems, Sonnet executes the plan efficiently.
3. **`copilot-instructions.md` is a superpower** — for codebases you work on regularly, investing time in comprehensive instruction files pays enormous dividends. Document your architectural patterns, naming conventions, testing approaches, common gotchas, and file organisation principles. Seriously, do this.
4. **Context switching is okay** — you're not married to one tool. Use Claude Code to scaffold a new service, then switch to VS Code + Copilot Chat for ongoing development. Use whatever fits the task.

## The Future is Multi-Tool

The most productive agentic coding workflow isn't about picking one tool and sticking with it dogmatically. It's about understanding the strengths of each approach and reaching for the right one:

- **New project planning**: Claude Code with Opus
- **Plan execution**: Claude Code with Sonnet
- **Existing codebase work**: VS Code with Copilot Chat
- **Guided refactoring**: VS Code with instructions files
- **Exploratory changes**: Claude Code for thinking space

We're still early in the agentic coding revolution. These tools are evolving rapidly, and the best practices are still being discovered. But I reckon having multiple tools in your arsenal and knowing when to reach for each one is the path to getting the most out of all this. Stay tuned — I've got more to share as my workflow keeps evolving.
