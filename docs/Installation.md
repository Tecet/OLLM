# Installation Guide

This guide will help you install OLLM CLI on your computer. The process is straightforward and should take about 5-10 minutes.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Quick Install](#quick-install)
- [Detailed Installation](#detailed-installation)
- [Platform-Specific Instructions](#platform-specific-instructions)
- [Verify Installation](#verify-installation)
- [Next Steps](#next-steps)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before installing OLLM CLI, make sure you have:

### Required

**Node.js 20 or higher**
- Check your version: `node --version`
- Download from: [nodejs.org](https://nodejs.org/)
- We recommend the LTS (Long Term Support) version

**npm (comes with Node.js)**
- Check your version: `npm --version`
- Should be version 10 or higher

### Recommended

**Modern Terminal**
- **Windows:** Windows Terminal (from Microsoft Store)
- **macOS:** iTerm2 or built-in Terminal
- **Linux:** Your favorite terminal emulator

**System Resources**
- **RAM:** 8GB minimum, 16GB recommended
- **Storage:** 10GB free space (for models)
- **GPU:** Optional but recommended for faster AI

---

## Quick Install

The fastest way to get started:

```bash
# Install OLLM CLI globally
npm install -g @tecet/ollm

# The installer will guide you through:
# 1. Installing Ollama (if needed)
# 2. Downloading a starter model
# 3. Setting up configuration

# Start using OLLM CLI
ollm
```

That's it! The interactive installer will handle everything else.

---

## Detailed Installation

### Step 1: Install Node.js

If you don't have Node.js 20+ installed:

**Windows:**
1. Download installer from [nodejs.org](https://nodejs.org/)
2. Run the installer
3. Follow the setup wizard
4. Restart your terminal

**macOS:**
```bash
# Using Homebrew (recommended)
brew install node

# Or download from nodejs.org
```

**Linux:**
```bash
# Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Fedora
sudo dnf install nodejs

# Arch Linux
sudo pacman -S nodejs npm
```

**Verify installation:**
```bash
node --version  # Should show v20.x.x or higher
npm --version   # Should show 10.x.x or higher
```

### Step 2: Install OLLM CLI

**Global Installation (Recommended):**
```bash
npm install -g @tecet/ollm
```

This makes the `ollm` command available everywhere.

**Local Installation (Alternative):**
```bash
# In your project directory
npm install @tecet/ollm

# Run with npx
npx ollm
```

### Step 3: Interactive Setup

After installation, the setup wizard will start automatically:

```
┌─ OLLM CLI Setup ─────────────────────────┐
│                                           │
│ Welcome to OLLM CLI!                     │
│                                           │
│ Checking for Ollama...                   │
```

**If Ollama is not installed:**
```
│ ✗ Ollama not detected                    │
│                                           │
│ Ollama is required to run local models.  │
│ Install Ollama now? (Y/n):               │
```

Type `Y` and press Enter. The installer will:
1. Download Ollama for your platform
2. Install it automatically
3. Start the Ollama service

**Download a starter model:**
```
│ Pull default model (llama3.2:3b ~2GB)?  │
│ This will download a small model to      │
│ get you started. (Y/n):                  │
```

Type `Y` to download a small, fast model (about 2GB).

**Setup complete:**
```
│ ✓ Setup complete!                        │
│                                           │
│ Start using OLLM CLI:                    │
│   ollm                                   │
│                                           │
└───────────────────────────────────────────┘
```

---

## Platform-Specific Instructions

### Windows

**1. Install Node.js:**
- Download from [nodejs.org](https://nodejs.org/)
- Run the `.msi` installer
- Check "Add to PATH" during installation

**2. Install OLLM CLI:**
```powershell
# Open PowerShell as Administrator
npm install -g @tecet/ollm
```

**3. Ollama Installation:**
The installer will download `OllamaSetup.exe` and install it automatically.

**4. Windows Terminal (Recommended):**
- Install from Microsoft Store
- Better colors and Unicode support
- Modern features

**Common Issues:**
- If you get permission errors, run PowerShell as Administrator
- If `ollm` command not found, restart your terminal
- Check that npm global bin is in PATH: `npm config get prefix`

### macOS

**1. Install Node.js:**
```bash
# Using Homebrew (recommended)
brew install node

# Or download from nodejs.org
```

**2. Install OLLM CLI:**
```bash
npm install -g @tecet/ollm
```

**3. Ollama Installation:**
The installer will download and install Ollama.app automatically.

**4. Terminal Setup:**
- Built-in Terminal works fine
- iTerm2 recommended for better features
- Alacritty for maximum performance

**Common Issues:**
- If you get permission errors: `sudo npm install -g @tecet/ollm`
- Or configure npm to use user directory (see Troubleshooting)
- macOS may ask for permission to install Ollama - click "Allow"

### Linux

**1. Install Node.js:**
```bash
# Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Fedora
sudo dnf install nodejs

# Arch Linux
sudo pacman -S nodejs npm
```

**2. Install OLLM CLI:**
```bash
npm install -g @tecet/ollm
```

**3. Ollama Installation:**
The installer will run the official Ollama install script:
```bash
curl -fsSL https://ollama.ai/install.sh | sh
```

**4. Terminal:**
Most Linux terminals work great out of the box.

**Common Issues:**
- If you get permission errors, use `sudo` or configure npm user directory
- Ensure your terminal supports 256 colors
- Check that Ollama service is running: `systemctl status ollama`

---

## Verify Installation

After installation, verify everything works:

### 1. Check OLLM CLI Version
```bash
ollm --version
```

Should show: `0.1.0` or higher

### 2. Check Ollama
```bash
ollama --version
```

Should show Ollama version information.

### 3. List Available Models
```bash
ollama list
```

Should show at least one model (like `llama3.2:3b`).

### 4. Test OLLM CLI
```bash
ollm
```

You should see the OLLM CLI interface with a welcome message.

### 5. Send a Test Message
```
You: Hello!
```

The AI should respond with a greeting.

---

## Manual Ollama Installation

If the automatic installer doesn't work, install Ollama manually:

### Windows
1. Download from [ollama.ai/download](https://ollama.ai/download)
2. Run `OllamaSetup.exe`
3. Follow the installer
4. Ollama will start automatically

### macOS
1. Download from [ollama.ai/download](https://ollama.ai/download)
2. Open the `.zip` file
3. Drag Ollama.app to Applications
4. Open Ollama.app
5. It will run in the menu bar

### Linux
```bash
curl -fsSL https://ollama.ai/install.sh | sh
```

### Verify Ollama Installation
```bash
# Check if Ollama is running
curl http://localhost:11434/api/tags

# Should return JSON with model list
```

### Download a Model
```bash
# Download a small, fast model
ollama pull llama3.2:3b

# Or a larger, more capable model
ollama pull llama3.1:8b
```

---

## Configuration

OLLM CLI creates configuration files automatically, but you can customize them:

### User Configuration
**Location:** `~/.ollm/settings.json`

```json
{
  "provider": {
    "ollama": {
      "autoStart": true,
      "host": "localhost",
      "port": 11434,
      "url": "http://localhost:11434"
    }
  },
  "ui": {
    "theme": "solarized-dark"
  },
  "context": {
    "autoCompress": true
  }
}
```

### Workspace Configuration
**Location:** `.ollm/settings.json` (in your project)

Workspace settings override user settings.

### Environment Variables
```bash
# Ollama host
export OLLAMA_HOST=http://localhost:11434

# Log level
export OLLM_LOG_LEVEL=info

# Custom config path
export OLLM_CONFIG_PATH=~/.ollm/custom-config.json
```

---

## Updating

### Update OLLM CLI
```bash
# Check for updates
npm outdated -g @tecet/ollm

# Update to latest version
npm update -g @tecet/ollm

# Or reinstall
npm install -g @tecet/ollm@latest
```

### Update Ollama
```bash
# Ollama updates itself automatically
# Or manually:

# macOS/Linux
curl -fsSL https://ollama.ai/install.sh | sh

# Windows
# Download new installer from ollama.ai/download
```

### Update Models
```bash
# Update a specific model
ollama pull llama3.2:3b

# List outdated models
ollama list
```

---

## Uninstalling

### Uninstall OLLM CLI
```bash
npm uninstall -g @tecet/ollm
```

### Remove Configuration
```bash
# Remove user configuration
rm -rf ~/.ollm

# Remove workspace configuration
rm -rf .ollm
```

### Uninstall Ollama

**Windows:**
- Use "Add or Remove Programs"
- Search for "Ollama"
- Click "Uninstall"

**macOS:**
- Drag Ollama.app to Trash
- Remove data: `rm -rf ~/.ollama`

**Linux:**
```bash
# Stop service
sudo systemctl stop ollama

# Remove binary
sudo rm /usr/local/bin/ollama

# Remove data
rm -rf ~/.ollama
```

---

## Next Steps

Now that OLLM CLI is installed:

1. **[Quick Start Guide](Quickstart.md)** - Learn the basics in 5 minutes
2. **[User Interface Guide](UI&Settings/UIGuide.md)** - Understand the interface
3. **[Commands Reference](UI&Settings/Commands.md)** - Learn all commands
4. **[Configuration Guide](UI&Settings/Configuration.md)** - Customize your setup

---

## Troubleshooting

### Installation Issues

**"npm: command not found"**
- Node.js is not installed or not in PATH
- Install Node.js from [nodejs.org](https://nodejs.org/)
- Restart your terminal

**"Permission denied" errors**
- On macOS/Linux: Use `sudo npm install -g @tecet/ollm`
- Or configure npm to use user directory (recommended)
- See [Troubleshooting Guide](Troubleshooting.md#permission-errors)

**"ollm: command not found"**
- npm global bin not in PATH
- Find npm bin: `npm config get prefix`
- Add to PATH: `export PATH=$(npm config get prefix)/bin:$PATH`
- Restart terminal

**Ollama installation fails**
- Install Ollama manually from [ollama.ai/download](https://ollama.ai/download)
- Verify installation: `ollama --version`
- Start Ollama: `ollama serve`

**Model download fails**
- Check internet connection
- Check disk space (models are 2-10GB)
- Try a smaller model: `ollama pull llama3.2:3b`
- Check Ollama logs: `ollama logs`

### Connection Issues

**"Cannot connect to Ollama"**
- Check if Ollama is running: `curl http://localhost:11434/api/tags`
- Start Ollama: `ollama serve`
- Check firewall settings
- See [Troubleshooting Guide](Troubleshooting.md#connection-issues)

### More Help

For detailed troubleshooting, see the [Troubleshooting Guide](Troubleshooting.md).

For other issues:
- **GitHub Issues:** [github.com/tecet/ollm/issues](https://github.com/tecet/ollm/issues)
- **Discussions:** [github.com/tecet/ollm/discussions](https://github.com/tecet/ollm/discussions)
- **Documentation:** [Complete Documentation](README.md)

---

**Last Updated:** January 26, 2026  
**Version:** 0.1.0  
**Author:** tecet
