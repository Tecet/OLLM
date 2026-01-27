# OLLM CLI Development Roadmap

**Last Updated:** January 26, 2026  
**Current Version:** v0.1.0 (Alpha)

---

## Overview

OLLM CLI is a local-first command-line interface for open-source LLMs with tools, hooks, and MCP integration. This roadmap outlines the development path from pre-alpha stages through alpha (v0.1.0-v0.9.0) to beta (v1.0.0+).

---

## Development Phases

### Pre-Alpha: Stages 1-9 (Completed)

**Status:** ✅ Complete  
**Timeline:** Development stages 1-9  
**Result:** v0.1.0 Alpha Release

The foundation of OLLM CLI has been successfully implemented through 9 development stages:

**Stage 1-2: Foundation & Core Provider**
- Project structure and build system
- Provider abstraction layer
- Ollama provider integration
- Basic message handling

**Stage 3: Tools & Policy**
- Tool registry and execution
- Policy engine (ASK, AUTO, YOLO modes)
- Built-in tools (file ops, shell, web)
- Tool confirmation system

**Stage 4: Services & Sessions**
- Session recording and management
- Context management foundation
- Dynamic prompt system
- Service container architecture

**Stage 5: Hooks, Extensions & MCP**
- Hook system for automation
- Extension framework
- MCP client integration
- Event-driven architecture

**Stage 6: CLI & UI**
- React + Ink terminal UI
- Interactive TUI components
- Tool support detection
- Hooks panel UI

**Stage 7: Model Management**
- Model discovery and metadata
- Context window configuration
- Tool support detection
- Reasoning model support

**Stage 8: Testing & QA**
- Comprehensive test suite
- Property-based testing
- Integration tests
- CI/CD pipeline

**Stage 9: Documentation & Release**
- Complete documentation
- User guides and API reference
- Release preparation
- v0.1.0 launch

**See:** Development Plan (`.dev/development-plan.md`) for detailed stage information

---

## Alpha Releases (v0.1.0 - v0.9.0)

### v0.1.0 - Foundation Release ✅ COMPLETE

**Status:** Released  
**Focus:** Core functionality and stability

**Features:**
- Interactive TUI and non-interactive modes
- Provider-agnostic architecture (Ollama)
- Comprehensive tool system
- Policy engine with approval modes
- Context management with VRAM monitoring
- Session recording and compression
- Hook system for automation
- MCP integration
- Extension framework
- Testing infrastructure

**Documentation:** Complete user guides and API reference

---

### v0.2.0 - File Explorer & Enhanced MCP

**Status:** 📋 Planned  
**Focus:** File management and MCP improvements

**Features:**

**File Explorer:**
- Tree-based file browser in side panel
- File operations (open, edit, rename, delete)
- Git status indicators
- Quick file search
- Syntax highlighting preview
- Follow active file mode

**MCP Enhancements:**
- OAuth authentication support
- Health monitoring system
- Server restart capabilities
- Marketplace integration
- Enhanced server management

**Specifications:**
- `.kiro/specs/v0.2.0 File Explorer/`
- `.kiro/specs/v0.2.0 MCP Integration/`

---

### v0.3.0 - Advanced File Explorer & MCP Polish

**Status:** 📋 Planned  
**Focus:** Enhanced file management and MCP stability

**Features:**

**Advanced File Explorer:**
- Multi-file selection
- Drag and drop support
- File preview panel
- Advanced search and filtering
- Bookmarks and favorites
- Custom file actions

**MCP Polish:**
- Improved error handling
- Better resource management
- Enhanced tool discovery
- Performance optimizations
- Stability improvements

**Specifications:**
- `.kiro/specs/v0.3.0 File Explorer/`
- `.kiro/specs/v0.3.0 MCP Integration/`

---

### v0.4.0 - Code Editor

**Status:** 📋 Planned  
**Focus:** Built-in terminal-based code editor

**Features:**
- Terminal-based code editor (3rd window)
- Cursor navigation and text editing
- Undo/redo support
- Copy/cut/paste operations
- Find and go-to-line
- Syntax highlighting (50+ languages)
- Prettier formatting integration
- Multiple file tabs
- Auto-save functionality
- External change detection

**Benefits:**
- Edit files without leaving OLLM CLI
- Seamless integration with chat and terminal
- Quick edits during development
- Syntax-aware editing

