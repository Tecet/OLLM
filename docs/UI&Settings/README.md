# User Interface Documentation

**Last Updated:** January 26, 2026

Welcome to the OLLM CLI User Interface documentation. This section covers all aspects of the terminal-based user interface, including layout, components, commands, themes, and terminal integration.

---

## Documentation Index

### Core UI Documentation

- **[UI Guide](./UIGuide.md)** - Complete guide to the main interface layout and components
  - Header bar and status display
  - Chat view and message rendering
  - Side panel and tabs
  - Focus management and navigation

- **[Commands Reference](./Commands.md)** - Complete slash command reference
  - Session management commands
  - Context and model commands
  - MCP and extension commands
  - Git and workflow commands

- **[Keyboard Shortcuts](./keybinds.md)** - All keyboard shortcuts and keybindings
  - Global shortcuts
  - Chat interaction keys
  - Navigation and focus management
  - Tab and panel controls

- **[Themes Guide](./Themes.md)** - Theme system and customization
  - Built-in themes overview
  - Theme management commands
  - Color palette reference
  - Custom theme creation

- **[Terminal Guide](./Terminal.md)** - Integrated terminal system
  - Terminal architecture
  - PTY integration
  - ANSI rendering
  - Terminal commands and controls

- **[Configuration](./configuration.md)** - UI configuration options
  - Layout settings
  - Display preferences
  - Theme configuration
  - Performance tuning

---

## Quick Start

### Basic Navigation

- `Ctrl+P` - Toggle side panel
- `Ctrl+1-9` - Switch between tabs
- `Tab` / `Shift+Tab` - Cycle focus
- `Ctrl+K` - Open command palette

### Essential Commands

- `/help` - Show help information
- `/model list` - List available models
- `/theme list` - List available themes
- `/session save <name>` - Save current session

### Common Workflows

1. **Starting a Chat**
   - Type your message in the input area
   - Press `Return` to send
   - Use `Shift+Return` for new lines

2. **Managing Context**
   - Use `/context stats` to check usage
   - Use `/compact` to compress context
   - Use `/snapshot create` to save state

3. **Customizing Appearance**
   - Use `/theme list` to see themes
   - Use `/theme use <name>` to switch
   - Settings persist across sessions

---

## UI Architecture

### Layout Structure

```
┌─────────────────────────────────────────────────────────────────┐
│ HEADER BAR                                                       │
│ Model | Context | Mode | Clock                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ ┌─────────────────────┬─────────────────────────────────────┐  │
│ │                     │                                      │  │
│ │   MAIN CONTENT      │   SIDE PANEL (Optional)             │  │
│ │   (Chat/Tabs)       │   (Tools/Hooks/Files/MCP/Settings)  │  │
│ │                     │                                      │  │
│ └─────────────────────┴─────────────────────────────────────┘  │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ STATUS BAR                                                       │
│ Status | Keybind Hints | Notifications                          │
└─────────────────────────────────────────────────────────────────┘
```

### Component Hierarchy

- **App Container** - Root component
  - **Header Bar** - Model, context, mode display
  - **Main Content Area**
    - **Chat View** - Message history and input
    - **Side Panel** - Tabs for tools, hooks, files, etc.
  - **Status Bar** - Status indicators and hints

---

## Key Features

### Interactive Chat

- Real-time streaming responses
- Syntax highlighting for code blocks
- Tool call visualization
- Collapsible reasoning blocks
- Message history with scrollback

### Side Panel Tabs

- **Tools** - Manage available tools
- **Hooks** - Configure automation hooks
- **Files** - Browse project files
- **MCP** - Manage MCP servers
- **Settings** - Configure preferences

### Context Management

- Real-time token usage display
- Visual indicators for context limits
- Automatic compression when needed
- Snapshot creation and restoration

### Theme System

- 6 built-in themes
- Instant theme switching
- Persistent theme selection
- Custom theme support (planned)

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

## Performance

### Rendering Optimization

- Efficient React reconciliation
- Virtual scrolling for long lists
- Message pagination
- Lazy loading for large files

### Memory Management

- Limited message history in memory
- Older messages in session storage
- Compression for long conversations
- Automatic cleanup

---

## Platform Support

### Cross-Platform Compatibility

- **Windows** - Full support with PowerShell
- **macOS** - Full support with bash/zsh
- **Linux** - Full support with bash

### Terminal Requirements

- **Minimum Size:** 80x24 (columns x rows)
- **Recommended:** 120x40
- **Color Support:** 256 colors recommended
- **Unicode Support:** Required for icons

---

## Related Documentation

### Core Systems

- [Context Management](../Context/ContextManagment.md) - Context system architecture
- [MCP Integration](../MCP/MCP_Index.md) - Model Context Protocol
- [Hooks System](../Hooks/UserGuide.md) - Automation hooks
- [Tools System](../Tools/UserGuide.md) - Tool management

### Developer Resources

- [UI Components](../../dev/docs/knowledgeDB/dev_UI_Front.md) - Component architecture
- [Theme System](../../dev/docs/knowledgeDB/dev_UI_Themes.md) - Theme implementation
- [Terminal System](../../dev/docs/knowledgeDB/dev_Terminal.md) - Terminal architecture

---

## Getting Help

### In-App Help

- `/help` - General help
- `/help <command>` - Command-specific help
- `Ctrl+?` - Show keyboard shortcuts

### Documentation

- Read the [UI Guide](./UIGuide.md) for detailed interface documentation
- Check [Commands Reference](./Commands.md) for all available commands
- Review [Keyboard Shortcuts](./keybinds.md) for navigation tips

### Community

- Report issues on GitHub
- Join community discussions
- Contribute improvements

---

## Tips and Tricks

### Productivity Tips

1. **Use Keyboard Shortcuts** - Learn the essential shortcuts for faster navigation
2. **Save Sessions** - Use `/session save` to preserve your work
3. **Create Snapshots** - Use `/snapshot create` before major changes
4. **Customize Themes** - Find a theme that works for your eyes
5. **Learn Commands** - Master slash commands for quick actions

### Performance Tips

1. **Compress Context** - Use `/compact` when context gets large
2. **Close Side Panel** - Use `Ctrl+P` to hide panel when not needed
3. **Limit History** - Clear old messages with `/clear`
4. **Use Smaller Models** - Switch to faster models for simple tasks

### Workflow Tips

1. **Use Modes** - Switch modes with `/mode` for specialized assistance
2. **Enable Hooks** - Automate repetitive tasks with hooks
3. **Organize Sessions** - Name sessions descriptively for easy resumption
4. **Review Diffs** - Enable diff review for safer file modifications

---

**Last Updated:** January 26, 2026

<!-- Alpha Release: v0.1.6 -->

