# Progressive Checkpoint Compression - Flow Diagram

**Last Updated:** January 26, 2026  
**Status:** Source of Truth

**Related Documents:**
- `ContextCompression.md` - Detailed compression system documentation
- `ContextManagement.md` - Context sizing and management
- `ContextArchitecture.md` - Overall system architecture

---

## System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    Context Manager                               │
│                                                                  │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐               │
│  │  System    │  │ Checkpoints│  │   Recent   │               │
│  │  Prompt    │  │  (History) │  │  Messages  │               │
│  └────────────┘  └────────────┘  └────────────┘               │
│                                                                  │
│  Token Count: ████████░░░░░░░░░░ 62% (8,500 / 13,600)         │
│  Tier: 3 (Standard - 16K) | Mode: Developer                    │
│  Context: FIXED for session | Prompt: 1,000 tokens             │
└─────────────────────────────────────────────────────────────────┘
```

**Key Points:**
- Context size is FIXED for the entire session
- System prompt never compressed
- User messages never compressed
- Only assistant messages compressed into checkpoints

---

## Tier-Specific Compression

```
┌─────────────────────────────────────────────────────────────────┐
│ Tier Detection: Based on Context Size (FIXED at startup)        │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
                    ┌──────────────┐
                    │ Context Size?│
                    └──────────────┘
                            │
        ┌───────────────────┼───────────────────┼───────────────┬───────────────┐
        │                   │                   │               │               │
        ▼                   ▼                   ▼               ▼               ▼
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ Tier 1       │    │ Tier 2       │    │ Tier 3 ⭐    │    │ Tier 4       │    │ Tier 5       │
│ Minimal      │    │ Basic        │    │ Standard     │    │ Premium      │    │ Ultra        │
│ 2K, 4K       │    │ 8K           │    │ 16K          │    │ 32K          │    │ 64K, 128K    │
│              │    │              │    │              │    │              │    │              │
│ Rollover     │    │ Smart        │    │ Progressive  │    │ Structured   │    │ Rich         │
│ 0 checkpts   │    │ 1 checkpt    │    │ 3 checkpts   │    │ 10 checkpts  │    │ 15 checkpts  │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
```

**Tier Characteristics:**

| Tier | Context | Ollama (85%) | Checkpoints | Strategy |
|------|---------|--------------|-------------|----------|
| 1 | 2K, 4K | 1700, 3400 | 0 | Snapshot rollover |
| 2 | 8K | 6800 | 1 | Single checkpoint |
| 3 ⭐ | 16K | 13600 | 3 | Progressive aging |
| 4 | 32K | 27200 | 10 | Structured preservation |
| 5 | 64K, 128K | 54400, 108800 | 15 | Rich metadata |

---

## Compression Flow

```
┌─────────────────────────────────────────────────────────────────┐
│ Step 1: Monitor Context Usage                                   │
│                                                                  │
│  Available Budget = ollama_context_size - system - checkpoints  │
│  Trigger at: 80% of available budget                            │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ Step 2: Context Reaches Threshold (80% of available)            │
│                                                                  │
│  Example (16K context):                                          │
│  - Ollama limit: 13,600 tokens                                  │
│  - System prompt: 1,000 tokens                                  │
│  - Checkpoints: 2,100 tokens                                    │
│  - Available: 10,500 tokens                                     │
│  - Trigger at: 8,400 tokens (80% of 10,500)                    │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ Step 3: Identify Messages to Compress                           │
│                                                                  │
│  ✅ Compress: Assistant messages (LLM output)                   │
│  ❌ Never compress: User messages                               │
│  ❌ Never compress: System prompt                               │
│  ❌ Never compress: Active goals                                │
│  ❌ Never compress: Locked decisions                            │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ Step 4: LLM Summarization                                       │
│                                                                  │
│  Send to LLM:                                                    │
│  "Summarize the following conversation history,                 │
│   preserving key decisions, code changes, and context..."       │
│                                                                  │
│  LLM returns summary (50-70% compression)                       │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ Step 5: Create New Checkpoint                                   │
│                                                                  │
│  Checkpoint {                                                    │
│    id: "cp-3"                                                   │
│    level: 3 (DETAILED)                                          │
│    range: "Messages 51-100"                                     │
│    summary: "Implemented JWT authentication..."                 │
│    keyDecisions: ["Use JWT", "httpOnly cookies"]               │
│    filesModified: ["auth/login.ts", "auth/jwt.ts"]             │
│    originalTokens: 5000                                         │
│    currentTokens: 1200                                          │
│    compressionCount: 1                                          │
│  }                                                               │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ Step 6: Age Existing Checkpoints                                │
│                                                                  │
│  Checkpoint 3 (new)    → Level 3 (DETAILED)   ~1200 tokens     │
│  Checkpoint 2 (recent) → Level 2 (MODERATE)   ~600 tokens      │
│  Checkpoint 1 (old)    → Level 1 (COMPACT)    ~300 tokens      │
│                                                                  │
│  Each aging step compresses further:                            │
│  - Recent: 50-70% compression                                   │
│  - Old: 60% compression                                         │
│  - Ancient: 70% compression                                     │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ Step 7: Recalculate Available Budget                            │
│                                                                  │
│  New available budget:                                           │
│  13,600 - 1,000 (system) - 2,100 (checkpoints) = 10,500        │
│                                                                  │
│  New trigger threshold: 10,500 * 0.80 = 8,400 tokens           │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ Step 8: Continue Conversation                                   │
│                                                                  │
│  Context now has more available space for new messages          │
└─────────────────────────────────────────────────────────────────┘
```

---

## Dynamic Budget Calculation

```
┌─────────────────────────────────────────────────────────────────┐
│              Available Budget Over Time                          │
└─────────────────────────────────────────────────────────────────┘

