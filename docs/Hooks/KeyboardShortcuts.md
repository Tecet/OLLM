# Hooks Panel - Keyboard Shortcuts Quick Reference

**Quick reference for Hooks Panel UI keyboard shortcuts**

---

## Navigation

| Key     | Action      | Description                                  |
| ------- | ----------- | -------------------------------------------- |
| **Tab** | Enter Panel | Navigate to Hooks tab and activate panel     |
| **↑**   | Move Up     | Navigate to previous hook in list            |
| **↓**   | Move Down   | Navigate to next hook in list                |
| **Esc** | Exit Panel  | Return to main navigation (Browse Mode)      |
| **0**   | Exit Panel  | Alternative way to return to main navigation |

### Navigation Tips

- **↑** from first hook moves to Exit item
- **↓** from Exit item moves to first hook
- Categories are visual-only (not selectable)
- Auto-scroll keeps selected hook visible

---

## Hook Actions

| Key       | Action | Description                                       |
| --------- | ------ | ------------------------------------------------- |
| **Enter** | Toggle | Enable/disable selected hook                      |
| **←**     | Toggle | Alternative way to toggle hook                    |
| **→**     | Toggle | Alternative way to toggle hook                    |
| **A**     | Add    | Open Add Hook dialog                              |
| **E**     | Edit   | Open Edit Hook dialog (user hooks only)           |
| **D**     | Delete | Open Delete Confirmation dialog (user hooks only) |
| **T**     | Test   | Open Test Hook dialog                             |

### Action Tips

- **Enter** on Exit item exits the panel
- **Enter** on hook toggles enabled/disabled
- Built-in hooks can be toggled but not edited/deleted
- User hooks can be toggled, edited, and deleted

---

## Dialog Controls

| Key     | Action  | Description                       |
| ------- | ------- | --------------------------------- |
| **S**   | Save    | Save changes in Add/Edit dialog   |
| **C**   | Cancel  | Close dialog without saving       |
| **Esc** | Cancel  | Alternative way to close dialog   |
| **D**   | Confirm | Confirm deletion in Delete dialog |

### Dialog Tips

- Dialogs appear centered on screen
- Press **Esc** to close any dialog
- Changes are saved immediately when confirmed
- Canceled changes are discarded

---

## Visual Indicators

### Hook Status

- **● Green** - Hook is enabled
- **○ Gray** - Hook is disabled
- **Yellow highlight** - Currently selected hook (when panel has focus)
- **Cyan border** - Panel has focus

### Category Icons

- **📝** File Events
- **💬** Prompt Events
- **👤** User Triggered
- **🔄** Session Events
- **🤖** Agent Events
- **🧠** Model Events
- **🔧** Tool Events
- **📦** Compression Events
- **🔔** Notifications

### Scroll Indicators

- **▲** More hooks above (scroll up)
- **▼** More hooks below (scroll down)

---

## Common Workflows

### Quick Toggle

```
Tab → Enter → ↓ (to hook) → Enter (toggle)
```

### Add New Hook

```
Tab → Enter → A → (fill form) → S
```

### Edit Hook

```
Tab → Enter → ↓ (to hook) → E → (modify) → S
```

### Delete Hook

```
Tab → Enter → ↓ (to hook) → D → D (confirm)
```

### Test Hook

```
Tab → Enter → ↓ (to hook) → T → (review) → Esc
```

### Quick Exit

```
Esc (from anywhere in panel)
or
↑ (to Exit item) → Enter
or
0 (from anywhere in panel)
```

---

## Keyboard Layout

```
┌─────────────────────────────────────────────────────────────┐
│                    Navigation Keys                           │
│                                                              │
│                         ↑                                    │
│                    (Move Up)                                 │
│                                                              │
│              ←                    →                          │
│          (Toggle)            (Toggle)                        │
│                                                              │
│                         ↓                                    │
│                    (Move Down)                               │
│                                                              │
├─────────────────────────────────────────────────────────────┤
│                    Action Keys                               │
│                                                              │
│  A (Add)    E (Edit)    D (Delete)    T (Test)              │
│                                                              │
│  Enter (Toggle/Select)    Esc (Exit)    0 (Exit)            │
│                                                              │
│  Tab (Enter Panel)                                           │
└─────────────────────────────────────────────────────────────┘
```

---

## Accessibility

### Alternative Keys

Some keys have alternatives for different keyboard layouts:

- **Exit:** Esc or 0
- **Toggle:** Enter or ← or →
- **Cancel Dialog:** Esc or C

### Terminal Compatibility

- All shortcuts work in standard terminal emulators
- Arrow keys require terminal with arrow key support
- Some terminals may require different key bindings

---

## Tips & Tricks

### Speed Navigation

1. **Jump to Exit:** Press **↑** from first hook
2. **Quick Exit:** Press **Esc** from anywhere
3. **Fast Toggle:** Use **Enter** instead of arrow keys
4. **Category Skip:** Use **↓** repeatedly to skip through categories

### Efficient Workflow

1. **Review First:** Navigate through all hooks before making changes
2. **Test Before Enable:** Always test hooks before enabling
3. **Batch Operations:** Toggle multiple hooks in one session
4. **Quick Disable:** Disable hooks temporarily instead of deleting

### Keyboard Shortcuts Memory Aid

**Navigation:** Arrow keys (↑↓)  
**Actions:** First letter (A=Add, E=Edit, D=Delete, T=Test)  
**Toggle:** Enter or arrows (←→)  
**Exit:** Esc or 0

---

## Troubleshooting

### Keys Not Working

**Check panel focus:**

- Panel border should be cyan
- Selected hook should be yellow
- Press **Tab** to give focus

**Check terminal:**

- Ensure terminal supports arrow keys
- Try alternative keys (Esc, 0)
- Restart terminal if needed

### Dialogs Not Responding

**Close existing dialogs:**

- Press **Esc** to close
- Try action again

**Check terminal size:**

- Dialogs require minimum 80x24
- Resize terminal if too small

---

## See Also

- [Hooks User Guide](3%20projects/OLLM%20CLI/Hooks/user-guide.md) - Complete hooks documentation
- [Hook Development Guide](3%20projects/OLLM%20CLI/Hooks/development-guide.md) - Creating custom hooks
- [MCP Commands](MCP_commands.md) - Command-line hook management

---

**Last Updated:** 2026-01-18  
**Version:** 0.1.6  
**Feature:** Hooks Panel UI
