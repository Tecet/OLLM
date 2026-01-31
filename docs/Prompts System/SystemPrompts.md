# System Prompt Architecture

**Last Updated:** January 26, 2026

**Related Documents:**

- `ContextManagement.md` - Context sizing, tiers, VRAM
- `ContextCompression.md` - Compression, checkpoints, snapshots

---

## Overview

The System Prompt is the foundation of LLM behavior. It defines how the LLM should think, act, and respond. The prompt adapts based on context tier and operational mode to balance quality with efficiency.

**Core Principle:** Larger contexts can afford more detailed prompts, leading to significantly better output quality without sacrificing workspace.

---

## System Prompt Structure

The system prompt consists of multiple components assembled in a specific order:

```
[System Prompt]
├─ Core Mandates (tier-specific)
├─ Active Goal (if set)
├─ Active Skills (if any)
├─ Sanity Checks
└─ Mode-Specific Guidance
```

### Component Breakdown

**1. Core Mandates (Tier-Specific)**

- Essential behavior guidelines
- Quality standards
- Guardrails (what NOT to do)
- Examples (do this, not that)
- Token budget: 200-1500 tokens depending on tier

**2. Active Goal (Optional)**

- Current goal description
- Checkpoints (pending, in-progress, completed)
- Locked decisions
- Artifacts (files created/modified)
- Next steps
- Token budget: ~200 tokens
- **Never compressed** (see ContextCompression.md)

**3. Active Skills (Optional)**

- Skill-specific instructions
- Domain expertise
- Specialized techniques
- Token budget: ~100-300 tokens per skill

**4. Sanity Checks**

- Reality checks before responding
- Common mistake prevention
- Token budget: ~50-100 tokens

**5. Mode-Specific Guidance**

- Operational mode instructions
- Focus areas
- Expected behavior
- Token budget: Included in Core Mandates

---

## Concept

**Challenge:** System prompts are critical for LLM output quality, but they consume valuable context tokens. How do we balance quality with efficiency?

**Without Adaptive Prompts:**

- ❌ Fixed prompt size regardless of context capacity
- ❌ Small contexts waste space on detailed prompts
- ❌ Large contexts underutilize with minimal prompts
- ❌ Same guidance for 4K and 128K contexts
- ❌ No adaptation to operational mode

**Our Solution:** Adaptive system prompts that scale intelligently:

- **Tier 1 (Minimal):** Essential prompts (~200 tokens) - basic behavior + guardrails
- **Tier 2 (Basic):** Detailed prompts (~500 tokens) - comprehensive guidance
- **Tier 3 (Standard):** Comprehensive prompts (~1000 tokens) - full methodology ⭐
- **Tier 4 (Premium):** Expert prompts (~1500 tokens) - sophisticated reasoning
- **Tier 5 (Ultra):** Expert prompts (~1500 tokens) - maximum sophistication

**Key Insight:** Larger contexts can afford more detailed prompts, leading to significantly better output quality without sacrificing workspace.

---

## Context Tiers and Prompt Sizing

Context tiers are **labels** that represent different context window sizes. The system prompt scales with the tier to provide appropriate guidance without wasting tokens.

### Tier Definitions

| Tier              | Context Size | Ollama Size (85%) | Prompt Budget | Use Case                            |
| ----------------- | ------------ | ----------------- | ------------- | ----------------------------------- |
| Tier 1 (Minimal)  | 2K, 4K       | 1700, 3400        | ~200 tokens   | Quick tasks, minimal context        |
| Tier 2 (Basic)    | 8K           | 6800              | ~500 tokens   | Standard conversations              |
| Tier 3 (Standard) | 16K          | 13600             | ~1000 tokens  | Complex tasks, code review ⭐       |
| Tier 4 (Premium)  | 32K          | 27200             | ~1500 tokens  | Large codebases, long conversations |
| Tier 5 (Ultra)    | 64K, 128K    | 54400, 108800     | ~1500 tokens  | Maximum context, research tasks     |

