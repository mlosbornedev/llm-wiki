---
title: Check Point SR Number Format
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [sr-workflow, tac, check-point-sr]
sources: []
---

# Check Point SR Number Format

## Format
All Check Point Service Request numbers follow this format:

```
6-XXXXXXXXX
```

Where the 6 indicates it's a TAC SR (not a partner/CPE number).

## SR Work Directory
All SR work is organized in:
```
/home/michael/Work/<SR-number>/
```

Example:
```
/home/michael/Work/6-0004564689/
```

## SR Scaffold
Each SR directory follows the standard scaffold. See [[sr-scaffold-workflow]].

## SR Type Tags
Common SR categories that should be tagged:
- `vsx` — Virtual System Extension issues
- `clusterxl` — ClusterXL HA/LS configuration
- `maestro` — Maestro orchestrator/Security Group
- `vpn-issue` — VPN tunnel, site-to-site, remote access
- `hw-platform` — Hardware failure, appliance issues
- `needs-rd` — Requires Check Point R&D escalation
- `performance` — Throughput, latency, connection capacity
- `passthrough` — Transparent bridging/L2 mode

## Related
- [[sr-scaffold-workflow]]
- [[sr-6-0004564689-golfview]]
- [[check-point-company]]
