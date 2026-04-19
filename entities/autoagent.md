---
title: AutoAgent
created: 2026-04-18
updated: 2026-04-18
type: entity
tags: [software, ai, company]
sources: [raw/articles/nates-substack-karpathy-loop-2026.md]
---

# AutoAgent

AutoAgent is a project (by Kevin Gu/ThirdLayer) that applies the Karpathy Loop pattern to agentic harness engineering.

## Key Contributions
- **Harness Optimization:** Instead of weights, it optimizes prompts, tools, and orchestration logic.
- **Emergent Behaviors:** The meta-agent independently discovered strategies such as spot-checking (running individual tasks instead of the full benchmark for small edits), forced verification loops, formatting validators, task-specific sub-agents, and progressive disclosure (dumping long contexts to files).
- **Benchmarking (MIT-Licensed):**
  - **SpreadsheetBench:** 96.5% — #1 on leaderboard.
  - **TerminalBench:** 55.1% — Top GPT-5 score.
  - Both were the highest scores in their respective leaderboards.

## Design Insights
1. **Meta-Agent / Task-Agent Split is Critical:** A single agent cannot improve itself effectively. The capabilities of "doing" and "improving" are different.
2. **Model Empathy:** Same-model pairings (Claude $\rightarrow$ Claude) dramatically outperform cross-model pairings. The meta-agent shares weights with the task-agent, giving it implicit understanding of failure modes from the inside.
3. **The "Would This Still Matter?" Self-Reflection Check:** The team forces the meta-agent to ask if a harness improvement would matter if the exact task disappeared. This catches overfitting to the benchmark.

See also: [[karpathy-loop]], [[agent-orchestration]], [[march-of-nines]]
