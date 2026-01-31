# Hook Protocol Specification

**Technical Specification for Hook Communication Protocol**

This document defines the complete protocol for hook communication in OLLM CLI.

---

## 📋 Table of Contents

1. [Overview](#overview)
2. [Communication Protocol](#communication-protocol)
3. [Input Specification](#input-specification)
4. [Output Specification](#output-specification)
5. [Event Types](#event-types)
6. [Error Handling](#error-handling)
7. [Security Considerations](#security-considerations)

**See Also:**

- [Hook System Overview](3%20projects/OLLM%20CLI/Hooks/README.md) - Hook system introduction
- [Hook User Guide](3%20projects/OLLM%20CLI/Hooks/user-guide.md) - Using hooks
- [Hook Development Guide](3%20projects/OLLM%20CLI/Hooks/development-guide.md) - Creating hooks
- [MCP Architecture](MCP_architecture.md) - System architecture

---

## Overview

### Protocol Summary

The hook protocol defines how OLLM CLI communicates with hooks:

- **Transport:** Standard input/output (stdin/stdout)
- **Format:** JSON
- **Encoding:** UTF-8
- **Direction:** Bidirectional (request/response)

### Design Principles

1. **Simplicity** - Easy to implement in any language
2. **Extensibility** - Support for future event types
3. **Security** - Safe by default
4. **Performance** - Minimal overhead

---

## Communication Protocol

### Message Flow

```
┌─────────────┐                    ┌──────────┐
│  OLLM CLI   │                    │   Hook   │
└──────┬──────┘                    └────┬─────┘
       │                                │
       │  1. Write JSON to stdin        │
       │───────────────────────────────>│
       │                                │
       │  2. Hook processes event       │
       │                                │
       │  3. Read JSON from stdout      │
       │<───────────────────────────────│
       │                                │
       │  4. Process response           │
       │                                │
```

### Execution Model

1. **Event occurs** in OLLM CLI
2. **Hook registry** finds matching hooks
3. **Trust check** determines if approval needed
4. **Approval requested** (if needed)
5. **Hook executed** with event data on stdin
6. **Hook processes** event and returns response on stdout
7. **Response processed** by OLLM CLI
8. **Execution continues** or blocks based on response

### Timeout

- **Default timeout:** 5 seconds
- **Configurable:** Per hook or globally
- **On timeout:** Hook is terminated, operation blocked

---

## Input Specification

### Input Schema

```json
{
  "event": "string (required)",
  "context": {
    "...": "event-specific data (required)"
  },
  "metadata": {
    "timestamp": "ISO 8601 string (optional)",
    "hookName": "string (optional)",
    "hookVersion": "string (optional)",
    "sessionId": "string (optional)"
  }
}
```

### Field Definitions

#### event (required)

- **Type:** string
- **Description:** Event type identifier
- **Values:** See [Event Types](#event-types)
- **Example:** `"pre-tool-call"`

#### context (required)

- **Type:** object
- **Description:** Event-specific context data
- **Contents:** Varies by event type
- **Example:** `{"tool": "read-file", "args": {...}}`

#### metadata (optional)

- **Type:** object
- **Description:** Additional metadata
- **Fields:**
  - `timestamp` - ISO 8601 timestamp
  - `hookName` - Name of the hook
  - `hookVersion` - Version of the hook
  - `sessionId` - Current session ID

### Example Input

```json
{
  "event": "pre-tool-call",
  "context": {
    "tool": "read-file",
    "args": {
      "path": "README.md"
    },
    "session": {
      "id": "abc123",
      "user": "developer",
      "startTime": "2026-01-16T10:00:00Z"
    }
  },
  "metadata": {
    "timestamp": "2026-01-16T10:05:30Z",
    "hookName": "safety-check",
    "hookVersion": "1.0.0",
    "sessionId": "abc123"
  }
}
```

---

## Output Specification

### Output Schema

```json
{
  "allow": "boolean (required)",
  "message": "string (optional)",
  "metadata": {
    "...": "any additional data (optional)"
  }
}
```

### Field Definitions

#### allow (required)

- **Type:** boolean
- **Description:** Whether to allow the operation
- **Values:**
  - `true` - Allow operation to proceed
  - `false` - Block operation
- **Example:** `true`

#### message (optional)

- **Type:** string
- **Description:** Human-readable message
- **Use Cases:**
  - Success message
  - Error explanation
  - Warning message
- **Example:** `"Operation approved"`

#### metadata (optional)

- **Type:** object
- **Description:** Additional data to return
- **Contents:** Any JSON-serializable data
- **Use Cases:**
  - Execution metrics
  - Modified data
  - Debug information
- **Example:** `{"duration": 0.05, "checked": true}`

### Example Output

**Success:**

```json
{
  "allow": true,
  "message": "Operation approved",
  "metadata": {
    "checked": true,
    "duration": 0.05,
    "timestamp": "2026-01-16T10:05:30Z"
  }
}
```

**Blocked:**

```json
{
  "allow": false,
  "message": "Dangerous operation detected",
  "metadata": {
    "reason": "rm -rf detected",
    "severity": "critical"
  }
}
```

**Error:**

```json
{
  "allow": false,
  "message": "Hook execution failed: Invalid input",
  "metadata": {
    "error": "ValidationError",
    "code": "INVALID_INPUT"
  }
}
```

---

## Event Types

### pre-execution

**When:** Before LLM execution starts

**Context:**

```json
{
  "prompt": "string",
  "session": {
    "id": "string",
    "user": "string",
    "startTime": "ISO 8601"
  },
  "config": {
    "model": "string",
    "temperature": "number"
  }
}
```

### post-execution

**When:** After LLM execution completes

**Context:**

```json
{
  "prompt": "string",
  "response": "string",
  "session": {
    "id": "string",
    "user": "string"
  },
  "metrics": {
    "duration": "number (seconds)",
    "tokens": "number"
  }
}
```

### pre-tool-call

**When:** Before a tool is executed

**Context:**

```json
{
  "tool": "string",
  "args": "object",
  "session": {
    "id": "string",
    "user": "string"
  }
}
```

### post-tool-call

**When:** After a tool is executed

**Context:**

```json
{
  "tool": "string",
  "args": "object",
  "result": "any",
  "metrics": {
    "duration": "number (seconds)",
    "success": "boolean"
  },
  "session": {
    "id": "string"
  }
}
```

### on-error

**When:** When an error occurs

**Context:**

```json
{
  "error": {
    "message": "string",
    "code": "string",
    "stack": "string"
  },
  "operation": "string",
  "session": {
    "id": "string"
  }
}
```

### on-file-change

**When:** When a file is modified

**Context:**

```json
{
  "file": "string (path)",
  "changes": {
    "type": "string (created|modified|deleted)",
    "size": "number (bytes)",
    "timestamp": "ISO 8601"
  },
  "session": {
    "id": "string"
  }
}
```

### on-git-commit

**When:** Before a git commit

**Context:**

```json
{
  "files": ["string (paths)"],
  "message": "string (commit message)",
  "branch": "string",
  "session": {
    "id": "string"
  }
}
```

### on-session-start

**When:** When a session starts

**Context:**

```json
{
  "session": {
    "id": "string",
    "user": "string",
    "startTime": "ISO 8601",
    "config": "object"
  }
}
```

### on-session-end

**When:** When a session ends

**Context:**

```json
{
  "session": {
    "id": "string",
    "user": "string",
    "startTime": "ISO 8601",
    "endTime": "ISO 8601",
    "duration": "number (seconds)",
    "messageCount": "number"
  }
}
```

### on-context-overflow

**When:** When context is full

**Context:**

```json
{
  "session": {
    "id": "string"
  },
  "context": {
    "size": "number (tokens)",
    "maxSize": "number (tokens)",
    "utilization": "number (0-1)"
  }
}
```

### on-approval-request

**When:** When approval is needed

**Context:**

```json
{
  "operation": "string",
  "details": "object",
  "requester": "string",
  "session": {
    "id": "string"
  }
}
```

### custom

**When:** Custom events triggered by extensions

**Context:**

```json
{
  "eventName": "string",
  "data": "any",
  "source": "string (extension name)",
  "session": {
    "id": "string"
  }
}
```

---

## Error Handling

### Hook Errors

**Hook execution fails:**

- Hook returns non-zero exit code
- Hook times out
- Hook produces invalid JSON
- Hook crashes

**Behavior:**

- Operation is blocked (`allow: false`)
- Error message is logged
- User is notified
- Execution continues with next hook

### Invalid Input

**Hook receives invalid input:**

- Missing required fields
- Invalid JSON
- Unexpected event type

**Recommended behavior:**

- Return `allow: false`
- Include error message
- Log error details

**Example:**

```json
{
  "allow": false,
  "message": "Invalid input: missing required field 'event'",
  "metadata": {
    "error": "ValidationError",
    "field": "event"
  }
}
```

### Invalid Output

**Hook produces invalid output:**

- Invalid JSON
- Missing required fields
- Wrong data types

**Behavior:**

- Operation is blocked
- Error is logged
- User is notified

### Timeout Handling

**Hook exceeds timeout:**

- Hook process is terminated
- Operation is blocked
- Timeout error is logged

**Recommended timeout handling in hooks:**

```javascript
// Set internal timeout
const timeout = setTimeout(() => {
  console.log(
    JSON.stringify({
      allow: false,
      message: 'Hook timed out',
    })
  );
  process.exit(1);
}, 4000); // 4 seconds (before 5 second system timeout)

// Clear timeout on completion
clearTimeout(timeout);
```

---

## Security Considerations

### Input Validation

**Always validate:**

- Event type is expected
- Required fields are present
- Data types are correct
- Values are within expected ranges

**Example:**

```javascript
function validateInput(input) {
  if (!input.event) {
    throw new Error('Missing event field');
  }
  if (!input.context) {
    throw new Error('Missing context field');
  }
  // More validation...
}
```

### Output Sanitization

**Always sanitize:**

- Remove sensitive data
- Escape special characters
- Limit message length
- Validate JSON structure

### Command Injection

**Prevent command injection:**

- Never use `eval()` or `exec()`
- Sanitize file paths
- Validate command arguments
- Use safe APIs

**Bad:**

```bash
# DON'T DO THIS
file=$(echo "$input" | jq -r '.context.file')
eval "cat $file"  # DANGEROUS!
```

**Good:**

```bash
# DO THIS
file=$(echo "$input" | jq -r '.context.file')
cat "$file"  # Safe
```

### Resource Limits

**Implement limits:**

- Maximum execution time
- Maximum memory usage
- Maximum file size
- Maximum output size

### Privilege Separation

**Run with minimal privileges:**

- Don't run as root
- Use dedicated user account
- Limit file system access
- Restrict network access

---

## Versioning

### Protocol Version

Current version: **1.0.0**

### Version Negotiation

Future versions may include version field:

```json
{
  "version": "1.0.0",
  "event": "pre-tool-call",
  "context": {...}
}
```

### Backward Compatibility

- New fields are optional
- Existing fields maintain meaning
- Deprecated fields are documented
- Breaking changes require major version bump

---

## Best Practices

### Performance

**Do:**

- ✅ Process input efficiently
- ✅ Return response quickly (< 1 second)
- ✅ Use streaming for large data
- ✅ Cache results when appropriate

**Don't:**

- ❌ Make blocking network calls
- ❌ Process large files synchronously
- ❌ Perform expensive computations
- ❌ Wait for user input

### Reliability

**Do:**

- ✅ Handle all error cases
- ✅ Validate all input
- ✅ Return valid JSON always
- ✅ Log errors appropriately

**Don't:**

- ❌ Crash on invalid input
- ❌ Return empty output
- ❌ Ignore errors silently
- ❌ Assume input is valid

### Security

**Do:**

- ✅ Validate and sanitize input
- ✅ Use safe APIs
- ✅ Limit resource usage
- ✅ Run with minimal privileges

**Don't:**

- ❌ Execute arbitrary code
- ❌ Trust user input
- ❌ Use eval or exec
- ❌ Access sensitive files

---

## Examples

### Minimal Hook

```bash
#!/bin/bash
cat | jq '{allow: true, message: "OK"}'
```

### Complete Hook

```javascript
#!/usr/bin/env node

const fs = require('fs');

try {
  // Read and parse input
  const input = JSON.parse(fs.readFileSync(0, 'utf-8'));

  // Validate input
  if (!input.event || !input.context) {
    throw new Error('Invalid input');
  }

  // Process event
  const result = processEvent(input);

  // Return response
  console.log(
    JSON.stringify({
      allow: result.success,
      message: result.message,
      metadata: {
        duration: result.duration,
        timestamp: new Date().toISOString(),
      },
    })
  );
} catch (error) {
  // Return error response
  console.log(
    JSON.stringify({
      allow: false,
      message: `Error: ${error.message}`,
      metadata: {
        error: error.name,
        stack: error.stack,
      },
    })
  );
  process.exit(1);
}

function processEvent(input) {
  const start = Date.now();
  // Your logic here
  return {
    success: true,
    message: 'Success',
    duration: (Date.now() - start) / 1000,
  };
}
```

---

## Further Reading

### Documentation

- [Hook System Overview](3%20projects/OLLM%20CLI/Hooks/README.md) - Introduction
- [Hook User Guide](3%20projects/OLLM%20CLI/Hooks/user-guide.md) - Using hooks
- [Hook Development Guide](3%20projects/OLLM%20CLI/Hooks/development-guide.md) - Creating hooks
- [MCP Architecture](MCP_architecture.md) - System architecture

### Standards

- JSON Specification (https://www.json.org/)
- ISO 8601 (Timestamps) (https://en.wikipedia.org/wiki/ISO_8601)
- UTF-8 Encoding (https://en.wikipedia.org/wiki/UTF-8)

### Tools

- jq Manual (https://stedolan.github.io/jq/manual/)
- JSON Schema (https://json-schema.org/)
- JSON Validator (https://jsonlint.com/)

---

**Last Updated:** 2026-01-16  
**Version:** 1.0.0  
**Protocol Version:** 1.0.0

<!-- Alpha Release: v0.1.6 -->