**Specifications:** `.kiro/specs/v0.4.0 Code Editor/`

---

### v0.5.0 - RAG Integration

**Status:** 📋 Planned  
**Focus:** Semantic search and codebase understanding

**Features:**
- Codebase indexing with embeddings
- Semantic code search
- Context-aware file discovery
- Symbol and definition search
- Documentation search
- Vector database integration
- Incremental indexing
- Search result ranking

**Benefits:**
- Find relevant code quickly
- Better context for LLM
- Improved code understanding
- Faster development workflow

**Specifications:** `.kiro/specs/v0.5.0 RAG-integration/`

---

### v0.6.0 - Release Kraken

**Status:** 📋 Planned  
**Focus:** External LLM provider integration

**Features:**

**CLI-Based Providers:**
- Gemini CLI integration
- Claude Code integration
- Codex CLI integration
- Subprocess execution bridge
- STDIN/STDOUT communication

**API-Based Providers:**
- OpenAI API integration
- Anthropic API integration
- Google AI API integration
- Streaming response handling
- Token usage tracking

**Kraken Management:**
- Provider discovery and health checks
- Intelligent provider selection
- Context transfer between providers
- Cost tracking and budget enforcement
- Auto-escalation on local model failure
- Confirmation dialogs for external requests

**Benefits:**
- Access to powerful cloud models
- Fallback when local models struggle
- Cost-effective provider selection
- Seamless context sharing

**Specifications:** `.kiro/specs/v0.6.0 Release Kraken/`

---

### v0.7.0 - GitHub Integration

**Status:** 📋 Planned  
**Focus:** GitHub workflow integration

**Features:**
- GitHub API integration
- Repository management
- Issue and PR creation
- Code review assistance
- Branch management
- Commit history analysis
- GitHub Actions integration
- Gist support

**Benefits:**
- Manage GitHub from CLI
- Automated PR creation
- AI-assisted code review
- Streamlined workflow

**Specifications:** `.kiro/specs/v0.7.0 GitHub-integration/`

---

### v0.8.0 - Cross-Platform Support

**Status:** 📋 Planned  
**Focus:** Enhanced Windows, macOS, and Linux compatibility

**Features:**

**Platform Detection:**
- Automatic OS detection
- Platform-specific defaults
- Shell selection (cmd.exe, /bin/sh)
- Python command resolution

**Configuration Paths:**
- Windows: %APPDATA%\ollm
- macOS: ~/Library/Application Support/ollm
- Linux: XDG directories (~/.config/ollm)
- Legacy ~/.ollm support

**Terminal Capabilities:**
- Color support detection
- Unicode support detection
- TTY detection
- ASCII fallback characters

**GPU Monitoring:**
- NVIDIA (nvidia-smi) on Windows/Linux
- AMD (rocm-smi) on Linux
- Apple Silicon on macOS
- Graceful fallback to CPU mode

**Path Normalization:**
- Forward slash display
- Native separator for operations
- Mixed separator handling
- Cross-platform path resolution

**Benefits:**
- Consistent experience across platforms
- Platform-appropriate defaults
- Better terminal adaptation
- Reliable GPU monitoring

**Specifications:** `.kiro/specs/v0.8.0 Cross Platform/`

---

### v0.9.0 - vLLM & Open Source Providers

**Status:** 📋 Planned  
**Focus:** High-performance inference engines

**Features:**
- vLLM provider integration
- LM Studio support
- Text Generation WebUI support
- LocalAI support
- High-performance streaming
- Batch processing support
- Model serving optimization

**Benefits:**
- Faster inference
- Better resource utilization
- Production-ready deployment
- Multiple backend options

**Specifications:** `.kiro/specs/v0.9.0 vLLM-LMS Providers/`

---

## Beta Release (v1.0.0+)

### v1.0.0 - Beta Release

**Status:** 🎯 Future  
**Focus:** Production readiness and stability

**Goals:**
- Feature complete for core workflows
- Production-grade stability
- Comprehensive documentation
- Performance optimization
- Security hardening
- Community feedback integration

**Criteria for Beta:**
- All alpha features implemented and tested
- No critical bugs
- Complete user documentation
- API stability guarantees
- Migration guides
- Community adoption

---

## Feature Roadmap Summary

