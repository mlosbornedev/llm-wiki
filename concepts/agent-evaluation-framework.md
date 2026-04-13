---
title: Agent Evaluation Framework
created: 2026-04-13
updated: 2026-04-13
type: concept
tags: [ai-agents, evaluation, testing, frameworks]
sources: [raw/transcripts/nate-agent-testing-cowork-lindy-sauna-opal-2026-04-04.md, raw/transcripts/nate-55-percent-regret-ai-layoffs-2026-03-21.md]
---

# Agent Evaluation Framework

The core insight: AI agents are reliable in environments with automated feedback (code compiles, tests pass) but unreliable in knowledge work where humans are the only feedback mechanism.

## Why Code Worked First
| Environment | Feedback Type | Agent Reliability |
|-------------|---------------|------------------|
| Software/Code | Automated (compile, tests) | High |
| Knowledge Work | Human-dependent | Low |

## Three Questions for Evaluating Any Agent
1. **Where does it run?** (Cloud vs. local)
2. **Who controls the model?** (Platform vs. user)
3. **What does it assume about me?** (Expert vs. novice)

## Key Principles
1. **Memory Architecture** — Does the agent store and retrieve context properly?
2. **Inspectable Surfaces** — Can you verify what the agent did and why?
3. **Compounding Context** — Does the agent improve over time with feedback?

## The Grigorev Incident
Alexey Grigorev's AI coding agent wiped 1.9M rows of student data. The agent never made a technical error — every action was locally correct. It couldn't distinguish real infrastructure from temporary copies. That knowledge only existed in the engineer's head.

## Key Statistic
The best-performing outcome agent scored **1 out of 4** on the evaluation framework.

## The Middleware Trap (OpenClaw)
Agents executing on broken data models, unmapped workflows, or misaligned org structures inherit all three and execute on them at machine speed — with full confidence.

## Related
- [[agent-reliability-math]] — compounding fragility across agent primitives
- [[openclaw-ban]] — the specific case of agent deployments going wrong
