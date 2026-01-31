# OLLM CLI Release Notes

**Version History and Changelog**

This document tracks all OLLM CLI releases with detailed notes on features, changes, fixes, and breaking changes.

---

## v0.1.6 - Foundation Release

**Released:** January 30, 2026  
**Status:** ✅ Released

### Overview

The foundation release of OLLM CLI establishes the core architecture and essential features for a local-first, provider-agnostic LLM interface.

### ✨ Features Added

#### 🎨 Interactive Terminal UI

- React + Ink powered interface with streaming responses
- Syntax highlighting for code blocks with language detection
- Status bar showing model, context usage, and VRAM metrics
- Tool execution preview with diff visualization
- Real-time updates and responsive layout

#### 🧠 Smart Context Management

- Adaptive context sizing based on available VRAM
- Automatic compression when approaching context limits
- Snapshot and rollover support for long conversations
- Real-time monitoring of token usage and memory consumption
- Memory safety guards to prevent OOM errors

#### 🛠️ Powerful Tool System

- Built-in tools: File operations, shell execution, web fetch, search, memory
- Policy-based confirmation: ASK, AUTO, and YOLO approval modes
- Diff preview for file edits before applying changes
- Output truncation and streaming for long-running operations
- Tool registry with schema validation

#### 🔌 Extensibility (Pre-Alpha)

- Hook system for event-driven automation and safety gates
- Extension system with manifest-based configuration
- MCP integration (Model Context Protocol) for external tools
- Provider-agnostic architecture supporting multiple LLM backends

#### 💾 Session Management

- Record and resume conversations with full context
- Automatic compression to manage context limits
- Loop detection to prevent runaway tool calls
- Session history with searchable archives

#### 🌐 Offline First

- Works without internet when models are installed locally
- No telemetry - all data stays on your machine
- Local model management - pull, list, and remove models

### 📦 Technical Details

- **Architecture:** Provider-agnostic with adapter pattern
- **UI Framework:** React + Ink for terminal rendering
- **Testing:** Comprehensive test suite with property-based testing
- **Platform:** Node.js 20+, TypeScript with strict mode

---

## v0.2.0 - File Explorer & Enhanced MCP

**Status:** 📋 Planned  
**Target:** Q2 2026

### Planned Features

#### 📁 File Explorer

- Tree-based file browser with keyboard navigation
- File operations (create, delete, rename, move)
- Multi-file selection and batch operations
- Search and filter capabilities
- Git status integration
- File preview pane

#### 🔌 Enhanced MCP

- MCP Marketplace integration
- Server health monitoring
- OAuth configuration support
- Tool auto-approval management
- Server logs viewer
- Performance optimizations

### Configuration Example

```yaml
fileExplorer:
  enabled: true
  showHidden: false
  gitIntegration: true
  previewPane: true

mcp:
  marketplace:
    enabled: true
    autoUpdate: false
  healthCheck:
    interval: 60
```

---

## v0.3.0 - Advanced File Explorer & MCP Polish

**Status:** 📋 Planned  
**Target:** Q2 2026

### Planned Features

#### 🔍 Advanced File Explorer

- Multi-file selection with checkboxes
- Drag-and-drop file operations
- Advanced search with regex support
- File type filtering
- Custom file actions
- Workspace management

#### ⚡ MCP Polish

- Performance optimizations
- Enhanced error handling
- Improved server lifecycle management
- Better tool schema validation
- Connection pooling
- Caching layer

---

## v0.4.0 - Code Editor

**Status:** 📋 Planned  
**Target:** Q3 2026

### Planned Features

#### ✏️ Terminal Code Editor

- Built-in terminal-based code editor
- Syntax highlighting for multiple languages
- Line numbers and cursor positioning
- Search and replace functionality
- Multiple file tabs
- Integration with LLM for code suggestions
- Format on save
- Undo/redo support

### Configuration Example

```yaml
editor:
  enabled: true
  theme: monokai
  tabSize: 2
  formatOnSave: true
  tabs:
    maxOpen: 10
```

---

## v0.5.0 - Release Kraken

**Status:** 📋 Planned  
**Target:** Q3 2026

### Planned Features

#### 🦑 External LLM Provider Access

Access powerful cloud models when local models need assistance.

**CLI-Based Providers:**

- Gemini CLI integration
- Claude Code integration
- Codex CLI integration
- Subprocess execution bridge

**API-Based Providers:**

- OpenAI API integration
- Anthropic API integration
- Google AI API integration
- Streaming response handling

**Management:**

- Provider discovery and health checks
- Intelligent provider selection
- Context transfer between providers
- Cost tracking and budget enforcement
- Auto-escalation on local model failure
- Confirmation dialogs for external requests

### Configuration Example

