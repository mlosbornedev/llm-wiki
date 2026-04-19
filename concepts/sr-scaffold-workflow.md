---
title: SR Scaffold Workflow
created: 2026-04-19
updated: 2026-04-19
type: template
tags: [sr-scaffold, sr-workflow, tac, escalation, check-point-sr]
sources: []
---

# SR Scaffold Workflow

## Purpose
Standard Check Point Service Request scaffold. One directory per SR, versioned under git. All work products, logs, emails, and research live here.

## Directory Structure
```
6-0004564689/
├── summary.md            # SR overview: customer, problem, status, contacts
├── timeline.md           # Chronological log of all events
├── next-steps.md         # Current action items with owners and ETA
├── contacts.md           # Customer contacts, Check Point teammates, R&D
├── emails/               # Email threads (name + date)
├── notes/                # Working notes, theories, scratch
├── logs/                 # CLI output, fw/cpstat/cphaprob dumps
├── artifacts/            # Configs, policy exports, screen shots
├── pcaps/                # Packet captures
├── screenshots/          # GUI screenshots
├── exports/              # fw sam_policy, cpstat, etc.
├── drafts/               # Draft communications, not sent
└── <arbitrary>/          # Any other subdirectory as needed
```

## File Templates

### summary.md
```markdown
# SR #: Customer Name — Short Problem Description

## Customer
- **Name:** Customer Name
- **Asset/Serial:** XX:XX:XX:XX:XX:XX
- **Platform:** CPXXXX-NS | Maestro | VSX | etc.
- **Software:** RXX.XX | JHF ###

## Problem Statement
One-paragraph description of the issue.

## Status
- [ ] Investigating
- [ ] Waiting on Customer
- [ ] Pending R&D
- [ ] Resolved
- [ ] Closed

## Owner
**Check Point TAC:** Your Name

## Contacts
See contacts.md

## Key Files
- Logs: logs/
- Configs: artifacts/
- PCAPs: pcaps/
```

### timeline.md
```markdown
# SR # Timeline

## [YYYY-MM-DD] Initial Contact
- Customer reported: <description>
- SR opened: <number>
- Assigned to: <TAC engineer>

## [YYYY-MM-DD] <Event Title>
- <action taken>
- <finding>
- <next step>
```

### next-steps.md
```markdown
# SR # Next Steps

## Blocked (waiting on something)
-

## In Progress (being worked now)
- [ ] <task> — @owner — ETA: <date>

## Pending Customer
- [ ] <task> — customer: <name> — ETA: <date>

## Pending R&D / Backend
- [ ] <task> — R&D engineer: <name> — ETA: <date>
```

### contacts.md
```markdown
# SR # Contacts

## Customer
| Name | Role | Email | Phone | Timezone |
|------|------|-------|-------|----------|
|      |      |       |       |          |

## Check Point Internal
| Name | Role | Email | Notes |
|------|------|-------|-------|
|      |      |       |       |
```

## Workflow Rules
1. **Every SR gets a directory** — even if it's a quick phone SR, create the structure
2. **Everything that happened → timeline.md** — prevents "wait, when did we try that?"
3. **Emails go in emails/** — not just in your inbox, preserved and findable
4. **CLI output → logs/** — don't trust screenshots alone, capture actual command output
5. **Tag the SR type in next-steps** — e.g., "VPN Issue", "HW/Platform", "Needs R&D"
6. **Commit to git on key milestones** — don't lose work if laptop dies

## SR Naming Convention
- All SR directories live in `/home/michael/Work/`
- Naming: `6-0004564689/` (SR number with 6- prefix)
- This matches Check Point's internal SR format and makes Trello card naming consistent

## Related
- [[clusterxl-redundancy]]
- [[vsx-virtual-systems]]
- [[maestro-scalable-platforms]]
- [[gaia-os]]
