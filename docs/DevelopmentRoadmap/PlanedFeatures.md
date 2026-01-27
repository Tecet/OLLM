# Planned Features - Quick Reference

**Last Updated:** January 26, 2026  
**Current Version:** v0.1.0 (Alpha)

> **Status:** All features listed below are **planned for future alpha releases** (v0.2.0 - v0.9.0) and are not yet implemented.

This document provides a quick reference for planned features in OLLM CLI alpha releases.

**For detailed information, see:**
- [Development Roadmap](roadmap.md) - Complete roadmap overview
- [Visual Roadmap](road_map_visual.md) - Visual timeline and dependency graphs
- Version Specifications (`.kiro/specs/vX.X.X/`) - Detailed requirements and design documents

---

## v0.2.0: File Explorer & Enhanced MCP 📁

**File Management & MCP Improvements**

Tree-based file browser and enhanced MCP capabilities.

**Key Features:**
- Tree-based file browser in side panel
- File operations (open, edit, rename, delete)
- Git status indicators
- Quick file search
- MCP OAuth authentication
- MCP health monitoring
- MCP marketplace integration

**Use When:** Managing project files, browsing code, MCP server management

**Specs:** 
- `.kiro/specs/v0.2.0 File Explorer/`
- `.kiro/specs/v0.2.0 MCP Integration/`

---

## v0.3.0: Advanced File Explorer & MCP Polish 🔍

**Enhanced File Management**

Advanced file operations and MCP stability improvements.

**Key Features:**
- Multi-file selection
- File preview panel
- Advanced search and filtering
- Bookmarks and favorites
- MCP error handling improvements
- MCP performance optimizations

**Use When:** Complex file operations, advanced search, stable MCP usage

**Specs:**
- `.kiro/specs/v0.3.0 File Explorer/`
- `.kiro/specs/v0.3.0 MCP Integration/`

---

## v0.4.0: Code Editor ✏️

**Built-in Terminal Code Editor**

Edit files without leaving OLLM CLI.

**Key Features:**
- Terminal-based code editor (3rd window)
- Cursor navigation and text editing
- Undo/redo support
- Copy/cut/paste operations
- Find and go-to-line
- Syntax highlighting (50+ languages)
- Prettier formatting integration
- Multiple file tabs
- Auto-save functionality

**Use When:** Quick edits, staying in OLLM CLI, syntax-aware editing

**Spec:** `.kiro/specs/v0.4.0 Code Editor/`

---

## v0.5.0: RAG Integration 🔍

**Semantic Search & Codebase Understanding**

Find relevant code with semantic search.

**Key Features:**
- Codebase indexing with embeddings
- Semantic code search
- Context-aware file discovery
- Symbol and definition search
- Documentation search
- Vector database integration
- Incremental indexing
- Search result ranking

**Use When:** Large codebases, finding relevant code, better LLM context

**Spec:** `.kiro/specs/v0.5.0 RAG-integration/`

---

## v0.6.0: Release Kraken 🦑

**External LLM Provider Access**

Access powerful cloud models when local models need assistance.

**Key Features:**

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

**Use When:** Complex reasoning, large context, specialized domains, local model limitations

**Spec:** `.kiro/specs/v0.6.0 Release Kraken/`

---

## v0.7.0: GitHub Integration 🐙

**GitHub Workflow Integration**

Manage GitHub from the CLI.

**Key Features:**
- GitHub API integration
- Repository management
- Issue and PR creation
- Code review assistance
- Branch management
- Commit history analysis
- GitHub Actions integration
- Gist support

**Use When:** GitHub workflow automation, PR creation, code review

**Spec:** `.kiro/specs/v0.7.0 GitHub-integration/`

---

## v0.8.0: Cross-Platform Support 🖥️

**Enhanced Windows, macOS, Linux Compatibility**

Consistent behavior across all major operating systems.

**Key Features:**
- Platform detection and defaults
- Configuration path resolution (XDG, AppData)
- Terminal capability detection
- Cross-platform GPU monitoring (NVIDIA, AMD, Apple Silicon)
- Path normalization
- Shell selection (cmd.exe, /bin/sh)
- Python command resolution

**Use When:** Using OLLM CLI on different operating systems, platform-specific features

**Spec:** `.kiro/specs/v0.8.0 Cross Platform/`

---

## v0.9.0: vLLM & Open Source Providers 🔌

**High-Performance Inference Engines**

Extended provider system for production deployments.

**Key Features:**
- vLLM provider integration
- LM Studio support
- Text Generation WebUI support
- LocalAI support
- High-performance streaming
- Batch processing support
- Model serving optimization

**Use When:** Production deployments, faster inference, alternative LLM servers

**Spec:** `.kiro/specs/v0.9.0 vLLM-LMS Providers/`

---

## Feature Comparison Matrix

