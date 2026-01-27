# Context Management Documentation

**Last Updated:** January 26, 2026

Welcome to the Context Management documentation for OLLM CLI. This section covers the complete context management system, including context sizing, compression, checkpoints, and VRAM monitoring.

---

## 📚 Documentation Overview

### Core Documentation
- **[Context Architecture](ContextArchitecture.md)** - Complete system architecture and design
- **[Context Management](ContextManagment.md)** - Context sizing, tiers, and VRAM monitoring
- **[Context Compression](ContextCompression.md)** - Compression strategies and implementation
- **[Checkpoint Flow](CheckpointFlowDiagram.md)** - Checkpoint system and rollover flow

---

## 🎯 What is Context Management?

The **Context Management** system handles the conversation history and system prompts sent to the LLM, ensuring optimal performance within hardware constraints:

### 1. **Fixed Context Sizing**
Context size is determined once at startup based on available VRAM:
- VRAM detection (NVIDIA, AMD, Apple Silicon)
- Context tier selection (Minimal, Basic, Standard, Premium, Ultra)
- 85% utilization for optimal performance (pre-calculated in LLM_profiles.json)
- Context remains FIXED for the entire session

### 2. **Context Compression**
Automatic compression when context approaches limits:
- LLM-based summarization (LLM does the work, not the app)
- Multiple compression strategies (aggressive, balanced, conservative)
- Preserves recent messages and critical information
- Maintains conversation continuity

### 3. **Checkpoint System**
Save and restore conversation state:
- Manual checkpoints for important conversation points
- Automatic checkpoints before major operations
- Rollover to new session when context full
- Checkpoint restoration for session recovery

### 4. **VRAM Monitoring**
Real-time GPU memory tracking:
- Cross-platform support (NVIDIA, AMD, Apple Silicon)
- Automatic VRAM detection at startup
- Context tier selection based on available memory
- Memory safety guards to prevent OOM errors

---

## 📖 Documentation Structure

```
docs/Context/
├── README.md                    ← You are here
├── Index.md                     Quick reference with links
├── ContextArchitecture.md       System architecture
├── ContextManagment.md          Context sizing and VRAM
├── ContextCompression.md        Compression system
└── CheckpointFlowDiagram.md     Checkpoint flow
```

---

## 🎓 Key Concepts

### Context Tiers
Five fixed context tiers based on available VRAM:

| Tier | Context Size | VRAM Required | Use Case |
|------|--------------|---------------|----------|
| **Minimal** | 2K-4K | <4GB | Quick tasks, small models |
| **Basic** | 4K-8K | 4-8GB | Standard conversations |
| **Standard** | 8K-16K | 8-12GB | Extended conversations |
| **Premium** | 16K-32K | 12-24GB | Long conversations |
| **Ultra** | 32K+ | 24GB+ | Maximum context |

**See:** [Context Management](ContextManagment.md)

### Context Compression
When context approaches the limit, compression automatically triggers:

**Process:**
1. Detect context approaching limit (e.g., 90% full)
2. Select compression strategy (aggressive, balanced, conservative)
3. LLM summarizes older messages
4. Replace old messages with summary
5. Continue conversation with more space

**See:** [Context Compression](ContextCompression.md)

### Checkpoint System
Save conversation state for recovery or rollover:

**Types:**
- **Manual Checkpoints** - User-created save points
- **Automatic Checkpoints** - Created before major operations
- **Rollover Checkpoints** - Created when starting new session

**See:** [Checkpoint Flow](CheckpointFlowDiagram.md)

### VRAM Monitoring
Automatic GPU memory detection:

**Supported Platforms:**
- **NVIDIA** - nvidia-smi (Windows, Linux)
- **AMD** - rocm-smi (Linux)
- **Apple Silicon** - Metal API (macOS)

**See:** [Context Architecture](ContextArchitecture.md)

---

## 💡 Common Use Cases

### Check Context Usage
```bash
# View current context stats
/context stats

# Shows:
# - Current tokens used
# - Total context size
# - Percentage used
# - Compression status
```

### Manual Compression
```bash
# Trigger compression manually
/compact

# Options:
/compact aggressive    # Maximum compression
/compact balanced      # Default compression
/compact conservative  # Minimal compression
```

### Create Checkpoint
```bash
# Save current conversation state
/snapshot create my-checkpoint

# List checkpoints
/snapshot list

# Restore checkpoint
/snapshot restore my-checkpoint
```

### Start New Session
```bash
# Start fresh session (creates rollover checkpoint)
/session new

# Resume previous session
/session resume <name>
```

