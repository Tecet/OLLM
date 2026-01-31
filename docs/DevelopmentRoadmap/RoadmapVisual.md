# OLLM CLI - Visual Roadmap

**Last Updated:** January 26, 2026  
**Current Version:** v0.1.6 (Alpha)

> **Note:** This is a visual representation of the OLLM CLI development roadmap. All versions marked as "Planned" are future development and not yet implemented.

## Development Timeline

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         OLLM CLI Development Roadmap                     │
│                    Pre-Alpha → Alpha → Beta                              │
└─────────────────────────────────────────────────────────────────────────┘

PRE-ALPHA: STAGES 1-9 (COMPLETED)
═══════════════════════════════════════════════════════════════════════════

Stages 1-9: Foundation Development                         [████████████] 100%
├─ Stage 1-2: Foundation & Core Provider                   ✅ Complete
├─ Stage 3: Tools & Policy Engine                          ✅ Complete
├─ Stage 4: Services & Sessions                            ✅ Complete
├─ Stage 5: Hooks, Extensions & MCP                        ✅ Complete
├─ Stage 6: CLI & UI                                       ✅ Complete
├─ Stage 7: Model Management                               🔄 In Progress
├─ Stage 8: Testing & QA                                   🔄 In Progress
└─ Stage 9: Documentation & Release                        ✅ Complete

Result: v0.1.6 Alpha Release                               ✅ Released


ALPHA RELEASES (v0.1.6 - v0.9.0)
═══════════════════════════════════════════════════════════════════════════

v0.1.6: Foundation Release                                 [████████████] 100%
├─ Interactive TUI (React + Ink)                           ✅ Complete
├─ Non-Interactive Mode                                    ✅ Complete
├─ Tool System (File, Shell, Web)                          ✅ Complete
├─ Policy Engine (ASK/AUTO/YOLO)                           ✅ Complete
├─ Context Management + VRAM Monitoring                    ✅ Complete
├─ Session Recording & Compression                         ✅ Complete
├─ Hook System                                             🔄 In Progress
├─ MCP Integration                                         🔄 In Progress
├─ Model Management                                        🔄 In Progress
└─ Testing Infrastructure                                  🔄 In Progress

v0.2.0: File Explorer & Enhanced MCP                       [            ] 0%
Priority: HIGH
├─ Tree-based File Browser                                 ✅ Complete
├─ File Operations (open, edit, rename, delete)            🔄 In Progress
├─ Git Status Indicators                                   📋 Planned
├─ Quick File Search                                       📋 Planned
├─ MCP OAuth Authentication                                🔄 In Progress
├─ MCP Health Monitoring                                   🔄 In Progress
└─ MCP Marketplace Integration                             🔄 In Progress

v0.3.0: Advanced File Explorer & MCP Polish                [            ] 0%
Priority: HIGH
├─ Multi-file Selection                                    📋 Planned
├─ File Preview Panel                                      🔄 In Progress
├─ Advanced Search & Filtering                             📋 Planned
├─ Bookmarks & Favorites                                   📋 Planned
├─ MCP Error Handling Improvements                         📋 Planned
└─ MCP Performance Optimizations                           📋 Planned

v0.4.0: Code Editor                                        [            ] 0%
Priority: HIGH
├─ Terminal-based Code Editor                              📋 Planned
├─ Syntax Highlighting (50+ languages)                     📋 Planned
├─ Undo/Redo Support                                       📋 Planned
├─ Copy/Cut/Paste Operations                               📋 Planned
├─ Find & Go-to-Line                                       📋 Planned
├─ Prettier Formatting                                     📋 Planned
└─ Multiple File Tabs                                      📋 Planned

