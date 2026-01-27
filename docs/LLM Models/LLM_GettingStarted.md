# Getting Started with Model Management

**Quick Start Guide**

This guide will help you get started with model management, routing, memory, templates, and project profiles in OLLM CLI.

---

## 📋 Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Quick Start](#quick-start)
4. [Basic Model Management](#basic-model-management)
5. [Using Model Routing](#using-model-routing)
6. [Working with Memory](#working-with-memory)
7. [Using Templates](#using-templates)
8. [Project Profiles](#project-profiles)
9. [Next Steps](#next-steps)

**See Also:**
- [Model Management Overview](3%20projects/OLLM%20CLI/LLM%20Models/README.md)
- [Model Commands](Models_commands.md)
- [Configuration Guide](Models_configuration.md)

---

## Introduction

OLLM CLI provides comprehensive model management capabilities that help you:

- **Manage models**: List, download, delete, and inspect models
- **Route intelligently**: Automatically select appropriate models for tasks
- **Remember context**: Store facts and preferences across sessions
- **Use templates**: Create reusable prompts with variables
- **Configure projects**: Auto-detect and apply project-specific settings

This guide covers the basics to get you started quickly.

---

## Prerequisites

**Required:**
- OLLM CLI installed and configured
- Ollama (or compatible provider) running
- At least one model installed

**Optional:**
- Project workspace for profile detection
- Custom templates directory

**Check your setup:**
```bash
# Verify OLLM CLI is installed
ollm --version

# Check provider connection
/model list
```

---

## Quick Start

### 1. List Available Models

See what models you have installed:

```bash
/model list
```

**Output:**
```
Available Models:
  ● llama3.1:8b (loaded)     4.7 GB   Modified 2 days ago
    mistral:7b               4.1 GB   Modified 1 week ago
    codellama:7b             3.8 GB   Modified 3 days ago
```

### 2. Download a Model

Pull a new model from the registry:

```bash
/model pull llama3.1:8b
```

**Progress display:**
```
Pulling llama3.1:8b...
[████████████████████████] 100% 4.7 GB @ 12.3 MB/s
Model llama3.1:8b ready.
```

### 3. View Model Details

Get detailed information about a model:

```bash
/model info llama3.1:8b
```

**Output:**
```
Model: llama3.1:8b
Size: 4.7 GB
Parameters: 8B
Context Length: 128,000 tokens
Quantization: Q4_K_M
Family: llama
Capabilities:
  ✓ Tool calling
  ✓ Streaming
  ✗ Vision
```

### 4. Use a Model

Switch to a specific model:

```bash
/model use llama3.1:8b
```

---

## Basic Model Management

### Listing Models

View all available models with details:

```bash
/model list
```

**Shows:**
- Model name and version
- Size on disk
- Last modified date
- Load status (loaded/unloaded)

### Pulling Models

Download models from the provider registry:

```bash
# Pull a specific model
/model pull llama3.1:8b

# Pull with tag
/model pull mistral:7b-instruct
```

**Features:**
- Real-time progress display
- Transfer rate monitoring
- Cancellable with Ctrl+C

### Deleting Models

Remove models to free disk space:

```bash
/model delete codellama:7b
```

**Safety features:**
- Confirmation prompt
- Shows space to be freed
- Automatic unload if currently loaded

### Keeping Models Loaded

Keep frequently-used models in memory for faster responses:

```bash
# Keep a model loaded
/model keep llama3.1:8b

# Unload a model
/model unload llama3.1:8b
```

**Benefits:**
- Eliminates model load time (2-5 seconds)
- Reduces latency for subsequent requests
- Useful for interactive sessions

### Unknown Model Handling

When you switch to a model that isn't in the system's database, OLLM CLI will prompt you to configure its tool support:

```bash
/model use custom-model:latest
```

**Prompt:**
```
Unknown model detected: custom-model:latest
Does this model support function calling (tools)?
  [y] Yes, it supports tools
  [n] No, it doesn't support tools
  [a] Auto-detect (test with a sample request)
```

**Options:**

1. **Yes (y)**: Manually confirm the model supports tools
   - Tools will be enabled for this model
   - Choice is saved to `~/.ollm/user_models.json`

2. **No (n)**: Manually confirm the model doesn't support tools
   - Tools will be disabled for this model
   - Choice is saved to `~/.ollm/user_models.json`

3. **Auto-detect (a)**: Let the system test the model
   - Sends a test request with a minimal tool schema
   - Detects whether the model accepts or rejects tools
   - Takes ~5 seconds with timeout
   - Result is saved automatically

**Timeout:** If you don't respond within 30 seconds, the system defaults to tools disabled (safe default).

**Why this matters:**
- Sending tools to models that don't support them causes errors
- Proper configuration ensures smooth operation
- Your choice is remembered for future sessions

### Auto-Detect Details

When you choose auto-detect, here's what happens:

**Process:**
1. System sends a test request to the model with a minimal tool schema
2. Monitors the response for tool-related errors
3. Times out after 5 seconds if no response
4. Saves the result to `~/.ollm/user_models.json`

**Success indicators:**
- Model accepts the request without tool-related errors
- Tool support is enabled and saved

**Failure indicators:**
- Model returns errors like "unknown field: tools"
- Model returns 400 status with tool-related error messages
- Tool support is disabled and saved

**Fallback:**
- If auto-detect fails or times out, tools are disabled (safe default)
- You can manually update the setting later

**System messages:**
```
Auto-detecting tool support for custom-model:latest...
Tool support detected: Enabled
```

or

```
Auto-detecting tool support for custom-model:latest...
Tool support detected: Disabled
```

---

## Using Model Routing

### What is Model Routing?

Model routing automatically selects appropriate models based on task type, eliminating the need for manual model selection.

### Enable Routing

Add to your configuration (`~/.ollm/config.yaml`):

```yaml
model:
  routing:
    enabled: true
    defaultProfile: general
```

### Routing Profiles

Four built-in profiles optimize for different use cases:

**Fast Profile** - Quick responses with smaller models:
```yaml
routing:
  defaultProfile: fast
```

**General Profile** - Balanced performance for most tasks:
```yaml
routing:
  defaultProfile: general
```

**Code Profile** - Optimized for code generation:
```yaml
routing:
  defaultProfile: code
```

**Creative Profile** - Creative writing and storytelling:
```yaml
routing:
  defaultProfile: creative
```

### Profile Overrides

Specify models for specific profiles:

```yaml
model:
  routing:
    enabled: true
    defaultProfile: general
    overrides:
      code: deepseek-coder:6.7b
      fast: phi3:mini
```

### Manual Override

You can always manually select a model:

```bash
/model use llama3.1:8b
```

This overrides routing for the current session.

---

## Working with Memory

### What is Memory?

The memory system stores facts, preferences, and context across sessions, so you don't have to repeat information.

### Adding Memories

Store information for future sessions:

```bash
# Add a simple memory
/memory add user_name Alice

# Add with category
/memory add preferred_language TypeScript --category preference
```

### Listing Memories

View all stored memories:

```bash
/memory list
```

**Output:**
```
Stored Memories:
  user_name: Alice (preference)
  preferred_language: TypeScript (preference)
  project_type: monorepo (context)
```

### Searching Memories

Find specific memories:

```bash
/memory search project
```

### Forgetting Memories

Remove memories you no longer need:

```bash
# Forget a specific memory
/memory forget old_preference

# Clear all memories
/memory clear
```

### LLM-Initiated Memory

The LLM can also store memories during conversation:

```
User: My name is Alice and I prefer TypeScript.
Assistant: I'll remember that. [Stores: user_name=Alice, preferred_language=TypeScript]
```

### How Memory Works

1. Memories are stored in `~/.ollm/memory.json`
2. At session start, memories are loaded
3. Memories are injected into the system prompt (within token budget)
4. Recently accessed memories are prioritized
5. Access count and timestamps are tracked

---

## Using Templates

### What are Templates?

Templates are reusable prompts with variable substitution, allowing you to quickly use common prompts with different inputs.

### Listing Templates

View available templates:

```bash
/template list
```

**Output:**
```
Available Templates:
  code_review - Review code for quality and security
  explain_code - Explain how code works
  write_tests - Generate unit tests for code
```

### Using Templates

Apply a template with variables:

```bash
# Use a template with variables
/template use code_review language=TypeScript code="function add(a, b) { return a + b; }"

# Variables with spaces
/template use explain_code language="Python" code="def factorial(n): return 1 if n <= 1 else n * factorial(n-1)"
```

### Creating Templates

Create a new template:

```bash
/template create my_template
```

**Template format (YAML):**
```yaml
name: code_review
description: Review code for quality and security
template: "Review this {language} code for {focus:bugs and security}:\n\n{code}"
variables:
  - name: language
    required: true
    description: Programming language
  - name: focus
    required: false
    default: "bugs and security"
    description: Review focus areas
  - name: code
    required: true
    description: Code to review
```

### Template Locations

Templates are loaded from:
- User templates: `~/.ollm/templates/`
- Workspace templates: `.ollm/templates/`

Workspace templates override user templates with the same name.

---

## Project Profiles

### What are Project Profiles?

Project profiles auto-detect your project type and apply appropriate settings automatically.

### Auto-Detection

OLLM CLI detects project type from characteristic files:

```bash
/project detect
```

**Detected types:**
- **TypeScript**: `package.json` with TypeScript dependencies
- **Python**: `requirements.txt`, `pyproject.toml`, `setup.py`
- **Rust**: `Cargo.toml`
- **Go**: `go.mod`

### Using a Profile

Manually select a profile:

```bash
/project use typescript
```

### Initializing a Project

Create a project configuration file:

```bash
/project init
```

This creates `.ollm/project.yaml` with the selected profile settings.

### Project Configuration

Example `.ollm/project.yaml`:

```yaml
# Project profile
profile: typescript

# Override global model
model: deepseek-coder:6.7b

# Project-specific routing
routing:
  defaultProfile: code

# Project-specific options
options:
  temperature: 0.3
  maxTokens: 4096
```

### Built-in Profiles

**TypeScript Profile:**
- Code-optimized model
- Code routing profile
- File and shell tools enabled

**Python Profile:**
- Code-optimized model
- Code routing profile
- Python-specific tools

**Rust Profile:**
- Code-optimized model
- Emphasis on memory safety

**Go Profile:**
- Code-optimized model
- Emphasis on concurrency

**Documentation Profile:**
- Writing-optimized model
- Creative routing profile

---

## Managing Tools

### What are Tools?

Tools are functions that the LLM can call to perform actions like reading files, executing shell commands, searching the web, and more. OLLM CLI provides 15 built-in tools organized into 6 categories.

### Tools Panel

Access the Tools Panel to enable or disable individual tools:

**Navigation:**
- Switch to the Tools tab in the UI
- Use keyboard shortcuts to navigate:
  - `↑/↓`: Navigate between tools
  - `←/→/Enter`: Toggle tool on/off
  - `Tab`: Switch between tabs

**Tool Categories:**

1. **File Operations** (4 tools)
   - `fsWrite`: Create or overwrite files
   - `fsAppend`: Append content to files
   - `strReplace`: Replace text in files
   - `deleteFile`: Delete files

2. **File Discovery** (5 tools)
   - `readFile`: Read file contents
   - `readMultipleFiles`: Read multiple files at once
   - `listDirectory`: List directory contents
   - `fileSearch`: Search for files by name
   - `grepSearch`: Search file contents with regex

3. **Shell** (4 tools)
   - `executePwsh`: Execute shell commands
   - `controlPwshProcess`: Manage background processes
   - `listProcesses`: List running processes
   - `getProcessOutput`: Read process output

4. **Web** (2 tools)
   - `remote_web_search`: Search the web
   - `webFetch`: Fetch content from URLs

5. **Memory** (1 tool)
   - `userInput`: Get input from the user

6. **Context** (4 tools)
   - `prework`: Acceptance criteria testing prework
   - `taskStatus`: Update task status
   - `updatePBTStatus`: Update property-based test status
   - `invokeSubAgent`: Delegate to specialized agents

### Enabling/Disabling Tools

**Why disable tools?**
- Reduce the number of tools sent to the LLM (improves focus)
- Prevent certain actions (e.g., disable shell execution for safety)
- Customize tool availability per project

**How to disable:**
1. Navigate to the Tools tab
2. Use arrow keys to select a tool
3. Press Enter or Left/Right to toggle

**Visual indicators:**
- `[✓]` Tool is enabled
- `[ ]` Tool is disabled

**Persistence:**
- Tool settings are saved to `~/.ollm/settings.json`
- Settings persist across sessions
- Workspace-specific settings can override user settings

### Tool Filtering

Tools are filtered in two stages:

1. **Model Capability Check**: If the current model doesn't support function calling, all tools are automatically disabled
2. **User Preference Check**: Even if the model supports tools, you can disable specific tools via the Tools Panel

**System message when tools are disabled:**
```
Switched to gemma3:1b. Tools: Disabled
```

### Model Tool Support

Some models don't support function calling. When you switch to such a model:

- Tools are automatically disabled
- System prompt includes a note: "This model does not support function calling"
- Tools Panel shows: "Model doesn't support tools"
- You can still view and configure tool preferences for when you switch back to a tool-capable model

---

## Next Steps

### Learn More

**Model Management:**
- [Model Commands Reference](Models_commands.md)
- [Model Architecture](Models_architecture.md)
- [Configuration Guide](Models_configuration.md)

**Routing:**
- [Routing User Guide](3%20projects/OLLM%20CLI/LLM%20Models/routing/user-guide.md)
- [Routing Development Guide](3%20projects/OLLM%20CLI/LLM%20Models/routing/development-guide.md)
- [Profiles Reference](profiles-reference.md)

**Memory:**
- [Memory User Guide](3%20projects/OLLM%20CLI/LLM%20Models/memory/user-guide.md)
- [Memory API Reference](api-reference.md)

**Templates:**
- [Templates User Guide](3%20projects/OLLM%20CLI/LLM%20Models/templates/user-guide.md)
- [Template Reference](template-reference.md)

**Profiles:**
- [Profiles User Guide](3%20projects/OLLM%20CLI/LLM%20Models/profiles/user-guide.md)
- [Built-in Profiles](built-in-profiles.md)

### Advanced Topics

- [Custom Routing Profiles](3%20projects/OLLM%20CLI/LLM%20Models/routing/development-guide.md)
- [Template Libraries](template-reference.md)
- [API Reference](api/)

### Get Help

- [Troubleshooting Guide](../troubleshooting.md)
- GitHub Issues (https://github.com/ollm/ollm-cli/issues)
- [Community Forum](#)

---

**Last Updated:** 2026-01-16  
**Version:** 0.1.0

