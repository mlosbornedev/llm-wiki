---
title: Routing Command Reference
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [reference, routing, ospf, bgp, rip, r81, r82]
sources: []
---

# Routing Command Reference

**Version scope:** R81.20–R82 | **Confidence:** provisional-medium

## Gaia CLISH Routing Commands

### `show route all`
- **Purpose:** Inspect the routing table from Gaia.
- **What it proves:** Currently selected routes and next-hop visibility.
- **Version scope:** R81.20–R82.

### `show route destination <prefix>`
- **Purpose:** Inspect routing decision for a specific destination.
- **What it proves:** Route choice and path resolution for a queried prefix.
- **Note:** Next refinement pass should extract exact syntax/provenance.

### `show configuration`
- **Purpose:** Correlate configured networking state with routing behavior.
- **What it proves:** Static configuration context when route tables and intent disagree.

## OSPF/BGP/RIP (Gaia Advanced Routing)

### `show route ospf`
- **Purpose:** Inspect OSPF routing table entries.

### `show route bgp`
- **Purpose:** Inspect BGP routing table entries.

### `show route rip`
- **Purpose:** Inspect RIP routing table entries.

### `set ospf area`
- **Purpose:** Configure OSPF area on an interface.

### `set ospf neighbor`
- **Purpose:** Configure OSPF neighbor parameters.

### `set ospf redistribute`
- **Purpose:** Control redistribution into OSPF.

### `set bgp area`
- **Purpose:** Configure BGP autonomous system area.

### `set bgp neighbor`
- **Purpose:** Configure BGP neighbor parameters.

### `set bgp redistribute`
- **Purpose:** Control redistribution into BGP.

### `set rip area`
- **Purpose:** Configure RIP area on an interface.

## Notes
- Routing guides are fully ingested for R81.20 and R82.
- Next refinement pass should extract exact route subcommands and version deltas from normalized PDF text.
- Static routes also configured via `set static route <prefix> gateway <next-hop>`.

---

## Related Pages
- [[gaia-os]] — Gaia OS overview
- [[gaia-r81-r82-delta]] — R81.20 vs R82 Gaia delta
- [[check-point-sr-workflow-hermes]] — SR evidence methodology
