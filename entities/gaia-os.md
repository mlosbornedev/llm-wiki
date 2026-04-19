---
title: Gaia OS
created: 2026-04-19
updated: 2026-04-19
type: entity
tags: [gaia-os, software, r77, r80, r81, r82]
sources: []
---

# Gaia OS

## Overview
Gaia is Check Point's standard operating system for all security appliances and virtual gateways. Based on a hardened Red Hat / CentOS Linux foundation. Replaced the legacy SPLAT (SecurePlatform) OS starting in R77.

## Access Methods

### CLISH (Check Point Interactive Shell)
Standard CLI. Use for:
- Interface configuration
- Routing (static, OSPF, BGP, RIP)
- Cluster configuration
- System diagnostics

### Expert Mode
Root bash access. Use for:
- Debugging (fw ctl commands)
- Kernel parameter tuning
- Advanced troubleshooting
- Viewing /var/log/* files directly

```bash
# Enter expert mode
clish -c "set expert-mode on"
exit  # return to clish

# Or from expert back to clish
exit  # from expert → clish
```

## Key Commands

### Network / Interface
```bash
show interface <name>
show interfaces
add interface <name> type <type>
set interface <name> ipv4-address <ip>/<mask>
set interface <name> state on/off
```

### Routing
```bash
show route
set static route <dest> nexthop <gateway> priority <N>
set ospf <config>
set bgp <config>
```

### Cluster
```bash
show cluster state
show cluster members
cphaprob state
cphaprob list
```

### Performance / Monitoring
```bash
cpview              # Interactive performance view
cpstat fw          # Firewall stats
cpstat -f fw -p    # Per-CPU fw stats
fw ctl affinity -l # CPU affinity
sim affinity -l    # SIM affinity
```

### Gaia WebUI
HTTPS on port 443. Enable via:
```bash
set webgui enable
```

## Configuration Files
- `/etc/conf/mcpd.conf` — management plane
- `/etc/fw.boot` — firewall bootup
- `$FWDIR/state.local` — firewall runtime state
- `/var/log/` — all system and firewall logs

## Version History
- **Gaia R77** — Initial release, SPLAT replacement
- **Gaia R80.x** — Added policy layers awareness, VRRP changes
- **Gaia R81.x** — WebUI refresh, new clish syntax for VSX
- **Gaia R82** — API-first management, new monitoring stack

## Related
- [[check-point-company]]
- [[clusterxl-redundancy]]
- [[vsx-virtual-systems]]
- [[r82-release]]
