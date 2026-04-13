---
title: MCP — Model Context Protocol
created: 2026-04-12
updated: 2026-04-12
type: concept
tags: [ai, protocol, open-source, standard, anthropic]
sources: [raw/transcripts/nate-anthropic-conway-agent-os-2026.md]
---

# Model Context Protocol (MCP)

Anthropic's open standard for connecting AI tools to data sources. Published by Anthropic. Adopted by OpenAI, Google. Hosted by the Linux Foundation. Designed to be the universal connector between AI tools and data sources.

## What It Does
Any AI client can talk to any data source through one open protocol. If you built Open Brain, you're running an MCP server right now.

## The Tension: MCP vs .cnw.zip

[[conway]] uses MCP — but Conway's `.cnw.zip` proprietary extension format sits on top and creates a proprietary layer. This is the Google Play Services pattern applied to AI:

- MCP = the open foundation (Android Open Source Project)
- .cnw.zip = the proprietary layer on top (Google Play Services)

Anthropic gets the **credibility of publishing an open standard** and the **commercial advantage of building valuable tooling on a format that only runs in their environment**.

## The Developer Dilemma
Build a standard MCP tool (portable, no distribution) or build a Conway extension (proprietary .cnw.zip, but inside the app store)? See [[cnw-extension-format]] for the full analysis.

## Related Concepts
- [[cnw-extension-format]] — the proprietary layer on top of MCP
- [[conway]] — the agent that uses both MCP and .cnw.zip
- [[anthropic-platform-strategy]] — why Anthropic publishes open standards while building proprietary layers
- [[intelligence-portability]] — the question of whether tools built on MCP will ever be truly portable
