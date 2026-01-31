# Tool System Documentation

**Complete Documentation for the Tool Execution System**

---

## 📚 Quick Navigation

- [User Guide](UserGuide.md) - Using tools in OLLM CLI
- [Architecture](Architecture.md) - Technical architecture and design
- [Extension System](GettingStarted.md) - Extension tools
- [Manifest Reference](ManifestReference.md) - Extension manifest schema

---

## Overview

The Tool Execution System enables LLMs to perform actions through a unified interface. Tools can read files, access the web, execute commands, manage memory, and more.

### Key Features

- ✅ Unified tool registry for all tool types
- ✅ Schema-based parameter validation
- ✅ Permission-based approval system
- ✅ Dynamic tool registration at runtime
- ✅ MCP tool integration
- ✅ Result formatting and truncation
- ✅ Error handling and recovery

---

## Documentation by Audience

### For New Users

**Start Here:**

1. [User Guide](UserGuide.md) - Complete guide to using tools
2. [Available Tools](#available-tools) - Tool reference
3. [Approval Modes](#approval-modes) - Safety settings

**Then Explore:**

- [Extension System](GettingStarted.md) - Adding more tools
- [MCP Integration](../MCP/MCP_Integration.md) - External tools

---

### For Developers

**Start Here:**

1. [Architecture](Architecture.md) - System design
2. [Tool Categories](#tool-categories) - Tool organization
3. [Integration Points](#integration-points) - System integration

**Then Explore:**

- [MCP Architecture](../MCP/MCP_Architecture.md) - MCP tool integration
- [Hook Architecture](../Hooks/Architecture.md) - Hook integration
- [Extension Development](GettingStarted.md) - Creating tools

---

## Available Tools

### File Discovery

| Tool   | Description             | Risk Level | Auto-Approve |
| ------ | ----------------------- | ---------- | ------------ |
| `glob` | Find files by pattern   | Low        | ✅ Yes       |
| `ls`   | List directory contents | Low        | ✅ Yes       |
| `grep` | Search file contents    | Low        | ✅ Yes       |

### File Operations

| Tool              | Description            | Risk Level | Auto-Approve |
| ----------------- | ---------------------- | ---------- | ------------ |
| `read_file`       | Read single file       | Low        | ✅ Yes       |
| `read_many_files` | Read multiple files    | Low        | ✅ Yes       |
| `edit_file`       | Edit file sections     | Medium     | ❌ No        |
| `write_file`      | Create/overwrite files | Medium     | ❌ No        |

### Web Tools

| Tool         | Description       | Risk Level | Auto-Approve |
| ------------ | ----------------- | ---------- | ------------ |
| `web_search` | Search internet   | Low        | ✅ Yes       |
| `web_fetch`  | Fetch URL content | Low        | ✅ Yes       |

### Shell Tool

| Tool    | Description      | Risk Level | Auto-Approve |
| ------- | ---------------- | ---------- | ------------ |
| `shell` | Execute commands | High       | ❌ No        |

### Memory Tools

| Tool                | Description           | Risk Level | Auto-Approve |
| ------------------- | --------------------- | ---------- | ------------ |
| `memory`            | Persistent memory     | Low        | ✅ Yes       |
| `remember`          | Simplified memory     | Low        | ✅ Yes       |
| `write_memory_dump` | Context snapshot      | Low        | ✅ Yes       |
| `read_reasoning`    | Review past reasoning | Low        | ✅ Yes       |

### Goal Tools

| Tool                | Description        | Risk Level | Auto-Approve |
| ------------------- | ------------------ | ---------- | ------------ |
| `create_goal`       | Create new goal    | Low        | ✅ Yes       |
| `switch_goal`       | Switch active goal | Low        | ✅ Yes       |
| `complete_goal`     | Mark goal complete | Low        | ✅ Yes       |
| `create_checkpoint` | Create checkpoint  | Low        | ✅ Yes       |
| `record_decision`   | Record decision    | Low        | ✅ Yes       |

---

## Approval Modes

### YOLO Mode

**Description:** Auto-approve all tools

**Use When:** Maximum speed, trusted environment

**Risks:** No safety checks, potential for mistakes

### AUTO Mode (Recommended)

**Description:** Auto-approve safe tools, ask for dangerous ones

**Use When:** Normal development work, balanced safety

**Behavior:**

- ✅ Auto-approve: Read operations, web search
- ❌ Require approval: Write operations, shell commands

### ASK Mode

**Description:** Confirm every tool

**Use When:** Learning, critical data, maximum safety

**Behavior:**

- ❌ Require approval: All tools

---

## Tool Categories

### By Purpose

**File Operations:**

- Discovery: glob, ls, grep
- Reading: read_file, read_many_files
- Writing: edit_file, write_file

**Web Access:**

- Search: web_search
- Fetch: web_fetch

**System:**

- Shell: shell

**Data Management:**

- Memory: memory, remember
- Context: write_memory_dump, read_reasoning
- Goals: create_goal, switch_goal, complete_goal, create_checkpoint, record_decision

### By Risk Level

**Low Risk (Auto-Approve in AUTO mode):**

- All read operations
- Web search and fetch
- Memory operations
- Goal management

**Medium Risk (Require Approval):**

- File write operations
- File edit operations

**High Risk (Always Require Approval):**

- Shell command execution

---

## Integration Points

### MCP Integration

MCP tools are automatically registered in the Tool Registry:

```
MCP Server → Tool Discovery → Schema Conversion → Tool Registration → Available to LLM
```

**See:** [MCP Integration](../MCP/MCP_Integration.md)

### Hook Integration

Hooks can trigger tool execution through `askAgent` action:

```json
{
  "action": {
    "type": "askAgent",
    "prompt": "Run linting using the shell tool"
  }
}
```

**See:** [Hook Architecture](../Hooks/Architecture.md)

### Extension Integration

Extensions can provide tools via MCP servers:

```json
{
  "mcpServers": {
    "custom-tools": {
      "command": "node",
      "args": ["server.js"]
    }
  }
}
```

**See:** [Extension System](GettingStarted.md)

---

## Quick Start

### Using Tools

Simply ask the LLM to perform actions:

```
"Read the README file"
"Search for React documentation"
"Run npm test"
"Create a new component file"
```

### Setting Approval Mode

```bash
# Recommended: AUTO mode
/config set approvalMode AUTO

# Maximum speed: YOLO mode
/config set approvalMode YOLO

# Maximum safety: ASK mode
/config set approvalMode ASK
```

### Approving Tools

When prompted:

- `y` - Approve this execution
- `n` - Deny this execution
- `a` - Approve all (switch to YOLO)
- `s` - Skip and continue

---

## Troubleshooting

### Tools Not Working

**Check approval mode:**

```bash
/config get approvalMode
```

**Check tool registration:**

```bash
/tools list
```

**Known Issue:** Tools not being passed to provider  
**Status:** Fix in progress

**See:** [Troubleshooting](UserGuide.md#troubleshooting)

### Tool Execution Fails

**Check error message:**

- Read error carefully
- Verify parameters
- Check file paths
- Verify permissions

**See:** [Troubleshooting](UserGuide.md#troubleshooting)

---

## Best Practices

### Tool Usage

**Do:**

- ✅ Use natural language requests
- ✅ Let LLM choose tools
- ✅ Review actions before approval
- ✅ Use AUTO mode for balance

**Don't:**

- ❌ Manually invoke tools
- ❌ Use YOLO on critical data
- ❌ Ignore approval prompts
- ❌ Approve without reading

**See:** [Best Practices](UserGuide.md#best-practices)

---

## Examples

### File Operations

```
User: "Read package.json and update the version to 2.0.0"

LLM: [Uses read_file tool]
LLM: [Uses edit_file tool with approval]
LLM: "Version updated to 2.0.0"
```

### Web Research

```
User: "Search for React hooks best practices and summarize"

LLM: [Uses web_search tool]
LLM: [Uses web_fetch tool]
LLM: "Here's a summary..."
```

### Development Workflow

```
User: "Run tests, and if they fail, show me the errors"

LLM: [Uses shell tool with approval]
LLM: [Uses read_file tool if needed]
LLM: "Tests failed with 2 errors..."
```

**See:** [Examples](UserGuide.md#examples)

---

## Documentation Status

### Completed ✅

| Document             | Status      |
| -------------------- | ----------- |
| README.md            | ✅ Complete |
| UserGuide.md         | ✅ Complete |
| Architecture.md      | ✅ Complete |
| GettingStarted.md    | ✅ Complete |
| ManifestReference.md | ✅ Complete |

**Overall Progress:** 100% complete (5/5 files)

---

## External Resources

### Official Resources

- Tool Use Patterns (https://docs.anthropic.com/claude/docs/tool-use)
- JSON Schema (https://json-schema.org/)

### OLLM CLI Resources

- [Main Documentation](../) - OLLM CLI docs
- [MCP Documentation](../MCP/) - MCP integration
- [Hook Documentation](../Hooks/) - Hook system

---

## Contributing

Want to contribute to Tool documentation?

1. Read Contributing Guide (../../CONTRIBUTING.md)
2. Check Documentation Tracking
3. Submit pull requests

---

## Search Tips

### Finding Information

**By Task:**

- Using tools → [User Guide](UserGuide.md)
- Understanding architecture → [Architecture](Architecture.md)
- Adding tools → [Extension System](GettingStarted.md)

**By Tool:**

- File operations → [File Tools](#file-operations)
- Web access → [Web Tools](#web-tools)
- Shell commands → [Shell Tool](#shell-tool)
- Memory → [Memory Tools](#memory-tools)
- Goals → [Goal Tools](#goal-tools)

**By Topic:**

- Approval → [Approval Modes](#approval-modes)
- Security → [Architecture - Security Model](Architecture.md#security-model)
- Integration → [Integration Points](#integration-points)

---

**Last Updated:** 2026-01-26  
**Version:** 0.1.6  
**Documentation Progress:** 100% complete