Initial State (16K context):
├─ Ollama context: 13,600 tokens (FIXED)
├─ System prompt: 1,000 tokens (fixed)
├─ Checkpoints: 0 tokens
└─ Available: 12,600 tokens
   └─ Trigger at: 10,080 tokens (80%)

After 1st Compression:
├─ Ollama context: 13,600 tokens (FIXED)
├─ System prompt: 1,000 tokens (fixed)
├─ Checkpoint 1: 1,200 tokens (recent, 50-70% compressed)
└─ Available: 10,400 tokens
   └─ Trigger at: 8,320 tokens (80%)

After 2nd Compression:
├─ Ollama context: 13,600 tokens (FIXED)
├─ System prompt: 1,000 tokens (fixed)
├─ Checkpoint 1: 600 tokens (aged, 60% compressed)
├─ Checkpoint 2: 1,200 tokens (recent, 50-70% compressed)
└─ Available: 10,800 tokens
   └─ Trigger at: 8,640 tokens (80%)

After 3rd Compression:
├─ Ollama context: 13,600 tokens (FIXED)
├─ System prompt: 1,000 tokens (fixed)
├─ Checkpoint 1: 300 tokens (ancient, 70% compressed)
├─ Checkpoint 2: 600 tokens (old, 60% compressed)
├─ Checkpoint 3: 1,200 tokens (recent, 50-70% compressed)
└─ Available: 10,500 tokens
   └─ Trigger at: 8,400 tokens (80%)
```

**Key Insight:** Available budget shrinks with each checkpoint, but aging keeps it sustainable for 3-5+ compressions.

---

## Checkpoint Lifecycle

```
┌─────────────────────────────────────────────────────────────────┐
│                    Checkpoint Aging                              │
└─────────────────────────────────────────────────────────────────┘

Creation (Checkpoint 3 - Recent):
┌──────────────────────────────────────┐
│ Level 3 - DETAILED                   │
│ ────────────────────────────────────│
│ Range: Messages 51-100               │
│ Summary: Full detailed summary...    │
│ Key Decisions: [Use JWT, cookies]    │
│ Files Modified: [login.ts, jwt.ts]   │
│ Next Steps: [Add registration]       │
│ Original: 5,000 tokens               │
│ Current: 1,200 tokens (50-70% comp)  │
│ Compression Count: 1                 │
└──────────────────────────────────────┘
            │
            │ Next compression triggered
            ▼
┌──────────────────────────────────────┐
│ Level 2 - MODERATE                   │
│ ────────────────────────────────────│
│ Range: Messages 51-100               │
│ Summary: Condensed summary...        │
│ Key Decisions: [...]  ← Preserved    │
│ Current: 600 tokens (60% comp)       │
│ Compression Count: 2                 │
└──────────────────────────────────────┘
            │
            │ Next compression triggered
            ▼
