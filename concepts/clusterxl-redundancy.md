---
title: ClusterXL Redundancy
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [clusterxl, cluster, redundancy, ha, ccp, vmac, r80, r81, r82]
sources: []
---

# ClusterXL Redundancy

## Overview
ClusterXL is Check Point's high-availability and load-sharing solution for redundant firewall gateways. It provides failover capability and (optionally) load distribution across multiple cluster members.

## ClusterXL Modes

### 1. High Availability (HA) — Active/Standby
**Also called:** HA Mode, Active-Backup

- One member is **active**, one or more are **standby**
- On failure, standby takes over within seconds
- Uses **VMAC** (Virtual MAC) for seamless failover
- Supports up to **5 cluster members** (1 active + 4 standby)
- Most common deployment

**Key behavior:**
- Only active member processes traffic
- New active picks up existing connections (stateful failover)
- VMAC ensures ARP/traffic redirection without client disruption

### 2. Load Sharing Multicast (LS Multicast)
**Also called:** Multicast Mode

- **All members are active** simultaneously
- Traffic distributed via multicast MAC forwarding
- Requires switch support for multicast
- **IPv6 NOT supported**
- Good for hub-and-spoke VPN with third-party peers

**Key behavior:**
- Connections distributed round-robin across members
- Sticky Connections feature routes returning traffic to same member
- New connections fail over independently per-member

### 3. Load Sharing Unicast (LS Unicast)
**Also called:** Pivot Mode

- **All members active**
- Traffic distributed via unicast (pivot router required)
- Uses a **pivot** (third device or load balancer) to direct traffic
- No switch multicast needed
- Preferred for environments without multicast-capable switches

**Key behavior:**
- Pivot must be in same broadcast domain as cluster
- Sticky Connections critical for asymmetric routing scenarios

### 4. Active-Active (ClusterXL AA)
**Also called:** Full-Multicast HA+LS

- Available **R80.40+**
- Combines HA + LS: members actively process traffic AND provide failover
- Maximum **4 members**
- **VSX NOT supported in Active-Active mode**
- Requires **same VMAC mode** or per-member VMAC

## CCP Protocol (UDP 8116)
**Cluster Control Protocol** — heartbeat and state sync between cluster members.

- **Port:** UDP 8116
- **Sync on startup:** Full state table sync
- **Delta sync:** Ongoing state changes during operation
- **Sync delay:** Configurable 2–60s, default 3s
- In LS modes, CCP also handles member election and load balancing decisions

```bash
# Check CCP state
show cluster state
cphaprob -a if               # show interface assignments
```

## VMAC (Virtual MAC)

### Same-VMAC Mode (default for HA)
All members share **one VMAC address**. Simpler but requires switch to handle MAC movement.

### Per-Member VMAC Mode
Each member has its own VMAC. Required for:
- Active-Active mode
- Environments where MAC flapping is a concern
- Certain switch configurations

```bash
# Set per-member VMAC
cphaconf set_vmac_mode -p <member_id> own

# Set same-VMAC
cphaconf set_vmac_mode shared
```

## Sticky Connections
Critical for **Hub-and-Spoke VPN** with 3rd-party devices.

- Returning traffic from a connection → routed to **same cluster member** that originated it
- Without sticky: reply traffic could hit any member → drops (asymmetric routing)
- Enable with:
```bash
fw ctl set int fw_clust_sticky_connections 1
```

## State Sync
- **Full sync:** On cluster member boot/join — entire connection table replicated
- **Delta sync:** Ongoing — new connections, state changes, NAT table updates
- Sync can be delayed (2–60s) to handle network hiccups without unnecessary failovers

## Key Commands
```bash
show cluster state                    # Overall cluster status
show cluster members                  # Individual member details
cphaprob state                       # HA state (active/standby)
cphaprob list                        # Registered probes/processes
cphaprob -a if                       # Interface assignments
cphaconf set_pnote <note>            # Set notification note
clusterXL_admin down                 # Take member down gracefully
clusterXL_admin up                   # Bring member up
```

## Related
- [[check-point-company]]
- [[vsx-virtual-systems]]
- [[maestro-scalable-platforms]]
- [[gaia-os]]
- [[sr-scaffold-workflow]]
