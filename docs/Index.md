# OLLM CLI Documentation Index

**Complete Index of All Documentation**

This index provides a comprehensive overview of all OLLM CLI documentation organized by topic, audience, and type.

---

## 📚 Quick Navigation

- [Getting Started](#getting-started)
- [User Documentation](#user-documentation)
- [System Documentation](#system-documentation)
- [Architecture Documentation](#architecture-documentation)
- [Development Documentation](#development-documentation)
- [Reference Documentation](#reference-documentation)

---

## Getting Started

### [Introduction](Introduction.md)
**What is OLLM CLI and why use it?**

A friendly introduction to OLLM CLI written in natural language. Explains what OLLM CLI is, how it works, key features, and who it's for.

**Topics:** Overview, Features, Use Cases, Philosophy  
**Audience:** Everyone  
**Length:** ~15 min read

---

### [Installation](Installation.md)
**Install OLLM CLI on your computer**

Step-by-step installation guide for all platforms. Includes prerequisites, installation steps, verification, and troubleshooting.

**Topics:** Prerequisites, Installation, Configuration, Verification  
**Audience:** New users  
**Length:** ~10 min read

---

### [Quick Start](Quickstart.md)
**Get up and running in 5 minutes**

Hands-on guide to your first conversation with OLLM CLI. Covers basic usage, tools, commands, and common tasks.

**Topics:** First Steps, Using Tools, Commands, Tips  
**Audience:** New users  
**Length:** ~15 min read

---

### [Troubleshooting](Troubleshooting.md)
**Common issues and solutions**

Comprehensive troubleshooting guide covering connection issues, installation problems, tool execution, and more.

**Topics:** Quick Fixes, Common Issues, Debug Mode, Getting Help  
**Audience:** All users  
**Length:** ~20 min read

---

## User Documentation

### User Interface & Settings

**[README](UI&Settings/README.md)** - Overview and navigation  
**[Index](UI&Settings/Index.md)** - Quick reference with links  
**[UI Guide](UI&Settings/UIGuide.md)** - Complete interface documentation  
**[Commands](UI&Settings/Commands.md)** - All slash commands  
**[Keybinds](UI&Settings/Keybinds.md)** - Keyboard shortcuts  
**[Themes](UI&Settings/Themes.md)** - Theme system and customization  
**[Terminal](UI&Settings/Terminal.md)** - Integrated terminal  
**[Configuration](UI&Settings/Configuration.md)** - Settings and options  
**[Architecture](UI&Settings/Architecture.md)** - UI technical architecture

**Topics:** Interface, Commands, Shortcuts, Themes, Terminal, Settings  
**Files:** 9 documents

---

### Context Management

**[README](Context/README.md)** - Overview and navigation  
**[Index](Context/Index.md)** - Quick reference with links  
**[Context Architecture](Context/ContextArchitecture.md)** - System architecture  
**[Context Management](Context/ContextManagment.md)** - Context sizing and VRAM  
**[Context Compression](Context/ContextCompression.md)** - Compression system  
**[Checkpoint Flow](Context/CheckpointFlowDiagram.md)** - Checkpoint system

**Topics:** Context Tiers, VRAM, Compression, Checkpoints, Memory  
**Files:** 6 documents

---

### Model Management

**[README](LLM%20Models/README.md)** - Overview and navigation  
**[Index](LLM%20Models/Index.md)** - Quick reference with links  
**[LLM Index](LLM%20Models/LLM_Index.md)** - Complete documentation index  
**[Models List](LLM%20Models/LLM_ModelsList.md)** - Ollama models reference  
**[Model Compatibility](LLM%20Models/LLM_ModelCompatibility.md)** - Compatibility matrix  
**[Memory System](LLM%20Models/LLM_MemorySystem.md)** - Cross-session memory

**Topics:** Models, Providers, Context Windows, Tool Support, Memory  
**Files:** 6 documents

---

### Tools System

**[README](Tools/README.md)** - Overview and navigation  
**[Index](Tools/Index.md)** - Quick reference with links  
**[User Guide](Tools/UserGuide.md)** - Using tools  
**[Architecture](Tools/Architecture.md)** - System architecture  
**[Getting Started](Tools/GettingStarted.md)** - Extension tools  
**[Manifest Reference](Tools/ManifestReference.md)** - Extension manifest schema

**Topics:** Tools, Execution, Approval, Extensions, Configuration  
**Files:** 6 documents

---

### Hooks System

**[README](Hooks/README.md)** - Overview and navigation  
**[Index](Hooks/Index.md)** - Quick reference with links  
**[Architecture](Hooks/Architecture.md)** - System architecture  
**[User Guide](Hooks/UserGuide.md)** - Using hooks  
**[Protocol](Hooks/Protocol.md)** - Hook protocol specification  
**[Visual Guide](Hooks/VisualGuide.md)** - Visual diagrams  
**[Keyboard Shortcuts](Hooks/KeyboardShortcuts.md)** - Hook keybindings

**Topics:** Hooks, Events, Automation, Trust Model, Protocol  
**Files:** 7 documents

---

### MCP Integration

**[README](MCP/README.md)** - Overview and navigation  
**[MCP Index](MCP/MCP_Index.md)** - Complete documentation index  
**[Getting Started](MCP/MCP_GettingStarted.md)** - Quick start guide  
**[Architecture](MCP/MCP_Architecture.md)** - System architecture  
**[Integration](MCP/MCP_Integration.md)** - Integration guide  
**[Commands](MCP/MCP_Commands.md)** - CLI commands  
**[Marketplace](MCP/MCP_Marketplace.md)** - Server marketplace

**Topics:** MCP, Servers, Tools, OAuth, Integration  
**Files:** 7 documents

---

### Prompts System

**[README](Prompts%20System/README.md)** - Overview and navigation  
**[Index](Prompts%20System/Index.md)** - Quick reference with links  
**[Architecture](Prompts%20System/Architecture.md)** - System architecture  
**[System Prompts](Prompts%20System/SystemPrompts.md)** - Prompt construction  
**[Prompt Templates](Prompts%20System/PromptsTemplates.md)** - Template system  
**[Prompt Routing](Prompts%20System/PromptsRouting.md)** - Routing and modes

**Topics:** System Prompts, Templates, Routing, Modes, Goals  
**Files:** 6 documents

---

## System Documentation

### By Component

**User Interface**
- [UI Guide](UI&Settings/UIGuide.md) - Interface layout
- [UI Architecture](UI&Settings/Architecture.md) - Technical design
- [Commands](UI&Settings/Commands.md) - All commands
- [Keybinds](UI&Settings/Keybinds.md) - Keyboard shortcuts

**Context System**
- [Context Management](Context/ContextManagment.md) - Context sizing
- [Context Architecture](Context/ContextArchitecture.md) - System design
- [Context Compression](Context/ContextCompression.md) - Compression
- [Checkpoint Flow](Context/CheckpointFlowDiagram.md) - Checkpoints

**Model System**
- [Model Management](LLM%20Models/README.md) - Model lifecycle
- [Models List](LLM%20Models/LLM_ModelsList.md) - Available models
- [Model Compatibility](LLM%20Models/LLM_ModelCompatibility.md) - Compatibility
- [Memory System](LLM%20Models/LLM_MemorySystem.md) - Memory

**Tool System**
- [Tools User Guide](Tools/UserGuide.md) - Using tools
- [Tools Architecture](Tools/Architecture.md) - System design
- [Extension Tools](Tools/GettingStarted.md) - Extensions
- [Manifest Reference](Tools/ManifestReference.md) - Manifest schema

**Hook System**
- [Hooks User Guide](Hooks/UserGuide.md) - Using hooks
- [Hooks Architecture](Hooks/Architecture.md) - System design
- [Hook Protocol](Hooks/Protocol.md) - Protocol spec
- [Visual Guide](Hooks/VisualGuide.md) - Diagrams

**MCP System**
- [MCP Getting Started](MCP/MCP_GettingStarted.md) - Quick start
- [MCP Architecture](MCP/MCP_Architecture.md) - System design
- [MCP Integration](MCP/MCP_Integration.md) - Integration
- [MCP Commands](MCP/MCP_Commands.md) - Commands

**Prompts System**
- [System Prompts](Prompts%20System/SystemPrompts.md) - Prompt construction
- [Prompts Architecture](Prompts%20System/Architecture.md) - System design
- [Prompt Templates](Prompts%20System/PromptsTemplates.md) - Templates
- [Prompt Routing](Prompts%20System/PromptsRouting.md) - Routing

---

## Architecture Documentation

### System Architectures

**[UI Architecture](UI&Settings/Architecture.md)**
- React + Ink integration
- Component hierarchy
- State management
- Theme system
- Terminal integration
- Rendering optimization

**[Context Architecture](Context/ContextArchitecture.md)**
- VRAM monitoring
- Context sizing
- Compression service
- Checkpoint system
- Component interactions

**[Tools Architecture](Tools/Architecture.md)**
- Tool registry
- Execution engine
- Approval system
- Schema validation
- Security model

**[Hooks Architecture](Hooks/Architecture.md)**
- Hook registry
- Event system
- Execution engine
- Trust model
- Security and sandboxing

**[MCP Architecture](MCP/MCP_Architecture.md)**
- MCP client
- Transport layer
- Schema conversion
- Tool integration
- OAuth system

**[Prompts Architecture](Prompts%20System/Architecture.md)**
- Prompt orchestrator
- Template system
- Goal management
- Mode system
- Prompt construction

---

## Development Documentation

### Development Roadmap

**[README](DevelopmentRoadmap/README.md)** - Overview  
**[Index](DevelopmentRoadmap/Index.md)** - Quick reference  
**[Roadmap](DevelopmentRoadmap/Roadmap.md)** - Detailed roadmap  
**[Roadmap Visual](DevelopmentRoadmap/RoadmapVisual.md)** - Visual timeline  
**[Planned Features](DevelopmentRoadmap/PlanedFeatures.md)** - Feature reference  
**[Releases](DevelopmentRoadmap/OLLM-CLI_Releases.md)** - Release notes

**Topics:** Versions, Features, Timeline, Releases  
**Files:** 6 documents

---

## Reference Documentation

### Command References

**[Commands Reference](UI&Settings/Commands.md)**
- Session management
- Context and model commands
- MCP and extension commands
- Git and workflow commands
- Configuration commands

**[MCP Commands](MCP/MCP_Commands.md)**
- MCP server commands
- Health commands
- OAuth commands
- Hook commands
- Extension commands

### Configuration References

**[Configuration Guide](UI&Settings/Configuration.md)**
- Layout settings
- Display preferences
- Theme configuration
- Performance tuning

**[Manifest Reference](Tools/ManifestReference.md)**
- Extension manifest schema
- Required fields
- Optional fields
- MCP server configuration

### Protocol References

**[Hook Protocol](Hooks/Protocol.md)**
- Hook file format
- Event types and payloads
- Action types
- Input/output format
- Protocol versioning

---

## Documentation by Audience

### For New Users

**Start Here:**
1. [Introduction](Introduction.md) - What is OLLM CLI?
2. [Installation](Installation.md) - Install on your computer
3. [Quick Start](Quickstart.md) - Get started in 5 minutes

**Then Explore:**
4. [UI Guide](UI&Settings/UIGuide.md) - Learn the interface
5. [Commands](UI&Settings/Commands.md) - Learn commands
6. [Tools User Guide](Tools/UserGuide.md) - Use tools

### For Regular Users

**Configuration:**
1. [Configuration Guide](UI&Settings/Configuration.md)
2. [Themes Guide](UI&Settings/Themes.md)
3. [Model Management](LLM%20Models/README.md)

**Advanced Features:**
4. [Context Management](Context/ContextManagment.md)
5. [MCP Getting Started](MCP/MCP_GettingStarted.md)
6. [Hooks User Guide](Hooks/UserGuide.md)

### For Developers

**Architecture:**
1. [UI Architecture](UI&Settings/Architecture.md)
2. [Context Architecture](Context/ContextArchitecture.md)
3. [Tools Architecture](Tools/Architecture.md)
4. [Hooks Architecture](Hooks/Architecture.md)
5. [MCP Architecture](MCP/MCP_Architecture.md)
6. [Prompts Architecture](Prompts%20System/Architecture.md)

**Development:**
7. [Development Roadmap](DevelopmentRoadmap/Roadmap.md)
8. [Extension Development](Tools/GettingStarted.md)
9. [Hook Development](Hooks/UserGuide.md)

---

## Documentation by Topic

### Installation & Setup
- [Installation Guide](Installation.md)
- [Quick Start](Quickstart.md)
- [Configuration](UI&Settings/Configuration.md)
- [Troubleshooting](Troubleshooting.md)

### User Interface
- [UI Guide](UI&Settings/UIGuide.md)
- [Commands Reference](UI&Settings/Commands.md)
- [Keyboard Shortcuts](UI&Settings/Keybinds.md)
- [Themes](UI&Settings/Themes.md)
- [Terminal](UI&Settings/Terminal.md)

### Context & Memory
- [Context Management](Context/ContextManagment.md)
- [Context Compression](Context/ContextCompression.md)
- [Checkpoint System](Context/CheckpointFlowDiagram.md)
- [Memory System](LLM%20Models/LLM_MemorySystem.md)

### Models & Providers
- [Model Management](LLM%20Models/README.md)
- [Models List](LLM%20Models/LLM_ModelsList.md)
- [Model Compatibility](LLM%20Models/LLM_ModelCompatibility.md)

### Tools & Automation
- [Tools System](Tools/README.md)
- [Hooks System](Hooks/README.md)
- [MCP Integration](MCP/README.md)

### Prompts & Modes
- [System Prompts](Prompts%20System/SystemPrompts.md)
- [Prompt Templates](Prompts%20System/PromptsTemplates.md)
- [Prompt Routing](Prompts%20System/PromptsRouting.md)

---

## Documentation Statistics

### By Section

| Section | Files | Status |
|---------|-------|--------|
| Getting Started | 4 | ✅ Complete |
| UI & Settings | 9 | ✅ Complete |
| Context Management | 6 | ✅ Complete |
| Model Management | 6 | ✅ Complete |
| Tools System | 6 | ✅ Complete |
| Hooks System | 7 | ✅ Complete |
| MCP Integration | 7 | ✅ Complete |
| Prompts System | 6 | ✅ Complete |
| Development Roadmap | 6 | ✅ Complete |
| **Total** | **57** | **100%** |

### By Type

| Type | Count |
|------|-------|
| Overview (README) | 9 |
| Index | 9 |
| User Guides | 12 |
| Architecture | 6 |
| Reference | 8 |
| Getting Started | 4 |
| Other | 9 |
| **Total** | **57** |

---

## External Resources

### Official Resources
- **GitHub:** [github.com/tecet/ollm](https://github.com/tecet/ollm)
- **Issues:** [github.com/tecet/ollm/issues](https://github.com/tecet/ollm/issues)
- **Discussions:** [github.com/tecet/ollm/discussions](https://github.com/tecet/ollm/discussions)

### Related Projects
- **Ollama:** [ollama.ai](https://ollama.ai)
- **Model Context Protocol:** [modelcontextprotocol.io](https://modelcontextprotocol.io)
- **Ink:** [github.com/vadimdemedes/ink](https://github.com/vadimdemedes/ink)

---

## Search Tips

### Finding Information

**By Feature:**
- Search for feature name in this index
- Check the relevant section README
- Look in the Index.md for that section

**By Task:**
- "How do I..." → Check User Guides
- "What is..." → Check Overview/README files
- "How does it work..." → Check Architecture files

**By Problem:**
- Check [Troubleshooting](Troubleshooting.md) first
- Search for error message
- Check relevant system documentation

---

**Last Updated:** January 26, 2026  
**Version:** 0.1.0  
**Total Documentation:** 57 files

For the latest documentation, visit [github.com/tecet/ollm](https://github.com/tecet/ollm)