┌──────────────────────────────────────┐
│ Level 1 - COMPACT                    │
│ ────────────────────────────────────│
│ Range: Messages 51-100               │
│ Summary: "Built JWT auth..."         │
│ Current: 300 tokens (70% comp)       │
│ Compression Count: 3                 │
└──────────────────────────────────────┘
```

---

## Never-Compressed Content

```
┌─────────────────────────────────────────────────────────────────┐
│              Content Preservation Rules                          │
└─────────────────────────────────────────────────────────────────┘

ALWAYS PRESERVED (Never Compressed):
┌──────────────────────────────────────┐
│ ✅ System Prompt                     │
│    - Core Mandates                   │
│    - Sanity Checks                   │
│    - Mode Guidance                   │
│                                      │
│ ✅ User Messages                     │
│    - All user input                  │
│    - All user questions              │
│    - All user instructions           │
│                                      │
│ ✅ Active Goals                      │
│    - Goal description                │
│    - Checkpoints (pending/progress)  │
│    - Locked decisions                │
│    - Artifacts (files created)       │
│                                      │
│ ✅ Architecture Decisions            │
│    - Design patterns chosen          │
│    - Technology stack decisions      │
│    - API contracts                   │
│    - Database schemas                │
└──────────────────────────────────────┘

COMPRESSED WHEN NEEDED:
┌──────────────────────────────────────┐
│ 🔄 Assistant Messages                │
│    - LLM output                      │
│    - Code generated                  │
│    - Explanations                    │
│                                      │
│ 🔄 Tool Outputs                      │
│    - File contents                   │
│    - Search results                  │
│    - Command outputs                 │
│                                      │
│ 🔄 Exploratory Work                  │
│    - Debugging attempts              │
│    - Experimental code               │
│    - Brainstorming                   │
└──────────────────────────────────────┘
```

---

## Context Structure Example

```
┌─────────────────────────────────────────────────────────────────┐
│              Context After 3 Compressions (16K)                  │
└─────────────────────────────────────────────────────────────────┘

[System Prompt] - 1,000 tokens (never compressed)
  ├─ Core Mandates: 200 tokens
  ├─ Sanity Checks: 100 tokens
  ├─ Active Goal: 200 tokens (never compressed)
  │  ├─ Goal: "Implement user authentication"
  │  ├─ Checkpoints: ✅ Design, ✅ Login, 🔄 JWT
  │  ├─ Decisions: 🔒 Use JWT, 🔒 httpOnly cookies
  │  └─ Artifacts: login.ts, jwt.ts, api.ts
  └─ Mode Guidance: 500 tokens

[Checkpoint 1] - 300 tokens (ancient, 70% compressed)
  └─ "Early exploration and design decisions..."

[Checkpoint 2] - 600 tokens (old, 60% compressed)
  └─ "Implemented login endpoint with validation..."

[Checkpoint 3] - 1,200 tokens (recent, 50-70% compressed)
  └─ "Added JWT token generation and refresh logic..."

[User Messages] - 3,000 tokens (never compressed)
  └─ All user input preserved in full

[Recent Assistant Messages] - 7,500 tokens (not yet compressed)
  └─ Most recent LLM output

────────────────────────────────────────────────────────────────
Total: 13,600 tokens (100% of Ollama limit)
Available for new: ~2,000 tokens before next compression
```

---

## Token Budget Over Time

```
┌─────────────────────────────────────────────────────────────────┐
│                Token Usage Across Compressions                   │
└─────────────────────────────────────────────────────────────────┘

Compression 1 (16K context):
├─ System:     1,000 tokens
├─ CP1 (D):    1,200 tokens
└─ Recent:     7,500 tokens
   Total:      9,700 tokens (71% of 13,600)
   Available:  3,900 tokens

Compression 2:
├─ System:     1,000 tokens
├─ CP1 (M):      600 tokens  ← Aged!
├─ CP2 (D):    1,200 tokens
└─ Recent:     7,500 tokens
   Total:     10,300 tokens (76% of 13,600)
   Available:  3,300 tokens