**Key Points:**

- Tiers are **labels only** - they don't make decisions
- Context size drives everything
- Each tier has specific context sizes (not ranges)
- Prompt budget scales with tier
- The 85% values are **pre-calculated** in `LLM_profiles.json`

---

## Token Budget Strategy

### Token Allocations by Tier

```
Tier 1 (Minimal):  ~200 tokens  (5.0% of 4K)   - Basic behavior + guardrails
Tier 2 (Basic):    ~500 tokens  (6.3% of 8K)   - Detailed guidance + examples
Tier 3 (Standard): ~1000 tokens (3.1% of 32K)  - Comprehensive + do/don't examples ⭐
Tier 4 (Premium):  ~1500 tokens (2.3% of 64K)  - Expert-level detail + frameworks
Tier 5 (Ultra):    ~1500 tokens (1.2% of 128K) - Full expertise + extensive examples
```

**Design Philosophy:**

- **Tier 1:** Essential behavior, basic guardrails, minimal examples
- **Tier 2:** Detailed guidance, key examples, important guardrails
- **Tier 3:** Comprehensive instructions, multiple examples, full guardrails ⭐
- **Tier 4:** Expert-level detail, extensive examples, sophisticated reasoning
- **Tier 5:** Maximum sophistication, advanced patterns, mentoring approach

**What Each Tier Includes:**

- ✅ Behavioral guidelines (tone, style, approach)
- ✅ Guardrails (what NOT to do)
- ✅ Concrete examples (do this, not that)
- ✅ Reasoning and trade-offs
- ✅ Quality expectations

**Efficiency Analysis:**

- Tier 1: 5.0% overhead, 95.0% workspace
- Tier 2: 6.3% overhead, 93.7% workspace
- Tier 3: 3.1% overhead, 96.9% workspace ⭐
- Tier 4: 2.3% overhead, 97.7% workspace
- Tier 5: 1.2% overhead, 98.8% workspace

---

## Operational Modes

The system supports multiple operational modes, each with specific focus areas and behavior:

### Mode Definitions

**1. Developer Mode**

- **Focus:** Code quality, architecture, testing
- **Guidance:** SOLID principles, design patterns, error handling
- **Output:** Production-quality code with tests and documentation
- **Use Case:** Software development, code review, refactoring

**2. Planning Mode**

- **Focus:** Task breakdown, dependencies, estimation
- **Guidance:** Risk assessment, realistic planning, clear criteria
- **Output:** Actionable plans with dependencies and estimates
- **Use Case:** Project planning, task organization, sprint planning

**3. Assistant Mode**

- **Focus:** Clear communication, helpful responses
- **Guidance:** Conversational style, examples, explanations
- **Output:** Informative, well-structured answers
- **Use Case:** General questions, learning, exploration

**4. Debugger Mode**

- **Focus:** Systematic debugging, root cause analysis
- **Guidance:** Debugging process, hypothesis testing, verification
- **Output:** Clear diagnosis with reproduction steps and fixes
- **Use Case:** Bug investigation, error analysis, troubleshooting

### Mode Selection

Modes are selected based on:

- User explicit selection (`/mode developer`)
- Task analysis (automatic detection)
- Context clues (code files → developer, planning docs → planning)

---

## Prompt Components by Tier

### What's Included at Each Level

**Tier 1 (~200 tokens):**

- ✅ Core behavior guidelines
- ✅ Basic guardrails (what NOT to do)
- ✅ 1-2 simple examples
- ✅ Essential quality standards

**Tier 2 (~500 tokens):**

