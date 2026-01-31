# Ollama Models Reference

**Complete reference for Ollama-compatible models**

This document provides comprehensive information about Ollama models, focusing on context management, tool calling capabilities, and VRAM requirements for OLLM CLI.

---

## 📋 Table of Contents

1. [Context Window Fundamentals](#context-window-fundamentals)
2. [VRAM Requirements](#vram-requirements)
3. [Complete Model Database](#complete-model-database)
4. [Model Selection Matrix](#model-selection-matrix)
5. [Tool Calling Support](#tool-calling-support)
6. [Quantization Guide](#quantization-guide)
7. [Configuration](#configuration)
8. [Performance Benchmarks](#performance-benchmarks)
9. [VRAM Calculation Deep Dive](#vram-calculation-deep-dive)

**See Also:**

- [Model Commands](Models_commands.md)
- [Model Configuration](Models_configuration.md)
- [Routing Guide](3%20projects/OLLM%20CLI/LLM%20Models/routing/user-guide.md)

---

## Context Window Fundamentals

### Ollama Default Behavior

- **Default context window**: 2,048 tokens (conservative default)
- **Configurable via**: `num_ctx` parameter in Modelfile or API request
- **Environment variable**: `OLLAMA_CONTEXT_LENGTH`

### Context Window vs num_ctx

- **Context window**: Maximum tokens a model architecture supports
- **num_ctx**: Ollama-specific parameter setting active context length for inference
- Models can support larger contexts than Ollama's default - must be explicitly configured

**Example:**

```bash
# Llama 3.1 supports 128K context, but Ollama defaults to 2K
# Must configure explicitly:
export OLLAMA_CONTEXT_LENGTH=32768
```

---

## VRAM Requirements

### Memory Calculation Formula

```
Total VRAM = Model Weights + KV Cache + System Overhead (~0.5-1GB)
```

### Base VRAM by Model Size (Q4_K_M Quantization)

| Model Size | Est. File Size | Recommended VRAM  | Example Models                          |
| ---------- | -------------- | ----------------- | --------------------------------------- |
| 3B-4B      | 2.0-2.5 GB     | 3-4 GB            | Llama 3.2 3B, Qwen3 4B                  |
| 7B-9B      | 4.5-6.0 GB     | 6-8 GB            | Llama 3.1 8B, Qwen3 8B, Mistral 7B      |
| 12B-14B    | 7.5-9.0 GB     | 10-12 GB          | Gemma 3 12B, Qwen3 14B, Phi-4 14B       |
| 22B-35B    | 14-21 GB       | 16-24 GB          | Gemma 3 27B, Qwen3 32B, DeepSeek R1 32B |
| 70B-72B    | 40-43 GB       | 48 GB (or 2×24GB) | Llama 3.3 70B, Qwen2.5 72B              |

### KV Cache Memory Impact

KV cache grows linearly with context length:

- **8B model at 32K context**: ~4.5 GB for KV cache alone
- **Q8_0 KV quantization**: Halves KV cache memory (minimal quality impact)
- **Q4_0 KV quantization**: Reduces to 1/3 (noticeable quality reduction)

### Context Length VRAM Examples (8B Model, Q4_K_M)

| Context Length | KV Cache (FP16) | KV Cache (Q8_0) | Total VRAM |
| -------------- | --------------- | --------------- | ---------- |
| 4K tokens      | ~0.6 GB         | ~0.3 GB         | ~6-7 GB    |
| 8K tokens      | ~1.1 GB         | ~0.55 GB        | ~7-8 GB    |
| 16K tokens     | ~2.2 GB         | ~1.1 GB         | ~8-9 GB    |
| 32K tokens     | ~4.5 GB         | ~2.25 GB        | ~10-11 GB  |

**Key Insight:** Always aim to fit the entire model in VRAM. Partial offload causes 5-20x slowdown.

---

## Complete Model Database

This section contains all 35+ models currently supported in OLLM CLI with accurate VRAM requirements for each context tier.

### Coding Models

#### CodeGeeX4 9B

- **Company:** Zhipu AI
- **Quantization:** Q4_0
- **Size:** 5.5 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Multilingual code generation model with high inference speed
- **URL:** [ollama.com/library/codegeex4](https://ollama.com/library/codegeex4)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 7.5 GB | 8.1 GB | 9.2 GB | 11.5 GB | 15.8 GB | 24.5 GB |

#### CodeGemma 2B

- **Company:** Google DeepMind
- **Quantization:** Q4_K_M
- **Size:** 1.6 GB
- **Max Context:** 8K
- **Tools:** ❌ No | **Reasoning:** ❌ No
- **Description:** Lightweight code completion model for on-device use
- **URL:** [ollama.com/library/codegemma](https://ollama.com/library/codegemma)

**VRAM Requirements:**
| Context | 4k | 8k |
|---------|----|----|
| VRAM | 3.0 GB | 3.4 GB |

#### CodeGemma 7B

- **Company:** Google DeepMind
- **Quantization:** Q4_K_M
- **Size:** 5.0 GB
- **Max Context:** 8K
- **Tools:** ❌ No | **Reasoning:** ❌ No
- **Description:** Specialized code generation and completion model
- **URL:** [ollama.com/library/codegemma](https://ollama.com/library/codegemma)

**VRAM Requirements:**
| Context | 4k | 8k |
|---------|----|----|
| VRAM | 7.0 GB | 8.5 GB |

#### Codestral 22B

- **Company:** Mistral AI
- **Quantization:** Q4_0
- **Size:** 12 GB
- **Max Context:** 32K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Mistral's model optimized for intermediate code generation tasks
- **URL:** [ollama.com/library/codestral](https://ollama.com/library/codestral)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k |
|---------|----|----|-----|-----|
| VRAM | 14.5 GB | 15.6 GB | 17.8 GB | 22.0 GB |

#### DeepSeek Coder V2 16B

- **Company:** DeepSeek
- **Quantization:** Q4_K_M
- **Size:** 8.9 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Mixture-of-Experts (MoE) model for advanced coding tasks with highly efficient MLA Cache
- **URL:** [ollama.com/library/deepseek-coder-v2](https://ollama.com/library/deepseek-coder-v2)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 11.2 GB | 11.5 GB | 12.0 GB | 13.1 GB | 15.2 GB | 19.5 GB |

#### Granite Code 3B

- **Company:** IBM
- **Quantization:** Q4_K_M
- **Size:** 2.0 GB
- **Max Context:** 4K
- **Tools:** ❌ No | **Reasoning:** ❌ No
- **Description:** IBM enterprise coding model, very fast
- **URL:** [ollama.com/library/granite-code](https://ollama.com/library/granite-code)

**VRAM Requirements:**
| Context | 4k |
|---------|----|
| VRAM | 3.5 GB |

#### Granite Code 8B

- **Company:** IBM
- **Quantization:** Q4_K_M
- **Size:** 4.6 GB
- **Max Context:** 4K
- **Tools:** ❌ No | **Reasoning:** ❌ No
- **Description:** IBM enterprise coding model, robust for Python/Java/JS
- **URL:** [ollama.com/library/granite-code](https://ollama.com/library/granite-code)

**VRAM Requirements:**
| Context | 4k |
|---------|----|
| VRAM | 6.5 GB |

#### Magicoder Latest

- **Company:** ise-uiuc (OSS-Instruct Team)
- **Quantization:** Q4_K_M
- **Size:** 3.8 GB
- **Max Context:** 16K
- **Tools:** ❌ No | **Reasoning:** ❌ No
- **Description:** Code model trained on synthetic data (OSS-Instruct)
- **URL:** [ollama.com/library/magicoder](https://ollama.com/library/magicoder)

**VRAM Requirements:**
| Context | 4k | 8k | 16k |
|---------|----|----|-----|
| VRAM | 5.5 GB | 6.0 GB | 7.1 GB |

#### OpenCoder 8B

- **Company:** OpenCoder Team / INF
- **Quantization:** Q4_K_M
- **Size:** 4.7 GB
- **Max Context:** 8K
- **Tools:** ❌ No | **Reasoning:** ❌ No
- **Description:** Fully open-source coding model with transparent dataset
- **URL:** [ollama.com/library/opencoder](https://ollama.com/library/opencoder)

**VRAM Requirements:**
| Context | 4k | 8k |
|---------|----|----|
| VRAM | 6.5 GB | 7.2 GB |

#### Qwen 2.5 Coder 3B

- **Company:** Alibaba Cloud
- **Quantization:** Q4_K_M
- **Size:** 1.9 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Small but potent coding assistant
- **URL:** [ollama.com/library/qwen2.5-coder](https://ollama.com/library/qwen2.5-coder)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 3.5 GB | 4.0 GB | 4.8 GB | 6.5 GB | 9.8 GB | 16.5 GB |

#### Qwen 2.5 Coder 7B

- **Company:** Alibaba Cloud
- **Quantization:** Q4_K_M
- **Size:** 4.7 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** SOTA coding model in the 7B class
- **URL:** [ollama.com/library/qwen2.5-coder](https://ollama.com/library/qwen2.5-coder)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 6.5 GB | 7.2 GB | 8.4 GB | 10.8 GB | 15.5 GB | 24.5 GB |

#### Qwen 3 Coder 30B

- **Company:** Alibaba Cloud
- **Quantization:** Q4_0
- **Size:** 18 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Heavyweight coding model, expert level generation
- **URL:** [ollama.com/library/qwen3-coder](https://ollama.com/library/qwen3-coder)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 20.5 GB | 22.0 GB | 24.5 GB | 29.5 GB | 39.5 GB | 59.5 GB |

---

### Reasoning Models

#### DeepSeek R1 1.5B

- **Company:** DeepSeek
- **Quantization:** Q4_K_M
- **Size:** 1.1 GB
- **Max Context:** 128K
- **Tools:** ❌ No | **Reasoning:** ✅ Yes
- **Description:** Distilled reasoning model, highly efficient for logic puzzles
- **URL:** [ollama.com/library/deepseek-r1](https://ollama.com/library/deepseek-r1)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 2.5 GB | 2.8 GB | 3.2 GB | 4.1 GB | 6.0 GB | 9.8 GB |

#### DeepSeek R1 7B

- **Company:** DeepSeek
- **Quantization:** Q4_K_M
- **Size:** 4.7 GB
- **Max Context:** 128K
- **Tools:** ❌ No | **Reasoning:** ✅ Yes
- **Description:** Distilled reasoning model based on Qwen 2.5, excels in math/logic
- **URL:** [ollama.com/library/deepseek-r1](https://ollama.com/library/deepseek-r1)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 6.5 GB | 7.2 GB | 8.4 GB | 10.8 GB | 15.5 GB | 24.5 GB |

#### DeepSeek R1 8B

- **Company:** DeepSeek
- **Quantization:** Q4_K_M
- **Size:** 5.2 GB
- **Max Context:** 128K
- **Tools:** ❌ No | **Reasoning:** ✅ Yes
- **Description:** Distilled reasoning model based on Llama 3, balanced for logic
- **URL:** [ollama.com/library/deepseek-r1](https://ollama.com/library/deepseek-r1)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 7.0 GB | 7.8 GB | 9.0 GB | 11.5 GB | 16.2 GB | 25.0 GB |

#### Phi-4 Mini Reasoning

- **Company:** Microsoft
- **Quantization:** Q4_K_M
- **Size:** 3.2 GB
- **Max Context:** 128K
- **Tools:** ❌ No | **Reasoning:** ✅ Yes
- **Description:** Microsoft model specialized in math and logical reasoning steps
- **URL:** [ollama.com/library/phi4](https://ollama.com/library/phi4)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 5.0 GB | 5.6 GB | 6.8 GB | 9.2 GB | 13.8 GB | 23.0 GB |

---

### General Purpose Models

#### Command R7B 7B

- **Company:** Cohere
- **Quantization:** Q4_K_M
- **Size:** 5.1 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Optimized for RAG and tool use, excellent instruction following
- **URL:** [ollama.com/library/command-r7b](https://ollama.com/library/command-r7b)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 7.0 GB | 7.6 GB | 8.8 GB | 11.0 GB | 15.5 GB | 24.5 GB |

#### Dolphin3 8B

- **Company:** Cognitive Computations
- **Quantization:** Q4_K_M
- **Size:** 4.9 GB
- **Max Context:** 32K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Uncensored/compliant model optimized for general conversation/coding
- **URL:** [ollama.com/library/dolphin3](https://ollama.com/library/dolphin3)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k |
|---------|----|----|-----|-----|
| VRAM | 7.0 GB | 7.7 GB | 8.9 GB | 11.2 GB |

#### Gemma 3 1B

- **Company:** Google DeepMind
- **Quantization:** Q4_K_M
- **Size:** 815 MB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Ultra-compact text model from Google, high efficiency
- **URL:** [ollama.com/library/gemma3](https://ollama.com/library/gemma3)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 2.0 GB | 2.4 GB | 3.2 GB | 4.8 GB | 8.0 GB | 14.2 GB |

#### Gemma 3 4B

- **Company:** Google DeepMind
- **Quantization:** Q4_K_M
- **Size:** 3.3 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Balanced Google model with strong general purpose capabilities
- **URL:** [ollama.com/library/gemma3](https://ollama.com/library/gemma3)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 5.0 GB | 5.8 GB | 7.5 GB | 10.5 GB | 16.5 GB | 28.5 GB |

#### Gemma 3 Nano E2B

- **Company:** Google DeepMind
- **Quantization:** Q4_K_M
- **Size:** 5.6 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Nano/Experimental variant of Gemma 3 (high cache usage)
- **URL:** [ollama.com/library/gemma3](https://ollama.com/library/gemma3)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 7.5 GB | 8.5 GB | 10.5 GB | 14.5 GB | 22.5 GB | 38.5 GB |

#### Gemma 3 Nano Latest

- **Company:** Google DeepMind
- **Quantization:** Q4_K_M
- **Size:** 7.5 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Latest standard variant of the Gemma 3 Nano/Next series
- **URL:** [ollama.com/library/gemma3](https://ollama.com/library/gemma3)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 9.5 GB | 10.8 GB | 13.5 GB | 18.5 GB | 28.5 GB | 48.0 GB |

#### GPT-OSS 20B

- **Company:** OpenAI (Hypothetical/Mirror)
- **Quantization:** Q4_0
- **Size:** 13 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Open-weight model by OpenAI (hypothetical/leaked) for general tasks
- **URL:** [ollama.com/library/gpt-oss](https://ollama.com/library/gpt-oss)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 16.5 GB | 17.5 GB | 19.5 GB | 23.5 GB | 31.5 GB | 47.5 GB |

#### Granite 3.3 8B

- **Company:** IBM
- **Quantization:** Q4_K_M
- **Size:** 4.9 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Updated IBM model with improved reasoning and instruction following
- **URL:** [ollama.com/library/granite3.3](https://ollama.com/library/granite3.3)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 7.0 GB | 7.7 GB | 8.9 GB | 11.2 GB | 15.8 GB | 24.8 GB |

#### Granite 4 Latest

- **Company:** IBM
- **Quantization:** Q4_K_M
- **Size:** 2.1 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Next-gen IBM model, hybrid Mamba architecture for speed
- **URL:** [ollama.com/library/granite4](https://ollama.com/library/granite4)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 4.0 GB | 4.5 GB | 5.5 GB | 7.5 GB | 11.5 GB | 19.5 GB |

#### Llama 3 Latest (8B)

- **Company:** Meta
- **Quantization:** Q4_K_M
- **Size:** 4.7 GB
- **Max Context:** 8K (Native Llama 3.0)
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Meta's 8B standard model (robust generalist)
- **URL:** [ollama.com/library/llama3](https://ollama.com/library/llama3)

**VRAM Requirements:**
| Context | 4k | 8k |
|---------|----|----|
| VRAM | 6.5 GB | 7.2 GB |

#### Llama 3.2 3B

- **Company:** Meta
- **Quantization:** Q4_K_M
- **Size:** 2.0 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Lightweight Llama optimized for edge devices
- **URL:** [ollama.com/library/llama3.2](https://ollama.com/library/llama3.2)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 3.5 GB | 3.9 GB | 4.6 GB | 6.0 GB | 8.8 GB | 14.5 GB |

#### Ministral 3 3B

- **Company:** Mistral AI
- **Quantization:** Q4_K_M
- **Size:** 3.0 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Mistral's edge model with high intelligence-to-size ratio
- **URL:** [ollama.com/library/ministral-3](https://ollama.com/library/ministral-3)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 5.0 GB | 5.6 GB | 6.8 GB | 9.0 GB | 13.5 GB | 22.5 GB |

#### Ministral 3 8B

- **Company:** Mistral AI
- **Quantization:** Q4_K_M
- **Size:** 6.0 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Larger edge model from Mistral, strong instruction following
- **URL:** [ollama.com/library/ministral-3](https://ollama.com/library/ministral-3)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 8.0 GB | 8.8 GB | 10.5 GB | 13.6 GB | 20.0 GB | 32.5 GB |

#### Phi-3 Mini 3.8B

- **Company:** Microsoft
- **Quantization:** Q4_K_M
- **Size:** 2.2 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Microsoft's highly capable small model (Mini)
- **URL:** [ollama.com/library/phi3](https://ollama.com/library/phi3)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 4.0 GB | 4.5 GB | 5.5 GB | 7.5 GB | 11.5 GB | 19.5 GB |

#### Qwen 2.5 7B

- **Company:** Alibaba Cloud
- **Quantization:** Q4_K_M
- **Size:** 4.7 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Alibaba's strong generalist, beats Llama 3.1 8B in benchmarks
- **URL:** [ollama.com/library/qwen2.5](https://ollama.com/library/qwen2.5)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 6.5 GB | 7.2 GB | 8.4 GB | 10.8 GB | 15.5 GB | 24.5 GB |

#### Qwen 3 4B

- **Company:** Alibaba Cloud
- **Quantization:** Q4_K_M
- **Size:** 2.5 GB
- **Max Context:** 256K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Next-gen Qwen generalist, high efficiency
- **URL:** [ollama.com/library/qwen3](https://ollama.com/library/qwen3)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 4.5 GB | 5.0 GB | 6.0 GB | 8.0 GB | 12.0 GB | 20.0 GB |

#### Qwen 3 8B

- **Company:** Alibaba Cloud
- **Quantization:** Q4_K_M
- **Size:** 5.2 GB
- **Max Context:** 256K
- **Tools:** ✅ Yes | **Reasoning:** ✅ Yes
- **Description:** Next-gen Qwen generalist 8B, high reasoning capability
- **URL:** [ollama.com/library/qwen3](https://ollama.com/library/qwen3)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 7.5 GB | 8.2 GB | 9.5 GB | 12.0 GB | 17.0 GB | 27.0 GB |

---

### Vision-Language Models

#### Qwen 3 VL 4B

- **Company:** Alibaba Cloud
- **Quantization:** Q4_K_M
- **Size:** 3.3 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Vision-Language model, can analyze images/video inputs
- **URL:** [ollama.com/library/qwen3-vl](https://ollama.com/library/qwen3-vl)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 5.0 GB | 5.5 GB | 6.5 GB | 8.5 GB | 12.5 GB | 20.5 GB |

#### Qwen 3 VL 8B

- **Company:** Alibaba Cloud
- **Quantization:** Q4_K_M
- **Size:** 6.1 GB
- **Max Context:** 128K
- **Tools:** ✅ Yes | **Reasoning:** ❌ No
- **Description:** Larger Vision-Language model for detailed visual reasoning
- **URL:** [ollama.com/library/qwen3-vl](https://ollama.com/library/qwen3-vl)

**VRAM Requirements:**
| Context | 4k | 8k | 16k | 32k | 64k | 128k |
|---------|----|----|-----|-----|-----|------|
| VRAM | 8.0 GB | 8.7 GB | 10.0 GB | 12.5 GB | 17.5 GB | 27.5 GB |

---

## Model Selection Matrix

### By VRAM Available

| VRAM  | Recommended Models                    | Context Sweet Spot |
| ----- | ------------------------------------- | ------------------ |
| 4GB   | Llama 3.2 3B, Qwen3 4B, Phi-3 Mini    | 4K-8K              |
| 8GB   | Llama 3.1 8B, Qwen3 8B, Mistral 7B    | 8K-16K             |
| 12GB  | Gemma 3 12B, Qwen3 14B, Phi-4 14B     | 16K-32K            |
| 16GB  | Qwen3 14B (high ctx), DeepSeek R1 14B | 32K-64K            |
| 24GB  | Gemma 3 27B, Qwen3 32B, Mixtral 8x7B  | 32K-64K            |
| 48GB+ | Llama 3.3 70B, Qwen2.5 72B            | 64K-128K           |

### By Use Case

| Use Case       | Recommended Model    | Why                           |
| -------------- | -------------------- | ----------------------------- |
| General coding | Qwen2.5-Coder 7B/32B | Best open-source coding model |
| Tool calling   | Llama 3.1 8B         | Native support, well-tested   |
| Long context   | Qwen3 8B/14B         | Up to 256K context            |
| Reasoning      | DeepSeek-R1 14B/32B  | Matches frontier models       |
| Low VRAM       | Llama 3.2 3B         | Excellent for 4GB             |
| Multilingual   | Qwen3 series         | 29+ languages                 |

---

## Tool Calling Support

### Tier 1: Best Tool Calling Support

#### Llama 3.1/3.3 (8B, 70B)

- **Context**: 128K tokens (native)
- **Tool calling**: Native function calling support
- **VRAM**: 8GB (8B), 48GB+ (70B)
- **Strengths**: Best overall, largest ecosystem, excellent instruction following
- **Downloads**: 108M+ (most popular)

#### Qwen3 (4B-235B)

- **Context**: Up to 256K tokens
- **Tool calling**: Native support with hybrid reasoning modes
- **VRAM**: 4GB (4B) to 48GB+ (32B)
- **Strengths**: Multilingual, thinking/non-thinking modes, Apache 2.0 license
- **Special**: Can set "thinking budget" for reasoning depth

#### Mistral 7B / Mixtral 8x7B

- **Context**: 32K tokens (Mistral), 64K tokens (Mixtral)
- **Tool calling**: Native function calling
- **VRAM**: 7GB (7B), 26GB (8x7B MoE - only 13B active)
- **Strengths**: Efficient, excellent European languages

### Tier 2: Good Tool Calling Support

#### DeepSeek-R1 (1.5B-671B)

- **Context**: 128K tokens
- **Tool calling**: Supported via reasoning
- **VRAM**: 4GB (1.5B) to enterprise (671B)
- **Strengths**: Advanced reasoning, matches o1 on benchmarks
- **Architecture**: Mixture of Experts (MoE)

#### Gemma 3 (4B-27B)

- **Context**: 128K tokens (4B+)
- **Tool calling**: Supported
- **VRAM**: 4GB (4B) to 24GB (27B)
- **Strengths**: Multimodal (vision), efficient architecture

#### Phi-4 (14B)

- **Context**: 16K tokens
- **Tool calling**: Supported
- **VRAM**: 12GB
- **Strengths**: Exceptional reasoning for size, synthetic data training

### Tier 3: ReAct Fallback Required

#### CodeLlama (7B-70B)

- **Context**: 16K tokens (2K for 70B)
- **Tool calling**: Via ReAct loop
- **VRAM**: 4GB (7B) to 40GB (70B)
- **Best for**: Code-specific tasks

#### Llama 2 (7B-70B)

- **Context**: 4K tokens
- **Tool calling**: Via ReAct loop
- **VRAM**: 4GB (7B) to 40GB (70B)
- **Note**: Legacy, prefer Llama 3.x

---

## Quantization Guide

### Recommended Quantization Levels

| Level      | VRAM Savings | Quality Impact | Recommendation  |
| ---------- | ------------ | -------------- | --------------- |
| Q8_0       | ~50%         | Minimal        | Best quality    |
| Q6_K       | ~60%         | Very slight    | High quality    |
| Q5_K_M     | ~65%         | Slight         | Good balance    |
| **Q4_K_M** | ~75%         | Minor          | **Sweet spot**  |
| Q4_0       | ~75%         | Noticeable     | Budget option   |
| Q3_K_M     | ~80%         | Significant    | Not recommended |
| Q2_K       | ~85%         | Severe         | Avoid           |

### KV Cache Quantization

Enable KV cache quantization to reduce memory usage:

```bash
# Enable Q8_0 KV cache (recommended)
export OLLAMA_KV_CACHE_TYPE=q8_0

# Enable Q4_0 KV cache (aggressive)
export OLLAMA_KV_CACHE_TYPE=q4_0
```

**Note**: KV cache quantization requires Flash Attention enabled.

### Quantization Selection Guide

**For 8GB VRAM:**

- Use Q4_K_M for 8B models
- Enables 16K-32K context windows
- Minimal quality impact

**For 12GB VRAM:**

- Use Q5_K_M or Q6_K for 8B models
- Use Q4_K_M for 14B models
- Better quality, still good context

**For 24GB+ VRAM:**

- Use Q8_0 for best quality
- Can run larger models (32B+)
- Maximum context windows

---

## Configuration

### Setting Context Length

#### Via Modelfile

```dockerfile
FROM llama3.1:8b
PARAMETER num_ctx 32768
```

#### Via API Request

```json
{
  "model": "llama3.1:8b",
  "options": {
    "num_ctx": 32768
  }
}
```

#### Via Environment

```bash
export OLLAMA_CONTEXT_LENGTH=32768
```

#### Via OLLM Configuration

```yaml
# ~/.ollm/config.yaml
options:
  numCtx: 32768
```

### Performance Optimization

```bash
# Enable Flash Attention
export OLLAMA_FLASH_ATTENTION=1

# Set KV cache quantization
export OLLAMA_KV_CACHE_TYPE=q8_0

# Keep model loaded
export OLLAMA_KEEP_ALIVE=24h

# Set number of GPU layers (for partial offload)
export OLLAMA_NUM_GPU=35  # Adjust based on VRAM
```

---

## Performance Benchmarks

### Inference Speed (tokens/second)

| Model        | Full VRAM | Partial Offload | CPU Only |
| ------------ | --------- | --------------- | -------- |
| Llama 3.1 8B | 40-70 t/s | 8-15 t/s        | 3-6 t/s  |
| Qwen3 8B     | 35-60 t/s | 7-12 t/s        | 2-5 t/s  |
| Mistral 7B   | 45-80 t/s | 10-18 t/s       | 4-7 t/s  |

**Key Insight:** Partial VRAM offload causes 5-20x slowdown. Always aim to fit model entirely in VRAM.

### Context Processing Speed

| Context Length | Processing Time (8B model) |
| -------------- | -------------------------- |
| 4K tokens      | ~0.5-1 second              |
| 8K tokens      | ~1-2 seconds               |
| 16K tokens     | ~2-4 seconds               |
| 32K tokens     | ~4-8 seconds               |

**Note:** Times vary based on hardware and quantization level.

---

## Troubleshooting

### Out of Memory Errors

**Symptoms:**

```
Error: Failed to load model: insufficient VRAM
```

**Solutions:**

1. Use smaller model (8B instead of 70B)
2. Use more aggressive quantization (Q4_K_M instead of Q8_0)
3. Reduce context window (`num_ctx`)
4. Enable KV cache quantization
5. Close other GPU applications

### Slow Inference

**Symptoms:**

- Very slow token generation (< 5 tokens/second)
- High CPU usage

**Solutions:**

1. Check if model fits entirely in VRAM
2. Increase `OLLAMA_NUM_GPU` if using partial offload
3. Enable Flash Attention
4. Reduce context window
5. Use faster quantization (Q4_K_M)

### Context Window Issues

**Symptoms:**

```
Error: Context length exceeds model maximum
```

**Solutions:**

1. Check model's native context window
2. Set `num_ctx` appropriately
3. Use model with larger context window
4. Enable context compression in OLLM

---

## Best Practices

### Model Selection

1. **Match VRAM to model size**: Use the selection matrix above
2. **Consider context needs**: Larger contexts need more VRAM
3. **Test before committing**: Try models before configuring projects
4. **Use routing**: Let OLLM select appropriate models automatically

### VRAM Management

1. **Monitor usage**: Use `/context` command to check VRAM
2. **Keep models loaded**: Use `/model keep` for frequently-used models
3. **Unload when switching**: Use `/model unload` to free VRAM
4. **Enable auto-sizing**: Use `/context auto` for automatic context sizing

### Performance Optimization

1. **Enable Flash Attention**: Significant speed improvement
2. **Use KV cache quantization**: Reduces memory, minimal quality impact
3. **Keep models in VRAM**: Avoid partial offload
4. **Optimize context size**: Use only what you need

---

## VRAM Calculation Deep Dive

### Understanding VRAM Usage Components

VRAM usage comes from two distinct places:

#### 1. The "Parking Fee" (Model Weights)

This is the fixed cost just to load the model.

- **For an 8B model (Q4 quantization):** You need roughly 5.5 GB just to store the "brain" of the model
- This never changes, regardless of how much text you type

#### 2. The "Running Cost" (KV Cache)

This is the memory needed to "remember" the conversation. It grows with every single word (token) you add.

- **For short context (4k tokens):** You only need ~0.5 GB extra
  - Total: 5.5 + 0.5 = 6.0 GB (Fits in 8GB card)
- **For full context (128k tokens):** You need ~10 GB to 16 GB of extra VRAM just for the memory
  - Total: 5.5 + 16 = ~21.5 GB (Requires a 24GB card like an RTX 3090/4090)

### Context Cost by Model Class

#### The "Small" Class (7B - 9B)

**Models:** Llama 3 8B, Mistral 7B v0.3, Qwen 2.5 7B

- **Context Cost:** ~0.13 GB per 1,000 tokens
- **Notes:** These are extremely efficient. You can easily fit 32k context on a 12GB card (RTX 3060/4070) if you use 4-bit weights
- **Warning:** Gemma 2 9B is an outlier. It uses a much larger cache (approx 0.34 GB per 1k tokens) due to its architecture (high head dimension). It eats VRAM much faster than Llama 3

#### The "Medium" Class (Mixtral 8x7B)

**Models:** Mixtral 8x7B, Mixtral 8x22B

- **Context Cost:** ~0.13 GB per 1,000 tokens
- **Notes:** Mixtral is unique. While the weights are huge (26GB+), the context cache is tiny (identical to the small 7B model). This means if you can fit the weights, increasing context to 32k is very "cheap" in terms of extra VRAM

#### The "Large" Class (70B+)

**Models:** Llama 3 70B, Qwen2 72B

- **Context Cost:** ~0.33 GB per 1,000 tokens
- **32k Context:** Requires ~10.5 GB just for the context
- **128k Context:** Requires ~42 GB just for the context
- **Notes:** To run Llama 3 70B at full 128k context, you generally need roughly 80GB+ VRAM (e.g., 2x A6000 or Mac Studio Ultra), even with 4-bit weights

### Tips for Saving VRAM

#### KV Cache Quantization (FP8)

If you use llama.cpp or ExLlamaV2, you can enable "FP8 Cache" (sometimes called Q8 cache). This cuts the context VRAM usage in half with negligible quality loss.

**Example:** Llama 3 70B at 32k context drops from ~50.5 GB total to ~45 GB total

#### System Overhead

Always leave 1-2 GB of VRAM free for your display and OS overhead. If the chart says 23.8 GB and you have a 24 GB card, it will likely crash (OOM).

### Context Size Reference Table

| Context Size | What it represents   | Total VRAM Needed (8B Model) | GPU Class                  |
| ------------ | -------------------- | ---------------------------- | -------------------------- |
| 4k           | A long article       | ~6.0 GB                      | 8GB Cards (RTX 3060/4060)  |
| 16k          | A small book chapter | ~7.5 GB                      | 8GB Cards (Tight fit)      |
| 32k          | A short book         | ~9.5 GB                      | 12GB Cards (RTX 3060/4070) |
| 128k         | A full novel         | ~22.0 GB                     | 24GB Cards (RTX 3090/4090) |

### Practical Examples

#### Example 1: Running Qwen 2.5 7B on 8GB Card

- **Model weights:** 4.7 GB
- **4k context:** 6.5 GB total ✅ Fits comfortably
- **8k context:** 7.2 GB total ✅ Fits with headroom
- **16k context:** 8.4 GB total ❌ Will OOM
- **Recommendation:** Use 8k context maximum

#### Example 2: Running DeepSeek Coder V2 16B on 12GB Card

- **Model weights:** 8.9 GB (MoE with efficient MLA cache)
- **4k context:** 11.2 GB total ✅ Fits
- **8k context:** 11.5 GB total ✅ Fits
- **16k context:** 12.0 GB total ⚠️ Very tight, may OOM
- **32k context:** 13.1 GB total ❌ Will OOM
- **Recommendation:** Use 8k context for safety

#### Example 3: Running Qwen 3 Coder 30B on 24GB Card

- **Model weights:** 18 GB
- **4k context:** 20.5 GB total ✅ Fits
- **8k context:** 22.0 GB total ✅ Fits
- **16k context:** 24.5 GB total ❌ Will OOM
- **Recommendation:** Use 8k context maximum, or upgrade to 32GB+ VRAM

### GPU Recommendations by Use Case

#### Budget Setup (8GB VRAM)

- **Best models:** Llama 3.2 3B, Qwen 3 4B, Gemma 3 4B, DeepSeek R1 1.5B
- **Context sweet spot:** 8k-16k
- **Use case:** Code completion, quick queries, edge deployment

#### Mid-Range Setup (12GB VRAM)

- **Best models:** Qwen 2.5 7B, Qwen 2.5 Coder 7B, Llama 3 8B, Mistral 7B
- **Context sweet spot:** 16k-32k
- **Use case:** General development, moderate context needs

#### High-End Setup (24GB VRAM)

- **Best models:** Codestral 22B, Qwen 3 Coder 30B, DeepSeek Coder V2 16B
- **Context sweet spot:** 32k-64k
- **Use case:** Professional development, large codebases

#### Workstation Setup (48GB+ VRAM)

- **Best models:** Llama 3.3 70B, Qwen 2.5 72B, Large MoE models
- **Context sweet spot:** 64k-128k
- **Use case:** Enterprise, research, maximum capability

---

## See Also

- [Model Commands](Models_commands.md) - Model management commands
- [Model Configuration](Models_configuration.md) - Configuration options
- [Routing Guide](3%20projects/OLLM%20CLI/LLM%20Models/routing/user-guide.md) - Automatic model selection
- [Context Management](3%20projects/OLLM%20CLI/LLM%20Context%20Manager/README.md) - Context and VRAM monitoring

---

**Last Updated:** 2026-01-28  
**Version:** 0.2.0  
**Source:** Ollama Documentation, llama.cpp, community benchmarks, OLLM CLI Model Database

<!-- Alpha Release: v0.1.6 -->

