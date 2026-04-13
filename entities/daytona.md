---
title: Daytona
created: 2026-04-13
updated: 2026-04-13
type: entity
tags: [company, sandbox, compute, agentic, infrastructure]
sources: [raw/transcripts/nate-agent-stack-six-layers-2026.md]
---

# Daytona

AI sandboxing company. $24M Series A (February 2026). Docker containers with a shared host kernel, built for speed and persistent state.

## Key Facts
- **Funding:** $24M Series A, February 2026
- **Architecture:** Docker containers, shared host kernel
- **Key Feature:** 90ms cold starts, persistent state — agent can install dependencies, create files, and come back later
- **Philosophy:** Persistent sandboxes (vs. [[e2b|E2B's]] ephemeral approach)
- **Position:** [[agent-stack-six-layers|Layer 1: Compute and Sandboxing]]

## The Persistent vs Ephemeral Split

Daytona and [[e2b]] represent two philosophical bets on how agent sessions work:

| | Daytona | E2B |
|---|---|---|
| Model | Persistent workspace | Ephemeral disposable |
| Cold start | 90ms | Slower |
| State | Survives between sessions | Torn down with session |
| Use case | Long-lived agent tasks | Short burst execution |

Both survive because different workloads need different models.

## Related Concepts
- [[agent-stack-six-layers]] — where Daytona fits
- [[e2b]] — competitor with different architecture bet (ephemeral vs persistent)
