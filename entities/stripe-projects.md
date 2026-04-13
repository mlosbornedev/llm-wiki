---
title: Stripe Projects
created: 2026-04-13
updated: 2026-04-13
type: entity
tags: [company, billing, provisioning, payments, agentic, ai-stack]
sources: [raw/transcripts/nate-agent-stack-six-layers-2026.md]
---

# Stripe Projects

Stripe's agent provisioning and billing product. First credible trust layer for agent-to-service transactions. Launched late March 2026.

## Key Facts
- **Launch:** Late March 2026
- **What it does:** Agents use the same CLI commands a developer would. When provisioning a database or upgrading a hosting tier, Stripe tokenizes payment credentials into a shared payment token and grants the provider a scoped credential for that specific transaction. Raw card details never leave Stripe's vault.

## The Gap It Closes
Since early 2026, agents can take a project from `git init` to a running app autonomously — **except** for creating accounts and provisioning infrastructure. That's the gap Stripe Projects closes.

### Neon (Co-design Partner)
- Databases ready in 350 milliseconds
- Free to start
- Scale to zero when inactive
- Every design choice built for agent-speed provisioning, not human-speed dashboard clicking

## What's Still Missing
1. **Agent-to-agent payments** — no infrastructure for this yet
2. **Metered billing for agent compute patterns** — SaaS billing assumes human usage curves; agents behave differently
3. **Dynamic budget allocation** — agent A can spend up to $X without human approval
4. **Financial observability across multi-agent workflows** — no consolidated view

## Durability
High for Stripe specifically. Payments is a trust problem — Stripe already solved trust at scale. New-entrant opportunity is in the metering, budgeting, and financial orchestration layer **on top of** the payment rail.

## Position
[[agent-stack-six-layers|Layer 5: Provisioning and Billing]]

## Related Concepts
- [[agent-stack-six-layers]] — where Stripe Projects fits
- [[agent-orchestration]] — Layer 6; financial observability across workflows is one of the five missing pieces
