---
title: ClawdMarket
created: 2026-04-19
updated: 2026-04-19
type: entity
tags: [ai, finance, company]
sources: [~/Documents/Last30Days/autoresearch-stock-trading-backtesting-raw-v3.md]
---

# ClawdMarket

ClawdMarket (clawdmkt.com) is an **agent marketplace** and the first live platform to implement a **Karpathy-style recursive self-improvement loop** for financial trading agents.

## What It Does
ClawdMarket describes itself as "the first agent marketplace to implement a live Karpathy-style recursive self-improvement loop":
- Agents **benchmark themselves** against each other
- **Compete variants** in parallel — each running its own optimization loop
- Best-performing variants **compound gains** autonomously
- Losers are reverted (git-style keep/revert at the portfolio level)

## Relationship to the Karpathy Loop
This is the closest real-world example of a **[[local-hard-takeoff]]** event in financial systems — an agent population where the top performers improve faster than the bottom performers can compensate, creating compounding returns that have no obvious ceiling in the short term.

Per @BankQuote on X: *"ClawdMarket now runs a Karpathy loop. Give an LLM a harness, and a singular objective, it can perpetually loop and improve as long as the objective is well, objective (verifiable)."*

## Significance
Most Autoresearch applications are personal tools or research projects. ClawdMarket is the first **production multi-agent marketplace** where the Karpathy loop runs live on real capital, with agents competing for economic returns rather than benchmark scores.

See also: [[karpathy-loop]], [[autoresearch]], [[local-hard-takeoff]], [[autoresearch-trading]]
