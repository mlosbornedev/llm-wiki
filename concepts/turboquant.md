---
title: TurboQuant
created: 2026-04-12
updated: 2026-04-12
type: concept
tags: [ai, compression, inference, research, google]
sources: [raw/transcripts/nate-turboquant-gpu-6x-breakthrough-2026-04-11.md]
---

# TurboQuant

Google Research's breakthrough KV cache compression algorithm, published March 25, 2026. Compresses LLM working memory by **6x with zero accuracy loss** — no retraining, no calibration, no new hardware required.

## What It Is

The KV cache is the working memory (RAM) that a transformer model uses while reasoning — every token in the current context is stored as a key-value pair. TurboQuant compresses this from 16–32 bits per value down to **3 bits** with no measurable accuracy loss across question answering, code generation, summarization, and needle-in-a-haystack retrieval at 104K tokens.

The practical result: the same GPU that served 9 concurrent users now serves 50+.

## Technical Mechanism

TurboQuant combines two stages:

1. **PolarQuant** — Rotates data into polar coordinates where structure becomes predictable. Instead of storing Cartesian coordinates (x, y), stores radius + angle. Because angular distributions are concentrated and predictable, no per-block normalization constants are needed. This is analogous to how the brain stores "memory keys" that can reactivate forgotten memories (see [[neuroscience-memory]]).

2. **QJL (Quantized Johnson-Lindenstrauss)** — Uses a single bit to correct the residual error from PolarQuant, eliminating bias in attention scores. A mathematical error-checker.

The algorithm is **data-oblivious**: requires no knowledge of the specific model or dataset, no calibration pass, no fine-tuning. It's a mathematical property, not an engineering hack.

## Competitive Impact

- **Google wins twice** — wrote TurboQuant and runs Gemini. Primary application is removing the KV cache bottleneck in Gemini's inference stack.
- **NVIDIA's pricing power challenged** — if existing GPUs go 6x further, urgency to buy next-gen hardware at premium prices drops (though demand growth absorbs much of this).
- **Middleware squeezed** — efficiency gains flow to whoever is closest to the metal; orchestration layers get squeezed.
- **Enterprises get real relief** — for self-hosted inference, KV cache compression multiplies effective GPU capacity without new hardware.

## Related Concepts

- [[kv-cache]] — what TurboQuant compresses
- [[polarquant]] — first stage of TurboQuant
- [[three-body-ai-memory]] — the framework: supply constraints + demand explosion + compression as the third force
- [[percepta]] — proof the transformer is a literal computer, constrained by KV cache
- [[vllm]] — already supports FP8 KV cache compression production-ready today

## Community Status

Google's official code expected Q2 2026, but five independent community implementations shipped within two weeks, including one running a 104B-parameter model on a MacBook. Integration into [[vllm]] and llama.cpp is actively underway.
