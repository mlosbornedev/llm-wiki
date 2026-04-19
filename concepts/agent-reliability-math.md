---
title: Agent Reliability Math
created: 2026-04-13
updated: 2026-04-13
type: concept
tags: [ai, infrastructure, reliability, compounding, fragility]
sources: [raw/transcripts/nate-agent-stack-six-layers-2026.md]
---

# Agent Reliability Math

How reliability compounds across an agent stack — and why it points toward the [[agent-orchestration|orchestration gap]].

## The Compounding Problem

When your agent depends on five primitives, end-to-end reliability = product of five reliabilities.

| Per-Primitive Uptime | 5 Dependencies | 7 Dependencies |
|---|---|---|
| 99% | 95.0% | 93.2% |
| 97% | 86.1% | 80.9% |
| 95% | 77.4% | 70.3% |

**Microservices fragility problem, except worse.** Agents are non-deterministic, so failures are harder to diagnose. A service returning a wrong answer looks different from a service returning an error.

## Why This Matters for Stack Design

Every additional primitive you depend on degrades your end-to-end reliability. This creates pressure to:
1. Minimize dependencies (fewer primitives, more vertical integration)
2. Choose primitives with higher uptime (reliability becomes a selection criteria)
3. Build [[agent-orchestration|orchestration infrastructure]] that handles failures gracefully

## The New Physics for Builders

The article frames this as "new physics" — the reliability math is a constraint that changes how you design agent systems:

- **Context engineering** matters more than prompt engineering because bad context creates failures that look like model errors but are actually stack errors
- **Eval-driven development** becomes critical: success rate + cost per task as the core feedback loop, replacing unit tests
- **Stack literacy** is a competitive advantage: knowing which layers are stable vs. transitional lets you avoid shims that will create migration costs

## The Transition Lock-in Problem

Building on shims (e.g., email-as-identity, framework-level orchestration) creates **migration costs when native protocols arrive.** Every shim adopted is a bet that it either becomes the standard or that you'll have time to swap it out.

The compounding math cuts both ways: a 97% reliable shim looks fine in isolation, but layered with four other 97% components gives you 86% end-to-end. When the native protocol arrives, migrating five shim-dependent components is harder than migrating one.

## Related Concepts
- [[agent-stack-six-layers]] — the six layers this math applies to
- [[agent-orchestration]] — the gap that widens when reliability math isn't addressed
- [[ai-consulting-build-buy]] — why some agent problems are engineering problems (solvable) vs domain expertise problems (require consultants)
