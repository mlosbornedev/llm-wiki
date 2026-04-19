---
title: SecureXL Module — adp
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [securexl, fwaccel, debugging, adp, nics, r82]
sources: []
---

# SecureXL Module — adp

## Purpose
ADP (Accelerated Data Path) / NIC-adjacent handling. Covers host route/next-hop work, queueing, bonds, multicast, IPSCTL, hardware offload, and low-level packet I/O.

## Primary Use Case
When symptoms involve: ADP path logging in `/var/log/messages`, host route/next-hop issues, backplane drops, bond state changes, or hardware offload failures.

## Key Functions (from reversed R82)
```
adp_phys_slot_get              — get physical slot
adp_boardtype_get              — get board type
adp_npmask_get                 — NP mask
adp_get_num_queues             — number of queues
adp_get_cfg_queue_start        — config queue start
adp_get_data_queue_start       — data queue start
host_nh_set                    — set host next-hop
host_nhstats_intr_*            — next-hop stats interrupt handling
host_nhstats_buf_flush         — flush stats buffer
host_nhstats_add_list          — add to stats list
```

## Debug Flags
| Flag | Use |
|------|-----|
| `rt` | Route handling |
| `nh` | Next-hop handling |
| `eth` | Ethernet path |
| `heth` | Host Ethernet path |
| `wrp` | WRP interfaces |
| `inf` | General info/infrastructure |
| `mbs` | MBS/management-bus messaging |
| `bpl` | Backplane handling |
| `bplinf` | Backplane info |
| `if` | Interface handling |
| `drop` | Drops in ADP |
| `bond` | Bond interfaces |
| `xmode` | XMODE / special dataplane mode |
| `ipsctl` | IPSCTL interactions |
| `cpfifo` | CP FIFO queues |
| `qconf` | Queue config |
| `qcomm` | Queue communication |
| `packet` | Packet handling |
| `mcast` | Multicast |
| `hw_offload` | Hardware offload |
| `hw_expn` | Hardware exception path |
| `rte_api` | DPDK/RTE API activity |

## Suggested Debug Command
```bash
fwaccel dbg -m adp + rt nh eth heth wrp inf mbs bpl bplinf if drop bond xmode ipsctl
```

## Related
- [[securexl-debugging]]
- [[securexl-module-pkt]]
