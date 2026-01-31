# Hook System Documentation

**Last Updated:** January 26, 2026

Welcome to the Hook System documentation for OLLM CLI. This section covers the complete event-driven automation system, including hook types, trust model, protocol, and visual guides.

---

## 📚 Documentation Overview

### Core Documentation

- **[Architecture](Architecture.md)** - Complete system architecture and design
- **[User Guide](UserGuide.md)** - Using hooks for automation
- **[Protocol](Protocol.md)** - Hook protocol specification
- **[Visual Guide](VisualGuide.md)** - Visual diagrams and examples
- **[Keyboard Shortcuts](KeyboardShortcuts.md)** - Hook-related keybindings

---

## 🎯 What are Hooks?

The **Hook System** enables event-driven automation and safety gates in OLLM CLI. Hooks are scripts that run automatically when specific events occur:

### 1. **Event-Driven Automation**

Hooks respond to IDE and workflow events:

- File operations (create, edit, delete, save)
- Agent lifecycle (prompt submit, agent stop)
- User triggers (manual execution)
- Git operations (commit, push, pull)

### 2. **Safety Gates**

Hooks can prevent dangerous operations:

- Block dangerous shell commands
- Validate file modifications
- Enforce code quality standards
- Require approval for sensitive operations

### 3. **Workflow Automation**

Hooks automate repetitive tasks:

- Auto-format code on save
- Run tests before commit
- Generate documentation
- Update dependencies

### 4. **Trust Model**

Three-tier trust system for security:

- **Trusted** - System hooks, always allowed
- **Workspace** - Project hooks, require approval
- **Downloaded** - External hooks, require explicit trust

---

## 📖 Documentation Structure

```
docs/Hooks/
├── README.md                    ← You are here
├── Index.md                     Quick reference with links
├── Architecture.md              System architecture
├── UserGuide.md                 Using hooks
├── Protocol.md                  Hook protocol spec
├── VisualGuide.md               Visual diagrams
└── KeyboardShortcuts.md         Hook keybindings
```

---

## 🎓 Key Concepts

### Hook Events

Hooks can respond to various events:

**File Events:**

- `fileEdited` - File saved/modified
- `fileCreated` - New file created
- `fileDeleted` - File deleted

**Agent Events:**

- `promptSubmit` - User sends message
- `agentStop` - Agent completes execution

**User Events:**

- `userTriggered` - Manual hook execution

