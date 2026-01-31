# Commands Reference

**Last Updated:** January 26, 2026

Complete reference for all slash commands available in OLLM CLI. Commands provide quick access to features and can be executed by typing them in the chat input.

---

## Command Format

```
/command [subcommand] [arguments] [--flags]
```

**Examples:**

```bash
/help                    # Show help
/model list              # List models
/session save my-work    # Save session
/mode assist             # Switch to assist mode
```

---

## Session Management

### /new

Create a new session (clears current context).

**Usage:** `/new`

**Aliases:** None

**Description:** Starts a fresh session with empty context. Current conversation is not saved unless you use `/session save` first.

---

### /clear

Clear chat history (keeps context).

**Usage:** `/clear`

**Aliases:** `/cls`

**Description:** Removes all messages from the chat display while preserving the underlying context. Useful for decluttering the UI.

---

### /compact

Compress context to reduce token usage.

**Usage:** `/compact`

**Aliases:** None

**Description:** Triggers context compression to summarize older messages and free up tokens. The LLM performs the summarization.

---

### /session

Manage chat sessions.

**Usage:** `/session <subcommand> [arguments]`

**Subcommands:**

- `save <name>` - Save current session
- `list` - List saved sessions
- `resume <name>` - Resume a saved session
- `delete <name>` - Delete a saved session
- `export <name> [format]` - Export session (json, markdown, html)

**Examples:**

```bash
/session save my-work
/session list
/session resume my-work
/session export my-work markdown
```

---

## Context Management

### /context

Manage context settings and snapshots.

**Usage:** `/context <subcommand> [arguments]`

**Subcommands:**

- `size <tokens>` - Set context size
- `auto` - Enable auto-sizing
- `snapshot` - Create context snapshot
- `restore <id>` - Restore from snapshot
- `list` - List snapshots
- `stats` - Show context statistics

**Examples:**

```bash
/context size 16384
/context auto
/context snapshot
/context stats
```

---

### /snapshot

Manage context snapshots.

**Usage:** `/snapshot <subcommand> [arguments]`

**Subcommands:**

- `list` - List all snapshots
- `show <id>` - Show snapshot details
- `restore <id>` - Restore from snapshot
- `rollback` - Rollback to previous snapshot
- `create [name]` - Create named snapshot

**Examples:**

```bash
/snapshot list
/snapshot create before-refactor
/snapshot restore snap-123
/snapshot rollback
```

---

## Model Management

### /model

Manage models and model selection.

**Usage:** `/model <subcommand> [arguments]`

**Subcommands:**

- `list` - List available models
- `use <name>` - Switch to a model
- `pull <name>` - Download a model
- `delete <name>` - Delete a model
- `info <name>` - Show model information
- `keep <name>` - Keep model in memory
- `unload <name>` - Unload model from memory
- `help` - Show model help

**Examples:**

```bash
/model list
/model use llama3:8b
/model pull llama3.2:3b
/model info llama3:8b
/model keep llama3:8b
```

---

## Mode Management

### /mode

Switch operational modes.

**Usage:** `/mode <mode-name>`

**Available Modes:**

- `assist` - Assistant mode (general help)
- `plan` - Planning mode (task planning)
- `dev` - Developer mode (coding focus)
- `debug` - Debugger mode (error analysis)
- `review` - Review mode (code review)
- `architect` - Architect mode (system design)
- `test` - Test mode (testing focus)
- `docs` - Documentation mode
- `research` - Research mode
- `project` - Project mode

**Aliases:** `/m`

**Examples:**

```bash
/mode assist
/mode dev
/mode plan
```

---

### Mode Shortcuts

Quick shortcuts for common modes:

**`/assist`** (alias: `/a`)

- Switch to assistant mode
- General-purpose help and guidance

**`/plan`** (alias: `/p`)

- Switch to planning mode
- Task breakdown and planning

**`/dev`** (alias: `/d`)

- Switch to developer mode
- Coding and development focus

**`/debug`**

- Debugger mode with subcommands:
  - `trace` - Analyze stack trace
  - `reproduce` - Reproduce error
  - `bisect` - Binary search for bug

**Examples:**

```bash
/assist
/plan
/dev
/debug trace
```

---

## Provider Management

### /provider

Manage LLM providers.

**Usage:** `/provider <subcommand> [arguments]`