**Learn more:** [Context Commands](../UI&Settings/Commands.md#context-management)

---

## 🛠️ Configuration

### Context Settings
```yaml
context:
  # Context tier (auto-detected from VRAM)
  tier: Standard
  
  # Enable automatic compression
  autoCompress: true
  
  # Compression trigger threshold
  compressionThreshold: 0.9  # 90% full
  
  # Compression strategy
  compressionStrategy: balanced  # aggressive, balanced, conservative
```

### VRAM Settings
```yaml
vram:
  # Enable VRAM monitoring
  enabled: true
  
  # VRAM detection method
  # auto, nvidia-smi, rocm-smi, metal
  detectionMethod: auto
  
  # Manual VRAM override (GB)
  manualVRAM: null
```

### Checkpoint Settings
```yaml
checkpoints:
  # Enable automatic checkpoints
  autoCheckpoint: true
  
  # Checkpoint before major operations
  checkpointBeforeOperations: true
  
  # Maximum checkpoints to keep
  maxCheckpoints: 10
```

**Learn more:** [Configuration](../UI&Settings/Configuration.md)

---

## 🔍 Troubleshooting

### Common Issues

**Context fills up too quickly:**
- Enable automatic compression: `/config set autoCompress true`
- Use more aggressive compression: `/compact aggressive`
- Create checkpoint and start new session: `/snapshot create` then `/session new`

**VRAM detection not working:**
- Check GPU drivers installed
- Verify nvidia-smi/rocm-smi in PATH
- Manual override: `/config set vram.manualVRAM 8` (for 8GB)

**Compression not triggering:**
- Check compression enabled: `/config get autoCompress`
- Check threshold: `/config get compressionThreshold`
- Manually trigger: `/compact`

**Checkpoint restore fails:**
- Verify checkpoint exists: `/snapshot list`
- Check checkpoint file permissions
- Try creating new checkpoint: `/snapshot create test`

**See:** [Troubleshooting Guide](../troubleshooting.md)

---

## 📊 Implementation Status

### Current (v0.1.0)
- ✅ VRAM Detection (NVIDIA, AMD, Apple Silicon)
- ✅ Fixed Context Sizing
- ✅ Context Tiers (Minimal, Basic, Standard, Premium, Ultra)
- ✅ LLM-based Compression
- ✅ Checkpoint System
- ✅ Context Usage Display
- ✅ Memory Safety Guards

### Planned (v0.2.0)
- ⏳ Advanced Compression Strategies
- ⏳ Context Analytics
- ⏳ Compression Quality Metrics

### Planned (v0.3.0)
- ⏳ Multi-tier Compression
- ⏳ Semantic Context Pruning
- ⏳ Context Optimization

---

## 🤝 Related Documentation

### Core Systems
- [Model Management](../LLM%20Models/README.md) - Model selection and configuration
- [Prompts System](../Prompts%20System/README.md) - System prompts and templates
- [User Interface](../UI&Settings/README.md) - UI documentation

### Developer Resources
- Knowledge DB: `dev_ContextManagement.md` - Context management architecture
- Knowledge DB: `dev_ContextCompression.md` - Compression implementation

---

## 🎯 Quick Start

### For New Users

1. **Check Your Context**
   ```bash
   /context stats
   ```

2. **Enable Auto-Compression**
   ```bash
   /config set autoCompress true
   ```

3. **Create Your First Checkpoint**
   ```bash
   /snapshot create my-first-checkpoint
   ```

### For Advanced Users

1. **Optimize Compression**
   ```bash
   /config set compressionStrategy aggressive
   /config set compressionThreshold 0.85
   ```

2. **Monitor VRAM**
   ```bash
   /config get vram
   ```

3. **Manage Checkpoints**
   ```bash
   /snapshot list
   /snapshot restore <name>
   /snapshot delete <name>
   ```

---

## 📈 Performance Tips

### Maximize Context Efficiency

1. **Use Appropriate Tier** - Don't force larger context than needed
2. **Enable Auto-Compression** - Let system manage context automatically
3. **Create Checkpoints** - Save important conversation points
4. **Start Fresh Sessions** - Use rollover for very long conversations
5. **Monitor Usage** - Check `/context stats` regularly

### Optimize for Your Hardware

**Low VRAM (<4GB):**
- Use Minimal tier (2K-4K)
- Aggressive compression
- Frequent checkpoints

**Medium VRAM (4-8GB):**
- Use Basic tier (4K-8K)
- Balanced compression
- Regular checkpoints

**High VRAM (8GB+):**
- Use Standard/Premium tier (8K-32K)
- Conservative compression
- Occasional checkpoints

---

**Last Updated:** January 26, 2026  
**Version:** 0.1.0  
**Status:** Active Development
