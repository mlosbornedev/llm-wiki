---
title: R81.20 vs R82 ClusterXL Delta
created: 2026-04-19
updated: 2026-04-19
type: comparison
tags: [comparison, clusterxl, r81, r82]
sources: []
---

# R81.20 vs R82 ClusterXL Delta

**Scope:** R81.20 → R82 ClusterXL Administration Guide delta

## Core ClusterXL Commands — Unchanged
The following commands are confirmed in both R81.20 and R82 guides with identical behavior:
- `show cluster state` ↔ `cphaprob state` (member state)
- `show cluster members pnotes problem` ↔ `cphaprob -l -ia -e list` (critical device health)
- `show cluster members interfaces all` ↔ `cphaprob -a if` (monitored interface state)
- `show cluster members interfaces vlans` (VLAN trunk monitoring)
- `show cluster statistics sync` ↔ `cphaprob syncstat` (sync health)
- `show cluster failover` ↔ `cphaprob show_failover` (failover history)
- `show cluster bond all` ↔ `cphaprob show_bond <bond>` (bond state)
- `cpstat ha` (HA subsystem status)

## ClusterXL Modes
- **HA (Active/Standby):** Active/standby with VMAC. Max 5 members.
- **LS Multicast:** All members active. Uses IP+Ports+SPIs. No IPv6.
- **LS Unicast (pivot):** Pivot member forwards; VMAC mode.
- **Active-Active (R80.40+):** Max 4 members, no VSX support, CCP encryption.
- All modes confirmed in both R81.20 and R82.

## New in R82: ClusterXL Active-Active Improvements
- R82 ClusterXL Admin Guide explicitly documents Active-Active improvements over R81.20.
- Key enhancement: better support for CCP encryption in Active-Active mode.

## Deprecated
- **`fw hastat`:** Marked as outdated for cluster-member state checks in R82. Use `show cluster state` or `cphaprob state` instead.

## Sync Behavior
- Full sync on startup; Delta sync during operations.
- Sync delay: 2–60s (default 3s) in both releases.
- Consistent across both R81.20 and R82.

## VMAC
- Per-member VMAC or same-VMAC mode available in both releases.
- Consistent behavior.

## Bond Modes
- **HA:** active-backup mode.
- **LS:** LACP/802.3ad mode.
- Consistent across both releases.

---

## Key Takeaway
ClusterXL command syntax and modes are stable between R81.20 and R82. The primary R82 change is improved Active-Active CCP encryption and continued deprecation of `fw hastat` in favor of `show cluster state`/`cphaprob state`.

---

## Related Pages
- [[clusterxl-redundancy]] — ClusterXL full concept
- [[gaia-r81-r82-delta]] — Gaia delta
- [[vpn-r81-r82-delta]] — VPN delta
- [[r82-release]] — R82 release overview