**Subcommands:**

- `list` - List available providers
- `use <name>` - Switch to a provider

**Examples:**

```bash
/provider list
/provider use ollama
```

---

## Configuration

### /config

View and manage configuration.

**Usage:** `/config <subcommand> [arguments]`

**Subcommands:**

- `show` - Show current configuration
- `set <key> <value>` - Set configuration value
- `get <key>` - Get configuration value
- `reset` - Reset to defaults

**Examples:**

```bash
/config show
/config set theme dark
/config get theme
```

---

## MCP (Model Context Protocol)

### /mcp

Manage MCP servers and capabilities.

**Usage:** `/mcp <subcommand> [arguments]`

**Subcommands:**

- `list [--tools|--resources|--prompts]` - List MCP servers or capabilities
- `tools <server>` - List tools from a server
- `resources <server>` - List resources from a server
- `prompts <server>` - List prompts from a server
- `status [server]` - Show server status
- `health` - Check health of all servers
- `health check <server>` - Check specific server health
- `restart <server>` - Restart a server
- `health start` - Start health monitoring
- `health stop` - Stop health monitoring
- `health status` - Show monitoring status

**Examples:**

```bash
/mcp list
/mcp list --tools
/mcp tools github
/mcp status github
/mcp health
/mcp restart github
```

---

### MCP OAuth

Manage OAuth authentication for MCP servers.

**Usage:** `/mcp oauth <subcommand> [arguments]`

**Subcommands:**

- `login <server>` - Authenticate with OAuth
- `status <server>` - Check token status
- `revoke <server>` - Revoke tokens
- `list` - List all tokens

**Examples:**

```bash
/mcp oauth login github
/mcp oauth status github
/mcp oauth revoke github
```

---

## Extensions

### /extensions

Manage extensions from the marketplace.

**Usage:** `/extensions <subcommand> [arguments]`

**Aliases:** `/ext`

**Subcommands:**

- `search <query> [--limit <n>]` - Search marketplace
- `install <name> [version]` - Install extension
- `list [--all]` - List installed extensions
- `enable <name>` - Enable extension
- `disable <name>` - Disable extension
- `info <name>` - Show extension details
- `reload` - Reload all extensions

**Examples:**

```bash
/extensions search github
/extensions install github-integration
/extensions list
/extensions enable github-integration
/extensions info github-integration
```

---

## Hooks

### /hooks

Manage automation hooks.

**Usage:** `/hooks <subcommand>`

**Subcommands:**

- `debug on` - Enable hook debugging
- `debug off` - Disable hook debugging

**Examples:**

```bash
/hooks debug on
/hooks debug off
```

---

## Git Integration

### /git

Git operations and version control.

**Usage:** `/git <subcommand> [arguments]`

**Subcommands:**

- `status` - Show git status
- `diff` - Show git diff
- `commit <message>` - Commit changes
- `log [n]` - Show commit log
- `branch` - List branches
- `checkout <branch>` - Switch branch

**Examples:**

```bash
/git status
/git diff
/git commit "Fix bug"
/git log 5
```

---

## Review & Diff

### /review

Manage diff review mode.

**Usage:** `/review <subcommand>`

**Subcommands:**

- `enable` - Enable diff review mode
- `disable` - Disable diff review mode
- `pending` - Show pending reviews

**Examples:**

```bash
/review enable
/review pending
/review disable
```

---

## Workflows

### /workflow

Manage multi-step workflows.

**Usage:** `/workflow <subcommand> [arguments]`

**Aliases:** `/wf`

**Subcommands:**

- `start <name>` - Start a workflow
- `status` - Show current workflow progress
- `next` - Move to next step
- `prev` - Go back to previous step
- `skip` - Skip current step (if optional)
- `pause` - Pause workflow
- `resume` - Resume paused workflow
- `exit` - Exit current workflow
- `list` - List available workflows

**Examples:**

```bash
/workflow start refactor
/workflow status
/workflow next
/workflow pause
```

---

## Templates

### /template

Manage prompt templates.

**Usage:** `/template <subcommand> [arguments]`

**Subcommands:**

- `list` - List available templates
- `use <name>` - Use a template
- `create <name>` - Create new template

**Examples:**

```bash
/template list
/template use code-review
/template create my-template
```

---

## Themes

