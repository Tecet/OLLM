# User Interface Guide

**Last Updated:** January 26, 2026

This guide provides a comprehensive overview of the OLLM CLI terminal user interface, including layout structure, components, and interaction patterns.

---

## Overview

OLLM CLI features a terminal-based interface built with React + Ink. The UI consists of three main areas: Header Bar, Main Content Area (with optional Side Panel), and Status Bar.

---

## Layout Structure

```
┌─────────────────────────────────────────────────────────────────┐
│ HEADER BAR (SystemBar)                                          │
│ Model | Context | Mode | Clock                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ ┌─────────────────────┬─────────────────────────────────────┐  │
│ │                     │                                      │  │
│ │   MAIN CONTENT      │   SIDE PANEL (Optional)             │  │
│ │   (Chat/Tabs)       │   (Tools/Hooks/Files/MCP/Settings)  │  │
│ │                     │                                      │  │
│ │   - Chat History    │   - Tab Navigation                  │  │
│ │   - Input Area      │   - Tab Content                     │  │
│ │                     │   - Actions                         │  │
│ │                     │                                      │  │
│ └─────────────────────┴─────────────────────────────────────┘  │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ STATUS BAR                                                       │
│ Status | Keybind Hints | Notifications                          │
└─────────────────────────────────────────────────────────────────┘
```

---

## Header Bar

The header bar displays critical information about your current session.

### Layout

```
┌─────────────────────────────────────────────────────────────────┐
│ 🦙 llama3:8b │ 5.2K/13.9K │ 🎯 Assistant │ 🕐 14:32:15        │
└─────────────────────────────────────────────────────────────────┘
```

### Elements

**Model Display**

- Shows currently active model
- Icon indicates model type
- Click or use `/model` to switch

**Context Usage**

- Format: `{current}/{max}` tokens
- Example: `5,234/13,926`
- Color coding:
  - 🟢 Green: < 60% usage
  - 🟡 Yellow: 60-80% usage
  - 🔴 Red: > 80% usage

**Mode Indicator**

- Shows current operational mode
- Examples: Assistant, Developer, Planning
- Use `/mode` to switch modes

**Clock**

- Displays current time
- Updates every second

---

## Main Content Area

### Chat View

The left column displays your conversation with the AI.

#### Chat History

**Features:**

- Scrollable message list
- Auto-scroll to bottom on new messages
- Syntax highlighting for code blocks
- Tool call visualization
- Collapsible reasoning blocks

**Message Types:**

1. **User Messages**
   - Blue text, left-aligned
   - Your input to the AI

2. **Assistant Messages**
   - White text, left-aligned
   - AI responses

3. **System Messages**
   - Gray text, italic
   - System notifications

4. **Tool Calls**
   - Yellow box with tool name and result
   - Shows tool execution details

**Tool Call Display:**

```
┌─ Tool: glob ─────────────────────────┐
│ Pattern: **/*.ts                      │
│ Result: Found 42 files                │
│ - src/index.ts                        │
│ - src/utils.ts                        │
│ - ...                                 │
└───────────────────────────────────────┘
```

#### Reasoning Blocks

For models that support reasoning (e.g., DeepSeek R1), the UI displays the thinking process.

**States:**

1. **Streaming (Expanded)**
   - Shows full reasoning content
   - Content streams in real-time
   - Lets you see the thinking process

2. **Complete (Collapsed)**
   - Auto-collapses to summary view
   - Shows "Reasoning: (collapsed)"
   - Saves screen space

3. **Historical Messages**
   - Completed reasoning starts collapsed
   - Shows summary line only

**Controls:**

- Navigate to reasoning block with arrow keys
- Press `Space` or `Return` to toggle expand/collapse

#### Input Area

**Features:**

- Multi-line text input
- Auto-resize based on content
- Keybind hints
- Character/token count (optional)

**Keybinds:**

- `Return` - Send message
- `Shift+Return` - New line
- `Escape` - Cancel/clear
- `Up` - Edit previous message

---

### Side Panel

The right column provides access to tools, settings, and project information.

**Toggle:** Press `Ctrl+P` to show/hide the side panel

#### Tab Bar

Navigate between different side panel views:

```
[Tools] [Hooks] [Files] [MCP] [Settings]
  ^^^^
  Active (highlighted)
```

**Available Tabs:**

