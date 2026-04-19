---
title: Mem0
created: 2026-04-13
updated: 2026-04-13
type: entity
tags: [company, memory, agentic, infrastructure, ai-stack]
sources: [raw/transcripts/nate-agent-stack-six-layers-2026.md]
---

# Mem0

The clear leader in Layer 3: Memory and State for AI agents. The "memory and state" layer — the difference between a stateless tool and an agent you can actually work with across sessions, tasks, and days.

## Key Facts
- **Funding:** $24M combined (seed + Series A)
- **GitHub:** 41,000 stars
- **Downloads:** 14 million
- **AWS:** Exclusive memory provider for AWS Agent SDK
- **API Call Growth:** 5x in two quarters (35M → 186M Q1→Q3 2025)

## Performance vs OpenAI Built-in Memory
On LOCOMO benchmark:
- **26% better accuracy** than OpenAI's built-in memory
- **91% lower latency**
- **90% reduced token usage**

## The Insight
Memory isn't "save the conversation." It's **active curation:**
- Stores important information
- Forgets outdated and conflicting details
- Recalls relevant context at inference time

Architecture: hybrid datastore combining graph, vector, and key-value stores. Treats memory as managed infrastructure, not a feature bolted onto a model.

## Platform Risk
Every frontier lab (OpenAI, Anthropic) is building memory into its own models. If memory becomes a model-level feature controlled by the labs — the way search got integrated into ChatGPT rather than remaining a separate tool — standalone memory companies get squeezed from above.

**Mem0's counter-thesis:** "Memory passport" — your AI memory travels with you across apps and models. Portability like a database, not like a model feature.

**Unresolved:** Whether users actually demand portable memory. History suggests lock-in usually wins.

## Position
[[agent-stack-six-layers|Layer 3: Memory and State]]

## Related Concepts
- [[agent-stack-six-layers]] — where Mem0 fits
- [[agent-stack-six-layers]] — deeper dive on the memory layer debate
- [[conway]], [[anthropic]] — companies also building memory into their agents