v0.5.0: Release Kraken                                     [            ] 0%
Priority: HIGH
├─ CLI Bridge (Gemini, Claude, Codex)                      📋 Planned
├─ API Providers (OpenAI, Anthropic, Google)               📋 Planned
├─ Provider Discovery & Health Checks                      📋 Planned
├─ Context Transfer & Session Management                   📋 Planned
├─ Cost Tracking & Budget Enforcement                      📋 Planned
└─ Auto-Escalation on Local Model Failure                  📋 Planned

v0.6.0: RAG Integration                                    [            ] 0%
Priority: HIGH
├─ Codebase Indexing with Embeddings                       📋 Planned
├─ Semantic Code Search                                    📋 Planned
├─ Context-aware File Discovery                            📋 Planned
├─ Symbol & Definition Search                              📋 Planned
├─ Vector Database Integration                             📋 Planned
└─ Incremental Indexing                                    📋 Planned

v0.7.0: GitHub Integration                                 [            ] 0%
Priority: MEDIUM
├─ GitHub API Integration                                  📋 Planned
├─ Repository Management                                   📋 Planned
├─ Issue & PR Creation                                     📋 Planned
├─ Code Review Assistance                                  📋 Planned
├─ Branch Management                                       📋 Planned
└─ GitHub Actions Integration                              📋 Planned

v0.8.0: Cross-Platform Support                             [            ] 0%
Priority: MEDIUM
├─ Platform Detection & Defaults                           📋 Planned
├─ Configuration Path Resolution (XDG, AppData)            📋 Planned
├─ Terminal Capability Detection                           📋 Planned
├─ Cross-Platform GPU Monitoring                           📋 Planned
└─ Path Normalization                                      📋 Planned

v0.9.0: vLLM & Open Source Providers                       [            ] 0%
Priority: MEDIUM
├─ vLLM Provider Integration                               📋 Planned
├─ LM Studio Support                                       📋 Planned
├─ Text Generation WebUI Support                           📋 Planned
├─ LocalAI Support                                         📋 Planned
└─ High-Performance Streaming                              📋 Planned


BETA RELEASE (v1.0.0+)
═══════════════════════════════════════════════════════════════════════════

v1.0.0: Beta Release                                       [            ] 0%
Priority: CRITICAL
├─ Production-Grade Stability                              🎯 Future
├─ API Stability Guarantees                                🎯 Future
├─ Complete Documentation                                  🎯 Future
├─ Performance Optimization                                🎯 Future
├─ Security Hardening                                      🎯 Future
└─ Community Feedback Integration                          🎯 Future
```

## Release Dependencies

```
┌──────────────────────────────────────────────────────────────────────┐
│                         Release Dependency Graph                      │
└──────────────────────────────────────────────────────────────────────┘

                    ┌─────────────────────┐
                    │   Pre-Alpha         │
                    │   Stages 1-9        │
                    │   → v0.1.6          │
                    └──────────┬──────────┘
                               │
                ┌──────────────┼──────────────┐
                │              │              │
                ▼              ▼              ▼
        ┌───────────┐  ┌───────────┐  ┌───────────┐
        │  v0.2.0   │  │  v0.4.0   │  │  v0.6.0   │
        │  File     │  │  Code     │  │  RAG      │
        │  Explorer │  │  Editor   │  │  Search   │
        └─────┬─────┘  └─────┬─────┘  └─────┬─────┘
              │              │              │
              ▼              │              │
        ┌───────────┐        │              │
        │  v0.3.0   │        │              │
        │  Advanced │        │              │
        │  Explorer │        │              │
        └─────┬─────┘        │              │
              │              │              │
              └──────┬───────┴──────┬───────┘
                     │              │
                     ▼              ▼
             ┌───────────┐  ┌───────────┐
             │  v0.5.0   │  │  v0.7.0   │
             │  Kraken   │  │  GitHub   │
             │  (LLMs)   │  │  Integr.  │
             └─────┬─────┘  └─────┬─────┘
                   │              │
                   └──────┬───────┘
                          │
                          ▼
                  ┌───────────┐
                  │  v0.8.0   │
                  │  Cross-   │
                  │  Platform │
                  └─────┬─────┘
                        │
                        ▼
                  ┌───────────┐
                  │  v0.9.0   │
                  │  vLLM &   │
                  │  Providers│
                  └─────┬─────┘
                        │
                        ▼
                  ┌───────────┐
                  │  v1.0.0   │
                  │  Beta     │
                  │  Release  │
                  └───────────┘
