# Welcome to OLLM CLI

**Your Local AI Assistant in the Terminal**

OLLM CLI is a friendly command-line tool that brings the power of AI language models right to your terminal. Think of it as having a knowledgeable assistant who can help you code, write, research, and solve problems—all while keeping your data private and secure on your own computer.

---

## What is OLLM CLI?

Imagine having a conversation with an AI that can:
- Help you write and debug code
- Answer questions about your project
- Search the web for information
- Read and edit files in your workspace
- Remember important details across conversations
- Run commands and automate tasks

That's OLLM CLI. It's like ChatGPT, but it runs entirely on your computer using open-source AI models through [Ollama](https://ollama.ai).

---

## Why Use OLLM CLI?

### 🔒 Privacy First
Your conversations, code, and data never leave your computer. No cloud services, no data collection, no privacy concerns.

### 💰 Completely Free
No subscriptions, no API costs, no hidden fees. Once installed, it's yours to use as much as you want.

### 🚀 Powerful Features
- **Smart Tools** - The AI can read files, search the web, and run commands
- **Memory System** - Remembers important information across sessions
- **Multiple Models** - Choose from dozens of open-source AI models
- **Customizable** - Themes, modes, and settings to match your workflow

### 🎯 Built for Developers
- Syntax highlighting for code
- Git integration
- File explorer
- Terminal integration
- Keyboard-driven interface

---

## How Does It Work?

OLLM CLI is like a bridge between you and AI models:

```
You → OLLM CLI → Ollama → AI Model → Response → OLLM CLI → You
```

1. **You type a message** in the terminal
2. **OLLM CLI** processes your request and adds context
3. **Ollama** runs the AI model on your computer
4. **The AI** generates a response
5. **OLLM CLI** displays the response with nice formatting

The AI can also use "tools" to help you:
- Read files in your project
- Search the internet
- Run shell commands
- Edit code
- And more!

---

## What Can You Do With It?

### For Coding

```
You: "Read the main.ts file and explain what it does"
AI: [Reads file] "This file is the entry point for your application..."

You: "Add error handling to the login function"
AI: [Edits file] "I've added try-catch blocks and proper error messages..."
```

### For Learning

```
You: "Explain how async/await works in JavaScript"
AI: "Async/await is a way to handle asynchronous operations..."

You: "Show me an example"
AI: "Here's a practical example: [code example]"
```

### For Research

```
You: "Search for the latest React 19 features"
AI: [Searches web] "React 19 introduces several new features..."

You: "How do I use the new 'use' hook?"
AI: "The 'use' hook allows you to..."
```

### For Automation

```
You: "Run the tests and tell me if they pass"
AI: [Runs npm test] "All 42 tests passed! Here's the summary..."

You: "If they failed, fix the issues"
AI: [Analyzes errors, edits files] "I've fixed the failing tests..."
```

---

## Key Features Explained

### 🎨 Beautiful Terminal Interface

OLLM CLI has a modern, colorful interface that makes conversations easy to follow:
- **Header Bar** - Shows your current model, context usage, and mode
- **Chat Area** - Your conversation with the AI
- **Side Panel** - Quick access to tools, files, and settings
- **Status Bar** - Helpful keyboard shortcuts and status

### 🧠 Smart Context Management

The AI needs to remember your conversation, but it has limits. OLLM CLI automatically:
- Tracks how much "memory" is being used
- Compresses old messages when needed
- Keeps important information
- Lets you save and restore conversation states

### 🛠️ Powerful Tools

The AI can use tools to help you:
- **File Tools** - Read, write, and edit files
- **Web Tools** - Search the internet and fetch web pages
- **Shell Tool** - Run commands in your terminal
- **Memory Tools** - Remember important facts across sessions
- **MCP Tools** - Connect to external services (GitHub, databases, etc.)

### 🎯 Multiple Modes

Switch between different "personalities" for different tasks:
- **Assistant Mode** - General help and conversation
- **Developer Mode** - Focused on coding and technical tasks
- **Planning Mode** - Help with project planning and architecture
- **Debugger Mode** - Systematic problem-solving

### 🎨 Customizable Themes

Choose from beautiful color schemes:
- Solarized Dark (default)
- Neon Dark
- Dracula
- Nord
- Monokai

---

## Who Is It For?

### Developers
- Write code faster with AI assistance
- Debug issues with intelligent help
- Learn new technologies
- Automate repetitive tasks

### Students
- Get explanations of complex topics
- Help with homework and projects
- Learn programming concepts
- Practice coding with feedback

### Researchers
- Search and summarize information
- Analyze documents
- Generate reports
- Organize research notes

### Anyone Who Uses a Terminal
- Automate tasks
- Get quick answers
- Manage files and projects
- Learn command-line tools

---

## What Makes It Different?

### vs. ChatGPT
- ✅ Runs on your computer (private)
- ✅ Free to use (no subscription)
- ✅ Can access your files and run commands
- ✅ Customizable and extensible
- ❌ Requires more setup
- ❌ Needs a decent computer

### vs. GitHub Copilot
- ✅ Works in the terminal, not just editors
- ✅ Can search the web and run commands
- ✅ Free and open-source
- ✅ More conversational
- ❌ Not integrated into your code editor
- ❌ Requires manual setup

### vs. Other CLI Tools
- ✅ Full conversation interface
- ✅ Rich terminal UI with colors and formatting
- ✅ Tool system for file operations
- ✅ Memory across sessions
- ✅ Multiple AI models to choose from

---

## System Requirements

### Minimum Requirements
- **Computer:** Modern laptop or desktop
- **RAM:** 8GB (16GB recommended)
- **Storage:** 10GB free space
- **OS:** Windows 10+, macOS 11+, or Linux
- **Node.js:** Version 20 or higher

### Recommended Setup
- **RAM:** 16GB or more
- **GPU:** NVIDIA, AMD, or Apple Silicon (for faster AI)
- **Storage:** SSD with 20GB+ free space
- **Terminal:** Modern terminal emulator (Windows Terminal, iTerm2, Alacritty)

---

## Quick Example

Here's what a typical conversation looks like:

```
$ ollm

┌─────────────────────────────────────────────────────────────────┐
│ 🦙 llama3.2:3b │ 1.2K/8K │ 🎯 Assistant │ 🕐 14:32:15        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ You: Hello! Can you help me with my project?                   │
│                                                                  │
│ Assistant: Of course! I'd be happy to help. What are you       │
│ working on?                                                     │
│                                                                  │
│ You: I'm building a web app with React. Can you read my        │
│ package.json and tell me what dependencies I have?             │
│                                                                  │
│ Assistant: [Uses read_file tool]                               │
│ I can see you're using React 18.2.0, TypeScript 5.0, and       │
│ several other libraries. Here's a summary:                      │
│                                                                  │
│ - React 18.2.0 (UI framework)                                  │
│ - TypeScript 5.0 (type safety)                                 │
│ - Vite 4.3 (build tool)                                        │
│ - React Router 6.11 (routing)                                  │
│                                                                  │
│ Would you like me to suggest any updates or improvements?      │
│                                                                  │
│ You: Yes, please check if there are newer versions             │
│                                                                  │
│ Assistant: [Uses web_search tool]                              │
│ I found that React 19 was recently released with some great    │
│ new features. Would you like me to help you upgrade?           │
│                                                                  │
│ > _                                                             │
├─────────────────────────────────────────────────────────────────┤
│ ● Ready | Ctrl+K: Commands | Ctrl+P: Panel | Ctrl+/: Help     │
└─────────────────────────────────────────────────────────────────┘
```

---

## Getting Started

Ready to try OLLM CLI? Here's what to do next:

1. **[Install OLLM CLI](Installation.md)** - Step-by-step installation guide
2. **[Quick Start](Quickstart.md)** - Get up and running in 5 minutes
3. **[User Guide](UI&Settings/UIGuide.md)** - Learn the interface
4. **[Commands Reference](UI&Settings/Commands.md)** - All available commands

---

## Need Help?

- **[Troubleshooting Guide](Troubleshooting.md)** - Common issues and solutions
- **[Documentation](README.md)** - Complete documentation
- **[GitHub Issues](https://github.com/tecet/ollm/issues)** - Report bugs or request features
- **[Discussions](https://github.com/tecet/ollm/discussions)** - Ask questions and share ideas

---

## Philosophy

OLLM CLI is built on these principles:

**🔒 Privacy First**
Your data stays on your computer. No telemetry, no tracking, no cloud services.

**💡 User-Friendly**
Powerful features with a simple, intuitive interface. You shouldn't need a PhD to use AI.

**🎯 Practical**
Built for real work, not demos. Tools that actually help you get things done.

**🌟 Open Source**
Free forever, community-driven, transparent development.

**⚡ Fast & Efficient**
Optimized for performance, minimal resource usage, quick responses.

---

## What's Next?

OLLM CLI is actively developed with exciting features coming soon:

- **v0.2.0** - Enhanced context management
- **v0.3.0** - Advanced compression strategies
- **v0.4.0** - Better reasoning model support
- **v0.5.0** - Improved session management
- **v0.6.0** - Multiple AI providers (Claude, GPT, Gemini)

See the [Roadmap](DevelopmentRoadmap/Roadmap.md) for details.

---

## Join the Community

OLLM CLI is open source and welcomes contributions:

- **GitHub:** [github.com/tecet/ollm](https://github.com/tecet/ollm)
- **Issues:** Report bugs and request features
- **Discussions:** Share ideas and get help
- **Pull Requests:** Contribute code and documentation

---

**Ready to get started?** Head to the [Installation Guide](Installation.md) to install OLLM CLI on your computer!

---

**Last Updated:** January 26, 2026  
**Version:** 0.1.0  
**Author:** tecet
