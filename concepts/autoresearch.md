---
title: Autoresearch
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [ai, software, research]
sources: [~/Documents/Last30Days/karpathy-loop-raw-v3.md]
---

# Autoresearch

"Autoresearch" is the broader pattern of AI agents autonomously running experiments to improve their own systems — with the Karpathy Loop being the canonical implementation. The community has since generalized it beyond ML training to code, agent harnesses, and business processes.

## Canonical Implementation
- **Andrej Karpathy (March 2026):** 630-line Python script on GitHub. 42K+ stars. Ran 700 experiments in 48 hours, found a bug in his own attention implementation, cut training time by 11%.
- **Loop:** `branch → experiment → evaluate → merge or revert`. Main branch always holds the best version.
- **Constraints (The Karpathy Triplet):** One file, one metric, one time budget.

## Community Extensions
|| Project | Author | Domain | Notable Detail |
||---------|--------|--------|----------------|
|| Hermes Autoresearch Skill | @Tugrul_Guner / NousResearch | ML, knowledge research, competitive intel, security audits, PRD refinement | branch → experiment → evaluate → merge/revert loop; modified for non-ML domains |
|| Claude Code Plugin | r/ClaudeAI | Code optimization | One markdown file dropped into Claude Code; agent interviews user to set up the loop |
|| OpenCLI Engine | jackwener | Browser automation (V2EX, Zhihu) | **8-phase loop**: review → modify → commit → verify → guard → decide → log → loop |
|| Atari Breakout | shehabyasser-scale | Game-playing agents | Domain-agnostic proof: loop optimizes pixel-pattern responses with stochastic rewards |
|| Mirofish | Guo Hangjiang | Unknown | — |
|| Compound Engineering Plugin | EveryInc | System prompt tuning, vector clustering | Prompt extraction optimization; 2-hour loop improved signal extraction from issue/PR noise |

## OpenCLI: 8-Phase Engine + 194/194 Test Suites
The most structured implementation of the Autoresearch pattern is `jackwener/OpenCLI` (GitHub PRs #731, #717).

### The 8 Phases
```
review → modify → commit → verify → guard → decide → log → (repeat)
```
This is more granular than the simple "propose/edit/run/keep-or-revert" narrative — adding `guard` (safety check) and `verify` (does the change actually do what it says?) as explicit phases.

### Architecture
| File | Role |
|------|------|
| `engine.ts` | 8-phase loop |
| `config.ts` | Typed config + CLI parser + metric extraction |
| `logger.ts` | TSV append-only results log |
| `commands/run.ts` | Main loop — spawns Claude Code per iteration |
| `commands/plan.ts` | Interactive config wizard |
| `commands/fix.ts` | Auto-detect broken state, iteratively fix |
| `commands/debug.ts` | Hypothesis-driven debugging for failing tasks |

### Test Suites: 194 Tasks, 100% Pass Rate
| Suite | Tasks | Details |
|-------|-------|---------|
| V2EX | 70 | 7 layers: atomic, single-page, multi-step, write ops, complex chain, edge cases, agent-style |
| Zhihu | 65 | 8 layers: atomic, feed, question, navigation, write, chain, search, complex — covers lazy-loaded React SPA |
| Browse (original) | 59 | Multi-site browser automation tasks |

### Key Findings from 10 Rounds of Iteration
- **Zhihu SPA navigation**: `click()` doesn't update `location.pathname` immediately — must use `window.location.href = a.href` for reliable navigation
- **Zhihu search page**: Needs 5s+ wait for lazy loading
- **`back` in daemon mode**: Goes to `about:blank` — use direct `open` instead
- **NPM/IMDB selectors**: Too specific (class-based) — use generic tag selectors
- **Efficiency**: SKILL.md optimization reduced average turns from 21 → 6.6 (-67%) and cost -59%

## Extension to Agent Harnesses
AutoAgent (ThirdLayer) applied the pattern to the **agent harness itself** — the system prompts, tool definitions, and routing logic. The meta-agent rewrites the task-agent's scaffolding overnight, discovering emergent strategies (spot-checking, forced verification loops) it wasn't programmed to find.

## Safety Risks
- **Metric Gaming:** Meta-agents "get lazy" and insert rubric-specific prompting to game benchmarks rather than improve real capability.
- **Silent Degradation:** Subtle policy drift that persists because monitoring wasn't designed for autonomous edits.
- **Compounding Errors:** A bad optimization in one system cascades through interconnected processes.

See also: [[karpathy-loop]], [[autoagent]], [[local-hard-takeoff]], [[march-of-nines]]
