---
title: Behavioral Lock-In
created: 2026-04-12
updated: 2026-04-12
type: concept
tags: [ai, lock-in, behavioral, switching-costs, platform]
sources: [raw/transcripts/nate-anthropic-conway-agent-os-2026.md]
---

# Behavioral Lock-In

The deepest form of vendor lock-in the technology industry has ever produced. Created by always-on agents like [[conway]] that accumulate understanding of how you work over time.

## Why It's Different From Previous Lock-In

| Platform | What It Locked In | Switch Pain |
|---|---|---|
| Microsoft Windows | Files | Exportable, consultants exist |
| Salesforce | Customer records | Data export tools |
| Slack | Communication history | Export available |
| [[conway]] | How you think/work | No export path exists |

When you switch away from [[conway]] after six months:
- You don't just lose an agent
- You lose six months of compounding that made the agent useful
- You're back to a "brilliant stranger"
- The new platform's agent starts from zero, learning you all over again
- You remember how good it was when the last one **just knew**

## The Switching Cost Is Asymmetric
- **You** spent 6 months teaching the agent how you work
- **You** reviewed and corrected its outputs every day, building its model of your preferences
- **You** bear the cost of re-teaching a new agent from scratch
- The new agent gets a head start from everyone else's behavioral data, but your specific context is gone

## The Competitive Implication
Whoever owns the persistent agent layer owns the customer — not because the model is better, but because the switching cost of abandoning 6 months of accumulated behavioral context is **prohibitive**.

This is why [[anthropic-platform-strategy]] matters: [[conway]] is the "Active Directory play" that makes everything else in the stack sticky.

## The Honest Caveat
[[conway]] will be ~1/3 wrong in its outputs. But the ratio of errors shifts toward less babysitting every month. The article acknowledges running agent workflows daily: "transformative and they require babysitting. Both are true."

The behavioral lock-in builds even through imperfect outputs — because the user corrects the agent, which refines the model, which reduces errors, which increases dependency.

## Related Concepts
- [[conway]] — the always-on agent creating this lock-in
- [[intelligence-portability]] — the new category of lock-in, with no export path
- [[anthropic-platform-strategy]] — why Anthropic is building toward this
- [[anthropic]] — the company creating behavioral lock-in
