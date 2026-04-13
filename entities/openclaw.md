---
title: OpenClaw
created: 2026-04-12
updated: 2026-04-12
type: entity
tags: [ai, agent, open-source, community, anthropic]
sources: [raw/transcripts/nate-anthropic-conway-agent-os-2026.md]
---

# OpenClaw

Third-party tool for accessing Claude (and other AI models) through arbitrary interfaces — chat apps, Discord, Telegram, etc. Created by Peter Steinberger. Anthropic blocked it in April 2026, driving users to 10-50x higher per-use pricing for non-approved access.

## What It Was
OpenClaw let you message Claude Code through any interface — the "any channel" layer that Anthropic later copied into Claude Code Channels.

## Timeline
- **January 9:** Anthropic quietly blocked subscription OAuth tokens from working with third-party tools. No advance warning. Framed as "tightening safeguards against spoofing."
- **February 14:** Peter Steinberger (OpenClaw creator) announced he's joining OpenAI. Sam Altman called him "a genius" who would "drive the next generation of personal agents." OpenClaw moved to a foundation with OpenAI backing.
- **Days after hire:** Anthropic revised Terms of Service to explicitly prohibit third-party tools from using subscription credentials.
- **Friday (April 2026):** Full enforcement — OpenClaw cut off, along with all other third-party tools.

Steinberger and investor Dave Morin tried to negotiate a softer landing. By their account, they managed to delay enforcement by a single week.

## Steinberger's Own Read
> "First they copy some popular features into their closed harness, then they lock out open source."

## The Pattern
OpenClaw's experience is the template for how [[anthropic-platform-strategy]] works:
1. Build first-party version of community tool (OpenClaw → Claude Code Channels)
2. Make first-party free/subsidized
3. Make third-party expensive/impossible
4. Ship proprietary extension format ([[cnw-extension-format]])

## Related Concepts
- [[anthropic-platform-strategy]] — the strategy that killed OpenClaw
- [[conway]] — the always-on agent that replaces the need for third-party agent interfaces
- [[anthropic]] — the company that blocked OpenClaw
- [[cnw-extension-format]] — the proprietary extension format that's next in the pattern
- [[mcp-model-context-protocol]] — the open protocol OpenClaw was built on
