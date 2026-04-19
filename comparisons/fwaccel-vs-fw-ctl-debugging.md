---
title: fwaccel vs fw-ctl — Debugging Contrast
created: 2026-04-19
updated: 2026-04-19
type: comparison
tags: [securexl, fwaccel, fw-ctl, debugging, comparison, reference]
sources: []
---

# fwaccel vs fw-ctl — Debugging Contrast

## Overview
Two parallel debug systems that must both be configured for complete kernel-to-SecureXL visibility.

## fw ctl debug (Firewall Kernel Debug)

| Aspect | Detail |
|--------|--------|
| **What it debugs** | Firewall module — traffic that falls through from SecureXL or is processed in the full kernel path |
| **Command** | `fw ctl debug -m <module> + <flags>` |
| **Log** | `$FWDIR/log/fwk.elg` |
| **Syntax** | `fw ctl debug ... -m <module> {all \| [+-\] <flags>}` |
| **Combined with** | `fwaccel dbg` (both needed simultaneously) |

## fwaccel dbg (SecureXL Debug)

| Aspect | Detail |
|--------|--------|
| **What it debugs** | SecureXL fast-path — connection offload, packet processing, NAT, routing, acceleration decisions |
| **Command** | `fwaccel dbg -m <module> + <flags>` |
| **Log** | `/var/log/usim_x86.elg` (UPPAK mode) |
| **Syntax** | `fwaccel dbg -m <module> {all \| + \| - \| reset \| list \| resetall}` |
| **Combined with** | `fw ctl debug` (both needed simultaneously) |

## Key Differences

| Dimension | fw ctl debug | fwaccel dbg |
|-----------|-------------|-------------|
| **Layer** | Firewall kernel module | SecureXL acceleration layer |
| **Traffic path** | Slow path / full processing | Fast path / accelerated |
| **Kernel buffer** | `fw ctl debug -buf 8200` | N/A (SecureXL managed) |
| **Short syntax** | `fw ctl zdebug` | No equivalent |
| **VSX support** | `-v` / `-k` flags | Same |
| **Boot-time** | `$FWDIR/modules/fwkern.conf` | `$FWDIR/conf/fwaccel_dbg_flags.cfg` |

## Why Both Are Needed
A single packet's life cycle often traverses BOTH paths:
1. Packet arrives → SecureXL fast path (`fwaccel dbg`)
2. Packet triggered a hold/reorder → falls through to Firewall (`fw ctl debug`)
3. Firewall sends notification back → SecureXL releases (`fwaccel dbg` + `fw ctl debug`)

Debugging only one gives an incomplete picture.

## Standard Combined Command Pattern
```bash
fw ctl debug -buf 8200
fw ctl debug -m <fw_module> + <flags>
fwaccel dbg -m <sxl_module> + <flags>
fw ctl kdebug -T -f -o /var/log/kernel_debug.txt
```

## Related
- [[securexl-debugging]]
- [[clusterxl-redundancy]]
- [[cpinfo-troubleshooting-runbook]]
