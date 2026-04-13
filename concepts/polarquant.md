---
title: PolarQuant
created: 2026-04-12
updated: 2026-04-12
type: concept
tags: [ai, compression, mathematics, kv-cache, research]
sources: [raw/transcripts/nate-turboquant-gpu-6x-breakthrough-2026-04-11.md]
---

# PolarQuant

First stage of [[turboquant]]'s two-stage compression pipeline. A coordinate rotation technique that transforms how LLM memory vectors are represented.

## What It Does

Traditional vector quantization stores extra "quantization constants" alongside compressed data — like packing a suitcase tightly but then carrying a separate bag for the folding instructions.

PolarQuant rotates data into a **polar coordinate system** where structure becomes predictable:
- Instead of "go 3 blocks east and 4 blocks north" → "go 5 blocks at a 37-degree angle"
- The **radius** captures signal strength
- The **angles** capture the direction of meaning

Because angular distributions are concentrated and predictable, no per-block normalization constants are needed. The "separate bag of instructions" is eliminated.

## Connection to Neuroscience

This is structurally analogous to how human brains store memories — see [[neuroscience-memory]]. The brain stores a retrieval key that can reactivate a "forgotten" memory. PolarQuant stores a radius + angles that can reconstruct the full vector. Different substrates, same mathematical insight.

## Role in TurboQuant

PolarQuant provides aggressive compression from the coordinate rotation. [[turboquant]]'s second stage, QJL (Quantized Johnson-Lindenstrauss), then corrects the tiny residual error using a single bit. Together they achieve 3-bit KV cache representation (down from 16–32 bits) with zero measurable accuracy loss.

## Related Concepts

- [[turboquant]] — the full algorithm PolarQuant is part of
- [[kv-cache]] — what gets compressed
- [[neuroscience-memory]] — the structural analogy to brain memory encoding
