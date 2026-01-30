# Getting Started with MCP

**Quick Start Guide for Model Context Protocol in OLLM CLI**

This guide will help you get started with MCP (Model Context Protocol), hooks, and extensions in OLLM CLI.

---

## 📋 Table of Contents

1. [What is MCP?](#what-is-mcp)
2. [Prerequisites](#prerequisites)
3. [Quick Start](#quick-start)
4. [Using MCP Servers](#using-mcp-servers)
5. [Using Hooks](#using-hooks)
6. [Using Tools
7. [Next Steps](#next-steps)

**See Also:**

- [Complete Documentation Index](MCP_index.md) - All documentation with summaries
- [MCP Commands Reference](MCP_commands.md) - All available commands
- [MCP Architecture](MCP_architecture.md) - System design details

---

## What is MCP?

**Model Context Protocol (MCP)** is an open protocol that standardizes how applications provide context to LLMs. In OLLM CLI, MCP enables three powerful features:

### 1. MCP Servers

External tools and services that your LLM can use:

- File operations
- Database queries
- API integrations
- Web searches
- Custom tools

### 2. Hooks

Event-driven automation:

- Run commands before/after execution
- Automate workflows
- Add safety gates
- Custom event handling

### 3. Extensions

Modular functionality packages:

- Skills (prompt templates)
- Settings (configuration)
- MCP servers (tools)
- Hooks (automation)

---

## Prerequisites

### Required

- OLLM CLI installed and configured
- Node.js 20+ (for some MCP servers)
- Basic command-line knowledge

### Optional

- Git (for extension installation)
- Python 3.8+ (for Python-based MCP servers)
- Docker (for containerized MCP servers)

### Verify Installation

```bash
# Check OLLM CLI version
ollm --version

# Check if MCP is available
ollm /help
# Look for /mcp, /hooks, /extensions commands
```

---

## Quick Start

### 1. Check MCP Status

```bash
# Start OLLM CLI
ollm

# Check available MCP commands
/help

# List MCP servers (if any configured)
/mcp list

# Check MCP health
/mcp health
```

### 2. Your First Hook

Hooks automate tasks based on events. Let's create a simple hook:

```bash
# Create a hook that runs before each execution
/hooks create pre-execution "echo 'Starting execution...'"

# List your hooks
/hooks list

# Test it by running a prompt
Hello, world!
```

### 3. Your First Extension

Extensions add functionality. Let's install one:

```bash
# Search for extensions
/extensions search github

# Install an extension (example)
/extensions install https://github.com/ollm/ext-github/releases/latest/download/github.tar.gz

# List installed extensions
/extensions list

# Enable the extension
/extensions enable github
```

---

## Using MCP Servers

MCP servers provide tools that your LLM can use. Here's how to work with them:

### Listing Servers

```bash
# List all configured servers
/mcp list

# Check server health
/mcp health

# Check specific server
/mcp health check <server-name>
```

### Starting/Stopping Servers

```bash
# Start a server
/mcp start <server-name>

# Stop a server
/mcp stop <server-name>

# Restart a server
/mcp restart <server-name>
```

### Using Server Tools

Once a server is running, its tools are automatically available to the LLM:

```bash
# Example: Using a file server
Can you read the contents of README.md?

# Example: Using a GitHub server
Show me the latest issues in the ollm/ollm-cli repository

# Example: Using a database server
Query the users table and show me the first 10 rows
```

### Configuring Servers

MCP servers are configured in your OLLM config file:

**Location:** `~/.ollm/config.yaml` or `.ollm/config.yaml`

**Example:**

```yaml
mcp:
  servers:
    filesystem:
      command: 'npx'
      args: ['-y', '@modelcontextprotocol/server-filesystem', '/path/to/workspace']
      transport: 'stdio'

    github:
      command: 'npx'
      args: ['-y', '@modelcontextprotocol/server-github']
      transport: 'stdio'
      oauth:
        provider: 'github'
        clientId: 'your-client-id'
        scopes: ['repo', 'user']
```

**See:** [MCP Integration Guide](MCP_integration.md) for detailed configuration

---

## Using Hooks

Hooks automate workflows based on events. Here's how to use them:

### Hook Events

OLLM CLI supports 12 hook events:

| Event                 | When it fires         |
| --------------------- | --------------------- |
| `pre-execution`       | Before LLM execution  |
| `post-execution`      | After LLM execution   |
| `pre-tool-call`       | Before tool execution |
| `post-tool-call`      | After tool execution  |
| `on-error`            | When an error occurs  |
| `on-file-change`      | When a file changes   |
| `on-git-commit`       | Before git commit     |
| `on-session-start`    | When session starts   |
| `on-session-end`      | When session ends     |
| `on-context-overflow` | When context is full  |
| `on-approval-request` | When approval needed  |
| `custom`              | Custom events         |

### Creating Hooks

```bash
# Create a simple hook
/hooks create pre-execution "echo 'Starting...'"

# Create a hook with a script
/hooks create on-file-change "./scripts/format.sh {file}"

# Create a hook from a file
/hooks create pre-tool-call ./hooks/safety-check.js
```

### Managing Hooks

```bash
# List all hooks
/hooks list

# Enable a hook
/hooks enable <hook-name>

# Disable a hook
/hooks disable <hook-name>

# Remove a hook
/hooks remove <hook-name>

# Debug hooks
/hooks debug on
```

### Hook Trust Levels

Hooks have three trust levels:

1. **Trusted** - System hooks, always run
2. **Workspace** - Project hooks, require approval
3. **Downloaded** - Extension hooks, require approval

```bash
# Trust a workspace hook
/hooks trust <hook-name>

# Untrust a hook
/hooks untrust <hook-name>
```

### Example Hooks

**Format code on save:**

```bash
/hooks create on-file-change "prettier --write {file}"
```

**Run tests before commit:**

```bash
/hooks create on-git-commit "npm test"
```

**Safety check before tool execution:**

```bash
/hooks create pre-tool-call "./scripts/safety-check.sh {tool} {args}"
```

**See:** [Hooks User Guide](3%20projects/OLLM%20CLI/Hooks/user-guide.md) for more examples

---

## Using Extensions

Extensions package functionality for easy distribution and installation.

### Searching Extensions

```bash
# Search for extensions
/extensions search <keyword>

# Example: Search for GitHub extensions
/extensions search github

# Example: Search for database extensions
/extensions search database
```

### Installing Extensions

```bash
# Install from URL
/extensions install https://example.com/extension.tar.gz

# Install from file
/extensions install ./my-extension.tar.gz

# Install from registry (when available)
/extensions install github-integration
```

### Managing Extensions

```bash
# List installed extensions
/extensions list

# Show extension info
/extensions info <extension-name>

# Enable an extension
/extensions enable <extension-name>

# Disable an extension
/extensions disable <extension-name>

# Reload extensions (after changes)
/extensions reload
```

### Extension Components

Extensions can include:

1. **Skills** - Prompt templates
2. **Settings** - Configuration
3. **Servers** - MCP servers
4. **Hooks** - Automation

**Example:** Installing a GitHub extension might add:

- GitHub MCP server (tools for GitHub API)
- GitHub hooks (auto-commit, PR creation)
- GitHub skills (PR review templates)
- GitHub settings (API token, default repo)

### Creating Extensions

To create your own extension:

1. Create a directory with `manifest.json`
2. Add components (skills, servers, hooks)
3. Package as `.tar.gz`
4. Install locally or publish

**See:** [Extension Development Guide](3%20projects/OLLM%20CLI/Extensions/development-guide.md)

---

## Next Steps

### Learn More

**For Users:**

- [MCP Commands Reference](MCP_commands.md) - All available commands
- [Hooks User Guide](3%20projects/OLLM%20CLI/Hooks/user-guide.md) - Advanced hook usage
- [Extensions User Guide](3%20projects/OLLM%20CLI/Extensions/user-guide.md) - Extension management

**For Developers:**

- [MCP Architecture](MCP_architecture.md) - System design
- [Creating Hooks](3%20projects/OLLM%20CLI/Hooks/development-guide.md) - Build custom hooks
- [Creating Extensions](3%20projects/OLLM%20CLI/Extensions/development-guide.md) - Build extensions
- [Creating MCP Servers](3%20projects/OLLM%20CLI/MCP/servers/development-guide.md) - Build servers

**For Administrators:**

- [OAuth Setup](oauth-setup.md) - Configure OAuth
- [Health Monitoring](health-monitoring.md) - Monitor servers
- [Extension Marketplace](marketplace.md) - Manage marketplace

### Common Workflows

**Development Workflow:**

```bash
# 1. Install development extensions
/extensions install dev-tools

# 2. Set up hooks for code quality
/hooks create on-file-change "eslint --fix {file}"
/hooks create pre-git-commit "npm test"

# 3. Configure MCP servers
# Edit ~/.ollm/config.yaml to add filesystem, git servers

# 4. Start coding with AI assistance
```

**Data Analysis Workflow:**

```bash
# 1. Install database extensions
/extensions install postgres-tools

# 2. Configure database MCP server
# Edit config to add database connection

# 3. Set up hooks for data validation
/hooks create pre-tool-call "./validate-query.sh {args}"

# 4. Start querying with AI
```

**Content Creation Workflow:**

```bash
# 1. Install content extensions
/extensions install markdown-tools

# 2. Set up hooks for formatting
/hooks create on-file-change "prettier --write {file}"

# 3. Configure web search MCP server
# Edit config to add search API

# 4. Start writing with AI assistance
```

### Troubleshooting

**MCP server not working:**

```bash
# Check server status
/mcp health check <server>

# Check logs
/mcp logs <server>

# Restart server
/mcp restart <server>
```

**Hook not executing:**

```bash
# Enable debug mode
/hooks debug on

# Check hook status
/hooks list

# Trust the hook if needed
/hooks trust <hook-name>
```

**Extension not loading:**

```bash
# Check extension status
/extensions info <extension>

# Enable the extension
/extensions enable <extension>

# Reload extensions
/extensions reload
```

**See:** [Troubleshooting Guide](../Troubleshooting.md) for more help

---

## Examples

### Example 1: GitHub Integration

```bash
# 1. Install GitHub extension
/extensions install github-integration

# 2. Configure OAuth
/mcp oauth login github

# 3. Use GitHub tools
Can you show me the latest issues in my repository?
Create a PR for the feature branch
```

### Example 2: Automated Testing

```bash
# 1. Create pre-commit hook
/hooks create pre-git-commit "npm test"

# 2. Create post-execution hook
/hooks create post-execution "./scripts/save-session.sh"

# 3. Trust the hooks
/hooks trust pre-git-commit
/hooks trust post-execution
```

### Example 3: Custom Workflow

```bash
# 1. Create custom extension
mkdir my-workflow
cd my-workflow
# Create manifest.json, add hooks and skills

# 2. Package extension
tar -czf my-workflow.tar.gz .

# 3. Install extension
/extensions install ./my-workflow.tar.gz

# 4. Enable extension
/extensions enable my-workflow
```

---

## Resources

### Documentation

- [MCP Architecture](MCP_architecture.md) - System design
- [MCP Integration](MCP_integration.md) - Integration guide
- [MCP Commands](MCP_commands.md) - Command reference

### Community

- GitHub: ollm/ollm-cli (https://github.com/ollm/ollm-cli)
- Discord: [Join our server](#)
- Forum: [Community forum](#)

### External Resources

- MCP Specification (https://modelcontextprotocol.io)
- MCP Servers (https://github.com/modelcontextprotocol/servers)
- Extension Examples (https://github.com/ollm/extensions)

---

## Support

Need help? Here's how to get support:

1. **Check Documentation** - Most questions are answered here
2. **Search Issues** - Someone may have had the same problem
3. **Ask Community** - Discord and forum are active
4. **Report Bugs** - GitHub issues for bugs and feature requests

---

**Last Updated:** 2026-01-16  
**Version:** 0.1.0  
**Next:** [MCP Commands Reference](MCP_commands.md)
