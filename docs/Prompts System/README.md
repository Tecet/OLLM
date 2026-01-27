# Prompts System Documentation

**Last Updated:** January 26, 2026

Welcome to the Prompts System documentation for OLLM CLI. This section covers system prompts, prompt templates, and prompt routing.

---

## 📚 Documentation Overview

### Core Documentation
- **[Architecture](Architecture.md)** - System architecture and design (Coming Soon)
- **[System Prompts](SystemPrompts.md)** - System prompt construction
- **[Prompt Templates](PromptsTemplates.md)** - Template system
- **[Prompt Routing](PromptsRouting.md)** - Routing and modes

---

## 🎯 What is the Prompts System?

The **Prompts System** manages how instructions are sent to the LLM, including system prompts, templates, and routing:

### 1. **System Prompts**
Core instructions that define LLM behavior:
- Identity and capabilities
- Tool usage instructions
- Context management rules
- Response formatting
- Safety guidelines

### 2. **Prompt Templates**
Reusable prompt patterns:
- Task-specific templates
- Variable substitution
- Template composition
- Template library

### 3. **Prompt Routing**
Intelligent prompt selection:
- Mode-based routing (chat, code, planning)
- Context-aware selection
- Dynamic prompt assembly
- Optimization strategies

---

## 📖 Documentation Structure

```
docs/Prompts System/
├── README.md                    ← You are here
├── Index.md                     Quick reference with links
├── Architecture.md              System architecture (Coming Soon)
├── SystemPrompts.md             System prompt construction
├── PromptsTemplates.md          Template system
└── PromptsRouting.md            Routing and modes
```

---

## 🎓 Key Concepts

### System Prompt Construction
System prompts are built from multiple components:

**Core Components:**
- **Identity** - Who the assistant is
- **Capabilities** - What it can do
- **Tools** - Available tools and usage
- **Context** - Current context information
- **Rules** - Behavioral guidelines

**Assembly Process:**
1. Load base system prompt
2. Add tool descriptions
3. Inject context information
4. Add mode-specific instructions
5. Apply optimizations

**See:** [System Prompts](SystemPrompts.md)

### Prompt Templates
Reusable patterns for common tasks:

**Template Types:**
- **Task Templates** - Specific tasks (code review, debugging)
- **Mode Templates** - Mode-specific prompts
- **Workflow Templates** - Multi-step workflows
- **Custom Templates** - User-defined templates

**Template Features:**
- Variable substitution (`{{variable}}`)
- Conditional sections
- Template inheritance
- Composition

**See:** [Prompt Templates](PromptsTemplates.md)

### Prompt Routing
Intelligent selection of prompts:

**Routing Modes:**
- **Chat Mode** - General conversation
- **Code Mode** - Code-focused assistance
- **Planning Mode** - Task planning and breakdown
- **Review Mode** - Code review and analysis

**Routing Logic:**
1. Detect user intent
2. Select appropriate mode
3. Load mode-specific prompt
4. Apply context
5. Optimize for model

**See:** [Prompt Routing](PromptsRouting.md)

---

## 💡 Common Use Cases

### Using System Prompts
```bash
# View current system prompt
/prompt show

# Reload system prompt
/prompt reload

# View prompt stats
/prompt stats
```

### Using Templates
```bash
# List available templates
/template list

# Use a template
/template use code-review

# Create custom template
/template create my-template
```

### Switching Modes
```bash
# Switch to code mode
/mode code

# Switch to planning mode
/mode planning

# Return to chat mode
/mode chat
```

