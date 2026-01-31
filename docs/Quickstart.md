# Quick Start Guide

Get up and running with OLLM CLI in 5 minutes! This guide will walk you through your first conversation with the AI.

---

## Before You Start

Make sure you've [installed OLLM CLI](Installation.md). You should have:

- ✅ OLLM CLI installed (`ollm --version` works)
- ✅ Ollama running (`ollama list` shows models)
- ✅ At least one model downloaded

If not, go back to the [Installation Guide](Installation.md).

---

## Your First Conversation

### 1. Start OLLM CLI

Open your terminal and type:

```bash
ollm
```

You'll see the OLLM CLI interface:

```
┌─────────────────────────────────────────────────────────────────┐
│ 🦙 llama3.2:3b │ 0/8K │ 🎯 Assistant │ 🕐 14:32:15            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ Welcome to OLLM CLI!                                            │
│                                                                  │
│ Type your message and press Return to chat.                    │
│ Press Ctrl+K for commands, Ctrl+? for help.                    │
│                                                                  │
│ > _                                                             │
├─────────────────────────────────────────────────────────────────┤
│ ● Ready | Ctrl+K: Commands | Ctrl+P: Panel | Ctrl+?: Help     │
└─────────────────────────────────────────────────────────────────┘
```

### 2. Say Hello

Type a message and press `Return`:

```
You: Hello! What can you help me with?
```

The AI will respond:

```
Assistant: Hello! I'm here to help you with a variety of tasks:

- Writing and debugging code
- Answering questions about programming
- Searching the web for information
- Reading and editing files in your workspace
- Running commands and automating tasks
- And much more!

What would you like to work on today?
```

### 3. Ask a Question

Try asking something:

```
You: What's the difference between let and const in JavaScript?
```

```
Assistant: Great question! Here's the key difference:

**let:**
- Creates a variable that can be reassigned
- Block-scoped (only exists within its { } block)
- Example: let count = 0; count = 1; // ✓ Works

**const:**
- Creates a variable that cannot be reassigned
- Also block-scoped
- Must be initialized when declared
- Example: const name = "Alice"; name = "Bob"; // ✗ Error

However, const doesn't make objects immutable:
const user = { name: "Alice" };
user.name = "Bob"; // ✓ Works (modifying property)
user = {}; // ✗ Error (reassigning variable)

Use const by default, and only use let when you need to reassign.
```

---

## Using Tools

The AI can use tools to help you. Let's try some:

### Reading Files

```
You: Read the package.json file
```

```
Assistant: [Uses read_file tool]

I can see your package.json file. Here's what I found:

{
  "name": "my-project",
  "version": "1.0.0",
  "dependencies": {
    "react": "^18.2.0",
    "typescript": "^5.0.0"
  }
}

You're using React 18.2.0 and TypeScript 5.0. Would you like me to
check if there are newer versions available?
```

### Searching the Web

```
You: Search for the latest React version
```

```
Assistant: [Uses web_search tool]

According to my search, React 19 was released in December 2024.
The latest stable version is 19.0.0.

Key new features in React 19:
- Actions for handling form submissions
- New 'use' hook for reading promises
- Improved error handling
- Better server components support

Would you like help upgrading your project to React 19?
```

### Running Commands

```
You: Run npm test
```

```
Assistant: [Uses shell tool]

Running tests...

> my-project@1.0.0 test
> vitest run

✓ src/utils.test.ts (5 tests) 234ms
✓ src/components/Button.test.tsx (3 tests) 156ms

Test Files  2 passed (2)
     Tests  8 passed (8)
  Start at  14:32:45
  Duration  1.2s

All tests passed! ✓
```

---

## Essential Keyboard Shortcuts

Learn these shortcuts to work faster:

| Shortcut       | Action               |
| -------------- | -------------------- |
| `Return`       | Send message         |
| `Shift+Return` | New line in message  |
| `Ctrl+K`       | Open command palette |
| `Ctrl+P`       | Toggle side panel    |
| `Ctrl+?`       | Show help            |
| `Ctrl+C`       | Exit OLLM CLI        |
| `Ctrl+L`       | Clear screen         |
| `Up Arrow`     | Previous message     |

---

## Using Slash Commands

Slash commands let you control OLLM CLI:

### Model Commands

```bash
# List available models
/model list

# Switch to a different model
/model use llama3.1:8b

# Get model information
/model info llama3.2:3b
```

### Context Commands

```bash
# Check context usage
/context stats

# Compress context
/compact

# Clear conversation
/clear
```

### Theme Commands

```bash
# List themes
/theme list

# Change theme
/theme use dracula-dark
```

### Help Commands

```bash
# Show all commands
/help

# Get help for specific command
/help model
```

---

## Working with Files

OLLM CLI can read and edit files in your workspace.

### Reading Files

```
You: Read the README.md file
```

The AI will use the `read_file` tool to read and show you the file.

### Editing Files

```
You: Add a new section to README.md about installation
```

The AI will:

1. Read the current file
2. Generate the new content
3. Ask for your approval
4. Edit the file

### Creating Files

```
You: Create a new file called utils.ts with a function to format dates
```

The AI will:

1. Generate the code
2. Show you what it will create
3. Ask for approval
4. Create the file

