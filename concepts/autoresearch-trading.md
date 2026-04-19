---
title: Autoresearch Trading
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [ai, finance, software]
sources: [~/Documents/Last30Days/autoresearch-stock-trading-backtesting-raw-v3.md]
---

# Autoresearch Trading

A growing application of the Karpathy Loop pattern applies the autonomous optimization loop to **algorithmic trading and quantitative finance** — using agents to discover, backtest, and improve trading strategies overnight.

## Notable Projects

### AutoThesis
By @morganlinton — an open-source Rust tool applying the Autoresearch loop to stock research:
- Loop: `thesis → evidence → self-critique → fill gaps → new thesis`
- Found that every database call opened a brand-new SQLite connection (140+ sites), causing massive overhead
- Used both Codex and GLM; Opus found significant room for improvement in production code
- Also used for investment thesis validation: *"Agent loop: thesis → research evidence → self-critique: what's missing, what assumptions are weak?"*

### Stock Backtesting (r/HowToAIAgent)
A practitioner applied the loop to **Supertrend strategy** optimization:
- `strategy.py` — the only mutable file (the trading strategy code)
- `prepare.py` — runs a **10-year backtest via vectorbt**
- **5-metric composite score:** return, Sharpe ratio, drawdown, profit factor, win rate
- Ran **45 experiments** autonomously

### Trading Agent Infrastructure
- **Claude Cowork** (Anthropic): Can open exchanges, execute positions, run entire trading workflows autonomously
- **Codex-Infinity** (lee101): Autonomous coding agent researching trading algorithms all night with GPT-5.4 xhigh
- **@iuditg**: Used Claude's autoresearch skill to place real stock trades with a strict stop-loss strategy for experimentation

## The Pattern Applied
| Autoresearch Component | Trading Equivalent |
|------------------------|-------------------|
| `agent.py` (editable) | `strategy.py` — the trading algorithm |
| `prepare.py` (locked) | `vectorbt` — 10-year backtest harness |
| Metric | Composite: Sharpe + return + drawdown + win rate |
| `program.md` | Strategy constraints, asset class, risk limits |

## Risks & Caveats
- **Overfitting to backtest data** is the primary risk — the loop can find strategies that worked historically but fail live
- Per @LeeLeepenkman: *"repo is balooning out with lots of slop though due me running autoresearch agents"* — noisy experiments accumulate when the eval harness isn't tight enough
- Market regime changes break strategies that worked in backtesting
- **Real-money deployment** requires holdout periods and out-of-sample validation the loop doesn't automatically provide

## The ClawdMarket Case
ClawdMarket runs a **live Karpathy-style recursive self-improvement loop** for agentic trading: agents benchmark themselves, compete variants against each other, and the best performers compound gains. This is the closest thing to a "local hard takeoff" in financial systems.

See also: [[karpathy-loop]], [[autoresearch]], [[eval-harness]], [[local-hard-takeoff]]
