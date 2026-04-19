---
title: Hermes Autoresearch
created: 2026-04-19
updated: 2026-04-19
type: entity
tags: [ai, software, company, open-source]
sources: [~/Documents/Last30Days/karpathy-loop-raw-v3.md]
---

# Hermes Autoresearch

The Hermes Autoresearch skill (by **@Tugrul_Guner / NousResearch**, GitHub PRs #5112, #5175) is the native implementation of the Karpathy Loop pattern inside the Hermes Agent framework. It is one of the most complete and production-ready ports of the pattern.

## Relationship to the Karpathy Loop
The Hermes skill is a faithful implementation of `karpathy/autoresearch` but adapted for:
- **Hermes's cron system** for background scheduling
- **Hermes's skill architecture** for zero-change integration
- **Multi-domain use** beyond just ML training (knowledge research, security audits, competitive intelligence, PRD refinement)

## The Five Supported Flows

### Flow 1: ML / Code Optimization
Iterate on `train.py`, try different models/hyperparameters, keep only what beats baseline.

**Evaluation:** Deterministic numerical metric comparison (val_bpb, accuracy, latency)
**Merge:** Only if new score beats previous best

### Flow 2: Knowledge Research
Build `research.md` via web research. Self-evaluated against a rubric.

**Evaluation:** 5-criteria rubric (1-5 each):
| Criterion | Gate | Purpose |
|-----------|------|---------|
| Evidence | >= 3 | Must cite real sources |
| Accuracy | — | Factual correctness |
| Depth | — | Beyond surface-level |
| Relevance | >= 3 | Addresses the query |
| Net Improvement | >= 3 | Adds value over previous |
| **Total** | **>= 13** | Overall quality floor |

### Flow 3: Code Analysis / Security Audit
Analyze codebase for issues using static analysis. **Hard evidence gate**: must include file paths, line numbers, and code snippets to merge.

### Flow 4: Recurring Competitive Intelligence
Weekly cron that checks for competitor changes. **Delta-only**: only genuinely new findings are merged — no restating known facts.

### Flow 5: Product Requirements Refinement
Start with rough PRD, iteratively deepen each section (feasibility, competitors, market gaps) via research.

## Architecture
```
skills/research/autoresearch/
├── SKILL.md                    # Skill definition
├── scripts/                    # 7 stdlib Python helpers (zero external dependencies)
│   ├── state.py                # Atomic JSON I/O, budget enforcement
│   ├── plan.py                 # Experiment CRUD (investigate/deepen/verify/synthesize)
│   ├── evaluate.py              # Scoring rubric + ML metric comparison
│   ├── workspace.py             # Git branch/merge/revert operations
│   ├── report.py                # Markdown report generation
│   ├── registry.py              # Multi-user run tracking at ~/.hermes/autoresearch/
│   └── usage.py                # Token/cost tracking via SessionDB
├── templates/
│   ├── cron_prompt.md           # Main loop (4 phases: setup, planning, loop, synthesis)
│   ├── watchdog_prompt.md      # Progress monitor — fires every 15min
│   └── resume_prompt.md         # Resume from checkpoint
├── test_e2e.py                 # 32 assertions
└── test_integration.py         # 44 tests, 10 classes
```

## Key Design Decisions

### Main Branch as Permanent Memory
Accumulated knowledge survives context loss — `main` always holds the best version.

### Mid-Run Replanning
Every 5 experiments, the agent re-reads stats and the plan to avoid drifting from the research goal.

### Budget Enforcement
Three budget types: time, tokens, experiment count. If any is exceeded, the loop pauses and reports.

### Watchdog (Every 15 Minutes)
Monitors `status.json` for stalls (>30min no update). Alerts user if loop seems stuck.

### State Persistence
`checkpoint.json`, `status.json`, `results.log` enable resume after context reset.

## Launching
```python
cronjob(action="create", name="research-<id>", schedule="1m",
        skills=["autoresearch"], prompt=<filled_template>, deliver="origin")
```
Note: `schedule="1m"` (not `"now"`) — the scheduler doesn't reliably fire immediate jobs.

## Test Results
| Test Suite | Assertions | Result |
|------------|-----------|--------|
| test_integration.py | 44 tests, 10 classes | ALL PASSING |
| test_e2e.py | 32 assertions | ALL PASSING |
| Mode 1 (ML) | Wine dataset RF, 6 experiments | All correctly evaluated |
| Mode 2 (Knowledge) | AI coding agents analysis, 6 experiments merged | 450-line report |

## Benchmark
Nous Research published results: **an agent using self-created skills completed research tasks 40% faster than a fresh instance** — demonstrating the compounding value of accumulated skills.

## Community Ecosystem
The skill has spawned related projects:
- **novix-science/autoresearch** — cited in ClawTeam-OpenClaw as a full results fork
- **uditgoenka/autoresearch** — community skill variant (3.2K stars on the skill-of-skills repo)
- **ARIS** (wanshuiyin/Auto-claude-code-research-in-sleep) — "Inspired by Hermes Agent; AutoResearch (Andrej Karpathy) — End-to-end research automation"
- **SEO autoresearch** — `skill-of-skills` repo lists an SEO-specific variant at `.autoresearch/seo/`

## Related
- [[karpathy-loop]] — the canonical implementation this skill ports
- [[autoresearch]] — the broader pattern across all implementations
- [[eval-harness]] — the scoring infrastructure the skill depends on
- [[program-md]] — the human's research direction that drives the loop

See also: [[local-hard-takeoff]], [[march-of-nines]]
