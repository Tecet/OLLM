# Themes Guide

**Last Updated:** January 26, 2026

Complete guide to the OLLM CLI theme system, including built-in themes, theme management, and customization options.

---

## Overview

OLLM CLI includes a comprehensive theming system with 6 built-in color schemes. Themes define colors for backgrounds, text, roles, status states, borders, and diff highlighting. Theme selection persists across sessions.

---

## Built-in Themes

### 1. Solarized Dark (Default)

**Name:** `solarized-dark`

**Description:** A precision theme designed to decrease eye strain with carefully balanced colors.

**Best For:** Long coding sessions, reduced eye fatigue

**Color Palette:**

- **Background:** Deep blue-black (#002b36)
- **Text:** Light gray (#839496)
- **Accent:** Yellow-orange (#b58900)
- **User:** Blue (#268bd2)
- **Assistant:** Cyan (#2aa198)
- **Success:** Green (#859900)
- **Error:** Red (#dc322f)

---

### 2. Neon Dark

**Name:** `neon-dark`

**Description:** A balanced dark theme with teal accents, suitable for most terminals.

**Best For:** General use, modern terminals

**Color Palette:**

- **Background:** Very dark gray (#1e1e1e)
- **Text:** Light gray (#d4d4d4)
- **Accent:** Teal (#4ec9b0)
- **User:** Blue (#569cd6)
- **Assistant:** Teal (#4ec9b0)
- **Success:** Teal (#4ec9b0)
- **Error:** Red (#f48771)

---

### 3. Dracula Dark

**Name:** `dracula-dark`

**Description:** A high-contrast theme based on the popular Dracula specification.

**Best For:** High contrast preference, vibrant colors

**Color Palette:**

- **Background:** Dark purple-gray (#282a36)
- **Text:** Off-white (#f8f8f2)
- **Accent:** Purple (#bd93f9)
- **User:** Cyan (#8be9fd)
- **Assistant:** Purple (#bd93f9)
- **Success:** Green (#50fa7b)
- **Error:** Red (#ff5555)

---

### 4. Nord Dark

**Name:** `nord-dark`

**Description:** An arctic, north-bluish color palette with cool tones.

**Best For:** Cool color preference, minimal aesthetic

**Color Palette:**

- **Background:** Dark blue-gray (#2e3440)
- **Text:** Off-white (#eceff4)
- **Accent:** Cyan (#88c0d0)
- **User:** Blue (#81a1c1)
- **Assistant:** Cyan (#88c0d0)
- **Success:** Green (#a3be8c)
- **Error:** Red (#bf616a)

---

### 5. Monokai Dark

**Name:** `monokai-dark`

**Description:** A vibrant, high-contrast theme popular in Sublime Text.

**Best For:** Vibrant colors, high contrast

**Color Palette:**

- **Background:** Dark olive-gray (#272822)
- **Text:** Off-white (#f8f8f2)
- **Accent:** Orange (#fd971f)
- **User:** Cyan (#66d9ef)
- **Assistant:** Green (#a6e22e)
- **Success:** Green (#a6e22e)
- **Error:** Pink (#f92672)

---

### 6. Solarized Dark 2

**Name:** `solarized-dark-2`

**Description:** Alternative Solarized Dark variant (identical to solarized-dark).

**Note:** This is a duplicate of `solarized-dark` with the same color palette.

---

## Theme Management

### Listing Themes

View all available themes:

```bash
/theme list
```

**Output:**

```
Available themes:
  • solarized-dark (default)
  • neon-dark
  • dracula-dark
  • nord-dark
  • monokai-dark
  • solarized-dark-2
```

---

### Switching Themes

Switch to a theme permanently:

```bash
/theme use <theme-name>
```

**Examples:**

```bash
/theme use dracula-dark
/theme use nord-dark
/theme use monokai-dark
```

**Behavior:**

1. Validates theme name
2. Loads theme colors
3. Applies theme to UI
4. Saves selection to `~/.ollm/settings.json`

**Output:**

```
✓ Theme changed to: dracula-dark
```

---

### Previewing Themes

Preview a theme temporarily without saving:

```bash
/theme preview <theme-name>
```

**Examples:**

```bash
/theme preview nord-dark
/theme preview monokai-dark
```

**Behavior:**

1. Validates theme name
2. Loads theme colors
3. Applies theme to UI
4. Does NOT save to settings

**Output:**

```
✓ Previewing theme: nord-dark
(Use /theme use nord-dark to save permanently)
```

---

## Theme Configuration

### Configuration File

Theme selection is stored in your user settings:

**Location:** `~/.ollm/settings.json`

**Structure:**

```json
{
  "ui": {
    "theme": "solarized-dark"
  },
  "llm": { ... },
  "tools": { ... }
}
```

**Path Resolution:**

- Windows: `C:\Users\<username>\.ollm\settings.json`
- macOS: `/Users/<username>/.ollm/settings.json`
- Linux: `/home/<username>/.ollm/settings.json`

---

### Default Theme

The default theme is set in the application configuration:

**Default:** `solarized-dark`

**Override:** Use `/theme use <name>` to change the default for your user.

---

## Theme Structure

### Color Categories

Themes define colors for the following categories:

**Background Colors:**

- `primary` - Main background
- `secondary` - Secondary background
- `tertiary` - Tertiary background

**Text Colors:**

- `primary` - Main text color
- `secondary` - Secondary text color
- `accent` - Accent/highlight color

**Role Colors:**

- `user` - User message color
- `assistant` - Assistant message color
- `system` - System message color
- `tool` - Tool call color

**Status Colors:**

- `success` - Success state color
- `warning` - Warning state color
- `error` - Error state color
- `info` - Info state color

**Border Colors:**

- `primary` - Primary border color
- `secondary` - Secondary border color
- `active` - Active border color
- `style` - Border style (round, single, double)

**Diff Colors:**

- `added` - Added lines in diff
- `removed` - Removed lines in diff

---

## Theme Usage Examples

### Text Colors

```typescript
// Primary text (main content)
<Text color={theme.text.primary}>Content</Text>

// Secondary text (less important)
<Text color={theme.text.secondary}>Details</Text>

// Accent text (highlights, emphasis)
<Text color={theme.text.accent}>Important!</Text>
```

---

### Role Colors

```typescript
// User messages
<Text color={theme.role.user}>User: Hello</Text>

// Assistant messages
<Text color={theme.role.assistant}>Assistant: Hi!</Text>

// System messages
<Text color={theme.role.system}>System: Ready</Text>

// Tool calls
<Text color={theme.role.tool}>Tool: execute</Text>
```

---

### Status Colors

```typescript
// Success state
<Text color={theme.status.success}>✓ Success</Text>

// Warning state
<Text color={theme.status.warning}>⚠ Warning</Text>

// Error state
<Text color={theme.status.error}>✗ Error</Text>

// Info state
<Text color={theme.status.info}>ℹ Info</Text>
```

---

### Borders

```typescript
// Primary border
<Box borderColor={theme.border.primary} borderStyle={theme.border.style}>
  Content
</Box>

// Active border (focused elements)
<Box borderColor={theme.border.active} borderStyle={theme.border.style}>
  Focused
</Box>
```

---

### Diff Colors

```typescript
// Added lines
<Text color={theme.diff.added}>+ Added line</Text>

// Removed lines
<Text color={theme.diff.removed}>- Removed line</Text>
```

---

## Choosing a Theme

### By Use Case

**Long Coding Sessions:**

- `solarized-dark` - Reduced eye strain
- `nord-dark` - Cool, minimal aesthetic

**High Contrast:**

- `dracula-dark` - Vibrant, high contrast
- `monokai-dark` - Vibrant with warm tones

**General Use:**

- `neon-dark` - Balanced, modern
- `solarized-dark` - Precision colors

---

### By Color Preference

**Cool Colors (Blues, Cyans):**

- `solarized-dark`
- `nord-dark`
- `neon-dark`

**Warm Colors (Purples, Oranges):**

- `dracula-dark`
- `monokai-dark`

**Balanced:**

- `neon-dark`
- `solarized-dark`

---

## Terminal Compatibility

### Color Support

**Required:** 256-color support  
**Recommended:** True color (24-bit) support

**Check Support:**

```bash
# Check TERM variable
echo $TERM

# Should be one of:
# - xterm-256color
# - screen-256color
# - tmux-256color
```

---

### Terminal Emulators

**Fully Supported:**

- iTerm2 (macOS)
- Windows Terminal (Windows)
- Alacritty (Cross-platform)
- Kitty (Cross-platform)
- Hyper (Cross-platform)

**Partially Supported:**

- macOS Terminal (limited colors)
- GNOME Terminal (Linux)
- Konsole (Linux)

**Not Recommended:**

- cmd.exe (Windows) - Use Windows Terminal instead
- Basic xterm - Use modern terminal emulator

---

## Troubleshooting

### Theme Not Applying

**Symptoms:**

- Theme command succeeds but colors don't change
- Colors look wrong or default

**Solutions:**

1. Check theme name is correct (case-sensitive)
2. Verify terminal supports 256 colors
3. Restart OLLM CLI
4. Check `~/.ollm/settings.json` for correct theme name

**Verify:**

```bash
# List themes
/theme list

# Use correct theme name
/theme use solarized-dark
```

---

### Theme Not Persisting

**Symptoms:**

- Theme resets to default on restart
- Settings not saved

**Solutions:**

1. Check `~/.ollm/` directory exists
2. Verify `settings.json` is writable
3. Check for permission errors
4. Manually edit `settings.json`

**Verify:**

```bash
# Check settings file
cat ~/.ollm/settings.json

# Should show: {"ui":{"theme":"theme-name"},...}
```

---

### Colors Look Wrong

**Symptoms:**

- Colors don't match theme description
- Washed out or incorrect colors

**Solutions:**

1. Verify terminal supports true color
2. Check terminal color scheme not overriding
3. Try different terminal emulator
4. Test with different theme

**Verify:**

```bash
# Test true color support
curl -s https://gist.githubusercontent.com/lifepillar/09a44b8cf0f9397465614e622979107f/raw/24-bit-color.sh | bash
```

---

## Best Practices

### For Users

1. **Try Multiple Themes** - Preview themes to find your favorite
2. **Match Terminal** - Choose theme that works with your terminal emulator
3. **Consider Lighting** - Dark themes for low light, adjust as needed
4. **Accessibility** - Choose high contrast if needed

### For Long Sessions

1. **Reduce Eye Strain** - Use `solarized-dark` or `nord-dark`
2. **Take Breaks** - No theme eliminates need for breaks
3. **Adjust Brightness** - Match terminal brightness to environment
4. **Use Blue Light Filter** - Consider OS-level blue light reduction

---

## Future Enhancements

### Planned Features

**Custom Themes:**

- User-defined themes in `~/.ollm/themes/`
- Theme import/export
- Theme editor UI

**Theme Variants:**

- Light themes
- High contrast themes
- Colorblind-friendly themes

**Dynamic Theming:**

- Time-based theme switching
- Context-aware themes
- Adaptive contrast

**Theme Marketplace:**

- Community themes
- Theme ratings
- Theme preview gallery

---

## Related Documentation

- [UI Guide](./UIGuide.md) - Main interface documentation
- [Configuration](./configuration.md) - Configuration options
- [Commands Reference](./Commands.md) - All slash commands

---

## External Resources

- [Solarized Color Scheme](https://ethanschoonover.com/solarized/)
- [Dracula Theme](https://draculatheme.com/)
- [Nord Theme](https://www.nordtheme.com/)
- [Monokai Theme](https://monokai.pro/)

---

**Last Updated:** January 26, 2026

<!-- Alpha Release: v0.1.6 -->

