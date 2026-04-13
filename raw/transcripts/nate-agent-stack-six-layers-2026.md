# AI Agent Stack — Six Layers (Nate's Substack)

## Source Metadata
- **Title:** Your AI Agent Depends on Six Layers — Here's Which Ones Won't Last
- **Author:** Nate
- **Published:** April 6, 2026
- **Type:** Substack article
- **Date captured:** 2026-04-13

## Summary

A new infrastructure stack is forming underneath AI agents. Almost nobody can name which layers will last. The stack is for AI agents as the primary user, not humans.

**The analogy:** system calls, not Lego bricks. Agents need defined, reliable interfaces to identity, compute, memory, persistence, communication, and payments — same way processes need system calls to interact with hardware.

**Two previous analogous shifts:**
1. 2006-2015: on-premise → cloud primitives (EC2, S3, Lambda)
2. 2012-2016: monolithic apps → composable APIs/microservices
3. Now: human-first tools → agent-first primitives

**Six layers:**

### Layer 1: Compute and Sandboxing
- Status: Production-ready. Load-bearing wall.
- Competition: E2B ($32M, Firecracker microVMs), Daytona ($24M Series A, Docker containers, 90ms cold starts), Modal (GPU-heavy), Browserbase ($300M Series B, headless browser)
- Split: ephemeral vs persistent sandboxes
- Durability: High. Specialist wins in medium term.

### Layer 2: Identity and Communication
- Today: give the agent an email address
- AgentMail: $6M seed, lets agents create email inboxes programmatically. Thesis: email is the universal key to the internet.
- But: this is a shim. Email was designed for humans.
- Real need: native agent identity and communication protocol (on-chain, A2A, MCP-based discovery)
- Durability: Medium. Works for 2-3 years while native protocols get sorted.

### Layer 3: Memory and State
- Mem0: clear leader. $24M combined, 41K GitHub stars, 14M downloads, AWS exclusive memory provider. 5x growth Q1→Q3 2025 (35M → 186M API calls). Hybrid datastore (graph, vector, key-value). 26% better than OpenAI built-in memory on LOCOMO benchmark, 91% lower latency, 90% reduced token usage.
- Platform risk: frontier labs (OpenAI, Anthropic) building memory into models
- Counter-thesis: "memory passport" — memory travels across apps and models
- Durability: Uncertain. Depends on whether memory becomes model-locked or portable.

### Layer 4: Tool Access and Integration
- Composio: $29M total (Series A, Lightspeed), 250+ managed integrations, auth handling including OAuth, sandboxed execution, observability
- Problem: N×M integration nightmare without middleware
- Durability: High for near term. Long-term risk: MCP standardization.

### Layer 5: Provisioning and Billing
- Stripe Projects: launched late March 2026. First credible trust layer for agent-to-service transactions. Agents provision infra using CLI commands; Stripe tokenizes payment credentials into scoped tokens.
- Neon (co-design partner): databases ready in 350ms, free to start, scale to zero.
- What's missing: agent-to-agent payments, metered billing for agent compute patterns, dynamic budget allocation, financial observability across multi-agent workflows.
- Durability: High for Stripe specifically.

### Layer 6: Orchestration and Coordination
- The layer that matters most and exists least.
- Evidence of gap: Gartner 1,445% surge in multi-agent system inquiries (Q1 2024 → Q2 2025); Deloitte says autonomous agent market 15-30% larger if orchestration improves; 76% of enterprises report sub-optimal processes holding back agentic AI despite 85% wanting to become agentic enterprise in 3 years.
- Current tooling (LangChain, CrewAI, AutoGen): framework-level, not infrastructure-level
- What's missing (5 things):
  1. Scheduling and lifecycle layer (Kubernetes for agents) — agent creation, assignment, health checking, scaling, termination as managed service
  2. Merge and coordination infrastructure — merge queues, conflict detection, resolution protocols for parallel agent work
  3. Supervision hierarchies — meta-agents that monitor, evaluate, course-correct other agents as infrastructure, not code
  4. Financial observability across agent workflows — FinOps for agents
  5. Standard failure modes and recovery patterns — equivalent of HTTP status codes, circuit breakers for agent-to-tool interactions
- Durability: This is where the next infrastructure-defining company gets built.

**Key statistics:**
- E2B: ~$32M total funding
- Daytona: $24M Series A, February 2026
- Browserbase: $300M valuation, Series B
- Mem0: $24M combined, 41K GitHub stars, 14M downloads
- Composio: $29M total, Series A (Lightspeed)
- AgentMail: $6M seed, March 2026
- Gartner: 1,445% surge in multi-agent inquiries
- Deloitte: 15-30% larger autonomous agent market if orchestration improved
- Celonis: 76% enterprises report sub-optimal processes holding back agentic AI

**Reliability math:** 5 primitives at 99% each = 95% end-to-end. At 97% each = 86%. Microservices fragility problem, but worse because agents are non-deterministic.

**New builder skills:**
- Context engineering > prompt engineering
- Eval-driven development (success rate + cost per task as feedback loop)
- Stack literacy (which layer is competitive advantage, which you rent, which commoditizes)
