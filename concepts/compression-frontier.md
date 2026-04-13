---
title: AI Memory Compression Frontier
created: 2026-04-12
updated: 2026-04-12
type: concept
tags: [ai, research, compression, inference, kv-cache, landscape]
sources: [raw/transcripts/nate-turboquant-gpu-6x-breakthrough-2026-04-11.md]
---

# AI Memory Compression Frontier

The full attack on the LLM KV cache / working memory bottleneck, spanning five distinct research directions. [[turboquant]] is one entry in a much broader picture.

## Five Approaches

### 1. Quantization
Represent the same vectors in fewer bits.

| Method | Technique | Status |
|---|---|---|
| [[turboquant]] | 3-bit via PolarQuant + QJL | 6x compression, zero loss, community impl |
| KIVI | 2-bit asymmetric quantization | Prior work |
| ZipCache | Mixed-precision: salient tokens at high precision, rest aggressive | Research |

### 2. Eviction and Sparsity
Instead of compressing everything, throw away tokens that don't matter.

| Method | Technique | Tradeoff |
|---|---|---|
| H2O (Heavy-Hitter Oracle) | Keep only high-attention tokens, evict rest | May lose context that matters later |
| StreamingLLM | Sliding window + attention sink tokens | Loses early context unless sink tokens preserved |

### 3. Architectural Redesign
Attack the problem at the model level — shrink KV cache by design.

| Method | Technique | Limitation |
|---|---|---|
| MLA (DeepSeek-V2) | Project keys/values into lower-dimensional latent space during training | Requires training from scratch |
| Hybrid architectures (Granite 4.0, Nemotron-H) | Replace quadratic attention with linear-time SSMs | Same — requires new training |

### 4. Offloading and Tiering
Keep full cache but move it strategically between GPU and CPU.

| Method | Technique | Result |
|---|---|---|
| ShadowKV | Compressed keys on GPU, values offloaded to CPU | 6x larger batch sizes, 3x throughput on A100s |

### 5. Attention Optimization
Make the computation cheaper without shrinking the data.

| Method | Technique | Result |
|---|---|---|
| FlashAttention v1–v3 | Restructures GPU memory IO | Reaches 75% of theoretical H100 performance |

## Stacking Effects

These approaches **compound**, they don't compete:
- [[turboquant]] stacks with ShadowKV offloading
- Eviction stacks with quantization
- MLA stacks with FlashAttention
- Each generation across all five fronts multiplies effective memory of existing hardware

## Why the Frontier Moves Fastest

The three forces in [[three-body-ai-memory]] operate on different timescales:
- Supply: fab timelines (years)
- Demand: adoption curves (quarters)
- Compression: speed of math papers (weeks)

New results land weekly. The third force is the fastest-moving variable in a system where the other two are structurally slow.

## Related Concepts

- [[kv-cache]] — what all these approaches are optimizing
- [[turboquant]] — the highest-profile recent breakthrough
- [[three-body-ai-memory]] — the economic framework
- [[percepta]] — orthogonal approach: make the transformer compute, not just store more efficiently
- [[vllm]] — production-ready implementation vehicle for these techniques