### /theme

Manage UI themes.

**Usage:** `/theme <subcommand> [arguments]`

**Subcommands:**

- `list` - List available themes
- `use <name>` - Switch to a theme
- `preview <name>` - Preview a theme

**Examples:**

```bash
/theme list
/theme use dark
/theme preview monokai
```

See [Themes Guide](./Themes.md) for detailed theme documentation.

---

## Memory Management

### /memory

Manage cross-session memory.

**Usage:** `/memory <subcommand> [arguments]`

**Subcommands:**

- `list` - List stored memories
- `add <key> <value>` - Add memory
- `get <key>` - Get memory value
- `delete <key>` - Delete memory
- `clear` - Clear all memories

**Examples:**

```bash
/memory list
/memory add project-name "My Project"
/memory get project-name
```

---

## Project Profiles

### /project

Manage project profiles.

**Usage:** `/project <subcommand> [arguments]`

**Subcommands:**

- `detect` - Auto-detect project type
- `use <profile>` - Use a project profile
- `init` - Initialize project profile

**Examples:**

```bash
/project detect
/project use react
/project init
```

---

## Metrics & Performance

### /metrics

Manage performance metrics display.

**Usage:** `/metrics [subcommand]`

**Subcommands:**

- `toggle` - Toggle metrics display
- `reset` - Reset metrics
- (no args) - Show current metrics

**Examples:**

```bash
/metrics
/metrics toggle
/metrics reset
```

---

## Reasoning Display

### /reasoning

Manage reasoning display for models that support it.

**Usage:** `/reasoning <subcommand>`

**Subcommands:**

- `toggle` - Toggle reasoning display
- `expand` - Expand reasoning
- `collapse` - Collapse reasoning

**Examples:**

```bash
/reasoning toggle
/reasoning expand
```

---

## Comparison Mode

### /compare

Compare outputs from multiple models.

**Usage:** `/compare <model1> <model2> [model3...]`

**Description:** Sends the same prompt to multiple models and displays their responses side-by-side for comparison.

**Examples:**

```bash
/compare llama3:8b llama3:13b
/compare llama3:8b mistral:7b codellama:7b
```

---

## Utility Commands

### /help

Show help information.

**Usage:** `/help [command]`

**Aliases:** `/?`

**Description:** Displays general help or detailed help for a specific command.

**Examples:**

```bash
/help
/help model
/help session
```

---

### /exit

Exit OLLM CLI.

**Usage:** `/exit`

**Aliases:** `/quit`, `/q`

**Description:** Closes the application. Unsaved sessions will be lost unless auto-save is enabled.

---

### /home

Return to the launch screen.

**Usage:** `/home`

**Description:** Returns to the main menu or launch screen.

---

### /test prompt

Dump current prompt/context details.

**Usage:** `/test prompt`

**Description:** Displays detailed information about the current prompt and context as a system message. Useful for debugging.

---

## Command Aliases Quick Reference

| Command       | Aliases       |
| ------------- | ------------- |
| `/help`       | `/?`          |
| `/exit`       | `/quit`, `/q` |
| `/clear`      | `/cls`        |
| `/mode`       | `/m`          |
| `/assist`     | `/a`          |
| `/plan`       | `/p`          |
| `/dev`        | `/d`          |
| `/workflow`   | `/wf`         |
| `/extensions` | `/ext`        |

---

## Tips

### Command Discovery

- Use `/help` to see all available commands
- Use `/help <command>` for detailed command help
- Commands support tab completion (when implemented)

### Command Shortcuts

- Learn common aliases for faster typing
- Use mode shortcuts (`/assist`, `/dev`, `/plan`) instead of `/mode`
- Abbreviate subcommands when unambiguous

### Command History

- Use `Up` arrow to recall previous commands
- Commands are saved in shell history
- Edit and re-execute previous commands

---

## Related Documentation

- [UI Guide](./UIGuide.md) - Main interface documentation
- [Keyboard Shortcuts](./keybinds.md) - Keybind reference
- [Configuration](./configuration.md) - Configuration options
- [MCP Integration](../MCP/MCP_Index.md) - MCP system documentation
- [Hooks System](../Hooks/UserGuide.md) - Hooks documentation

---

**Last Updated:** January 26, 2026

<!-- Alpha Release: v0.1.6 -->

