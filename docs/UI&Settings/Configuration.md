# Configuration Reference

## Overview

OLLM CLI uses a layered configuration system that allows you to customize behavior at multiple levels. Configuration can be specified through config files, environment variables, and command-line flags, with a clear precedence order.

## Configuration File Locations

OLLM CLI looks for configuration files in two locations:

### User Configuration

**Location:** `~/.ollm/config.yaml`

This is your personal configuration that applies to all OLLM CLI sessions across all projects. Use this for your preferred defaults like model selection, UI preferences, and provider endpoints.

**Example path:**
- Linux/macOS: `/home/username/.ollm/config.yaml`
- Windows: `C:\Users\username\.ollm\config.yaml`

### Workspace Configuration

**Location:** `.ollm/config.yaml` (in your project directory)

This is project-specific configuration that only applies when running OLLM CLI from within that project directory. Use this for project-specific settings like custom models, tool policies, or provider configurations.

**Example path:**
- Linux/macOS: `/path/to/project/.ollm/config.yaml`
- Windows: `C:\path\to\project\.ollm\config.yaml`

## Configuration Precedence

When multiple configuration sources are present, they are merged with the following precedence order (highest priority first):

1. **CLI Flags** - Command-line arguments (e.g., `--model llama3.1`)
2. **Environment Variables** - Environment variables (e.g., `OLLAMA_HOST`)
3. **Workspace Config** - Project-specific `.ollm/config.yaml`
4. **User Config** - Personal `~/.ollm/config.yaml`
5. **System Defaults** - Built-in default values

Settings from higher priority sources override settings from lower priority sources. For nested objects, the merge is performed at the field level, not the object level.

### Precedence Example

```yaml
# User config (~/.ollm/config.yaml)
model:
  default: llama3.2:3b
  temperature: 0.7

# Workspace config (.ollm/config.yaml)
model:
  default: codellama:13b

# Result: workspace model overrides user model, but temperature is inherited
# Final config:
#   model.default: codellama:13b
#   model.temperature: 0.7
```

### CLI Flag Precedence

```bash
# User config has model: llama3.2:3b
# This command overrides it:
ollm --model llama3.1:8b

# Final model used: llama3.1:8b
```

### Environment Variable Precedence

```bash
# User config has provider.ollama.host: http://localhost:11434
# This environment variable overrides it:
export OLLAMA_HOST=http://192.168.1.100:11434
ollm

# Final host used: http://192.168.1.100:11434
```

## Configuration File Format

Configuration files use YAML format. Here's the basic structure:

```yaml
# Provider configuration
provider:
  default: ollama
  ollama:
    host: http://localhost:11434
    timeout: 30000

# Model configuration
model:
  default: llama3.2:3b
  temperature: 0.7
  maxTokens: 4096

# UI configuration
ui:
  layout: hybrid
  sidePanel: true
  showGpuStats: true
  showCost: true

# Status monitoring
status:
  pollInterval: 5000
  highTempThreshold: 80
  lowVramThreshold: 512

# Diff review
review:
  enabled: true
  inlineThreshold: 5

# Session management
session:
  autoSave: true
  saveInterval: 60000
```


## Settings Reference

### Provider Settings

Controls which LLM provider to use and how to connect to it.

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `provider.default` | string | `"ollama"` | Default provider to use (`ollama`, `vllm`, `openai-compatible`) |
| `provider.ollama.host` | string (URI) | `"http://localhost:11434"` | Ollama server endpoint URL |
| `provider.ollama.timeout` | number | `30000` | Request timeout in milliseconds (min: 0) |
| `provider.vllm.host` | string (URI) | - | vLLM server endpoint URL |
| `provider.vllm.apiKey` | string | - | Optional API key for vLLM authentication |
| `provider.openaiCompatible.host` | string (URI) | - | OpenAI-compatible server endpoint URL |
| `provider.openaiCompatible.apiKey` | string | - | Optional API key for OpenAI-compatible authentication |

**Configuration Methods:**

```yaml
# Config file
provider:
  default: ollama
  ollama:
    host: http://localhost:11434
    timeout: 30000
```

```bash
# Environment variables
export OLLAMA_HOST=http://localhost:11434
export VLLM_HOST=http://localhost:8000
export VLLM_API_KEY=your-api-key
export OPENAI_COMPATIBLE_HOST=http://localhost:1234
export OPENAI_COMPATIBLE_API_KEY=your-api-key

# CLI flags
ollm --provider ollama --host http://localhost:11434
```


