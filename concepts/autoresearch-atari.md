---
title: Autoresearch Atari
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [ai, software, research]
sources: [~/Documents/Last30Days/autoresearch-atari-breakout-game-optimization-raw-v4.md]
---

# Autoresearch Atari

The Atari Breakout application of the Karpathy Loop proves the pattern is **truly domain-agnostic** — not limited to code, ML training, or any text-bounded domain. An AI agent can optimize a game-playing agent through the same edit/eval/keep-or-revert loop.

## Project
`shehabyasser-scale/autoresearch-hack` (GitHub PR #1) forked from `karpathy/autoresearch`.

## The Three Files (Atari Version)

| File | Owner | Role |
|------|-------|------|
| `atari/program.md` | Human | Instructions for the AI researcher |
| `atari/agent.py` | Agent | The game-playing agent — modified to maximize `mean_reward` |
| `atari/prepare.py` | **Locked** | Fixed eval harness: 30-episode evaluation with fixed seeds, Gymnasium ALE environment factory |

## The Loop
```
1. AI reads program.md + agent.py
2. Modifies agent.py with an experimental idea
3. git commit
4. Runs: python agent.py > run.log 2>&1
5. grep "^mean_reward:" run.log
6. If improved → keep commit. If worse → git reset
7. Log to results.tsv. Goto 1.
```

## Baselines
| Agent | Mean Reward |
|-------|-------------|
| Random | ~2.0 |
| Heuristic (ball-tracking) | ~3.07 |

## Ideas the Researcher Explores
- Ball trajectory prediction from consecutive frames
- Frame differencing for velocity estimation
- Anticipatory paddle positioning
- Evolutionary parameter search during `train()`

## Rate Limit Handling
The implementation includes production-grade robustness:
- Parses 429 rate limit reset time, sleeps until reset rather than hammering the API
- Exponential backoff for non-rate-limit errors (30s, 60s, ... up to 300s)
- Handles "branch already exists" gracefully (switches to existing branch)
- Built on litellm proxy — works with any LLM provider (Anthropic, OpenAI, etc.)

## Why It Matters
The Atari domain is fundamentally different from code/ML optimization:
- **No text involved** — the agent optimizes pixel-pattern responses, not code or prompts
- **Stochastic environment** — same action from same state can produce different outcomes
- **No source of truth except the score** — the eval harness is the only ground truth

This makes it the purest possible proof that the Karpathy Loop "ports to anything with a score."

See also: [[karpathy-loop]], [[autoresearch]], [[eval-harness]], [[local-hard-takeoff]]
