# Keyboard Shortcuts Reference

**Complete Guide to Keyboard Shortcuts in OLLM CLI**

---

## Table of Contents

1. [Overview](#overview)
2. [Tab Navigation](#tab-navigation)
3. [Layout Controls](#layout-controls)
4. [Chat Interaction](#chat-interaction)
5. [Review Mode](#review-mode)
6. [Navigation](#navigation)
7. [File Explorer](#file-explorer)
8. [Terminal](#terminal)
9. [Global Focus Management](#global-focus-management)
10. [Customization](#customization)

**See Also:**

- [UI Guide](UIGuide.md) - Complete UI documentation
- [Slash Commands](Commands.md) - Command reference
- [Themes](Themes.md) - Theme customization

---

## Overview

OLLM CLI provides comprehensive keyboard shortcuts for efficient navigation and interaction. All keybinds are customizable through the Settings UI or configuration file.

### Keybind Format

Keybinds use the following format:

**Modifiers:**

- `ctrl` - Control key
- `shift` - Shift key
- `alt` - Alt key (Option on macOS)
- `meta` - Meta key (Command on macOS, Windows key on Windows)

**Special Keys:**

- `return` - Enter key
- `escape` - Escape key
- `tab` - Tab key
- `backspace` - Backspace key
- `space` - Space bar
- `up`, `down`, `left`, `right` - Arrow keys

**Format:** `[modifier+][modifier+]key`

**Examples:**

- `ctrl+s` - Control + S
- `shift+return` - Shift + Enter
- `ctrl+shift+tab` - Control + Shift + Tab

---

## Tab Navigation

Switch between main application tabs using Ctrl + Number.

| Keybind  | Action       | Description                  |
| -------- | ------------ | ---------------------------- |
| `Ctrl+1` | Chat Tab     | Switch to chat view          |
| `Ctrl+2` | Tools Tab    | Switch to tools panel        |
| `Ctrl+3` | Hooks Tab    | Switch to hooks panel        |
| `Ctrl+4` | Files Tab    | Switch to file explorer      |
| `Ctrl+5` | Search Tab   | Switch to search panel       |
| `Ctrl+6` | Docs Tab     | Switch to documentation      |
| `Ctrl+7` | GitHub Tab   | Switch to GitHub integration |
| `Ctrl+8` | MCP Tab      | Switch to MCP servers        |
| `Ctrl+9` | Settings Tab | Switch to settings           |

---

## Layout Controls

Global layout and panel management.

| Keybind          | Action              | Description                  |
| ---------------- | ------------------- | ---------------------------- |
| `Ctrl+P`         | Toggle Panel        | Toggle side panel visibility |
| `Ctrl+K`         | Command Palette     | Open command palette         |
| `Ctrl+/`         | Toggle Debug        | Toggle debug mode            |
| `Ctrl+Shift+Tab` | Switch Window Left  | Switch to previous window    |
| `Ctrl+Tab`       | Switch Window Right | Switch to next window        |

---

## Chat Interaction

Shortcuts for chat input and session management.

| Keybind        | Action        | Description                |
| -------------- | ------------- | -------------------------- |
| `Return`       | Send Message  | Send message to LLM        |
| `Shift+Return` | New Line      | Insert new line in message |
| `Ctrl+L`       | Clear Chat    | Clear chat history         |
| `Ctrl+S`       | Save Session  | Save current session       |
| `Escape`       | Cancel        | Cancel current operation   |
| `Up`           | Edit Previous | Edit previous message      |

---

## Review Mode

Code diff review shortcuts.

| Keybind | Action  | Description          |
| ------- | ------- | -------------------- |
| `Y`     | Approve | Approve current diff |
| `N`     | Reject  | Reject current diff  |

---

## Navigation

Standard navigation keys with Vim-style alternatives.

| Keybind     | Action      | Description                 |
| ----------- | ----------- | --------------------------- |
| `J`         | Scroll Down | Scroll down (Vim-style)     |
| `K`         | Scroll Up   | Scroll up (Vim-style)       |
| `Return`    | Select      | Select current item         |
| `Backspace` | Back        | Go back                     |
| `Tab`       | Cycle Focus | Cycle focus forward         |
| `Left`      | Move Left   | Move cursor/selection left  |
| `Right`     | Move Right  | Move cursor/selection right |
| `Up`        | Move Up     | Move cursor/selection up    |
| `Down`      | Move Down   | Move cursor/selection down  |

---

## File Explorer

File tree navigation and operations.

| Keybind   | Action         | Description                     |
| --------- | -------------- | ------------------------------- |
| `O`       | Open File      | Open selected file              |
| `F`       | Focus Explorer | Focus file explorer             |
| `E`       | Edit File      | Edit selected file              |
| `R`       | Rename         | Rename file/folder              |
| `D`       | Delete         | Delete file/folder              |
| `C`       | Copy Path      | Copy file path to clipboard     |
| `J`       | Move Down      | Move selection down (Vim-style) |
| `K`       | Move Up        | Move selection up (Vim-style)   |
| `H`       | Collapse       | Collapse folder (Vim-style)     |
| `L`       | Expand         | Expand folder (Vim-style)       |
| `Shift+F` | Toggle Follow  | Toggle follow active file       |
| `?`       | Toggle Help    | Toggle help overlay             |
| `Return`  | Select         | Select/open item                |
| `P`       | Quick Open     | Quick file open                 |
| `A`       | Actions Menu   | Open actions menu               |

---

## Terminal

Terminal interaction shortcuts.

| Keybind     | Action       | Description                       |
| ----------- | ------------ | --------------------------------- |
| `Up`        | Scroll Up    | Scroll terminal up                |
| `Down`      | Scroll Down  | Scroll terminal down              |
| `Ctrl+Up`   | History Up   | Navigate command history up       |
| `Ctrl+Down` | History Down | Navigate command history down     |
| `Ctrl+C`    | Interrupt    | Send interrupt signal (hardcoded) |

---

## Global Focus Management

Focus management across the application.

| Keybind      | Action           | Description                     |
| ------------ | ---------------- | ------------------------------- |
| `Ctrl+Space` | Focus Chat Input | Focus chat input field          |
| `Ctrl+M`     | Focus Navigation | Focus navigation panel          |
| `Ctrl+C`     | Focus Context    | Focus context panel             |
| `Ctrl+F`     | Focus File Tree  | Focus file explorer             |
| `Ctrl+I`     | Focus Functions  | Focus functions panel           |
| `Tab`        | Cycle Next       | Cycle focus to next element     |
| `Shift+Tab`  | Cycle Previous   | Cycle focus to previous element |

---

## Customization

### Configuration File

**Location:** `~/.ollm/user_keybinds.json`

**Format:**

```json
{
  "tabNavigation": {
    "tabChat": "ctrl+1",
    "tabTools": "ctrl+2"
  },
  "chat": {
    "send": "return",
    "newline": "shift+return"
  },
  "navigation": {
    "scrollDown": "j",
    "scrollUp": "k"
  }
}
```

### Customization via Settings UI

1. Open Settings: `Ctrl+9` or `/settings`
2. Navigate to Keybinds section
3. Click on keybind to edit
4. Press new key combination
5. Save changes

### Reset to Defaults

Use the "Reset All" button in Settings UI or delete `~/.ollm/user_keybinds.json`.

---

## Known Issues & Conflicts

### Potential Conflicts

**Terminal Conflicts:**

- Some terminal emulators intercept certain key combinations
- `Ctrl+C` is hardcoded for interrupt (cannot be changed)
- `Ctrl+Z` may be intercepted by terminal for suspend

**System Conflicts:**

- `Ctrl+Tab` may be intercepted by browser/OS for tab switching
- `Ctrl+W` may close terminal window
- `Ctrl+Q` may quit terminal application

**Internal Conflicts:**

- Some keybinds may conflict with each other
- Focus context determines which keybind takes precedence

### Hardcoded Keybinds

**Known Hardcoded Keybinds:**

- `Ctrl+C` - Terminal interrupt (in `TerminalContext.tsx`)
- Some `useInput` hooks may have hardcoded keys

---

## Examples

### Custom Keybinds

**Example 1: Vim-style navigation everywhere**

```json
{
  "navigation": {
    "scrollDown": "j",
    "scrollUp": "k",
    "left": "h",
    "right": "l"
  },
  "fileExplorer": {
    "moveDown": "j",
    "moveUp": "k",
    "collapse": "h",
    "expand": "l"
  }
}
```

**Example 2: Emacs-style shortcuts**

```json
{
  "chat": {
    "send": "ctrl+return",
    "newline": "return"
  },
  "navigation": {
    "moveUp": "ctrl+p",
    "moveDown": "ctrl+n",
    "left": "ctrl+b",
    "right": "ctrl+f"
  }
}
```

**Example 3: Custom tab navigation**

```json
{
  "tabNavigation": {
    "tabChat": "alt+1",
    "tabTools": "alt+2",
    "tabFiles": "alt+3"
  }
}
```

---

## Best Practices

### For Users

1. **Avoid system conflicts** - Don't override system shortcuts
2. **Test after changes** - Verify keybinds work as expected
3. **Keep backups** - Save `user_keybinds.json` before major changes
4. **Report conflicts** - Help identify problematic keybinds
5. **Use Settings UI** - Easier than manual JSON editing

### For Developers

1. **Always use KeybindsContext** - Don't hardcode keybinds
2. **Check focus context** - Keybinds should respect focus
3. **Document conflicts** - Note any known conflicts
4. **Test on multiple platforms** - Windows, macOS, Linux
5. **Provide alternatives** - Offer mouse/menu alternatives

---

## Troubleshooting

### Keybind Not Working

1. Check if keybind conflicts with terminal/system
2. Check if correct focus context
3. Check if keybind is hardcoded elsewhere
4. Try different key combination
5. Check console for errors

### Keybind Conflicts

1. Open Settings → Keybinds
2. Look for duplicate keybinds
3. Change one of the conflicting keybinds
4. Save and test

### Reset Not Working

1. Delete `~/.ollm/user_keybinds.json` manually
2. Restart application
3. Defaults will be restored

---

## File Locations

| File                                           | Purpose                        |
| ---------------------------------------------- | ------------------------------ |
| `packages/cli/src/config/keybinds.ts`          | Default keybind definitions    |
| `packages/cli/src/services/KeybindsService.ts` | Keybind loading/saving service |
| `~/.ollm/user_keybinds.json`                   | User keybind overrides         |

---

**Last Updated:** 2026-01-26  
**Version:** 0.1.0  
**Next:** [UI Guide](UIGuide.md)
