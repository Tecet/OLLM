# Changelog

All notable changes to this project will be documented in this file.

The format is based on Keep a Changelog (https://keepachangelog.com/en/1.0.0/),
and this project adheres to Semantic Versioning (https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- New features in development

### Changed

- Changes to existing functionality

### Fixed

- Bug fixes

## [0.1.2] - 2026-01-30

### Added

- DEFAULT_CONTEXT_OPTIONS constant in test utilities for consistent test setup
- Comprehensive test coverage documentation in `.dev/backlog/30-01-2026-PublishAlpha/`
- Documentation for tool overload issue and fix

### Changed

- **CRITICAL FIX: Tool Overload** - Reduced default tools per mode from 18 to 5-10
  - Developer mode: 8 tools (was: all 18)
  - Debugger mode: 7 tools (was: all 18)
  - User mode: 10 tools (was: all 18)
  - Assistant mode: 3 tools (unchanged)
  - Planning mode: 10 tools (unchanged)
  - **Impact:** Prevents LLM from launching 5+ tools per request
  - **Benefit:** Faster responses, more focused tool usage, better UX
- **Context Management**: Tier budgets now calculated dynamically based on context size
  - Tier 1: 12% of context (min 450 tokens) - increased to accommodate system prompt mandates
  - Tier 2: 9% of context (min 700 tokens) - increased to accommodate system prompt mandates
  - Tier 3: 6% of context (min 1000 tokens)
  - Tier 4: 5% of context (min 1500 tokens)
  - Tier 5: 2% of context (min 1500 tokens)
- **Compression System**: Removed `tierBudget` parameter from compression methods
  - `shouldCompress()` now takes 3 parameters: currentTokens, modelId, systemPromptTokens
  - `getCompressionUrgency()` now takes 3 parameters: currentTokens, modelId, systemPromptTokens
  - Tier budget is already included in systemPromptTokens, no need to subtract separately
- **ChatClient**: Now requires either `contextMgmtManager` or explicit `contextSize`/`ollamaContextSize` in options
- **Package Name**: Changed from `@ollm/cli` to `@tecet/ollm` (scope ownership)
- Updated all test files to use new context management system
- Updated repository URLs to tecet/ollm

### Fixed

- Fixed 42 failing tests (improved pass rate from 93.8% to 96.4%)
- Fixed all tier-aware compression tests (12 tests) - updated to dynamic budget calculations
- Fixed all provider-aware compression tests (5 tests) - removed tierBudget parameter
- Fixed 25 chatClient tests - added context size options
- Fixed ESLint errors (19 errors, 1 warning) - removed unused imports, fixed import order
- Fixed TypeScript compilation - 0 errors
- Fixed `/test prompt` command to show actual tool schemas from ToolRegistry
- Fixed `/test prompt --budget` validation script paths

### Known Issues



### Documentation

- Added `.dev/backlog/30-01-2026-PublishAlpha/TASKS.md` - Alpha release task tracker
- Added `.dev/backlog/30-01-2026-PublishAlpha/TEST_FIXES_SUMMARY.md` - Test fix analysis
- Added `.dev/backlog/30-01-2026-PublishAlpha/TEST_FIXES_FINAL.md` - Final test report
- Updated `.dev/backlog/backlog.md` with unfinished work from previous sessions

### Technical Details

**Test Suite Statistics:**

- Total Tests: 1596
- Passing: 1539 (96.4%)
- Failing: 57 (3.6%)
- Improvement: +42 tests fixed, +2.6% pass rate

**Code Quality:**

- ESLint: 0 errors, 0 warnings ✅
- TypeScript: 0 compilation errors ✅
- Prettier: All files formatted ✅

## [0.1.6] - 2026-01-30

### Added

#### Comprehensive Documentation

- **57 complete documentation files** covering every aspect of OLLM CLI
- **Getting Started Guides**: Introduction, Installation, Quick Start, Troubleshooting
- **User Interface Documentation**: UI Guide, Commands, Keybinds, Themes, Terminal, Configuration
- **Context Management Documentation**: Architecture, Compression, Management, Checkpoints
- **Model Management Documentation**: Model discovery, compatibility, memory system
- **Tools System Documentation**: Architecture, User Guide, Getting Started, Manifest Reference
- **Hooks System Documentation**: Architecture, User Guide, Protocol, Visual Guide, Keyboard Shortcuts
- **MCP Integration Documentation**: Architecture, Getting Started, Integration, Commands, Marketplace
- **Prompts System Documentation**: Architecture, System Prompts, Templates, Routing
- **Development Roadmap**: Complete roadmap with version-based releases (v0.2.0-v0.9.0)
- **Complete Index**: All documentation organized by topic, audience, and goal

#### Interactive Terminal UI

- React + Ink powered terminal interface with streaming responses
- Syntax highlighting for code blocks with automatic language detection
- Status bar displaying model name, context usage, and VRAM metrics
- Tool execution preview with diff visualization for file changes
- Side panel with context information and GPU statistics
- Hybrid and simple layout modes for different use cases

#### Context Management

- **Fixed context sizing** based on available VRAM (determined at startup)
- Five context tiers: Minimal (2K), Basic (4K), Standard (8K), Premium (16K), Ultra (32K+)
- Automatic compression when approaching context limits using sliding-window strategy
- Snapshot and rollover support for long conversations
- Real-time monitoring of token usage and memory consumption
- VRAM buffer management with configurable thresholds
- Context pool for efficient memory management
- Memory safety guards to prevent out-of-memory errors
- Pre-calculated 85% utilization values in LLM_profiles.json

#### Tool System

- Built-in tools for file operations (read, write, edit, list, glob, grep)
- Shell command execution with timeout and output truncation
- Web fetch and search capabilities
- Memory tool for persistent context across sessions
- Policy-based confirmation system (ASK, AUTO, YOLO modes)
- Diff preview for file edits before applying changes
- Output streaming for long-running operations
- Tool timeout configuration

#### Session Management

- Record and resume conversations with full context preservation
- Automatic session compression to manage context limits
- Loop detection to prevent runaway tool calls
- Session history with searchable archives
- Auto-save functionality with configurable intervals
- Session state persistence across restarts

#### Extensibility

- Hook system for event-driven automation and safety gates
- Extension system with manifest-based configuration
- MCP (Model Context Protocol) integration for external tools
- Provider-agnostic architecture supporting multiple LLM backends
- Skills system for task-specific instruction modules

#### Configuration System

- Layered configuration with clear precedence order
- User-level config at `~/.ollm/settings.json`
- Workspace-level config at `.ollm/settings.json`
- Environment variable support for all major settings
- CLI flag overrides for temporary changes
- JSON schema validation for configuration files
- Support for Ollama provider configuration

#### Model Management

- List available models from provider
- Pull models from remote repositories
- Remove models to free up disk space
- Inspect model details and capabilities
- Model discovery and metadata enrichment
- Tool support detection (four-tier precedence system)
- Context window configuration (85% utilization)
- Reasoning model support with extended timeouts

#### Performance Monitoring

- Real-time GPU statistics (temperature, VRAM usage, utilization)
- Token usage tracking and cost estimation
- Performance metrics display (TTFT, tokens/sec)
- Compact and detailed metrics modes
- Status bar integration for key metrics
- Configurable polling intervals for status updates

#### Developer Features

- Diff review mode with approve/reject workflow
- Inline diff display for small changes
- External pager for large diffs
- Reasoning display for models that support it
- Collapsible reasoning sections
- Debug mode with detailed logging
- Comprehensive error messages with context

#### Testing Infrastructure

- Vitest-based testing framework
- Property-based testing with fast-check
- Unit tests for core functionality
- Integration tests for end-to-end workflows
- Test utilities and fixtures
- Code coverage reporting
- Mock providers for testing

### Changed

- Updated README.md with npm package installation (`@tecet/ollm`)
- Updated all documentation to reflect fixed context sizing (not dynamic)
- Updated context tier system from ranges to labels (Minimal, Basic, Standard, Premium, Ultra)
- Updated compression documentation to reflect LLM-based summarization
- Updated roadmap to use version-based releases (v0.2.0-v0.9.0) instead of stages
- Updated all GitHub links to use correct username (tecet)

### Documentation Structure

- **9 major sections** with complete coverage
- **Getting Started** (4 files): Introduction, Installation, Quickstart, Troubleshooting
- **UI & Settings** (9 files): Complete interface and configuration documentation
- **Context Management** (6 files): Architecture, compression, management, checkpoints
- **Model Management** (6 files): Discovery, compatibility, memory, configuration
- **Tools System** (6 files): Architecture, user guide, getting started, manifest
- **Hooks System** (7 files): Architecture, user guide, protocol, visual guide
- **MCP Integration** (6 files): Architecture, getting started, integration, commands
- **Prompts System** (6 files): Architecture, system prompts, templates, routing
- **Development Roadmap** (6 files): Roadmap, visual timeline, planned features

### Deprecated

- N/A (initial release)

### Removed

- N/A (initial release)

### Fixed

- N/A (initial release)

### Security

- Environment variable sanitization to prevent secret leakage
- Secret redaction in logs and error messages
- Safe shell command execution with proper escaping
- File operation permissions validation
- Sandboxed tool execution environment


<!-- Alpha release v0.1.6 -->

