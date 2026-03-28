---
title: My Agentic Coding Stack
date: 2026-01-07 10:00:00 +1100
author: Fuzzwah
description: A run through of my agentic coding setup and the workflow that's changed how I build things.
update_notice:
  message: "My workflow has evolved a lot since this post. I now run parallel agents across multiple repos using Conductor, with CLAUDE.md files as shared context."
  url: /2026/03/28/conductor-orchestrating-ai-agents-across-repos/
  link_text: "See the Conductor multi-repo workflow"
---

I've been messing around with AI-assisted development for months now, and after a frankly embarrassing amount of tinkering I've landed on a setup that actually works. I can already feel your eyes preparing to glaze over, but stick with me — this stuff is genuinely exciting and has completely changed how I build things.

## The Foundation: VS Code Insiders

I run [VS Code Insiders](https://code.visualstudio.com/insiders/) rather than the stable release because:

- MCP support and Copilot improvements land here first
- It's surprisingly stable for a daily build
- I can keep stable VS Code installed side-by-side for critical work if needed

The only real downside is the occasional breaking change, but for agentic coding development being on the bleeding edge is worth it.

## The Brain: GitHub Copilot

The [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extension is the core of my agentic workflow. I'm using inline chat (`Cmd+I`) constantly for quick refactoring without leaving the editor, the chat panel for longer conversations about architecture, and `@workspace` to search and reference my entire codebase so answers are grounded in my actual code rather than generic examples.

On top of that, the original [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) extension handles inline code completions — function implementations from comments, test generation, boilerplate reduction, pattern continuation. All the stuff that makes you wonder how you ever typed all that crap by hand.

## Model Selection

Not all AI models are created equal, and I've found specific models shine at specific tasks.

**Claude Sonnet 4.5** is my primary choice for feature implementation, especially when using custom agents. The reasoning is superb — it handles complex requirements and edge cases, maintains coherent context across long conversations, and generates code that's actually maintainable and idiomatic. I've configured Copilot to use it as my default for development work and I'm pretty damn chuffed with the results.

For **code review tasks** I switch over to **GPT-5.2**. It's faster for review comments, excellent at spotting common bugs and anti-patterns, and has a strong eye for security vulnerabilities. This two-model approach gives me the best of both worlds.

## The Planning → PRD → Implementation Workflow

This is the bit I'm most excited to share because it's genuinely changed the game for me. Here's how it works for complex features:

**1. Planning Mode Session**

I start with planning mode enabled and describe the feature:
```
I need to add user authentication with OAuth support.
Requirements:
- Support Google and GitHub
- Store tokens securely
- Handle token refresh
- Admin can revoke access
```

**2. Generate a PRD**

I ask the agent to create a comprehensive PRD covering:
- User stories
- Technical approach
- Database schema changes
- API endpoints
- Security considerations
- Testing strategy

The agent produces a detailed document using all available context.

**3. Review and Edit**

This is the critical bit: **I review every line of the PRD**. I clarify ambiguous requirements, add constraints the AI might have missed, remove over-engineered solutions, and make sure it all aligns with existing architecture. I often spend 30 to 60 minutes refining the PRD. This time is absolutely worth it.

**4. Fresh Session for Implementation**

Here's the key insight that took me too long to figure out: **start a completely new chat session**. No baggage from exploratory conversations, clean context focused on execution, and the agent has one clear source of truth — the PRD. I paste the finalised PRD and say: "Implement this PRD. Ask clarifying questions before you start."

**5. Iterative Implementation**

The agent implements step by step — database migrations, models and business logic, API endpoints, tests, documentation. Because the PRD is solid, the implementation is remarkably smooth.

### Why This Works

The core idea is separation of concerns — planning brain and implementation brain are different things. Separating them means explicit requirements with no "I thought you meant..." moments, reviewable artefacts that become documentation for the team, and human review at the critical juncture. A fresh session means a focused agent.

This workflow has reduced my feature development time by roughly 40% while actually *improving* code quality. I can happily report back that I'm a total convert.

## Essential Extensions

A few extensions that really make the AI-assisted workflow sing:

- **[ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)** — Copilot's suggestions aren't always lint-clean, so real-time feedback and auto-fix on save keeps things tidy
- **[Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)** — consistent formatting without thinking about it, works seamlessly with generated code
- **[GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)** — shows when and why code was changed, essential context when asking Copilot about existing code
- **[Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)** + **[Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance)** — IntelliSense, debugging, and type checking that catches issues Copilot might miss

## MCP Servers

MCP is a game changer for giving AI context beyond your codebase. Here are the servers I'm running.

### [postgres-mcp](https://github.com/crystaldba/postgres-mcp)

Direct database access for the AI agent. I can ask things like "show me all users created in the last week" or "what's the schema for the orders table" and it just works.

```json
{
  "mcp.servers": {
    "postgres": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-server-postgres",
        "postgresql://user:pass@localhost/dbname",
        "--access-mode",
        "dml_only"
      ]
    }
  }
}
```

I run it in `dml_only` mode to prevent accidental schema changes — I wrote about building that safety net in my [DML Only Mode post](2026-01-05-building-safe-dml-only-mode-for-postgres-mcp.md).

### [filesystem-mcp](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)

Lets the agent read and write files outside the current workspace — processing log files, reading config from other projects, generating files in specific directories. Dead useful.

```json
{
  "mcp.servers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/me/allowed-directory"
      ]
    }
  }
}
```

### [github-mcp](https://github.com/modelcontextprotocol/servers/tree/main/src/github)

Search repos, read issues, create PRs without leaving the chat. The less context switching, the better.

```json
{
  "mcp.servers": {
    "github": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-github"
      ],
      "env": {
        "GITHUB_TOKEN": "${env:GITHUB_TOKEN}"
      }
    }
  }
}
```

## What I've Learnt

After living with this stack for a while, a few things have become clear:

1. **MCP is a multiplier** — the more context you give AI, the better it performs
2. **STDIO beats HTTP for MCP** — auto-start means zero friction (more on that in my [STDIO vs HTTP post](2026-01-06-stdio-vs-http-mcp-servers-in-vscode.md))
3. **Quality tools matter** — ESLint + Prettier keep AI-generated code maintainable
4. **Limit scope wisely** — DML-only postgres, restricted filesystem paths. Let the agent do its thing without worrying it's going to nuke your database
5. **Right model for the right task** — Claude for development, GPT for review
6. **Separate planning from implementation** — the PRD workflow with fresh sessions is the single biggest productivity unlock I've found

I'm currently exploring custom MCP servers for internal tools, better prompt engineering for complex refactorings, and integration with CI/CD for automated fixes. Stay tuned.