1. **Tools** (`Ctrl+2`) - Tool management
2. **Hooks** (`Ctrl+3`) - Hook management
3. **Files** (`Ctrl+4`) - File explorer
4. **Search** (`Ctrl+5`) - Search panel
5. **Docs** (`Ctrl+6`) - Documentation
6. **GitHub** (`Ctrl+7`) - GitHub integration
7. **MCP** (`Ctrl+8`) - MCP servers
8. **Settings** (`Ctrl+9`) - Settings

**Navigation:**

- `Ctrl+1-9` - Switch tabs directly
- `Tab` - Cycle through tabs

#### Tools Tab

Manage available tools and their permissions.

```
┌─ Tools ──────────────────────────────────┐
│                                           │
│ File Operations                           │
│ ☑ read_file    ☑ write_file              │
│ ☑ edit_file    ☑ glob                    │
│                                           │
│ Search                                    │
│ ☑ grep         ☑ web_search              │
│                                           │
│ Execution                                 │
│ ☑ shell        ☑ web_fetch               │
│                                           │
│ MCP Tools (github)                        │
│ ☑ create_issue ☑ list_repos              │
│                                           │
└───────────────────────────────────────────┘
```

**Features:**

- Tool categories (collapsible)
- Enable/disable toggles
- Tool descriptions on hover
- MCP tools integration

#### Hooks Tab

Configure automation hooks for event-driven actions.

```
┌─ Hooks ──────────────────────────────────┐
│                                           │
│ File Events                               │
│ ✓ lint-on-save (fileEdited)              │
│ ✓ format-on-save (fileEdited)            │
│                                           │
│ Agent Events                              │
│ ○ log-prompts (promptSubmit)             │
│                                           │
│ [+ Add Hook] [Debug: OFF]                │
│                                           │
└───────────────────────────────────────────┘
```

**Features:**

- Hook list by category
- Enable/disable toggles
- Add/edit/delete hooks
- Debug mode toggle

#### Files Tab

Browse and manage project files.

```
┌─ Files ──────────────────────────────────┐
│ 📁 /home/user/project                    │
│                                           │
│ ▼ 📁 src                                  │
│   ▼ 📁 components                         │
│     📄 Button.tsx                         │
│     📄 Input.tsx                          │
│   📄 index.ts                             │
│ ▼ 📁 tests                                │
│   📄 app.test.ts                          │
│                                           │
│ [o]pen [e]dit [r]ename [d]elete [?]help  │
└───────────────────────────────────────────┘
```

**Features:**

- Tree view with expand/collapse
- File operations (open, edit, rename, delete)
- Git status indicators
- Quick open (`P`)
- Follow active file mode

#### MCP Tab

Manage Model Context Protocol servers.

```
┌─ MCP Servers ────────────────────────────┐
│                                           │
│ Installed Servers                         │
│ ✓ github (connected) 🟢                  │
│   Tools: 12 | Resources: 5               │
│   [View] [Restart] [Configure]           │
│                                           │
│ ○ slack (disconnected) 🔴                │
│   [Connect] [Configure]                   │
│                                           │
│ [+ Install Server] [Marketplace]          │
│                                           │
└───────────────────────────────────────────┘
```

**Features:**

- Server list with status
- Health indicators
- Server actions (restart, configure)
- Marketplace integration
- OAuth status

#### Settings Tab

Configure OLLM CLI preferences.

```
┌─ Settings ───────────────────────────────┐
│                                           │
│ Model                                     │
│ [llama3:8b ▼]                            │
│                                           │
│ Context Size                              │
│ [16384 ▼] (2K, 4K, 8K, 16K, 32K, 64K)   │
│                                           │
│ Theme                                     │
│ [default-dark ▼]                         │
│                                           │
│ Provider                                  │
│ [Ollama ▼]                               │
│                                           │
│ [Save] [Reset to Defaults]               │
│                                           │
└───────────────────────────────────────────┘
```

**Features:**

- Model picker
- Context size selector
- Theme picker
- Provider selector
- Keybinds editor (planned)

---

## Status Bar

The status bar displays current status and helpful hints.

```
┌─────────────────────────────────────────────────────────────────┐
│ ● Ready | Ctrl+K: Commands | Ctrl+P: Panel | Ctrl+/: Debug     │
└─────────────────────────────────────────────────────────────────┘
```

### Elements

**Status Indicator**

