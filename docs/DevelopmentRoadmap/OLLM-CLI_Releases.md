
# OLLM CLI v0.1.0
Released 30 January 2026


Description:


# ✨ Features  

### 🎨 Interactive Terminal UI

- **React + Ink powered interface** with streaming responses and real-time updates
- **Syntax highlighting** for code blocks with language detection
- **Status bar** showing model, context usage, and VRAM metrics
- **Tool execution preview** with diff visualization for file changes

### 🧠 Smart Context Management

- **Adaptive context sizing** based on available VRAM
- **Automatic compression** when approaching context limits
- **Snapshot and rollover** support for long conversations
- **Real-time monitoring** of token usage and memory consumption

### 🛠️ Powerful Tool System

- **Built-in tools**: File operations, shell execution, web fetch, search, memory
- **Policy-based confirmation**: ASK, AUTO, and YOLO approval modes
- **Diff preview** for file edits before applying changes
- **Output truncation** and streaming for long-running operations

### 🔌 Extensibility

- **Hook system** for event-driven automation and safety gates
- **Extension system** with manifest-based configuration
- **MCP integration** (Model Context Protocol) for external tools
- **Provider-agnostic** architecture supporting multiple LLM backends

### 💾 Session Management

- **Record and resume** conversations with full context
- **Automatic compression** to manage context limits
- **Loop detection** to prevent runaway tool calls
- **Session history** with searchable archives  

### 🌐 Offline First
- **Works without internet** when models are installed locally
- **No telemetry** - all data stays on your machine
- **Local model management** - pull, list, and remove models


## Completed Features (v0.1.0)  

### ✅ Core Features  

The foundation of OLLM CLI has been successfully implemented:  

- **Interactive TUI and Non-Interactive Modes** - Full-featured terminal UI (React + Ink) plus headless execution
- **Provider-Agnostic Architecture** - Flexible adapter system supporting multiple LLM backends
- **Comprehensive Tool System** - Built-in tools for file operations, shell, web fetch/search, memory
- **Policy Engine** - Configurable approval modes (ASK, AUTO, YOLO) with diff preview
- **Context Management** - Dynamic sizing with VRAM monitoring, compression, memory safety
- **Session Recording** - Full session capture with resume and compression
- **Testing Infrastructure** - Comprehensive test suite with property-based testing

#### Pre-Alpha features:
- **Hook System** pre-alpha - Event-driven automation with safety gates
- **MCP Integration** pre-alpha - Model Context Protocol support for external tools
- **Extension System** pre-alpha- Manifest-based extensions for custom functionality




# PLANED DEVELOPMENT ROADMAP

## v0.3.0 The Hooks, Extensions, and MCP

The Hooks, Extensions, and MCP system provides a comprehensive extensibility framework for OLLM CLI. It consists of three interconnected subsystems:
  
1. **Hook System**: Event-driven execution of custom scripts at specific lifecycle points
2. **Extension System**: Manifest-based packaging of hooks, MCP servers, settings, and skills
3. **MCP Integration**: Client for communicating with Model Context Protocol servers to provide external tools  

The design prioritizes security through a trust model, reliability through error isolation, and developer experience through clear protocols and conventions.

#### Tools Overview
  
This design defines the tool system that enables LLMs to interact with the file system, execute shell commands, fetch web content, and perform other operations through a secure, policy-controlled interface. The system consists of four main layers:
  
1. **Tool Registry Layer**: Central registration and discovery of tools with schema exposure
2. **Tool Invocation Layer**: Declarative tool definitions with parameter validation and execution
3. **Policy Engine Layer**: Rule-based confirmation system for controlling tool execution
4. **Built-in Tools Layer**: File operations, shell execution, web tools, and persistent storage  

The design prioritizes safety (policy-controlled execution), extensibility (easy addition of new tools), and usability (clear error messages and streaming output).


## v0.4.0 Intelligence Layer - RAG Vector DB

The intelligence layer brings advanced AI capabilities including codebase indexing with semantic search that's RAG for your local code structured JSON output with schema enforcement, a sandboxed code execution environment for testing snippets, and vision support for analyzing screenshots and images. Combined with cost tracking and prompt templates, this transforms OLLM from a chat interface into a true coding agent.

RAG Vector DB Functionality


# v0.5.0  Release Kraken !
*Planed release end of February 2026*