---

## Exploring the Interface

### Header Bar

```
┌─────────────────────────────────────────────────────────────────┐
│ 🦙 llama3.2:3b │ 1.2K/8K │ 🎯 Assistant │ 🕐 14:32:15        │
```

- **Model** - Current AI model
- **Context** - Memory usage (1.2K used of 8K total)
- **Mode** - Current mode (Assistant, Developer, Planning, Debugger)
- **Clock** - Current time

### Side Panel

Press `Ctrl+P` to open the side panel:

```
┌─ Side Panel ─────────────────────────────┐
│ [Tools] [Hooks] [Files] [MCP] [Settings] │
├───────────────────────────────────────────┤
│                                           │
│ Tools Tab:                                │
│ ☑ read_file    ☑ write_file              │
│ ☑ edit_file    ☑ glob                    │
│ ☑ grep         ☑ web_search              │
│ ☑ shell        ☑ web_fetch               │
│                                           │
└───────────────────────────────────────────┘
```

**Tabs:**

- **Tools** - Enable/disable tools
- **Hooks** - Automation hooks
- **Files** - File explorer
- **MCP** - External services
- **Settings** - Configuration

### Status Bar

```
│ ● Ready | Ctrl+K: Commands | Ctrl+P: Panel | Ctrl+?: Help     │
```

Shows current status and helpful shortcuts.

---

## Common Tasks

### Get Help with Code

```
You: I'm getting an error: "Cannot read property 'map' of undefined"
```

```
Assistant: This error means you're trying to use .map() on something
that's undefined. Common causes:

1. Data hasn't loaded yet
2. API request failed
3. Variable not initialized

Can you show me the code where this error occurs?
```

### Debug an Issue

```
You: Read the login.ts file and help me debug why login isn't working
```

```
Assistant: [Reads file]

I found a potential issue on line 23. You're not handling the case
where the API returns an error. Here's the fix:

[Shows code with fix]

Would you like me to apply this fix?
```

### Learn Something New

```
You: Explain how promises work in JavaScript with examples
```

```
Assistant: Promises are a way to handle asynchronous operations...

[Detailed explanation with code examples]

Would you like me to show you how to use async/await, which makes
promises easier to work with?
```

### Automate a Task

```
You: Run the tests, and if they pass, commit the changes with message "Fix login bug"
```

```
Assistant: [Runs tests]
All tests passed!

[Runs git commands]
Changes committed with message "Fix login bug"

Done! Your changes are committed and ready to push.
```

---

## Switching Modes

Different modes for different tasks:

### Assistant Mode (Default)

General help and conversation.

```bash
/mode assistant
```

### Developer Mode

Focused on coding tasks.

```bash
/mode developer
```

### Planning Mode

Project planning and architecture.

```bash
/mode planning
```

### Debugger Mode

Systematic problem-solving.

```bash
/mode debugger
```

---

## Tips for Better Results

### Be Specific

❌ **Vague:** "Fix my code"  
✅ **Specific:** "Fix the login function in auth.ts - it's not handling errors correctly"

### Provide Context

❌ **No context:** "How do I do this?"  
✅ **With context:** "I'm building a React app. How do I fetch data from an API and display it?"

### Break Down Complex Tasks

❌ **Too complex:** "Build a complete authentication system"  
✅ **Step by step:** "First, create a login form component. Then we'll add the API integration."

### Use Tools

❌ **Describe files:** "My package.json has React 18"  
✅ **Let AI read:** "Read package.json and check the React version"

---

## Next Steps

Now that you know the basics:

1. **[User Interface Guide](UI&Settings/UIGuide.md)** - Learn all interface features
2. **[Commands Reference](UI&Settings/Commands.md)** - Complete command list
3. **[Configuration Guide](UI&Settings/Configuration.md)** - Customize your setup
4. **[Tools Guide](Tools/UserGuide.md)** - Master the tool system

---

## Common Questions

### How do I exit?

Press `Ctrl+C` or type `/exit`

### How do I clear the conversation?

Type `/clear` to start fresh

### How do I change models?

Type `/model list` to see available models, then `/model use <name>`

### How do I save my conversation?

Type `/session save <name>` to save, `/session resume <name>` to load

### What if I make a mistake?

The AI will ask for confirmation before making changes to files. You can always say no!

### Can I undo changes?

Yes! Use `/undo` to revert the last file change

### How do I get help?

- Type `/help` for command list
- Press `Ctrl+?` for keyboard shortcuts
- See [Troubleshooting Guide](Troubleshooting.md) for issues

---

## Practice Exercise

Try this to practice what you've learned:

1. Start OLLM CLI: `ollm`
2. Ask: "What files are in the current directory?"
3. Ask: "Read the README.md file"
4. Ask: "Create a new file called hello.js with a simple hello world function"
5. Ask: "Run the file with node hello.js"
6. Type `/clear` to start fresh
7. Type `/exit` to quit

---

**Congratulations!** You now know the basics of OLLM CLI. Happy coding! 🎉

For more advanced features, explore the [complete documentation](README.md).

---

**Last Updated:** January 26, 2026  
**Version:** 0.1.6  
**Author:** tecet
