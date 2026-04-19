---
title: VPN Tunnel Not Establishing Runbook
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [troubleshooting, vpn, r81, r82]
sources: []
---

# VPN Tunnel Not Establishing Runbook

**Version scope:** R81.20–R82 | **Confidence:** medium

## Goal
Determine whether VPN tunnel failure is caused by IKE/auth mismatch, routing/domain issues, NAT/fragmentation, or policy access problems.

---

## Step 1 — Classify the Failure Domain
- **Intent:** Before changing any config, classify the failure type.
- **Action:** Capture exact failure text, timestamp, peer type (Remote Access vs Site-to-Site), and whether failure is pre-connect, auth, or post-connect traffic.
- **Expected:** Issue mapped to one domain: IKE/auth mismatch, routing/domain, NAT/fragmentation, or policy access.
- **If not:** Request a one-path topology sketch and one reproducible failing attempt window.

## Step 2 — Validate Basic Path Prerequisites
- **Intent:** Confirm reachability and interface health before VPN-specific troubleshooting.
- **Action:** `show interfaces all` and `show route all`.
- **Expected:** Peer/internal path is reachable and interface state is healthy.
- **If not:** Fix path/asymmetry first — VPN changes are unlikely to succeed before reachability is stable.

## Step 3 — Confirm IKE/Auth Compatibility
- **Intent:** Validate that both sides can agree on Phase 1 and Phase 2 parameters.
- **Action:** Compare configured Phase 1/2 methods and auth scheme (certificate/PSK/RADIUS/etc.) with peer/client capabilities.
- **Expected:** Compatible proposal/auth set exists on both sides.
- **If not:** Align to mutually supported proposals/auth methods and retest.

## Step 4 — Check Remote Access-Specific Routing Behavior
- **Intent:** Identify "connects but no access" patterns specific to Remote Access VPN.
- **Action:** Validate Office Mode method and outcome (IP pool/DHCP/RADIUS), and verify VPN-domain/routing treatment of assigned Office Mode ranges.
- **Expected:** User gets expected virtual IP behavior and can reach intended resources.
- **If not:** Correct Office Mode/routing-domain configuration and retest.

## Step 5 — Identify NAT/Egress Constraints
- **Intent:** Catch NAT, MTU, fragmentation, and port-restriction issues.
- **Action:** Evaluate NAT/MTU/fragmentation conditions and restricted egress scenarios (Visitor Mode path where relevant).
- **Expected:** Tunnel setup proceeds without NAT/fragmentation or blocked-port constraints.
- **If not:** Apply NAT-T/encapsulation-compatible path and retest.

## Step 6 — Gather Escalation-Grade Evidence
- **Intent:** Collect evidence when still unresolved.
- **Action:** Collect `cpinfo`/`cpreport` plus focused VPN logs for one controlled failing attempt.
- **Expected:** Failing phase and likely root-cause domain are isolated with source evidence.
- **If not:** Reduce to one peer/client path and repeat controlled capture.

---

## Minimum Artifact Request Checklist (in order)
1. Exact error text + timestamp
2. Peer/client type and affected scope
3. One failing-attempt time window
4. `show version all`, `show interfaces all`, `show route all`
5. Office Mode details (for Remote Access cases)
6. Peer capability details: auth method, Phase 1/2 proposals, NAT-T context
7. Focused logs + `cpinfo/cpreport` when needed

---

## Source-Backed Troubleshooting Themes
- **IKE phase negotiation mismatch** can produce early tunnel failures
- **NAT and MTU/fragmentation** conditions can break setup even with valid policy
- **Office Mode routing/domain scope mistakes** can allow connect-but-no-resource access
- **IKE Phase configuration and authentication method alignment** explicitly called out in Remote Access guides
- **NAT-T/encapsulation** behavior affects connectivity in NAT-heavy environments
- **strongSwan clients:** verify `ike=` and `esp=` proposals match gateway support

---

## Related Pages
- [[r82-release]] — R82 VPN improvements
- [[gaia-os]] — Baseline commands (`show interfaces all`, `show route all`)
- [[clusterxl-redundancy]] — Sticky Connections for Hub+Spoke VPN with 3rd party
- [[securexl-debugging]] — SecureXL VPN fast path module
