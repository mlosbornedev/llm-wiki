---
title: E2B
created: 2026-04-13
updated: 2026-04-13
type: entity
tags: [company, sandbox, compute, agentic, infrastructure]
sources: [raw/transcripts/nate-agent-stack-six-layers-2026.md]
---

# E2B

AI sandboxing company. ~$32M total funding. Uses Firecracker microVMs (the same technology behind AWS Lambda) to give each agent session its own dedicated kernel.

## Key Facts
- **Funding:** ~$32M total
- **Architecture:** Firecracker microVMs — each agent session gets its own dedicated kernel
- **Philosophy:** Ephemeral sandboxes. Spin one up, run code, tear it down.
- **Position:** [[agent-stack-six-layers|Layer 1: Compute and Sandboxing]]

## Related Concepts
- [[agent-stack-six-layers]] — where E2B fits
- [[daytona]] — competitor with different architecture bet (persistent vs ephemeral)
- [[modal]], [[browserbase]] — other Layer 1 players
