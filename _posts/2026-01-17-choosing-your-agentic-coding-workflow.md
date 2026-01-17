---
layout: post
title: "Choosing Your Agentic Coding Workflow: VS Code vs Claude Code CLI"
date: 2026-01-17 10:00:00 +1100
author: Fuzzwah
description: A practical comparison of agentic coding tools—VS Code with Copilot Chat, VS Code with Claude Code extension, and CLI-based Claude Code—and when to use each one.
---

# Choosing Your Agentic Coding Workflow: VS Code vs Claude Code CLI

After months of experimenting with different agentic coding approaches, I've developed strong opinions about when to use each tool in my workflow. The landscape has evolved rapidly, and we now have several compelling options: VS Code with GitHub Copilot Chat, VS Code with the Claude Code extension, and the CLI-based Claude Code. Each has its strengths, and choosing the right one depends heavily on your context.

## The Contenders

### VS Code + Copilot Chat
GitHub's native agentic coding experience, deeply integrated into the editor with support for `copilot-instructions.md` files, workspace context awareness, and the ability to observe and guide the agent's actions in real-time.

### VS Code + Claude Code Extension
Brings Claude's capabilities directly into VS Code, combining Anthropic's powerful models with the familiar editor environment.

### CLI-based Claude Code
A terminal-based workflow using Claude Code from the command line, offering a different paradigm for human-AI collaboration.

## For Existing, Well-Understood Codebases: VS Code Wins

When working with established projects—codebases you know well, with existing patterns and conventions—the VS Code-based approaches (particularly Copilot Chat) have a significant advantage: **visibility and control**.

### Why VS Code Excels Here

**Real-time Observation**: You can watch the agent work. See which files it's reading, what changes it's considering, and intervene before it goes down the wrong path. This is invaluable when you have domain knowledge the agent lacks.

**Guiding with copilot-instructions.md**: This is a game-changer. You can document your codebase's conventions, architectural decisions, and common patterns in `.github/copilot-instructions.md` files. The agent reads these and follows your guidelines, leading to more consistent, on-brand code changes.

**Contextual Awareness**: The agent has access to your workspace structure, open files, and can navigate your existing code more naturally. It understands what you're looking at and working on.

**Incremental Guidance**: When the agent starts implementing a change, you can course-correct immediately. "No, use the existing `UserService` instead of creating a new one" or "Follow the pattern in `handlers/auth.ts`" work beautifully in this context.

**Lower Stakes**: For codebases you understand deeply, you're not relying on the agent to figure out architecture—you're using it to accelerate implementation of changes you've already mentally designed.

## For New Projects: Claude Code Shines

When starting from scratch or exploring unfamiliar territory, I consistently reach for **Claude Code** (whether CLI or extension). Here's why:

### The Power of Plan Mode

New projects benefit enormously from thoughtful planning before implementation. Claude Code's plan mode is specifically designed for this workflow:

1. **Use Opus for Planning**: Start with Claude Opus in plan mode. Give it your requirements and let it think through the architecture, file structure, dependencies, and implementation approach.

2. **Human Review Phase**: This is critical. Review the plan carefully. Ask questions. Request alternatives. Refine the approach. The plan is cheap to modify—implementation is expensive.

3. **Switch to Sonnet for Implementation**: Once you're satisfied with the plan, switch to Claude Sonnet to execute it. Sonnet is faster and more cost-effective for implementation while still being highly capable.

### Why This Works

**Architectural Thinking**: Opus excels at high-level reasoning about architecture and design patterns. For new projects, this thoughtful planning phase prevents costly mistakes.

**Clean Slate**: Without existing code to navigate, you're not benefiting as much from VS Code's contextual awareness. Claude Code can build the structure from first principles.

**Batch Creation**: Creating multiple files, setting up project structure, configuring build tools—these tasks often work better in a more autonomous mode where the agent can work through a plan systematically.

**Focus on Design**: The plan mode forces you to think through your design before committing to code. This front-loaded thinking time pays dividends later.

## My Recommended Workflow

Here's what I do in practice:

### Starting a New Project
```
1. Use Claude Code CLI with Opus
2. Generate comprehensive plan
3. Review and refine plan (multiple iterations if needed)
4. Switch to Sonnet
5. Execute plan
6. Review generated code carefully
7. Transition to VS Code for ongoing development
```

### Working on Existing Projects
```
1. Use VS Code + Copilot Chat
2. Maintain copilot-instructions.md files
3. Guide the agent using workspace context
4. Intervene and course-correct as needed
5. Keep changes focused and reviewable
```

### Large Refactors or Feature Additions
```
1. If the change is well-defined: VS Code + Copilot Chat
2. If the approach needs design work: Claude Code (Opus) for planning
3. Review plan in VS Code
4. Execute in whichever environment feels right
```

## Key Learnings

### Always Review Plans Heavily
Whether using Opus or another model, **never skip the plan review phase** for significant work. Plans are cheap to modify; implemented code is expensive to refactor. Ask questions, request alternatives, and ensure you understand the approach before implementation begins.

### Opus for Architecture, Sonnet for Implementation
This cost-performance sweet spot works consistently well. Opus thinks through the hard problems, Sonnet executes the plan efficiently.

### copilot-instructions.md is a Superpower
For codebases you work on regularly, investing time in comprehensive `copilot-instructions.md` files pays enormous dividends. Document:
- Architectural patterns you follow
- Naming conventions
- Testing approaches
- Common gotchas
- File organization principles

### Context Switching is Okay
You're not married to one tool. Use Claude Code to scaffold a new service, then switch to VS Code + Copilot Chat for ongoing development. Use whatever tool fits the task.

## The Future is Multi-Tool

The most productive agentic coding workflow isn't about picking one tool and sticking with it dogmatically. It's about understanding the strengths of each approach and choosing the right one for your current context:

- **New project planning**: Claude Code with Opus
- **Plan execution**: Claude Code with Sonnet  
- **Existing codebase work**: VS Code with Copilot Chat
- **Guided refactoring**: VS Code with instructions files
- **Exploratory changes**: Claude Code for thinking space

We're still early in the agentic coding revolution. These tools are evolving rapidly, and the best practices are still being discovered. But one thing is clear: having multiple tools in your arsenal and knowing when to use each one is the path to maximum productivity.

---

*Note: Tool capabilities and features evolve quickly. This post reflects my experience as of January 2026.*
