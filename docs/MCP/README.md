# Model Context Protocol (MCP) Documentation

**Last Updated:** January 26, 2026

Welcome to the Model Context Protocol (MCP) documentation for OLLM CLI. This section covers MCP integration, servers, commands, and the marketplace.

---

## 📚 Documentation Overview

### Core Documentation

- **[MCP Index](MCP_Index.md)** - Complete documentation index
- **[Getting Started](MCP_GettingStarted.md)** - Quick start guide
- **[Architecture](MCP_Architecture.md)** - System architecture and design
- **[Integration](MCP_Integration.md)** - Integration guide
- **[Commands](MCP_Commands.md)** - CLI command reference
- **[Marketplace](MCP_Marketplace.md)** - MCP server marketplace

---

## 🎯 What is MCP?

The **Model Context Protocol (MCP)** is an open standard that enables LLMs to securely access external tools and data sources. OLLM CLI implements MCP to provide:

### 1. **External Tool Integration**

Connect to MCP servers that provide tools:

- File system operations
- Database queries
- API integrations
- Web services
- Custom tools

### 2. **Resource Access**

Access external resources:

- Documentation
- Code repositories
- Databases
- APIs
- File systems

### 3. **Prompt Templates**

Use server-provided prompts:

- Pre-configured workflows
- Domain-specific templates
- Best practice patterns

### 4. **OAuth Authentication**

Secure authentication for services:

- GitHub, Google, Slack, etc.
- Token management
- Automatic refresh
- Secure storage

---

## 📖 Documentation Structure

```
docs/MCP/
├── README.md                    ← You are here
├── MCP_Index.md                 Complete documentation index
├── MCP_GettingStarted.md        Quick start guide
├── MCP_Architecture.md          System architecture
├── MCP_Integration.md           Integration guide
├── MCP_Commands.md              CLI commands
└── MCP_Marketplace.md           Server marketplace
```

---

## 🎓 Key Concepts

### MCP Servers

External processes that provide tools to LLMs:

**Server Types:**

- **Official Servers** - Maintained by MCP team
- **Community Servers** - Third-party servers
- **Custom Servers** - Your own servers

**Communication:**

- **stdio** - Standard input/output (most common)
- **SSE** - Server-Sent Events (HTTP streaming)
- **HTTP** - REST API (planned)

