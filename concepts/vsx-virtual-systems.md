---
title: VSX Virtual Systems
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [vsx, virtual-system, virtual-switch, vsx-virtual-device, cluster, r80, r81, r82]
sources: []
---

# VSX Virtual Systems

## Overview
VSX (Virtual System Extension) allows a single physical Check Point appliance to host multiple virtual security gateways. Each virtual system (VS) operates as an independent firewall with its own routing, policies, and security blade stack.

## VSX Components

### Virtual System (VS)
The core virtual firewall. Each VS has:
- Own routing table (independent routing instance)
- Own security policy
- Own interfaces (VLANs or bridge groups)
- Own logs
- Can be managed independently or as part of a VSX cluster

### Virtual Switch (VSW)
A virtual Layer-2 switch that connects:
- VS interfaces to each other
- VS interfaces to physical ports
- VS to VS without routing

### Virtual Device (VSD)
The management context — one VSD runs the VSX management plane, others can run specific VS instances.

## Architecture
```
Physical Appliance
├── VS0 (VSD — VSX Director / Management)
│   └── Manages VSs, runs CPSM (VSX management daemon)
├── VS1 (Virtual System 1)
│   └── Own routing, policy, interfaces
├── VS2 (Virtual System 2)
│   └── Own routing, policy, interfaces
└── VSW0 (Virtual Switch)
    └── Connects VS1/VS2 to physical ports
```

## VSX vs. VRF
- VSX is Check Point's proprietary virtualization (hardware-accelerated)
- VRF-lite is routing-table virtualization (not the same)
- VSX provides **full firewall isolation** — not just routing separation
- Each VS can run its own Threat Prevention, VPN, NAT independently

## VSX Limits (by Platform)
| Appliance | Max VS | Notes |
|-----------|--------|-------|
| CP3200-NS | 3 | Entry-level |
| CP5400-NS | 5 | Mid-range |
| CP5600-NS | 10 | Mid-high |
| CP24000 (R82) | 50+ | Hyperscale |

## Key Commands
```bash
# VSX overview
show vsx all
show vsx virtual-systems
show vsx switches

# VSX diagnostics
vsx_util show
vsx_util -p <vsid> cpu
vsx_util -p <vsid> memory

# Enter VS context
vclid <vsid>        # CLISH in VS context
fw vsid <vsid>      # Firewall commands in VS context

# VSX cluster
show cluster vsx
```

## ClusterXL with VSX
- VSX cluster members can be VSX-enabled appliances
- VS can fail over independently per-VS (not full member failover)
- **Active-Active ClusterXL NOT supported with VSX** — this is a hard limitation
- VSX HA requires VS cluster object in SmartConsole

## Passthrough Mode
VSX can operate in **passthrough** mode — frames pass through without routing/NAT processing. Used when:
- A device behind the VSX needs to be managed by an upstream gateway
- Transparent bridging is required
- See [[sr-6-0004564689-golfview]] for a real-world passthrough configuration

## Related
- [[check-point-company]]
- [[clusterxl-redundancy]]
- [[gaia-os]]
- [[r82-release]]
- [[sr-6-0004564689-golfview]]
