# Extension Manifest Reference

**Complete Schema for Extension Manifests**

---

## Schema

```json
{
  "name": "string (required)",
  "version": "string (required, semver)",
  "description": "string (required)",
  "author": "string (required)",
  "license": "string (optional)",
  "homepage": "string (optional, URL)",
  "repository": "string (optional, URL)",
  "keywords": ["string (optional)"],
  "components": {
    "skills": ["string (glob patterns)"],
    "settings": ["string (glob patterns)"],
    "servers": ["string (glob patterns)"],
    "hooks": ["string (glob patterns)"]
  },
  "permissions": {
    "filesystem": ["read", "write"],
    "network": ["https://domain.com"],
    "tools": ["tool-name"],
    "hooks": ["event-name"],
    "settings": ["read", "write"]
  },
  "dependencies": {
    "extension-name": "version"
  }
}
```

---

## Field Reference

### name (required)

- **Type:** string
- **Format:** kebab-case
- **Example:** `"github-integration"`

### version (required)

- **Type:** string
- **Format:** Semantic versioning (X.Y.Z)
- **Example:** `"1.2.0"`

### description (required)

- **Type:** string
- **Length:** 50-200 characters
- **Example:** `"GitHub integration with MCP server and hooks"`

### author (required)

- **Type:** string
- **Format:** Name or Name <email>
- **Example:** `"John Doe <john@example.com>"`

### components (optional)

- **Type:** object
- **Fields:** skills, settings, servers, hooks
- **Values:** Array of glob patterns
- **Example:**

```json
{
  "skills": ["skills/*.md"],
  "hooks": ["hooks/*.sh"]
}
```

### permissions (optional)

- **Type:** object
- **Fields:** filesystem, network, tools, hooks, settings
- **Example:**

```json
{
  "filesystem": ["read", "write"],
  "network": ["https://api.github.com"]
}
```

---

## Examples

### Minimal Extension

```json
{
  "name": "simple-extension",
  "version": "1.0.0",
  "description": "A simple extension",
  "author": "Developer"
}
```

### Complete Extension

```json
{
  "name": "github-integration",
  "version": "1.2.0",
  "description": "Complete GitHub integration",
  "author": "OLLM Team <team@ollm.dev>",
  "license": "MIT",
  "homepage": "https://github.com/ollm/ext-github",
  "repository": "https://github.com/ollm/ext-github",
  "keywords": ["github", "git", "integration"],
  "components": {
    "skills": ["skills/*.md"],
    "settings": ["settings/github.json"],
    "servers": ["servers/github-server.js"],
    "hooks": ["hooks/*.sh"]
  },
  "permissions": {
    "filesystem": ["read", "write"],
    "network": ["https://api.github.com"],
    "tools": ["read-file", "write-file", "shell"],
    "hooks": ["pre-execution", "on-git-commit"]
  },
  "dependencies": {
    "base-tools": "^1.0.0"
  }
}
```

---

**Last Updated:** 2026-01-16  
**Version:** 0.1.6

<!-- Alpha release v0.1.6 -->

