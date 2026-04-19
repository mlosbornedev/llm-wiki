---
title: Program.md
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [ai, software, research]
sources: [~/Documents/Last30Days/program-md-eval-harness-autoresearch-raw-v3.md]
---

# Program.md

`program.md` is the **human's primary output** in a Karpathy Loop setup — a plain-English instruction file that tells the agent what directions are worth exploring, what constraints to respect, and what counts as progress. It is the mechanism by which human domain knowledge and research taste gets injected into an otherwise autonomous loop.

## What It Contains
`program.md` tells the agent:
- **What to explore** — Which hypotheses are worth testing
- **What to avoid** — Which directions are known dead ends
- **Constraints** — Time budgets, resource limits, style guidelines
- **Context** — Why this optimization matters, who the end user is

Per Data Science Dojo: *"In autoresearch, you don't touch the Python files at all. Instead, you write program.md — a plain English instruction file that tells the agent what kinds of directions are worth exploring, what to avoid, and what counts as progress."*

## The Human's Actual Job
The skill in writing `program.md` is **research taste** — knowing which directions will compound versus which will plateau. As one practitioner noted: *"This is where the real research taste shows up. program.md tells the agent what kinds of directions are worth exploring, what to avoid."*

This is a **higher-skill, higher-impact** role than manually running experiments:
- Requires deep domain knowledge
- Requires understanding of what metrics actually reflect value
- Requires ability to spot when the agent is gaming the proxy instead of improving the outcome

## Markdown Flows Both Ways
The human communicates through `program.md` updates. The agent communicates through git commits. As one analysis put it: *"Markdown flows in both directions. Building Git-based experiment tracking into the loop makes the whole thing auditable."*

## Relation to Agentic Engineering
In broader agentic engineering (Vivek Haldar): *"A harness is basically what we used to call scaffolding. Claude Code is a harness around the base Anthropic Claude models. The job of a harness is to run some kind of reasoning, planning, tool use loop on top of a model to accomplish a certain task."*

`program.md` is the human-authored document that defines the harness's goals.

## Limitations
Karpathy himself noted the loop is **not fully autonomous**: *"Atm it's not a fully autonomous process, I add every source manually, one by one and I am in the loop, especially in early stages. After a while, the LLMs 'gets' the pattern and the marginal document is a lot easier."*

The human remains the bottleneck for research direction — the agent runs fast, but the human has to point it somewhere useful.

See also: [[karpathy-loop]], [[eval-harness]], [[autoresearch]], [[local-hard-takeoff]]
