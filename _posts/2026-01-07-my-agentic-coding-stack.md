---
title: My Agentic Coding Stack
---

## Building with AI: My Current Setup

After months of experimenting with AI-assisted development, I've settled on a stack that maximizes productivity while maintaining control and understanding of my code. Here's what I'm running and why each piece matters.

## The Foundation: VS Code Insiders

**Why Insiders?** I run [VS Code Insiders](https://code.visualstudio.com/insiders/) rather than the stable release because:

- **Early access to features**: MCP (Model Context Protocol) support and Copilot improvements land here first
- **Bleeding edge stability**: It's surprisingly stable for a daily build
- **Side-by-side installation**: I can keep stable VS Code for critical work if needed

The only downside is occasional breaking changes, but for agentic coding development, being on the cutting edge is worth it.

## The Brain: GitHub Copilot

### Copilot Chat Extension

The [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extension is the core of my agentic workflow. Key features I use daily:

**1. Inline Chat (`Cmd+I`)**
- Quick refactoring and modifications without leaving the editor
- "Extract this to a function"
- "Add error handling here"
- "Make this async"

**2. Chat Panel**
- Longer conversations about architecture
- Explaining complex code
- Planning multi-step implementations

**3. Workspace Context**
- `@workspace` to search and reference my entire codebase
- Answers grounded in my actual code, not generic examples

### GitHub Copilot (Code Completions)

The original [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) extension for inline suggestions:
- Function implementations from comments
- Test case generation
- Boilerplate reduction
- Pattern continuation

## Model Selection Strategy

Not all AI models are created equal, and I've found specific models excel at specific tasks:

### Claude Sonnet 4.5 for Development

**My primary choice for feature implementation** is Claude Sonnet 4.5, especially when using custom agents. Why?

- **Superior reasoning**: Better at understanding complex requirements and edge cases
- **Context retention**: Maintains coherent understanding across long conversations
- **Code quality**: Generates more maintainable, idiomatic code
- **Planning capabilities**: Excellent at breaking down features into logical steps

I configure Copilot to use Claude Sonnet 4.5 as my default model for development work.

### GPT-5.2 for Code Review

For **code review tasks**, I switch to GPT-5.2:

- **Pattern recognition**: Excellent at spotting common bugs and anti-patterns
- **Speed**: Faster responses for review comments
- **Security focus**: Strong at identifying security vulnerabilities
- **Best practices**: Well-trained on conventional wisdom and standards

This two-model approach ensures I get the best of both worlds.

### The Planning → PRD → Implementation Workflow

My most successful pattern for complex features:

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

**2. Generate PRD**

I ask the agent: "Create a comprehensive PRD for this feature, including:
- User stories
- Technical approach
- Database schema changes
- API endpoints
- Security considerations
- Testing strategy"

The agent produces a detailed document using all available context.

**3. Review and Edit**

This is critical: **I review every line of the PRD**. I:
- Clarify ambiguous requirements
- Add constraints the AI might have missed
- Remove over-engineered solutions
- Ensure alignment with existing architecture

I often spend 30-60 minutes refining the PRD. This time is worth it.

**4. Fresh Session for Implementation**

Here's the key insight: **Start a completely new chat session**.

Why fresh?
- No baggage from exploratory conversations
- Clean context focused on execution
- Agent has one clear source of truth: the PRD
- Reduces hallucinations from earlier discussions

I paste the finalized PRD and say: "Implement this PRD. Ask clarifying questions before you start."

**5. Iterative Implementation**

The agent implements step-by-step:
- Database migrations
- Models and business logic
- API endpoints
- Tests
- Documentation

Because the PRD is solid, the implementation is remarkably smooth.

### Why This Works

**Separation of concerns**: Planning brain ≠ implementation brain

**Explicit requirements**: No "I thought you meant..." moments

**Reviewable artifacts**: PRD becomes documentation for the team

**Quality control**: Human review at the critical juncture

**Context efficiency**: Fresh session = focused agent

This workflow has reduced my feature development time by 40% while *improving* code quality.

## Essential Extensions

These extensions enhance the AI-assisted workflow:

### Code Quality

**[ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)**
- Copilot's suggestions aren't always lint-clean
- Real-time feedback keeps code quality high
- Auto-fix on save for common issues

**[Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)**
- Consistent formatting without thinking about it
- Works seamlessly with Copilot's generated code
- Format on save keeps everything clean

### Git Integration

**[GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)**
- Understand when and why code was changed
- Essential context when asking Copilot about existing code
- Blame annotations help identify who to ask for clarification

### Language-Specific

**[Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)**
- IntelliSense for Python
- Debugging
- Environment management

**[Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance)**
- Fast, feature-rich Python language server
- Type checking that catches issues Copilot might miss
- Better completions and suggestions

## Model Context Protocol (MCP) Servers

MCP is game-changing for giving AI context beyond my codebase. Here are the servers I run:

### [postgres-mcp](https://github.com/crystaldba/postgres-mcp)

**Why?** Direct database access for the AI agent.

**Use cases:**
- "Show me all users created in the last week"
- "What's the schema for the orders table?"
- "Write a query to find duplicate emails"

**Setup:**
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

I use `dml_only` mode to prevent accidental schema changes (see my [DML Only Mode post](2026-01-05-building-safe-dml-only-mode-for-postgres-mcp.md)).

### [filesystem-mcp](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)

**Why?** Read/write files outside the current workspace.

**Use cases:**
- Processing log files
- Reading config from other projects
- Generating files in specific directories

**Setup:**
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

**Why?** Search repos, read issues, create PRs without leaving the chat.

**Use cases:**
- "Show me recent issues labeled 'bug'"
- "Create a PR with these changes"
- "Search for examples of authentication in our org's repos"

**Setup:**
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

## Workflow in Practice

Here's how these pieces work together:

### Example: Adding a Feature

1. **Planning**: Chat with Copilot about architecture
   - "How should I implement user preferences in this Django app?"
   - Uses `@workspace` to understand current structure

2. **Database Design**: Use postgres-mcp
   - "What migrations do I need for a user_preferences table?"
   - Agent inspects current schema via MCP

3. **Implementation**: Inline suggestions + chat
   - Copilot suggests model code
   - Pylance catches type errors
   - ESLint enforces style

4. **Testing**: Copilot generates tests
   - "Write tests for the preference save logic"
   - Runs tests to verify

5. **Documentation**: github-mcp for PR
   - "Create a PR with summary and link to issue #123"
   - Automatically formats and submits

### Example: Debugging

1. **Understand the issue**
   - GitLens shows when the bug was introduced
   - `@workspace` finds related code

2. **Investigate data**
   - postgres-mcp queries production data (read-only!)
   - "Show me the last 10 failed transactions"

3. **Fix and verify**
   - Copilot suggests fix based on context
   - ESLint ensures code quality
   - Write regression test

## Key Learnings

1. **MCP is a multiplier**: The more context you give AI, the better it performs
2. **STDIO > HTTP for MCP**: Auto-start means zero friction (see my [STDIO vs HTTP post](2026-01-06-stdio-vs-http-mcp-servers-in-vscode.md))
3. **Quality tools matter**: ESLint + Prettier keep AI-generated code maintainable
4. **Limit scope wisely**: DML-only postgres, restricted filesystem paths—safety first
5. **Right model for right task**: Claude for development, GPT for review
6. **Planning mode + PRD workflow**: Separate thinking from doing for better results
7. **Fresh sessions are powerful**: Clean slate prevents context pollution

## What's Next?

I'm exploring:
- Custom MCP servers for internal tools
- Better prompt engineering for complex refactorings
- Integration with CI/CD for automated fixes

---

*Agentic coding isn't about replacing developers—it's about amplifying what we can do. The right stack makes that amplification seamless.*
