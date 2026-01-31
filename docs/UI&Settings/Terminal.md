# Terminal Guide

**Last Updated:** January 26, 2026

Complete guide to the integrated terminal system in OLLM CLI, including architecture, usage, and integration with the shell tool.

---

## Overview

OLLM CLI includes an integrated terminal system that provides two independent terminal sessions within the UI. The terminal uses PTY (pseudo-terminal) for shell process management and xterm.js headless for ANSI parsing and rendering.

---

## Terminal Sessions

### Dual Terminal Design

OLLM CLI provides two independent terminal sessions:

1. **Terminal 1** - Primary terminal (implementation TBD)
2. **Terminal 2** - Secondary terminal (fully implemented)

**Features:**

- Each terminal runs in its own PTY process
- Independent shell sessions
- Separate scrollback buffers
- Can be focused and controlled independently

---

## Accessing the Terminal

### Opening the Terminal

**Method 1: Side Panel Tab**

1. Press `Ctrl+P` to open side panel (if closed)
2. Press `Ctrl+8` to switch to Terminal tab
3. Terminal displays in the side panel

**Method 2: Direct Navigation**

- Press `Ctrl+8` directly to open side panel and switch to terminal

---

### Terminal Layout

```
┌─ Terminal ───────────────────────────────┐
│ $ ls -la                                  │
│ total 48                                  │
│ drwxr-xr-x  12 user  staff   384 Jan 26  │
│ drwxr-xr-x   5 user  staff   160 Jan 25  │
│ -rw-r--r--   1 user  staff  1234 Jan 26  │
│                                           │
│ $ _                                       │
│                                           │
│ [Ctrl+L: Clear | Ctrl+C: Interrupt]      │
└───────────────────────────────────────────┘
```

---

## Terminal Features

### Shell Integration

**Platform-Specific Shells:**

- **Windows:** PowerShell
- **macOS:** bash or zsh
- **Linux:** bash

**Shell Configuration:**

- PowerShell: Prediction disabled for cleaner output
- Bash: Standard configuration
- Environment variables inherited from parent process

---

### ANSI Rendering

The terminal supports full ANSI escape sequences:

**Text Formatting:**

- Bold, italic, underline
- Dim, inverse
- Strikethrough

**Colors:**

- 16 basic colors
- 256-color palette
- True color (24-bit RGB)

**Cursor Control:**

- Cursor positioning
- Cursor visibility
- Cursor styles

---

### Scrollback Buffer

**Configuration:**

- Default: 1000 lines
- Prevents unbounded memory growth
- Older lines automatically removed

**Scrolling:**

- `Ctrl+Shift+Up` - Scroll up
- `Ctrl+Shift+Down` - Scroll down
- Auto-scroll to bottom on new output

---

## Terminal Commands

### Sending Commands

**Method 1: Type in Terminal**

1. Focus the terminal (click or navigate)
2. Type your command
3. Press `Return` to execute

**Method 2: Programmatic**

```typescript
// Via Terminal Context
const { sendCommand } = useTerminal();
sendCommand('ls -la');
```

---

### Command Examples

**File Operations:**

```bash
ls -la                  # List files
cd src                  # Change directory
cat file.txt            # View file contents
```

**Git Operations:**

```bash
git status              # Check git status
git diff                # View changes
git log --oneline       # View commit history
```

**System Information:**

```bash
# Windows (PowerShell)
Get-Process             # List processes
Get-Service             # List services

# macOS/Linux (bash)
ps aux                  # List processes
df -h                   # Disk usage
```

---

## Terminal Controls

### Keyboard Shortcuts

**Scrolling:**

- `Ctrl+Shift+Up` - Scroll up one line
- `Ctrl+Shift+Down` - Scroll down one line
- `Page Up` - Scroll up one page
- `Page Down` - Scroll down one page

**Control:**

- `Ctrl+C` - Interrupt current process
- `Ctrl+L` - Clear terminal
- `Ctrl+D` - Send EOF (exit shell)

