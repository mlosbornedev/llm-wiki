---
title: Karpathy Loop
created: 2026-04-18
updated: 2026-04-18
type: concept
tags: [ai, software, research, productivity]
sources: [raw/articles/nates-substack-karpathy-loop-2026.md]
---

# Karpathy Loop

The Karpathy Loop is a minimalist design philosophy for autonomous AI research and optimization. It focuses on closing an optimization loop around a specific system to achieve rapid, compounding improvements.

## The Triplet (Core Constraints)
The effectiveness of the loop comes from extreme constraints, not the raw intelligence of the agent:
1. **One Editable Surface:** The agent can only modify a single file (e.g., `train.py` or an agent harness).
2. **One Objectively Testable Metric:** A single, clear numeric metric (e.g., validation bits per byte, benchmark score) that determines if a change is kept or reverted.
3. **Fixed Time Budget:** A strict limit per experiment to ensure high iteration rates.

## Mechanism: The Three Files
The loop is built around exactly **three files**, each with a distinct owner:

| File | Owner | Role |
|------|-------|------|
| `program.md` | Human | Research direction, constraints, what to explore/avoid |
| `agent.py` (or `train.py`) | Agent | The **only mutable file** — what gets edited |
| `prepare.py` / `evaluate.py` | **Locked** | The eval harness — runs experiment, produces score |

**Process:** Agent proposes edit to `agent.py` → runs experiment via locked harness → measures metric → **git commit keeps winners, git reset reverts losers.**

**Iteration Rate:** While a human might run 8-10 experiments a day, a Karpathy Loop can run hundreds overnight (e.g., 700 experiments in 2 days; 910 experiments in 8 hours on a 16-GPU cluster for under $300).

## Reality Check: Not Fully Autonomous
Karpathy himself on X (310 likes):
> *"Atm it's not a fully autonomous process, I add every source manually, one by one and I am in the loop, especially in early stages. After a while, the LLMs 'gets' the pattern and the marginal document is a lot easier."*

The human remains the bottleneck for **research direction** — the agent runs fast, but the human has to point it somewhere useful via `program.md`.

## Evolution: From Code to Harnesses
The pattern has evolved from optimizing ML training code (Karpathy) to optimizing **Agent Harnesses** (e.g., ThirdLayer's AutoAgent).
- **Agent Harnesses:** Optimizing system prompts, tool definitions, routing logic, and orchestration strategy.
- **Meta-Agent vs. Task-Agent:** A key design insight is the separation of the agent *doing* the task from the meta-agent *improving* the harness.
- **Model Empathy:** Same-model pairings (e.g., Claude meta-agent optimizing a Claude task-agent) significantly outperform cross-model pairings due to implicit understanding of reasoning tendencies.

## Ecosystem & Community
The pattern has spawned a broader "Autoresearch" movement:
- **Karpathy's Original Script:** 630 lines, 42K+ GitHub stars. Ran 700 experiments in 2 days, found a bug in his own attention implementation, and cut training time by 11%.
- **Community Ports:** Hermes agent skill (by @Tugrul_Guner/NousResearch), Claude Code plugin (r/ClaudeAI), Mirofish (by Guo Hangjiang), [[autokernel]] (RightNow AI's GPU kernel optimizer), [[clawdmarket]] (first live agent marketplace)
- **Community Variations:** [[hermes-autoresearch]] (Hermes native implementation), [[autoresearch-atari]] (Atari game-playing proof), [[autoresearch-trading]] (algorithmic trading variant)
- **Cost:** High-scale runs via SkyPilot ran 910 experiments on a 16-GPU cluster for under $300 total (~$260 GPU + ~$9 Claude API).
- **Shopify CEO Tobi Lütke:** Ran the pattern on Shopify's templating engine, achieving 53% faster rendering from 93 experiments in 8 hours.

## The "March of Nines" Reliability Problem
A critical debate surfaced by "The AI Automators" on YouTube:
- Reaching the first 90% reliability is "easy" — a good demo.
- But a 10-step workflow at 90%/step = **6+ failures per day**.
- 99%/step = ~1 failure/day.
- 99.9%/step = ~1 failure every 10 days.
- This is the real business value gap — getting from 90% to 99.9% is exponentially harder.

## The Trace Bottleneck
Optimization loops only work if you have **detailed reasoning traces** — not just scores. Without traces:
- The meta-agent performs **random mutations** rather than surgical edits.
- It can't distinguish "this change improved the harness" from "this worked on a small test batch."
- The quality of your trace infrastructure determines the quality of your auto-improvement.

See also: [[agent-orchestration]], [[agent-taxonomy]], [[local-hard-takeoff]], [[march-of-nines]], [[autoresearch]]
