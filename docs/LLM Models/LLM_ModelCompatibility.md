# Model Compatibility Matrix

## Overview

This document provides compatibility information for various LLM models tested with OLLM CLI. It documents which features work with which models, known issues, workarounds, and recommendations for model selection based on your use case.

## Test Environment

- **OLLM CLI Version:** 0.1.6
- **Test Date:** 2026-01-15
- **Server:** Ollama (latest)
- **Test Framework:** Vitest with integration tests
- **Test Location:** `packages/test-utils/src/__tests__/modelCompatibility.integration.test.ts`

## How to Use This Document

- **✅ Pass** - Feature works as expected
- **❌ Fail** - Feature does not work
- **⚠️ Partial** - Feature works with limitations
- **⊘ Not Tested** - Feature not yet tested

## Summary

This compatibility matrix is maintained through automated testing. To run the compatibility tests yourself:

```bash
# Ensure Ollama server is running
ollama serve

# Pull the models you want to test
ollama pull llama3.1:8b
ollama pull codellama:7b
ollama pull phi3:mini

# Run the compatibility tests
npm test -- modelCompatibility.integration.test.ts
```

The tests will automatically detect which models are available and test them. Results are logged to the console and can be used to update this document.

## Model Categories

### General-Purpose Models

Best for: General conversation, question answering, content generation, multi-domain tasks

**Recommended Models:**

- `llama3.1:8b` - Excellent balance of capability and performance
- `llama3.2:3b` - Faster, good for simpler tasks
- `mistral:7b` - Strong reasoning capabilities

### Code-Specialized Models

Best for: Code generation, code review, technical documentation, debugging

**Recommended Models:**

- `codellama:7b` - Optimized for code tasks
- `deepseek-coder:6.7b` - Strong code understanding
- `starcoder2:7b` - Multi-language code support

### Small/Fast Models

Best for: Quick responses, resource-constrained environments, high-throughput scenarios

**Recommended Models:**

- `phi3:mini` - Excellent quality for size
- `gemma:2b` - Fast and efficient
- `tinyllama:1.1b` - Ultra-fast for simple tasks

## Detailed Compatibility Results

### llama3.1:8b (General-Purpose)

| Feature             | Status       | Notes                            |
| ------------------- | ------------ | -------------------------------- |
| Basic Chat          | ⊘ Not Tested | Requires real server for testing |
| Streaming           | ⊘ Not Tested | Requires real server for testing |
| Native Tool Calling | ⊘ Not Tested | Requires real server for testing |
| ReAct Fallback      | ⊘ Not Tested | Requires real server for testing |
| Context 4K          | ⊘ Not Tested | Requires real server for testing |
| Context 8K          | ⊘ Not Tested | Requires real server for testing |
| Context 16K         | ⊘ Not Tested | Requires real server for testing |
| Context 32K         | ⊘ Not Tested | Requires real server for testing |
| Context 64K         | ⊘ Not Tested | Requires real server for testing |
| Context 128K        | ⊘ Not Tested | Requires real server for testing |

**Known Issues:**

- Performance may degrade at context sizes above 32K tokens
- Tool calling requires specific prompt formatting

**Recommendations:**

- Best for general-purpose tasks with moderate context requirements
- Use for conversational AI, content generation, and question answering
- Consider smaller models (llama3.2:3b) for faster responses
- Use context sizes up to 16K for optimal performance

**Workarounds:**

- For large context: Use context compression or summarization
- For tool calling: Ensure proper tool schema formatting
- For performance: Reduce context window or use quantized versions

---

### llama3.2:3b (General-Purpose, Small)

| Feature             | Status       | Notes                            |
| ------------------- | ------------ | -------------------------------- |
| Basic Chat          | ⊘ Not Tested | Requires real server for testing |
| Streaming           | ⊘ Not Tested | Requires real server for testing |
| Native Tool Calling | ⊘ Not Tested | Requires real server for testing |
| ReAct Fallback      | ⊘ Not Tested | Requires real server for testing |
| Context 4K          | ⊘ Not Tested | Requires real server for testing |
| Context 8K          | ⊘ Not Tested | Requires real server for testing |
| Context 16K         | ⊘ Not Tested | Requires real server for testing |
| Context 32K         | ⊘ Not Tested | Requires real server for testing |
| Context 64K         | ⊘ Not Tested | Requires real server for testing |
| Context 128K        | ⊘ Not Tested | Requires real server for testing |