When local models hit their limits on complex tasks, "Release Kraken" lets you seamlessly escalate to cloud powerhouses. With a single /kraken command, you can invoke Gemini CLI, Claude Code, or Codex CLI directly from O.L.L.M. It also supports API providers like OpenAI and Anthropic. The system handles context transfer, tracks costs, and returns control to your local model when done.

Think of it as having a "phone a friend" option built into your local AI,, call in the big guns for architecture decisions or complex refactoring, then return to fast, private local inference.

Full policy control ensures you approve every escalation, with budget limits and detailed usage tracking.

### The Kraken Integration

The Kraken Integration feature extends OLLM CLI's provider system to support external LLM providers, enabling users to access powerful cloud-based models and CLI-based coding agents when local models need assistance. This design implements a unified provider adapter pattern that seamlessly integrates with the existing architecture while adding new capabilities for subprocess execution, API communication, context transfer, and cost tracking.

The Kraken system integrates with existing OLLM CLI components:  

1. **Provider Registry**: Extended to support Kraken provider types
2. **Policy Engine**: Extended with Kraken-specific confirmation policies
3. **Hook System**: New hook events for Kraken lifecycle
4. **CLI Commands**: New `/kraken` slash command
5. **Status Bar**: Kraken availability indicator
6. **Configuration**: New `kraken` section in config.yaml
  
#### The system introduces two new provider types:

- **CLI Bridge Providers**: Execute terminal-based coding agents (Gemini CLI, Claude Code, Codex CLI) via subprocess enabling users to bring in subscription / CLI based agents to OLLM CLI

- **API Providers**: Connect to cloud LLM APIs (Open AI, Anthropic, Google AI) via HTTPS

  Both provider types implement the existing `ProviderAdapter` interface, ensuring consistency with the local Ollama provider and enabling transparent switching between local and external models.

Benchmarks and fine tuning of LLM behaviours and context management
Cross platform development prep works
  

# v0.5.0  GitHub Integration
*Planed release end of March - mid April 2026*


The Developer Productivity Tools system adds three high-impact features that enable Aider-like developer workflows: Git integration for version control operations, @-mentions for explicit context loading, and diff review mode for interactive change approval. These features work together to provide developers with fine-grained control over context, changes, and version history while maintaining the conversational interface of OLLM CLI.

#### Component Responsibilities  

**Git Tool**: Provides Git operations as a declarative tool (status, diff, commit, log, undo, stash, branch), validates repository state, formats output for LLM consumption, and integrates with policy engine for confirmations.  

**Git Service**: Wraps simple-git library, manages repository operations, tracks AI-made changes for undo, generates semantic commit messages, handles auto-commit workflows, and respects .gitignore patterns. 

**Mention Parser**: Parses @-mentions from user input using regex patterns, identifies mention types (file, glob, symbol, URL, directory), resolves mentions to paths/content, and handles special Git mentions (@git:status, @git:diff, @git:log:N). 

**Context Loader**: Loads file content with metadata, resolves glob patterns to file lists, fetches URL content, provides symbol lookup (when AST parsing available), enforces token limits, and caches loaded content.  

**Diff Reviewer**: Generates unified diffs for file changes, parses diffs into hunks, manages pending review queue, applies approved changes, rejects unwanted changes, and integrates with auto-commit.  

**Configuration Manager**: Manages settings for Git integration (auto-commit, message style), mention parsing (token limits, file limits), and review mode (enabled, auto-approve rules, context lines).  

**UI Components**: Displays diff review panels with syntax highlighting, handles keyboard shortcuts for approval/rejection, shows status bar indicators for Git state and pending reviews, and provides visual feedback for mentions.


#### Additional Tools and Hooks:

##### Tools
- Additional set of finetuned Tools
- Allowing User tools and edits thru UI Manager
##### Hooks
- additional set of fine tuned hooks
- Allowing User Hooks and edits thru UI Manager


# Beta 0.6.0 - 0.9.0
Planed release March - April 2026


Cross-Platform Support 🖥️  

**Windows, macOS, Linux Compatibility** 

Consistent behavior across all major operating systems.

**Key Capabilities:**

- Platform-appropriate config locations
- Cross-platform GPU monitoring
- Terminal capability detection
- Path normalization


- ## v0.6.0 - 

## v0.7.0 -

## v0.8.0 -

## v0.9.0 -



OLLM CLI v 1.0.0

Planed Release - 2nd Quarter 2026