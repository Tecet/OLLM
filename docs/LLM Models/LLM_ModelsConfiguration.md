# Model Management Configuration

**Complete Configuration Guide**

This document covers all configuration options for the Model Management system, including model settings, routing, memory, templates, and project profiles.

---

## Table of Contents

1. [Configuration Files](#configuration-files)
2. [Model Management](#model-management)
   - [Unknown Model Handling](#unknown-model-handling)
   - [Cache Settings](#cache-settings)
   - [Keep-Alive Settings](#keep-alive-settings)
   - [Auto-Pull Settings](#auto-pull-settings)
   - [Tool Configuration](#tool-configuration)
3. [Model Routing](#model-routing)
4. [Memory System](#memory-system)
5. [Template System](#template-system)
6. [Project Profiles](#project-profiles)
7. [Environment Variables](#environment-variables)
8. [Configuration Precedence](#configuration-precedence)

---

## Configuration Files

### User Configuration

**Location:** `~/.ollm/config.yaml`

**Purpose:** Global settings for all projects

**Example:**

```yaml
# Model management
models:
  cache_ttl: 60
  keep_alive: 300
  auto_pull: false

# Routing
routing:
  profile: general
  preferred_families:
    - llama
    - mistral
  fallback_profile: fast

# Memory
memory:
  enabled: true
  system_prompt_budget: 500
  file: ~/.ollm/memory.json

# Templates
templates:
  user_dir: ~/.ollm/templates
  workspace_dir: .ollm/templates

# Project profiles
project:
  auto_detect: true
  profile: null
```

### Project Configuration

**Location:** `.ollm/config.yaml` (in project root)

**Purpose:** Project-specific settings (override user settings)

**Example:**

```yaml
# Project-specific model
model: codellama:13b

# Project-specific routing
routing:
  profile: code
  preferred_families:
    - codellama
    - deepseek-coder

# Project-specific memory
memory:
  enabled: true
  system_prompt_budget: 1000

# Project profile
project:
  profile: typescript
```

### Project Profile

**Location:** `.ollm/project.yaml`

**Purpose:** Project type and settings

**Example:**

```yaml
# Project metadata
name: my-project
type: typescript
version: 1.0.0

# Model settings
model: llama3.1:8b
system_prompt: |
  You are a TypeScript expert helping with a web application.
  Follow TypeScript best practices and modern patterns.

# Tool restrictions
tools:
  allowed:
    - read_file
    - write_file
    - shell
  denied:
    - web_fetch

# Routing
routing:
  profile: code
  preferred_families:
    - llama
    - qwen
```

---

## Model Management

### Unknown Model Handling

**Overview:** When you install a model that isn't in OLLM's database, the system automatically creates a profile using the "user-unknown-model" template. This allows you to use any model with Ollama, even if it's not officially supported.

**How It Works:**

1. **Automatic Detection:** On startup, OLLM queries Ollama for installed models
2. **Database Matching:** Each model is matched against the master database
3. **Template Application:** Unknown models receive default settings based on Llama 3.2 3B
4. **User Customization:** You can manually edit the generated profile

**User Profile Location:**

- **Windows:** `C:\Users\{username}\.ollm\LLM_profiles.json`
- **Linux/Mac:** `~/.ollm/LLM_profiles.json`

**Example Unknown Model Entry:**

```json
{
  "id": "custom-model:latest",
  "name": "Unknown Model (custom-model:latest)",
  "creator": "User",
  "parameters": "Based on Llama 3.2 3B",
  "quantization": "Based on Llama 3.2 3B (4-bit estimated)",
  "description": "Unknown model \"custom-model:latest\". Please edit your settings at ~/.ollm/LLM_profiles.json",
  "abilities": ["Unknown"],
  "tool_support": false,
  "ollama_url": "Unknown",
  "max_context_window": 131072,
  "context_profiles": [
    {
      "size": 4096,
      "size_label": "4k",
      "vram_estimate": "2.5 GB",
      "ollama_context_size": 2867,
      "vram_estimate_gb": 2.5
    }
    // ... more context profiles
  ],
  "default_context": 4096
}
```

**Customizing Unknown Models:**

1. **Locate the file:**

   ```bash
   # Windows
   notepad %USERPROFILE%\.ollm\LLM_profiles.json

   # Linux/Mac
   nano ~/.ollm/LLM_profiles.json
   ```

2. **Find your model:** Search for the model ID (e.g., "custom-model:latest")

3. **Update fields:**

   ```json
   {
     "id": "custom-model:latest",
     "name": "My Custom Model 7B", // ← Update display name
     "creator": "Custom Creator", // ← Update creator
     "parameters": "7B", // ← Update parameter count
     "quantization": "4-bit", // ← Update quantization
     "description": "My custom fine-tuned model",
     "abilities": ["Coding", "Math"], // ← Update capabilities
     "tool_support": true, // ← Enable if model supports tools
     "ollama_url": "https://...", // ← Add documentation link
     "context_profiles": [
       {
         "size": 4096,
         "vram_estimate": "5.5 GB", // ← Adjust VRAM estimates
         "ollama_context_size": 3482,
         "vram_estimate_gb": 5.5
       }
       // ... adjust other profiles
     ]
   }
   ```

4. **Save and restart:** Your changes persist across app restarts

**Important Notes:**

- **Preservation:** Your edits are preserved when the profile is recompiled
- **Context Sizes:** The `ollama_context_size` values are pre-calculated at 85% of the `size` value
- **VRAM Estimates:** Adjust based on your actual GPU memory usage
- **Tool Support:** Only enable if your model supports function calling
- **Default Template:** Based on Llama 3.2 3B (3.2B parameters, 4-bit quantization)

**Common Customizations:**

**For Larger Models (13B+):**

```json
{
  "parameters": "13B",
  "context_profiles": [
    {
      "size": 4096,
      "vram_estimate": "8.5 GB",
      "vram_estimate_gb": 8.5
    }
  ]
}
```

**For Code-Specialized Models:**

```json
{
  "abilities": ["Coding", "Debugging", "Code Review"],
  "tool_support": true
}
```

**For Reasoning Models:**

```json
{
  "abilities": ["Reasoning", "Math", "Logic"],
  "thinking_enabled": true,
  "reasoning_buffer": "Variable",
  "warmup_timeout": 120000
}
```

**Troubleshooting:**

**Model Not Detected:**

- Ensure Ollama is running: `curl http://localhost:11434/api/tags`
- Check model is installed: `ollama list`
- Restart OLLM to trigger recompilation

**Wrong VRAM Estimates:**

- Monitor actual usage with `nvidia-smi` (NVIDIA) or `rocm-smi` (AMD)
- Update `vram_estimate_gb` values in your profile
- Consider reducing context size if running out of memory

**Tool Support Not Working:**

- Verify model actually supports function calling
- Check Ollama model documentation
- Test with simple tool call before enabling

**See Also:**

- [Model Compiler System](../../.dev/docs/knowledgeDB/dev_ModelCompiler.md) - Technical details
- [Model Database](../../.dev/docs/knowledgeDB/dev_ModelDB.md) - Database schema
- [Model Management](../../.dev/docs/knowledgeDB/dev_ModelManagement.md) - Model selection

---

### Cache Settings

**Option:** `models.cache_ttl`  
**Type:** Number (seconds)  
**Default:** 60  
**Description:** How long to cache model list

**Example:**

```yaml
models:
  cache_ttl: 120 # Cache for 2 minutes
```

**Impact:**

- Higher values: Fewer provider calls, faster responses, stale data
- Lower values: More provider calls, slower responses, fresh data

### Keep-Alive Settings

**Option:** `models.keep_alive`  
**Type:** Number (seconds)  
**Default:** 300  
**Description:** How long to keep models loaded

**Example:**

```yaml
models:
  keep_alive: 600 # Keep loaded for 10 minutes
```

**Impact:**

- Higher values: Models stay in memory longer, faster responses, more VRAM used
- Lower values: Models unload sooner, slower responses, less VRAM used

**Special Values:**

- `0`: Unload immediately after use
- `-1`: Keep loaded indefinitely

### Auto-Pull Settings

**Option:** `models.auto_pull`  
**Type:** Boolean  
**Default:** false  
**Description:** Automatically pull missing models

**Example:**

```yaml
models:
  auto_pull: true # Pull models automatically
```

**Impact:**

- `true`: Convenient, but may download large files unexpectedly
- `false`: Manual control, but requires explicit pull commands

### Tool Configuration

**Option:** `tools`  
**Type:** Object (map of tool ID to boolean)  
**Default:** All tools enabled  
**Description:** Enable or disable individual tools

**Example:**

```yaml
tools:
  executePwsh: false # Disable shell execution
  controlPwshProcess: false # Disable process management
  remote_web_search: true # Enable web search
  webFetch: true # Enable web fetch
```

**Available Tools:**

**File Operations:**

- `fsWrite`: Create or overwrite files
- `fsAppend`: Append content to files
- `strReplace`: Replace text in files
- `deleteFile`: Delete files

**File Discovery:**

- `readFile`: Read file contents
- `readMultipleFiles`: Read multiple files
- `listDirectory`: List directory contents
- `fileSearch`: Search for files by name
- `grepSearch`: Search file contents with regex

**Shell:**

- `executePwsh`: Execute shell commands
- `controlPwshProcess`: Manage background processes
- `listProcesses`: List running processes
- `getProcessOutput`: Read process output

**Web:**

- `remote_web_search`: Search the web
- `webFetch`: Fetch content from URLs

**Memory:**

- `userInput`: Get input from the user

**Context:**

- `prework`: Acceptance criteria testing prework
- `taskStatus`: Update task status
- `updatePBTStatus`: Update property-based test status
- `invokeSubAgent`: Delegate to specialized agents

**Persistence:**

- Tool settings are saved to `~/.ollm/settings.json`
- Settings persist across sessions
- Workspace settings (`.ollm/settings.json`) override user settings

**Tool Filtering:**
Tools are filtered in two stages:

1. **Model Capability**: If model doesn't support function calling, all tools are disabled
2. **User Preference**: Disabled tools are never sent to the LLM

**Use Cases:**

- Disable shell tools for safety in untrusted environments
- Disable web tools for offline work
- Reduce tool count to improve LLM focus
- Project-specific tool restrictions

---

## Model Routing

### Routing Profile

**Option:** `routing.profile`  
**Type:** String  
**Default:** general  
**Values:** fast, general, code, creative  
**Description:** Task profile for model selection

**Example:**

```yaml
routing:
  profile: code # Optimize for coding tasks
```

**Profiles:**

- `fast`: Quick responses, smaller models
- `general`: Balanced performance
- `code`: Programming tasks, code-specialized models
- `creative`: Creative writing, larger models

### Preferred Families

**Option:** `routing.preferred_families`  
**Type:** Array of strings  
**Default:** []  
**Description:** Model families to prefer

**Example:**

```yaml
routing:
  preferred_families:
    - llama # Prefer Llama models
    - mistral # Then Mistral models
    - qwen # Then Qwen models
```

**Impact:**

- Models from preferred families score higher
- Order matters (first is most preferred)
- Empty array means no preference

### Fallback Profile

**Option:** `routing.fallback_profile`  
**Type:** String  
**Default:** general  
**Values:** fast, general, code, creative  
**Description:** Profile to use if primary fails

**Example:**

```yaml
routing:
  fallback_profile: fast # Fall back to fast profile
```

**Impact:**

- Used when no models match primary profile
- Prevents selection failures
- Can chain multiple fallbacks

### Manual Override

**Option:** `model`  
**Type:** String  
**Default:** null  
**Description:** Manually specify model (bypasses routing)

**Example:**

```yaml
model: llama3.1:8b # Always use this model
```

**Impact:**

- Overrides routing completely
- Useful for testing specific models
- Disables automatic selection

---

## Memory System

### Enable/Disable

**Option:** `memory.enabled`  
**Type:** Boolean  
**Default:** true  
**Description:** Enable memory system

**Example:**

```yaml
memory:
  enabled: false # Disable memory
```

**Impact:**

- `true`: Memories injected into system prompt
- `false`: No memory injection, faster responses

### System Prompt Budget

**Option:** `memory.system_prompt_budget`  
**Type:** Number (tokens)  
**Default:** 500  
**Description:** Maximum tokens for memory injection

**Example:**

```yaml
memory:
  system_prompt_budget: 1000 # Allow more memories
```

**Impact:**

- Higher values: More memories included, less room for conversation
- Lower values: Fewer memories included, more room for conversation

**Recommendations:**

- Small context (2K): 200-300 tokens
- Medium context (8K): 500-800 tokens
- Large context (32K+): 1000-2000 tokens

### Memory File

**Option:** `memory.file`  
**Type:** String (path)  
**Default:** ~/.ollm/memory.json  
**Description:** Location of memory storage

**Example:**

```yaml
memory:
  file: /custom/path/memory.json
```

**Impact:**

- Different projects can have different memory files
- Useful for isolation or sharing

---

## Template System

### User Template Directory

**Option:** `templates.user_dir`  
**Type:** String (path)  
**Default:** ~/.ollm/templates  
**Description:** User-level template directory

**Example:**

```yaml
templates:
  user_dir: ~/my-templates
```

**Impact:**

- Templates available across all projects
- Good for personal templates

### Workspace Template Directory

**Option:** `templates.workspace_dir`  
**Type:** String (path)  
**Default:** .ollm/templates  
**Description:** Project-level template directory

**Example:**

```yaml
templates:
  workspace_dir: .templates
```

**Impact:**

- Templates specific to project
- Workspace templates override user templates
- Good for team-shared templates

---

## Project Profiles

### Auto-Detection

**Option:** `project.auto_detect`  
**Type:** Boolean  
**Default:** true  
**Description:** Automatically detect project type

**Example:**

```yaml
project:
  auto_detect: false # Disable auto-detection
```

**Impact:**

- `true`: Automatic profile selection based on files
- `false`: Manual profile selection required

**Detection Rules:**

- TypeScript: package.json with typescript dependency
- Python: requirements.txt or setup.py
- Rust: Cargo.toml
- Go: go.mod
- Documentation: docs/ directory

### Manual Profile

**Option:** `project.profile`  
**Type:** String  
**Default:** null  
**Values:** typescript, python, rust, go, documentation  
**Description:** Manually specify project profile

**Example:**

```yaml
project:
  profile: typescript # Force TypeScript profile
```

**Impact:**

- Overrides auto-detection
- Useful when detection fails
- Applies profile-specific settings

---

## Environment Variables

### Model Override

**Variable:** `OLLM_MODEL`  
**Type:** String  
**Description:** Override default model

**Example:**

```bash
export OLLM_MODEL=llama3.1:8b
```

**Precedence:** Highest (overrides all config)

### Temperature

**Variable:** `OLLM_TEMPERATURE`  
**Type:** Number (0.0-2.0)  
**Description:** Override temperature

**Example:**

```bash
export OLLM_TEMPERATURE=0.7
```

### Max Tokens

**Variable:** `OLLM_MAX_TOKENS`  
**Type:** Number  
**Description:** Override max tokens

**Example:**

```bash
export OLLM_MAX_TOKENS=2048
```

### Context Size

**Variable:** `OLLM_CONTEXT_SIZE`  
**Type:** Number  
**Description:** Override context window

**Example:**

```bash
export OLLM_CONTEXT_SIZE=8192
```

### Ollama Host

**Variable:** `OLLAMA_HOST`  
**Type:** String (URL)  
**Default:** http://localhost:11434  
**Description:** Ollama server URL

**Example:**

```bash
export OLLAMA_HOST=http://remote-server:11434
```

### Log Level

**Variable:** `OLLM_LOG_LEVEL`  
**Type:** String  
**Values:** debug, info, warn, error  
**Default:** info  
**Description:** Logging verbosity

**Example:**

```bash
export OLLM_LOG_LEVEL=debug
```

---

## Configuration Precedence

### Order (Highest to Lowest)

1. **Environment Variables** (highest)
   - `OLLM_MODEL`, `OLLM_TEMPERATURE`, etc.
   - Override everything

2. **Command-Line Arguments**
   - `--model`, `--temperature`, etc.
   - Override config files

3. **Project Configuration**
   - `.ollm/config.yaml`
   - Project-specific settings

4. **User Configuration**
   - `~/.ollm/config.yaml`
   - Global settings

5. **Defaults** (lowest)
   - Built-in defaults
   - Used when nothing else specified

### Example

```yaml
# User config (~/.ollm/config.yaml)
model: llama3.1:8b
temperature: 0.7

# Project config (.ollm/config.yaml)
model: codellama:13b  # Overrides user config

# Environment variable
export OLLM_MODEL=mistral:7b  # Overrides project config

# Final result: mistral:7b is used
```

---

## Complete Configuration Example

### User Configuration

```yaml
# ~/.ollm/config.yaml

# Model management
models:
  cache_ttl: 60
  keep_alive: 300
  auto_pull: false

# Default model
model: llama3.1:8b

# Generation options
options:
  temperature: 0.7
  top_p: 0.9
  top_k: 40
  repeat_penalty: 1.1

# Routing
routing:
  profile: general
  preferred_families:
    - llama
    - mistral
  fallback_profile: fast

# Memory
memory:
  enabled: true
  system_prompt_budget: 500
  file: ~/.ollm/memory.json

# Templates
templates:
  user_dir: ~/.ollm/templates
  workspace_dir: .ollm/templates

# Project profiles
project:
  auto_detect: true
  profile: null

# Logging
log_level: info
```

### Project Configuration

```yaml
# .ollm/config.yaml

# Project-specific model
model: codellama:13b

# Project-specific routing
routing:
  profile: code
  preferred_families:
    - codellama
    - deepseek-coder
    - qwen

# Higher memory budget for this project
memory:
  system_prompt_budget: 1000

# Project profile
project:
  profile: typescript
```

### Project Profile

```yaml
# .ollm/project.yaml

# Project metadata
name: my-web-app
type: typescript
version: 1.0.0
description: A TypeScript web application

# Model settings
model: llama3.1:8b
system_prompt: |
  You are a TypeScript expert helping with a React web application.
  Follow TypeScript best practices, use modern React patterns,
  and prioritize type safety.

# Tool restrictions
tools:
  allowed:
    - read_file
    - write_file
    - shell
    - grep
    - glob
  denied:
    - web_fetch
    - web_search

# Routing
routing:
  profile: code
  preferred_families:
    - llama
    - qwen
    - codellama

# Memory
memory:
  enabled: true
  system_prompt_budget: 800
```

---

## Configuration Tips

### Performance Optimization

```yaml
# Fast responses
models:
  cache_ttl: 120 # Cache longer
  keep_alive: 600 # Keep models loaded

routing:
  profile: fast # Use smaller models
  preferred_families:
    - phi # Fast models
    - gemma
```

### Quality Optimization

```yaml
# Best quality
models:
  keep_alive: -1 # Keep loaded indefinitely

routing:
  profile: general # Use larger models
  preferred_families:
    - llama # High-quality models
    - qwen

options:
  temperature: 0.3 # More focused
  top_p: 0.95
```

### Memory Optimization

```yaml
# Minimize memory usage
models:
  keep_alive: 0 # Unload immediately

memory:
  enabled: false # Disable memory injection

routing:
  profile: fast # Use smaller models
```

### Development Setup

```yaml
# Development configuration
models:
  auto_pull: true # Auto-download models

memory:
  enabled: true
  system_prompt_budget: 1000

templates:
  workspace_dir: .templates

project:
  auto_detect: true

log_level: debug # Verbose logging
```

---

## Troubleshooting

### Model Not Found

**Problem:** Model not available

**Solution:**

```yaml
models:
  auto_pull: true # Enable auto-pull
```

Or manually pull:

```bash
/model pull llama3.1:8b
```

### Slow Responses

**Problem:** Model takes too long to respond

**Solution:**

```yaml
routing:
  profile: fast # Use faster models
  preferred_families:
    - phi
    - gemma

models:
  keep_alive: 600 # Keep models loaded
```

### Memory Not Injected

**Problem:** Memories not appearing in responses

**Solution:**

```yaml
memory:
  enabled: true # Enable memory
  system_prompt_budget: 1000 # Increase budget
```

### Wrong Model Selected

**Problem:** Routing selects unexpected model

**Solution:**

```yaml
model: llama3.1:8b # Manual override
```

Or adjust routing:

```yaml
routing:
  preferred_families:
    - llama # Prefer specific family
```

---

## See Also

### User Documentation

- [Getting Started](3%20projects/OLLM%20CLI/LLM%20Models/getting-started.md) - Quick start guide
- [Commands Reference](Models_commands.md) - CLI commands
- [Architecture](Models_architecture.md) - System design
- [Routing Guide](3%20projects/OLLM%20CLI/LLM%20Models/routing/user-guide.md) - Routing details
- [Memory Guide](3%20projects/OLLM%20CLI/LLM%20Models/memory/user-guide.md) - Memory system
- [Template Guide](3%20projects/OLLM%20CLI/LLM%20Models/templates/user-guide.md) - Templates
- [Profile Guide](3%20projects/OLLM%20CLI/LLM%20Models/profiles/user-guide.md) - Project profiles

### Developer Documentation

- [Model Compiler System](../../.dev/docs/knowledgeDB/dev_ModelCompiler.md) - Profile compilation system
- [Model Database](../../.dev/docs/knowledgeDB/dev_ModelDB.md) - Database schema and access patterns
- [Model Management](../../.dev/docs/knowledgeDB/dev_ModelManagement.md) - Model selection and profiles

---

**Document Version:** 1.1  
**Last Updated:** 2026-01-27  
**Status:** Complete
