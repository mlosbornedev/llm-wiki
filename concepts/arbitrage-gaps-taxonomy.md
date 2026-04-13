---
title: AI Gap Taxonomy — Five Types of Arbitrage
created: 2026-04-12
updated: 2026-04-12
type: concept
tags: [ai, economics, arbitrage, strategy, gaps]
sources: [raw/transcripts/nate-ai-closing-inefficiencies-arbitrage-2026.md]
---

# AI Gap Taxonomy

Five categories of inefficiency that AI makes newly exploitable. The lens for seeing where value is heading in any industry, role, or business model.

## The Five Gaps

### 1. Speed Gaps
One system updates slower than reality.

**The Polymarket case:** Bot reacted faster than markets could reprice. When Bitcoin moved sharply on Binance, Polymarket's 15-minute contracts were still showing ~50/50 odds. The bot bought the mispriced side, 6,615 times in a row.

**Analogous cases:**
- Competitor's pricing model updates in real-time; yours updates weekly
- Customer support bot resolves issues in seconds; your team takes 24 hours
- Hiring pipeline screens candidates in minutes; yours takes three weeks

**Compresses to:** sub-100ms execution required (73% of Polymarket arbitrage profits now go to bots under 100ms).

### 2. Reasoning Gaps
Not speed — interpretation. Information is public and available to everyone simultaneously. The gap is how quickly and accurately someone can reason about what it means and update their model of the world.

The $2.2M bot didn't have information others lacked. It interpreted public information faster and acted before the crowd caught up. LLMs don't get tired, don't have a backlog, can process the full context of a document in seconds.

**Analogous cases:** Every decision that waits for someone to read, synthesize, and recommend. That wait time is a reasoning gap. Closing it = AI reading and synthesizing faster.

### 3. Fragmentation Gaps
The same thing is priced differently in different places because nobody's looking at all the places simultaneously.

Sports arbitrage: bots scan Polymarket against traditional bookmakers, buying both sides when combined price implies a mathematical edge. Risk-free margin from aggregation.

Business analog: the consultant who charges $50,000 to synthesize five publicly available data sources. Value isn't in the data — it's in the aggregation. AI does aggregation for free.

**Compresses to:** anyone with access to both data sources can see the gap.

### 4. Discipline Gaps
The inefficiency isn't in the market or the information. It's in the human executing.

Same strategy, double the profit when executed by bot vs. human. 6,000 consecutive decisions, each precisely calibrated. No fatigue at 3 AM, no oversized positions on "confident" bets, no missed trades during lunch.

**Analogous cases:**
- Sales team knows the playbook but doesn't follow it consistently
- Content pipeline produces erratic quality depending on who's working
- Operations team drifts from protocol under pressure

AI doesn't close these by replacing the human. It closes them by **enforcing the consistency the human can't maintain alone.**

### 5. Knowledge Asymmetry Gaps
**The big one. The macro layer.**

For 30 years, the dominant gap was the **labor pricing gap**: same work, different cost, depending on geography. Offshore teams existed because the gap between a SF engineer and a Bangalore engineer was wide enough to build an industry in.

AI replaces labor arbitrage with **intelligence arbitrage**. The unit of value shifts from person-hour to outcome. One prompt generates a working system that scales at near-zero marginal cost.

The company that produces a deliverable in 3 hours that its competitor scopes at 3 weeks has an intelligence gap in its favor.

**Unlike the labor gap** (stable for decades — you can't move a million engineers), the **intelligence gap shifts with every model release.**

## The Capture Gap (Sixth — from reader comment)
George Nelson's observation: all five gaps assume data already exists digitally. What about industries where the foundational gap is whether the data exists at all?

Construction: ~80% of what happens in the field never gets captured digitally. You can't close an arbitrage window on inspection data that was never recorded.

**A "capture gap" underneath the five types — data must be captured before any gap becomes exploitable.** Worth noting that this gap is itself closing (sensors, IoT, mobile devices), which will unlock exploitation of the other five in currently pre-digital industries.

## Related Concepts
- [[polymarket-bot]] — proof of concept for speed gap exploitation
- [[micro-turbulence]] — the permanent rotation of gap creation/destruction
- [[upstream-migration]] — where value goes after each gap closes
- [[discipline-gaps]] — the human execution problem
- [[arbitrage-gaps-taxonomy]] — this page (self-referential for completeness)