| Feature | Version | Priority | Dependencies |
|---------|---------|----------|--------------|
| File Explorer | v0.2.0 | High | v0.1.0 |
| Enhanced MCP | v0.2.0 | High | v0.1.0 |
| Advanced Explorer | v0.3.0 | High | v0.2.0 |
| MCP Polish | v0.3.0 | High | v0.2.0 |
| Code Editor | v0.4.0 | High | v0.1.0 |
| RAG Search | v0.5.0 | High | v0.1.0 |
| Kraken (CLI) | v0.6.0 | High | v0.1.0 |
| Kraken (API) | v0.6.0 | High | v0.1.0 |
| GitHub Integration | v0.7.0 | Medium | v0.1.0 |
| Cross-Platform | v0.8.0 | Medium | v0.1.0 |
| vLLM Provider | v0.9.0 | Medium | v0.1.0 |
| LM Studio | v0.9.0 | Medium | v0.1.0 |

---

## Configuration Preview

### v0.2.0: File Explorer

```yaml
fileExplorer:
  enabled: true
  showHidden: false
  gitStatus: true
  followActiveFile: true
  quickSearch: true
```

### v0.2.0: Enhanced MCP

```yaml
mcp:
  oauth:
    enabled: true
    autoRefresh: true
  health:
    enabled: true
    checkInterval: 60000  # 60 seconds
  marketplace:
    enabled: true
    autoUpdate: false
```

### v0.4.0: Code Editor

```yaml
editor:
  enabled: true
  syntaxHighlighting: true
  autoSave: true
  autoSaveDelay: 2000  # 2 seconds
  prettier:
    enabled: true
    formatOnSave: true
  tabs:
    maxOpen: 10
```

### v0.5.0: RAG Integration

```yaml
codebaseIndex:
  enabled: true
  autoIndex: true
  extensions: ['.ts', '.js', '.py', '.java', '.go']
  excludePatterns: ['node_modules', 'dist', '.git']
  maxFileSize: 1048576  # 1MB
  vectorDatabase: 'lancedb'
```

### v0.6.0: Kraken Integration

```yaml
kraken:
  enabled: true
  confirmBeforeRelease: true
  autoEscalation:
    enabled: false
    triggers:
      - contextOverflow
      - localModelError
  sessionBudget: 10.00  # USD
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

### v0.7.0: GitHub Integration

```yaml
github:
  enabled: true
  token: ${GITHUB_TOKEN}
  defaultRepo: owner/repo
  autoLink: true  # Auto-link issues/PRs in chat
  features:
    issues: true
    pullRequests: true
    codeReview: true
    actions: true
```

### v0.8.0: Cross-Platform

```yaml
platform:
  shell:
    windows: cmd.exe
    unix: /bin/sh
  python:
    windows: python
    unix: python3
  paths:
    normalizeDisplay: true  # Show forward slashes in UI
  terminal:
    forceUnicode: false
    forceColor: false
```

### v0.9.0: vLLM Provider

```yaml
provider: vllm  # ollama, vllm, lmstudio, etc.

providers:
  vllm:
    baseUrl: http://localhost:8000
    apiKey: ${VLLM_API_KEY}
    timeout: 60000
  
  lmstudio:
    baseUrl: http://localhost:1234
    timeout: 30000
```

---

## Release Timeline

**Current Status:** v0.1.0 (Alpha) - Released

**Alpha Releases (v0.2.0 - v0.9.0):** Timeline to be determined based on:
- Community feedback and feature requests
- Resource availability
- Technical dependencies
- User workflow priorities

**Estimated Timeline:**
- Q2 2026: v0.2.0 - v0.3.0 (File Explorer & MCP)
- Q3 2026: v0.4.0 - v0.5.0 (Code Editor & RAG)
- Q4 2026: v0.6.0 - v0.7.0 (Kraken & GitHub)
- Q1 2027: v0.8.0 - v0.9.0 (Cross-Platform & vLLM)

**Beta Release (v1.0.0):** Q2 2027 (tentative)

---

## Priority Levels

### High Priority
- **v0.2.0:** File Explorer & Enhanced MCP
- **v0.4.0:** Code Editor
- **v0.5.0:** RAG Integration
- **v0.6.0:** Release Kraken

### Medium Priority
- **v0.3.0:** Advanced File Explorer
- **v0.7.0:** GitHub Integration
- **v0.8.0:** Cross-Platform Support
- **v0.9.0:** vLLM & Open Source Providers

---

## Contributing

Interested in implementing a planned feature?

1. **Review the Specification**
   - Read detailed requirements in `.kiro/specs/vX.X.X Feature/`
   - Understand design and architecture
   - Review task breakdown

2. **Discuss Your Approach**
   - Open an issue with "feature" label
   - Propose implementation plan
   - Get feedback from maintainers

3. **Start Development**
   - Fork the repository
   - Create feature branch
   - Follow coding standards
   - Write tests

4. **Submit Pull Request**
   - Follow PR template
   - Include tests and documentation
   - Respond to review feedback

See [Development Roadmap](roadmap.md#contributing) for detailed contribution guidelines.

---

## Related Documentation

- **[Roadmap Overview](roadmap.md)** - Main roadmap document
- **[Visual Roadmap](road_map_visual.md)** - Timeline and diagrams
- **[Future Development](future-development.md)** - Detailed feature plans
- **Version Specifications** - `.kiro/specs/vX.X.X/` directories

---

**Last Updated:** January 26, 2026  
**Current Version:** v0.1.0 (Alpha)  
**Document Version:** 2.0
