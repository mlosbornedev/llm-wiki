---
title: SecureXL Module — default
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [securexl, fwaccel, debugging, default, r82]
sources: []
---

# SecureXL Module — default

## Purpose
Core SecureXL control plane and generic fast-path behavior: connection lifecycle, offload, routing, NAT, ranges, queues, stats, and infrastructure glue.

## Primary Use Case
Broad "something is wrong in the fast-path state machine" investigation. Best generic module for connection/offload/routing/NAT context.

## Key Functions (from reversed R82)
```
sim_mgr_pending_queue_flush           — flush pending queue
fwk_snd_flush_pending_queue          — send flush pending queue
fwk_snd_enqueue_pending               — enqueue to pending
fwk_snd_get_usermode_run_state        — get user-mode run state
fwk_snd_main_wrapper                  — main wrapper
fwk_snd_get_fw2sxl_queue_max_entry_sz — get queue max entry size
fwk_snd_enable_adpdrv                 — enable ADP driver
fwk_snd_check_ip_forward_sanity       — IP forward sanity check
fwk_snd_get_uppak_run_state            — get UPPAK run state
fwk_snd_get_sim_run_state             — get SIM run state
fwk_snd_should_enable_netfilter        — netfilter enable check
fwk_snd_update_run_state_v4 / _v6     — update run state
fwk_snd_set_vs_started                — set VS started
fwk_snd_max_vsid_registered            — max VSID registered
fwk_snd_set_sim_initialized            — set SIM initialized
```

## Debug Flags
| Flag | Use |
|------|-----|
| `conn` | Connection processing |
| `offload` | Offloading decisions from Firewall to SecureXL |
| `nat` | NAT processing |
| `routing` | SecureXL routing decisions |
| `queue` | Connections queue behavior |
| `htab` | Hash table activity |
| `init` | Initialization and bring-up |
| `tag` | Tags added to packets before forwarding to Firewall |
| `lock` | Lock behavior |
| `update` | Connection/state updates |
| `del` | Deletion of connections/state |
| `acct` | Connection accounting |
| `stat` | Statistics handling |
| `ant` | Anticipated connections |
| `conn_app` | Application-related connection processing |
| `infra_ids` | Identity-range ID allocation |

## Suggested Debug Command
```bash
fwaccel dbg -m default + conn offload queue routing nat htab update
```

## Cross-Module Visibility
- [[securexl-module-pkt]] — packets that default routes through SecureXL
- [[securexl-module-infras]] — reorder queue that default connections may hit
- [[securexl-module-api]] — API calls that update default connection state

## Related
- [[securexl-debugging]]
- [[securexl-module-pkt]]
