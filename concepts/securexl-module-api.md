---
title: SecureXL Module — api
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [securexl, fwaccel, debugging, api, r82]
sources: []
---

# SecureXL Module — api

## Purpose
API layer between SecureXL and the Firewall/internal services. Covers add/update/delete operations, notifications, statistics retrieval, PacketXL integration, and VPN SA/template coordination.

## Primary Use Case
When a packet appears to be **waiting on a state update or notification exchange** from the Firewall. The ACK reorder notification/response path is one of the gates that can release held packets.

## Key Functions (from reversed R82 binary)
```
hold_new_connection_and_send_ack_to_fw     — hold new connection and notify FW
cphwd_api_reorder_ack_response             — ACK reorder response
cphwd_api_update_ff                        — update forward forward (FF) state
cphwd_api_revert_prepare / _ex            — revert preparation
cphwd_api_commit                           — commit configuration
cphwd_api_prepare                         — prepare for configuration
cphwd_api_light_prepare / _commit         — lightweight path
cphwd_api_sim_sdwan_*                     — SD-WAN specific API calls
cphwd_api_get_features                    — feature buffer retrieval
cphwd_api_reset_state_timer               — reset state timer
```

## Debug Flags
| Flag | Use |
|------|-----|
| `notif` | Notifications to Firewall or peer services |
| `get_state` | Get connection state from SecureXL |
| `long_ver` | Verbose connection information |
| `pxl` | PacketXL / streaming API between SecureXL and PSL |
| `update` | Update connections/state |
| `add` | Add connections/state |
| `del` | Delete connections/state |
| `sv` | Sequence-validation related API |
| `get_features` | Feature buffer retrieval during init |
| `get_stat` | Statistics retrieval |
| `reset_stat` | Statistics reset |
| `add_sa` / `del_sa` | VPN SA offload/delete |
| `upd_link_sel` | VPN link-selection updates |

## Suggested Debug Command
```bash
fwaccel dbg -m api + notif get_state update long_ver pxl misc vpn upd_link_sel
```

## Cross-Module Visibility
- [[securexl-module-infras]] — for the hold/unhold decision that api notifications drive
- [[securexl-module-pkt]] — for packet-stage transitions triggered by API notifications
- [[securexl-module-default]] — for connection state that api reads/writes

## Related
- [[securexl-debugging]]
- [[securexl-module-infras]]
- [[securexl-module-pkt]]