**Navigation:**

- `Up` / `Down` - Command history
- `Ctrl+R` - Reverse search history
- `Tab` - Auto-complete

---

### Terminal Actions

**Clear Terminal:**

```bash
/terminal clear
# or
Ctrl+L
```

**Interrupt Process:**

```bash
Ctrl+C
```

**Restart Terminal:**

```bash
/terminal restart
```

---

## Shell Tool Integration

### Automatic Execution

When the AI uses the shell tool, commands are automatically executed in the terminal:

**Flow:**

1. AI decides to use shell tool
2. Command sent to terminal
3. Terminal executes command
4. Output displayed in terminal
5. Result returned to AI

**Example:**

```
User: List all TypeScript files
Assistant: I'll search for TypeScript files.
[Tool: shell]
Command: find . -name "*.ts"
Output:
./src/index.ts
./src/utils.ts
./tests/app.test.ts
```

---

### Manual Execution

You can also execute commands manually in the terminal:

**Method 1: Direct Input**

1. Focus terminal
2. Type command
3. Press `Return`

**Method 2: Via Chat**

```
User: Run "npm test" in the terminal
Assistant: I'll execute that command.
[Tool: shell]
Command: npm test
```

---

## Terminal Context API

### Using Terminal Context

**Import:**

```typescript
import { useTerminal } from '../contexts/TerminalContext';
```

**Access:**

```typescript
const {
  output, // Structured terminal output
  isRunning, // PTY process status
  sendCommand, // Send command + Enter
  sendRawInput, // Send raw characters
  clear, // Clear terminal
  interrupt, // Send Ctrl+C
  resize, // Resize terminal
} = useTerminal();
```

---

### Methods

**sendCommand(command: string)**

- Sends command with carriage return
- Use for complete commands
- Example: `sendCommand('ls -la')`

**sendRawInput(char: string)**

- Sends raw characters without modification
- Use for interactive input
- Example: `sendRawInput('y')` for yes/no prompts

**clear()**

- Clears xterm buffer
- Sends `clear` command to shell
- Resets output state

**interrupt()**

- Sends Ctrl+C to PTY
- Interrupts running process
- Does not clear terminal

**resize(cols: number, rows: number)**

- Resizes both xterm and PTY
- Minimum: 1 col × 1 row
- Silently fails on error

---

## Terminal Output

### Output Structure

Terminal output is structured as ANSI tokens:

```typescript
interface AnsiToken {
  text: string;
  bold: boolean;
  italic: boolean;
  underline: boolean;
  dim: boolean;
  inverse: boolean;
  fg: string; // Hex color
  bg: string; // Hex color
}

type AnsiLine = AnsiToken[];
type AnsiOutput = AnsiLine[];
```

---

### Rendering

**Text Formatting:**

```typescript
<Text
  color={token.fg}
  backgroundColor={token.bg}
  bold={token.bold}
  italic={token.italic}
  underline={token.underline}
  dimColor={token.dim}
  inverse={token.inverse}
>
  {token.text}
</Text>
```

**Color Support:**

- Default colors (terminal defaults)
- 256-color ANSI palette
- True color (24-bit RGB)

---

## Advanced Usage

### Command History

Command history is managed by the shell:

**Bash:**

- Stored in `~/.bash_history`
- Navigate with `Up` / `Down` arrows
- Search with `Ctrl+R`

**PowerShell:**

- Managed by PSReadLine module
- Navigate with `Up` / `Down` arrows
- Search with `Ctrl+R`

---

### Interactive Programs

The terminal supports interactive programs:

**Examples:**

- `vim` / `nano` - Text editors
- `htop` / `top` - Process monitors
- `less` / `more` - Pagers
- Interactive prompts (y/n)

**Note:** Some programs may not render correctly due to terminal size constraints.

---

### Long-Running Commands

**Behavior:**

- Commands run in background
- Output streams in real-time
- Can be interrupted with `Ctrl+C`