**Known Issues:**

- Smaller model may have reduced reasoning capabilities compared to 8B version
- May struggle with complex multi-step tasks

**Recommendations:**

- Best for faster responses when quality can be slightly reduced
- Good for simple conversational tasks and quick queries
- Use when response time is more important than maximum quality
- Ideal for resource-constrained environments

**Workarounds:**

- For complex tasks: Use llama3.1:8b instead
- For better quality: Provide more detailed prompts
- For tool calling: Use simpler tool schemas

---

### codellama:7b (Code-Specialized)

| Feature             | Status       | Notes                            |
| ------------------- | ------------ | -------------------------------- |
| Basic Chat          | ⊘ Not Tested | Requires real server for testing |
| Streaming           | ⊘ Not Tested | Requires real server for testing |
| Native Tool Calling | ⊘ Not Tested | Likely requires ReAct fallback   |
| ReAct Fallback      | ⊘ Not Tested | Requires real server for testing |
| Context 4K          | ⊘ Not Tested | Requires real server for testing |
| Context 8K          | ⊘ Not Tested | Requires real server for testing |
| Context 16K         | ⊘ Not Tested | Requires real server for testing |
| Context 32K         | ⊘ Not Tested | Requires real server for testing |
| Context 64K         | ⊘ Not Tested | Requires real server for testing |
| Context 128K        | ⊘ Not Tested | Requires real server for testing |

**Known Issues:**

- May not support native tool calling (requires ReAct fallback)
- Optimized for code, may be less effective for general conversation
- Tool calling may require specific prompt formatting

**Recommendations:**

- Best for code generation, code review, and technical tasks
- Use for debugging, refactoring, and code explanation
- Excellent for multi-language code support
- Consider for technical documentation generation

**Workarounds:**

- For tool calling: Use ReAct format instead of native tool calling
- For general chat: Use general-purpose model instead
- For better code quality: Provide code context and examples

---

### deepseek-coder:6.7b (Code-Specialized)

| Feature             | Status       | Notes                            |
| ------------------- | ------------ | -------------------------------- |
| Basic Chat          | ⊘ Not Tested | Requires real server for testing |
| Streaming           | ⊘ Not Tested | Requires real server for testing |
| Native Tool Calling | ⊘ Not Tested | Requires real server for testing |
| ReAct Fallback      | ⊘ Not Tested | Requires real server for testing |
| Context 4K          | ⊘ Not Tested | Requires real server for testing |
| Context 8K          | ⊘ Not Tested | Requires real server for testing |
| Context 16K         | ⊘ Not Tested | Requires real server for testing |
| Context 32K         | ⊘ Not Tested | Requires real server for testing |
| Context 64K         | ⊘ Not Tested | Requires real server for testing |
| Context 128K        | ⊘ Not Tested | Requires real server for testing |

**Known Issues:**

- May have different prompt format requirements than other models
- Tool calling support varies by version

**Recommendations:**

- Strong code understanding and generation capabilities
- Good for complex code refactoring tasks
- Use for code analysis and bug detection
- Excellent for explaining complex code

**Workarounds:**

- For tool calling: Test both native and ReAct formats
- For best results: Provide clear code context
- For performance: Use appropriate quantization level

---

### phi3:mini (Small/Fast)

