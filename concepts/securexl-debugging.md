---
title: SecureXL Debugging Reference
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [securexl, fwaccel, debugging, r81, r82, reference]
sources: [raw/transcripts/checkpoint-fwaccel-kernel-debug-reference-2026-04-01.md]
---

# SecureXL Debugging Reference

## Overview
SecureXL is Check Point's hardware/software acceleration layer. Debugging it requires coordinating two parallel debug systems: the Firewall kernel debug (`fw ctl debug`) and the SecureXL debug (`fwaccel dbg`). Both must be configured together for complete visibility.

**Source:** SK171943 — Advanced "fw ctl debug" features (Jira ACCL-930)
**Versions:** R80.20+, R81.10, R81.20, R82, R82.10
**Last Modified:** 2026-03-30

## Two Debug Systems

### 1. Firewall Kernel Debug (`fw ctl debug`)
Debugs the Firewall module's processing of traffic that is not handled by SecureXL (or falls through from it).

### 2. SecureXL Debug (`fwaccel dbg`)
Debugs SecureXL's fast-path: connection offload, packet processing, NAT, routing, and acceleration decisions.

**Important:** Run both simultaneously. On Scalable Platforms (Maestro/Chassis), connect to the applicable Security Group first.

## SecureXL Debug Modules

| Module | Purpose |
|--------|---------|
| [[securexl-module-default]] | Core control plane: connection lifecycle, offload, routing, NAT, queues |
| [[securexl-module-pkt]] | Packet processing: forwarding, drops, TCP state, NAT, VLAN, Geneve |
| [[securexl-module-infras]] | Infrastructure: reorder/holding queue, pattern matching, IPC |
| [[securexl-module-api]] | API layer: Firewall notifications, state updates, PacketXL, VPN SA |
| [[securexl-module-synatk]] | Accelerated SYN Defender: SYN attack handling |
| [[securexl-module-adp]] | ADP/NIC layer: host routes, next-hop, bonds, queues, hardware offload |
| [[securexl-module-vpn]] | VPN fast path: encryption, link selection, ClusterXL LS forwarding |
| [[securexl-module-cpaq]] | CPqA queue operations: broadcast, client/server transport |
| [[securexl-module-dos]] | DoS protection: configuration and packet handling |
| [[securexl-module-gtp]] | GTP tunnel handling |
| [[securexl-module-nac]] | Network Access Control |
| [[securexl-module-usdisp]] | User-space dispatcher |
| [[securexl-module-db]] | Database operations |
| [[securexl-module-tmpl]] | Template operations |

## Log File Locations

**Kernel Mode (KPPAK):**
- `$FWDIR/log/fwk.elg` — Firewall module
- `/var/log/messages` — additional

**User Mode (UPPAK):**
- `$FWDIR/log/fwk.elg` — Firewall module
- `/var/log/usim_x86.elg` — SecureXL
- `/var/log/messages` — ADP (NVIDIA ConnectX 100G)

## Standard Debug Procedure

### Three-Action Standard
1. `fw ctl debug -buf <size>`
2. `fw ctl debug -m <module> + <flags>` + `fwaccel dbg -m <module> + <flags>`
3. `fw ctl kdebug -f`

### Short Version
`fw ctl zdebug` — resets flags, 1024K buffer, no advanced options.

### Full Step-by-Step
```
1. expert
2. fw ctl debug 0
3. fwaccel dbg resetall
4. fw ctl set int simple_debug_filter_off 1
5. Configure filters (5-tuple / host IP / VPN peer)
6. fw ctl debug -buf 8200
7. Verify buffer
8. fw ctl debug -m <module> + <flags>
9. fwaccel dbg -m <module> + <flags>
10. Verify: fwaccel dbg list
11. fw ctl kdebug -T -f -o /var/log/kernel_debug.txt
12. Reproduce issue
13. CTRL+C
14. fw ctl debug 0
15. fwaccel dbg resetall
16. fw ctl set int simple_debug_filter_off 1
17. Verify defaults restored
18. Collect: /var/log/kernel_debug.txt, /var/log/messages*, $FWDIR/log/fwk.elg*, /var/log/usim_x86.elg*
```

## Debug Filters

### 5-Tuple Filter
```
fw ctl set str simple_debug_filter_saddr_1 "192.168.20.30"
fw ctl set str simple_debug_filter_daddr_1 "172.16.40.50"
fw ctl set int simple_debug_filter_dport_1 80
```
Up to 5 simultaneous. 0 = any port/protocol.

### Host IP Filter
Up to 3 host-IP filters via `simple_debug_filter_addr_<N>`.

### VPN Peer Filter
Up to 2 VPN-peer filters via `simple_debug_filter_vpn_<N>`.

### Disable All Filters
`fw ctl set int simple_debug_filter_off 1`

## Connection Life Cycle Debug
```
Start: conn_life_cycle.sh -a start -o /var/log/kernel_debug.txt -T -f "<5-tuple>"
Stop:  conn_life_cycle.sh -a stop -o /var/log/kernel_debug_formatted.txt
```
Output: Ruby-style hierarchical text. Up to 5 filters.

## Boot-Time Debug (R81.20+)
- SecureXL: `$FWDIR/conf/fwaccel_dbg_flags.cfg` (one line per module)
- Kernel boot flags: `$FWDIR/modules/fwkern.conf`

## Best Practices
- Use maintenance window — debug raises CPU load
- Prefer console access
- Use `fw ctl debug 0` to reset
- Avoid `fw ctl debug -x` unless needed
- Allocate kernel buffer: `fw ctl debug -buf 8200`
- Use `-T` for microsecond timestamps
- Write to `/var/log/`
- Consider cyclic files for long captures
- On R82 and high-core-count: new kernel debug behavior applies

## Related
- [[clusterxl-redundancy]]
- [[check-point-company]]
- [[r82-release]]
- [[cpinfo-troubleshooting-runbook]]
