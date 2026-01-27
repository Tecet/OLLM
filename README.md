# OLLM CLI

> A local-first command-line interface for open-source LLMs with intelligent context management, tools, hooks, and MCP integration.

![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)
![Node Version](https://img.shields.io/badge/node-%3E%3D20.0.0-brightgreen.svg)
![TypeScript](https://img.shields.io/badge/TypeScript-5.9-blue.svg)
![npm](https://img.shields.io/badge/npm-@tecet%2Follm-red.svg)

OLLM CLI brings the power of open-source large language models to your terminal with a focus on local-first operation, intelligent resource management, and extensibility. Built with TypeScript and React, it provides a modern terminal UI while maintaining compatibility with automation workflows.

---

## ✨ Features

### 🎨 Interactive Terminal UI
- **React + Ink powered interface** with streaming responses and real-time updates
- **Syntax highlighting** for code blocks with language detection
- **Status bar** showing model, context usage, and VRAM metrics
- **Tool execution preview** with diff visualization for file changes

### 🧠 Smart Context Management
- **Fixed context sizing** based on available VRAM (determined at startup)
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

---

## 🚀 Quick Start

### Installation

Install OLLM CLI globally using npm:

```bash
# Install from npm
npm install -g @tecet/ollm

# The interactive installer will guide you through:
# 1. Installing Ollama (if needed)
# 2. Downloading a starter model
# 3. Setting up configuration

# Start using OLLM CLI
ollm
```

**That's it!** The installer handles everything automatically.

For detailed installation instructions, see the **[Installation Guide](docs/Installation.md)**.

### First Steps

```bash
# Start interactive mode
ollm

# Or try a quick question
ollm -p "Explain async/await in JavaScript"

# Select a specific model
ollm --model llama3.1:8b

# Get help
ollm --help
```

### Learn More

- **[Introduction](docs/Introduction.md)** - What is OLLM CLI and why use it?
- **[Quick Start Guide](docs/Quickstart.md)** - Get up and running in 5 minutes
- **[Complete Documentation](docs/README.md)** - Full documentation (57 guides)

---

## 📖 Documentation

### Getting Started
- **[Introduction](docs/Introduction.md)** - What is OLLM CLI? (friendly, non-technical)
- **[Installation](docs/Installation.md)** - Complete installation guide
- **[Quick Start](docs/Quickstart.md)** - Get started in 5 minutes
- **[Troubleshooting](docs/Troubleshooting.md)** - Common issues and solutions

### Core Features
- **[User Interface & Settings](docs/UI&Settings/README.md)** - Interface, commands, themes, keybinds, and configuration
- **[Context Management](docs/Context/README.md)** - Context sizing, compression, checkpoints, and VRAM monitoring
- **[Model Management](docs/LLM%20Models/README.md)** - Models, providers, compatibility, and memory system
- **[Tools System](docs/Tools/README.md)** - Tool execution, architecture, and manifest reference
- **[Hooks System](docs/Hooks/README.md)** - Event-driven automation, protocol, and visual guide
- **[MCP Integration](docs/MCP/README.md)** - Model Context Protocol, marketplace, and commands
- **[Prompts System](docs/Prompts%20System/README.md)** - System prompts, templates, and routing

### Development
- **[Development Roadmap](docs/DevelopmentRoadmap/README.md)** - Future plans and version releases (v0.2.0-v0.9.0)
- **[Complete Index](docs/Index.md)** - All 57 documentation files organized by topic

**Total Documentation:** 57 comprehensive guides covering every aspect of OLLM CLI

---

## ⚙️ System Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **Node.js** | 20.0.0 | 22.x LTS |
| **RAM** | 8GB | 16GB+ |
| **VRAM** | 4GB | 8GB+ |
| **Storage** | 10GB | 50GB+ (for models) |
| **OS** | Windows 10, macOS 11, Linux | Latest versions |

### Recommended Models by VRAM

| VRAM | Recommended Models | Context Sweet Spot |
|------|-------------------|-------------------|
| 4GB | Llama 3.2 3B, Qwen3 4B | 4K-8K tokens |
| 8GB | Llama 3.1 8B, Mistral 7B | 8K-16K tokens |
| 12GB | Gemma 3 12B, Phi-4 14B | 16K-32K tokens |
| 24GB+ | Qwen3 32B, Mixtral 8x7B | 32K-64K tokens |

---

## 🎯 Key Concepts

### Context Management

OLLM CLI uses **fixed context sizing** determined at startup based on your available VRAM:

- **Minimal** (2K tokens) - Very small models or limited VRAM
- **Basic** (4K tokens) - Small models, basic conversations
- **Standard** (8K tokens) - Most common use case
- **Premium** (16K tokens) - Larger models, complex tasks
- **Ultra** (32K+ tokens) - High-end hardware, long conversations

Context is **fixed for the session** and automatically compressed when approaching limits.

### Tool System

The AI can use tools to help you:
- **File Tools** - Read, write, edit files
- **Web Tools** - Search internet, fetch pages
- **Shell Tool** - Run commands
- **Memory Tools** - Remember facts across sessions
- **MCP Tools** - Connect to external services

### Approval Modes

Control how tools are executed:
- **ASK** - Confirm each tool use (default, safest)
- **AUTO** - Auto-approve safe tools, ask for risky ones
- **YOLO** - Auto-approve everything (use with caution!)

---

## 🔧 Common Commands

### CLI Flags

```bash
# Interactive mode
ollm

# One-shot prompt
ollm -p "Explain async/await"

# Select model
ollm --model llama3.1:8b

# List models
ollm --list-models

# JSON output
ollm -p "List 5 languages" --output json

# Debug mode
ollm --debug

# Show version
ollm --version
```

### Interactive Slash Commands

```bash
/model list              # List available models
/model use <name>        # Switch model
/context                 # Show context status
/theme list              # List themes
/theme use <name>        # Switch theme
/session list            # List sessions
/help                    # Show help
/exit                    # Exit
```

See **[Commands Reference](docs/UI&Settings/Commands.md)** for complete list.

---

## 🏗️ Project Structure

```
ollm-cli/
├── packages/
│   ├── cli/              # CLI entry point and UI components
│   │   ├── src/
│   │   │   ├── cli.tsx           # Main entry
│   │   │   ├── commands/         # Slash commands
│   │   │   └── ui/               # React components
│   │   └── package.json
│   │
│   ├── core/             # Core runtime and business logic
│   │   ├── src/
│   │   │   ├── context/          # Context management
│   │   │   ├── tools/            # Tool system
│   │   │   ├── hooks/            # Hook system
│   │   │   ├── services/         # Business logic
│   │   │   └── mcp/              # MCP integration
│   │   └── package.json
│   │
│   ├── ollm-bridge/      # Provider adapters
│   │   ├── src/
│   │   │   └── provider/         # Ollama, vLLM, OpenAI adapters
│   │   └── package.json
│   │
│   └── test-utils/       # Shared test utilities
│       └── package.json
│
├── docs/                 # Documentation
├── scripts/              # Build and utility scripts
├── schemas/              # JSON schemas
└── package.json          # Root workspace config
```

---

## 🛠️ Development

### For Contributors

Want to contribute to OLLM CLI? Here's how to get started:

```bash
# Clone the repository
git clone https://github.com/tecet/ollm.git
cd ollm

# Install dependencies
npm install

# Build all packages
npm run build

# Run tests
npm test

# Start development
npm start
```

### Development Commands

```bash
npm run build          # Build all packages
npm test               # Run tests
npm run lint           # Lint code
npm run format         # Format code
npm start              # Run CLI
```

### Project Structure

```
ollm-cli/
├── packages/
│   ├── cli/              # CLI entry and UI
│   ├── core/             # Core runtime
│   ├── ollm-bridge/      # Provider adapters
│   └── test-utils/       # Test utilities
├── docs/                 # Documentation (57 files)
├── scripts/              # Build scripts
└── package.json          # Root workspace
```

See **[Project Structure](.kiro/steering/structure.md)** for detailed architecture.

---

## 🗺️ Roadmap

OLLM CLI is under active development with a clear roadmap for future features.

### ✅ Completed (v0.1.0 - Alpha)
- Interactive TUI with React + Ink
- Context management with VRAM monitoring
- Tool system with policy engine
- Session recording and compression
- Hook system for automation
- MCP integration
- Comprehensive documentation (57 guides)
- Testing infrastructure

### 🔮 Planned Features (v0.2.0 - v0.9.0)

**v0.2.0 - Enhanced Context Management**
- Advanced context pool management
- Multi-tier context strategies
- Improved VRAM optimization

**v0.3.0 - Advanced Compression**
- Multiple compression strategies
- Semantic compression
- Context checkpointing

**v0.4.0 - Reasoning Models**
- Extended reasoning support
- Reasoning capture and display
- Specialized reasoning modes

**v0.5.0 - Session Management**
- Enhanced session persistence
- Session templates
- Collaborative sessions

**v0.6.0 - Multi-Provider Support**
- OpenAI, Anthropic, Google AI
- Cost tracking and budgets
- Auto-escalation between providers

**v0.7.0 - Developer Productivity**
- Git integration
- @-mentions for context
- Diff review workflows

**v0.8.0 - Intelligence Layer**
- Semantic codebase search (RAG)
- Structured output
- Code execution sandbox
- Vision support

**v0.9.0 - Cross-Platform Polish**
- Platform-specific optimizations
- Enhanced Windows support
- Improved terminal compatibility

**v1.0.0+ - Beta and Beyond**
- Production-ready release
- Enterprise features
- Plugin marketplace

See **[Development Roadmap](docs/DevelopmentRoadmap/Roadmap.md)** for detailed specifications.

---

## 🧰 Tech Stack

### Runtime & Language
- **Node.js 20+** - JavaScript runtime
- **TypeScript 5.9** - Type-safe development
- **ES Modules** - Modern module system

### Build & Tooling
- **npm workspaces** - Monorepo management
- **esbuild** - Fast bundling
- **Vitest** - Testing framework
- **ESLint** - Code linting
- **Prettier** - Code formatting

### UI Framework
- **React 19** - UI library
- **Ink 6** - Terminal rendering

### Key Dependencies
- **yargs** - CLI argument parsing
- **yaml** - Configuration parsing
- **ajv** - JSON schema validation
- **fast-check** - Property-based testing

---

## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

```
Copyright 2026 OLLM CLI Contributors

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

---

## 🤝 Contributing

We welcome contributions! Here's how you can help:

### Ways to Contribute

1. **Report Bugs** - Open an issue with details and reproduction steps
2. **Suggest Features** - Share your ideas in the discussions
3. **Submit Pull Requests** - Fix bugs or implement features
4. **Improve Documentation** - Help make the docs clearer
5. **Write Tests** - Increase test coverage

### Contribution Guidelines

1. **Fork the repository** and create a feature branch
2. **Follow the code style** - Run `npm run lint` and `npm run format`
3. **Write tests** for new features
4. **Update documentation** as needed
5. **Submit a pull request** with a clear description

### Development Workflow

```bash
# 1. Fork and clone
git clone https://github.com/tecet/ollm.git
cd ollm

# 2. Create a feature branch
git checkout -b feature/my-feature

# 3. Make changes and test
npm install
npm run build
npm test

# 4. Commit with clear messages
git commit -m "feat: add new feature"

# 5. Push and create PR
git push origin feature/my-feature
```

### Code of Conduct

- Be respectful and inclusive
- Provide constructive feedback
- Focus on the code, not the person
- Help create a welcoming environment

---

## 🙏 Acknowledgments

OLLM CLI is built on the shoulders of giants:

- **[Ollama](https://ollama.com/)** - Local LLM runtime
- **[React](https://react.dev/)** & **[Ink](https://github.com/vadimdemedes/ink)** - Terminal UI
- **[Vitest](https://vitest.dev/)** - Testing framework
- **[fast-check](https://fast-check.dev/)** - Property-based testing
- All our **[contributors](https://github.com/tecet/ollm/graphs/contributors)**

---

## 📞 Support

- **Documentation**: [docs/README.md](docs/README.md)
- **Issues**: [GitHub Issues](https://github.com/tecet/ollm/issues)
- **Discussions**: [GitHub Discussions](https://github.com/tecet/ollm/discussions)

---

<div align="center">

**[⬆ Back to Top](#ollm-cli)**

Made with ❤️ by **[tecet](https://github.com/tecet)**

</div>
