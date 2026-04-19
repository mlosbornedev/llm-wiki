---
title: AutoKernel
created: 2026-04-19
updated: 2026-04-19
type: entity
tags: [ai, software, company, open-source]
sources: [~/Documents/Last30Days/autokernel-gpu-kernel-optimization-raw-v3.md]
---

# AutoKernel

AutoKernel is an open-source framework (by **RightNow AI**, arXiv:2603.21331) that applies the Karpathy Loop pattern to **GPU kernel optimization** for arbitrary PyTorch models.

## What It Does
The agent loop:
1. **Profiles** the PyTorch model to identify GPU kernel bottlenecks
2. **Ranks** bottlenecks by **Amdahl's Law** — which optimizations give the most end-to-end speedup
3. **Extracts** each bottleneck as a standalone Triton or CUDA C++ kernel
4. **Writes** optimized replacements via the keep/revert agent loop
5. **Benchmarks** and promotes winners

## Key Results
- **5.29x speedup** over PyTorch Eager on a RangeNet model
- **278 teraflops** on H100 (vs. cuBLAS at 989.5 — the gap is the opportunity)
- **First place** on the **Vector Sum V2 B200 leaderboard**
- Runs **300+ experiments overnight** with no human in the loop
- Supports both **Triton** (1-5 second compilation for rapid iteration) and **CUDA C++** (maximum control)

## Architecture
AutoKernel mirrors the three-file Karpathy architecture:
- `kernel.py` — the editable surface (the kernel being optimized)
- `benchmark.py` — the locked eval harness (measures teraflops)
- `program.md` — the human's research direction

## Integration
- **KernelBench** (Stanford's 250-problem benchmark for AI-generated GPU kernels) provides standardized scoring
- **HuggingFace export path** for sharing optimized kernels
- Integration with **PyTorch models** for seamless profiling

## Why It Matters
Per @saumya_812 on X: *"GPU kernel optimization was a moat for teams with deep CUDA expertise. AutoKernel democratizes it. Any ML team with a GPU can now run overnight optimization experiments that previously required a specialist. The hardware edge is compressing."*

## Key Insight: Amdahl's Law Prioritization
A 1.5x speedup on a kernel running 60% of total runtime beats a 3x speedup on a kernel running 5% of runtime. The meta-agent independently discovers this prioritization strategy.

See also: [[karpathy-loop]], [[autoresearch]], [[eval-harness]], [[program-md]]