**Examples:**

```bash
# Development server
npm run dev

# Watch mode
npm run test -- --watch

# Long-running script
./deploy.sh
```

---

## Troubleshooting

### Terminal Not Starting

**Symptoms:**

- Terminal tab is empty
- No shell prompt appears

**Solutions:**

1. Check shell executable exists
2. Verify PTY spawn permissions
3. Check environment variables
4. Restart OLLM CLI

**Verify:**

```bash
# Check shell path
which bash          # macOS/Linux
where powershell    # Windows
```

---

### Output Not Displaying

**Symptoms:**

- Commands execute but no output
- Partial output displayed

**Solutions:**

1. Check xterm.js buffer state
2. Verify ANSI parsing
3. Clear terminal and retry
4. Check terminal size

**Verify:**

```bash
# Test with simple command
echo "Hello World"

# Check terminal size
echo $COLUMNS $LINES    # bash
$Host.UI.RawUI.WindowSize  # PowerShell
```

---

### Commands Not Working

**Symptoms:**

- Commands don't execute
- No response to input

**Solutions:**

1. Check terminal focus
2. Verify PTY process running
3. Check for hung process
4. Restart terminal

**Verify:**

```bash
# Check if shell is responsive
echo "test"

# If hung, interrupt
Ctrl+C

# If still hung, restart terminal
/terminal restart
```

---

### Colors Look Wrong

**Symptoms:**

- Colors don't match theme
- Washed out or incorrect colors

**Solutions:**

1. Check terminal color support
2. Verify theme settings
3. Test with different theme
4. Check ANSI color mode

**Verify:**

```bash
# Test colors
ls --color=auto    # Linux
ls -G              # macOS
Get-ChildItem      # PowerShell (auto-colored)
```

---

## Performance Considerations

### Buffer Optimization

**Viewport-Based Rendering:**

- Only renders visible lines
- Uses viewport position
- Avoids re-rendering entire buffer

**Memory Management:**

- Scrollback limit prevents unbounded growth
- Older lines automatically removed
- Token grouping reduces React components

---

### Responsive Behavior

**Width Calculation:**

- Adapts to UI layout
- Accounts for side panel visibility
- Minimum width: 10 columns

**Height Calculation:**

- Adapts to available space
- Accounts for header and status bar
- Minimum height: 1 row

**Dynamic Resize:**

- Automatically resizes on layout changes
- Resizes both xterm and PTY together
- Errors silently caught

---

## Best Practices

### Terminal Usage

1. **Check Process Status** - Verify `isRunning` before sending commands
2. **Use Appropriate Method** - `sendCommand()` for complete commands, `sendRawInput()` for interactive input
3. **Handle Errors** - Wrap PTY operations in try-catch
4. **Clean Up** - Dispose properly on unmount

---

### Command Execution

1. **Test Commands** - Test commands manually before automation
2. **Handle Output** - Parse output appropriately
3. **Check Exit Codes** - Verify command success
4. **Timeout Long Commands** - Set timeouts for long-running commands

---

### Performance

1. **Limit Scrollback** - Keep scrollback buffer reasonable
2. **Clear Regularly** - Clear terminal when not needed
3. **Avoid Spam** - Don't send commands too rapidly
4. **Monitor Memory** - Watch for memory leaks

---

## Related Documentation

- [UI Guide](./UIGuide.md) - Main interface documentation
- [Commands Reference](./Commands.md) - All slash commands
- [Keyboard Shortcuts](./keybinds.md) - Keybind reference
- [Tools System](../Tools/UserGuide.md) - Tool documentation

---

## External Resources

- [xterm.js Documentation](https://xtermjs.org/)
- [node-pty Documentation](https://github.com/microsoft/node-pty)
- [ANSI Escape Codes](https://en.wikipedia.org/wiki/ANSI_escape_code)

---

**Last Updated:** January 26, 2026

<!-- Alpha Release: v0.1.6 -->

