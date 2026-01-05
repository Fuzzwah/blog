---
title: STDIO vs HTTP MCP Servers in VS Code
---

## The Two Paths to MCP Integration

When building a Model Context Protocol (MCP) server for VS Code, one of your first architectural decisions is choosing the transport mechanism: **STDIO** or **HTTP**. Each has distinct advantages and trade-offs that affect deployment, security, and user experience.

Let me share what I've learned building and deploying both types of MCP servers.

## STDIO Servers: The Automatic Advantage

### The Core Benefit: Zero-Touch Startup ✨

The killer feature of STDIO MCP servers is elegantly simple: **VS Code automatically starts them for you.**

When a user installs your MCP server and adds it to their `settings.json`:

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

That's it. VS Code handles:
- Starting the process when needed
- Restarting it if it crashes
- Shutting it down cleanly
- Managing the lifecycle completely

Users don't run separate commands, manage background processes, or worry about "is my server running?"

### Additional STDIO Advantages

**1. Simpler Deployment**
```bash
# User installation
npm install -g my-mcp-server

# Configuration
# Just add to settings.json - done!
```

No ports to configure, no firewall rules, no network interfaces to bind.

**2. Better Isolation**

Each STDIO server runs as a separate process with its own:
- Environment variables
- Working directory
- Process isolation
- Resource limits

**3. Native Integration**

VS Code's MCP implementation is optimized for STDIO:
- Built-in process management
- Automatic logging capture
- Native error handling
- Standardized configuration

**4. No Port Conflicts**

Since there's no network binding, you can run multiple MCP servers without worrying about port collisions:

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

### STDIO Disadvantages

**1. Local Only**

STDIO servers must run on the same machine as VS Code. You can't:
- Share an MCP server across a team
- Run resource-intensive servers on remote hardware
- Connect to centralized company resources

**2. Process Management Complexity**

As the server developer, you need to handle:
```typescript
// Reading from stdin
process.stdin.on('data', handleMessage);

// Writing to stdout
process.stdout.write(JSON.stringify(response));

// Error handling without corrupting stdout
console.error('Debug info goes to stderr');
```

Mixing stdout with debug output will break the protocol.

**3. Harder Testing**

Testing STDIO servers requires:
- Process spawning in tests
- Pipe management
- Parsing JSON-RPC over streams

Compare to HTTP testing:
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

**4. No Load Balancing**

Each VS Code instance gets its own server process. If 100 developers use your MCP server, that's 100 separate processes doing duplicate work—each connecting to the same database, caching the same data, etc.

## HTTP Servers: Network Flexibility

### The Core Benefit: Centralized Services 🌐

HTTP MCP servers enable shared, centralized resources:

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

Now your entire engineering team connects to one managed instance.

### Additional HTTP Advantages

**1. Remote Resources**

Connect to servers running anywhere:
- Cloud-hosted MCP servers
- On-premise data centers
- GPU-enabled machines for AI workloads
- Dedicated database servers

**2. Better Resource Management**

One HTTP server can serve hundreds of clients:
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

Shared connection pools, caching, and resource limits.

**3. Easier Development & Testing**

Standard HTTP tooling works out of the box:
```bash
# Test with curl
curl http://localhost:3000/api \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "method": "..."}'

# Use browser dev tools
# Standard load testing tools
# Familiar debugging
```

**4. Authentication & Authorization**

HTTP enables enterprise-grade security:
```typescript
// OAuth integration
// API key management
// Rate limiting per user
// Audit logging
// Role-based access control
```

**5. Load Balancing & Scaling**

Deploy behind standard infrastructure:
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

### HTTP Disadvantages

**1. Manual Startup Required** ⚠️

This is the critical drawback. Users must:

```bash
# Terminal 1: Start the server
npm start mcp-server
# Server running on http://localhost:3000

# Terminal 2: Use VS Code
# Configure settings.json to point to localhost:3000
```

