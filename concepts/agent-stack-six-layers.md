---
title: Agent Stack — Six Layers
created: 2026-04-13
updated: 2026-04-13
type: concept
tags: [ai, infrastructure, agentic, stack, primitives]
sources: [raw/transcripts/nate-agent-stack-six-layers-2026.md]
---

# Agent Stack — Six Layers

A new infrastructure stack forming underneath AI agents. The new "customer" for infrastructure is an LLM with a tool-call interface, not a human with a browser. Every assumption about how software gets provisioned, authenticated, billed, and composed is being renegotiated.

**The analogy:** system calls, not Lego bricks. Agents need defined, reliable interfaces to identity, compute, memory, persistence, communication, and payments — same way processes need system calls to interact with hardware. Lego bricks require a single precisely engineered interface standard. Agent primitives don't share one yet.

**The two previous analogous shifts:**
1. 2006-2015: on-premise → cloud primitives (EC2, S3, Lambda)
2. 2012-2016: monolithic apps → composable APIs/microservices
3. Now: human-first tools → agent-first primitives

## The Six Layers

### Layer 1: Compute and Sandboxing
**Status: Production-ready. Load-bearing wall.**

Agents need somewhere safe to run code: isolated, sandboxed, auditable execution.

| Company | Funding | Architecture | Key Feature |
|---|---|---|---|
| [[e2b]] | ~$32M | Firecracker microVMs (AWS Lambda tech) | Ephemeral sandboxes, disposable sessions |
| [[daytona]] | $24M Series A | Docker containers, shared host kernel | 90ms cold starts, persistent state |
| [[modal]] | — | Containers | GPU-heavy workloads |
| [[browserbase]] | $300M Series B | Headless browser | Web interaction for agents |

**Split:** ephemeral (disposable) vs persistent (long-lived workspaces) sandboxes. Both survive because different workloads need different models.

**Durability: High.** Specialist wins in medium term. Agent-specific requirements (sub-second startup, deterministic credential injection, session-level observability) are genuinely different from general-purpose cloud functions.

### Layer 2: Identity and Communication
**Status: Shim. Important now, likely replaced.**

Today: give the agent an email address. Email is the universal key — every SaaS accepts it for signup, every verification flow sends codes to it.

| Company | Funding | Approach |
|---|---|---|
| [[agentmail]] | $6M seed | Programmatic email inboxes for agents; email as identity layer |

**The problem:** Email was designed for humans. Threading is brittle, rate limits designed to prevent spam throttle agent activity, signal-to-noise is terrible for autonomous systems.

**Real need:** Native agent identity and communication protocol (on-chain, A2A standards, MCP-based discovery). Nothing has real traction yet.

**Durability: Medium.** 2-3 years while native protocols get sorted. Email is famously cockroach-like — the shim may become the standard.

### Layer 3: Memory and State
**Status: Early but real. Platform risk significant.**

Memory is the difference between a stateless tool and an agent you can actually work with — across sessions, tasks, and days.

| Company | Key Facts |
|---|---|
| [[mem0]] | Clear leader. $24M combined, 41K GitHub stars, 14M downloads, AWS exclusive memory provider. 5x API call growth in two quarters (35M → 186M). 26% better than OpenAI built-in on LOCOMO benchmark. |

**Platform risk:** Frontier labs (OpenAI, Anthropic) building memory into models. If memory becomes model-level feature, standalone memory companies get squeezed from above.

**Counter-thesis:** "Memory passport" — memory travels across apps and models. Whether users demand portable memory is unproven.

**Durability: Uncertain.** Depends on whether memory becomes model-locked or portable.

### Layer 4: Tool Access and Integration
**Status: Growing fast. Solves real, immediate pain.**

Agents interact with existing SaaS tools (Slack, Jira, Salesforce, GitHub, Google Workspace) without requiring those tools to rebuild their interfaces.

| Company | Funding | Coverage |
|---|---|---|
| [[composio]] | $29M total (Series A, Lightspeed) | 250+ managed integrations, OAuth handling, sandboxed execution, observability |

**The N×M problem:** Without middleware, every agent builder independently manages credentials, auth flows, rate limits, error handling, and API schema changes for every tool. At enterprise scale (agent touching CRM + ticketing + email + calendar + docs + financial systems in one workflow): impossible without middleware.

**Durability: High for near term.** Long-term risk: MCP standardization could reduce managed integration value.

### Layer 5: Provisioning and Billing
**Status: First credible entrant arrived.**

Agents need to buy things. [[stripe-projects]] is the first credible trust layer for agent-to-service transactions.

| Company | Approach |
|---|---|
| [[stripe-projects]] | CLI provisioning; Stripe tokenizes payment into scoped credentials; raw card never leaves vault |

**What's missing:** agent-to-agent payments, metered billing for agent compute patterns (SaaS billing assumes human usage curves), dynamic budget allocation (agent A can spend up to $X without human approval), financial observability across multi-agent workflows.

**Durability: High for Stripe specifically.** Payments is a trust problem — Stripe already solved trust at scale. New-entrant opportunity is metering, budgeting, and financial orchestration on top of the payment rail.

### Layer 6: Orchestration and Coordination
**Status: The layer that matters most and exists least.**

The agent needs to work with other agents reliably, at scale, with fallback handling, audit trails, and cost controls.

**Evidence of the gap:**
- Gartner: 1,445% surge in multi-agent system inquiries (Q1 2024 → Q2 2025)
- Deloitte: autonomous agent market 15-30% larger if orchestration improves
- Celonis: 76% enterprises report sub-optimal processes holding back agentic AI, despite 85% wanting to become agentic enterprise within 3 years

**Current tooling (LangChain, CrewAI, AutoGen):** framework-level, not infrastructure-level. Gap between "spin up 3 agents in a notebook" and "reliably run 50 agents across enterprise systems with failure recovery, cost controls, audit logging, and human escalation paths" is enormous.

**The five things that don't exist yet (see [[agent-orchestration]]):**
1. Scheduling and lifecycle layer (Kubernetes for agents)
2. Merge and coordination infrastructure for parallel agent work
3. Supervision hierarchies as infrastructure
4. Financial observability across agent workflows (FinOps for agents)
5. Standard failure modes and recovery patterns

**Durability: This is where the next infrastructure-defining company gets built.** Structurally analogous to Kubernetes solving container orchestration.

## Reliability Math

**The compounding problem:** 5 primitives at 99% each = 95% end-to-end. At 97% each = 86%.

This is the microservices fragility problem, except worse — agents are non-deterministic, so failures are harder to diagnose.

## New Builder Skills
- **Context engineering > prompt engineering** — what you feed the agent matters more than how you word the request
- **Eval-driven development** — success rate and cost per task replace unit tests as the core feedback loop
- **Stack literacy** — which layer is your competitive advantage, which you rent, which commoditizes underneath you

## Related Concepts
- [[agent-orchestration]] — the gap that matters most; Layer 6 deep dive
- [[mem0]], [[composio]], [[e2b]], [[daytona]], [[browserbase]], [[agentmail]], [[stripe-projects]] — the companies filling these layers
- [[agent-stack-six-layers]]] — memory and state layer deep dive
- [[agent-stack-six-layers]]] — identity/shim problem deep dive
- [[agent-reliability-math]] — compounding reliability math