```

## Priority Matrix

```
┌──────────────────────────────────────────────────────────────────────┐
│                    Priority vs Complexity Matrix                      │
└──────────────────────────────────────────────────────────────────────┘

High Priority
    │
    │   v0.2.0            v0.4.0            v0.5.0
    │   File Explorer     Code Editor       Kraken
    │   [Medium]          [Medium]          [High]
    │
    │   v0.6.0            v0.7.0
    │   RAG Search        GitHub
    │   [High]            [Medium]
    │
    │   v0.8.0            v0.9.0            v1.0.0
    │   Cross-Platform    vLLM Providers    Beta
    │   [Low]             [Medium]          [High]
    │
Low Priority
    └────────────────────────────────────────────────────────────────▶
        Low Complexity                              High Complexity
```

## Release Timeline

```
┌──────────────────────────────────────────────────────────────────────┐
│                      Estimated Release Timeline                       │
│                    (Subject to change based on                        │
│                   community feedback and resources)                   │
└──────────────────────────────────────────────────────────────────────┘

Q1 2026  │ ████████████ v0.1.6: Foundation (Released)
         │
Q2 2026  │ █████████░░░ v0.2.0: File Explorer & MCP (Planned)
         │ ░░░░░░░░░░░░ v0.3.0: Advanced Explorer (Planned)
         │
Q3 2026  │ ░░░░░░░░░░░░ v0.4.0: Code Editor (Planned)
         │ ░░░░░░░░░░░░ v0.5.0: Release Kraken (Planned)
         │
Q3 2026  │ ░░░░░░░░░░░░ v0.6.0: RAG Integration (Planned)
         │ ░░░░░░░░░░░░ v0.7.0: GitHub Integration (Planned)
         │
Q4 2026  │ ░░░░░░░░░░░░ v0.8.0: Cross-Platform (Planned)
         │ ░░░░░░░░░░░░ v0.9.0: vLLM Providers (Planned)
         │
Q4 2026  │ ░░░░░░░░░░░░ v1.0.0: Beta Release (Future)
         │

Legend:
████ Released
░░░░ Planned (Timeline TBD)
```

## Feature Categories

```
┌──────────────────────────────────────────────────────────────────────┐
│                        Features by Category                           │
└──────────────────────────────────────────────────────────────────────┘

🎨 USER INTERFACE
├─ Interactive TUI                                         ✅ v0.1.6
├─ Non-Interactive Mode                                    ✅ v0.1.6
├─ Status Bar & Indicators                                 ✅ v0.1.6
├─ File Explorer                                           📋 v0.2.0
├─ Advanced File Explorer                                  📋 v0.3.0
└─ Code Editor                                             📋 v0.4.0

🔧 CORE FUNCTIONALITY
├─ Tool System                                             ✅ v0.1.6
├─ Policy Engine                                           ✅ v0.1.6
├─ Hook System                                             ✅ v0.1.6
├─ MCP Integration                                         ✅ v0.1.6
├─ Enhanced MCP                                            📋 v0.2.0
└─ GitHub Integration                                      📋 v0.7.0

🧠 INTELLIGENCE
├─ Context Management                                      ✅ v0.1.6
├─ VRAM Monitoring                                         ✅ v0.1.6
├─ Session Compression                                     ✅ v0.1.6
├─ Semantic Search (RAG)                                   📋 v0.6.0
├─ Codebase Indexing                                       📋 v0.6.0
└─ Vector Database                                         📋 v0.6.0

