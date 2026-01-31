# Memory System - User Guide

**Using Persistent Memory**

---

## Table of Contents

1. [Introduction](#introduction)
2. [Basic Operations](#basic-operations)
3. [Categories](#categories)
4. [System Prompt Injection](#system-prompt-injection)
5. [LLM Integration](#llm-integration)
6. [Best Practices](#best-practices)

---

## Introduction

The Memory System lets you store information that persists across sessions. The model automatically has access to your memories, providing context without you having to repeat yourself.

---

## Basic Operations

### Add Memory

```bash
/memory add key value
```

**Examples:**

```bash
/memory add user_name Alice
/memory add project_name my-web-app
/memory add preferred_style "functional programming"
```

### Recall Memory

```bash
/memory recall key
```

**Examples:**

```bash
/memory recall user_name
# Returns: Alice
```

### Search Memories

```bash
/memory search query
```

**Examples:**

```bash
/memory search project
# Returns all memories with "project" in key or value
```

### List All Memories

```bash
/memory list
```

**Output:**

```
user_name: Alice (fact, accessed 5 times)
project_name: my-web-app (context, accessed 3 times)
preferred_style: functional programming (preference, accessed 2 times)
```

### Forget Memory

```bash
/memory forget key
```

**Examples:**

```bash
/memory forget old_preference
```

### Clear All Memories

```bash
/memory clear
```

**Warning:** This deletes all memories permanently!

---

## Categories

### Fact Category

**Purpose:** Objective information

**Examples:**

```bash
/memory add user_name Alice --category fact
/memory add birth_year 1990 --category fact
/memory add company_name "Acme Corp" --category fact
```

### Preference Category

**Purpose:** User preferences and choices

**Examples:**

```bash
/memory add preferred_language TypeScript --category preference
/memory add code_style "functional" --category preference
/memory add editor VSCode --category preference
```

### Context Category

**Purpose:** Current work and temporary information

**Examples:**

```bash
/memory add working_on "authentication system" --category context
/memory add current_task "fix login bug" --category context
/memory add deadline "2026-01-20" --category context
```

---

## System Prompt Injection

Memories are automatically injected into the system prompt:

**Configuration:**

```yaml
memory:
  enabled: true
  system_prompt_budget: 500 # tokens
```

**How it works:**

1. Memories sorted by priority (access count + recency)
2. Highest priority memories included first
3. Stops when token budget reached

**Example injection:**

```
user_name: Alice
preferred_language: TypeScript
project_name: my-web-app
working_on: authentication system
```

---

## LLM Integration

The model can remember things for you:

**Example conversation:**

```
You: My name is Alice
Model: Nice to meet you, Alice! I'll remember that.
[Model automatically calls /remember tool]

You: What's my name?
Model: Your name is Alice.
[Model recalls from memory]
```

**How it works:**

- Model has access to `/remember` tool
- Can store memories during conversation
- Memories marked with source='llm'

---

## Best Practices

### 1. Use Descriptive Keys

**Good:**

```bash
/memory add user_preferred_language TypeScript
/memory add project_main_framework React
```

**Bad:**

```bash
/memory add lang TS
/memory add fw R
```

### 2. Categorize Appropriately

**Facts:** Objective, unchanging information
**Preferences:** User choices and styles
**Context:** Current work, temporary info

### 3. Keep Values Concise

**Good:**

```bash
/memory add preferred_style "functional programming"
```

**Bad:**

```bash
/memory add preferred_style "I prefer functional programming because it's more maintainable and easier to test and..."
```

### 4. Update Context Regularly

```bash
# Start of day
/memory add working_on "authentication system"

# End of day
/memory add working_on "user profile page"
```

### 5. Clean Up Old Memories

```bash
# Remove completed tasks
/memory forget old_task

# Remove outdated context
/memory forget previous_project
```

---

## Examples

### Example 1: Personal Information

```bash
/memory add user_name Alice --category fact
/memory add user_role "Senior Developer" --category fact
/memory add user_company "Acme Corp" --category fact
```

### Example 2: Project Context

```bash
/memory add project_name my-web-app --category context
/memory add project_tech "React + TypeScript" --category context
/memory add project_status "in development" --category context
```

### Example 3: Preferences

```bash
/memory add code_style functional --category preference
/memory add test_framework Jest --category preference
/memory add naming_convention camelCase --category preference
```

---

## Troubleshooting

### Memories Not Injected

**Problem:** Model doesn't seem to know memories

**Solution 1:** Check if enabled

```yaml
memory:
  enabled: true
```

**Solution 2:** Increase budget

```yaml
memory:
  system_prompt_budget: 1000
```

### Too Many Memories

**Problem:** Token budget exceeded, some memories not included

**Solution:** Remove less important memories

```bash
/memory forget old_memory
```

### Memory File Corrupted

**Problem:** Can't load memories

**Solution:** Check file at `~/.ollm/memory.json`

```bash
# Backup
cp ~/.ollm/memory.json ~/.ollm/memory.json.backup

# Fix or recreate
```

---

## See Also

- [Memory API Reference](api-reference.md)
- [Configuration](Models_configuration.md)
- [Commands Reference](Models_commands.md)

---

**Document Version:** 1.0  
**Last Updated:** 2026-01-16

<!-- Alpha Release: v0.1.6 -->

