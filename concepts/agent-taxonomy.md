---
title: Agent Taxonomy
created: 2026-04-13
updated: 2026-04-13
type: concept
tags: [ai-agents, taxonomy, frameworks, tool-selection]
sources: [raw/transcripts/nate-4-kinds-of-agents-2026-03-25.md]
---

# Agent Taxonomy

The four distinct agent architectures that the word "agent" conflates — "about as much in common as a forklift and a bicycle."

## The Four Types

| Type | Use Case | Common Mistake |
|------|----------|---------------|
| **Coding harnesses** | Software development, test suites | Using for creative writing |
| **Dark factories** | Automated production/manufacturing | Using for novel writing |
| **Auto research** | Information synthesis, research loops | Using for building new software |
| **Orchestration frameworks** | Coordinating multiple agents/tasks | Using single-agent tools when coordination needed |

## Market Context
- 2025 AI Agent Market: ~$8 billion
- 2030 Projected Market: >$50 billion

## Common Mistakes Observed
- Dark factory → Writing a novel (wrong tool for creative work)
- CrewAI → Simple coding problems (needed single agent + test suite)
- Auto research loop → Writing new software (like "pointing a profiler at an empty file")

## The One-Question Test
A diagnostic question that cuts through ambiguity — works for ICs and CTOs alike.

## Operating Principles
- Decomposition
- Specification-as-code
- Metric-plus-guardrail
- Handoff contracts

## Key Insight
> "The taxonomy matters because the fix for each problem lives in a different place than most people are looking."

## Related
- [[agent-evaluation-framework]] — evaluating which agent type for which use case
- [[agent-reliability-math]] — why reliability compounds across layers
