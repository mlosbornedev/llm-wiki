---
title: Eval Harness
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [ai, software, research]
sources: [~/Documents/Last30Days/program-md-eval-harness-autoresearch-raw-v3.md]
---

# Eval Harness

The "Eval Harness" (also called the "evaluation harness" or simply `prepare.py` / `evaluate.py`) is the **locked, read-only scoring environment** in a Karpathy Loop setup. It is the only ground truth the agent has — and it can never be edited by the agent.

## The Three-File Architecture
Every Autoresearch setup has exactly three files:

| File | Owner | Role |
|------|-------|------|
| `program.md` | Human | Research direction and constraints |
| `agent.py` (or `train.py`) | Agent | The only mutable file — what gets edited |
| `prepare.py` / `evaluate.py` | **Locked** | The eval harness — runs experiment, produces score |

## Why the Harness Must Be Locked
The eval harness is **locked the moment the first round starts.** If you change the scoring mid-loop, every previous round's data becomes meaningless. The agent's only job is to improve the score produced by the harness.

As @aakashgupta put it (778 likes): *"prepare.py is the locked eval harness. Git commit keeps winners, git reset reverts losers."*

## What a Harness Does
1. **Runs the experiment** — executes the agent's proposed edit against a fixed test environment
2. **Produces a score** — returns a single numeric value (or vector) that reflects quality
3. **Is never modified** — the agent can read it but not write to it

## Harness Examples by Domain
| Domain | Eval Harness |
|--------|-------------|
| ML Training | Run 5-min training → measure `val_bpb` |
| Agent Skills | Run test cases → measure pass rate |
| Stock Backtesting | Run 10-year backtest via `vectorbt` → composite score |
| GPU Kernel | Profile model → benchmark Triton/CUDA kernel |
| Atari | 30-episode eval with fixed seeds |

## The 5-Criteria Self-Evaluation Rubric
For domains without a clear numeric objective, the Hermes autoresearch skill uses a **5-criteria rubric** in `evaluate.py`:
1. **Evidence** — Does the answer cite sources?
2. **Accuracy** — Is the information correct?
3. **Depth** — Does it go beyond surface-level?
4. **Relevance** — Does it address the query?
5. **Net Improvement** — Does it add value over the previous version?

## The Eval-as-Infrastructure Problem
The most common reason Autoresearch fails is **not having a reliable eval harness**. The Automation Trap: speeding up a process without fixing a broken outcome is the Autoresearch version of this — an agent optimizing a metric that was never the right metric to begin with.

The quality of your trace infrastructure determines the quality of your auto-improvement. An optimization loop that only sees scores (not reasoning trajectories) will produce random mutations. An eval harness that only captures outcomes (not *why* the outcome happened) can't teach the meta-agent to make targeted edits.

See also: [[karpathy-loop]], [[autoresearch]], [[local-hard-takeoff]], [[program-md]]
