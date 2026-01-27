# Tool System User Guide

**Complete Guide to Using Tools in OLLM CLI**

This guide covers everything you need to know about using tools for LLM-powered automation.

---

## 📋 Table of Contents

1. [Introduction](#introduction)
2. [Tool Basics](#tool-basics)
3. [Available Tools](#available-tools)
4. [Using Tools](#using-tools)
5. [Approval Modes](#approval-modes)
6. [Tool Categories](#tool-categories)
7. [Troubleshooting](#troubleshooting)
8. [Best Practices](#best-practices)

**See Also:**
- [Tool Architecture](Architecture.md) - Technical architecture
- [MCP Integration](../MCP/MCP_Integration.md) - MCP tool integration
- [Extension System](GettingStarted.md) - Extension tools

---

## Introduction

### What are Tools?

Tools are functions that the LLM can call to perform actions:
- **File Operations** - Read, write, edit files
- **Web Access** - Search internet, fetch URLs
- **Shell Commands** - Execute system commands
- **Memory** - Store and retrieve information
- **Goals** - Manage goals and decisions

### Why Use Tools?

**Benefits:**
- 🚀 Automation - LLM can perform actions automatically
- 🔍 Information Access - Access files, web, and data
- 💾 Persistence - Store information across sessions
- 🛡️ Safety - Permission-based approval system
- 🔧 Extensibility - Add custom tools via MCP

---

## Tool Basics

### How Tools Work

```
User Request
    ↓
LLM Analyzes Request
    ↓
LLM Selects Tool
    ↓
Permission Check
    ↓
User Approval (if needed)
    ↓
Tool Executes
    ↓
Result Returned to LLM
    ↓
LLM Responds to User
```

### Tool Execution

**Automatic:**
- LLM decides when to use tools
- No explicit tool invocation needed
- Natural language requests

**Example:**
```
User: "What's in the README file?"
LLM: [Uses read_file tool]
LLM: "The README contains..."
```

### Tool Approval

Tools require approval based on risk level:
- **Low Risk** - Auto-approved (read operations)
- **Medium Risk** - Requires approval (write operations)
- **High Risk** - Always requires approval (shell commands)

---

## Available Tools

### File Discovery

| Tool | Description | Example |
|------|-------------|---------|
| `glob` | Find files by pattern | "Find all TypeScript files" |
| `ls` | List directory contents | "List files in src/" |
| `grep` | Search file contents | "Search for 'TODO' in all files" |

### File Operations

| Tool | Description | Example |
|------|-------------|---------|
| `read_file` | Read single file | "Read README.md" |
| `read_many_files` | Read multiple files | "Read all config files" |
| `edit_file` | Edit file sections | "Update the version in package.json" |
| `write_file` | Create/overwrite files | "Create a new test file" |

### Web Tools

| Tool | Description | Example |
|------|-------------|---------|
| `web_search` | Search internet | "Search for React hooks tutorial" |
| `web_fetch` | Fetch URL content | "Fetch content from example.com" |

### Shell Tool

| Tool | Description | Example |
|------|-------------|---------|
| `shell` | Execute commands | "Run npm test" |

### Memory Tools

| Tool | Description | Example |
|------|-------------|---------|
| `memory` | Persistent memory | "Remember my API key" |
| `remember` | Simplified memory | "Remember this preference" |
| `write_memory_dump` | Context snapshot | "Save current context" |
| `read_reasoning` | Review past reasoning | "Show previous decisions" |

### Goal Tools

| Tool | Description | Example |
|------|-------------|---------|
| `create_goal` | Create new goal | "Create goal for refactoring" |
| `switch_goal` | Switch active goal | "Switch to testing goal" |
| `complete_goal` | Mark goal complete | "Complete refactoring goal" |
| `create_checkpoint` | Create checkpoint | "Create checkpoint for progress" |
| `record_decision` | Record decision | "Record decision to use React" |

---

## Using Tools

### Natural Language Requests

Simply ask the LLM to perform actions:

**File Operations:**
```
"Read the package.json file"
"Show me all TypeScript files in src/"
"Update the version number in package.json to 2.0.0"
"Create a new file called test.ts with a basic test"
```

**Web Access:**
```
"Search for the latest React documentation"
"Fetch the content from https://example.com/api"
"Find tutorials about TypeScript generics"
```

**Shell Commands:**
```
"Run npm install"
"Execute the test suite"
"Check git status"
"Build the project"
```

**Memory:**
```
"Remember that I prefer tabs over spaces"
"What did I say about the API design?"
"Save this conversation for later"
```

**Goals:**
```
"Create a goal to refactor the authentication system"
"Switch to the testing goal"
"Mark the refactoring goal as complete"
"Record the decision to use PostgreSQL"
```

### Tool Approval

When a tool requires approval, you'll see:

```
🔧 Tool: write_file
📝 Action: Create file "test.ts"
📄 Content: [preview]

Approve? (y/n)
```

**Options:**
- `y` - Approve this execution
- `n` - Deny this execution
- `a` - Approve all (switch to YOLO mode)
- `s` - Skip and continue

---

## Approval Modes

### YOLO Mode

**Description:** Auto-approve all tools

**Use When:**
- You trust the LLM completely
- Working on non-critical tasks
- Maximum speed needed

**Risks:**
- No safety checks
- Potential for mistakes
- Irreversible actions

**Enable:**
```bash
/config set approvalMode YOLO
```

### AUTO Mode (Recommended)

**Description:** Auto-approve safe tools, ask for dangerous ones

**Use When:**
- Normal development work
- Balanced safety and speed
- Most use cases

**Behavior:**
- ✅ Auto-approve: Read operations, web search
- ❌ Require approval: Write operations, shell commands

**Enable:**
```bash
/config set approvalMode AUTO
```

### ASK Mode

**Description:** Confirm every tool

**Use When:**
- Learning the system
- Working with critical data
- Maximum safety needed

**Behavior:**
- ❌ Require approval: All tools

**Enable:**
```bash
/config set approvalMode ASK
```

---

## Tool Categories

### File Discovery Tools

**Purpose:** Find and locate files

**Tools:**
- `glob` - Pattern matching (wildcards, .gitignore aware)
- `ls` - Directory listing (recursive, sizes, permissions)
- `grep` - Content search (regex, case-sensitive/insensitive)

**Examples:**
```
"Find all JavaScript files"
"List files in the src directory"
"Search for 'TODO' comments in all files"
"Find files modified in the last week"
```

**Risk Level:** Low (read-only)  
**Auto-Approve:** ✅ Yes (in AUTO mode)

### File Operations Tools

**Purpose:** Read and write files

**Tools:**
- `read_file` - Read single file (line ranges, large file handling)
- `read_many_files` - Read multiple files (batch reading)
- `edit_file` - Edit file sections (search-replace, diff preview)
- `write_file` - Create/overwrite files (creates directories)

**Examples:**
```
"Read the README file"
"Show me the first 10 lines of app.ts"
"Update the version in package.json"
"Create a new component file"
```

**Risk Level:** 
- Read: Low
- Write: Medium

**Auto-Approve:**
- Read: ✅ Yes (in AUTO mode)
- Write: ❌ No (requires approval)

### Web Tools

**Purpose:** Access web resources

**Tools:**
- `web_search` - Search internet (multiple results, snippets)
- `web_fetch` - Fetch URL content (various content types, redirects)

**Examples:**
```
"Search for React hooks documentation"
"Fetch the content from example.com/api"
"Find the latest TypeScript release notes"
"Get the HTML from this URL"
```

**Risk Level:** Low (read-only)  
**Auto-Approve:** ✅ Yes (in AUTO mode)

### Shell Tool

**Purpose:** Execute shell commands

**Tool:**
- `shell` - Execute commands (streams output, env vars)

**Examples:**
```
"Run npm test"
"Execute git status"
"Build the project with npm run build"
"Check the Node.js version"
```

**Risk Level:** High (can modify system)  
**Auto-Approve:** ❌ No (always requires approval unless YOLO)

**Note:** Shell tool UI enhancement planned (collapsible terminal with visual indicators).

### Memory & Context Tools

**Purpose:** Store and retrieve information

**Tools:**
- `memory` - Persistent memory (survives sessions, searchable)
- `remember` - Simplified memory (quick storage)
- `write_memory_dump` - Context snapshot (debugging, backup)
- `read_reasoning` - Review past reasoning (conversation history)

**Examples:**
```
"Remember that I prefer TypeScript over JavaScript"
"What did I say about the database design?"
"Save the current context for later"
"Show me the reasoning from the last session"
```

**Risk Level:** Low (data storage)  
**Auto-Approve:** ✅ Yes (in AUTO mode)

### Goal Management Tools

**Purpose:** Manage goals and decisions

**Tools:**
- `create_goal` - Create new goal
- `switch_goal` - Switch active goal
- `complete_goal` - Mark goal complete
- `create_checkpoint` - Create goal checkpoint
- `record_decision` - Record decision with rationale

**Examples:**
```
"Create a goal to refactor the authentication system"
"Switch to the testing goal"
"Mark the refactoring goal as complete"
"Create a checkpoint for the current progress"
"Record the decision to use PostgreSQL with rationale"
```

**Risk Level:** Low (metadata)  
**Auto-Approve:** ✅ Yes (in AUTO mode)

**See:** Goal system documentation for more details

---

## Troubleshooting

### Tools Not Working

**Symptom:** LLM doesn't use tools, only provides advice

**Possible Causes:**
1. Tools not passed to provider (known issue)
2. Approval mode blocking tools
3. Tool not registered

**Solutions:**

**Check approval mode:**
```bash
/config get approvalMode
# Should be AUTO or YOLO for automatic tool use
```

**Check if tools are registered:**
```bash
/tools list
# Should show available tools
```

**Known Issue:** Tools not being passed to provider  
**Status:** Fix in progress (see Architecture doc)

### Tool Execution Fails

**Symptom:** Tool call returns error

**Possible Causes:**
1. Invalid parameters
2. Permission denied
3. File not found
4. Network error

**Solutions:**

**Check error message:**
- Read the error carefully
- Verify parameters are correct
- Check file paths exist
- Verify network connectivity

**Try manual execution:**
```bash
# Test the operation manually
cat README.md  # Instead of read_file
ls src/        # Instead of ls tool
```

**Check permissions:**
```bash
# Verify file permissions
ls -la file.txt

# Verify directory permissions
ls -la directory/
```

### Permission Denied

**Symptom:** Tool execution blocked by policy engine

**Possible Causes:**
1. Approval mode is ASK
2. Tool is high risk
3. User denied approval

**Solutions:**

**Change approval mode:**
```bash
# Switch to AUTO mode
/config set approvalMode AUTO

# Or YOLO mode (use with caution)
/config set approvalMode YOLO
```

**Approve manually:**
- When prompted, press `y` to approve
- Or press `a` to approve all

**Review tool risk:**
- Read operations: Low risk
- Write operations: Medium risk
- Shell commands: High risk

---

## Best Practices

### Tool Usage

**Do:**
- ✅ Use natural language requests
- ✅ Let LLM choose appropriate tools
- ✅ Review tool actions before approval
- ✅ Use AUTO mode for balanced safety
- ✅ Check tool results

**Don't:**
- ❌ Manually invoke tools (let LLM decide)
- ❌ Use YOLO mode on critical data
- ❌ Ignore approval prompts
- ❌ Approve without reading

### Approval Management

**Do:**
- ✅ Use AUTO mode for most work
- ✅ Use ASK mode when learning
- ✅ Review dangerous operations
- ✅ Switch modes as needed

**Don't:**
- ❌ Stay in YOLO mode permanently
- ❌ Approve blindly
- ❌ Ignore risk warnings

### File Operations

**Do:**
- ✅ Backup important files first
- ✅ Review diffs before approval
- ✅ Use version control
- ✅ Test on non-critical files first

**Don't:**
- ❌ Modify critical files without backup
- ❌ Approve large file changes without review
- ❌ Work without version control

### Shell Commands

**Do:**
- ✅ Review commands before approval
- ✅ Understand what commands do
- ✅ Use safe commands when possible
- ✅ Check command output

**Don't:**
- ❌ Approve dangerous commands (rm -rf)
- ❌ Run commands you don't understand
- ❌ Use YOLO mode with shell tool
- ❌ Ignore error messages

---

## Examples

### Example 1: File Operations

**Scenario:** Read and update a configuration file

```
User: "Read the package.json file"

LLM: [Uses read_file tool]
LLM: "The package.json contains..."

User: "Update the version to 2.0.0"

LLM: [Uses edit_file tool]
🔧 Tool: edit_file
📝 Action: Update version in package.json
📄 Changes: "version": "1.0.0" → "version": "2.0.0"

Approve? (y/n) y

LLM: "Version updated to 2.0.0"
```

### Example 2: Web Research

**Scenario:** Research and summarize information

```
User: "Search for React hooks best practices"

LLM: [Uses web_search tool]
LLM: "I found several resources about React hooks best practices..."

User: "Fetch the content from the first result"

LLM: [Uses web_fetch tool]
LLM: "Here's a summary of the best practices..."
```

### Example 3: Development Workflow

**Scenario:** Run tests and fix issues

```
User: "Run the test suite"

LLM: [Uses shell tool]
🔧 Tool: shell
📝 Command: npm test

Approve? (y/n) y

LLM: "Tests failed with 2 errors..."

User: "Show me the failing test file"

LLM: [Uses read_file tool]
LLM: "Here's the test file..."

User: "Fix the test"

LLM: [Uses edit_file tool]
🔧 Tool: edit_file
📝 Action: Fix test in test.ts

Approve? (y/n) y

LLM: "Test fixed. Run tests again?"
```

### Example 4: Goal Management

**Scenario:** Manage a refactoring goal

```
User: "Create a goal to refactor the authentication system"

LLM: [Uses create_goal tool]
LLM: "Goal created: Refactor authentication system"

User: "Record the decision to use JWT tokens"

LLM: [Uses record_decision tool]
LLM: "Decision recorded with rationale"

User: "Create a checkpoint for the current progress"

LLM: [Uses create_checkpoint tool]
LLM: "Checkpoint created"

User: "Mark the goal as complete"

LLM: [Uses complete_goal tool]
LLM: "Goal marked as complete"
```

---

## Advanced Usage

### Combining Tools

LLM can use multiple tools in sequence:

```
User: "Find all TODO comments and create a task list"

LLM: [Uses grep tool to find TODOs]
LLM: [Uses write_file tool to create task list]
LLM: "Created task list with 15 TODOs"
```

### Conditional Tool Use

LLM can decide whether to use tools based on context:

```
User: "If there are any errors in the logs, fix them"

LLM: [Uses grep tool to search logs]
LLM: "No errors found in logs"
# (No fix needed, no additional tools used)
```

### Tool Chaining

LLM can chain tools for complex workflows:

```
User: "Search for React documentation, fetch it, and summarize"

LLM: [Uses web_search tool]
LLM: [Uses web_fetch tool]
LLM: [Summarizes content]
LLM: "Here's a summary of React documentation..."
```

---

## Further Reading

### Documentation
- [Tool Architecture](Architecture.md) - Technical architecture
- [MCP Integration](../MCP/MCP_Integration.md) - MCP tool integration
- [Extension System](GettingStarted.md) - Extension tools

### Related Features
- [Hooks](../Hooks/UserGuide.md) - Hook system
- [MCP Servers](../MCP/MCP_GettingStarted.md) - MCP servers
- [Goals](dev_PromptSystem.md) - Goal management

### External Resources
- Tool Use Patterns (https://docs.anthropic.com/claude/docs/tool-use)
- JSON Schema (https://json-schema.org/)

---

**Last Updated:** 2026-01-26  
**Version:** 0.1.0  
**Next:** [Tool Architecture](Architecture.md)