| Feature             | Status       | Notes                            |
| ------------------- | ------------ | -------------------------------- |
| Basic Chat          | ⊘ Not Tested | Requires real server for testing |
| Streaming           | ⊘ Not Tested | Requires real server for testing |
| Native Tool Calling | ⊘ Not Tested | Requires real server for testing |
| ReAct Fallback      | ⊘ Not Tested | Requires real server for testing |
| Context 4K          | ⊘ Not Tested | Requires real server for testing |
| Context 8K          | ⊘ Not Tested | Requires real server for testing |
| Context 16K         | ⊘ Not Tested | Requires real server for testing |
| Context 32K         | ⊘ Not Tested | Requires real server for testing |
| Context 64K         | ⊘ Not Tested | Requires real server for testing |
| Context 128K        | ⊘ Not Tested | Requires real server for testing |

**Known Issues:**

- Smaller context window may limit use cases
- May struggle with very complex reasoning tasks
- Tool calling capabilities may be limited

**Recommendations:**

- Excellent quality-to-size ratio
- Best for quick responses and simple tasks
- Use when speed is critical
- Good for resource-constrained environments
- Ideal for high-throughput scenarios

**Workarounds:**

- For complex tasks: Use larger model
- For large context: Break into smaller chunks
- For tool calling: Use simpler tool schemas

---

### gemma:2b (Small/Fast)

| Feature             | Status       | Notes                            |
| ------------------- | ------------ | -------------------------------- |
| Basic Chat          | ⊘ Not Tested | Requires real server for testing |
| Streaming           | ⊘ Not Tested | Requires real server for testing |
| Native Tool Calling | ⊘ Not Tested | Requires real server for testing |
| ReAct Fallback      | ⊘ Not Tested | Requires real server for testing |
| Context 4K          | ⊘ Not Tested | Requires real server for testing |
| Context 8K          | ⊘ Not Tested | Requires real server for testing |
| Context 16K         | ⊘ Not Tested | Requires real server for testing |
| Context 32K         | ⊘ Not Tested | Requires real server for testing |
| Context 64K         | ⊘ Not Tested | Requires real server for testing |
| Context 128K        | ⊘ Not Tested | Requires real server for testing |

**Known Issues:**

- Very small model may have limited capabilities
- May not support advanced features like tool calling
- Context window is limited

**Recommendations:**

- Ultra-fast responses for simple queries
- Good for basic conversational tasks
- Use when minimal resource usage is required
- Suitable for embedded or edge deployments

**Workarounds:**

- For complex tasks: Use larger model
- For tool calling: May need to use larger model
- For better quality: Provide very clear, simple prompts

---

## Model Selection Guide

### By Use Case

**General Conversation & Q&A:**

- Primary: `llama3.1:8b`
- Fast alternative: `llama3.2:3b`
- Budget option: `phi3:mini`

**Code Generation & Review:**

- Primary: `codellama:7b`
- Alternative: `deepseek-coder:6.7b`
- Fast option: `phi3:mini` (for simple code tasks)

**Quick Responses & High Throughput:**

- Primary: `phi3:mini`
- Alternative: `gemma:2b`
- Balanced: `llama3.2:3b`

**Large Context Tasks:**

- Primary: `llama3.1:8b` (up to 32K)
- Alternative: Use context compression
- Note: Test performance at your required context size

**Tool Calling:**

- Native support: `llama3.1:8b`, `llama3.2:3b`
- ReAct fallback: `codellama:7b`, others
- Note: All models can use ReAct format

### By Resource Constraints

**High Memory Available (16GB+ VRAM):**

- Use 8B+ parameter models
- Enable larger context windows
- Consider multiple models for different tasks

**Medium Memory (8-16GB VRAM):**

- Use 7B parameter models
- Moderate context windows (8-16K)
- Balance between quality and speed

**Low Memory (<8GB VRAM):**

- Use 3B or smaller models
- Limit context windows (4-8K)
- Consider quantized versions (q4_0)

**CPU Only:**

- Use smallest models (2B or less)
- Expect slower responses
- Consider cloud-based alternatives

### By Performance Requirements

**Latency-Critical (<1s response time):**

- Use `phi3:mini` or `gemma:2b`
- Limit context size
- Use quantized versions

**Quality-Critical:**

- Use `llama3.1:8b` or larger
- Allow longer response times
- Use higher precision (q8_0 or f16)

**Balanced:**

