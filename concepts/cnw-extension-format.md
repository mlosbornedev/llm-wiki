---
title: .cnw.zip Extension Format
created: 2026-04-12
updated: 2026-04-12
type: concept
tags: [ai, anthropic, extension, platform, proprietary, mcp]
sources: [raw/transcripts/nate-anthropic-conway-agent-os-2026.md]
---

# .cnw.zip Extension Format

Conway's proprietary extension format. The detail in the [[conway]] leak that exposes the tension at the heart of [[anthropic-platform-strategy]].

## What It Is

Conway uses [[mcp-model-context-protocol]] (the open standard Anthropic published). But Conway's extensions packaged as `.cnw.zip` sit on top of MCP and create a **proprietary layer**:
- Custom interface panels
- Information handlers
- Tools that work specifically inside Conway's environment
- Not portable to other MCP clients (Claude, GPT, Gemini, any MCP-compatible model)

## The Google Play Services Pattern

```
MCP (open foundation)     ← Android Open Source Project
.cnw.zip (proprietary)    ← Google Play Services
```

- Android is open-source, free for anyone to use
- But the commercially viable layer — Maps, payments, push notifications, Play Store — is proprietary
- You can technically build an Android phone without Google services
- In practice, nobody does, because the valuable stuff lives in the proprietary layer

Same here:
- MCP is the open foundation — any AI client can talk to any data source
- `.cnw.zip` is the proprietary layer — tools that only work inside Conway
- The more extensions built as `.cnw.zip`, the more Conway becomes the only place those tools actually work

## Developer Choice

**Path 1 — Standard MCP tool:**
- Portable: works with Claude, GPT, Gemini, any MCP client
- But: no distribution mechanism, no app store, no featured placement
- Building a website in 2008 while everyone downloads iPhone apps

**Path 2 — Conway extension (.cnw.zip):**
- Only works inside Conway
- But: built-in extensions directory, discoverable inside the environment where people already work
- You're in the store from day one

## The Historical Precedent
Mobile developers in 2009 faced the same choice: open web or native iPhone. The open web was the right architectural choice. The App Store made all the money. A decade later, the web still matters but the center of gravity in mobile is native apps through platform-controlled stores.

Amazon tried building Android without Google services (Fire Phone, Fire tablets). It flopped — not because hardware was bad, but because the ecosystem had already organized around Google's proprietary layer.

## What to Watch
On [[conway]] launch: does `.cnw.zip` stay proprietary, or does Anthropic open it up? This is "the whole game" according to the article. Open extensions = Conway is a platform the broader ecosystem can build on. Proprietary = walled garden with open-source foundation for credibility. The history of this pattern (Android, iOS, web) says the proprietary layer wins the economics even when the open layer wins the argument.

## Related Concepts
- [[conway]] — the always-on agent that uses this format
- [[anthropic-platform-strategy]] — the strategy this format serves
- [[mcp-model-context-protocol]] — the open standard this sits on top of
- [[intelligence-portability]] — the broader question of whether tools built inside Conway can ever leave
