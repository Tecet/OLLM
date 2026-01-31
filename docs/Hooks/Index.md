# Hook System Documentation Index

**Quick Reference with Links**

This index provides quick navigation to all Hook System documentation with brief descriptions.

---

## 📚 Core Documentation

### [README.md](README.md)

**Overview and Navigation Guide**

Main entry point for Hook System documentation. Provides overview of event-driven automation, trust model, common use cases, and quick start guide.

**Topics:** Overview, Events, Trust Model, Use Cases  
**Audience:** All users

---

### [Architecture](Architecture.md)

**System Architecture and Design**

Complete technical documentation of the Hook System architecture. Covers hook registry, event system, execution engine, trust model, and all component interactions.

**Topics:** Architecture, Components, Event System, Trust Model, Data Flow  
**Audience:** Developers, architects

**Key Sections:**

- System overview and architecture
- Hook registry and management
- Event system and dispatching
- Hook execution engine
- Trust model implementation
- Security and sandboxing
- Component interactions

---

### [User Guide](UserGuide.md)

**Using Hooks for Automation**

Comprehensive guide to using hooks in OLLM CLI. Covers listing, enabling, creating, and managing hooks for workflow automation.

**Topics:** Usage, Commands, Examples, Best Practices  
**Audience:** All users

**Key Sections:**

- What are hooks
- Listing and managing hooks
- Enabling and disabling hooks
- Creating custom hooks
- Trust management
- Hook debugging
- Common patterns
- Troubleshooting

---

### [Protocol](Protocol.md)

**Hook Protocol Specification**

Technical specification for the hook protocol. Defines JSON schema for hook input/output, event types, action types, and communication format.

**Topics:** Protocol, JSON Schema, Events, Actions  
**Audience:** Developers

**Key Sections:**

- Hook file format (JSON schema)
- Event types and payloads
- Action types (askAgent, runCommand)
- Input/output format
- Error handling
- Protocol versioning
- Examples

---

### [Visual Guide](VisualGuide.md)

**Visual Diagrams and Examples**

Visual guide with diagrams showing hook workflows, event flows, and execution patterns. Includes mermaid diagrams and ASCII art.

**Topics:** Diagrams, Workflows, Visual Examples  
**Audience:** All users

**Key Sections:**

- Hook lifecycle diagram
- Event flow diagrams
- Trust model visualization
- Execution flow charts
- Common patterns illustrated
- Example workflows

---

### [Keyboard Shortcuts](KeyboardShortcuts.md)

**Hook-Related Keybindings**

Reference for keyboard shortcuts related to hook management and execution.

**Topics:** Keybindings, Shortcuts  
**Audience:** All users

**Key Sections:**

- Hook management shortcuts
- Hook execution shortcuts
- Hook debugging shortcuts
- Quick access keys

---

## 📖 Documentation by Topic

### Hook Events

- [User Guide](UserGuide.md#hook-events) - Event types overview
- [Protocol](Protocol.md#event-types) - Event specifications
- [Visual Guide](VisualGuide.md#event-flows) - Event flow diagrams

### Hook Actions

- [User Guide](UserGuide.md#hook-actions) - Action types overview
- [Protocol](Protocol.md#action-types) - Action specifications
- [Architecture](Architecture.md#execution-engine) - Action execution

### Trust Model

- [README](README.md#trust-model) - Trust overview
- [Architecture](Architecture.md#trust-model) - Trust implementation
- [User Guide](UserGuide.md#trust-management) - Managing trust

### Creating Hooks

- [User Guide](UserGuide.md#creating-hooks) - Hook creation guide
- [Protocol](Protocol.md#hook-format) - Hook file format
- [Visual Guide](VisualGuide.md#examples) - Example hooks

### Configuration

- [README](README.md#configuration) - Configuration overview
- [User Guide](UserGuide.md#configuration) - Detailed settings
- [Architecture](Architecture.md#configuration) - Configuration system

---

## 📖 Documentation by Audience

### For New Users

1. [README](README.md) - Start here
2. [User Guide](UserGuide.md) - Using hooks
3. [Visual Guide](VisualGuide.md) - Visual examples

### For Regular Users

1. [User Guide](UserGuide.md#creating-hooks) - Creating hooks
2. [Protocol](Protocol.md) - Hook format
3. [README](README.md#common-use-cases) - Use case examples

### For Developers

1. [Architecture](Architecture.md) - System design
2. [Protocol](Protocol.md) - Protocol specification
3. [User Guide](UserGuide.md#advanced-topics) - Advanced usage

---

## 🔗 Related Documentation

### Core Systems

- [Tools System](../Tools/README.md) - Tool execution
- [MCP Integration](../MCP/MCP_Index.md) - MCP servers
- [User Interface](../UI&Settings/README.md) - UI documentation

### Commands

- [Hook Commands](../UI&Settings/Commands.md#hook-management) - CLI commands

### Developer Resources

- Knowledge DB: `dev_HookSystem.md` - Architecture details

---

## 📊 Documentation Status

### Completed ✅

| Document             | Status      |
| -------------------- | ----------- |
| README.md            | ✅ Complete |
| Index.md             | ✅ Complete |
| Architecture.md      | ✅ Complete |
| UserGuide.md         | ✅ Complete |
| Protocol.md          | ✅ Complete |
| VisualGuide.md       | ✅ Complete |
| KeyboardShortcuts.md | ✅ Complete |

**Overall Progress:** 100% complete (7/7 files)

---

## 🎯 Quick Links

### Common Tasks

- List hooks → [User Guide](UserGuide.md#listing-hooks)
- Enable hook → [User Guide](UserGuide.md#enabling-hooks)
- Create hook → [User Guide](UserGuide.md#creating-hooks)
- Trust hook → [User Guide](UserGuide.md#trust-management)
- Debug hook → [User Guide](UserGuide.md#debugging-hooks)

### Understanding Systems

- How hooks work → [README](README.md)
- Hook events → [Protocol](Protocol.md#event-types)
- Hook actions → [Protocol](Protocol.md#action-types)
- Trust model → [Architecture](Architecture.md#trust-model)
- System architecture → [Architecture](Architecture.md)

### Examples

- Auto-format on save → [README](README.md#auto-format-on-save)
- Run tests → [README](README.md#run-tests-before-commit)
- Safety gates → [README](README.md#block-dangerous-commands)
- More examples → [User Guide](UserGuide.md#examples)

---

**Last Updated:** January 26, 2026  
**Version:** 0.1.6

<!-- Alpha release v0.1.6 -->