**Learn more:** [Commands](../UI&Settings/Commands.md#prompt-commands)

---

## 🛠️ Configuration

### System Prompt Settings
```yaml
prompts:
  # System prompt file
  systemPromptFile: system-prompt.md
  
  # Enable dynamic optimization
  dynamicOptimization: true
  
  # Include tool descriptions
  includeTools: true
  
  # Include context information
  includeContext: true
```

### Template Settings
```yaml
templates:
  # Template directories
  directories:
    - .ollm/templates/      # Workspace templates
    - ~/.ollm/templates/    # User templates
  
  # Enable template caching
  cacheTemplates: true
```

### Routing Settings
```yaml
routing:
  # Default mode
  defaultMode: chat
  
  # Enable auto-detection
  autoDetectMode: true
  
  # Mode-specific settings
  modes:
    code:
      temperature: 0.3
    planning:
      temperature: 0.7
```

**Learn more:** [Configuration](../UI&Settings/Configuration.md)

---

## 🔍 Troubleshooting

### Common Issues

**System prompt not loading:**
- Check file exists: `system-prompt.md`
- Verify file permissions
- Check syntax errors
- Reload: `/prompt reload`

**Template not found:**
- List templates: `/template list`
- Check template directory
- Verify template name
- Check file extension (.md)

**Mode not switching:**
- Check mode exists: `/mode list`
- Verify mode configuration
- Check for errors: `/mode status`
- Reset to default: `/mode chat`

**Prompt too long:**
- Enable optimization: `/config set prompts.dynamicOptimization true`
- Reduce context: `/context compact`
- Use shorter templates
- Disable unnecessary sections

**See:** [Troubleshooting Guide](../troubleshooting.md)

---

## 📊 Implementation Status

### Current (v0.1.0)
- ✅ System Prompt Construction
- ✅ Tool Description Injection
- ✅ Context Information Injection
- ✅ Basic Template System
- ✅ Mode Switching (chat, code, planning)
- ✅ Prompt Commands

### Planned (v0.2.0)
- ⏳ Advanced Template System
- ⏳ Template Marketplace
- ⏳ Auto Mode Detection
- ⏳ Prompt Analytics

### Planned (v0.3.0)
- ⏳ Prompt Optimization
- ⏳ A/B Testing
- ⏳ Prompt Versioning

---

## 🤝 Related Documentation

### Core Systems
- [Context Management](../Context/ContextManagment.md) - Context system
- [Tools System](../Tools/README.md) - Tool execution
- [Model Management](../LLM%20Models/README.md) - Model selection

### Commands
- [Prompt Commands](../UI&Settings/Commands.md#prompt-commands) - CLI commands
- [Template Commands](../UI&Settings/Commands.md#template-commands) - Template management
- [Mode Commands](../UI&Settings/Commands.md#mode-commands) - Mode switching

### Developer Resources
- Knowledge DB: `dev_PromptSystem.md` - Prompt system architecture

---

## 🎯 Quick Start

### For New Users

1. **View System Prompt**
   ```bash
   /prompt show
   ```

2. **List Templates**
   ```bash
   /template list
   ```

3. **Try Different Modes**
   ```bash
   /mode code
   /mode planning
   /mode chat
   ```

### For Advanced Users

1. **Create Custom Template**
   ```bash
   /template create my-workflow
   ```

2. **Optimize Prompts**
   ```bash
   /config set prompts.dynamicOptimization true
   ```

3. **Configure Modes**
   ```bash
   /config set routing.modes.code.temperature 0.2
   ```

---

## 📈 Best Practices

### System Prompts

1. **Keep It Focused** - Clear, specific instructions
2. **Include Examples** - Show desired behavior
3. **Update Regularly** - Refine based on usage
4. **Test Thoroughly** - Verify behavior with different models
5. **Version Control** - Track changes over time

### Templates

1. **Descriptive Names** - Clear, meaningful names
2. **Good Documentation** - Explain template purpose
3. **Use Variables** - Make templates reusable
4. **Test Variations** - Try different inputs
5. **Share Useful Templates** - Contribute to community

### Modes

1. **Choose Appropriate Mode** - Match mode to task
2. **Customize Settings** - Adjust temperature, etc.
3. **Create Custom Modes** - For specific workflows
4. **Monitor Performance** - Track mode effectiveness

---

**Last Updated:** January 26, 2026  
**Version:** 0.1.0  
**Status:** Active Development
