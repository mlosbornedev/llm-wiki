---
title: R81.20 vs R82 VPN Delta
created: 2026-04-19
updated: 2026-04-19
type: comparison
tags: [comparison, vpn, r81, r82]
sources: []
---

# R81.20 vs R82 VPN Delta

**Scope:** R81.20 → R82 Site-to-Site VPN and Remote Access VPN Administration Guide delta

## Site-to-Site VPN Commands — Stable
Key commands confirmed in both R81.20 and R82 guides:
- Phase 1/2 proposal configuration
- NAT-T behavior
- Tunnel monitoring
- `cpinfo`/`cpreport` for VPN evidence

## Remote Access VPN — Stable Core
- Office Mode assignment methods (IP pool/DHCP/RADIUS)
- IKE Phase 1/2 negotiation
- Authentication methods (certificate, PSK, RADIUS, SecurID, SAML)
- Visitor Mode for restricted egress

## Key R82 VPN Improvements (from guides)
- R82 Site-to-Site VPN Admin Guide documents improved VPN tunnel establishment diagnostics.
- R82 Remote Access VPN Admin Guide adds explicit VSX context handling notes for Remote Access in VSX environments.
- Continued strongSwan interoperability guidance in both releases.

## Troubleshooting Themes — Unchanged
Both releases share these primary failure classes:
1. **IKE phase negotiation mismatch** — early tunnel failures
2. **NAT and MTU/fragmentation** — breaks setup even with valid policy
3. **Office Mode routing/domain scope mistakes** — "connects but no access"
4. **NAT-T/encapsulation constraints** — NAT-heavy environments

---

## Key Takeaway
VPN command behavior and failure class taxonomy are consistent between R81.20 and R82. R82 adds explicit VSX context documentation for Remote Access and improved tunnel establishment diagnostics.

---

## Related Pages
- [[vpn-tunnel-not-establishing]] — VPN tunnel runbook
- [[clusterxl-r81-r82-delta]] — ClusterXL delta
- [[gaia-r81-r82-delta]] — Gaia delta
- [[r82-release]] — R82 release overview
