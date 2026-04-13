---
title: Intelligence Portability
created: 2026-04-12
updated: 2026-04-12
type: concept
tags: [ai, policy, law, lock-in, portability, behavioral, enterprise]
sources: [raw/transcripts/nate-anthropic-conway-agent-os-2026.md]
---

# Intelligence Portability

The new category of lock-in that [[conway]] creates. A term for the fact that the accumulated model of how you work — behavioral context, not just data — has no export path and no portability standard.

## The Core Problem

Previous platform lock-in was about **stuff**:
- Microsoft locked in your **files**
- Salesforce locked in your **customer records**
- Slack locked in your **communication history**

Stuff is painful to migrate but possible. Export tools exist. Consultants specialize in it. Switching cost is months and dollars.

[[behavioral-lock-in]] locks in something different:
- Not your files — the **patterns the agent learned by watching you use them**
- Not your Slack messages — the understanding of which messages you respond to in 5 minutes and which you ignore for 3 days
- Not your calendar — the knowledge that you always reschedule 2pm on Thursdays

That model doesn't export. There's no CSV of "how this person thinks." No migration consultant for behavioral context.

## The Legal Question
No legal framework exists yet because the category didn't exist until now. Key questions:
- Who owns the model of how you work that an always-on agent built?
- Can you take it with you when you switch platforms?
- In what format?
- Does it belong to the user, the employer, or the AI provider?

## The Employer vs. Worker Tension
The employer will argue: the workflow was created on company time, using company systems and data, so the agent's memory is corporate IP.

Workers (especially in Europe under GDPR) may argue: workflow patterns could be considered personal data, supporting some portability right.

This tension is likely to produce major labor and IP cases over the next several years.

## What Should Exist
The article's position: **behavioral context should be portable**. If you leave a platform, you should be able to export not just your data but the derived context in a format another system can use. Same principle as data portability regulations, extended one layer deeper.

Any enterprise deploying [[conway]] should be negotiating portability clauses in the contract **before deployment**, not discovering the problem at renewal.

The article calls on Anthropic to ship policies around behavioral context portability **before** Conway launches publicly, not after.

## Related Concepts
- [[behavioral-lock-in]] — the mechanism that creates this lock-in
- [[conway]] — the always-on agent that creates behavioral lock-in
- [[anthropic-platform-strategy]] — why Anthropic is building toward this
- [[intelligence-portability]] — this concept (self-referential link for completeness)
