---
title: vLLM
created: 2026-04-12
updated: 2026-04-12
type: entity
tags: [ai, open-source, inference, software, llm]
sources: [raw/transcripts/nate-turboquant-gpu-6x-breakthrough-2026-04-11.md]
---

# vLLM

Open-source LLM inference serving engine. Key player in the KV cache compression landscape.

## KV Cache Compression Status

- **FP8 KV cache quantization** is **production-ready today** in vLLM — not [[turboquant]]-level 3-bit zero-loss, but real compression on the same bottleneck, deployable without waiting for Google's official release
- Active integration discussions for TurboQuant community implementations underway
- Part of the broader attack on the [[kv-cache]] bottleneck from multiple angles

## Why It Matters in the Three-Body Framework

vLLM represents the "closest to the metal" layer where compression gains flow directly to inference economics. In the [[three-body-ai-memory]] competitive analysis, companies like vLLM that sit between model providers and enterprises are squeezed as efficiency gains accrue to whoever controls the serving stack directly.

## Related Concepts

- [[kv-cache]] — the memory bottleneck vLLM is attacking
- [[turboquant]] — the breakthrough vLLM is integrating
- [[three-body-ai-memory]] — why this compression race matters economically
- [[percepta]] — different approach: making the transformer itself compute, not just serving it more efficiently