- 🟢 Ready - System ready for input
- 🟡 Thinking - AI processing
- 🔴 Error - Error occurred
- 🔵 Info - Information message

**Keybind Hints**

- Context-sensitive shortcuts
- Updates based on current focus
- Shows most relevant actions

**Notifications**

- Temporary messages
- Success/warning/error alerts
- Auto-dismiss after timeout

---

## Focus Management

OLLM CLI uses a focus system to route keyboard input to the appropriate component.

### Focusable Elements

1. **Chat Input** - Default focus
2. **Side Panel Tabs** - Tab navigation
3. **Side Panel Content** - Tab-specific content
4. **File Explorer** - File tree navigation
5. **Dialogs** - Modal dialogs when open

### Focus Indicators

- Border highlight (theme-dependent)
- Cursor visibility
- Keybind hints update

### Focus Navigation

- `Tab` - Cycle focus forward
- `Shift+Tab` - Cycle focus backward
- `Ctrl+Space` - Focus chat input
- `Ctrl+M` - Focus navigation

---

## Responsive Behavior

### Terminal Size Adaptation

**Minimum Size:** 80x24 (columns x rows)  
**Recommended:** 120x40

**Behavior:**

- Side panel auto-hides on narrow terminals (< 100 columns)
- Chat history scrolls on short terminals (< 30 rows)
- Input area shrinks on very short terminals

### Overflow Handling

**Chat History:**

- Vertical scroll enabled
- No horizontal scroll (text wraps)

**Side Panel Content:**

- Vertical scroll enabled
- Long lines wrap

**Input Area:**

- Scrolls if exceeds max height
- Long lines wrap

---

## Theme Support

OLLM CLI includes 6 built-in themes with customizable colors.

### Built-in Themes

1. **Solarized Dark** (default) - Precision theme for reduced eye strain
2. **Neon Dark** - Balanced dark theme
3. **Dracula Dark** - High-contrast purple theme
4. **Nord Dark** - Arctic, north-bluish palette
5. **Monokai Dark** - Vibrant, high-contrast theme
6. **Solarized Dark 2** - Alternative Solarized variant

### Theme Management

**Commands:**

- `/theme list` - List available themes
- `/theme use <name>` - Switch to a theme
- `/theme preview <name>` - Preview temporarily

**Settings:**

- Theme selection persists across sessions
- Stored in `~/.ollm/settings.json`

See [Themes Guide](./Themes.md) for detailed theme documentation.

---

## Accessibility

### Keyboard-First Design

- All features accessible via keyboard
- No mouse required
- Vim-style alternatives available
- Clear focus indicators

### Screen Reader Support

- Text-based UI (no graphics)
- ANSI escape codes for formatting
- Clear status messages
- Semantic structure

### Color Blindness Support

- Status indicators use symbols + colors
- High contrast themes available
- Customizable color schemes

---

## Performance Optimization

### Rendering

- Efficient React reconciliation
- Only re-renders changed components
- Virtual scrolling for long lists
- Message pagination

### Memory Management

- Keep last 100 messages in memory
- Older messages in session storage
- Compression for long conversations
- Automatic cleanup

---

## Tips and Tricks

### Productivity

1. **Learn Keyboard Shortcuts** - Master essential shortcuts for faster navigation
2. **Use Side Panel** - Keep tools and settings accessible
3. **Customize Theme** - Find a theme that works for your eyes
4. **Save Sessions** - Preserve your work with `/session save`

### Navigation

1. **Quick Tab Switching** - Use `Ctrl+1-9` to jump to tabs
2. **Focus Management** - Use `Tab` to cycle through focusable elements
3. **Chat Input** - Press `Ctrl+Space` to quickly return to input
4. **Panel Toggle** - Use `Ctrl+P` to show/hide side panel

### Context Management

1. **Monitor Usage** - Watch the context indicator in header
2. **Compress Early** - Use `/compact` before hitting limits
3. **Create Snapshots** - Use `/snapshot create` before major changes
4. **Clear History** - Use `/clear` to remove old messages

---

## Related Documentation

- [Commands Reference](./Commands.md) - All slash commands
- [Keyboard Shortcuts](./keybinds.md) - Complete keybind reference
- [Themes Guide](./Themes.md) - Theme system documentation
- [Terminal Guide](./Terminal.md) - Integrated terminal system
- [Configuration](./configuration.md) - UI configuration options

---

**Last Updated:** January 26, 2026
