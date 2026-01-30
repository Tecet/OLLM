# Model Management Documentation

**Last Updated:** January 26, 2026

Welcome to the Model Management documentation for OLLM CLI. This guide covers model lifecycle management, provider integration, and model capabilities.

---

## 📚 Documentation Overview

### Core Documentation

- **[Model Index](LLM_Index.md)** - Complete documentation index
- **[Models List](LLM_ModelsList.md)** - Ollama models reference with VRAM requirements
- **[Model Compatibility](LLM_ModelCompatibility.md)** - Tested models and compatibility matrix
- **[Memory System](LLM_MemorySystem.md)** - Cross-session memory guide

---

## 🎯 What is Model Management?

The **Model Management** system provides comprehensive control over LLM models and their usage in OLLM CLI:

### 1. **Model Discovery & Metadata**

Track installed models and their capabilities:

- List available models from provider
- Enrich with metadata from shipped profiles
- Detect tool calling support
- Track context window sizes
- Monitor model parameters and VRAM requirements

### 2. **Provider Integration**

Unified interface for LLM backends:

- **Current:** Ollama provider (local execution)
- **Planned:** Codex, Claude, Gemini (v0.6.0)
- **Planned:** vLLM and open source providers (v0.9.0)
- Automatic provider health checks
- Model management operations (pull, remove, list)

### 3. **Context Window Configuration**

Flexible context sizing:

- Auto-detection from model metadata
- Manual context size configuration
- Context profiles (4K, 8K, 16K, 32K, etc.)
- VRAM-aware context sizing
- 85% utilization for optimal performance

### 4. **Tool Support Detection**

Intelligent tool calling capability detection:

- Auto-detection via test requests
- User confirmation prompts
- Runtime error learning
- Precedence system (user > auto > runtime > profile)

### 5. **Reasoning Model Support**

Special handling for reasoning models:

- Extended warmup timeouts (120s vs 30s)
- Reasoning content capture and display
- Collapsible reasoning blocks
- Separate reasoning token tracking

---

## 📖 Documentation Structure

```
docs/LLM Models/
├── README.md                    ← You are here
├── LLM_Index.md                 Complete documentation index
├── LLM_ModelsList.md            Ollama models reference
├── LLM_ModelCompatibility.md    Compatibility matrix
└── LLM_MemorySystem.md          Memory system guide
```

---

## 🎓 Key Concepts

### Model Discovery

Tracks installed models from provider and enriches with metadata.

**Process:**

1. Query provider for installed models (e.g., Ollama `/api/tags`)
2. Match with shipped profiles (`LLM_profiles.json`)
3. Enrich with metadata (context windows, tool support, etc.)
4. Save to `user_models.json` with user overrides
5. Display in model selection menu

**See:** [Model Index](LLM_Index.md)

### Provider System

Abstracts LLM backend communication through unified interface.

**Current Provider:**

- **Ollama** - Local execution, full privacy, no API costs
- Auto-start capability
- Model management (pull, remove, list)
- VRAM monitoring integration

**Planned Providers:**

- **v0.6.0:** Codex, Claude, Gemini
- **v0.9.0:** vLLM and open source providers

**See:** Knowledge DB `dev_ProviderSystem.md`

### Context Window Management

Flexible context sizing based on model capabilities and hardware.

**Features:**

- Auto-detection from model metadata
- Context profiles (4K, 8K, 16K, 32K, 64K, 128K)
- 85% utilization (pre-calculated in profiles)
- VRAM-aware sizing
- Manual override support

**See:** [Models List](LLM_ModelsList.md)

### Tool Support Detection

Intelligent detection of tool calling capabilities.

**Detection Methods:**

1. **User Confirmed** - User explicitly confirms (highest priority)
2. **Auto-Detected** - Automatic test request
3. **Runtime Error** - Learned from actual errors
4. **Profile** - Default from shipped profiles (lowest priority)

**See:** Knowledge DB `dev_ModelManagement.md`

### Reasoning Models

Special handling for models that expose thinking process.

**Features:**