**See:** [User Guide](UserGuide.md#hook-events)

### Hook Actions

Hooks can perform two types of actions:

**askAgent:**

- Send prompt to agent
- Agent processes and responds
- Valid with all event types

**runCommand:**

- Execute shell command
- Only valid with `promptSubmit` and `agentStop`
- Not valid with file events

**See:** [Protocol](Protocol.md#hook-actions)

### Trust Model

Three trust levels for security:

| Level          | Description    | Approval Required   |
| -------------- | -------------- | ------------------- |
| **Trusted**    | System hooks   | ❌ No               |
| **Workspace**  | Project hooks  | ✅ Yes (first time) |
| **Downloaded** | External hooks | ✅ Yes (explicit)   |

**See:** [Architecture](Architecture.md#trust-model)

### Hook Protocol

Hooks communicate via JSON stdin/stdout:

**Input:**

```json
{
  "event": "fileEdited",
  "file": "src/main.ts",
  "content": "...",
  "timestamp": "2026-01-26T10:00:00Z"
}
```

**Output:**

```json
{
  "action": "askAgent",
  "prompt": "Review the changes in src/main.ts",
  "metadata": {}
}
```

**See:** [Protocol](Protocol.md)

---

## 💡 Common Use Cases

### Auto-Format on Save

```json
{
  "name": "Auto-format on save",
  "version": "1.0.0",
  "when": {
    "type": "fileEdited",
    "patterns": ["*.ts", "*.tsx"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Run prettier on {{file}}"
  }
}
```

### Run Tests Before Commit

```json
{
  "name": "Test before commit",
  "version": "1.0.0",
  "when": {
    "type": "promptSubmit"
  },
  "then": {
    "type": "runCommand",
    "command": "npm test"
  }
}
```

### Block Dangerous Commands

```json
{
  "name": "Block rm -rf",
  "version": "1.0.0",
  "when": {
    "type": "promptSubmit"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Check if command contains 'rm -rf' and warn user"
  }
}
```

**Learn more:** [User Guide](UserGuide.md#examples)

---

## 🛠️ Configuration

### Hook Settings

```yaml
hooks:
  # Enable hook system
  enabled: true

  # Hook directories
  directories:
    - .ollm/hooks/ # Workspace hooks
    - ~/.ollm/hooks/ # User hooks

  # Trust settings
  trustWorkspaceHooks: false # Require approval
  trustDownloadedHooks: false # Require explicit trust
```

### Hook File Format

```json
{
  "name": "Hook Name",
  "version": "1.0.0",
  "description": "What this hook does",
  "when": {
    "type": "fileEdited",
    "patterns": ["*.ts"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Review changes"
  }
}
```

**Learn more:** [Protocol](Protocol.md#hook-format)

---

## 🔍 Troubleshooting

### Common Issues

**Hook not triggering:**

- Check hook enabled: `/hooks list`
- Verify event type matches
- Check file patterns match
- Enable hook: `/hooks enable <name>`

**Hook fails to execute:**

- Check hook syntax: `/hooks debug <name>`
- Verify trust level: `/hooks trust <name>`
- Check error logs
- Test manually: `/hooks run <name>`

**Permission denied:**

- Check trust level: `/hooks list`
- Trust hook: `/hooks trust <name>`
- Verify hook in correct directory

**Invalid action for event:**

- `runCommand` only works with `promptSubmit` and `agentStop`
- Use `askAgent` for file events
- Check protocol documentation

**See:** [Troubleshooting Guide](../Troubleshooting.md)

---

## 📊 Implementation Status

### Current (v0.1.6)

- ✅ Hook Registry
- ✅ Event System
- ✅ Trust Model
- ✅ Hook Protocol
- ✅ File Event Hooks
- ✅ Agent Event Hooks
- ✅ User Triggered Hooks
- ✅ Hook Commands

### Planned (v0.2.0)

- ⏳ Hook Marketplace
- ⏳ Hook Templates
- ⏳ Hook Analytics

### Planned (v0.3.0)

- ⏳ Advanced Hook Chaining
- ⏳ Conditional Hooks
- ⏳ Hook Performance Monitoring

---

## 🤝 Related Documentation

### Core Systems

- [Tools System](../Tools/README.md) - Tool execution
- [MCP Integration](../MCP/MCP_Index.md) - MCP servers
- [User Interface](../UI&Settings/README.md) - UI documentation

### Commands

- [Hook Commands](../UI&Settings/Commands.md#hook-management) - CLI commands

### Developer Resources

- Knowledge DB: `dev_HookSystem.md` - Hook system architecture

---

## 🎯 Quick Start

### For New Users

1. **List Available Hooks**

   ```bash
   /hooks list
   ```

2. **Enable a Hook**

   ```bash
   /hooks enable auto-format-on-save
   ```

3. **Create Your First Hook**
   ```bash
   /hooks create my-first-hook
   ```

### For Advanced Users

1. **Debug Hook Execution**

   ```bash
   /hooks debug <name>
   ```

2. **Trust Workspace Hooks**

   ```bash
   /hooks trust <name>
   ```

3. **Create Custom Hooks**
   - See [User Guide](UserGuide.md#creating-hooks)
   - See [Protocol](Protocol.md)

---

## 📈 Best Practices

### Hook Design

1. **Keep Hooks Simple** - One hook, one purpose
2. **Use Descriptive Names** - Clear, meaningful names
3. **Add Descriptions** - Explain what hook does
4. **Test Thoroughly** - Test before deploying
5. **Handle Errors** - Graceful error handling

### Security

1. **Review Downloaded Hooks** - Always review before trusting
2. **Use Workspace Hooks** - Keep project-specific hooks in workspace
3. **Limit Permissions** - Only grant necessary permissions
4. **Regular Audits** - Review trusted hooks periodically

### Performance

1. **Avoid Heavy Operations** - Keep hooks fast
2. **Use Async When Possible** - Don't block main thread
3. **Cache Results** - Cache expensive operations
4. **Monitor Performance** - Track hook execution time

---

**Last Updated:** January 26, 2026  
**Version:** 0.1.6  
**Status:** Active Development

<!-- Alpha release v0.1.6 -->

