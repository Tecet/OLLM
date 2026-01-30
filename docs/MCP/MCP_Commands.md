# MCP Commands Reference

**Complete CLI Command Reference for MCP Integration**

---

## Table of Contents

1. [Overview](#overview)
2. [MCP Server Commands](#mcp-server-commands)
3. [MCP Health Commands](#mcp-health-commands)
4. [MCP OAuth Commands](#mcp-oauth-commands)
5. [Hook Commands](#hook-commands)
6. [Extension Commands](#extension-commands)
7. [Command Examples](#command-examples)

---

## Overview

OLLM CLI provides comprehensive command-line interface for managing MCP servers, hooks, and extensions. All commands are accessible via the `/` prefix in the CLI.

### Command Categories

- **MCP Server**: Manage MCP server lifecycle
- **MCP Health**: Monitor and maintain server health
- **MCP OAuth**: Manage OAuth authentication
- **Hooks**: Debug and manage hooks
- **Extensions**: Install and manage extensions

---

## MCP Server Commands

### `/mcp`

List all MCP servers and their status.

**Usage:**

```bash
/mcp
```

**Output:**

```
3 MCP server(s):

[✓] github (connected)
    Tools: 12

[✗] slack (error)
    Error: Connection timeout

[○] notion (disconnected)
```

**Status Icons:**

- `✓` - Connected and healthy
- `✗` - Error or failed
- `○` - Disconnected or not started

---

### `/mcp tools <server>`

List tools available from a specific MCP server.

**Usage:**

```bash
/mcp tools github
```

**Output:**

```
12 tool(s) from 'github':

1. github_create_issue
   Create a new issue in a repository
   Input schema: {
     "repo": "string (required)",
     "title": "string (required)",
     "body": "string (optional)"
   }

2. github_list_issues
   List issues in a repository
   Input schema: {
     "repo": "string (required)",
     "state": "string (optional, default: open)"
   }

...
```

---

### `/mcp resources <server>`

List resources available from a specific MCP server.

**Usage:**

```bash
/mcp resources github
```

**Output:**

```
5 resource(s) from 'github':

1. repository
   URI: github://repo/{owner}/{repo}
   Access repository information

2. issue
   URI: github://issue/{owner}/{repo}/{number}
   Access issue details

...
```

---

### `/mcp prompts <server>`

List prompts available from a specific MCP server.

**Usage:**

```bash
/mcp prompts github
```

**Output:**

```
3 prompt(s) from 'github':

1. create-pr
   Create a pull request
   Arguments:
     - title (required): PR title
     - body (optional): PR description

2. review-code
   Review code changes
   Arguments:
     - repo (required): Repository name
     - pr (required): PR number

...
```

---

### `/mcp status [server]`

Show detailed status of MCP servers.

**Usage:**

```bash
# All servers
/mcp status

# Specific server
/mcp status github
```

**Output (all servers):**

```
3 MCP server(s):

[✓] github: connected (12 tools)
[✗] slack: error (0 tools)
[○] notion: disconnected (0 tools)
```

**Output (specific server):**

```
Server: github
Status: connected
Tools: 12
Resources: 5
Prompts: 3
Uptime: 2h 34m
Last Check: 2 seconds ago
```

---

## MCP Health Commands

### `/mcp health`

Check health status of all MCP servers.

**Usage:**

```bash
/mcp health
```

**Output:**

```
**MCP Server Health Status**

✅ **github** - connected
   Tools: 12

❌ **slack** - error
   Error: Connection timeout

⚠️ **notion** - degraded
   Tools: 8 (slow response)
```

---

### `/mcp health check <server>`

Check health of a specific MCP server.

**Usage:**

```bash
/mcp health check github
```

**Output:**

```
**github** Health Check

✅ **Status:** connected
🕐 **Checked:** 2:30:45 PM
⚡ **Response Time:** 45ms
🔧 **Tools:** 12

✅ Server is healthy and responding normally.
```

---

### `/mcp restart <server>`

Manually restart an MCP server.

**Usage:**

```bash
/mcp restart slack
```

**Output:**

```
🔄 Restarting server "slack"...
✅ Successfully restarted server "slack"
```

**Notes:**

- Stops the server gracefully
- Starts a new server process
- Re-discovers tools
- Updates tool registry

---

### `/mcp health start`

Start automatic health monitoring.

**Usage:**

```bash
/mcp health start
```

**Output:**

```
✅ Health monitoring started

MCP servers will be checked periodically.
Failed servers will be automatically restarted.
```

**Configuration:**

- Check interval: 30 seconds (configurable)
- Max restart attempts: 3
- Exponential backoff: 1s, 2s, 4s

---

### `/mcp health stop`

Stop automatic health monitoring.

**Usage:**

```bash
/mcp health stop
```

**Output:**

```
⏸️ Health monitoring stopped
```

---

### `/mcp health status`

Show health monitoring status.

**Usage:**

```bash
/mcp health status
```

**Output:**

```
**Health Monitoring Status**

📊 **Status:** ✅ Running
🖥️ **Servers:** 3
⏱️ **Check Interval:** 30 seconds
🔄 **Auto-Restart:** Enabled
📈 **Uptime:** 2h 34m

✅ Automatic health checks are active.
Failed servers will be automatically restarted.
```

---

### `/mcp health help`

Show help for health monitoring commands.

**Usage:**

```bash
/mcp health help
```

**Output:**

````
**MCP Health Monitoring**

Automatically monitors MCP server health and restarts failed servers.

**Available Commands:**

`/mcp health` - Check health of all servers
`/mcp health check <server>` - Check specific server
`/mcp restart <server>` - Manually restart a server
`/mcp health start` - Start automatic monitoring
`/mcp health stop` - Stop automatic monitoring
`/mcp health status` - Show monitoring status

**Features:**

- Periodic health checks (every 30 seconds)
- Automatic restart on failure (up to 3 attempts)
- Exponential backoff between restart attempts
- Event notifications for health changes

**Configuration:**

Health monitoring can be configured in your config file:

```yaml
mcpHealth:
  enabled: true
  checkInterval: 30000  # 30 seconds
  maxRestartAttempts: 3
  autoRestart: true
````

````

---

## MCP OAuth Commands

### `/mcp oauth login <server>`

Start OAuth authentication flow for an MCP server.

**Usage:**
```bash
/mcp oauth login github
````

**Output:**

```
🔐 Starting OAuth flow for "github"...

Opening browser for authorization...
URL: https://github.com/login/oauth/authorize?client_id=...

Waiting for authorization...
✅ Authorization successful!
🔑 Token stored securely.

Server "github" is now authenticated.
```

**Process:**

1. Opens browser to authorization page
2. User authorizes the application
3. Receives authorization code
4. Exchanges code for access token
5. Stores token securely (keychain or encrypted file)

---

### `/mcp oauth status <server>`

Check OAuth authentication status for an MCP server.

**Usage:**

```bash
/mcp oauth status github
```

**Output (authenticated):**

```
**OAuth Status: github**

✅ **Authenticated**
🔑 **Token:** Valid
⏰ **Expires:** 2026-02-15 14:30:00 (29 days)
🔄 **Auto-Refresh:** Enabled
📋 **Scopes:** repo, user, read:org

Token will be automatically refreshed before expiration.
```

**Output (not authenticated):**

```
**OAuth Status: github**

❌ **Not Authenticated**

Use `/mcp oauth login github` to authenticate.
```

---

### `/mcp oauth revoke <server>`

Revoke OAuth token for an MCP server.

**Usage:**

```bash
/mcp oauth revoke github
```

**Output:**

```
🔐 Revoking OAuth token for "github"...

✅ Token revoked successfully.
🗑️ Token removed from storage.

Server "github" is no longer authenticated.
Use `/mcp oauth login github` to re-authenticate.
```

**Actions:**

- Revokes token with OAuth provider (if supported)
- Removes token from local storage
- Requires re-authentication for future use

---

### `/mcp oauth list`

List OAuth authentication status for all servers.

**Usage:**

```bash
/mcp oauth list
```

**Output:**

```
**OAuth Authentication Status**

✅ **github** - Authenticated
   Expires: 2026-02-15 (29 days)
   Scopes: repo, user

❌ **slack** - Not Authenticated
   Use `/mcp oauth login slack`

✅ **notion** - Authenticated
   Expires: 2026-03-01 (44 days)
   Scopes: read, write
```

---

### `/mcp oauth help`

Show help for OAuth commands.

**Usage:**

```bash
/mcp oauth help
```

**Output:**

```
**MCP OAuth Authentication**

Manage OAuth 2.0 authentication for MCP servers.

**Available Commands:**

`/mcp oauth login <server>` - Start OAuth flow
`/mcp oauth status <server>` - Check authentication status
`/mcp oauth revoke <server>` - Revoke OAuth token
`/mcp oauth list` - List all OAuth statuses

**Features:**

- OAuth 2.0 with PKCE flow
- Secure token storage (keychain + encrypted file)
- Automatic token refresh before expiration
- Browser-based authorization
- Token revocation support

**Token Storage:**

Tokens are stored securely:
- Primary: Platform keychain (Credential Manager, Keychain, Secret Service)
- Fallback: Encrypted file (~/.ollm/oauth-tokens.json)
- Encryption: AES-256-GCM
- Permissions: User-only access
```

---

## Hook Commands

### `/hooks debug on`

Enable hook debugging mode.

**Usage:**

```bash
/hooks debug on
```

**Output:**

```
✅ Hook debugging enabled

Hook execution will be traced and logged.
Use `/hooks debug status` to view traces.
```

---

### `/hooks debug off`

Disable hook debugging mode.

**Usage:**

```bash
/hooks debug off
```

**Output:**

```
⏸️ Hook debugging disabled
```

---

### `/hooks debug status`

Show hook debugging status and recent traces.

**Usage:**

```bash
/hooks debug status
```

**Output:**

```
**Hook Debugging Status**

📊 **Status:** ✅ Enabled
📝 **Traces:** 15
⏱️ **Since:** 2026-01-16 14:30:00

**Recent Traces:**

1. validate-input (before_model)
   ✅ Success - 45ms
   Source: workspace

2. log-request (before_agent)
   ✅ Success - 12ms
   Source: user

3. check-permissions (before_tool)
   ❌ Failed - 230ms
   Error: Permission denied
   Source: downloaded
```

---

### `/hooks debug clear`

Clear all hook debug traces.

**Usage:**

```bash
/hooks debug clear
```

**Output:**

```
🗑️ Cleared 15 debug traces
```

---

### `/hooks debug export [format]`

Export hook debug traces.

**Usage:**

```bash
# Export as JSON
/hooks debug export json

# Export as pretty text
/hooks debug export pretty

# Export as compact text
/hooks debug export compact
```

**Output:**

```
📄 Exported 15 traces to: ~/.ollm/hook-traces-2026-01-16.json
```

---

### `/hooks debug summary`

Show summary statistics of hook execution.

**Usage:**

```bash
/hooks debug summary
```

**Output:**

```
**Hook Execution Summary**

📊 **Total Executions:** 150
✅ **Successful:** 142 (94.7%)
❌ **Failed:** 8 (5.3%)
⏱️ **Average Duration:** 67ms

**By Hook:**
- validate-input: 45 executions, 100% success, 45ms avg
- log-request: 50 executions, 100% success, 12ms avg
- check-permissions: 30 executions, 80% success, 230ms avg

**By Event:**
- before_model: 50 executions, 96% success
- before_agent: 50 executions, 100% success
- before_tool: 30 executions, 80% success
```

---

### `/hooks debug failed`

Show only failed hook executions.

**Usage:**

```bash
/hooks debug failed
```

**Output:**

```
**Failed Hook Executions (8)**

1. check-permissions (before_tool)
   ❌ Failed - 230ms
   Error: Permission denied
   Source: downloaded
   Time: 2026-01-16 14:35:12

2. validate-schema (before_model)
   ❌ Failed - 120ms
   Error: Invalid schema
   Source: workspace
   Time: 2026-01-16 14:32:45

...
```

---

### `/hooks list`

List all registered hooks.

**Usage:**

```bash
/hooks list
```

**Output:**

```
**Registered Hooks**

**before_model (3 hooks):**
1. validate-input (workspace)
2. log-request (user)
3. check-schema (downloaded)

**before_agent (2 hooks):**
1. init-session (user)
2. load-context (workspace)

**before_tool (1 hook):**
1. check-permissions (downloaded)
```

---

## Extension Commands

### `/extensions search <query>`

Search for extensions in the marketplace.

**Usage:**

```bash
/extensions search github
```

**Output:**

```
**Extension Search Results**

Found 5 extensions matching "github":

1. **github-integration** (v1.2.0) ⭐ 4.8
   GitHub integration with MCP server
   Author: ollm-community
   Downloads: 1.2k

2. **github-actions** (v0.9.0) ⭐ 4.5
   GitHub Actions workflow management
   Author: actions-team
   Downloads: 850

...

Use `/extensions install <name>` to install.
```

---

### `/extensions install <name>`

Install an extension from the marketplace.

**Usage:**

```bash
/extensions install github-integration
```

**Output:**

```
📦 Installing extension "github-integration"...

⬇️ Downloading from registry...
✅ Downloaded (2.3 MB)
🔐 Verifying checksum...
✅ Checksum verified
📂 Extracting to ~/.ollm/extensions/...
✅ Installed successfully

Extension "github-integration" is now available.
Use `/extensions enable github-integration` to activate.
```

---

### `/extensions list`

List all installed extensions.

**Usage:**

```bash
/extensions list
```

**Output:**

```
**Installed Extensions**

✅ **github-integration** (v1.2.0) - Enabled
   GitHub integration with MCP server
   Location: ~/.ollm/extensions/github-integration

⏸️ **slack-bot** (v0.5.0) - Disabled
   Slack bot integration
   Location: ~/.ollm/extensions/slack-bot

✅ **custom-tools** (v1.0.0) - Enabled
   Custom development tools
   Location: .ollm/extensions/custom-tools
```

---

### `/extensions enable <name>`

Enable an installed extension.

**Usage:**

```bash
/extensions enable slack-bot
```

**Output:**

```
✅ Enabling extension "slack-bot"...

🔧 Registering hooks...
🚀 Starting MCP servers...
🔧 Registering tools...

Extension "slack-bot" is now enabled.
```

---

### `/extensions disable <name>`

Disable an enabled extension.

**Usage:**

```bash
/extensions disable slack-bot
```

**Output:**

```
⏸️ Disabling extension "slack-bot"...

🔧 Unregistering hooks...
🛑 Stopping MCP servers...
🔧 Removing tools...

Extension "slack-bot" is now disabled.
```

---

### `/extensions info <name>`

Show detailed information about an extension.

**Usage:**

```bash
/extensions info github-integration
```

**Output:**

```
**Extension: github-integration**

📦 **Version:** 1.2.0
👤 **Author:** ollm-community
📝 **Description:** GitHub integration with MCP server
⭐ **Rating:** 4.8/5.0
⬇️ **Downloads:** 1,200
📅 **Updated:** 2026-01-10

**Status:** ✅ Enabled

**Provides:**
- 1 MCP server (github)
- 12 tools
- 3 hooks
- 2 skills

**Permissions:**
- Network: api.github.com
- Environment: GITHUB_TOKEN

**Location:** ~/.ollm/extensions/github-integration
```

---

### `/extensions reload <name>`

Reload an extension (hot-reload).

**Usage:**

```bash
/extensions reload custom-tools
```

**Output:**

```
🔄 Reloading extension "custom-tools"...

⏸️ Disabling...
✅ Disabled
🔄 Loading...
✅ Loaded
✅ Enabling...

Extension "custom-tools" reloaded successfully.
```

---

## Command Examples

### Example 1: Setting Up GitHub Integration

```bash
# 1. Check if GitHub server is configured
/mcp status github

# 2. If not configured, add to config.yaml
# (Edit ~/.ollm/config.yaml)

# 3. Restart OLLM CLI
# (Exit and restart)

# 4. Check server status
/mcp status github

# 5. Start OAuth authentication
/mcp oauth login github

# 6. Verify authentication
/mcp oauth status github

# 7. List available tools
/mcp tools github

# 8. Enable health monitoring
/mcp health start

# 9. Use GitHub tools in conversation
> Create an issue in my repo about the login bug
```

### Example 2: Debugging Hook Issues

```bash
# 1. Enable hook debugging
/hooks debug on

# 2. Trigger the problematic hook
> Test the validation hook

# 3. Check debug status
/hooks debug status

# 4. View failed executions
/hooks debug failed

# 5. Export traces for analysis
/hooks debug export json

# 6. Fix the hook and reload
/extensions reload my-extension

# 7. Test again
> Test the validation hook

# 8. Verify success
/hooks debug summary
```

### Example 3: Installing and Managing Extensions

```bash
# 1. Search for extensions
/extensions search github

# 2. Install an extension
/extensions install github-integration

# 3. View extension info
/extensions info github-integration

# 4. Enable the extension
/extensions enable github-integration

# 5. Verify it's working
/mcp status github

# 6. List all extensions
/extensions list

# 7. Disable if needed
/extensions disable github-integration
```

---

## Command Aliases

Many commands have shorter aliases:

| Full Command         | Alias                 |
| -------------------- | --------------------- |
| `/extensions`        | `/ext`                |
| `/mcp health check`  | `/mcp check`          |
| `/mcp health start`  | `/mcp monitor start`  |
| `/mcp health stop`   | `/mcp monitor stop`   |
| `/mcp health status` | `/mcp monitor status` |

---

## Next Steps

- [MCP Integration Guide](MCP_integration.md) - Learn how to integrate MCP servers
- [MCP Architecture](MCP_architecture.md) - Understand the system architecture
- [Hook System Guide](3%20projects/OLLM%20CLI/Hooks/README.md) - Learn about hooks
- [Extension Development](3%20projects/OLLM%20CLI/Extensions/README.md) - Create your own extensions

---

**Document Version:** 1.0  
**Last Updated:** 2026-01-16  
**Status:** ✅ Complete