### Model Settings

Controls model selection and generation parameters.

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `model.default` | string | `"llama3.2:3b"` | Default model name to use |
| `model.temperature` | number | `0.7` | Sampling temperature (0.0-2.0, higher = more creative) |
| `model.maxTokens` | number | `4096` | Maximum tokens to generate (min: 1) |

**Configuration Methods:**

```yaml
# Config file
model:
  default: llama3.2:3b
  temperature: 0.7
  maxTokens: 4096
```

```bash
# Environment variables
export OLLM_DEFAULT_MODEL=llama3.1:8b

# CLI flags
ollm --model llama3.1:8b
```


### Context Settings

Controls context management and VRAM-aware sizing (optional, uses defaults if not specified).

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `context.targetSize` | number | `8192` | Target context size in tokens |
| `context.minSize` | number | `2048` | Minimum context size in tokens |
| `context.maxSize` | number | `32768` | Maximum context size in tokens |
| `context.autoSize` | boolean | `true` | Enable automatic context sizing based on VRAM |
| `context.vramBuffer` | number | `1073741824` | VRAM buffer to reserve in bytes (1GB default) |
| `context.compressionEnabled` | boolean | `true` | Enable automatic context compression |
| `context.compressionThreshold` | number | `0.8` | Threshold (0-1) to trigger compression |
| `context.snapshotsEnabled` | boolean | `true` | Enable snapshots for context restoration |
| `context.maxSnapshots` | number | `5` | Maximum number of snapshots to keep |

**Configuration Methods:**

```yaml
# Config file
context:
  targetSize: 8192
  autoSize: true
  compressionEnabled: true
  compressionThreshold: 0.8
```

```bash
# Environment variables
# (Context settings are primarily configured via config file)

# CLI flags
# (Context settings are not available as CLI flags)
```


### UI Settings

Controls the terminal user interface appearance and behavior.

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `ui.layout` | string | `"hybrid"` | UI layout mode (`hybrid` or `simple`) |
| `ui.sidePanel` | boolean | `true` | Show side panel with context info |
| `ui.showGpuStats` | boolean | `true` | Display GPU statistics in status bar |
| `ui.showCost` | boolean | `true` | Display token cost estimates |
| `ui.metrics.enabled` | boolean | `true` | Enable performance metrics display |
| `ui.metrics.compactMode` | boolean | `false` | Use compact metrics display |
| `ui.metrics.showPromptTokens` | boolean | `true` | Show prompt token count |
| `ui.metrics.showTTFT` | boolean | `true` | Show time to first token |
| `ui.metrics.showInStatusBar` | boolean | `true` | Display metrics in status bar |
| `ui.reasoning.enabled` | boolean | `true` | Enable reasoning display for models that support it |
| `ui.reasoning.maxVisibleLines` | number | `8` | Maximum lines to show for reasoning (min: 1) |
| `ui.reasoning.autoCollapseOnComplete` | boolean | `true` | Auto-collapse reasoning when complete |

**Configuration Methods:**

```yaml
# Config file
ui:
  layout: hybrid
  sidePanel: true
  showGpuStats: true
  showCost: true
  metrics:
    enabled: true
    compactMode: false
    showPromptTokens: true
    showTTFT: true
    showInStatusBar: true
  reasoning:
    enabled: true
    maxVisibleLines: 8
    autoCollapseOnComplete: true
```

```bash
# Environment variables
export NO_COLOR=1  # Disable colored output

# CLI flags
ollm --no-color  # Disable colored output
```


### Status Settings

Controls GPU and system status monitoring.

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `status.pollInterval` | number | `5000` | Status polling interval in milliseconds (min: 1000) |
| `status.highTempThreshold` | number | `80` | Temperature threshold for warnings in °C (min: 0) |
| `status.lowVramThreshold` | number | `512` | Low VRAM warning threshold in MB (min: 0) |

**Configuration Methods:**

```yaml
# Config file
status:
  pollInterval: 5000
  highTempThreshold: 80
  lowVramThreshold: 512
```

```bash
# Environment variables
# (Status settings are configured via config file)

# CLI flags
# (Status settings are not available as CLI flags)
```


### Review Settings

Controls diff review behavior for file modifications.

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `review.enabled` | boolean | `true` | Enable diff review for file changes |
| `review.inlineThreshold` | number | `5` | Max lines to show inline (larger diffs open in pager, min: 0) |

**Configuration Methods:**

```yaml
# Config file
review:
  enabled: true
  inlineThreshold: 5
```