Every time they restart their machine, they need to remember to start the server again. This creates friction:
- Forgotten server startups
- "Why isn't my MCP working?" support tickets
- Manual process management
- Extra cognitive load

**2. Port Management**

Users need to:
- Choose available ports
- Avoid conflicts with other services
- Configure firewalls
- Handle "address already in use" errors

**3. Network Configuration**

More complex setup:
```json
{
  "mcp.servers": {
    "my-server": {
      "url": "http://localhost:3000",
      "headers": {
        "Authorization": "Bearer ${env:API_KEY}"
      },
      "timeout": 30000
    }
  }
}
```

vs STDIO's simple command + args.

**4. Security Considerations**

Even on localhost, you're exposing a network service:
- Need authentication for any sensitive data
- TLS configuration for remote servers
- Token management
- Attack surface considerations

## Decision Matrix

### Choose STDIO When:

✅ Building for individual developer use  
✅ Server runs on the same machine as VS Code  
✅ Want zero-configuration user experience  
✅ Each user should have isolated server instance  
✅ No shared state between users needed  
✅ Prioritizing ease of installation  

**Example use cases:**
- Local file system tools
- Personal productivity helpers
- Single-user database access
- Git integration
- Local code analysis

### Choose HTTP When:

✅ Need shared/centralized resources  
✅ Server runs on different hardware than VS Code  
✅ Multiple users should share one instance  
✅ Need enterprise authentication/authorization  
✅ Want to scale horizontally  
✅ Connecting to existing HTTP APIs  

**Example use cases:**
- Company-wide database access
- Shared API integrations (GitHub, Jira, etc.)
- Resource-intensive operations (ML models, etc.)
- Multi-tenant services
- Cloud-hosted tools

## The Hybrid Approach

Some MCP servers offer both transports:

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

This gives maximum flexibility, but doubles your testing surface and maintenance burden.

## Real-World Example: postgres-mcp

The [postgres-mcp](https://github.com/crystaldba/postgres-mcp) server defaults to STDIO because:

1. **Database connections are local or tunneled anyway**
   - Users SSH tunnel to remote databases
   - Connection strings contain sensitive credentials
   - Each developer needs different database access

2. **Auto-start is critical for UX**
   - Developers open VS Code and immediately start working
   - No "forgot to start my MCP server" delays
   - Server lifecycle matches VS Code lifecycle

3. **Isolation is a feature**
   - Each developer's queries don't impact others
   - Personal query history
   - Individual access mode settings (read-only, DML-only, unrestricted)

But an HTTP version could make sense for:
- Shared development databases
- Query logging/auditing across a team
- Centralized connection pool management

## My Recommendation

**Start with STDIO.** Here's why:

1. **User experience wins**: The automatic startup is transformative for adoption
2. **Simpler to build**: Less infrastructure code, fewer edge cases
3. **Easier to secure**: No network exposure to worry about
4. **Better defaults**: Most MCP servers are personal tools

Only move to HTTP when you have specific needs:
- Shared resources across teams
- Remote server requirements
- Resource pooling needs
- Enterprise authentication requirements

And if you do need both, start with STDIO to validate your MCP server, then add HTTP support once you have users asking for it.

## Key Takeaways

1. **STDIO's auto-start is a killer feature** that dramatically improves user experience
2. **HTTP enables centralized, shared resources** but at the cost of manual startup
3. **Choose based on deployment model**, not just technical preferences
4. **Most MCP servers should default to STDIO** unless they have specific centralized requirements
5. **Supporting both is possible** but doubles complexity—only do it if needed

The transport mechanism isn't just a technical detail—it fundamentally shapes how users interact with your MCP server. Choose wisely based on your users' workflows, not just what's easier to code.

---

*Building MCP servers? The choice between STDIO and HTTP affects everything from user onboarding to scaling strategy. Start simple, evolve when needed.*
