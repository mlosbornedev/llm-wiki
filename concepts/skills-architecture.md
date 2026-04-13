---
title: Skills Architecture
created: 2026-04-13
updated: 2026-04-13
type: concept
tags: [anthropic, skills, persistence, agent-memory]
sources: [raw/transcripts/nate-skills-architecture-permanent-2026-03-30.md]
---

# Skills Architecture

Anthropic Skills are reusable, persistent prompting templates that work across platforms. The March 2026 update (shipped March 11 into Excel and PowerPoint sidebars) changed the standard for what a "skill" means.

## The Shift: October → March Standard
| Era | Definition |
|-----|-----------|
| October 2025 | Personal prompting shortcut — stop re-explaining methodology |
| March 2026 | Works when agents call it with no one watching |

The old standard: works when you're at the keyboard.
The new standard: works when agents call it autonomously.

## Four Changes That Broke the Old Framing
1. **Agents invoke skills** as often as humans do, no one watching
2. **Team administration** — single upload provisions skills across entire org
3. **Cross-industry standard** — OpenAI, Microsoft, GitHub, Cursor all adopted
4. **Platform expansion** — same file runs in terminal, Excel, PowerPoint, M365

## The Scale
500,000 skills now running across platforms interchangeably.

## The Failure Asymmetry
- "Good enough for my use" vs. "good enough for agents" are categorically different standards
- Building from outputs beats building from intentions

## Four Prompts
1. **Backlog audit** — Evaluate existing skills for agent-readiness
2. **Output-extraction builder** — Build skills from outputs, not intentions
3. **Agent-readiness stress test** — Test skills without human oversight
4. **Team deployment planner** — Scale skills across orgs

## Related
- [[anthropic]] — Skills shipped into Excel/PowerPoint March 11
- [[intelligence-portability]] — Skills as the mechanism for portability vs. lock-in
