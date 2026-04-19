---
title: SecureXL Module — infras
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [securexl, fwaccel, debugging, infras, r82]
sources: []
---

# SecureXL Module — infras

## Purpose
Infrastructure services that sit under SecureXL features. Most important for: **reorder/holding-queue logic**, pattern matching, connection profiling, and IPC.

## Primary Use Case
**Held-SYN / delayed-release / grouped-packet-release investigation.** If packets appear to stop and then release together, this is the primary module.

## Key Function: `simi_reorder_hold*`

The core holding-queue functions observed in reverse engineering:
- `simi_reorder_hold` — hold a packet in the reorder queue
- `simi_reorder_hold_with_packet` — hold with packet context
- `simi_reorder_hold_ex` / `simi_reorder_hold_with_packet_ex` — extended variants
- `simi_reorder_unhold` — release from hold
- `simi_reorder_unhold_ex` — extended release
- `simi_reorder_enqueue_packet` — add to queue
- `simi_reorder_dequeue_all_packets` — drain queue
- `simi_reorder_handle_ack` — process ACK that triggers release
- `simi_reorder_send_ack_notification` — notify peer of ACK
- `simi_reorder_should_drop` — decide to drop vs. release
- `resume_from_holding_queue` — resume processing after release

## Debug Flags
| Flag | Use |
|------|-----|
| `reorder` | Reordering / holding queue. **Highest value for held-SYN/grouped-release.** |
| `pm` | Pattern matcher internals |
| `conn_prof` | Connection profiling internals |
| `ipc` | Inter-process/inter-thread communication |

## Suggested Debug Command
```bash
fwaccel dbg -m infras + reorder pm conn_prof ipc
```

## Related Functions (from reversed R82 binary)
```
simi_reorder_handle_ack
simi_reorder_send_ack_notification
simi_reorder_set_flag_enabled_or_disabled
simi_reorder_set_flag_and_timeout
simi_reorder_unset_flag_and_timeout
simi_reorder_free_opaque_if_exist
simi_reorder_unhold_action_to_string
simi_reorder_backup_queue
resume_from_holding_queue
```

## Cross-Module Visibility
For held-SYN investigation, combine with:
- [[securexl-module-pkt]] — `tcp_state`, `tcp_state_pkt`, `f2f`, `deliver`, `notif`
- [[securexl-module-api]] — `notif`, `get_state`, `long_ver`, `pxl` (ACK notification paths)
- [[securexl-module-default]] — `conn`, `queue`, `offload`

## Related
- [[securexl-debugging]]
- [[securexl-module-pkt]]
- [[securexl-module-api]]
