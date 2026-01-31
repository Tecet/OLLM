# Hook System User Guide

**Complete Guide to Using Hooks in OLLM CLI**

This guide covers everything you need to know about using hooks for workflow automation in OLLM CLI.

---

## 📋 Table of Contents

1. [Introduction](#introduction)
2. [Hook Basics](#hook-basics)
3. [Hooks Panel UI](#hooks-panel-ui)
4. [Creating Hooks](#creating-hooks)
5. [Managing Hooks](#managing-hooks)
6. [Hook Events](#hook-events)
7. [Trust and Security](#trust-and-security)
8. [Common Workflows](#common-workflows)
9. [Troubleshooting](#troubleshooting)

**See Also:**

- [Hook System Overview](3%20projects/OLLM%20CLI/Hooks/README.md) - Hook system introduction
- [Hook Development Guide](3%20projects/OLLM%20CLI/Hooks/development-guide.md) - Creating custom hooks
- [Hook Protocol](protocol.md) - Technical protocol specification
- [MCP Commands](MCP_commands.md) - Hook command reference

---

## Introduction

Hooks are executables that run in response to events during LLM execution. They enable:

- **Workflow Automation** - Automate repetitive tasks
- **Safety Gates** - Validate operations before execution
- **Custom Logic** - Add custom behavior to OLLM CLI
- **Integration** - Connect with external tools and services

### When to Use Hooks

**Use hooks when you want to:**

- Run commands before/after LLM execution
- Validate tool calls before execution
- Format code automatically on save
- Run tests before commits
- Integrate with external systems
- Add custom approval logic

**Don't use hooks when:**

- Simple configuration would work
- Performance is critical (hooks add overhead)
- You need real-time interaction

---

## Hook Basics

### What is a Hook?

A hook is an executable (script, binary, etc.) that:

1. Receives event data via **stdin** (JSON)
2. Processes the event
3. Returns a response via **stdout** (JSON)

### Hook Lifecycle

```
Event Occurs
    ↓
Hook Registry Finds Matching Hooks
    ↓
Check Trust Level
    ↓
Request Approval (if needed)
    ↓
Execute Hook (pass event data via stdin)
    ↓
Hook Processes Event
    ↓
Hook Returns Response (via stdout)
    ↓
Process Response (allow/block/modify)
    ↓
Continue Execution
```

### Hook Response

Hooks return JSON with:

- `allow` (boolean) - Whether to allow the operation
- `message` (string) - Message to display
- `metadata` (object) - Optional metadata

**Example:**

```json
{
  "allow": true,
  "message": "Operation approved",
  "metadata": {
    "checked": true,
    "timestamp": "2026-01-16T10:00:00Z"
  }
}
```

---

## Hooks Panel UI

### Overview

The Hooks Panel provides an interactive terminal UI for viewing and managing hooks without editing JSON files. Access it by pressing **Tab** to navigate to the Hooks tab in the main navigation bar.

### Panel Layout

The Hooks Panel uses a two-column layout:

```
┌─────────────────────────────────────────────────────────────┐
│ 🎣 Hooks Configuration                    ↑↓:Nav Enter:Toggle│
├──────────────┬──────────────────────────────────────────────┤
│ Left (30%)   │ Right (70%)                                   │
│              │                                               │
│ ⬅️ Exit      │ Hook Details:                                 │
│              │ - Name (bold, yellow)                         │
│ 📝 File      │ - ID                                          │
│  ● Hook 1    │ - Command                                     │
│  ○ Hook 2    │ - Arguments                                   │
│              │ - Source (builtin/user/extension)             │
│ 💬 Prompt    │ - Status (✓ Enabled / ✗ Disabled)            │
│  ● Hook 3    │                                               │
│              │                                               │
│ 👤 User      │                                               │
│  ○ Hook 4    │                                               │
└──────────────┴──────────────────────────────────────────────┘
```

**Left Column (30%):**

- Exit item at the top
- Hooks organized by category with icons
- Visual indicators for enabled (●) and disabled (○) hooks
- Selected hook highlighted in yellow
- Scroll indicators (▲/▼) when list is longer than window

**Right Column (70%):**

- Detailed information for the selected hook
- Command and arguments
- Source and extension information
- Current enabled/disabled status

### Keyboard Shortcuts

#### Navigation

| Key              | Action                                 |
| ---------------- | -------------------------------------- |
| **↑**            | Move up to previous hook               |
| **↓**            | Move down to next hook                 |
| **Tab**          | Enter Hooks Panel from main navigation |
| **Esc** or **0** | Exit to main navigation                |

#### Hook Actions

| Key                         | Action                       |
| --------------------------- | ---------------------------- |
| **Enter** or **←** or **→** | Toggle hook enabled/disabled |
| **A**                       | Add new hook                 |
| **E**                       | Edit selected hook           |
| **D**                       | Delete selected hook         |
| **T**                       | Test selected hook           |

### Visual Indicators

#### Hook Status

- **● Green** - Hook is enabled
- **○ Gray** - Hook is disabled
- **Yellow highlight** - Currently selected hook (when panel has focus)
- **Cyan border** - Panel has focus

#### Category Icons

- **📝** File Events - Hooks triggered by file changes
- **💬** Prompt Events - Hooks triggered by prompt submission
- **👤** User Triggered - Manually triggered hooks
- **🔄** Session Events - Hooks for session lifecycle
- **🤖** Agent Events - Hooks for agent operations
- **🧠** Model Events - Hooks for model operations
- **🔧** Tool Events - Hooks for tool execution
- **📦** Compression Events - Hooks for context compression
- **🔔** Notifications - Hooks for notifications

#### Action Icons

- **➕** Add new hook
- **✏️** Edit hook
- **🗑️** Delete hook
- **🧪** Test hook
- **🔄** Toggle hook
- **⬅️** Exit panel

### Using the Hooks Panel

#### Viewing Hooks

1. **Navigate to Hooks Panel:**
   - Press **Tab** repeatedly until "Hooks" is highlighted in the navigation bar
   - Press **Enter** to activate the Hooks Panel

2. **Browse Hooks:**
   - Use **↑** and **↓** arrow keys to navigate through hooks
   - Hooks are organized by category (File Events, Prompt Events, etc.)
   - The right panel shows detailed information for the selected hook

3. **View Hook Details:**
   - Select a hook to see its full details in the right panel
   - Details include: name, ID, command, arguments, source, and status

#### Enabling/Disabling Hooks

1. **Select a Hook:**
   - Navigate to the hook using **↑** and **↓** keys

2. **Toggle Status:**
   - Press **Enter**, **←**, or **→** to toggle the hook
   - The indicator changes from ● (enabled) to ○ (disabled) or vice versa
   - Changes are saved immediately to `~/.ollm/settings.json`

3. **Visual Feedback:**
   - Enabled hooks show a green ● indicator
   - Disabled hooks show a gray ○ indicator
   - The status box in the right panel updates to show ✓ Enabled or ✗ Disabled

#### Adding New Hooks

1. **Open Add Dialog:**
   - Press **A** key from anywhere in the Hooks Panel

2. **Fill in Hook Details:**
   - **Name:** Descriptive name for the hook
   - **Command:** The command to execute
   - **Arguments:** Command-line arguments (optional)

3. **Save Hook:**
   - Press **S** to save the hook
   - Press **C** or **Esc** to cancel

4. **Result:**
   - New hook appears in the list immediately
   - Hook is saved to `~/.ollm/hooks/` directory
   - Hook is enabled by default

#### Editing Hooks

1. **Select a Hook:**
   - Navigate to the hook you want to edit

2. **Open Edit Dialog:**
   - Press **E** key

3. **Modify Details:**
   - Update name, command, or arguments
   - **Note:** Built-in hooks cannot be edited (only disabled)

4. **Save Changes:**
   - Press **S** to save changes
   - Press **C** or **Esc** to cancel

5. **Result:**
   - Hook updates immediately in the list
   - Changes are saved to the hook file

#### Deleting Hooks

1. **Select a Hook:**
   - Navigate to the hook you want to delete

2. **Open Delete Dialog:**
   - Press **D** key

3. **Confirm Deletion:**
   - Review the warning message
   - Press **D** to confirm deletion
   - Press **C** or **Esc** to cancel

4. **Result:**
   - Hook is removed from the list immediately
   - Hook file is deleted from `~/.ollm/hooks/` directory
   - **Note:** Built-in hooks cannot be deleted (only disabled)

#### Testing Hooks

1. **Select a Hook:**
   - Navigate to the hook you want to test

2. **Open Test Dialog:**
   - Press **T** key

3. **View Test Results:**
   - The dialog simulates the hook trigger event
   - Shows success/failure status
   - Displays any errors or warnings
   - Test doesn't affect actual system state

4. **Close Dialog:**
   - Press **Enter** or **Esc** to close

### Windowed Rendering

For performance with large hook lists (> 15 hooks), the panel uses windowed rendering:

- Only visible hooks are rendered (default window size: 15 items)
- Scroll indicators (▲/▼) show when more hooks are available
- Auto-scroll keeps the selected hook visible
- Smooth navigation even with 50+ hooks

### Exit Item

The Exit item at the top of the list provides a quick way to return to the main navigation:

1. **Navigate to Exit:**
   - Press **↑** from the first hook to reach the Exit item
   - Exit item is highlighted in yellow when selected

2. **Activate Exit:**
   - Press **Enter** on the Exit item
   - Or press **Esc** or **0** from anywhere in the panel

3. **Result:**
   - Returns to Browse Mode (main navigation bar)
   - Hooks Panel loses focus

### Corrupted Hooks Warning

If corrupted hook files are detected, a warning banner appears at the top:

```
⚠️  2 corrupted hook(s) found
```

- Corrupted hooks are skipped during loading
- Other hooks continue to work normally
- Check hook files in `~/.ollm/hooks/` for JSON syntax errors

---

## Creating Hooks

### Quick Create

The simplest way to create a hook:

```bash
# Create a command hook
/hooks create pre-execution "echo '{\"allow\":true}'"

# Create a script hook
/hooks create on-file-change "./scripts/format.sh {file}"

# Create from file
/hooks create pre-tool-call ./hooks/safety-check.js
```

### Command Hooks

Simple one-line commands:

```bash
# Echo a message
/hooks create pre-execution "echo 'Starting execution...'"

# Run a formatter
/hooks create on-file-change "prettier --write {file}"

# Run tests
/hooks create on-git-commit "npm test"
```

### Script Hooks

More complex logic in scripts:

**Bash Script:**

```bash
#!/bin/bash
# save as: hooks/format-on-save.sh

# Read input
input=$(cat)

# Extract file path
file=$(echo "$input" | jq -r '.context.file')

# Format file
prettier --write "$file"

# Return success
echo '{"allow": true, "message": "File formatted"}'
```

**Create hook:**

```bash
/hooks create on-file-change ./hooks/format-on-save.sh
```

### Executable Hooks

Compiled binaries or any executable:

```bash
# Create from binary
/hooks create pre-tool-call ./bin/safety-checker

# Create from Python script
/hooks create on-error ./scripts/notify.py

# Create from Node.js script
/hooks create post-execution ./scripts/log.js
```

---

## Managing Hooks

### Listing Hooks

```bash
# List all hooks
/hooks list

# Output shows:
# - Hook name
# - Event type
# - Trust level
# - Enabled status
```

### Enabling/Disabling Hooks

```bash
# Disable a hook
/hooks disable my-hook

# Enable a hook
/hooks enable my-hook

# Hooks can be disabled temporarily without removing them
```

### Removing Hooks

```bash
# Remove a hook
/hooks remove my-hook

# This permanently deletes the hook
```

### Hook Information

```bash
# Show hook details
/hooks info my-hook

# Shows:
# - Name, event, command
# - Trust level
# - Enabled status
# - Last execution time
# - Execution count
```

---

## Hook Events

### Available Events

OLLM CLI supports 12 hook events:

#### 1. pre-execution

**When:** Before LLM execution starts  
**Use Cases:** Validate prompts, add context, check permissions  
**Context:** `{ prompt, session, user }`

**Example:**

```bash
/hooks create pre-execution "./validate-prompt.sh"
```

#### 2. post-execution

**When:** After LLM execution completes  
**Use Cases:** Save results, trigger actions, log execution  
**Context:** `{ prompt, response, session, duration }`

**Example:**

```bash
/hooks create post-execution "./save-session.sh"
```

#### 3. pre-tool-call

**When:** Before a tool is executed  
**Use Cases:** Safety checks, validation, approval  
**Context:** `{ tool, args, session }`

**Example:**

```bash
/hooks create pre-tool-call "./safety-check.sh"
```

#### 4. post-tool-call

**When:** After a tool is executed  
**Use Cases:** Logging, cleanup, notifications  
**Context:** `{ tool, args, result, duration }`

**Example:**

```bash
/hooks create post-tool-call "./log-tool.sh"
```

#### 5. on-error

**When:** When an error occurs  
**Use Cases:** Error handling, notifications, recovery  
**Context:** `{ error, stack, session }`

**Example:**

```bash
/hooks create on-error "./notify-error.sh"
```

#### 6. on-file-change

**When:** When a file is modified  
**Use Cases:** Format code, run linters, update docs  
**Context:** `{ file, changes, session }`

**Example:**

```bash
/hooks create on-file-change "prettier --write {file}"
```

#### 7. on-git-commit

**When:** Before a git commit  
**Use Cases:** Run tests, validate code, check style  
**Context:** `{ files, message, session }`

**Example:**

```bash
/hooks create on-git-commit "npm test"
```

#### 8. on-session-start

**When:** When a session starts  
**Use Cases:** Initialize, load context, setup  
**Context:** `{ session, user }`

**Example:**

```bash
/hooks create on-session-start "./load-context.sh"
```

#### 9. on-session-end

**When:** When a session ends  
**Use Cases:** Cleanup, save state, backup  
**Context:** `{ session, duration, messageCount }`

**Example:**

```bash
/hooks create on-session-end "./save-state.sh"
```

#### 10. on-context-overflow

**When:** When context is full  
**Use Cases:** Compress, summarize, archive  
**Context:** `{ session, contextSize, maxSize }`

**Example:**

```bash
/hooks create on-context-overflow "./compress-context.sh"
```

#### 11. on-approval-request

**When:** When approval is needed  
**Use Cases:** Custom approval logic, notifications  
**Context:** `{ operation, details, session }`

**Example:**

```bash
/hooks create on-approval-request "./custom-approval.sh"
```

#### 12. custom

**When:** Custom events triggered by extensions  
**Use Cases:** Any custom automation  
**Context:** `{ eventName, data, session }`

**Example:**

```bash
/hooks create custom "./custom-handler.sh"
```

---

## Trust and Security

### Trust Levels

Hooks have three trust levels:

#### 1. Trusted Hooks

- **Source:** System hooks, built-in
- **Approval:** Never required
- **Privilege:** Full access
- **Use Case:** Core functionality

#### 2. Workspace Hooks

- **Source:** Project-specific hooks
- **Approval:** Required first time
- **Privilege:** Limited to workspace
- **Use Case:** Project automation

#### 3. Downloaded Hooks

- **Source:** Extensions, external
- **Approval:** Required each time
- **Privilege:** Sandboxed
- **Use Case:** Third-party automation

### Trusting Hooks

```bash
# Trust a workspace hook
/hooks trust my-hook

# Now runs without approval

# Untrust a hook
/hooks untrust my-hook

# Now requires approval again
```

### Security Best Practices

**Do:**

- ✅ Review hook code before trusting
- ✅ Use workspace hooks for project-specific automation
- ✅ Keep hooks simple and focused
- ✅ Test hooks before trusting
- ✅ Use version control for hooks

**Don't:**

- ❌ Trust hooks from unknown sources
- ❌ Give hooks unnecessary permissions
- ❌ Run hooks with sensitive data without review
- ❌ Trust hooks that modify system files
- ❌ Disable approval for downloaded hooks

---

## Common Workflows

### Quick Start with Hooks Panel

**First Time Setup:**

1. Press **Tab** to navigate to Hooks tab
2. Press **Enter** to open Hooks Panel
3. Browse available hooks with **↑** and **↓**
4. Press **Enter** to enable/disable hooks
5. Press **A** to add your first custom hook

### Using the Hooks Panel for Common Tasks

#### Task 1: Enable Code Formatting

1. **Navigate to Hooks Panel:**
   - Press **Tab** until "Hooks" is highlighted
   - Press **Enter** to activate

2. **Find Format Hook:**
   - Use **↓** to navigate to "File Events" category
   - Look for "format-on-save" hook

3. **Enable Hook:**
   - Press **Enter** to toggle to enabled (●)
   - Status changes to ✓ Enabled

4. **Verify:**
   - Edit a file to trigger formatting
   - File should be formatted automatically

#### Task 2: Create a Custom Hook

1. **Open Add Dialog:**
   - Press **A** from Hooks Panel

2. **Enter Hook Details:**

   ```
   Name: lint-on-save
   Command: eslint
   Arguments: --fix {file}
   ```

3. **Save Hook:**
   - Press **S** to save
   - Hook appears in "File Events" category

4. **Enable Hook:**
   - Navigate to new hook
   - Press **Enter** to enable

#### Task 3: Disable Unwanted Hooks

1. **Find Hook:**
   - Navigate through categories
   - Look for enabled hooks (●)

2. **Disable Hook:**
   - Select the hook
   - Press **Enter** to toggle to disabled (○)

3. **Verify:**
   - Status changes to ✗ Disabled
   - Hook won't execute anymore

#### Task 4: Test Before Enabling

1. **Select Hook:**
   - Navigate to the hook you want to test

2. **Run Test:**
   - Press **T** to open Test dialog
   - Wait for test to complete

3. **Review Results:**
   - Check for success (✓) or failure (✗)
   - Read any error messages

4. **Enable if Successful:**
   - Press **Esc** to close test dialog
   - Press **Enter** to enable hook

#### Task 5: Organize Project Hooks

1. **Review Existing Hooks:**
   - Browse through all categories
   - Note which hooks are enabled

2. **Disable Unused Hooks:**
   - Navigate to each unused hook
   - Press **Enter** to disable

3. **Add Project-Specific Hooks:**
   - Press **A** to add new hooks
   - Create hooks for your workflow

4. **Test Configuration:**
   - Press **T** on each new hook
   - Verify they work as expected

### Code Quality Workflow

```bash
# Format on save
/hooks create on-file-change "prettier --write {file}"
/hooks trust on-file-change

# Lint on save
/hooks create on-file-change "eslint --fix {file}"
/hooks trust on-file-change

# Run tests before commit
/hooks create on-git-commit "npm test"
/hooks trust on-git-commit
```

### Safety Gate Workflow

```bash
# Validate tool calls
/hooks create pre-tool-call "./validate-tool.sh"
/hooks trust pre-tool-call

# Check file operations
/hooks create pre-tool-call "./check-file-access.sh"
/hooks trust pre-tool-call

# Approve destructive operations
/hooks create pre-tool-call "./approve-destructive.sh"
/hooks trust pre-tool-call
```

### Integration Workflow

```bash
# Notify on errors
/hooks create on-error "./notify-slack.sh"
/hooks trust on-error

# Log executions
/hooks create post-execution "./log-to-db.sh"
/hooks trust post-execution

# Sync with external system
/hooks create post-tool-call "./sync-external.sh"
/hooks trust post-tool-call
```

### Session Management Workflow

```bash
# Load context on start
/hooks create on-session-start "./load-context.sh"
/hooks trust on-session-start

# Save state on end
/hooks create on-session-end "./save-state.sh"
/hooks trust on-session-end

# Compress context when full
/hooks create on-context-overflow "./compress.sh"
/hooks trust on-context-overflow
```

---

## Troubleshooting

### Hooks Panel UI Issues

#### Panel Not Showing

**Check if Hooks tab is available:**

```bash
# Press Tab to cycle through tabs
# Look for "Hooks" in the navigation bar
```

**Verify installation:**

- Hooks Panel UI is available in OLLM CLI v0.1.6+
- Check version: `ollm --version`

**Restart CLI:**

```bash
# Exit and restart OLLM CLI
# Press Ctrl+C to exit
# Run ollm again
```

#### Hooks Not Loading

**Check for error messages:**

- Look for error banner at top of panel
- Check for corrupted hooks warning

**Verify hook files:**

```bash
# Check user hooks directory
ls ~/.ollm/hooks/

# Verify JSON syntax
cat ~/.ollm/hooks/my-hook.json | jq .
```

**Check permissions:**

```bash
# Ensure hooks directory is readable
chmod 755 ~/.ollm/hooks/
chmod 644 ~/.ollm/hooks/*.json
```

#### Navigation Not Working

**Ensure panel has focus:**

- Panel border should be cyan when focused
- Selected hook should be highlighted in yellow
- Press **Tab** to give focus to Hooks Panel

**Check keyboard input:**

- Try pressing **↑** and **↓** keys
- Ensure terminal supports arrow keys
- Try alternative keys: **Esc** or **0** to exit

**Restart panel:**

- Press **Esc** to exit panel
- Press **Tab** to re-enter panel

#### Toggle Not Working

**Check hook editability:**

- Built-in hooks can be toggled
- User hooks can be toggled
- Extension hooks can be toggled

**Verify settings file:**

```bash
# Check settings file exists
cat ~/.ollm/settings.json

# Verify hooks section
cat ~/.ollm/settings.json | jq .hooks
```

**Check file permissions:**

```bash
# Ensure settings file is writable
chmod 644 ~/.ollm/settings.json
```

#### Dialogs Not Appearing

**Check terminal size:**

- Dialogs require minimum terminal size
- Resize terminal if too small
- Recommended: 80x24 or larger

**Verify dialog trigger:**

- Press **A** for Add dialog
- Press **E** for Edit dialog (on user hook)
- Press **D** for Delete dialog (on user hook)
- Press **T** for Test dialog

**Close existing dialogs:**

- Press **Esc** or **C** to close any open dialog
- Try opening dialog again

#### Scroll Indicators Not Showing

**Check hook count:**

- Scroll indicators only appear with > 15 hooks
- Add more hooks to test scrolling

**Verify window size:**

- Default window size is 15 items
- Scroll indicators appear when list exceeds window

**Navigate to trigger scroll:**

- Use **↑** and **↓** to navigate
- Scroll indicators appear automatically

### Hook Not Executing

**Check if hook is enabled:**

```bash
/hooks list
# Look for "enabled: true"
```

**Check trust level:**

```bash
/hooks info my-hook
# If "workspace" or "downloaded", may need approval
```

**Enable debug mode:**

```bash
/hooks debug on
# Shows detailed execution logs
```

### Hook Failing

**Test hook manually:**

```bash
# Test with sample input
echo '{"event":"test","context":{}}' | ./my-hook.sh
```

**Check hook output:**

```bash
# Hook must output valid JSON
# Check for syntax errors
```

**Check permissions:**

```bash
# Make sure hook is executable
chmod +x ./my-hook.sh
```

### Hook Approval Issues

**Trust the hook:**

```bash
/hooks trust my-hook
```

**Check trust level:**

```bash
/hooks info my-hook
# Shows current trust level
```

**Review approval settings:**

```bash
# Check if approval is required
# Workspace hooks need approval first time
# Downloaded hooks need approval each time
```

### Performance Issues

**Disable slow hooks:**

```bash
/hooks disable slow-hook
```

**Optimize hook code:**

- Reduce processing time
- Cache results
- Use async operations

**Limit hook execution:**

- Only run on specific events
- Add conditions in hook code
- Use event filtering

---

## Advanced Usage

### Conditional Execution

Hooks can implement conditions:

```bash
#!/bin/bash
# Only run on TypeScript files

input=$(cat)
file=$(echo "$input" | jq -r '.context.file')

if [[ "$file" == *.ts ]]; then
  prettier --write "$file"
  echo '{"allow": true, "message": "Formatted TypeScript file"}'
else
  echo '{"allow": true, "message": "Skipped non-TypeScript file"}'
fi
```

### Chaining Hooks

Multiple hooks can run for the same event:

```bash
# Create multiple hooks for same event
/hooks create on-file-change "./format.sh"
/hooks create on-file-change "./lint.sh"
/hooks create on-file-change "./test.sh"

# All three will run in order
```

### Hook Variables

Hooks receive context variables:

- `{file}` - File path (on-file-change)
- `{tool}` - Tool name (pre/post-tool-call)
- `{args}` - Tool arguments (pre/post-tool-call)
- `{error}` - Error message (on-error)
- `{session}` - Session ID (all events)

**Example:**

```bash
/hooks create on-file-change "echo 'File changed: {file}'"
```

---

## Best Practices

### Hooks Panel UI Best Practices

**Organizing Hooks:**

- Keep related hooks in the same category
- Use descriptive names for easy identification
- Disable hooks you're not currently using
- Test hooks before enabling them

**Navigation Efficiency:**

- Use **↑** from first hook to quickly reach Exit
- Remember keyboard shortcuts (A/E/D/T)
- Use **Esc** for quick exit to main navigation
- Navigate by category to find hooks faster

**Hook Management:**

- Review enabled hooks regularly
- Test hooks after editing
- Delete unused custom hooks
- Keep hook list under 50 for best performance

**Safety:**

- Always test hooks before enabling
- Review hook details before toggling
- Be cautious with built-in hooks
- Disable hooks if they cause issues

### Hook Design

**Keep hooks simple:**

- One responsibility per hook
- Fast execution (< 1 second)
- Clear error messages
- Idempotent operations

**Use appropriate events:**

- `pre-*` for validation
- `post-*` for logging
- `on-*` for automation

**Handle errors gracefully:**

- Return proper JSON even on error
- Include helpful error messages
- Don't crash on invalid input

### Hook Organization

**Project structure:**

```
project/
├── .ollm/
│   └── hooks/
│       ├── format-on-save.sh
│       ├── lint-on-save.sh
│       ├── test-pre-commit.sh
│       └── README.md
└── ...
```

**Document your hooks:**

- Add comments to hook code
- Create README for hook directory
- Document required dependencies

**Version control:**

- Commit hooks to repository
- Share with team
- Track changes

---

## Examples

### Hooks Panel UI Examples

#### Example 1: Quick Hook Toggle

**Scenario:** You want to temporarily disable formatting while debugging.

**Steps:**

1. Press **Tab** to navigate to Hooks
2. Press **Enter** to open Hooks Panel
3. Navigate to "format-on-save" hook with **↓**
4. Press **Enter** to disable (○)
5. Do your debugging work
6. Return to Hooks Panel
7. Press **Enter** to re-enable (●)

**Result:** Formatting is temporarily disabled without deleting the hook.

#### Example 2: Adding a Test Hook

**Scenario:** You want to run tests before every commit.

**Steps:**

1. Open Hooks Panel (**Tab** → **Enter**)
2. Press **A** to open Add dialog
3. Enter details:
   ```
   Name: test-before-commit
   Command: npm
   Arguments: test
   ```
4. Press **S** to save
5. Navigate to new hook
6. Press **T** to test it
7. If successful, press **Enter** to enable

**Result:** Tests run automatically before commits.

#### Example 3: Reviewing All Hooks

**Scenario:** You want to see what hooks are currently active.

**Steps:**

1. Open Hooks Panel
2. Navigate through each category with **↓**
3. Look for ● (enabled) indicators
4. Select each enabled hook to view details
5. Note the command and arguments

**Result:** You have a complete overview of active automation.

#### Example 4: Cleaning Up Hooks

**Scenario:** You have too many hooks and want to clean up.

**Steps:**

1. Open Hooks Panel
2. Navigate through all hooks
3. For each unused hook:
   - Press **Enter** to disable (○)
   - Or press **D** to delete (user hooks only)
4. Keep only the hooks you actively use

**Result:** Cleaner hook list, better performance.

#### Example 5: Testing Hook Configuration

**Scenario:** You edited a hook file manually and want to verify it works.

**Steps:**

1. Open Hooks Panel
2. Navigate to the edited hook
3. Press **T** to test
4. Review test results:
   - ✓ Success: Hook works correctly
   - ✗ Failure: Check error message
5. If failed, press **E** to edit and fix
6. Test again until successful

**Result:** Verified working hook configuration.

### Example 1: Format on Save

```bash
#!/bin/bash
# .ollm/hooks/format-on-save.sh

input=$(cat)
file=$(echo "$input" | jq -r '.context.file')

# Format file
prettier --write "$file" 2>&1

if [ $? -eq 0 ]; then
  echo '{"allow": true, "message": "File formatted successfully"}'
else
  echo '{"allow": false, "message": "Format failed"}'
fi
```

**Create hook:**

```bash
/hooks create on-file-change ./.ollm/hooks/format-on-save.sh
/hooks trust on-file-change
```

### Example 2: Safety Check

```bash
#!/bin/bash
# .ollm/hooks/safety-check.sh

input=$(cat)
tool=$(echo "$input" | jq -r '.context.tool')
args=$(echo "$input" | jq -r '.context.args')

# Block dangerous operations
if [[ "$tool" == "shell" ]] && [[ "$args" == *"rm -rf"* ]]; then
  echo '{"allow": false, "message": "Dangerous operation blocked"}'
  exit 0
fi

echo '{"allow": true, "message": "Operation approved"}'
```

**Create hook:**

```bash
/hooks create pre-tool-call ./.ollm/hooks/safety-check.sh
/hooks trust pre-tool-call
```

### Example 3: Test Before Commit

```bash
#!/bin/bash
# .ollm/hooks/test-pre-commit.sh

# Run tests
npm test 2>&1

if [ $? -eq 0 ]; then
  echo '{"allow": true, "message": "All tests passed"}'
else
  echo '{"allow": false, "message": "Tests failed, commit blocked"}'
fi
```

**Create hook:**

```bash
/hooks create on-git-commit ./.ollm/hooks/test-pre-commit.sh
/hooks trust on-git-commit
```

---

## Frequently Asked Questions

### Hooks Panel UI

**Q: How do I access the Hooks Panel?**  
A: Press **Tab** repeatedly to navigate to the "Hooks" tab in the main navigation bar, then press **Enter** to activate the panel.

**Q: What's the difference between ● and ○ indicators?**  
A: ● (green) means the hook is enabled and will execute. ○ (gray) means the hook is disabled and won't execute.

**Q: Can I edit built-in hooks?**  
A: No, built-in hooks are read-only. You can disable them, but you cannot edit or delete them. Only user-created hooks can be edited or deleted.

**Q: Why can't I delete a hook?**  
A: Built-in hooks and extension hooks cannot be deleted. Only user-created hooks (stored in `~/.ollm/hooks/`) can be deleted.

**Q: How do I know if a hook is working?**  
A: Use the Test feature (press **T** on a hook) to simulate execution and see if it works correctly.

**Q: What happens when I toggle a hook?**  
A: The hook's enabled/disabled state is immediately saved to `~/.ollm/settings.json`. Enabled hooks will execute on their trigger events; disabled hooks won't.

**Q: Can I have multiple hooks for the same event?**  
A: Yes! Multiple hooks can be enabled for the same event type. They will execute in the order they appear in the list.

**Q: Why do I see a "corrupted hooks" warning?**  
A: Some hook files in `~/.ollm/hooks/` have invalid JSON syntax. Check the files and fix any syntax errors. Corrupted hooks are skipped during loading.

**Q: How many hooks can I have?**  
A: There's no hard limit, but the panel is optimized for up to 50 hooks. With more hooks, you may experience slower navigation.

**Q: Can I import/export hooks?**  
A: Not yet through the UI. You can manually copy hook files from `~/.ollm/hooks/` to share them. Import/export functionality is planned for a future release.

**Q: What's the Exit item for?**  
A: The Exit item at the top of the list provides a quick way to return to the main navigation. Press **Enter** on it, or press **Esc** or **0** from anywhere in the panel.

**Q: Why don't I see scroll indicators?**  
A: Scroll indicators (▲/▼) only appear when you have more than 15 hooks. They show automatically when the list is longer than the visible window.

**Q: Can I search for hooks?**  
A: Not yet. Hook search/filtering is planned for a future release. For now, navigate through categories to find hooks.

**Q: How do I create a hook for a specific file type?**  
A: When adding a hook, use file patterns in the arguments. For example: `prettier --write *.ts` for TypeScript files only.

**Q: What's the difference between user and extension hooks?**  
A: User hooks are created by you and stored in `~/.ollm/hooks/`. Extension hooks come from installed extensions and are managed by the extension system.

### General Hooks

**Q: What are hooks?**  
A: Hooks are executables that run in response to events during LLM execution. They enable workflow automation, safety gates, and custom logic.

**Q: When should I use hooks?**  
A: Use hooks for automating repetitive tasks, validating operations, formatting code, running tests, or integrating with external systems.

**Q: Are hooks secure?**  
A: Hooks have trust levels. Built-in hooks are trusted. User hooks require approval the first time. Downloaded hooks require approval each time. Always review hook code before trusting.

**Q: Do hooks slow down execution?**  
A: Hooks add some overhead. Keep hooks fast (< 1 second) and disable unused hooks for best performance.

**Q: Can hooks modify files?**  
A: Yes, hooks can modify files if they have the necessary permissions. Be careful with hooks that perform destructive operations.

**Q: How do I debug a failing hook?**  
A: Enable debug mode with `/hooks debug on`, then check the debug output with `/hooks debug status`. You can also test hooks manually from the command line.

---

## Further Reading

### Documentation

- [Hook System Overview](3%20projects/OLLM%20CLI/Hooks/README.md) - Introduction to hooks
- [Hook Development Guide](3%20projects/OLLM%20CLI/Hooks/development-guide.md) - Creating custom hooks
- [Hook Protocol](protocol.md) - Technical specification
- [MCP Commands](MCP_commands.md) - Hook commands

### Related Features

- [Extensions](../extensions/) - Extensions can include hooks
- [MCP Servers](../servers/) - MCP servers for tools
- [API Reference](hook-system.md) - Hook system API

### External Resources

- Bash Scripting Guide (https://www.gnu.org/software/bash/manual/)
- jq Manual (https://stedolan.github.io/jq/manual/)
- JSON Specification (https://www.json.org/)

---

**Last Updated:** 2026-01-18  
**Version:** 0.1.6  
**Features:** Hooks Panel UI added in v0.1.6  
**Next:** [Hook Development Guide](3%20projects/OLLM%20CLI/Hooks/development-guide.md)

<!-- Alpha release v0.1.6 -->

