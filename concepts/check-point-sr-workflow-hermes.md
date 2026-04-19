---
title: Check Point SR Workflow — HERMES Methodology
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [sr-workflow, tac, cpinfo, exp-jira, trello, workflow]
sources: []
---

# Check Point SR Workflow — HERMES Methodology

## Overview
Michael's personal SR workflow — how an SR is taken from first contact through to resolution. Documented from `HERMES.md`.

## Workflow Summary

### 1. New SR Received
- Michael gives new SR number and/or SR information
- Create case scaffolding under `/home/michael/Work/<SR-Number>/`
- Check `/home/michael/Work/Inbox/<SR-Number>/` for pre-existing files
- Move/intake files to appropriate case directories
- Track on Trello board (see below)

### 2. Trello SR Board
**URL:** https://trello.com/b/S3UtkgCa/srs | Board ID: `69ce4a170cc73cf436843e72`

**Lists (left to right = SR lifecycle):**
1. Project Resources — reference materials only
2. Questions For Next CFG Meeting — engineering questions
3. To Do — new/open, not yet being worked
4. Doing — actively being worked
5. Waiting on Customer — blocked on customer
6. Pending Remote — waiting on remote session access
7. Blocked — blocked on engineering/dependencies
8. Done — closed/completed

**Labels:** Urgent (red), New SR (yellow), Needs R&D (purple), VPN Issue (blue), HW/Platform (orange)

### 3. Standard SR Scaffolding
```
/home/michael/Work/<SR-Number>/
├── summary.md         # Current issue, status, hypotheses, latest conclusion
├── timeline.md        # Chronological events: meetings, emails, tests, findings
├── next-steps.md      # Action items with owners and ETA
├── contacts.md        # Customer, vendor, internal contacts + timezone
├── emails/           # Saved email correspondence
├── notes/             # Working notes, analysis, scratch
├── logs/              # CLI output, fw/cpstat dumps
├── artifacts/         # Configs, policy exports, screenshots, vendor artifacts
├── pcaps/             # Packet captures
├── screenshots/       # GUI screenshots specifically
├── exports/           # Reports, Jira exports, generated bundles
└── drafts/            # Customer-facing or TAC-facing writeups
```

### 4. cpinfo Handling
**Tool:** `/home/michael/Work/bin/sr-cpinfo-ingest`

```bash
sr-cpinfo-ingest <SR-NUMBER> <CPINFO-PATH>
sr-cpinfo-ingest 6-0001234567 ~/Downloads/gateway.cpinfo
sr-cpinfo-ingest 6-0001234567 /tmp/gw.tgz --extract-only --security
```

**What it does:**
1. Copies cpinfo to `<SR>/artifacts/`
2. Auto-extracts inner `.info` from wrapped `.cpinfo.info.tar.gz`
3. Runs cpinfo-parser → `<SR>/exports/cpinfo-parser/<YYYY-MM-DD_HHMM>_<basename>/`
4. Appends timeline entry
5. Creates `notes/cpinfo-findings.md` from template
6. Creates SR scaffolding if folder didn't exist

**Options:** `--extract-only`, `--read-only`, `--security`, `--no-copy`

### 5. Known-Issue Matching — Jira EXP Files
**Location:** `/home/michael/Work/Jira/EXP/EXP-<N>.md` (~760 files)

Each EXP file documents: Issue Key, Status, Priority, dates, assignee, product line, trigger, symptoms, affected versions, fix takes, related PRHF/PRJ/PMTR/TM IDs.

**Search approach:**
1. Search for product/version keywords (e.g., "R81.20 take < 99")
2. Search for symptom keywords (error messages, feature names, failure modes)
3. Search for related Jira IDs if referenced by customer/engineering
4. Review matched EXP Description for trigger conditions and affected versions
5. Check fix table for customer's version
6. Record matched EXP ID(s) in SR summary under "Hypotheses / likely cause"

**Rule:** EXP content is internal — paraphrase findings into customer-facing drafts.

### 6. File Naming
- Sortable format: `YYYY-MM-DD description.ext`
- With time: `YYYY-MM-DD_HHMM description.ext`
- Preserve vendor filenames when evidentiary

### 7. Closeout Expectations
- `summary.md` reflects current status
- Key findings and unresolved items recorded
- `next-steps.md` current if work remains

## Related
- [[sr-scaffold-workflow]]
- [[check-point-sr-numbers]]
- [[cpinfo-troubleshooting-runbook]]