Compression 3:
├─ System:     1,000 tokens
├─ CP1 (C):      300 tokens  ← Aged!
├─ CP2 (M):      600 tokens  ← Aged!
├─ CP3 (D):    1,200 tokens
└─ Recent:     7,500 tokens
   Total:     10,600 tokens (78% of 13,600)
   Available:  3,000 tokens

Legend: D=DETAILED, M=MODERATE, C=COMPACT
```

---

## Goal-Aware Compression

```
┌─────────────────────────────────────────────────────────────────┐
│              Goal-Guided Summarization                           │
└─────────────────────────────────────────────────────────────────┘

LLM receives goal context when summarizing:

ACTIVE GOAL: Implement user authentication system
Priority: high
Status: active

COMPLETED CHECKPOINTS:
✅ Design authentication flow
✅ Implement login endpoint

IN PROGRESS:
🔄 Add JWT token generation

LOCKED DECISIONS:
🔒 Use JWT for authentication
🔒 Store tokens in httpOnly cookies

ARTIFACTS:
- Created: src/auth/login.ts
- Created: src/auth/jwt.ts
- Modified: src/routes/api.ts

───────────────────────────────────────

Summarize the following conversation, focusing on progress toward the goal:
[Messages to compress...]

PRESERVE:
- Decisions made toward the goal
- Checkpoints completed
- Files created/modified
- Technical details relevant to the goal

SUMMARIZE AGGRESSIVELY:
- Off-topic discussions
- Exploratory conversations
- Debugging steps that succeeded
```

---

## System Benefits

### Progressive Preservation

**Without Progressive Checkpoints:**
```
After 3 compressions:
[System] + [Single Summary] + [Recent]
1,000    + 500             + 7,500 = 9,000 tokens
                            ↑
                ❌ Lost most conversation history!
```

**With Progressive Checkpoints:**
```
After 3 compressions:
[System] + [CP1] + [CP2] + [CP3] + [Recent]
1,000    + 300   + 600   + 1,200 + 7,500 = 10,600 tokens
                                  ↑
                  ✅ Full journey preserved!
```

**Key Advantages:**
- ✅ **No Information Loss**: All conversation history preserved
- ✅ **Hierarchical Compression**: Recent = detailed, old = compact
- ✅ **Automatic Aging**: Checkpoints compress as they age
- ✅ **Dynamic Budget**: Available space recalculated after each compression
- ✅ **Context Continuity**: LLM maintains full conversation awareness
- ✅ **Goal Preservation**: Goals and decisions never compressed

---

## Reliability Tracking

```
┌─────────────────────────────────────────────────────────────────┐
│              Conversation Reliability Score                      │
└─────────────────────────────────────────────────────────────────┘

Score Calculation:
modelFactor = 
  70B+ models: 0.95
  30B models:  0.85
  13B models:  0.70
  7B models:   0.50
  3B models:   0.30

compressionPenalty = max(1.0 - (compressionCount * 0.15), 0.30)

finalScore = modelFactor * compressionPenalty

Reliability Levels:
🟢 85-100%  High       - Excellent reliability
🟡 60-84%   Medium     - Good reliability
🟠 40-59%   Low        - Degraded reliability
🔴 <40%     Critical   - Poor reliability

Example (13B model, 3 compressions):
modelFactor = 0.70
compressionPenalty = 1.0 - (3 * 0.15) = 0.55
finalScore = 0.70 * 0.55 = 0.385 (38.5%)
Status: 🔴 Critical - Consider starting new conversation
```

---

## Event Timeline

```
┌─────────────────────────────────────────────────────────────────┐
│                    Event Sequence                                │
└─────────────────────────────────────────────────────────────────┘

