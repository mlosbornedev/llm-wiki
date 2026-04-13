# Agent 80% Plumbing — 12 Blind Spots (Nate's Substack)

## Source Metadata
- **Title:** Your Agent Is 80% Plumbing. Here Are the 12 Pieces You're Missing.
- **Author:** Nate (Nate's Substack)
- **Published:** April 3, 2026
- **URL:** https://natesnewsletter.substack.com/p/your-agent-has-12-blind-spots-you
- **Type:** Substack article (paid)

## The Leak That Changed Everything
Anthropic accidentally published the full source code of Claude Code:
- 1,902 files
- 512,000+ lines of code
- 29 subsystems
- ~$2.5 billion in annualized revenue exposed

## Core Thesis
```
LLM Call = ~20%
Plumbing = ~80%
```

The 80% that matters:
- Session Persistence — Sessions must survive crashes
- Permission Pipelines — Tools run with proper authorization
- Context Budget Management — Prevent context window overflow
- Tool Registries — Organized, secure tool access
- Security Stacks — Multi-layer protection
- Error Recovery — Graceful handling of failures

## The Tutorial Problem
Every "how to build agents" tutorial stops at the demo stage: get the prompt right, wire up tool calling, ship it. What tutorials miss:
- Sessions don't survive crashes
- Tools run without permission
- Context windows overflow
- Costs spiral out of control
- No way to diagnose what went wrong

## What's Promised
1. Two Leaks, One Week — AI-assisted development velocity outrunning operational discipline
2. The 12 Infrastructure Primitives (Prioritized) — organized by day one, week one, month one
3. An 18-Module Security Stack
4. Cross-Language Validation — patterns ported to Python/Rust within hours
5. Architecture Audit Tool — a prompt that interviews you about your agent system and returns gap analysis

## Key Insight
```
Model quality (the 20%)  ──────►  Is commoditizing rapidly
Infrastructure (the 80%)  ──────►  Is where competitive moats form
```
