---
title: STDIO vs HTTP MCP Servers in VS Code
date: 2026-01-06 10:00:00 +1100
author: Fuzzwah
description: Why STDIO should be your default for MCP servers, and when HTTP actually makes sense.
---

I've spent way too long messing around with MCP servers at this point, and one of the first decisions you hit when building or choosing one is the transport: STDIO or HTTP. Sounds like a boring infrastructure detail, right? Turns out it fundamentally changes how people actually use your server. Let me share what I've learnt building and deploying both.

## STDIO Servers: The Automatic Advantage

The killer feature of STDIO MCP servers is dead simple: **VS Code automatically starts them for you.**

When a user adds your server to their `settings.json`:

```json
{
  "mcp.servers": {
    "my-server": {
      "command": "node",
      "args": ["/path/to/server.js"]
    }
  }
}
```

That's it. VS Code handles starting the process when needed, restarting it if it crashes, shutting it down cleanly, and managing the whole lifecycle. Users don't run separate commands, manage background processes, or sit there wondering "is my server actually running?" It just works.

Beyond the auto-start magic, STDIO has a bunch of practical wins. Deployment is simpler — `npm install -g my-mcp-server`, add it to your settings, done. No ports to configure, no firewall rules, no network interfaces to bind. Each server runs as a separate process with its own environment variables, working directory, and resource limits, so you get proper isolation for free.

VS Code's MCP implementation is also optimised for STDIO with built-in process management, automatic logging capture, and native error handling. And since there's no network binding, you can run as many MCP servers as you want without worrying about port collisions:

```json
{
  "mcp.servers": {
    "postgres": { "command": "mcp-server-postgres", "args": ["..."] },
    "github": { "command": "mcp-server-github", "args": ["..."] },
    "filesystem": { "command": "mcp-server-filesystem", "args": ["..."] }
  }
}
```

All running simultaneously, zero configuration overhead.

### The Downsides

STDIO isn't perfect though. The big one is it's local only — the server has to run on the same machine as VS Code. You can't share an MCP server across a team, run resource-intensive servers on remote hardware, or connect to centralised company resources.

As the server developer, you also need to be careful with your I/O:

```typescript
// Reading from stdin
process.stdin.on('data', handleMessage);

// Writing to stdout
process.stdout.write(JSON.stringify(response));

// Error handling without corrupting stdout
console.error('Debug info goes to stderr');
```

Mixing stdout with debug output will break the protocol. I learnt that one the fun way.

Testing is a bit more fiddly too. STDIO testing requires process spawning, pipe management, and parsing JSON-RPC over streams. Compare that to HTTP where you can just `curl` your endpoint and call it a day:

```typescript
// HTTP testing
const response = await fetch('http://localhost:3000/api');
expect(response.status).toBe(200);

// STDIO testing
const child = spawn('node', ['server.js']);
child.stdin.write(JSON.stringify(request));
const output = await readFromStream(child.stdout);
// Parse JSON-RPC envelope...
```

And there's no load balancing — each VS Code instance gets its own server process. If 100 developers use your MCP server, that's 100 separate processes doing duplicate work, each connecting to the same database, caching the same data, and so on.

## HTTP Servers: Network Flexibility

Where HTTP shines is centralised, shared resources:

```json
{
  "mcp.servers": {
    "company-db": {
      "url": "https://mcp.company.com/database",
      "auth": "bearer ${env:MCP_TOKEN}"
    }
  }
}
```

Now your entire engineering team connects to one managed instance. That's pretty powerful for the right use case.

HTTP also opens up remote resources — cloud-hosted MCP servers, on-premise data centres, GPU-enabled machines for AI workloads, dedicated database servers. One HTTP server can serve hundreds of clients with shared connection pools, caching, and resource limits:

```
      ┌─────────┐
      │ MCP     │
  ┌───│ Server  │───┐
  │   │ (HTTP)  │   │
  │   └─────────┘   │
  │                 │
┌─▼────┐   ┌────▼──┐   ┌─▼────┐
│VS Code│   │VS Code│   │VS Code│
│ User1 │   │ User2 │   │ User3 │
└───────┘   └───────┘   └───────┘
```

Development and testing is easier with standard HTTP tooling — `curl`, browser dev tools, load testing tools, all the stuff you already know:

```bash
# Test with curl
curl http://localhost:3000/api \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "method": "..."}'
```

You also get proper enterprise security options — OAuth integration, API key management, rate limiting per user, audit logging, role-based access control. And you can deploy behind standard load balancing infrastructure:

```
┌─────────┐
│  Load   │
│Balancer │
└────┬────┘
     │
  ┌──┴──┬──────┬───────┐
  │     │      │       │
┌─▼──┐┌─▼──┐┌─▼──┐  ┌─▼──┐
│MCP ││MCP ││MCP │  │MCP │
│ S1 ││ S2 ││ S3 │  │ S4 │
└────┘└────┘└────┘  └────┘
```

### The HTTP Downsides

The critical drawback is manual startup. Users have to actually remember to start the server:

```bash
# Terminal 1: Start the server
npm start mcp-server
# Server running on http://localhost:3000

# Terminal 2: Use VS Code
# Configure settings.json to point to localhost:3000
```

Every time they restart their machine, they need to remember to fire it up again. This creates real friction — forgotten server startups, "why isn't my MCP working?" support tickets, and just general extra cognitive load that nobody wants.

On top of that, you've got port management (choosing available ports, avoiding conflicts, handling "address already in use" errors), more complex network configuration compared to STDIO's simple command + args, and security considerations. Even on localhost, you're exposing a network service, which means you need to think about authentication, TLS for remote servers, token management, and attack surface.

## When to Use Which

I reckon the decision comes down to your deployment model more than any technical preference.

**Go with STDIO when you're:**

- Building for individual developer use
- Running the server on the same machine as VS Code
- After a zero-configuration user experience
- Each user should have an isolated server instance
- No shared state between users is needed

Think local file system tools, personal productivity helpers, single-user database access, git integration, local code analysis. Basically most MCP servers.

**Go with HTTP when you need:**

- Shared or centralised resources across a team
- The server running on different hardware than VS Code
- Multiple users sharing one instance
- Enterprise authentication and authorisation
- Horizontal scaling

Think company-wide database access, shared API integrations, resource-intensive operations like ML models, multi-tenant services, cloud-hosted tools.

## The Hybrid Approach

Some MCP servers offer both transports, which is worth knowing about:

```typescript
// server.ts
if (process.env.TRANSPORT === 'http') {
  startHttpServer();
} else {
  startStdioServer();
}
```

Users choose based on their needs:

```bash
# Local/STDIO mode
mcp-server --transport stdio

# Shared/HTTP mode
mcp-server --transport http --port 3000
```

This gives maximum flexibility, but doubles your testing surface and maintenance burden. I'd only go down this path if you've got users actively asking for it.

## My Take

**Start with STDIO.** The automatic startup is transformative for adoption — it's the difference between "install and go" and "install, configure, remember to start, then go." It's simpler to build, easier to secure since there's no network exposure, and the reality is most MCP servers are personal tools anyway.

Only move to HTTP when you've got specific needs: shared resources across teams, remote server requirements, resource pooling, or enterprise auth. And if you do end up needing both, start with STDIO to validate your server, then add HTTP support once people are actually asking for it.

The transport mechanism isn't just a technical detail — it fundamentally shapes how users interact with your MCP server. I reckon getting this choice right early saves you a world of pain down the track. Stay tuned for more MCP adventures as I keep tinkering with this stuff.
