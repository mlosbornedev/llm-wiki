---
title: SecureXL Module — pkt
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [securexl, fwaccel, debugging, pkt, r82]
sources: []
---

# SecureXL Module — pkt

## Purpose
Packet processing in SecureXL: forwarding decisions, delivery, drop reasons, routing, PacketXL, NAT, anti-spoofing, TCP validation, encapsulations, and per-packet details.

## Primary Use Case
Understanding **packet-stage transitions**. Best fit for mapping trace points: `SecureXL stateless check`, `SecureXL lookup`, `IP Options Strip (in)`, `medium_path_f2p_*`.

## Key Functions (from reversed R82)
```
simi_ip_options_remove_do               — remove IP options
sim_pkt_prepare_f2f_routing_error_msg  — prepare f2f routing error
hold_and_send_pkt                       — hold and send packet
handle_inbound_packet                   — handle inbound
resume_from_holding_queue               — resume from holding queue
resume_from_qos                         — resume from QoS
resume_from_fw_forward                 — resume from FW forward
resume_from_ppack_forward              — resume from ppack forward
resume_from_thread_forward             — resume from thread forward
resume_from_routing                    — resume from routing
resume_from_error / _pre               — resume from error
sim_pkt_send_drop_notification         — send drop notification
sim_verify_ip_options                  — verify IP options
sim_pkt_trim                           — trim packet
```

## Debug Flags
| Flag | Use | Notes |
|------|------|-------|
| `f2f` | Forward to Firewall (reason a packet left fast path) | Very relevant |
| `tcp_state` | TCP state validation | Very relevant |
| `tcp_state_pkt` | TCP packet validation | Very relevant |
| `notif` | Notifications to Firewall | Very relevant |
| `deliver` | Packet delivery | Very relevant |
| `routing` | SecureXL routing handling | Very relevant |
| `pxl` | PacketXL / packet streaming | Very relevant |
| `nat` | NAT processing | Very relevant |
| `drop` | Packets dropped by SecureXL | |
| `frag` | Fragment handling | |
| `spoof` | Anti-spoofing | |
| `acct` | Packet/connection accounting | |
| `sv` | Sequence validation | |
| `cpls` | ClusterXL LS packet behavior | |
| `qos` | QoS acceleration | |
| `vlan` | VLAN handling | |
| `wrp` | WRP interfaces in VSX | |
| `corr` | Correction layer | |
| `caf` | Mirror and Decrypt traffic path | |
| `geneve` | Geneve packet handling | |
| `sctp` | SCTP handling | |

## Suggested Debug Command
```bash
fwaccel dbg -m pkt + f2f deliver notif routing pxl tcp_state tcp_state_pkt nat pkt
```

## Practical Interpretation
Use this to understand packet-stage transitions. Maps directly to `medium_path_f2p_*` trace points observed in packet captures.

## Related
- [[securexl-debugging]]
- [[securexl-module-infras]]
- [[securexl-module-default]]