- Use `llama3.2:3b` or `phi3:mini`
- Moderate context sizes
- Standard quantization (q4_0)

## Known Issues & Workarounds

### Tool Calling

**Issue:** Some models don't support native tool calling
**Affected Models:** codellama:7b, older models
**Workaround:** OLLM CLI automatically falls back to ReAct format for tool calling
**Status:** Handled automatically by the system

**Issue:** Tool calling may require specific prompt formatting
**Affected Models:** All models
**Workaround:** Use OLLM CLI's built-in tool system which handles formatting
**Status:** Handled automatically by the system

### Context Handling

**Issue:** Performance degrades at large context sizes (>32K tokens)
**Affected Models:** Most models
**Workaround:**

- Use context compression features
- Break large contexts into smaller chunks
- Use summarization for long conversations
  **Status:** Mitigation available through OLLM CLI features

**Issue:** Context window limits vary by model
**Affected Models:** All models
**Workaround:** OLLM CLI automatically detects and enforces model-specific limits
**Status:** Handled automatically by the system

### Streaming

**Issue:** Some models may have inconsistent streaming behavior
**Affected Models:** Varies by model and server version
**Workaround:** OLLM CLI handles streaming inconsistencies transparently
**Status:** Handled automatically by the system

### Model Availability

**Issue:** Models must be pulled before use
**Affected Models:** All models
**Workaround:** Use `ollama pull <model>` before first use
**Status:** User action required

**Issue:** Large models require significant disk space
**Affected Models:** 8B+ parameter models
**Workaround:**

- Use quantized versions (q4_0 instead of f16)
- Remove unused models with `ollama rm <model>`
- Monitor disk space
  **Status:** User management required

## Testing Methodology

### Automated Testing

The compatibility matrix is maintained through automated integration tests:

1. **Server Detection:** Tests automatically detect if Ollama server is running
2. **Model Discovery:** Tests check which models are available
3. **Capability Testing:** Each available model is tested for all capabilities
4. **Graceful Skipping:** Tests skip gracefully when server or models unavailable
5. **Result Documentation:** Test results are logged and can be used to update this document

### Test Capabilities

Each model is tested for:

- **Basic Chat:** Simple prompt/response interaction
- **Streaming:** Incremental response delivery
- **Native Tool Calling:** Built-in tool calling support
- **ReAct Fallback:** Text-based tool calling format
- **Context Sizes:** 4K, 8K, 16K, 32K, 64K, 128K token contexts

### Running Tests Yourself

```bash
# Start Ollama server
ollama serve

# Pull models you want to test
ollama pull llama3.1:8b
ollama pull codellama:7b
ollama pull phi3:mini

# Run compatibility tests
npm test -- modelCompatibility.integration.test.ts

# View detailed results in console output
```

### Updating This Document

When test results change:

1. Run the compatibility tests with your models
2. Review the console output for test results
3. Update the feature status tables above
4. Add any new known issues discovered
5. Update recommendations based on test results
6. Commit changes with test date and CLI version

## Contributing

If you discover issues or have recommendations:

1. Run the compatibility tests to verify the issue
2. Document the issue with specific model and version
3. Provide reproduction steps
4. Suggest workarounds if available
5. Submit a pull request or issue

## Version History

| Date       | CLI Version | Changes                              |
| ---------- | ----------- | ------------------------------------ |
| 2026-01-15 | 0.1.6       | Initial compatibility matrix created |

## References

- [OLLM CLI Documentation](../../README.md)
- [Models Documentation](3%20projects/OLLM%20CLI/LLM%20Models/README.md)
- [Model Management](Models_architecture.md)
- Testing Strategy (../../.kiro/specs/stage-08-testing-qa/design.md)
- [Integration Tests](../../packages/test-utils/src/__tests__/modelCompatibility.integration.test.ts)
- Ollama Model Library (https://ollama.ai/library)

---

**Note:** This document reflects the current state of testing. Status marked as "⊘ Not Tested" indicates that automated tests require a running server with the model installed. To get actual test results, run the integration tests with your models installed.
