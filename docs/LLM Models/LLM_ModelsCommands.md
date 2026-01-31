# Model Management Commands Reference

**Complete CLI Command Reference**

This document provides a comprehensive reference for all model management commands in OLLM CLI.

---

## 📋 Table of Contents

1. [Model Commands](#model-commands)
2. [Memory Commands](#memory-commands)
3. [Template Commands](#template-commands)
4. [Project Commands](#project-commands)
5. [Comparison Commands](#comparison-commands)
6. [Configuration Commands](#configuration-commands)

**See Also:**

- [Getting Started](3%20projects/OLLM%20CLI/LLM%20Models/getting-started.md)
- [Configuration Guide](Models_configuration.md)
- [Model Architecture](Models_architecture.md)

---

## Model Commands

### `/model list`

List all available models from the current provider.

**Syntax:**

```bash
/model list
```

**Output:**

```
Available Models:
  ● llama3.1:8b (loaded)     4.7 GB   Modified 2 days ago
    mistral:7b               4.1 GB   Modified 1 week ago
    codellama:7b             3.8 GB   Modified 3 days ago
    phi3:mini                2.3 GB   Modified 1 month ago
```

**Information displayed:**

- Model name and version
- Size on disk
- Last modified date
- Load status (● = loaded)

---

### `/model pull <name>`

Download a model from the provider registry.

**Syntax:**

```bash
/model pull <model-name>
```

**Examples:**

```bash
# Pull a specific model
/model pull llama3.1:8b

# Pull with tag
/model pull mistral:7b-instruct

# Pull latest version
/model pull qwen:latest
```

**Features:**

- Real-time progress display
- Transfer rate monitoring
- Cancellable with Ctrl+C
- Automatic cache invalidation

**Progress display:**

```
Pulling llama3.1:8b...
[████████████████████████] 100% 4.7 GB @ 12.3 MB/s
Model llama3.1:8b ready.
```

---

### `/model delete <name>`

Remove a model to free disk space.

**Syntax:**

```bash
/model delete <model-name>
```

**Examples:**

```bash
# Delete a specific model
/model delete codellama:7b

# Delete with confirmation
/model delete old-model:13b
```

**Safety features:**

- Confirmation prompt
- Shows space to be freed
- Automatic unload if currently loaded
- Cannot delete active model (switch first)

**Example interaction:**

```
Delete codellama:7b? This will free 3.8 GB. [y/N] y
Unloading model...
Deleting model files...
Model codellama:7b removed successfully.
```

---

### `/model info <name>`

View detailed model metadata and capabilities.

**Syntax:**

```bash
/model info <model-name>
```

**Examples:**

```bash
# View model information
/model info llama3.1:8b

# Check capabilities
/model info mistral:7b
```

**Information displayed:**

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
License: Llama 3.1 Community License
```

---

### `/model use <name>`

Switch to a specific model.

**Syntax:**

```bash
/model use <model-name>
```

**Examples:**

```bash
# Switch to a model
/model use llama3.1:8b

# Override routing
/model use deepseek-coder:6.7b
```

**Behavior:**

- Overrides routing for current session
- Loads model if not already loaded
- Updates status bar

---

### `/model keep <name>`

Keep a model loaded in VRAM for faster responses.

**Syntax:**

```bash
/model keep <model-name>
```

**Examples:**

```bash
# Keep model loaded
/model keep llama3.1:8b

# Keep multiple models
/model keep mistral:7b
/model keep codellama:7b
```

**Benefits:**

- Eliminates model load time (2-5 seconds)
- Reduces latency for subsequent requests
- Useful for interactive sessions

**How it works:**

- Sends periodic keep-alive requests to provider
- Prevents automatic unloading due to inactivity
- Continues until manual unload or timeout

---

### `/model unload <name>`

Unload a model from VRAM.

**Syntax:**

```bash
/model unload <model-name>
```

**Examples:**

```bash
# Unload a model
/model unload llama3.1:8b

# Free VRAM
/model unload old-model:13b
```

**When to unload:**

- Switching to a different model
- Freeing VRAM for other applications
- Reducing power consumption

---

## Memory Commands

### `/memory list`

Show all stored memories.

**Syntax:**

```bash
/memory list
```

**Output:**

```
Stored Memories:
  user_name: Alice (preference) - Accessed 5 times
  preferred_language: TypeScript (preference) - Accessed 3 times
  project_type: monorepo (context) - Accessed 2 times
  last_task: code review (fact) - Accessed 1 time
```

**Information displayed:**

- Key and value
- Category (fact, preference, context)
- Access count
- Last accessed date

---

### `/memory add <key> <value>`

Add a new memory.

**Syntax:**

```bash
/memory add <key> <value> [--category <category>]
```

**Examples:**

```bash
# Add a simple memory
/memory add user_name Alice

# Add with category
/memory add preferred_language TypeScript --category preference

# Add a fact
/memory add project_started "2026-01-15" --category fact

# Add context
/memory add working_on "authentication system" --category context
```

**Categories:**

- `fact`: Factual information
- `preference`: User preferences
- `context`: Contextual information

---

### `/memory search <query>`

Search memories by key or value.

**Syntax:**

```bash
/memory search <query>
```

**Examples:**

```bash
# Search by key
/memory search user

# Search by value
/memory search TypeScript

# Search by partial match
/memory search project
```

**Output:**

```
Found 2 memories:
  user_name: Alice (preference)
  user_email: alice@example.com (fact)
```

---

### `/memory forget <key>`

Remove a specific memory.

**Syntax:**

```bash
/memory forget <key>
```

**Examples:**

```bash
# Forget a memory
/memory forget old_preference

# Remove outdated information
/memory forget deprecated_setting
```

---

### `/memory clear`

Clear all memories.

**Syntax:**

```bash
/memory clear
```

**Safety:**

- Confirmation prompt
- Cannot be undone
- Backup recommended

**Example interaction:**

```
Clear all memories? This cannot be undone. [y/N] y
Cleared 12 memories.
```

---

## Template Commands

### `/template list`

Show all available templates.

**Syntax:**

```bash
/template list
```

**Output:**

```
Available Templates:
  code_review (3 variables) - Review code for quality and security
  explain_code (2 variables) - Explain how code works
  write_tests (3 variables) - Generate unit tests for code
  refactor (2 variables) - Suggest code refactoring
```

**Information displayed:**

- Template name
- Number of variables
- Description

---

### `/template use <name> [variables...]`

Use a template with variable substitution.

**Syntax:**

```bash
/template use <template-name> [variable=value ...]
```

**Examples:**

```bash
# Use with all variables
/template use code_review language=TypeScript code="function add(a, b) { return a + b; }"

# Use with default values
/template use code_review language=TypeScript code="..."

# Variables with spaces
/template use explain_code language="Python" code="def factorial(n): return 1 if n <= 1 else n * factorial(n-1)"
```

**Variable syntax:**

- `variable=value` - Set variable
- `variable="value with spaces"` - Value with spaces
- Missing optional variables use defaults
- Missing required variables prompt user

---

### `/template create <name>`

Create a new template.

**Syntax:**

```bash
/template create <template-name>
```

**Examples:**

```bash
# Create a new template
/template create my_template

# Create with editor
/template create code_analysis
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
    default: 'bugs and security'
    description: Review focus areas
  - name: code
    required: true
    description: Code to review
```

**Saved to:** `~/.ollm/templates/<name>.yaml`

---

### `/template delete <name>`

Delete a template.

**Syntax:**

```bash
/template delete <template-name>
```

**Examples:**

```bash
# Delete a template
/template delete old_template

# Remove unused template
/template delete deprecated
```

---

## Project Commands

### `/project detect`

Auto-detect project type from workspace files.

**Syntax:**

```bash
/project detect
```

**Output:**

```
Detected project type: TypeScript
Characteristic files:
  - package.json (with TypeScript dependencies)
  - tsconfig.json

Suggested profile: typescript
```

**Detection rules:**

- **TypeScript**: `package.json` with TypeScript dependencies
- **Python**: `requirements.txt`, `pyproject.toml`, `setup.py`
- **Rust**: `Cargo.toml`
- **Go**: `go.mod`

---

### `/project use <profile>`

Select a project profile.

**Syntax:**

```bash
/project use <profile-name>
```

**Examples:**

```bash
# Use TypeScript profile
/project use typescript

# Use Python profile
/project use python

# Use custom profile
/project use my-profile
```

**Built-in profiles:**

- `typescript` - TypeScript projects
- `python` - Python projects
- `rust` - Rust projects
- `go` - Go projects
- `documentation` - Documentation projects

---

### `/project init`

Initialize project configuration file.

**Syntax:**

```bash
/project init [profile-name]
```

**Examples:**

```bash
# Initialize with detected profile
/project init

# Initialize with specific profile
/project init typescript

# Initialize with custom settings
/project init my-profile
```

**Creates:** `.ollm/project.yaml`

**Example file:**

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

---

## Comparison Commands

### `/compare "<prompt>" <model1> <model2> [model3...]`

Run the same prompt through multiple models for comparison.

**Syntax:**

```bash
/compare "<prompt>" <model1> <model2> [model3...]
```

**Examples:**

```bash
# Compare two models
/compare "Explain recursion" llama3.1:8b mistral:7b

# Compare three models
/compare "Write a sorting algorithm" codellama:7b deepseek-coder:6.7b qwen:7b

# Compare with complex prompt
/compare "Review this code: function add(a, b) { return a + b; }" llama3.1:8b codellama:7b
```

**Output:**

```
Comparison Results:

┌─────────────────┬──────────────┬──────────┬─────────────┐
│ Model           │ Tokens       │ Latency  │ Tokens/sec  │
├─────────────────┼──────────────┼──────────┼─────────────┤
│ llama3.1:8b     │ 245          │ 2.3s     │ 106.5       │
│ mistral:7b      │ 198          │ 1.8s     │ 110.0       │
│ codellama:7b    │ 312          │ 3.1s     │ 100.6       │
└─────────────────┴──────────────┴──────────┴─────────────┘

[Responses displayed side-by-side below]
```

**Features:**

- Parallel execution
- Performance metrics
- Side-by-side display
- Partial failure handling

---

## Configuration Commands

### `/config show`

Display current configuration.

**Syntax:**

```bash
/config show
```

**Output:**

```
Current Configuration:

Model:
  Default: llama3.1:8b
  Routing: enabled (general profile)
  Keep-alive: enabled (1 model)

Memory:
  Enabled: true
  Token budget: 500
  Stored memories: 12

Templates:
  User templates: 5
  Workspace templates: 2

Project:
  Profile: typescript
  Auto-detect: true
```

---

### `/config set <key> <value>`

Set a configuration value.

**Syntax:**

```bash
/config set <key> <value>
```

**Examples:**

```bash
# Set default model
/config set model.default llama3.1:8b

# Enable routing
/config set model.routing.enabled true

# Set routing profile
/config set model.routing.defaultProfile code

# Set temperature
/config set options.temperature 0.7

# Set memory token budget
/config set memory.tokenBudget 500
```

---

### `/config reset`

Reset configuration to defaults.

**Syntax:**

```bash
/config reset [section]
```

**Examples:**

```bash
# Reset all configuration
/config reset

# Reset model configuration
/config reset model

# Reset memory configuration
/config reset memory
```

---

## Environment Variables

Configuration can also be set via environment variables:

```bash
# Model selection
export OLLM_MODEL=llama3.1:8b

# Generation parameters
export OLLM_TEMPERATURE=0.7
export OLLM_MAX_TOKENS=4096
export OLLM_CONTEXT_SIZE=8192

# Provider URL
export OLLAMA_HOST=http://localhost:11434
```

**Precedence:** Environment variables > Project config > Global config

---

## Command Aliases

Some commands have shorter aliases:

| Command           | Alias        |
| ----------------- | ------------ |
| `/model list`     | `/models`    |
| `/model use`      | `/use`       |
| `/memory list`    | `/memories`  |
| `/template list`  | `/templates` |
| `/project detect` | `/detect`    |

---

## See Also

- [Getting Started](3%20projects/OLLM%20CLI/LLM%20Models/getting-started.md)
- [Configuration Guide](Models_configuration.md)
- [Model Architecture](Models_architecture.md)
- [Routing Guide](3%20projects/OLLM%20CLI/LLM%20Models/routing/user-guide.md)
- [Memory Guide](3%20projects/OLLM%20CLI/LLM%20Models/memory/user-guide.md)
- [Templates Guide](3%20projects/OLLM%20CLI/LLM%20Models/templates/user-guide.md)
- [Profiles Guide](3%20projects/OLLM%20CLI/LLM%20Models/profiles/user-guide.md)

---

**Last Updated:** 2026-01-16  
**Version:** 0.1.6
