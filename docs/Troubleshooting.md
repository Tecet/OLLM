# Troubleshooting Guide

Having trouble with OLLM CLI? This guide covers common issues and their solutions. Most problems can be solved in a few minutes!

---

## Table of Contents

- [Quick Fixes](#quick-fixes)
- [Connection Issues](#connection-issues)
- [Installation Issues](#installation-issues)
- [Tool Execution Issues](#tool-execution-issues)
- [Context and Memory Issues](#context-and-memory-issues)
- [UI and Display Issues](#ui-and-display-issues)
- [Debug Mode](#debug-mode)
- [Getting Help](#getting-help)

---

## Quick Fixes

Try these first - they solve most problems:

### 1. Restart Everything

```bash
# Exit OLLM CLI
Ctrl+C

# Restart Ollama
ollama serve

# Start OLLM CLI again
ollm
```

### 2. Check Ollama is Running

```bash
# Test Ollama connection
curl http://localhost:11434/api/tags

# Should return JSON with model list
```

### 3. Verify Model is Downloaded

```bash
# List models
ollama list

# If empty, download a model
ollama pull llama3.2:3b
```

### 4. Clear Cache and Restart

```bash
# Clear npm cache
npm cache clean --force

# Reinstall OLLM CLI
npm install -g @tecet/ollm
```

### 5. Check Node.js Version

```bash
# Must be 20 or higher
node --version

# Update if needed
nvm install 20
nvm use 20
```

---

---

## Connection Issues

### Cannot connect to Ollama

**Symptoms:**

- Error message: `Connection refused` or `ECONNREFUSED`
- Error message: `Failed to connect to Ollama at http://localhost:11434`
- Commands hang or timeout when trying to communicate with Ollama

**Causes:**

- Ollama service is not running
- Ollama is running on a different host or port
- Firewall blocking the connection
- Network configuration issues

**Solutions:**

1. **Start Ollama service:**

   ```bash
   # On macOS/Linux
   ollama serve

   # On Windows
   # Ollama typically runs as a service, check if it's running
   ```

2. **Verify Ollama is running:**

   ```bash
   # Check if Ollama is responding
   curl http://localhost:11434/api/tags
   ```

3. **Specify custom host:**

   ```bash
   # If Ollama is running on a different host/port
   ollm --host http://192.168.1.100:11434

   # Or set environment variable
   export OLLAMA_HOST=http://192.168.1.100:11434
   ollm
   ```

4. **Check firewall settings:**
   - Ensure port 11434 is not blocked by your firewall
   - On Windows: Check Windows Defender Firewall settings
   - On macOS: Check System Preferences > Security & Privacy > Firewall
   - On Linux: Check iptables or ufw rules

5. **Verify network connectivity:**

   ```bash
   # Test if the host is reachable
   ping localhost

   # Test if the port is open
   telnet localhost 11434
   ```

### Model not found

**Symptoms:**

- Error message: `Model 'model-name' not found`
- Error message: `404 Not Found` when trying to use a model
- Model list doesn't show the expected model

**Causes:**

- Model hasn't been downloaded yet
- Model name is misspelled
- Model was removed or renamed

**Solutions:**

1. **List available models:**

   ```bash
   ollm --list-models
   # Or directly with Ollama
   ollama list
   ```

2. **Pull the model:**

   ```bash
   # Using OLLM CLI
   ollm --pull llama3.1:8b

   # Or directly with Ollama
   ollama pull llama3.1:8b
   ```

3. **Check model name spelling:**
   - Model names are case-sensitive
   - Use exact names from `ollama list`
   - Common models: `llama3.1:8b`, `codellama:7b`, `mistral:7b`

4. **Verify model installation:**
   ```bash
   # Check if model files exist
   ollama show llama3.1:8b
   ```

### Network/Firewall Issues

**Symptoms:**

- Intermittent connection failures
- Slow response times
- Timeout errors

**Causes:**

- Corporate firewall blocking connections
- VPN interfering with local connections
- Proxy configuration issues
- Network instability

**Solutions:**

1. **Check proxy settings:**

   ```bash
   # If behind a proxy, configure it
   export HTTP_PROXY=http://proxy.example.com:8080
   export HTTPS_PROXY=http://proxy.example.com:8080
   export NO_PROXY=localhost,127.0.0.1
   ```

2. **Disable VPN temporarily:**
   - Some VPNs interfere with localhost connections
   - Try disconnecting VPN and testing again

3. **Configure firewall exceptions:**
   - Add Ollama (port 11434) to firewall exceptions
   - Add OLLM CLI executable to allowed programs

4. **Use direct IP instead of localhost:**
   ```bash
   # Try using 127.0.0.1 instead of localhost
   ollm --host http://127.0.0.1:11434
   ```

---

## Installation Issues

### Global install fails

**Symptoms:**

- `npm install -g ollm-cli` fails with errors
- Permission denied errors during installation
- Installation completes but `ollm` command not found

**Causes:**

- Insufficient permissions
- npm global directory not in PATH
- Corrupted npm cache
- Node.js version incompatibility

**Solutions:**

1. **Use sudo (macOS/Linux):**

   ```bash
   sudo npm install -g ollm-cli
   ```

2. **Configure npm to use user directory (recommended):**

   ```bash
   # Create a directory for global packages
   mkdir ~/.npm-global

   # Configure npm to use it
   npm config set prefix '~/.npm-global'

   # Add to PATH (add to ~/.bashrc or ~/.zshrc)
   export PATH=~/.npm-global/bin:$PATH

   # Reload shell configuration
   source ~/.bashrc  # or source ~/.zshrc

   # Install without sudo
   npm install -g ollm-cli
   ```

3. **Clear npm cache:**

   ```bash
   npm cache clean --force
   npm install -g ollm-cli
   ```

4. **Verify installation:**

   ```bash
   # Check if ollm is in PATH
   which ollm

   # Check version
   ollm --version
   ```

5. **Manual PATH configuration:**

   ```bash
   # Find npm global bin directory
   npm bin -g

   # Add to PATH if not already there
   export PATH=$(npm bin -g):$PATH
   ```

### Permission errors

**Symptoms:**

- `EACCES` or `EPERM` errors during installation
- Cannot write to npm directories
- Installation fails with permission denied

**Causes:**

- npm global directory owned by root
- Insufficient file system permissions
- Protected system directories

**Solutions:**

1. **Fix npm permissions (macOS/Linux):**

   ```bash
   # Find npm directory
   npm config get prefix

   # Change ownership (replace USERNAME with your username)
   sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
   ```

2. **Use npx instead of global install:**

   ```bash
   # Run without installing globally
   npx ollm-cli -p "your prompt"
   ```

3. **Install in user directory (Windows):**

   ```cmd
   # npm should install to %APPDATA%\npm by default
   # Verify with:
   npm config get prefix

   # If needed, set to user directory:
   npm config set prefix %APPDATA%\npm
   ```

4. **Run as administrator (Windows):**
   - Right-click Command Prompt or PowerShell
   - Select "Run as administrator"
   - Run installation command

### Node version incompatibility

**Symptoms:**

- Error message about unsupported Node.js version
- Syntax errors during installation
- Module loading errors

**Causes:**

- Node.js version is too old (< 20.0.0)
- Using incompatible Node.js version

**Solutions:**

1. **Check Node.js version:**

   ```bash
   node --version
   ```

2. **Upgrade Node.js:**

   ```bash
   # Using nvm (recommended)
   nvm install 20
   nvm use 20

   # Or download from nodejs.org
   # https://nodejs.org/
   ```

3. **Install nvm (Node Version Manager):**

   ```bash
   # macOS/Linux
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

   # Windows - use nvm-windows
   # https://github.com/coreybutler/nvm-windows
   ```

4. **Verify installation after upgrade:**
   ```bash
   node --version  # Should be 20.x or higher
   npm install -g ollm-cli
   ```

---

## Tool Execution Issues

### Tool support errors

**Symptoms:**

- Error message: `unknown field: tools`
- Error message: `400 Bad Request` when using tools
- Model returns errors about function calling
- Tools don't work with certain models

**Causes:**

- Model doesn't support function calling
- Tool support metadata is incorrect
- Model was updated and capabilities changed
- Custom/unknown model without proper configuration

**Solutions:**

1. **Check model tool support:**

   ```bash
   /model info model-name
   ```

   Look for "Tool calling" in capabilities.

2. **For unknown models, configure tool support:**
   When switching to an unknown model, you'll be prompted:

   ```
   Does this model support function calling (tools)?
     [y] Yes, it supports tools
     [n] No, it doesn't support tools
     [a] Auto-detect (test with a sample request)
   ```

   Choose the appropriate option.

3. **Manually update tool support in user_models.json:**

   ```json
   {
     "user_models": [
       {
         "id": "custom-model:latest",
         "tool_support": false,
         "tool_support_source": "user_confirmed",
         "tool_support_confirmed_at": "2026-01-17T10:00:00Z"
       }
     ]
   }
   ```

4. **Use auto-detect for uncertain models:**
   - Select "auto-detect" when prompted
   - System will test the model with a sample tool request
   - Result is saved automatically

5. **Runtime learning:**
   If tool errors occur during usage, you'll be prompted:

   ```
   This model appears to not support tools. Update metadata? (y/n)
   ```

   Confirm to save the updated metadata.

6. **Check system messages:**
   When switching models, look for:

   ```
   Switched to model-name. Tools: Enabled/Disabled
   ```

7. **Verify tool filtering:**
   - Tools are automatically disabled for non-supporting models
   - Check the Tools Panel to see current tool status
   - System prompt includes note when tools are disabled

**Prevention:**

- Use models from the shipped profiles (LLM_profiles.json)
- Confirm tool support when adding custom models
- Keep user_models.json up to date with `/model list`

### Shell command timeout

**Symptoms:**

- Long-running commands are interrupted
- Error message: `Command timed out`
- Commands that should complete are killed prematurely

**Causes:**

- Default timeout is too short for the command
- Command is actually hanging or stuck
- System resource constraints

**Solutions:**

1. **Increase timeout in configuration:**

   ```yaml
   # ~/.ollm/config.yaml or .ollm/config.yaml
   tools:
     shell:
       timeout: 60000 # 60 seconds (in milliseconds)
   ```

2. **Use command-line flag:**

   ```bash
   ollm --tool-timeout 60000
   ```

3. **Set environment variable:**

   ```bash
   export OLLM_TOOL_TIMEOUT=60000
   ollm
   ```

4. **For specific long-running commands:**

   ```yaml
   # Configure per-tool timeouts
   tools:
     shell:
       timeout: 120000 # 2 minutes for shell commands
     web:
       timeout: 30000 # 30 seconds for web requests
   ```

5. **Check if command is actually stuck:**
   ```bash
   # Enable debug mode to see what's happening
   ollm --debug
   ```

### File operation denied

**Symptoms:**

- Error message: `EACCES: permission denied`
- Cannot read or write files
- File operations fail silently

**Causes:**

- Insufficient file system permissions
- File is locked by another process
- Protected system directories
- Workspace not in allowed paths

**Solutions:**

1. **Check file permissions:**

   ```bash
   # macOS/Linux
   ls -la /path/to/file

   # Fix permissions if needed
   chmod 644 /path/to/file  # For files
   chmod 755 /path/to/dir   # For directories
   ```

2. **Run from correct directory:**

   ```bash
   # Ensure you're in a directory you have access to
   cd ~/projects/my-project
   ollm
   ```

3. **Configure allowed paths:**

   ```yaml
   # ~/.ollm/config.yaml
   tools:
     file:
       allowedPaths:
         - ~/projects
         - ~/documents
         - /tmp
   ```

4. **Check file locks:**

   ```bash
   # macOS/Linux - check if file is open
   lsof /path/to/file

   # Windows - check file handles
   # Use Process Explorer or Resource Monitor
   ```

5. **Use workspace-relative paths:**
   ```bash
   # Instead of absolute paths, use relative paths
   # OLLM CLI operates within the current workspace
   cd /path/to/workspace
   ollm
   ```

### Tools Panel issues

**Symptoms:**

- Tool settings don't persist across sessions
- Disabled tools still appear to be called
- Tools Panel shows incorrect state
- Cannot toggle tools on/off

**Causes:**

- Settings file is corrupted or has wrong permissions
- Workspace settings override user settings
- Model doesn't support tools (all tools disabled)
- Cache not refreshed after settings change

**Solutions:**

1. **Check settings file location:**

   ```bash
   # User settings
   cat ~/.ollm/settings.json

   # Workspace settings (overrides user)
   cat .ollm/settings.json
   ```

2. **Verify settings file format:**

   ```json
   {
     "tools": {
       "executePwsh": false,
       "remote_web_search": true,
       "fsWrite": true
     }
   }
   ```

3. **Fix file permissions:**

   ```bash
   # macOS/Linux
   chmod 644 ~/.ollm/settings.json

   # Windows
   # Check file properties and ensure you have write access
   ```

4. **Reset to defaults:**

   ```bash
   # Backup current settings
   cp ~/.ollm/settings.json ~/.ollm/settings.json.backup

   # Remove settings file to reset
   rm ~/.ollm/settings.json

   # Restart OLLM CLI - new settings file will be created
   ```

5. **Check model tool support:**
   - If model doesn't support tools, all tools are disabled
   - Switch to a tool-capable model to enable tools
   - Check system message: "Switched to model. Tools: Enabled/Disabled"

6. **Verify workspace overrides:**

   ```bash
   # Check if workspace has tool settings
   cat .ollm/settings.json

   # Remove workspace settings to use user settings
   rm .ollm/settings.json
   ```

7. **Debug tool filtering:**
   - Enable debug mode: `ollm --debug`
   - Check logs for tool filtering messages
   - Verify two-stage filtering: model capability + user preference

**Prevention:**

- Don't manually edit settings.json (use Tools Panel)
- Keep backups of working configurations
- Use version control for workspace settings

---

## Context and Memory Issues

### Out of memory errors

**Symptoms:**

- Error message: `JavaScript heap out of memory`
- Process crashes during operation
- System becomes unresponsive
- Slow performance before crash

**Causes:**

- Context size too large for available memory
- Too many messages in conversation history
- Large files loaded into context
- Memory leak in long-running session

**Solutions:**

1. **Increase Node.js memory limit:**

   ```bash
   # Set max memory to 4GB
   export NODE_OPTIONS="--max-old-space-size=4096"
   ollm
   ```

2. **Reduce context size:**

   ```yaml
   # ~/.ollm/config.yaml
   context:
     maxTokens: 4096 # Reduce from default
     maxMessages: 50 # Limit conversation history
   ```

3. **Enable automatic context compression:**

   ```yaml
   # ~/.ollm/config.yaml
   context:
     compression:
       enabled: true
       strategy: 'summarize' # or "truncate"
       threshold: 0.8 # Compress at 80% capacity
   ```

4. **Clear conversation history:**

   ```bash
   # In interactive mode, use slash command
   /clear

   # Or start fresh session
   ollm --new-session
   ```

5. **Use context snapshots:**

   ```yaml
   # ~/.ollm/config.yaml
   context:
     snapshots:
       enabled: true
       interval: 100 # Save every 100 messages
   ```

6. **Monitor memory usage:**
   ```bash
   # Enable VRAM monitoring
   ollm --monitor-vram
   ```

### Context overflow

**Symptoms:**

- Error message: `Context length exceeded`
- Model refuses to process more input
- Responses become truncated or incomplete
- Warning about context limit

**Causes:**

- Conversation history too long
- Large files or documents in context
- Model's context limit reached
- Accumulated tool outputs

**Solutions:**

1. **Enable automatic context management:**

   ```yaml
   # ~/.ollm/config.yaml
   context:
     management:
       enabled: true
       strategy: 'sliding-window' # Keep recent messages
   ```

2. **Configure context limits:**

   ```yaml
   # ~/.ollm/config.yaml
   context:
     maxTokens: 8192 # Match model's capacity
     reserveTokens: 1024 # Reserve for response
   ```

3. **Use context compression:**

   ```yaml
   # ~/.ollm/config.yaml
   context:
     compression:
       enabled: true
       strategy: 'summarize'
       threshold: 0.75
   ```

4. **Manually manage context:**

   ```bash
   # Clear old messages
   /clear

   # Create snapshot before clearing
   /snapshot save important-context

   # Load snapshot later
   /snapshot load important-context
   ```

5. **Use models with larger context:**

   ```bash
   # Switch to model with larger context window
   ollm --model llama3.1:70b  # Has larger context capacity
   ```

6. **Optimize file loading:**
   ```yaml
   # ~/.ollm/config.yaml
   tools:
     file:
       maxFileSize: 100000 # Limit file size (bytes)
       truncateOutput: true
       maxOutputLines: 100
   ```

---

## UI and Display Issues

### Colors not showing correctly

**Symptoms:**

- Text appears in wrong colors
- No colors at all (plain text)
- Garbled characters or boxes

**Causes:**

- Terminal doesn't support 256 colors
- Terminal color scheme overriding
- Wrong terminal emulator

**Solutions:**

1. **Use a modern terminal:**
   - **Windows:** Windows Terminal (from Microsoft Store)
   - **macOS:** iTerm2 or built-in Terminal
   - **Linux:** Most modern terminals work

2. **Check color support:**

   ```bash
   # Test 256 color support
   curl -s https://gist.githubusercontent.com/HaleTom/89ffe32783f89f403bba96bd7bcd1263/raw/ | bash
   ```

3. **Try different theme:**

   ```bash
   /theme list
   /theme use neon-dark
   ```

4. **Disable terminal color scheme:**
   - Some terminals override colors
   - Check terminal preferences
   - Disable custom color schemes

### Text is too small/large

**Solutions:**

1. **Adjust terminal font size:**
   - **Windows Terminal:** Ctrl + Plus/Minus
   - **iTerm2:** Cmd + Plus/Minus
   - **Most terminals:** Ctrl + Plus/Minus

2. **Change terminal font:**
   - Use a monospace font
   - Recommended: Fira Code, JetBrains Mono, Cascadia Code

### Interface is cut off or wrapped weird

**Symptoms:**

- Text wraps incorrectly
- Side panel overlaps chat
- Status bar missing

**Causes:**

- Terminal window too small
- Terminal size not detected correctly

**Solutions:**

1. **Resize terminal window:**
   - Minimum: 80 columns × 24 rows
   - Recommended: 120 columns × 40 rows

2. **Check terminal size:**

   ```bash
   # Show terminal dimensions
   echo $COLUMNS x $LINES
   ```

3. **Restart OLLM CLI after resizing:**
   ```bash
   # Exit and restart
   Ctrl+C
   ollm
   ```

### Side panel won't open/close

**Solutions:**

1. **Use keyboard shortcut:**

   ```bash
   Ctrl+P  # Toggle side panel
   ```

2. **Check focus:**
   - Make sure OLLM CLI window is focused
   - Click on the terminal window

3. **Try slash command:**
   ```bash
   /panel toggle
   ```

### Keyboard shortcuts not working

**Symptoms:**

- Ctrl+K doesn't open commands
- Ctrl+P doesn't toggle panel
- Other shortcuts don't respond

**Causes:**

- Terminal intercepting shortcuts
- Wrong keyboard layout
- Focus not on OLLM CLI

**Solutions:**

1. **Check terminal key bindings:**
   - Some terminals use Ctrl+K for other things
   - Check terminal preferences
   - Disable conflicting shortcuts

2. **Use alternative shortcuts:**
   - Most commands have slash command alternatives
   - Type `/help` to see all commands

3. **Check keyboard layout:**
   - Ensure you're using the correct keyboard layout
   - Some layouts have different key positions

---

## Debug Mode

Debug mode provides detailed logging to help diagnose issues.

### Enable debug mode

**Using command-line flag:**

```bash
ollm --debug
```

**Using environment variable:**

```bash
# Set log level to debug
export OLLM_LOG_LEVEL=debug
ollm

# Or inline
OLLM_LOG_LEVEL=debug ollm
```

**In configuration file:**

```yaml
# ~/.ollm/config.yaml
logging:
  level: debug # Options: error, warn, info, debug
  file: ~/.ollm/logs/ollm.log # Optional: log to file
```

### Log levels

| Level   | Description             | Use Case        |
| ------- | ----------------------- | --------------- |
| `error` | Only errors             | Production use  |
| `warn`  | Errors and warnings     | Normal use      |
| `info`  | General information     | Default         |
| `debug` | Detailed debugging info | Troubleshooting |

### Interpreting debug output

**Connection debugging:**

```
[DEBUG] Connecting to Ollama at http://localhost:11434
[DEBUG] Request: POST /api/generate
[DEBUG] Response: 200 OK
```

**Tool execution debugging:**

```
[DEBUG] Executing tool: read-file
[DEBUG] Tool input: { path: "example.txt" }
[DEBUG] Tool output: [truncated 1024 bytes]
[DEBUG] Tool execution time: 45ms
```

**Context management debugging:**

```
[DEBUG] Context usage: 3456/8192 tokens (42%)
[DEBUG] VRAM usage: 4.2GB/8GB (52%)
[DEBUG] Compression triggered at 80% threshold
[DEBUG] Context compressed: 3456 -> 2048 tokens
```

**Model routing debugging:**

```
[DEBUG] Model routing: selected llama3.1:8b
[DEBUG] Routing reason: matches 'general' profile
[DEBUG] Model capabilities: tools=true, vision=false
```

### Debug output to file

```bash
# Redirect debug output to file
ollm --debug 2> debug.log

# Or configure in settings
```

```yaml
# ~/.ollm/config.yaml
logging:
  level: debug
  file: ~/.ollm/logs/debug.log
  maxSize: 10485760 # 10MB
  maxFiles: 5 # Keep 5 rotated logs
```

### Common debug patterns

**Trace a specific request:**

```bash
# Enable debug mode and watch for specific patterns
OLLM_LOG_LEVEL=debug ollm 2>&1 | grep "POST /api"
```

**Monitor tool execution:**

```bash
# See all tool calls
OLLM_LOG_LEVEL=debug ollm 2>&1 | grep "Executing tool"
```

**Check context usage:**

```bash
# Monitor context and memory
OLLM_LOG_LEVEL=debug ollm 2>&1 | grep -E "(Context|VRAM)"
```

---

## Getting Help

If you're still experiencing issues after trying the solutions above, here are additional resources:

### GitHub Issues

Report bugs or request features:

- **Repository:** https://github.com/ollm/ollm-cli
- **Issues:** https://github.com/ollm/ollm-cli/issues
- **Discussions:** https://github.com/ollm/ollm-cli/discussions

**Before creating an issue:**

1. Search existing issues to avoid duplicates
2. Include debug logs (`ollm --debug`)
3. Provide system information (OS, Node.js version, Ollama version)
4. Include steps to reproduce the problem
5. Share relevant configuration (redact sensitive info)

### Documentation

- **README:** [Main documentation](3%20projects/OLLM%20CLI/Development-Roadmap/README.md)
- **Configuration Reference:** [Configuration guide](./UI&Settings/Configuration.md)
- **Roadmap:** Future features (./DevelopmentRoadmap/Roadmap.md)

### Community Resources

- **Ollama Documentation:** https://github.com/ollama/ollama/tree/main/docs
- **Ollama Discord:** https://discord.gg/ollama
- **Model Context Protocol:** https://modelcontextprotocol.io/

### System Information

When reporting issues, include:

```bash
# Node.js version
node --version

# npm version
npm --version

# OLLM CLI version
ollm --version

# Ollama version
ollama --version

# Operating system
# macOS
sw_vers

# Linux
lsb_release -a

# Windows
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"

# Available models
ollama list
```

### Diagnostic checklist

Before seeking help, verify:

- [ ] Node.js 20+ is installed
- [ ] Ollama is running and accessible
- [ ] At least one model is downloaded
- [ ] OLLM CLI is installed globally
- [ ] `ollm --version` works
- [ ] Debug mode shows detailed logs
- [ ] Configuration file is valid YAML
- [ ] No firewall blocking connections
- [ ] Sufficient disk space and memory
- [ ] Latest version of OLLM CLI installed

### Quick diagnostic command

```bash
# Run comprehensive diagnostic
ollm --diagnose

# This will check:
# - Node.js version
# - Ollama connectivity
# - Available models
# - Configuration validity
# - Tool permissions
# - Memory availability
```

---

## Additional Tips

### Performance optimization

```yaml
# ~/.ollm/config.yaml
performance:
  # Reduce memory usage
  context:
    maxTokens: 4096

  # Faster model for quick tasks
  routing:
    defaultProfile: 'fast'

  # Limit tool output
  tools:
    truncateOutput: true
    maxOutputLines: 100
```

### Security best practices

```yaml
# ~/.ollm/config.yaml
security:
  # Restrict file access
  tools:
    file:
      allowedPaths:
        - ~/projects
      deniedPaths:
        - ~/.ssh
        - ~/.aws

  # Require confirmation for shell commands
  policy:
    shell: 'ask'
    file: 'auto'
```

### Backup and recovery

```bash
# Backup configuration
cp ~/.ollm/config.yaml ~/.ollm/config.yaml.backup

# Backup session data
cp -r ~/.ollm/session-data ~/.ollm/session-data.backup

# Restore from backup
cp ~/.ollm/config.yaml.backup ~/.ollm/config.yaml
```

---

**Last Updated:** January 2026  
**Version:** 0.1.6

For the latest troubleshooting information, visit the GitHub repository (https://github.com/ollm/ollm-cli).
