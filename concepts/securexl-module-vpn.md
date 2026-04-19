---
title: SecureXL Module — vpn
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [securexl, fwaccel, debugging, vpn, clusterxl, r82]
sources: []
---

# SecureXL Module — vpn

## Purpose
VPN fast-path handling: packet processing, link selection, ClusterXL Load Sharing forwarding, routing, and FEC-related VPN behavior.

## Primary Use Case
When the trace shows `fwha vpn sticky tunnel fwd` — VPN sticky tunnel forwarding in ClusterXL Load Sharing mode. Also relevant for any VPN-related drop or latency.

## Encryption Algorithms Observed (from reversed R82)
| Algorithm | Type |
|-----------|------|
| `rijndael_gcm_encrypt/decrypt` | AES-GCM |
| `rijndael128_cbc_encrypt/decrypt` | AES-CBC |
| `rijndael_cbc_encrypt/decrypt` | AES-CBC |
| `cp3des_cbc_encrypt/decrypt` | 3DES-CBC |
| `cpdes_cbc_encrypt/decrypt` | DES-CBC |
| `cpcast_cbc_encrypt/decrypt` | CAST-CBC |

## Debug Flags
| Flag | Use |
|------|-----|
| `vpnpkt` | VPN packet processing |
| `linksel` | VPN Link Selection |
| `routing` | VPN encryption/routing information |
| `vpn` | VPN connection processing |
| `ls` | ClusterXL Load Sharing forwarding between members |
| `fec` | Forward Error Correction path |

## Suggested Debug Command
```bash
fwaccel dbg -m vpn + routing vpn vpnpkt linksel ls fec
```

## Cross-Module Visibility
- [[clusterxl-redundancy]] — sticky connections and LS Unicast/Multicast
- [[securexl-module-default]] — for routing decisions that affect VPN
- [[securexl-module-api]] — for VPN SA add/del operations

## Related
- [[securexl-debugging]]
- [[clusterxl-redundancy]]
