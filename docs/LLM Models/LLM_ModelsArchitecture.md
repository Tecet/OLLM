# Model Management Architecture

**System Design and Component Overview**

This document describes the architecture of the Model Management system, including components, data flow, and integration points.

---

## Table of Contents

1. [System Overview](#system-overview)
2. [Core Components](#core-components)
3. [Data Flow](#data-flow)
4. [Integration Points](#integration-points)
5. [Design Decisions](#design-decisions)
6. [Performance Considerations](#performance-considerations)

---

## System Overview

The Model Management system provides comprehensive lifecycle management for LLM models, intelligent routing, persistent memory, prompt templates, and project profiles.

### Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLI Layer                                │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ Model Cmds   │  │ Memory Cmds  │  │ Template Cmds│          │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘          │
└─────────┼──────────────────┼──────────────────┼─────────────────┘
          │                  │                  │
┌─────────┼──────────────────┼──────────────────┼─────────────────┐
│         │      Service Layer                  │                  │
│  ┌──────▼───────────┐  ┌──▼──────────┐  ┌───▼──────────┐       │
│  │ Model Mgmt Svc   │  │ Memory Svc  │  │ Template Svc │       │
│  │ - List models    │  │ - CRUD ops  │  │ - Load       │       │
│  │ - Pull/delete    │  │ - Search    │  │ - Substitute │       │
│  │ - Keep-alive     │  │ - Inject    │  │ - Persist    │       │
│  └──────┬───────────┘  └──────┬──────┘  └──────┬───────┘       │
│         │                     │                 │                │
│  ┌──────▼───────────┐  ┌─────▼──────────┐  ┌──▼──────────┐    │
│  │ Model Router     │  │ Project Profile│  │ Comparison  │    │
│  │ - Select model   │  │ - Detect type  │  │ - Parallel  │    │
│  │ - Apply profile  │  │ - Apply config │  │ - Metrics   │    │
│  └──────┬───────────┘  └─────┬──────────┘  └─────────────┘    │
└─────────┼────────────────────┼─────────────────────────────────┘
          │                    │
┌─────────┼────────────────────┼─────────────────────────────────┐
│         │      Data Layer    │                                  │
│  ┌──────▼───────────┐  ┌────▼──────────┐  ┌──────────────┐    │
│  │ Model Database   │  │ Memory Store  │  │ Template Dir │    │
│  │ - Known models   │  │ - JSON file   │  │ - YAML files │    │
│  │ - Capabilities   │  │ - Categories  │  │ - Variables  │    │
│  └──────┬───────────┘  └───────────────┘  └──────────────┘    │
└─────────┼──────────────────────────────────────────────────────┘
          │
┌─────────▼──────────────────────────────────────────────────────┐
│                    Provider Layer                               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ Ollama       │  │ vLLM         │  │ OpenAI Compat│         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
└─────────────────────────────────────────────────────────────────┘
```

### Key Characteristics

- **Service-Oriented:** Independent services with clear responsibilities
- **Provider-Agnostic:** Works with multiple LLM backends
- **Persistent:** Memory and templates survive restarts
- **Intelligent:** Automatic model selection and project detection
- **Performant:** Caching and lazy loading throughout

---

## Core Components

### 1. Model Management Service

**Purpose:** Manage model lifecycle (list, pull, delete, inspect)

**Responsibilities:**

- List available models with caching
- Pull models with progress tracking
- Delete models with auto-unload
- Show detailed model information
- Keep models loaded with timeout
- Invalidate cache on mutations

**Key Features:**

- 60-second TTL cache for model list
- Progress events during pull operations
- Automatic unload before deletion
- Configurable keep-alive timeout (default 300s)

**Dependencies:**

- Provider adapter (Ollama, vLLM, OpenAI-compatible)
- Model Database (for capability lookup)

**File:** `packages/core/src/services/modelManagementService.ts`

### 2. Model Router

**Purpose:** Intelligent model selection based on task requirements

**Responsibilities:**

- Select best model for task profile
- Filter by context window and capabilities
- Score by preferred families
- Handle fallback chains
- Apply configuration overrides

**Key Features:**

- 4 built-in profiles: fast, general, code, creative
- Preferred family prioritization
- Circular fallback detection
- Configuration override support

**Dependencies:**

- Model Database (for model lookup)
- Routing Profiles (for selection criteria)
- Model Management Service (for available models)

**File:** `packages/core/src/routing/modelRouter.ts`

### 3. Model Database

**Purpose:** Central registry of known models and their capabilities

**Responsibilities:**

- Store model metadata (context window, capabilities)
- Pattern matching with wildcards
- Provide safe defaults for unknown models
- Support model family grouping

**Key Features:**

- 50+ known model families
- Wildcard pattern matching (e.g., `llama*`)
- Capability flags (tools, vision, streaming)
- Context window limits

**File:** `packages/core/src/routing/modelDatabase.ts`

### 4. Memory Service

**Purpose:** Persistent key-value storage with system prompt injection

**Responsibilities:**

- CRUD operations (remember, recall, search, forget, listAll)
- Persist to JSON file (~/.ollm/memory.json)
- Track access count and timestamps
- Categorize memories (fact, preference, context)
- Mark source (user, llm, system)
- Inject into system prompt with token budget

**Key Features:**

- Persistent storage across sessions
- Categorization for organization
- Source tracking for provenance
- System prompt injection (500-token budget)
- Prioritization by access count and recency
- LLM tool integration

**Dependencies:**

- File system (for persistence)
- Token counter (for budget management)

**File:** `packages/core/src/services/memoryService.ts`

### 5. Template Service

**Purpose:** Prompt template management with variable substitution

**Responsibilities:**

- Load templates from user and workspace directories
- Parse YAML template definitions
- Validate template structure
- Substitute variables with defaults
- Cache parsed templates
- CRUD operations

**Key Features:**

- YAML template format
- Variable substitution: `{var}` and `{var:default}`
- Required vs optional variables
- Workspace overrides user templates
- Template caching for performance

**Dependencies:**

- File system (for template loading)
- YAML parser (for template parsing)

**File:** `packages/core/src/services/templateService.ts`

### 6. Project Profile Service

**Purpose:** Auto-detect project type and apply appropriate settings

**Responsibilities:**

- Detect project type from files
- Load profile from .ollm/project.yaml
- Define built-in profiles
- Apply profile settings with precedence
- Support manual override
- Initialize project configuration

**Key Features:**

- Auto-detection (TypeScript, Python, Rust, Go, Documentation)
- 5 built-in profiles with defaults
- Project settings override global settings
- Manual profile selection
- Project initialization command

**Dependencies:**

- File system (for detection and config)
- Configuration service (for settings)

**File:** `packages/core/src/services/projectProfileService.ts`

### 7. Comparison Service

**Purpose:** Compare multiple models side-by-side

**Responsibilities:**

- Execute prompt on multiple models in parallel
- Collect response text, token count, latency
- Calculate tokens per second
- Handle individual model failures
- Support cancellation

**Key Features:**

- Parallel execution for speed
- Performance metrics collection
- Graceful failure handling
- Cancellation support via AbortController

**Dependencies:**

- Model Management Service (for model execution)
- Provider adapters (for model calls)

**File:** `packages/core/src/services/comparisonService.ts`

---

## Data Flow

### Model Selection Flow

```
1. User Request
   ↓
2. Determine Task Profile
   - Fast: Quick responses
   - General: Balanced performance
   - Code: Programming tasks
   - Creative: Creative writing
   ↓
3. Model Router
   - Query available models
   - Filter by context window
   - Filter by capabilities
   - Score by preferred families
   ↓
4. Apply Configuration
   - Check manual override
   - Apply project profile
   - Apply environment variables
   ↓
5. Select Model
   - Choose highest scoring model
   - Fall back if unavailable
   - Detect circular fallbacks
   ↓
6. Execute Request
```

### Memory Injection Flow

```
1. Chat Request
   ↓
2. Memory Service
   - Load all memories
   - Filter by relevance (optional)
   - Sort by priority (access count + recency)
   ↓
3. Budget Allocation
   - Default: 500 tokens
   - Configurable via settings
   ↓
4. Format Memories
   - Key-value pairs
   - Category labels
   - Compact format
   ↓
5. Inject into System Prompt
   - Prepend to existing prompt
   - Stay within token budget
   ↓
6. Send to Model
```

### Template Substitution Flow

```
1. Template Request
   ↓
2. Template Service
   - Load template from file
   - Parse YAML structure
   - Extract variables
   ↓
3. Variable Validation
   - Check required variables provided
   - Apply default values
   - Validate variable names
   ↓
4. Substitution
   - Replace {var} with values
   - Replace {var:default} with value or default
   - Handle special characters
   ↓
5. Return Prompt
   - Fully substituted prompt
   - Ready for model execution
```

### Project Profile Flow

```
1. Startup / Project Init
   ↓
2. Project Detection
   - Check for package.json (TypeScript/JavaScript)
   - Check for requirements.txt (Python)
   - Check for Cargo.toml (Rust)
   - Check for go.mod (Go)
   - Check for docs/ (Documentation)
   ↓
3. Profile Loading
   - Load .ollm/project.yaml if exists
   - Otherwise use detected profile
   - Fall back to manual selection
   ↓
4. Settings Application
   - Merge profile settings
   - Project settings override global
   - Apply model, system prompt, tools
   ↓
5. Configuration Active
   - Profile applied to all requests
   - Can be overridden manually
```

---

## Integration Points

### 1. Provider Integration

**Interface:** `ProviderAdapter`

**Methods:**

- `listModels()` - Get available models
- `pullModel(name, onProgress)` - Download model
- `deleteModel(name)` - Remove model
- `showModel(name)` - Get model details
- `chat(messages, options)` - Execute chat request

**Providers:**

- Ollama (Tier 1) - Local models
- vLLM (Tier 2) - High performance
- OpenAI-compatible (Tier 3) - Universal

**Location:** `packages/ollm-bridge/src/provider/`

### 2. Configuration Integration

**Interface:** `Settings`

**Sections:**

- `models` - Model management options
- `routing` - Routing configuration
- `memory` - Memory settings
- `templates` - Template directories
- `project` - Project profile settings

**Precedence:**

1. Environment variables (highest)
2. Project config (.ollm/config.yaml)
3. User config (~/.ollm/config.yaml)
4. Defaults (lowest)

**Location:** `packages/core/src/config/settings.ts`

### 3. Token Limits Integration

**Interface:** `TokenLimits`

**Integration:**

- Query Model Database for context window
- Use safe defaults for unknown models
- Trigger compression when approaching limits
- Dynamic context sizing based on model

**Location:** `packages/core/src/core/tokenLimits.ts`

### 4. Context Management Integration

**Interface:** `ContextManager`

**Integration:**

- Memory injection into system prompt
- Token budget management
- Context compression triggers
- Snapshot management

**Location:** `packages/core/src/context/contextManager.ts`

### 5. CLI Integration

**Commands:**

- `/model` - Model management commands
- `/memory` - Memory commands
- `/template` - Template commands
- `/compare` - Model comparison
- `/project` - Project profile commands

**Location:** `packages/cli/src/commands/`

### 6. UI Integration

**Components:**

- StatusBar - Show loaded models and active profile
- ComparisonView - Side-by-side model comparison
- Model picker - Select model manually

**Location:** `packages/cli/src/ui/components/`

---

## Design Decisions

### 1. Service-Oriented Architecture

**Decision:** Use independent services with clear responsibilities

**Rationale:**

- Easier to test in isolation
- Clear separation of concerns
- Easier to extend and maintain
- Supports dependency injection

**Trade-offs:**

- More files and classes
- Requires coordination between services
- More complex initialization

### 2. Caching Strategy

**Decision:** Use TTL-based caching with manual invalidation

**Rationale:**

- Reduces provider calls (95% reduction)
- Improves response time
- Balances freshness and performance

**Implementation:**

- Model list: 60-second TTL
- Template cache: In-memory, no expiry
- Invalidate on mutations (pull, delete)

**Trade-offs:**

- Stale data possible within TTL
- Memory usage for cache
- Complexity of invalidation logic

### 3. File-Based Persistence

**Decision:** Use JSON files for memory, YAML for templates

**Rationale:**

- Simple and portable
- Human-readable and editable
- No database dependency
- Easy backup and version control

**Implementation:**

- Memory: `~/.ollm/memory.json`
- Templates: `~/.ollm/templates/*.yaml`
- Project config: `.ollm/project.yaml`

**Trade-offs:**

- Not suitable for large datasets
- No concurrent access control
- Manual file locking needed

### 4. Pattern Matching for Models

**Decision:** Use wildcard patterns for model families

**Rationale:**

- Flexible matching (e.g., `llama*` matches all Llama models)
- Reduces database size
- Easy to add new models

**Implementation:**

- Exact match first
- Pattern match second
- Default fallback third

**Trade-offs:**

- Pattern conflicts possible
- Order matters
- Less precise than exact match

### 5. Profile-Based Routing

**Decision:** Use task profiles instead of manual selection

**Rationale:**

- Automatic model selection
- Optimized for task type
- Reduces cognitive load
- Consistent behavior

**Implementation:**

- 4 built-in profiles
- Configurable preferred families
- Fallback chains
- Manual override available

**Trade-offs:**

- May not always pick "best" model
- Requires profile tuning
- Less control for advanced users

### 6. Token Budget for Memory

**Decision:** Limit memory injection to 500 tokens by default

**Rationale:**

- Prevents context overflow
- Leaves room for conversation
- Configurable per user

**Implementation:**

- Sort by priority (access count + recency)
- Include highest priority memories first
- Stop when budget reached

**Trade-offs:**

- Some memories may not be included
- Requires tuning for optimal results
- Priority algorithm may not be perfect

---

## Performance Considerations

### 1. Model List Caching

**Problem:** Listing models is slow (2-3 seconds per call)

**Solution:** Cache with 60-second TTL

**Results:**

- 95% reduction in provider calls
- <10ms response time for cached data
- Automatic invalidation on mutations

### 2. Template Caching

**Problem:** Parsing YAML on every use is slow

**Solution:** Parse once, cache in memory

**Results:**

- 90% reduction in template load time
- Instant template retrieval
- File I/O only on initial load

### 3. Lazy Loading

**Problem:** Loading all data at startup is slow

**Solution:** Load on demand

**Implementation:**

- Templates loaded when first used
- Profiles loaded when first accessed
- Memory loaded when needed

**Results:**

- Fast startup time
- Reduced memory usage
- Better resource utilization

### 4. Parallel Execution

**Problem:** Comparing models sequentially is slow

**Solution:** Execute in parallel with Promise.all

**Results:**

- N models in ~same time as 1 model
- Better resource utilization
- Faster comparison results

### 5. Efficient Search

**Problem:** Linear search through memories is slow

**Solution:** Optimize search algorithm

**Implementation:**

- O(n) linear search (acceptable for typical usage)
- Index by key for exact lookups
- Filter early to reduce iterations

**Results:**

- <1ms for 100 entries
- Suitable for typical usage
- Can optimize further if needed

---

## Security Considerations

### 1. File Permissions

**Issue:** Memory and templates contain sensitive data

**Mitigation:**

- Create files with restrictive permissions (0600)
- Store in user home directory
- Validate file paths to prevent traversal

### 2. Template Injection

**Issue:** Templates could inject malicious content

**Mitigation:**

- Validate variable names (alphanumeric + underscore)
- Escape special characters
- Limit template size

### 3. Memory Injection

**Issue:** Memories could inject malicious prompts

**Mitigation:**

- Token budget limits injection size
- Sanitize memory values
- User controls what's remembered

### 4. Model Selection

**Issue:** Malicious model names could cause issues

**Mitigation:**

- Validate model names
- Use provider's model list
- Sanitize user input

---

## Extensibility

### Adding New Routing Profiles

```typescript
// Define profile in routingProfiles.ts
export const myProfile: RoutingProfile = {
  name: 'my-profile',
  description: 'My custom profile',
  preferredFamilies: ['llama', 'mistral'],
  minContextWindow: 4096,
  requiredCapabilities: ['tools'],
  fallbackProfile: 'general',
};
```

### Adding New Project Profiles

```typescript
// Define profile in projectProfileService.ts
const myProfile: ProjectProfile = {
  name: 'my-project',
  description: 'My project type',
  detectionRules: {
    files: ['my-config.json'],
    directories: ['my-dir'],
  },
  defaultSettings: {
    model: 'my-model',
    systemPrompt: 'My prompt',
    tools: ['my-tool'],
  },
};
```

### Adding New Memory Categories

```typescript
// Add to MemoryCategory type
export type MemoryCategory = 'fact' | 'preference' | 'context' | 'my-category'; // Add here
```

---

## Testing Strategy

### Unit Tests

**Coverage:**

- Each service tested independently
- Mock dependencies
- Test edge cases

**Example:**

```typescript
describe('ModelManagementService', () => {
  it('should cache model list', async () => {
    // Test caching behavior
  });
});
```

### Property Tests

**Coverage:**

- Universal properties
- 100 iterations per property
- Random input generation

**Example:**

```typescript
fc.assert(
  fc.asyncProperty(fc.array(fc.string()), async (models) => {
    // Test property holds for all inputs
  })
);
```

### Integration Tests

**Coverage:**

- Full lifecycle workflows
- Service interactions
- End-to-end scenarios

**Example:**

```typescript
describe('Model Lifecycle', () => {
  it('should pull, use, and delete model', async () => {
    // Test full workflow
  });
});
```

---

## Monitoring and Observability

### Metrics

**Model Management:**

- Cache hit rate
- Pull success rate
- Average pull time
- Keep-alive usage

**Memory:**

- Total memories
- Memory size
- Injection frequency
- Search performance

**Templates:**

- Template usage
- Substitution errors
- Cache hit rate

**Routing:**

- Profile usage
- Selection time
- Fallback frequency

### Logging

**Levels:**

- ERROR: Failures and exceptions
- WARN: Degraded performance
- INFO: Important events
- DEBUG: Detailed diagnostics

**Events:**

- Model operations (pull, delete, keep-alive)
- Memory operations (add, forget, inject)
- Template operations (load, substitute)
- Routing decisions (selection, fallback)

---

## Future Enhancements

### Planned Features

1. **Model Performance Benchmarking**
   - Track response time per model
   - Track tokens per second
   - Automatic model recommendations

2. **Advanced Routing Rules**
   - Cost-based routing
   - Latency-based routing
   - Load balancing

3. **Memory Enhancements**
   - Fuzzy search
   - Semantic search
   - Memory expiration

4. **Template Marketplace**
   - Share templates
   - Download templates
   - Template ratings

5. **Project Profile Customization**
   - UI for profile editing
   - Profile inheritance
   - Profile validation

---

## References

### Specifications

- Requirements (../../.kiro/specs/stage-07-model-management/requirements.md)
- Design (../../.kiro/specs/stage-07-model-management/design.md)
- Tasks (../../.kiro/specs/stage-07-model-management/tasks.md)

### Implementation

- [Model Management Service](../../packages/core/src/services/modelManagementService.ts)
- [Model Router](../../packages/core/src/routing/modelRouter.ts)
- [Memory Service](../../packages/core/src/services/memoryService.ts)
- [Template Service](../../packages/core/src/services/templateService.ts)
- [Project Profile Service](../../packages/core/src/services/projectProfileService.ts)

### Documentation

- [Getting Started](3%20projects/OLLM%20CLI/LLM%20Models/getting-started.md)
- [Commands Reference](Models_commands.md)
- [Configuration Guide](Models_configuration.md)
- [API Reference](3%20projects/OLLM%20CLI/LLM%20Models/api/README.md)

---

**Document Version:** 1.0  
**Last Updated:** 2026-01-16  
**Status:** Complete

<!-- Alpha Release: v0.1.6 -->

