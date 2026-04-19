---
title: KV Cache
created: 2026-04-12
updated: 2026-04-12
type: concept
tags: [ai, architecture, inference, memory, llm]
sources: [raw/transcripts/nate-turboquant-gpu-6x-breakthrough-2026-04-11.md]
---

# KV Cache

**Key-Value Cache** — the working memory of a transformer LLM. Every token the model has seen in the current context is stored as a key-value pair, and the attention mechanism computes over all of those pairs for every new token generated.

## Why It Matters

The KV cache is what lets a model connect token 90,000 to token 3,000. It enables:
- Holding a conversation without forgetting the beginning
- Following complex multi-step arguments
- Tracking a codebase across thousands of lines
- Finding a needle in a haystack of text

If model weights are the processor, the KV cache is the **RAM**. This is not a metaphor — [[percepta]] demonstrated that a transformer can literally execute arbitrary programs through its forward pass, and every step writes to the KV cache.

## The Memory Crisis

The KV cache is at the center of the AI memory crisis described in [[three-body-ai-memory]]:

- **Supply side:** HBM (high-bandwidth memory) is structurally constrained. New fabs won't help until 2027–2030.
- **Demand side:** Agents happened. Token consumption per developer is measured in billions annually. DRAM prices surged 172%.
- **The third force:** Compression — making the same memory do radically more work.

## Compression Approaches

Multiple research directions are attacking the KV cache bottleneck simultaneously:

| Approach | Technique | Status |
|---|---|---|
| [[turboquant]] | 3-bit quantization via PolarQuant + QJL | 6x compression, zero loss, community impl available |
| KIVI | 2-bit asymmetric quantization | Prior work |
| ShadowKV | Offload values to CPU | 6x larger batch sizes on A100s |
| H2O | Evict low-attention tokens | Tradeoff: may lose important context |
| StreamingLLM | Sliding window + attention sinks | Production used |
| MLA (DeepSeek-V2) | Latent space projection during training | Architectural redesign |
| [[vllm]] FP8 | Production-ready FP8 quantization | Available today |

## Practical Impact of 6x Compression

For a 70B-parameter model serving 32K-context sessions:
- Per concurrent user: ~16 GB VRAM → ~3 GB
- Same B200 GPU pair: 9 concurrent sessions → 50+
- Revenue per GPU: ~5x increase

See [[percepta]] for why this matters beyond cost — it's what enables qualitatively new computation when the "RAM" of the transformer expands without new hardware.

Token efficiency ([[token-management]]) is the user-side complement: [[turboquant]] compresses memory at the hardware level, while token management compresses memory at the protocol level — both multiply effective GPU capacity from different angles.
