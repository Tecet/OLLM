# MCP Integration Guide

**Practical Guide to Integrating MCP Servers with OLLM CLI**

---

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Quick Start](#quick-start)
4. [Configuration](#configuration)
5. [Using MCP Tools](#using-mcp-tools)
6. [OAuth Authentication](#oauth-authentication)
7. [Health Monitoring](#health-monitoring)
8. [Troubleshooting](#troubleshooting)
9. [Best Practices](#best-practices)

---

## Introduction

This guide walks you through integrating MCP (Model Context Protocol) servers with OLLM CLI. By the end, you'll be able to:

- Configure and start MCP servers
- Discover and use external tools
- Set up OAuth authentication
- Monitor server health
- Troubleshoot common issues

### What is MCP?

MCP (Model Context Protocol) is a standard protocol that allows AI assistants to connect to external data sources and tools. It enables:

- **Tool Integration**: Access external APIs and services
- **Resource Access**: Read files, databases, and other resources
- **Prompt Templates**: Use pre-defined prompts
- **Secure Authentication**: OAuth 2.0 support

---

## Prerequisites

### System Requirements

- Node.js 20+ installed
- OLLM CLI installed and configured
- Internet connection (for remote MCP servers)

### Optional Requirements

- OAuth credentials (for secure servers)
- MCP server packages (for local servers)

---

## Quick Start

### 1. Install an MCP Server

Let's start with a simple example using the GitHub MCP server:

```bash
# Install the GitHub MCP server globally
npm install -g @modelcontextprotocol/server-github
```

### 2. Configure the Server

Create or edit your OLLM configuration file:

**Location:** `~/.ollm/config.yaml` or `.ollm/config.yaml`

```yaml
mcp:
  enabled: true
  connectionTimeout: 10000
  servers:
    github:
      command: 'npx'
      args: ['-y', '@modelcontextprotocol/server-github']
      env:
        GITHUB_TOKEN: '${GITHUB_TOKEN}'
      transport: 'stdio'
```

### 3. Set Environment Variable

```bash
# Set your GitHub token
export GITHUB_TOKEN='your_github_token_here'
```

### 4. Start OLLM CLI

```bash
ollm
```

The GitHub MCP server will start automatically, and its tools will be available to the agent.

### 5. Use MCP Tools

In the OLLM CLI, you can now use GitHub tools:

```
> Create an issue in my repo about the bug we discussed

The agent will use the `github_create_issue` tool automatically.
```

---

## Configuration

### Configuration File Structure

```yaml
mcp:
  # Enable/disable MCP integration
  enabled: true
  
  # Connection timeout in milliseconds
  connectionTimeout: 10000
  
  # MCP servers configuration
  servers:
    # Server name (can be anything)
    server-name:
      # Command to start the server
      command: 'node'
      
      # Command arguments
      args: ['path/to/server.js']
      
      # Environment variables
      env:
        API_KEY: '${API_KEY}'
        DEBUG: 'true'
      
      # Transport type: stdio, sse, or http
      transport: 'stdio'
      
      # Optional: Connection timeout override
      timeout: 15000
```

### Transport Types

#### Stdio Transport (Recommended)

Best for local MCP servers running as child processes.

```yaml
servers:
  local-server:
    command: 'node'
    args: ['server.js']
    transport: 'stdio'
```

**Pros:**
- Fast and efficient
- No network overhead
- Easy to debug

**Cons:**
- Only works for local servers
- Requires server executable

#### SSE Transport

Best for remote MCP servers with long-lived connections.

```yaml
servers:
  remote-server:
    command: 'https://api.example.com/mcp'
    transport: 'sse'
    env:
      AUTHORIZATION: 'Bearer ${API_TOKEN}'
```

**Pros:**
- Works with remote servers
- Real-time updates
- Efficient for streaming

**Cons:**
- Requires server support
- Network dependent

#### HTTP Transport

Best for simple remote MCP servers.

```yaml
servers:
  api-server:
    command: 'https://api.example.com/mcp'
    transport: 'http'
    env:
      API_KEY: '${API_KEY}'
```

**Pros:**
- Simple and universal
- Works with any HTTP server
- Easy to implement

**Cons:**
- Higher latency
- No streaming support
- More network overhead

### Environment Variable Substitution

Use `${VAR_NAME}` syntax to reference environment variables:

```yaml
servers:
  github:
    env:
      GITHUB_TOKEN: '${GITHUB_TOKEN}'
      GITHUB_ORG: '${GITHUB_ORG}'
      DEBUG: 'true'  # Literal value
```

**Rules:**
- Variables are substituted from parent process environment
- Missing variables log a warning and use empty string
- Literal values don't need substitution syntax

---

## Using MCP Tools

### Discovering Available Tools

Use the `/mcp` command to list servers and tools:

```bash
# List all MCP servers
/mcp

# List tools from a specific server
/mcp tools github
```

**Output:**
```
3 tool(s) from 'github':

1. github_create_issue
   Create a new issue in a repository
   Input schema: {
     "repo": "string (required)",
     "title": "string (required)",
     "body": "string (optional)"
   }

2. github_list_issues
   List issues in a repository
   ...
```

### Using Tools in Conversation

The agent will automatically use MCP tools when appropriate:

```
User: Create an issue titled "Fix login bug" in my repo

Agent: I'll create that issue for you.
[Uses github_create_issue tool]
✓ Issue created: #123
```

### Manual Tool Invocation

You can also invoke tools manually (advanced):

```typescript
const result = await mcpClient.callTool('github', 'github_create_issue', {
  repo: 'owner/repo',
  title: 'Fix login bug',
  body: 'Description of the bug'
});
```

### Tool Call Timeout

Default timeout: 30 seconds (configurable)

```yaml
mcp:
  servers:
    slow-server:
      command: 'node'
      args: ['server.js']
      timeout: 60000  # 60 seconds
```

---

## OAuth Authentication

### When to Use OAuth

OAuth is required for MCP servers that access protected resources:

- GitHub (private repositories)
- Google Workspace (Gmail, Drive, Calendar)
- Slack (private channels)
- Other authenticated APIs

### OAuth Configuration

#### 1. Configure OAuth in Manifest

If using an extension:

```json
{
  "name": "github-extension",
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "oauth": {
        "enabled": true,
        "authorizationUrl": "https://github.com/login/oauth/authorize",
        "tokenUrl": "https://github.com/login/oauth/access_token",
        "scopes": ["repo", "user"]
      }
    }
  }
}
```

#### 2. Start OAuth Flow

```bash
# Check OAuth status
/mcp oauth status github

# Start OAuth login
/mcp oauth login github
```

**What Happens:**
1. Browser opens to authorization page
2. You authorize the application
3. Token is stored securely
4. Server can now access protected resources

#### 3. Token Storage

Tokens are stored securely:

**Primary:** Platform keychain
- Windows: Credential Manager
- macOS: Keychain
- Linux: Secret Service API

**Fallback:** Encrypted file
- Location: `~/.ollm/oauth-tokens.json`
- Encryption: AES-256-GCM
- Permissions: User-only read/write

#### 4. Token Refresh

Tokens are automatically refreshed before expiration:

```typescript
// Automatic refresh (no user action needed)
if (token.expiresAt < Date.now() + 300000) {  // 5 minutes before expiry
  await oauthProvider.refreshToken(serverName);
}
```

#### 5. Token Revocation

```bash
# Revoke OAuth token
/mcp oauth revoke github

# This will:
# - Remove token from storage
# - Revoke token with provider (if supported)
# - Require re-authentication
```

### OAuth Troubleshooting

**Problem:** Browser doesn't open

**Solution:**
```bash
# Manually open the URL shown in the terminal
# Copy the authorization code from the callback URL
```

**Problem:** Token expired

**Solution:**
```bash
# Tokens refresh automatically
# If refresh fails, re-authenticate:
/mcp oauth login github
```

**Problem:** Permission denied

**Solution:**
```bash
# Check required scopes in manifest
# Re-authenticate with correct scopes:
/mcp oauth revoke github
/mcp oauth login github
```

---

## Health Monitoring

### Automatic Health Checks

OLLM CLI automatically monitors MCP server health:

- **Check Interval:** 30 seconds (configurable)
- **Auto-Restart:** Up to 3 attempts
- **Exponential Backoff:** 1s, 2s, 4s

### Health Commands

```bash
# Check health of all servers
/mcp health

# Check specific server
/mcp health check github

# Manually restart a server
/mcp restart github

# Start automatic monitoring
/mcp health start

# Stop automatic monitoring
/mcp health stop

# Check monitoring status
/mcp health status
```

### Health States

- **✅ Healthy:** Server responding normally
- **⚠️ Degraded:** Server slow or intermittent
- **❌ Failed:** Server not responding
- **🔄 Restarting:** Attempting to restart

### Configuration

```yaml
mcpHealth:
  enabled: true
  checkInterval: 30000      # 30 seconds
  maxRestartAttempts: 3
  autoRestart: true
```

### Health Events

You can listen to health events via hooks:

```json
{
  "hooks": {
    "notification": [
      {
        "name": "health-alert",
        "command": "node",
        "args": ["hooks/health-alert.js"]
      }
    ]
  }
}
```

**Hook Input:**
```json
{
  "event": "notification",
  "data": {
    "type": "mcp_health",
    "server": "github",
    "status": "failed",
    "error": "Connection timeout"
  }
}
```

---

## Troubleshooting

### Common Issues

#### 1. Server Won't Start

**Symptoms:**
```
❌ Failed to start MCP server 'github'
Error: Command not found: npx
```

**Solutions:**
- Verify command is in PATH: `which npx`
- Install required package: `npm install -g npx`
- Use absolute path: `command: '/usr/local/bin/npx'`
- Check permissions: `chmod +x /path/to/server`

#### 2. Connection Timeout

**Symptoms:**
```
❌ MCP server 'github' connection timeout
```

**Solutions:**
- Increase timeout in configuration:
  ```yaml
  mcp:
    connectionTimeout: 30000  # 30 seconds
  ```
- Check server logs for errors
- Verify network connectivity (for remote servers)
- Try manual server start to debug

#### 3. Tools Not Appearing

**Symptoms:**
- Server starts successfully
- No tools available in `/mcp tools`

**Solutions:**
- Check server status: `/mcp status github`
- Verify server implements `tools/list`
- Check server logs for errors
- Restart server: `/mcp restart github`

#### 4. OAuth Failures

**Symptoms:**
```
❌ OAuth authentication failed
Error: Invalid client credentials
```

**Solutions:**
- Verify OAuth configuration in manifest
- Check client ID and secret
- Ensure redirect URI is correct
- Check required scopes
- Re-authenticate: `/mcp oauth login github`

#### 5. Permission Denied

**Symptoms:**
```
❌ Permission denied: filesystem access
Path: /path/to/file
```

**Solutions:**
- Check extension permissions in manifest
- Grant permission when prompted
- Verify file/directory exists
- Check file permissions: `ls -la /path/to/file`

### Debug Mode

Enable debug logging:

```yaml
mcp:
  debug: true
```

Or via environment variable:

```bash
export OLLM_LOG_LEVEL=debug
ollm
```

### Viewing Logs

```bash
# View MCP client logs
tail -f ~/.ollm/logs/mcp-client.log

# View server logs (stdio transport)
tail -f ~/.ollm/logs/mcp-server-github.log
```

### Testing Servers Manually

Test an MCP server outside of OLLM CLI:

```bash
# Start server manually
npx -y @modelcontextprotocol/server-github

# Send test request (in another terminal)
echo '{"method":"tools/list"}' | npx -y @modelcontextprotocol/server-github
```

---

## Best Practices

### 1. Configuration Management

**✅ Do:**
- Use environment variables for secrets
- Keep configuration in version control (without secrets)
- Document required environment variables
- Use workspace-specific configuration for projects

**❌ Don't:**
- Hardcode API keys in configuration
- Commit secrets to version control
- Use same configuration for all projects

### 2. Error Handling

**✅ Do:**
- Enable health monitoring
- Set appropriate timeouts
- Handle tool failures gracefully
- Log errors for debugging

**❌ Don't:**
- Ignore connection failures
- Use infinite timeouts
- Crash on tool errors

### 3. Security

**✅ Do:**
- Use OAuth for authenticated servers
- Store tokens securely (keychain)
- Revoke tokens when no longer needed
- Review extension permissions

**❌ Don't:**
- Share OAuth tokens
- Store tokens in plain text
- Grant unnecessary permissions
- Trust all extensions

### 4. Performance

**✅ Do:**
- Use stdio transport for local servers
- Enable connection pooling
- Set reasonable timeouts
- Monitor server health

**❌ Don't:**
- Use HTTP for local servers
- Create new connections for each call
- Use very long timeouts
- Ignore performance issues

### 5. Development

**✅ Do:**
- Test servers independently first
- Use debug mode during development
- Enable hot-reload for extensions
- Write comprehensive error messages

**❌ Don't:**
- Test only in production
- Ignore debug output
- Restart CLI for every change
- Use generic error messages

---

## Examples

### Example 1: GitHub Integration

**Configuration:**
```yaml
mcp:
  enabled: true
  servers:
    github:
      command: 'npx'
      args: ['-y', '@modelcontextprotocol/server-github']
      env:
        GITHUB_TOKEN: '${GITHUB_TOKEN}'
      transport: 'stdio'
```

**Usage:**
```
> List issues in my repo

Agent uses github_list_issues tool

> Create an issue about the login bug

Agent uses github_create_issue tool
```

### Example 2: Custom API Server

**Configuration:**
```yaml
mcp:
  enabled: true
  servers:
    my-api:
      command: 'https://api.mycompany.com/mcp'
      transport: 'http'
      env:
        API_KEY: '${MY_API_KEY}'
```

**Usage:**
```
> Query customer data for ID 12345

Agent uses my-api tools
```

### Example 3: Local Python Server

**Configuration:**
```yaml
mcp:
  enabled: true
  servers:
    python-tools:
      command: 'python'
      args: ['mcp-server.py']
      transport: 'stdio'
      env:
        PYTHONPATH: '${PWD}/lib'
```

**Server (mcp-server.py):**
```python
import sys
import json

def handle_request(request):
    if request['method'] == 'tools/list':
        return {
            'tools': [
                {
                    'name': 'calculate',
                    'description': 'Perform calculations',
                    'inputSchema': {
                        'type': 'object',
                        'properties': {
                            'expression': {'type': 'string'}
                        }
                    }
                }
            ]
        }
    # Handle other methods...

# Read from stdin, write to stdout
for line in sys.stdin:
    request = json.loads(line)
    response = handle_request(request)
    print(json.dumps(response))
    sys.stdout.flush()
```

---

## Next Steps

- [MCP Commands Reference](MCP_commands.md) - Complete command documentation
- [Hook System Guide](3%20projects/OLLM%20CLI/Hooks/README.md) - Learn about hooks
- [Extension Development](3%20projects/OLLM%20CLI/Extensions/README.md) - Create your own extensions
- [MCP Architecture](MCP_architecture.md) - Deep dive into architecture

---

## Support

### Getting Help

- Check [Troubleshooting](#troubleshooting) section
- Review MCP Specification (https://spec.modelcontextprotocol.io/)
- Search existing issues on GitHub
- Ask in community forums

### Reporting Issues

When reporting MCP issues, include:

1. OLLM CLI version: `ollm --version`
2. MCP server name and version
3. Configuration (redact secrets)
4. Error messages and logs
5. Steps to reproduce

---

**Document Version:** 1.0  
**Last Updated:** 2026-01-16  
**Status:** ✅ Complete
