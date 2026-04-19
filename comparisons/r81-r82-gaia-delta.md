---
title: R81.20 vs R82 Gaia Delta
created: 2026-04-19
updated: 2026-04-19
type: comparison
tags: [comparison, gaia-os, r81, r82]
sources: []
---

# R81.20 vs R82 Gaia Delta

**Scope:** R81.20 → R82 Gaia Admin Guide delta

## `show version all`
- **R81.20:** Documented in Gaia Admin Guide as full version output command.
- **R82:** Confirmed present; same function.
- **Delta:** No functional change.

## `show interfaces all`
- **R81.20:** Interface state and addressing from Gaia CLISH.
- **R82:** Same command; confirmed in both R81.20 and R82 guides.
- **Delta:** No change.

## `show route all`
- **R81.20:** Routing table inspection from Gaia CLISH.
- **R82:** Same command; confirmed in Gaia and Gaia Advanced Routing guides.
- **Delta:** No change.

## `show configuration`
- **R81.20:** Effective Gaia configuration export.
- **R82:** Same function.
- **Delta:** No change.

## `show asset`
- **R81.20:** Hardware/platform asset information.
- **R82:** Same command; confirmed in both guides.
- **Delta:** No change.

## Gaia Advanced Routing
- **R81.20:** `show route ospf/bgp/rip`, `set ospf/bgp/rip area/neighbor/redistribute`.
- **R82:** Same commands; same routing protocols.
- **Delta:** Consistent across both releases.

---

## Key Takeaway
Gaia CLISH commands for basic system inspection (`show version all`, `show interfaces all`, `show route all`, `show configuration`, `show asset`) are stable between R81.20 and R82. No deprecation or syntax changes documented in the ingested guides.

---

## Related Pages
- [[gaia-os]] — Gaia OS overview
- [[clusterxl-r81-r82-delta]] — ClusterXL delta
- [[vpn-r81-r82-delta]] — VPN delta
- [[r82-release]] — R82 release overview