🔌 PROVIDERS
├─ Ollama (Local)                                          ✅ v0.1.6
├─ External LLMs (Kraken)                                  📋 v0.5.0
│  ├─ CLI Bridge (Gemini, Claude, Codex)                   📋 v0.5.0
│  └─ API Providers (OpenAI, Anthropic, Google)            📋 v0.5.0
├─ vLLM                                                    📋 v0.9.0
├─ LM Studio                                               📋 v0.9.0
└─ Text Generation WebUI                                   📋 v0.9.0

📁 FILE HANDLING
├─ File Read/Write Tools                                   ✅ v0.1.6
├─ File Explorer                                           📋 v0.2.0
├─ Multi-file Selection                                    📋 v0.3.0
└─ File Preview                                            📋 v0.3.0

🖥️ PLATFORM
├─ Basic Cross-Platform                                    ✅ v0.1.6
└─ Enhanced Cross-Platform                                 📋 v0.8.0
   ├─ Platform Detection                                   📋 v0.8.0
   ├─ XDG/AppData Path Resolution                          📋 v0.8.0
   ├─ Terminal Capability Detection                        📋 v0.8.0
   └─ Cross-Platform GPU Monitoring                        📋 v0.8.0

💰 COST & TRACKING
├─ Basic Token Counting                                    ✅ v0.1.6
├─ Cost Tracking                                           📋 v0.5.0
└─ Budget Enforcement                                      📋 v0.5.0

🔍 SEARCH & DISCOVERY
├─ File Search                                             📋 v0.2.0
├─ Advanced Filtering                                      📋 v0.3.0
├─ Semantic Code Search                                    📋 v0.6.0
└─ Symbol Search                                           📋 v0.6.0

✏️ EDITING
├─ Basic File Operations                                   ✅ v0.1.6
├─ Code Editor                                             📋 v0.4.0
├─ Syntax Highlighting                                     📋 v0.4.0
└─ Prettier Formatting                                     📋 v0.4.0
```

## Implementation Status Legend

```
✅ Released       - Feature is implemented, tested, and released
🔄 In Progress    - Feature is currently being developed
📋 Planned        - Feature is planned for future release
🎯 Future         - Feature planned for beta or later
⏸️ On Hold        - Feature development is paused
❌ Cancelled      - Feature will not be implemented
```

## Version Status

```
┌──────────────────────────────────────────────────────────────────────┐
│                           Version Status                              │
└──────────────────────────────────────────────────────────────────────┘

Pre-Alpha (Stages 1-9)
├─ Stage 1-9: Foundation Development                       ✅ Complete

Alpha Releases
├─ v0.1.6: Foundation Release                              ✅ Released
├─ v0.2.0: File Explorer & Enhanced MCP                    📋 Planned
├─ v0.3.0: Advanced File Explorer & MCP Polish             📋 Planned
├─ v0.4.0: Code Editor                                     📋 Planned
├─ v0.5.0: Release Kraken                                  📋 Planned
├─ v0.6.0: RAG Integration                                 📋 Planned
├─ v0.7.0: GitHub Integration                              📋 Planned
├─ v0.8.0: Cross-Platform Support                          📋 Planned
└─ v0.9.0: vLLM & Open Source Providers                    📋 Planned

Beta Release
└─ v1.0.0: Beta Release                                    🎯 Future
```

## Quick Links

- **[Roadmap Overview](Roadmap.md)** - Main roadmap document
- **[Planned Features](PlanedFeatures.md)** - Detailed feature plans
- **[Contributing](future-development.md#contributing)** - How to contribute
- **[Feedback](future-development.md#feedback)** - Share your thoughts

---

**Last Updated:** January 26, 2026  
**Current Version:** v0.1.6 (Alpha)  
**Document Version:** 2.0  
**Status:** Living document - Updated as development progresses

<!-- Alpha release v0.1.6 -->

