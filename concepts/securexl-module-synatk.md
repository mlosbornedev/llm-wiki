---
title: SecureXL Module — synatk
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [securexl, fwaccel, debugging, synatk, dos, r82]
sources: []
---

# SecureXL Module — synatk

## Purpose
Accelerated SYN Defender (SYN attack handling) — configuration, states, packets, and logging.

## Primary Use Case
SYN-specific acceleration paths. When symptoms involve SYN retransmits, SYN drops, or SYN Defender behavior.

## Key Functions (from reversed R82)
```
handle_synatk_syn_pkt          — process incoming SYN
handle_synatk_ack_pkt          — process ACK response
synatk_timer_cb                — periodic monitor timer
sim_synatk_send_stats          — send stats to manager
sim_synatk_if_add_hook         — interface add hook
sim_synatk_if_del_hook         — interface delete hook
sim_synatk_dump_packet         — packet dump
synatk_send_validation         — send validation
synatk_do_sequence_adjust      — sequence adjustment
synatk_is_ifn_addr             — interface address check
_sim_synatk_no_conn_hook       — no-connection hook
_sim_synatk_conn_found_hook    — connection-found hook
cphwd_api_synatk_config        — configuration API
cphwd_api_synatk_state         — state API
sim_mgr_send_synatk_stats      — stats to manager
sim_mgr_send_synatk_notify     — notify manager
```

## Debug Flags
| Flag | Use |
|------|-----|
| `init` | Accelerated SYN Defender init |
| `conf` | Receiving/updating SYN Defender config |
| `conn` | TCP connection handling in SYN Defender |
| `log` | Periodic monitor log timing |
| `pkt` | TCP packet handling in SYN Defender |
| `state` | SYN Defender state-machine information |
| `msg` | Internal SYN Defender messages |
| `proxy` | Not currently used |

## Suggested Debug Command
```bash
fwaccel dbg -m synatk + init conf conn log pkt proxy state msg
```

## Related
- [[securexl-debugging]]
- [[securexl-module-pkt]]
- [[securexl-module-dos]]