**See:** [Getting Started](MCP_GettingStarted.md#mcp-servers)

### MCP Tools

Functions provided by MCP servers:

**Tool Discovery:**

1. Connect to MCP server
2. Server lists available tools
3. Tools registered in tool registry
4. LLM can call tools

**Tool Execution:**

1. LLM requests tool call
2. OLLM CLI validates parameters
3. Request sent to MCP server
4. Server executes and returns result
5. Result passed back to LLM

**See:** [Architecture](MCP_Architecture.md#tool-execution)

### OAuth Integration

Secure authentication for MCP servers:

**Supported Providers:**

- GitHub
- Google
- Slack
- Microsoft
- Custom OAuth providers

**Flow:**

1. Server requests OAuth
2. User authorizes in browser
3. Token stored securely
4. Automatic token refresh

**See:** [Integration](MCP_Integration.md#oauth-setup)

### MCP Marketplace

Discover and install MCP servers:

**Features:**

- Browse available servers
- Search by category
- Install with one command
- Automatic configuration
- Update management

**See:** [Marketplace](MCP_Marketplace.md)

---

## 💡 Common Use Cases

### File System Operations

```bash
# Install filesystem server
/mcp install @modelcontextprotocol/server-filesystem

# Configure
/mcp config filesystem --path /workspace

# Use in chat
"List all TypeScript files in src/"
```

### GitHub Integration

```bash
# Install GitHub server
/mcp install @modelcontextprotocol/server-github

# Authenticate
/mcp oauth login github

# Use in chat
"Create a new issue in my repo"
"List recent pull requests"
```

### Database Queries

```bash
# Install database server
/mcp install @modelcontextprotocol/server-postgres

# Configure
/mcp config postgres --connection-string $DATABASE_URL

# Use in chat
"Query users table"
"Show database schema"
```

**Learn more:** [Getting Started](MCP_GettingStarted.md#examples)

---

## 🛠️ Configuration

### MCP Server Configuration

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/workspace"],
      "env": {}
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

### OAuth Configuration

```json
{
  "oauth": {
    "github": {
      "clientId": "your-client-id",
      "clientSecret": "${GITHUB_CLIENT_SECRET}",
      "scopes": ["repo", "user"]
    }
  }
}
```

**Learn more:** [Integration](MCP_Integration.md#configuration)

---

## 🔍 Troubleshooting

### Common Issues

**MCP server not starting:**

- Check server installed: `/mcp list`
- Verify command path
- Check environment variables
- View logs: `/mcp logs <server>`

**Tools not available:**

- Check server running: `/mcp status`
- Restart server: `/mcp restart <server>`
- Check tool registration: `/tools list`

**OAuth authentication fails:**

- Check OAuth configured: `/mcp oauth status`
- Re-authenticate: `/mcp oauth login <provider>`
- Check token expiry: `/mcp oauth status <provider>`

**Server crashes:**

- Check logs: `/mcp logs <server>`
- Verify dependencies installed
- Check server version compatibility
- Report issue to server maintainer

**See:** [Troubleshooting Guide](../Troubleshooting.md)

---

## 📊 Implementation Status

### Current (v0.1.6)

- ✅ MCP Client Implementation
- ✅ stdio Transport
- ✅ Tool Discovery and Registration
- ✅ Tool Execution
- ✅ OAuth Integration (basic)
- ✅ Server Management Commands
- ✅ Health Monitoring

### Planned (v0.2.0)

- ⏳ SSE Transport
- ⏳ HTTP Transport
- ⏳ Resource Support
- ⏳ Prompt Template Support
- ⏳ Advanced OAuth Features

### Planned (v0.3.0)

- ⏳ MCP Marketplace UI
- ⏳ Server Analytics
- ⏳ Performance Monitoring

---

## 🤝 Related Documentation

### Core Systems

- [Tools System](../Tools/README.md) - Tool execution
- [Hooks System](../Hooks/README.md) - Event automation
- [User Interface](../UI&Settings/README.md) - UI documentation

### Commands

- [MCP Commands](MCP_Commands.md) - All MCP commands
- [OAuth Commands](MCP_Commands.md#oauth-commands) - OAuth management

### Developer Resources

- Knowledge DB: `dev_MCPIntegration.md` - MCP architecture
- MCP Specification: https://modelcontextprotocol.io

---

## 🎯 Quick Start

### For New Users

1. **List Available Servers**

   ```bash
   /mcp marketplace
   ```

2. **Install Your First Server**

   ```bash
   /mcp install @modelcontextprotocol/server-filesystem
   ```

3. **Start Using Tools**
   ```
   "List files in the current directory"
   ```

### For Advanced Users

1. **Configure Custom Server**

   ```bash
   /mcp add my-server --command node --args server.js
   ```

2. **Set Up OAuth**

   ```bash
   /mcp oauth login github
   ```

3. **Monitor Server Health**
   ```bash
   /mcp health check
   ```

---

## 📈 Best Practices

### Server Management

1. **Start Small** - Install one server at a time
2. **Test Thoroughly** - Verify tools work before relying on them
3. **Monitor Health** - Check server status regularly
4. **Update Regularly** - Keep servers up to date
5. **Review Logs** - Check logs for errors

### Security

1. **Review Servers** - Only install trusted servers
2. **Limit Permissions** - Grant minimal necessary permissions
3. **Secure Tokens** - Use environment variables for secrets
4. **Regular Audits** - Review installed servers periodically
5. **OAuth Best Practices** - Use OAuth when available

### Performance

1. **Lazy Loading** - Only start servers when needed
2. **Resource Limits** - Set appropriate resource limits
3. **Cache Results** - Cache expensive operations
4. **Monitor Usage** - Track server resource usage

---

## 🌐 External Resources

### Official MCP Resources

- **MCP Specification:** https://modelcontextprotocol.io
- **MCP SDK:** https://github.com/modelcontextprotocol/sdk
- **Official Servers:** https://github.com/modelcontextprotocol/servers

### Community

- **Discord:** Join MCP community
- **GitHub Discussions:** Ask questions
- **Server Registry:** Browse community servers

---

**Last Updated:** January 26, 2026  
**Version:** 0.1.6  
**Status:** Active Development