### Current (v0.1.0)
- ✅ Core TUI and non-interactive modes
- ✅ Ollama provider integration
- ✅ Tool system with policy engine
- ✅ Context management with VRAM monitoring
- ✅ Session recording and compression
- ✅ Hook system and extensions
- ✅ MCP integration
- ✅ Model management
- ✅ Testing infrastructure

### Near Term (v0.2.0 - v0.4.0)
- 📋 File Explorer with git integration
- 📋 Enhanced MCP with OAuth and health monitoring
- 📋 Built-in code editor
- 📋 Advanced file management

### Mid Term (v0.5.0 - v0.7.0)
- 📋 RAG integration for semantic search
- 📋 External LLM providers (Kraken)
- 📋 GitHub integration
- 📋 Cost tracking and budgets

### Long Term (v0.8.0 - v0.9.0)
- 📋 Enhanced cross-platform support
- 📋 vLLM and open source providers
- 📋 Performance optimizations
- 📋 Production deployment features

### Beta (v1.0.0+)
- 🎯 Production readiness
- 🎯 API stability
- 🎯 Community features
- 🎯 Enterprise support

---

## Implementation Priorities

### High Priority
1. **File Explorer** (v0.2.0) - Essential for file management
2. **Code Editor** (v0.4.0) - Core development workflow
3. **Kraken Integration** (v0.6.0) - Access to powerful models
4. **RAG Integration** (v0.5.0) - Better code understanding

### Medium Priority
1. **GitHub Integration** (v0.7.0) - Workflow automation
2. **Cross-Platform** (v0.8.0) - Broader compatibility
3. **vLLM Support** (v0.9.0) - Performance improvements

### Community-Driven
- Feature requests from users
- Integration with popular tools
- Platform-specific enhancements
- Performance optimizations

---

## Contributing

We welcome contributions to any planned feature!

### How to Contribute

1. **Review Specifications**
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

### New Feature Ideas

- Check existing specifications
- Open GitHub Discussion
- Describe use case and benefits
- Gather community feedback

---

## Feedback and Priorities

Your input shapes the roadmap!

**Feature Requests:**
- Open issue with "feature-request" label
- Describe your use case
- Explain expected benefits

**Priority Feedback:**
- Comment on existing issues
- Vote with 👍 reactions
- Share your workflow needs

**Roadmap Discussion:**
- Open issue with "roadmap" label
- Suggest priority changes
- Discuss feature dependencies

---

## Related Documentation

### Roadmap Documents
- [Roadmap Overview](roadmap.md) - This document
- [Future Development](future-development.md) - Detailed future plans
- [Visual Roadmap](road_map_visual.md) - Timeline and diagrams
- [Future Features](future-features.md) - Quick reference

### Development Documents
- Development Plan (`.dev/development-plan.md`) - Completed stages 1-9
- Specifications (`.kiro/specs/`) - Detailed feature specs
- Knowledge DB (`dev/docs/knowledgeDB/`) - Technical documentation

### User Documentation
- [README](../../README.md) - Project overview
- [Quick Start](../quickstart.md) - Getting started
- [Configuration](../UserInterface/configuration.md) - Settings reference
- [Troubleshooting](../troubleshooting.md) - Common issues

---

## Version History

| Version | Release Date | Status | Highlights |
|---------|--------------|--------|------------|
| v0.1.0 | 2026-01-26 | ✅ Released | Foundation release with core features |
| v0.2.0 | TBD | 📋 Planned | File Explorer & Enhanced MCP |
| v0.3.0 | TBD | 📋 Planned | Advanced File Explorer & MCP Polish |
| v0.4.0 | TBD | 📋 Planned | Code Editor |
| v0.5.0 | TBD | 📋 Planned | RAG Integration |
| v0.6.0 | TBD | 📋 Planned | Release Kraken |
| v0.7.0 | TBD | 📋 Planned | GitHub Integration |
| v0.8.0 | TBD | 📋 Planned | Cross-Platform Support |
| v0.9.0 | TBD | 📋 Planned | vLLM & Open Source Providers |
| v1.0.0 | TBD | 🎯 Future | Beta Release |

---

**Last Updated:** January 26, 2026  
**Current Version:** v0.1.0 (Alpha)  
**Next Release:** v0.2.0 (Planned)