```bash
# Environment variables
# (Review settings are configured via config file)

# CLI flags
ollm --review-diffs    # Enable diff review
ollm --no-review       # Disable diff review
```


### Session Settings

Controls session recording and auto-save behavior.

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `session.autoSave` | boolean | `true` | Automatically save session state |
| `session.saveInterval` | number | `60000` | Auto-save interval in milliseconds (min: 1000) |

**Configuration Methods:**

```yaml
# Config file
session:
  autoSave: true
  saveInterval: 60000
```

```bash
# Environment variables
# (Session settings are configured via config file)

# CLI flags
ollm --session <id>  # Resume a specific session
```


## Environment Variables

OLLM CLI supports the following environment variables for configuration:

### Provider Configuration

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `OLLAMA_HOST` | string (URI) | `http://localhost:11434` | Ollama server endpoint URL |
| `VLLM_HOST` | string (URI) | - | vLLM server endpoint URL |
| `VLLM_API_KEY` | string | - | API key for vLLM authentication |
| `OPENAI_COMPATIBLE_HOST` | string (URI) | - | OpenAI-compatible server endpoint URL |
| `OPENAI_COMPATIBLE_API_KEY` | string | - | API key for OpenAI-compatible authentication |

### Model Configuration

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `OLLM_DEFAULT_MODEL` | string | `llama3.2:3b` | Default model name to use |

### Logging and Debugging

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `OLLM_LOG_LEVEL` | string | `info` | Logging verbosity (`debug`, `info`, `warn`, `error`) |

### System Paths

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `OLLM_CONFIG_PATH` | string | - | Custom config file path (overrides default locations) |
| `OLLM_INDEX_PATH` | string | - | Custom codebase index location |
| `XDG_CONFIG_HOME` | string | `~/.config` | Linux config directory (affects `~/.ollm` location) |
| `XDG_DATA_HOME` | string | `~/.local/share` | Linux data directory |
| `XDG_CACHE_HOME` | string | `~/.cache` | Linux cache directory |

### Feature Flags

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `OLLM_DISABLE_INDEXING` | boolean | `false` | Disable automatic codebase indexing |
| `OLLM_DISABLE_EXECUTION` | boolean | `false` | Disable code execution sandbox |
| `NO_COLOR` | boolean | `false` | Disable colored output (standard convention) |

### GPU Monitoring

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `NVIDIA_SMI_PATH` | string | - | Custom path to nvidia-smi executable |

### Usage Examples

```bash
# Set Ollama host
export OLLAMA_HOST=http://192.168.1.100:11434

# Enable debug logging
export OLLM_LOG_LEVEL=debug

# Use custom config file
export OLLM_CONFIG_PATH=/path/to/custom/config.yaml

# Disable colored output
export NO_COLOR=1

# Run with environment variables
OLLAMA_HOST=http://localhost:11434 OLLM_LOG_LEVEL=debug ollm
```


## Configuration Examples

### Minimal Configuration

This is the simplest configuration that just changes the default model:

```yaml
# ~/.ollm/config.yaml or .ollm/config.yaml
model:
  default: llama3.1:8b
```

### Basic Configuration

A typical configuration for everyday use:

```yaml
# ~/.ollm/config.yaml
provider:
  default: ollama
  ollama:
    host: http://localhost:11434
    timeout: 30000

model:
  default: llama3.2:3b
  temperature: 0.7
  maxTokens: 4096

ui:
  layout: hybrid
  sidePanel: true
  showGpuStats: true

session:
  autoSave: true
  saveInterval: 60000
```


### Advanced Configuration

A comprehensive configuration with all major settings:

```yaml
# ~/.ollm/config.yaml - Advanced configuration example

# Provider configuration
provider:
  default: ollama
  ollama:
    host: http://localhost:11434
    timeout: 30000
  vllm:
    host: http://localhost:8000
    apiKey: your-api-key-here
  openaiCompatible:
    host: http://localhost:1234
    apiKey: your-api-key-here

# Model configuration
model:
  default: llama3.2:3b
  temperature: 0.7
  maxTokens: 4096

# Context management (optional - uses smart defaults if omitted)
context:
  targetSize: 8192
  minSize: 2048
  maxSize: 32768
  autoSize: true
  vramBuffer: 1073741824  # 1GB in bytes
  compressionEnabled: true
  compressionThreshold: 0.8
  snapshotsEnabled: true
  maxSnapshots: 5

# UI configuration
ui:
  layout: hybrid
  sidePanel: true
  showGpuStats: true
  showCost: true
  metrics:
    enabled: true
    compactMode: false
    showPromptTokens: true
    showTTFT: true
    showInStatusBar: true
  reasoning:
    enabled: true
    maxVisibleLines: 8
    autoCollapseOnComplete: true

# Status monitoring
status:
  pollInterval: 5000
  highTempThreshold: 80
  lowVramThreshold: 512

# Diff review
review:
  enabled: true
  inlineThreshold: 5

# Session management
session:
  autoSave: true
  saveInterval: 60000
```


