---
title: Conway (Anthropic Always-On Agent)
created: 2026-04-12
updated: 2026-04-12
type: entity
tags: [ai, agent, anthropic, product, always-on, platform]
sources: [raw/transcripts/nate-anthropic-conway-agent-os-2026.md]
---

# Conway

Anthropic's internal always-on agent project, discovered in a source code leak (packaging error pushed ~500K lines to a public registry in April 2026). Not officially announced.

Named after Conway's Law (external link: "organizations which design systems are constrained to produce systems which are copies of their communication structures").

## What It Is

Conway operates as a **standalone sidebar environment** in the Claude interface — not a chat window, a persistent agent environment with three core areas:

- **Search** — query across accumulated context
- **Chat** — interact with the agent
- **System** — configure extensions, connectors, and automatic triggers

### System Section Details
- **Extensions** — install add-ons via `.cnw.zip` files (custom tools, interface panels, information handlers). Conway's proprietary extension format sits on top of [[mcp-model-context-protocol]] — see [[cnw-extension-format]].
- **Connectors and Tools** — shows what services are plugged in; toggle for Claude in Chrome to connect directly to the Conway instance
- **Automatic Triggers** — public web addresses outside services can ping to wake the agent. User controls which services are allowed.

## The Tuesday Morning Scenario
Six months after setup, a typical morning looks like:
- Reviewed 3 emails matching patterns learned over 6 months; drafted responses for 2, flagged 1 from VP
- Monitored Slack channels; drafted reply to #engineering thread using context from a design doc reviewed last month
- Pulled latest numbers from dashboards, compared to last quarter, highlighted 3 biggest deltas with explanations
- **User hasn't typed a word yet**

The article acknowledges ~1/3 of what Conway does overnight will be wrong — but the net is still massively positive, and the ratio of errors shifts toward less babysitting every month.

## Why Conway Matters

Conway is the capstone of [[anthropic-platform-strategy]] — the "Active Directory play" that makes everything else sticky. The persistent agent that knows your organization is the thing you **cannot rip out without losing institutional memory**.

This is [[behavioral-lock-in]] — the most consequential form of vendor lock-in the technology industry has ever produced, deeper than Windows or Active Directory because:

- Windows locked in your **files**
- Active Directory locked in your **credentials**
- Conway locks in **how your people think**

## Three Things to Watch on Launch
1. Does the `.cnw.zip` format stay proprietary or open up?
2. Does OpenAI ship a competing always-on agent before Conway goes public? (Steinberger is at OpenAI specifically to build this)
3. Do enterprises accept a model provider holding their organizational memory?

## Related Concepts
- [[anthropic-platform-strategy]] — the five-move platform strategy Conway is the capstone of
- [[cnw-extension-format]] — the proprietary extension format, Google Play Services pattern
- [[behavioral-lock-in]] — why accumulated behavioral context is deeper than traditional lock-in
- [[intelligence-portability]] — the new category: model of how you work has no export path
- [[mcp-model-context-protocol]] — the open standard that Conway uses but layers proprietary tools on top of
- [[openclaw]] — the third-party tool Anthropic blocked to make Claude Code Channels more attractive
- [[anthropic]] — the company building Conway