Time ──────────────────────────────────────────────────────────▶

  │
  ├─ message-added ──────────────────────────────────────────────┐
  │                                                               │
  ├─ message-added ──────────────────────────────────────────────┤
  │                                                               │
  ├─ message-added ──────────────────────────────────────────────┤
  │                                                               │
  ├─ context-threshold-reached (80% of available) ───────────────┤
  │                                                               │
  ├─ compression-triggered ──────────────────────────────────────┤
  │                                                               │
  ├─ llm-summarizing ────────────────────────────────────────────┤
  │   └─ LLM generates summary                                    │
  │                                                               │
  ├─ checkpoint-created ─────────────────────────────────────────┤
  │   └─ New checkpoint added                                     │
  │                                                               │
  ├─ checkpoints-aged ───────────────────────────────────────────┤
  │   └─ Existing checkpoints compressed further                 │
  │                                                               │
  ├─ budget-recalculated ────────────────────────────────────────┤
  │   └─ New available space calculated                           │
  │                                                               │
  ├─ compressed ──────────────────────────────────────────────────┤
  │                                                               │
  └─ message-added ──────────────────────────────────────────────┘
```

---

## Dual Storage System

```
┌─────────────────────────────────────────────────────────────────┐
│              Active Context vs Full History                      │
└─────────────────────────────────────────────────────────────────┘

Active Context (Memory):
┌──────────────────────────────────────┐
│ Compressed for LLM efficiency        │
│ ────────────────────────────────────│
│ [System Prompt]                      │
│ [Checkpoints] ← Compressed           │
│ [User Messages] ← Never compressed   │
│ [Recent Messages] ← Not yet          │
│                                      │
│ Sent to LLM with each message        │
└──────────────────────────────────────┘

Full History (Disk):
┌──────────────────────────────────────┐
│ Uncompressed complete record         │
│ ────────────────────────────────────│
│ ALL messages (uncompressed)          │
│ ALL tool calls                       │
│ Metadata (tokens, compressions)      │
│                                      │
│ Saved to:                            │
│ ~/.ollm/sessions/{sessionId}.json    │
│                                      │
│ Never affected by compression        │
│ User can review anytime              │
└──────────────────────────────────────┘
```

**Key Principle:** Compression only affects what's sent to the LLM, not what's saved to disk.

---

## Configuration

```typescript
interface CompressionConfig {
  enabled: boolean;              // Enable compression
  strategy: 'summarize';         // Always use LLM summarization
  preserveRecent: number;        // Tokens to preserve (not compress)
  summaryMaxTokens: number;      // Max tokens for summary
  triggerThreshold: number;      // Trigger at % of available (0.80)
}

const DEFAULT_CONFIG = {
  enabled: true,
  strategy: 'summarize',
  preserveRecent: 2048,
  summaryMaxTokens: 1024,
  triggerThreshold: 0.80,
};
```

---

## Legend

```
Checkpoint Levels:
  D = DETAILED (Level 3)   - 1,200 tokens (50-70% compression)
  M = MODERATE (Level 2)   - 600 tokens (60% compression)
  C = COMPACT (Level 1)    - 300 tokens (70% compression)

Symbols:
  ✅ = Success / Benefit / Preserved
  ❌ = Problem / Issue / Never Compress
  🔄 = Compressed When Needed
  🔒 = Locked / Never Changes
  ← = Preserved / Maintained
  → = Transformed / Compressed
  ▼ = Flow direction
  ⭐ = Primary / Recommended

Status Icons:
  🟢 = High reliability / Normal
  🟡 = Medium reliability / Warning
  🟠 = Low reliability / Critical
  🔴 = Poor reliability / Emergency
```

---

## Summary

### Key Features

1. **Dynamic Budget Management** ✅
   - Available budget recalculated after each compression
   - Accounts for system prompt and checkpoints
   - Triggers at 80% of available space

2. **Checkpoint Aging** ✅
   - Recent: 50-70% compression (~1,200 tokens)
   - Old: 60% compression (~600 tokens)
   - Ancient: 70% compression (~300 tokens)

3. **LLM-Based Summarization** ✅
   - LLM summarizes its own output
   - Preserves meaning and context
   - Quality scales with model size

4. **Never-Compressed Content** ✅
   - System prompts
   - User messages
   - Active goals
   - Locked decisions

5. **Goal-Aware Compression** ✅
   - Goals guide summarization
   - Important decisions preserved
   - Progress tracked across compressions

6. **Dual Storage** ✅
   - Active context: Compressed for LLM
   - Full history: Uncompressed on disk
   - Complete audit trail maintained

---

**Document Status:** ✅ Updated  
**Last Updated:** January 26, 2026  
**Purpose:** Visual guide to checkpoint compression flow