### Project-Specific Configuration

Example workspace configuration for a coding project:

```yaml
# .ollm/config.yaml - Project-specific configuration

# Use a code-specialized model for this project
model:
  default: codellama:13b
  temperature: 0.3  # Lower temperature for more deterministic code generation
  maxTokens: 8192

# Enable diff review for code changes
review:
  enabled: true
  inlineThreshold: 10  # Show larger diffs inline for code review

# Optimize UI for coding
ui:
  layout: simple
  sidePanel: false
  showGpuStats: false
  metrics:
    enabled: true
    compactMode: true
```

### Remote Server Configuration

Example configuration for connecting to a remote Ollama server:

```yaml
# ~/.ollm/config.yaml - Remote server configuration

provider:
  default: ollama
  ollama:
    host: http://192.168.1.100:11434
    timeout: 60000  # Longer timeout for remote connections

model:
  default: llama3.1:70b  # Use larger model on powerful remote server
  temperature: 0.7
  maxTokens: 8192
```

### Multi-Provider Configuration

Example configuration with multiple providers configured:

```yaml
# ~/.ollm/config.yaml - Multi-provider setup

provider:
  default: ollama  # Default to local Ollama
  ollama:
    host: http://localhost:11434
    timeout: 30000
  vllm:
    host: http://gpu-server:8000
    apiKey: vllm-secret-key
  openaiCompatible:
    host: http://localhost:1234
    # No API key needed for local LM Studio

model:
  default: llama3.2:3b
  temperature: 0.7
  maxTokens: 4096

# Switch providers at runtime:
# ollm --provider vllm --model mistral-7b
# ollm --provider openai-compatible --model local-model
```

### Performance-Optimized Configuration

Example configuration optimized for performance on limited hardware:

```yaml
# ~/.ollm/config.yaml - Performance-optimized

model:
  default: llama3.2:3b  # Smaller model for faster inference
  temperature: 0.7
  maxTokens: 2048  # Limit output length

context:
  targetSize: 4096  # Smaller context window
  autoSize: true
  compressionEnabled: true
  compressionThreshold: 0.7  # Compress earlier

ui:
  layout: simple
  sidePanel: false
  showGpuStats: true
  metrics:
    enabled: true
    compactMode: true

status:
  pollInterval: 10000  # Poll less frequently
```

## Validation

OLLM CLI validates your configuration on startup. If there are errors, you'll see detailed messages indicating:

- Which file contains the error
- The line and column number (for YAML syntax errors)
- What's wrong and how to fix it
- Suggestions for common mistakes

Example validation error:

```
Configuration Error: Expected number, got string
  File: /home/user/.ollm/config.yaml
  Line: 5, Column: 15

  5 | temperature: "high"
                   ^

Tip - Check for missing or unmatched quotes
```

## Tips

1. **Start Simple**: Begin with a minimal configuration and add settings as needed
2. **Use Workspace Configs**: Keep project-specific settings in `.ollm/config.yaml`
3. **Environment Variables**: Use environment variables for temporary overrides
4. **CLI Flags**: Use CLI flags for one-off changes without modifying config files
5. **Validation**: Run `ollm --help` to verify your configuration loads correctly
6. **Comments**: YAML supports comments with `#` - use them to document your settings

---

## Planned Future Configuration Options

The following configuration options are **planned for future development** and are not yet available in the current version. These will be added as new features are implemented according to the roadmap (ROADMAP.md).

### 🔮 Kraken Integration (Planned)

External LLM provider configuration for accessing cloud models:

```yaml
# PLANNED - Not yet available
# kraken:
  # enabled: true
  # confirmBeforeRelease: true
  # autoEscalation:
    # enabled: false
    # triggers:
      # - contextOverflow
      # - localModelError
  # providers:
    # geminiCli:
      # enabled: true
      # executable: gemini
      # timeout: 120000
      # defaultModel: gemini-pro
    # claudeCode:
      # enabled: true
      # executable: claude-code
      # timeout: 120000
    # openai:
      # enabled: true
      # apiKey: ${OPENAI_API_KEY}
      # model: gpt-4
      # maxTokens: 8192
    # anthropic:
      # enabled: true
      # apiKey: ${ANTHROPIC_API_KEY}
      # model: claude-3-opus
  # costTracking:
    # enabled: true
    # sessionBudget: 5.00  # USD
    # warnThreshold: 1.00
```

**Status:** Planned for Stage 10  
**Documentation:** Kraken Integration Spec (.kiro/specs/stage-10-kraken-integration-future-dev/)

### 🔮 Developer Productivity Tools (Planned)

Git integration and diff review configuration:

```yaml
# PLANNED - Not yet available
# git:
  # enabled: true
  # includeInSystemPrompt: true
  # autoCommit:
    # enabled: false
    # messageStyle: semantic  # semantic, descriptive, conventional
    # groupChanges: true

# mentions:
  # enabled: true
  # maxFilesPerGlob: 50
  # maxTokensPerMention: 4096
  # warnOnLargeContext: true

# diffReview:
  # enabled: true
  # autoApprove:
    # readOperations: true
    # smallChanges: true
    # smallChangeThreshold: 10  # lines
  # showFullContext: true
  # contextLines: 3
```

**Status:** Planned for Stage 11  
**Documentation:** Developer Productivity Spec (.kiro/specs/stage-11-developer-productivity-future-dev/)

### 🔮 Cross-Platform Support (Planned)

Platform-specific configuration options:

```yaml
# PLANNED - Not yet available
# platform:
  # shell:
    # windows: cmd.exe
    # unix: /bin/sh
  # python:
    # windows: python
    # unix: python3
  # paths:
    # normalizeDisplay: true  # Show forward slashes in UI
  # terminal:
    # forceUnicode: false
    # forceColor: false
```

**Status:** Planned for Stage 12  
**Documentation:** Cross-Platform Spec (.kiro/specs/stage-12-cross-platform-future-dev/)

### 🔮 File Upload System (Planned)

File upload and storage configuration:

```yaml
# PLANNED - Not yet available
# uploads:
  # enabled: true
  # methods:
    # slashCommand: true
    # clipboard: true
    # dragDrop: true
    # mentions: true
  # storage:
    # perFileLimit: 10485760  # 10MB in bytes
    # perSessionLimit: 104857600  # 100MB in bytes
    # retentionDays: 7
  # images:
    # maxWidth: 2048
    # maxHeight: 2048
    # quality: 85
  # allowedTypes:
    # - image/*
    # - text/*
    # - application/json
```

**Status:** Planned for Stage 14  
**Documentation:** File Upload Spec (.kiro/specs/stage-14-file-upload-future-dev/)

### 🔮 Intelligence Layer (Planned)

Advanced AI capabilities configuration:

```yaml
# PLANNED - Not yet available
# intelligence:
  # codebaseIndex:
    # enabled: true
    # autoIndex: true
    # extensions:
      # - .ts
      # - .js
      # - .py
      # - .java
      # - .go
    # excludePatterns:
      # - node_modules/**
      # - dist/**
      # - .git/**
    # maxFileSize: 1048576  # 1MB
  
  # structuredOutput:
    # enabled: true
    # maxRetries: 3
    # useGuidedDecoding: true
  
  # sandbox:
    # enabled: true
    # timeout: 30000
    # allowedLanguages:
      # - javascript
      # - python
      # - bash
    # restrictions:
      # networkAccess: false
      # filesystemAccess: false
    # memoryLimit: 536870912  # 512MB
  
  # vision:
    # enabled: true
    # defaultModel: llava:13b
    # maxImageSize: 5242880  # 5MB
  
  # costTracking:
    # enabled: true
    # monthlyBudget: 50.00  # USD
    # warnThreshold: 40.00
```

**Status:** Planned for Stage 15  
**Documentation:** Intelligence Layer Spec (.kiro/specs/stage-15-intelligence-layer-future-dev/)

---

### Notes on Planned Features

- **Not Yet Available**: These configuration options are documented for planning purposes and will be implemented in future releases
- **Subject to Change**: Exact configuration structure may change during implementation based on technical requirements and user feedback
- **Roadmap**: See the full roadmap (ROADMAP.md) for timeline and priority information
- **Contributions Welcome**: If you're interested in implementing any of these features, please review the detailed specifications and open a discussion

---

**Last Updated:** January 15, 2026