- ✅ Detailed responsibilities
- ✅ Comprehensive guardrails
- ✅ 3-5 concrete examples (do/don't)
- ✅ Behavioral guidelines
- ✅ Quality standards with reasoning

**Tier 3 (~1000 tokens) ⭐:**

- ✅ Comprehensive methodology
- ✅ Full guardrails with explanations
- ✅ 5-8 detailed examples with code
- ✅ Decision-making frameworks
- ✅ Trade-off analysis
- ✅ Best practices with rationale
- ✅ Common pitfalls to avoid

**Tier 4/5 (~1500 tokens):**

- ✅ Expert-level detail
- ✅ Extensive examples (10+)
- ✅ Sophisticated reasoning
- ✅ Multiple scenario handling
- ✅ Advanced patterns and techniques
- ✅ Mentoring approach
- ✅ Industry best practices
- ✅ Performance and security considerations

### Why This Design Works

**1. Proper Guardrails ✅**

- Clear "what NOT to do" sections
- Prevents common mistakes
- Sets quality expectations

**2. Concrete Examples ✅**

- Shows good vs bad code
- Demonstrates concepts clearly
- Reduces ambiguity

**3. Behavioral Guidance ✅**

- Sets tone and approach
- Defines communication style
- Establishes expectations

**4. Reasoning and Trade-offs ✅**

- Explains the "why" behind guidelines
- Helps LLM make better decisions
- Improves output quality

**5. Scales Appropriately ✅**

- Small contexts: Essential guidance only
- Medium contexts: Detailed with examples
- Large contexts: Comprehensive methodology
- Premium contexts: Expert-level sophistication

---

## Prompt Examples

### Tier 1 (Minimal) - Essential (~200 tokens)

**Developer Mode:**

```
You are a coding assistant focused on practical solutions.

Core Behavior:
- Write clean, working code
- Use TypeScript with types
- Add brief comments for complex logic
- Handle errors appropriately

Guardrails:
- Don't over-engineer simple solutions
- Don't skip error handling
- Don't ignore edge cases

Example:
✓ DO: Write simple, clear functions with error handling
✗ DON'T: Create complex abstractions for simple tasks

Keep responses concise but complete.
```

**~200 tokens** - Essential behavior + basic guardrails

**Planning Mode:**

```
You help plan and organize tasks effectively.

Core Behavior:
- Break down goals into specific tasks
- Identify dependencies clearly
- Estimate effort realistically
- Define success criteria

Guardrails:
- Don't create overly detailed plans
- Don't skip dependency analysis
- Don't underestimate complexity

Example:
✓ DO: "Task: Add login form (2h) - Depends on: Auth API"
✗ DON'T: "Task: Build authentication system"

Be practical and actionable.
```

**~200 tokens** - Essential planning guidance

---

### Tier 2 (Basic) - Detailed (~500 tokens)

**Developer Mode:**

```
You are an expert coding assistant focused on quality and maintainability.

Core Responsibilities:
- Write production-quality code with proper error handling
- Design clear, maintainable architectures
- Follow TypeScript best practices with strict types
- Document decisions and complex logic
- Consider edge cases and failure modes

Code Quality Standards:
- Use meaningful variable and function names
- Keep functions focused and single-purpose
- Add JSDoc comments for public APIs
- Write unit tests for critical logic
- Handle errors explicitly, never silently fail

Behavioral Guidelines:
- Explain your approach before implementing
- Point out potential issues or trade-offs
- Suggest improvements to existing code
- Ask clarifying questions when requirements are unclear

Guardrails - What NOT to Do:
✗ Don't use 'any' types without justification
✗ Don't skip error handling for "happy path only"
✗ Don't create deeply nested code (max 3 levels)
✗ Don't ignore TypeScript errors
✗ Don't write functions longer than 50 lines

Examples:
✓ DO: Validate input, handle errors, return typed results
✗ DON'T: Assume input is valid, let errors bubble silently

✓ DO: Break complex logic into small, testable functions
✗ DON'T: Write monolithic functions that do everything

✓ DO: Use descriptive names like 'calculateUserDiscount'
✗ DON'T: Use vague names like 'process' or 'handle'

When in doubt, choose clarity over cleverness.
```

**~500 tokens** - Detailed guidance with examples

---

### Tier 3 (Standard) - Comprehensive (~1000 tokens) ⭐ PRIMARY

**Developer Mode:**

```
You are an expert software developer and architect with a focus on production-quality code.

Core Responsibilities:
- Write clean, maintainable, well-tested code
- Design scalable architectures with clear separation of concerns
- Follow SOLID principles and appropriate design patterns
- Document architectural decisions with clear rationale
- Consider performance, security, accessibility, and maintainability
- Anticipate edge cases and failure modes

Code Quality Standards:
- Use TypeScript with strict mode and comprehensive types
- Write self-documenting code with meaningful names
- Keep functions focused (single responsibility)
- Add JSDoc comments for public APIs and complex logic
- Implement proper error handling with specific error types
- Write tests at appropriate levels (unit, integration, e2e)
- Optimize for readability first, performance second

Architectural Thinking:
- Start with requirements and constraints
- Consider multiple approaches and their trade-offs
- Choose the simplest solution that meets requirements
- Document why you chose this approach over alternatives
- Think about how the code will evolve
- Plan for monitoring, debugging, and maintenance

Behavioral Guidelines:
- Explain your reasoning and approach
- Point out potential issues or risks
- Suggest improvements to existing code
- Ask clarifying questions when requirements are ambiguous
- Acknowledge trade-offs and limitations honestly
- Provide context for your decisions

Guardrails - What NOT to Do:
✗ Don't use 'any' types without explicit justification
✗ Don't skip error handling or validation
✗ Don't create deeply nested code (max 3 levels)
✗ Don't write functions longer than 50 lines
✗ Don't ignore TypeScript errors or warnings
✗ Don't over-engineer simple solutions
✗ Don't skip tests for critical logic
✗ Don't leave TODO comments without tracking
✗ Don't copy-paste code without understanding it
✗ Don't optimize prematurely

Examples - Do This, Not That:

Error Handling:
✓ DO:
  try {
    const result = await fetchData();
    return { success: true, data: result };
  } catch (error) {
    logger.error('Failed to fetch data', { error });
    return { success: false, error: 'Data fetch failed' };
  }

✗ DON'T:
  const result = await fetchData(); // No error handling
  return result;

Type Safety:
✓ DO:
  interface User {
    id: string;
    email: string;
    role: 'admin' | 'user';
  }
  function getUser(id: string): Promise<User> { ... }

✗ DON'T:
  function getUser(id: any): Promise<any> { ... }

Function Design:
✓ DO:
  // Single responsibility, clear purpose
  function calculateDiscount(price: number, userTier: string): number {
    const rate = DISCOUNT_RATES[userTier] ?? 0;
    return price * rate;
  }

✗ DON'T:
  // Does too much, unclear purpose
  function process(data: any): any {
    // 100 lines of mixed logic
  }

When Making Decisions:
1. Understand the full context and requirements
2. Consider 2-3 different approaches
3. Evaluate trade-offs (complexity, performance, maintainability)
4. Choose the approach that best fits the constraints
5. Document why you chose this approach
6. Implement with proper error handling and tests
7. Consider how this fits into the larger system

Remember: Code is read far more often than it's written. Optimize for clarity and maintainability.
```

**~1000 tokens** - Comprehensive guidance with multiple examples

---

### Tier 4/5 (Premium/Ultra) - Expert Level (~1500 tokens)

**Note:** Tier 4 and 5 use the same prompts. At these context sizes, the prompt overhead is negligible (2.3% for Tier 4, 1.2% for Tier 5), and the focus shifts to maintaining quality across extremely long conversations.

**Developer Mode includes:**

- Extensive examples (10+ scenarios)
- Sophisticated reasoning frameworks
- Multiple scenario handling
- Advanced trade-off analysis
- Mentoring and teaching approach
- Industry best practices
- Common pitfalls and how to avoid them
- Performance optimization strategies
- Security considerations
- Scalability patterns

---

## Token Efficiency Analysis

### Overhead vs Workspace

**Tier 1 (4K context):**

```
System Prompt:    200 tokens (5.0%)
Work Space:     3,800 tokens (95.0%)
────────────────────────────────
Total:          4,000 tokens
```

✅ Efficient for small contexts

**Tier 2 (8K context):**

```
System Prompt:    500 tokens (6.3%)
Work Space:     7,500 tokens (93.7%)
────────────────────────────────
Total:          8,000 tokens
```

✅ Good balance

**Tier 3 (32K context) ⭐:**

```
System Prompt:  1,000 tokens (3.1%)
Work Space:    31,000 tokens (96.9%)
────────────────────────────────
Total:         32,000 tokens
```

✅ Excellent balance

**Tier 4 (64K context):**

```
System Prompt:  1,500 tokens (2.3%)
Work Space:    62,500 tokens (97.7%)
────────────────────────────────
Total:         64,000 tokens
```

✅ Very efficient

**Tier 5 (128K context):**

```
System Prompt:   1,500 tokens (1.2%)
Work Space:    126,500 tokens (98.8%)
────────────────────────────────
Total:         128,000 tokens
```

✅ Minimal overhead

---

## System Benefits

### 1. Automatic Adaptation ✅

**No Configuration Required:**

- System detects context size automatically
- Selects appropriate prompt tier
- Adapts to operational mode
- Updates seamlessly on changes

**Example Flow:**

```
User switches from 8K to 32K model
→ System detects new context size
→ Upgrades prompt from Tier 2 to Tier 3
→ LLM immediately produces better output
→ No user action needed
```

### 2. Quality Scaling ✅

**Tier 1:** Basic functionality

- "Write code" → Gets simple code
- Good enough for quick tasks

**Tier 3:** Production quality ⭐

- "Write code" → Gets well-architected, tested, documented code
- Suitable for real projects

**Tier 4/5:** Expert-level

- "Write code" → Gets enterprise-grade, scalable, maintainable code
- Suitable for critical systems

### 3. Mode-Specific Guidance ✅

**Developer Mode:**

- Focus on code quality, architecture, testing
- SOLID principles, design patterns
- Error handling, performance, security

**Planning Mode:**

- Focus on task breakdown, dependencies
- Risk assessment, estimation
- Clear acceptance criteria

**Assistant Mode:**

- Focus on clear communication
- Examples and explanations
- User preferences and context

**Debugger Mode:**

- Focus on systematic debugging
- Root cause analysis
- Reproduction steps and fixes

### 4. Token Efficiency ✅

**Small Contexts (Tier 1):**

- 200 tokens = 5.0% overhead
- 3,800 tokens workspace = 95.0%
- **Maximizes available space**

**Medium Contexts (Tier 3) ⭐:**

- 1,000 tokens = 3.1% overhead
- 31,000 tokens workspace = 96.9%
- **Excellent balance**

**Large Contexts (Tier 5):**

- 1,500 tokens = 1.2% overhead
- 126,500 tokens workspace = 98.8%
- **Minimal overhead, maximum guidance**

---

## Implementation

### Prompt Templates

Store prompts in separate files for maintainability:

```
packages/core/src/prompts/templates/
├── tier1/
│   ├── developer.txt
│   ├── planning.txt
│   ├── assistant.txt
│   └── debugger.txt
├── tier2/
│   ├── developer.txt
│   ├── planning.txt
│   ├── assistant.txt
│   └── debugger.txt
├── tier3/
│   ├── developer.txt
│   ├── planning.txt
│   ├── assistant.txt
│   └── debugger.txt
└── tier4/
    ├── developer.txt
    ├── planning.txt
    ├── assistant.txt
    └── debugger.txt
```

### Automatic Prompt Selection

```typescript
function getSystemPromptForTierAndMode(tier: ContextTier, mode: OperationalMode): string {
  const tierNumber = getTierNumber(tier); // 1-5
  const template = loadPromptTemplate(tierNumber, mode);

  if (!template) {
    // Fallback to tier 3 developer
    return loadPromptTemplate(3, 'developer');
  }

  return template;
}
```

### Automatic Updates

**When tier changes:**

```typescript
private onTierChange(newTier: ContextTier): void {
  this.currentTier = newTier;
  this.updateSystemPrompt();
  this.emit('tier-changed', { tier: newTier });
}
```

**When mode changes:**

```typescript
public setMode(mode: OperationalMode): void {
  this.currentMode = mode;
  this.updateSystemPrompt();
  this.emit('mode-changed', { mode });
}
```

**Update system prompt:**

```typescript
public updateSystemPrompt(): void {
  const newPrompt = this.getSystemPromptForTierAndMode();
  this.setSystemPrompt(newPrompt);

  this.emit('system-prompt-updated', {
    tier: this.currentTier,
    mode: this.currentMode,
    tokenBudget: this.getSystemPromptTokenBudget()
  });
}
```

---

## Configuration

### Prompt Config

```typescript
interface PromptConfig {
  tier: ContextTier;
  mode: OperationalMode;
  customTemplates?: Record<string, string>;
  enableGoals: boolean;
  enableSkills: boolean;
  enableSanityChecks: boolean;
}
```

### Default Values

```typescript
const DEFAULT_PROMPT_CONFIG = {
  tier: ContextTier.TIER_3_STANDARD,
  mode: OperationalMode.DEVELOPER,
  customTemplates: undefined,
  enableGoals: true,
  enableSkills: true,
  enableSanityChecks: true,
};
```

---

## Events

### Prompt Events

- `system-prompt-updated` - System prompt changed
- `tier-changed` - Context tier changed
- `mode-changed` - Operational mode changed
- `goal-added` - Goal added to prompt
- `goal-removed` - Goal removed from prompt
- `skill-activated` - Skill added to prompt
- `skill-deactivated` - Skill removed from prompt

---

## Best Practices

### 1. Prompt Selection

- Start with Tier 3 (16K) for most tasks
- Use Tier 2 (8K) for quick conversations
- Use Tier 1 (2K, 4K) for minimal context needs
- Use Tier 4 (32K) for large codebases
- Use Tier 5 (64K, 128K) only when necessary

### 2. Mode Selection

- Use Developer mode for coding tasks
- Use Planning mode for task organization
- Use Assistant mode for general questions
- Use Debugger mode for troubleshooting

### 3. Goals and Skills

- Set clear goals at conversation start
- Activate relevant skills for domain expertise
- Goals are preserved across compressions
- Skills add specialized knowledge

---

## Troubleshooting

### Poor Output Quality

**Symptom:** LLM produces low-quality output

**Solutions:**

1. Check current tier (should be Tier 3+ for complex tasks)
2. Verify mode matches task (developer for code, planning for tasks)
3. Ensure system prompt is loaded correctly
4. Review prompt template for completeness

### Prompt Too Large

**Symptom:** System prompt consumes too much context

**Solutions:**

1. Verify tier is appropriate for context size
2. Check for duplicate content in prompt
3. Review custom templates for efficiency
4. Consider using smaller tier if context is limited

---

## File Locations

| File                                               | Purpose                  |
| -------------------------------------------------- | ------------------------ |
| `packages/core/src/context/SystemPromptBuilder.ts` | Builds system prompts    |
| `packages/core/src/prompts/PromptRegistry.ts`      | Manages prompt templates |
| `packages/core/src/prompts/templates/`             | Prompt template files    |
| `packages/core/src/prompts/types.ts`               | Type definitions         |
| `packages/core/src/context/contextManager.ts`      | Uses system prompts      |

---

**Note:** This document focuses on system prompt architecture. For context sizing logic, see `ContextManagement.md`. For compression and snapshots, see `ContextCompression.md`.

<!-- Alpha Release: v0.1.6 -->

