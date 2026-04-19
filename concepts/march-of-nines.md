---
title: March of Nines
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [ai, software, research, productivity]
sources: [~/Documents/Last30Days/karpathy-loop-raw-v3.md]
---

# March of Nines

The "March of Nines" describes the exponential difficulty curve of achieving high reliability in multi-step agentic workflows. Named by Andrej Karpathy.

## The Reliability Math
For an N-step agentic workflow, the end-to-end success rate is:

$$P_{success} = p^N$$

where $p$ = per-step reliability.

| Per-Step | 5 Steps | 10 Steps |
|----------|---------|----------|
| 90% | 59% | 35% |
| 95% | 77% | 60% |
| 99% | 95% | 90% |
| 99.9% | 99.5% | 99% |

## The Business Value Gap
- **First 90%:** Achievable with a good demo. This is where most agentic products live today.
- **90% $\rightarrow$ 99%:** The "hard part." A 10-step workflow at 90%/step produces **6+ failures per day**.
- **99% $\rightarrow$ 99.9%:** The "business value" zone. Down to ~1 failure/day.
- **99.9% $\rightarrow$ 99.99%:** "Real world deployment." ~1 failure every 10 days.

## Why It's Hard
- Each 9 requires not just better models but better **harnesses, evals, and trace infrastructure**.
- The Karpathy Loop (closed optimization loop with a single metric) is one proposed solution — automating the harness improvement process itself.
- See also: [[karpathy-loop]], [[agent-reliability-math]], [[local-hard-takeoff]]

See also: [[karpathy-loop]], [[agent-reliability-math]], [[local-hard-takeoff]]
