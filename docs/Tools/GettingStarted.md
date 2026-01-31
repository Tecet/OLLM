# Extension System User Guide

**Complete Guide to Using Extensions in OLLM CLI**

This guide covers everything you need to know about finding, installing, and managing extensions.

---

## 📋 Table of Contents

1. [Introduction](#introduction)
2. [Finding Extensions](#finding-extensions)
3. [Installing Extensions](#installing-extensions)
4. [Managing Extensions](#managing-extensions)
5. [Extension Components](#extension-components)
6. [Permissions](#permissions)
7. [Troubleshooting](#troubleshooting)

**See Also:**

- [Extension System Overview](3%20projects/OLLM%20CLI/Extensions/README.md) - Extension system introduction
- [Extension Development Guide](3%20projects/OLLM%20CLI/Extensions/development-guide.md) - Creating extensions
- [Manifest Reference](manifest-reference.md) - Manifest schema
- [Marketplace Guide](marketplace.md) - Extension marketplace

---

## Introduction

### What are Extensions?

Extensions are modular packages that add functionality to OLLM CLI. They can include:

- **Skills** - Prompt templates for common tasks
- **Settings** - Configuration presets
- **Servers** - MCP servers for tools
- **Hooks** - Automation scripts

### Why Use Extensions?

**Benefits:**

- 🚀 Quick setup - Install pre-configured functionality
- 🔄 Reusable - Share configurations across projects
- 🛡️ Safe - Sandboxed execution with permissions
- 🔥 Hot-reload - Update without restart
- 🌐 Community - Access community-created extensions

---

## Finding Extensions

### Search Extensions

```bash
# Search by keyword
/extensions search github

# Search for database tools
/extensions search database

# Search for development tools
/extensions search dev
```

### Browse Categories

**Official Extensions:**

- `ollm-dev-tools` - Development workflows
- `ollm-github` - GitHub integration
- `ollm-database` - Database tools
- `ollm-docs` - Documentation templates

**Community Extensions:**

- `code-quality` - Linting and formatting
- `test-automation` - Testing workflows
- `deployment` - Deployment automation
- `monitoring` - System monitoring

### Extension Information

```bash
# View extension details before installing
/extensions info github-integration

# Shows:
# - Description
# - Version
# - Author
# - Components
# - Permissions required
# - Download URL
```

---

## Installing Extensions

### From URL

```bash
# Install from direct URL
/extensions install https://github.com/user/ext/releases/download/v1.0.0/extension.tar.gz

# Install from GitHub release
/extensions install https://github.com/ollm/ext-github/releases/latest/download/github.tar.gz
```

### From File

```bash
# Install from local file
/extensions install ./my-extension.tar.gz

# Install from downloaded file
/extensions install ~/Downloads/extension.tar.gz
```

### From Registry

```bash
# Install by name (when registry is available)
/extensions install github-integration

# Install specific version
/extensions install github-integration@1.2.0
```

### Installation Process

1. **Download** - Extension is downloaded
2. **Verify** - Checksum is verified (if available)
3. **Extract** - Archive is extracted
4. **Parse** - Manifest is parsed
5. **Permissions** - Permission approval requested
6. **Register** - Components are registered
7. **Enable** - Extension is enabled

---

## Managing Extensions

### List Extensions

```bash
# List all installed extensions
/extensions list

# Output shows:
# - Name
# - Version
# - Status (enabled/disabled)
# - Components count
```

### Enable/Disable

```bash
# Disable an extension
/extensions disable github-integration

# Enable an extension
/extensions enable github-integration

# Extensions can be disabled without uninstalling
```

### Update Extensions

```bash
# Check for updates
/extensions update github-integration

# Update all extensions
/extensions update --all
```

### Remove Extensions

```bash
# Remove an extension
/extensions remove github-integration

# This permanently deletes the extension
```

### Extension Information

```bash
# Show detailed information
/extensions info github-integration

# Shows:
# - Name, version, author
# - Description
# - Components (skills, settings, servers, hooks)
# - Permissions
# - Installation path
# - Status
```

### Reload Extensions

```bash
# Reload all extensions
/extensions reload

# Useful after:
# - Modifying extension files
# - Installing new extensions
# - Changing permissions
```

---

## Extension Components

### Skills

Prompt templates for common tasks.

**Example:**

```bash
# Extension includes code review skill
/skill code-review

# Uses the template from the extension
```

**Viewing Skills:**

```bash
# List skills from extension
/extensions info github-integration

# Shows available skills
```

### Settings

Configuration presets and defaults.

**Example:**

```yaml
# Extension provides GitHub settings
github:
  token: ${GITHUB_TOKEN}
  defaultRepo: user/repo
  autoCommit: true
```

**Using Settings:**

- Settings are automatically loaded
- Can be overridden in user config
- Merged with existing settings

### Servers

MCP servers for tools and integrations.

**Example:**

```bash
# Extension includes GitHub MCP server
# Automatically available after installation

# Use GitHub tools
Can you show me the latest issues in my repository?
```

**Managing Servers:**

```bash
# List servers from extension
/mcp list

# Shows servers provided by extensions
```

### Hooks

Automation scripts for workflows.

**Example:**

```bash
# Extension includes pre-commit hook
# Automatically runs before commits

# List hooks from extension
/hooks list

# Shows hooks provided by extensions
```

---

## Permissions

### Permission Types

Extensions request permissions in their manifest:

| Permission   | Description     | Example                        |
| ------------ | --------------- | ------------------------------ |
| `filesystem` | File operations | `["read", "write"]`            |
| `network`    | Network access  | `["https://api.github.com"]`   |
| `tools`      | Tool execution  | `["read-file", "shell"]`       |
| `hooks`      | Hook events     | `["pre-execution", "on-save"]` |
| `settings`   | Settings access | `["read", "write"]`            |

### Granting Permissions

**During Installation:**

```bash
/extensions install github-integration

# Prompt:
# "github-integration requests:
#  - filesystem: read, write
#  - network: https://api.github.com
#  - tools: read-file, write-file
# Allow? (y/n)"
```

**After Installation:**

```bash
# Review permissions
/extensions info github-integration

# Revoke specific permission
/extensions revoke github-integration filesystem

# Grant permission
/extensions grant github-integration filesystem read
```

### Permission Best Practices

**Do:**

- ✅ Review permissions before installing
- ✅ Only grant necessary permissions
- ✅ Revoke unused permissions
- ✅ Check extension source

**Don't:**

- ❌ Grant all permissions blindly
- ❌ Install untrusted extensions
- ❌ Ignore permission warnings
- ❌ Grant network access to unknown domains

---

## Troubleshooting

### Extension Not Loading

**Check if installed:**

```bash
/extensions list
```

**Check if enabled:**

```bash
/extensions info my-extension
# Look for "enabled: true"
```

**Enable extension:**

```bash
/extensions enable my-extension
```

**Reload extensions:**

```bash
/extensions reload
```

### Components Not Working

**Check permissions:**

```bash
/extensions info my-extension
# Review granted permissions
```

**Check logs:**

```bash
/extensions logs my-extension
```

**Reinstall extension:**

```bash
/extensions remove my-extension
/extensions install <url>
```

### Installation Fails

**Check URL:**

- Verify URL is correct
- Check if file exists
- Ensure proper format (.tar.gz)

**Check network:**

- Verify internet connection
- Check firewall settings
- Try different network

**Check disk space:**

- Ensure sufficient disk space
- Check installation directory permissions

### Permission Issues

**Grant missing permissions:**

```bash
/extensions grant my-extension filesystem read
```

**Review required permissions:**

```bash
/extensions info my-extension
```

**Reset permissions:**

```bash
# Revoke all
/extensions revoke my-extension --all

# Reinstall to re-grant
/extensions remove my-extension
/extensions install <url>
```

---

## Advanced Usage

### Development Mode

Install extension in development mode for hot-reload:

```bash
# Install in dev mode
/extensions install ./my-extension --dev

# Enable hot-reload
/extensions watch my-extension

# Changes are automatically reloaded
```

### Custom Installation Path

```bash
# Install to custom location
/extensions install <url> --path ~/.ollm/custom-extensions/
```

### Offline Installation

```bash
# Download extension
curl -L <url> -o extension.tar.gz

# Install from file
/extensions install ./extension.tar.gz
```

### Extension Dependencies

Some extensions depend on others:

```bash
# Install dependencies first
/extensions install base-tools
/extensions install github-integration

# Or install with dependencies
/extensions install github-integration --with-deps
```

---

## Best Practices

### Extension Selection

**Choose extensions that:**

- ✅ Have clear documentation
- ✅ Are actively maintained
- ✅ Have good reviews
- ✅ Request minimal permissions
- ✅ Come from trusted sources

**Avoid extensions that:**

- ❌ Request excessive permissions
- ❌ Have no documentation
- ❌ Are unmaintained
- ❌ Come from unknown sources

### Extension Management

**Do:**

- ✅ Keep extensions updated
- ✅ Remove unused extensions
- ✅ Review permissions regularly
- ✅ Test in development first
- ✅ Backup extension configurations

**Don't:**

- ❌ Install too many extensions
- ❌ Grant unnecessary permissions
- ❌ Ignore update notifications
- ❌ Skip permission reviews

### Performance

**Optimize performance:**

- Disable unused extensions
- Remove unnecessary extensions
- Use hot-reload only in development
- Monitor resource usage

---

## Examples

### Example 1: GitHub Workflow

```bash
# Install GitHub extension
/extensions install github-integration

# Grant permissions
# (prompted during installation)

# Use GitHub features
Can you show me open issues?
Create a PR for the feature branch
```

### Example 2: Development Setup

```bash
# Install dev tools
/extensions install dev-tools

# Includes:
# - Code formatting hooks
# - Linting tools
# - Test runners
# - Git integration

# Automatically runs on save and commit
```

### Example 3: Documentation

```bash
# Install docs extension
/extensions install documentation-tools

# Use documentation templates
/skill api-docs
/skill readme
/skill changelog
```

---

## Further Reading

### Documentation

- [Extension System Overview](3%20projects/OLLM%20CLI/Extensions/README.md) - Introduction
- [Extension Development Guide](3%20projects/OLLM%20CLI/Extensions/development-guide.md) - Creating extensions
- [Manifest Reference](manifest-reference.md) - Manifest schema
- [Marketplace Guide](marketplace.md) - Extension marketplace

### Related Features

- [Hooks](../hooks/) - Hook system
- [MCP Servers](../servers/) - MCP servers
- [MCP Commands](MCP_commands.md) - Extension commands

### External Resources

- Extension Registry (https://github.com/ollm/extensions) - Official extensions
- Community Extensions (https://github.com/topics/ollm-extension) - Community extensions

---

**Last Updated:** 2026-01-16  
**Version:** 0.1.6  
**Next:** [Extension Development Guide](3%20projects/OLLM%20CLI/Extensions/development-guide.md)

<!-- Alpha release v0.1.6 -->