- Extended warmup timeouts (120s vs 30s)
- Reasoning content capture (`thinking` field)
- Collapsible reasoning blocks in UI
- Auto-expand during streaming, auto-collapse when complete

**Examples:** DeepSeek R1, QwQ

**See:** Knowledge DB `dev_ReasoningModels.md`

---

## 💡 Common Use Cases

### Manage Models

```bash
# List available models
/model list

# Download a model
/model pull llama3.1:8b

# View model details
/model info llama3.1:8b

# Keep model loaded
/model keep llama3.1:8b

# Delete unused model
/model delete old-model:7b
```

**Learn more:** [Model Commands](../UserInterface/Commands.md#model-management)

### Configure Context Size

```bash
# Check current context
/context stats

# Set context size
/context size 16384

# Enable auto-sizing
/context auto
```

**Learn more:** [Context Management](../Context/ContextManagment.md)

### Use Memory System

```bash
# Add a memory
/memory add user_name Alice

# List memories
/memory list

# Search memories
/memory search project

# Forget a memory
/memory forget old_preference
```

**Learn more:** [Memory System](LLM_MemorySystem.md)

---

## 🛠️ Configuration

### Provider Settings

```yaml
# Ollama provider (current)
provider:
  ollama:
    autoStart: true # Auto-start ollama serve
    host: localhost
    port: 11434
    url: http://localhost:11434
```

### Model Settings

```yaml
model:
  default: llama3.2:3b
  temperature: 0.7
  maxTokens: 4096
```

### Context Settings

```yaml
context:
  targetSize: 8192
  autoSize: true
  compressionEnabled: true
```

### Memory Settings

```yaml
memory:
  enabled: true
  tokenBudget: 500
```

**Learn more:** [Configuration](../UserInterface/configuration.md)

---

## 🔍 Troubleshooting

### Common Issues

**Ollama not running:**

- **If auto-start enabled:** Check app logs for startup errors
- **If auto-start disabled:** Run `ollama serve` manually
- **Custom server:** Configure with `/config ollama host <hostname>`

**Model not found:**

- Check model name: `/model list`
- Pull the model: `/model pull <name>`
- Verify provider is running

**Tool calling not working:**

- Check tool support: Model info shows tool_support field
- Auto-detect: System prompts for confirmation
- Manual override: Confirm in user_models.json

**Context window issues:**

- Check model's native context window
- Set appropriate `num_ctx` value
- Use model with larger context window
- Enable context compression

**Memory not persisting:**

- Check memory enabled in config
- Verify file permissions
- Check storage location: `~/.ollm/memory.json`

**See:** [Troubleshooting Guide](../Troubleshooting.md)

---

## 📊 Implementation Status

### Current (v0.1.0)

- ✅ Model Discovery & Metadata
- ✅ Ollama Provider Integration
- ✅ Context Window Configuration
- ✅ Tool Support Detection
- ✅ Reasoning Model Support (basic)
- ✅ Memory System
- ✅ Model Commands

### Planned (v0.6.0)

- ⏳ Codex Provider
- ⏳ Claude Provider
- ⏳ Gemini Provider
- ⏳ Model Routing System
- ⏳ Prompt Templates
- ⏳ Project Profiles

### Planned (v0.9.0)

- ⏳ vLLM Provider
- ⏳ Open Source Providers
- ⏳ Advanced Routing
- ⏳ Reasoning Analytics

---

## 🤝 Related Documentation

### Core Systems

- [Context Management](../Context/ContextManagment.md) - Context sizing and VRAM
- [Context Compression](../Context/ContextCompression.md) - Compression system
- [MCP Integration](../MCP/MCP_Index.md) - Model Context Protocol
- [User Interface](../UserInterface/README.md) - UI documentation

### Developer Resources

- Knowledge DB: `dev_ModelManagement.md` - Model management architecture
- Knowledge DB: `dev_ProviderSystem.md` - Provider system design
- Knowledge DB: `dev_ReasoningModels.md` - Reasoning model support

---

**Last Updated:** January 26, 2026  
**Version:** 0.1.0  
**Status:** Active Development
