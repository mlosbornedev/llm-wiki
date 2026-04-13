---
title: AI Consulting Build vs Buy
created: 2026-04-13
updated: 2026-04-13
type: concept
tags: [ai-strategy, consulting, build-buy, engineering]
sources: [raw/transcripts/nate-accenture-2b-ai-consulting-2026-03-24.md]
---

# AI Consulting: Build vs Buy

The 4:1 ratio framework for agent deployment problems — separating engineering problems your team can solve from domain expertise problems worth paying consultants.

## The Two Competing Stories

### "Build Your Own" (Nvidia/Open Source)
- Jensen Huang released NeMoClaw at GTC — open-source agent security stack
- Nvidia's theory: agent deployment is a solvable engineering problem
- "Build your own" agent infrastructure

### "You Need Help" (OpenAI/Consulting Firms)
- OpenAI signed multi-year partnerships with McKinsey, BCG, Accenture, Capgemini
- Anthropic signed with Accenture and Deloitte
- Their theory: "The limiting factor isn't model intelligence — it's how agents are built and run in organizations"

## The 4:1 Ratio

| Category | Problems | Who Should Handle |
|----------|----------|------------------|
| **Engineering (50-yr precedent)** | Context compression, Codebase instrumentation, Linting, Multi-agent coordination | Your team |
| **Domain expertise** | The specification problem | Pay for help |

## Why The Ratio Stays Hidden
Neither side selling (consulting firms or open-source advocates) has incentive to reveal the 4:1 ratio because it changes the build-or-buy calculus entirely.

## The Four Prompts
A prompt kit to run before signing any consulting contract — identifies what your team handles vs. pays for.

## Key Insight
> "The agent deployment problems that everyone frames as unprecedented are, with one critical exception, well-understood engineering with fifty years of precedent."

## Related
- [[agent-stack-six-layers]] — which layers are which
- [[agent-orchestration]] — the gap layer that requires most help
