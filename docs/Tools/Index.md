# Tool System Documentation Index

**Quick Reference with Links**

This index provides quick navigation to all Tool System documentation with brief descriptions.

---

## 📚 Core Documentation

### [README.md](README.md)

**Overview and Navigation Guide**

Main entry point for Tool System documentation. Provides complete overview of the tool execution system, available tools, approval modes, and integration points.

**Topics:** Overview, Tools, Approval Modes, Integration  
**Audience:** All users

---

### [Architecture](Architecture.md)

**System Architecture and Design**

Complete technical documentation of the Tool System architecture. Covers tool registry, execution engine, approval system, schema validation, and all component interactions.

**Topics:** Architecture, Components, Tool Registry, Execution, Security  
**Audience:** Developers, architects

**Key Sections:**

- System overview and architecture
- Tool registry and discovery
- Tool execution engine
- Approval and policy system
- Schema validation
- Result formatting
- Error handling
- Security model

---

### [User Guide](UserGuide.md)

**Using Tools in OLLM CLI**

Comprehensive guide to using tools in OLLM CLI. Covers tool discovery, execution, approval modes, and troubleshooting.

**Topics:** Usage, Tools, Approval, Examples, Troubleshooting  
**Audience:** All users

**Key Sections:**

- What are tools
- Available tools overview
- Tool discovery and listing
- Tool execution
- Approval modes (YOLO, AUTO, ASK)
- Tool parameters
- Common patterns
- Troubleshooting

---

### [Getting Started](GettingStarted.md)

**Extension Tools**

Guide to extension tools and the extension system. Covers how extensions provide additional tools through MCP servers.

**Topics:** Extensions, MCP Tools, Installation  
**Audience:** All users

**Key Sections:**

- What are extension tools
- Finding extensions
- Installing extensions
- Using extension tools
- Extension management

---

### [Manifest Reference](ManifestReference.md)

**Extension Manifest Schema**

Technical reference for extension manifest files. Defines the schema for extension configuration, including MCP server definitions and permissions.

**Topics:** Manifest, Schema, Configuration  
**Audience:** Developers

**Key Sections:**

- Manifest file format
- Required fields
- Optional fields
- MCP server configuration
- Permissions system
- Examples

---

## 📖 Documentation by Topic

### Tool Usage

- [User Guide](UserGuide.md) - Complete usage guide
- [README](README.md#available-tools) - Tool reference
- [Architecture](Architecture.md#tool-execution) - Execution details

### Approval System

- [User Guide](UserGuide.md#approval-modes) - Approval modes
- [README](README.md#approval-modes) - Quick reference
- [Architecture](Architecture.md#approval-system) - Implementation

### Extension Tools

- [Getting Started](GettingStarted.md) - Extension tools
- [Manifest Reference](ManifestReference.md) - Manifest schema
- [Architecture](Architecture.md#mcp-integration) - MCP integration

### Configuration

- [README](README.md#configuration) - Configuration overview
- [User Guide](UserGuide.md#configuration) - Detailed settings

---

## 📖 Documentation by Audience

### For New Users

1. [README](README.md) - Start here
2. [User Guide](UserGuide.md) - Using tools
3. [README](README.md#approval-modes) - Approval modes

### For Regular Users

1. [User Guide](UserGuide.md#advanced-usage) - Advanced usage
2. [Getting Started](GettingStarted.md) - Extension tools
3. [README](README.md#best-practices) - Best practices

### For Developers

1. [Architecture](Architecture.md) - System design
2. [Manifest Reference](ManifestReference.md) - Extension development
3. [User Guide](UserGuide.md#troubleshooting) - Debugging

---

## 🔗 Related Documentation

### Core Systems

- [MCP Integration](../MCP/MCP_Index.md) - MCP servers and tools
- [Hooks System](../Hooks/README.md) - Hook integration
- [User Interface](../UI&Settings/README.md) - UI documentation

### Commands

- [Tool Commands](../UI&Settings/Commands.md#tool-management) - CLI commands

### Developer Resources

- Knowledge DB: `dev_ToolExecution.md` - Architecture details

---

## 📊 Documentation Status

### Completed ✅

| Document             | Status      |
| -------------------- | ----------- |
| README.md            | ✅ Complete |
| Index.md             | ✅ Complete |
| Architecture.md      | ✅ Complete |
| UserGuide.md         | ✅ Complete |
| GettingStarted.md    | ✅ Complete |
| ManifestReference.md | ✅ Complete |

**Overall Progress:** 100% complete (6/6 files)

---

## 🎯 Quick Links

### Common Tasks

- List tools → [User Guide](UserGuide.md#listing-tools)
- Set approval mode → [User Guide](UserGuide.md#setting-approval-mode)
- Use tools → [User Guide](UserGuide.md#using-tools)
- Install extensions → [Getting Started](GettingStarted.md)

### Understanding Systems

- How tools work → [README](README.md)
- Tool categories → [README](README.md#tool-categories)
- Approval system → [Architecture](Architecture.md#approval-system)
- System architecture → [Architecture](Architecture.md)

### Tool Reference

- File tools → [README](README.md#file-operations)
- Web tools → [README](README.md#web-tools)
- Shell tool → [README](README.md#shell-tool)
- Memory tools → [README](README.md#memory-tools)
- Goal tools → [README](README.md#goal-tools)

---

**Last Updated:** January 26, 2026  
**Version:** 0.1.0