```yaml
kraken:
  enabled: true
  confirmBeforeRelease: true
  autoEscalation:
    enabled: false
    triggers:
      - contextOverflow
      - localModelError
  sessionBudget: 10.00 # USD
  providers:
    geminiCli:
      enabled: true
      executable: gemini
      defaultModel: gemini-2.0-flash-exp
    claudeCode:
      enabled: true
      executable: claude-code
    openai:
      enabled: true
      apiKey: ${OPENAI_API_KEY}
      model: gpt-4
      maxTokens: 8192
```

---

## v0.6.0 - RAG Integration

**Status:** 📋 Planned  
**Target:** Q3 2026

### Planned Features

#### 🔍 Semantic Search & Codebase Understanding

Find relevant code with semantic search.

**Key Features:**

- Codebase indexing with embeddings
- Semantic code search
- Context-aware file discovery
- Symbol and definition search
- Documentation search
- Vector database integration (LanceDB)
- Incremental indexing
- Search result ranking

### Configuration Example

```yaml
codebaseIndex:
  enabled: true
  autoIndex: true
  extensions: ['.ts', '.js', '.py', '.java', '.go']
  excludePatterns: ['node_modules', 'dist', '.git']
  maxFileSize: 1048576 # 1MB
  vectorDatabase: 'lancedb'
```

---

## v0.7.0 - GitHub Integration

**Status:** 📋 Planned  
**Target:** Q3 2026

### Planned Features

#### 🐙 GitHub Workflow Integration

**Key Features:**

- GitHub API integration
- Issue and PR management
- Code review workflows
- Commit and push operations
- Branch management
- GitHub Actions integration

### Configuration Example

```yaml
github:
  enabled: true
  token: ${GITHUB_TOKEN}
  defaultRepo: owner/repo
  autoLink: true # Auto-link issues/PRs in chat
  features:
    issues: true
    pullRequests: true
    codeReview: true
```

---

## v0.8.0 - Cross-Platform Support

**Status:** 📋 Planned  
**Target:** Q4 2026

### Planned Features

#### 🖥️ Enhanced Windows, macOS, Linux Compatibility

**Key Features:**

- Platform detection and defaults
- Cross-platform GPU monitoring (NVIDIA, AMD, Apple Silicon)
- Terminal capability detection
- Path normalization
- Platform-specific optimizations

### Configuration Example

```yaml
platform:
  autoDetect: true
  gpu:
    monitoring: true
    vendor: auto # nvidia, amd, apple, or auto
  terminal:
    colorSupport: auto
    unicodeSupport: auto
```

---

## v0.9.0 - vLLM & Open Source Providers

**Status:** 📋 Planned  
**Target:** Q4 2026

### Planned Features

#### 🔌 High-Performance Inference Engines

**Key Features:**

- vLLM provider integration
- OpenAI-compatible API support
- High-throughput inference
- Batching and caching
- Multi-GPU support
- Custom model loading

### Configuration Example

```yaml
providers:
  vllm:
    enabled: true
    host: http://localhost:8000
    apiKey: ${VLLM_API_KEY}
    timeout: 30
  openaiCompatible:
    enabled: true
    host: http://localhost:1234
    apiKey: ${OPENAI_COMPATIBLE_API_KEY}
```

---

## v1.0.0 - Beta Release

**Status:** 🔮 Future  
**Target:** Q4 2026

### Planned Features

- Production-ready stability
- Performance optimizations
- Enterprise features
- Advanced security features
- Comprehensive documentation
- Migration tools

---

## Version Comparison

| Version | Release Date | Status      | Key Features                               |
| ------- | ------------ | ----------- | ------------------------------------------ |
| v0.1.6  | Jan 2026     | ✅ Released | Foundation, TUI, Tools, Context Management |
| v0.2.0  | Q2 2026      | 📋 Planned  | File Explorer, Enhanced MCP                |
| v0.3.0  | Q2 2026      | 📋 Planned  | Advanced Explorer, MCP Polish              |
| v0.4.0  | Q3 2026      | 📋 Planned  | Code Editor                                |
| v0.5.0  | Q3 2026      | 📋 Planned  | Release Kraken (External Providers)        |
| v0.6.0  | Q3 2026      | 📋 Planned  | RAG Integration                            |
| v0.7.0  | Q3 2026      | 📋 Planned  | GitHub Integration                         |
| v0.8.0  | Q4 2026      | 📋 Planned  | Cross-Platform Support                     |
| v0.9.0  | Q4 2026      | 📋 Planned  | vLLM & Open Source Providers               |
| v1.0.0  | Q4 2026      | 🔮 Future   | Beta Release                               |

---

## Related Documentation

- [Roadmap](Roadmap.md) - Detailed development roadmap
- [Roadmap Visual](RoadmapVisual.md) - Visual timeline and diagrams
- [Planned Features](PlanedFeatures.md) - Feature reference guide
- [README](README.md) - Roadmap overview

---

**Last Updated:** January 28, 2026  
**Current Version:** v0.1.6
