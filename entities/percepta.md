---
title: Percepta
created: 2026-04-12
updated: 2026-04-12
type: entity
tags: [ai, research, company, architecture, neural-computing]
sources: [raw/transcripts/nate-turboquant-gpu-6x-breakthrough-2026-04-11.md]
---

# Percepta

Startup that demonstrated the transformer is a **literal computer**, not just an analogy.

## What They Did

Percepta compiled a WebAssembly interpreter directly into the weight matrix of a standard PyTorch transformer. Not as an external tool call, not as a sandbox alongside the model. **Inside the weights.**

The model executes arbitrary C programs through its forward pass, step by step, emitting a stack trace as tokens.

## Demonstrations

- Solved the "world's hardest" Sudoku puzzle via actual backtracking search algorithm — deterministic, 100% accuracy (not "reasoning" about the puzzle)
- Multi-digit addition without a single error
- 30,000+ tokens per second on CPU

## Significance

Today's agentic ecosystem (Claude Code, Codex, OpenClaw) is built on bolting System 2 (deterministic code execution) onto System 1 (intuitive LLM reasoning) from the outside:
1. Model reasons (System 1)
2. Writes code (System 1 → System 2 instructions)
3. Ships to sandbox (external System 2)
4. Incorporates result (back to System 1)

Percepta demonstrated both systems can be **fused into a single substrate** — the same transformer that generates language can also execute deterministic programs.

## The KV Cache Connection

Every System 2 operation the model runs internally writes to the same KV cache that holds the conversation. Fusion only works if working memory is large enough to hold both linguistic context and computational state simultaneously.

Percepta's innovation, **HullKVCache** (2D attention heads), reduced attention complexity to O(log n), making million-step programs feasible. But size constraint remains — if KV cache runs out, the computer stops.

This connects directly to [[turboquant]]: if the transformer is a computer and the KV cache is its RAM, then 6x KV cache compression is the equivalent of upgrading every GPU to 480 GB of working memory without new hardware. That's not a cost optimization — it's a capability threshold.

## Related Concepts

- [[kv-cache]] — the RAM being expanded
- [[turboquant]] — the compression enabling larger effective memory
- [[three-body-ai-memory]] — the economic framework for why this matters
- [[neuroscience-memory]] — structural analogy to how human brains store and retrieve memories
