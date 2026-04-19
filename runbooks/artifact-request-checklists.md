---
title: Artifact Request Checklists by Symptom
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [troubleshooting, reference, artifacts, escalation, r81, r82]
sources: []
---

# Artifact Request Checklists by Symptom

**Version scope:** R81.20–R82 | **Confidence:** medium-high

Use these lists to ask for the fewest artifacts that can prove or disprove the top failure domain first.

---

## Cluster Failover / Instability

1. **Exact failover time window and whether failover is current or historical.**  
   *Separates active instability from a past event; anchors all counters/logs.*

2. **Output from both members: `show cluster state` or `cphaprob state`.**  
   *Proves current role agreement, active/standby state, and whether views differ between members.*

3. **Output from both members: `show cluster members pnotes problem` or `cphaprob -l -ia -e list`.**  
   *Proves whether a critical device/pnote is the direct failover reason.*

4. **Output from both members: `show cluster members interfaces all` or `cphaprob -a if`, plus `show interfaces all`.**  
   *Distinguishes ClusterXL-monitored failure from ordinary Gaia link state.*

5. **If trunks/VLANs involved: `show cluster members interfaces vlans`.**  
   *Proves whether the affected VLAN is actually monitored by ClusterXL.*

6. **If bonds involved: `show cluster bond all` or `cphaprob show_bond <bond>`.**  
   *Catches subordinate-link or bond-state triggers hidden from top-level interface views.*

7. **Output from both members: `show cluster statistics sync` or `cphaprob syncstat`.**  
   *Proves whether sync health is contributing to instability.*

8. **`show cluster failover` or `cphaprob show_failover`.**  
   *Proves recurrence versus a one-time event.*

---

## VPN Tunnel Issues

1. **Exact error text, failing peer/client type, and one controlled failing-attempt time window.**  
   *Classifies the case before asking for broad logs.*

2. **Clarify symptom stage: pre-connect, auth/IKE, tunnel up but no traffic, or intermittent drops.**  
   *Separates establishment, authentication, and post-connect routing/policy problems.*

3. **`show version all`, `show interfaces all`, `show route all`.**  
   *Proves version/Jumbo context and basic path prerequisites.*

4. **Peer capability details: auth method, Phase 1/2 proposals, NAT-T or restricted-egress context.**  
   *Proposal/auth mismatch is a primary guide-backed cause.*

5. **For Remote Access: Office Mode method and assigned-address behavior.**  
   *Proves whether "connects but no access" is really routing/domain handling.*

6. **Minimal topology sketch for the failing path.**  
   *Exposes asymmetry, NAT placement, or wrong-domain assumptions quickly.*

7. **Focused VPN logs plus `cpinfo`/`cpreport` only if prior items still leave the failing phase unclear.**  
   *Keeps collection narrow until symptom domain is proven.*

---

## Policy Install Failures (Gateway Side)

1. **Exact SmartConsole/install-policy error text and failure timestamp.**  
   *First split is SIC/trust, connectivity, or policy-content/application failure.*

2. **`show version all` from the gateway.**  
   *Proves release/Jumbo context before comparing behavior.*

3. **SIC status from management for the gateway object: Communicating/Unknown/Not Communicating.**  
   *Quantum Security Management guides tie SIC directly to policy install capability.*

4. **Gateway-side name/IP resolution and recent changes (host replacement, re-IP, certificate/trust work, upgrade).**  
   *Common triggers for broken trust or mismatched identity.*

5. **Focused SIC evidence: relevant `sic_info.elg` excerpt or support-bundle reference around failure window.**  
   *Source-documented place to inspect SIC failures.*

6. **If trust suspected: confirmation clocks are synchronized and activation key/trust workflow was or was not recently changed.**  
   *Guides explicitly call out time sync and activation-key alignment.*

7. **`cpstat mg` / support-bundle references if already collected.**  
   *Seed solved SR used management-status evidence alongside SIC reset workflow notes.*

8. **`cpinfo`/`cpreport` only if the above still does not isolate trust, path, or service failure.**  
   *Broad bundle comes after the highest-yield trust checks.*

---

## Performance Degradation

1. **Exact symptom window, impact type, and whether the issue is sustained or spiky.**  
   *Determines whether live counters or time-based evidence is needed.*

2. **CPView captures for CPU, memory, disk, and affected blades.**  
   *Safest first-pass baseline across guide-backed performance workflows.*

3. **If CPU suspected: `fw ctl multik stat` and `fw ctl multik utilize`.**  
   *Proves whether pressure is distributed or isolated to one CoreXL instance.*

4. **If imbalance suspected: `fw ctl affinity -l -r`.**  
   *Proves whether affinity/layout could explain the hot CPU.*

5. **If logging pressure suspected: per-instance `fwsyslog_nlogs_counter` and, if needed, short `fw ctl zdebug | grep logs` sample with `fw ctl set int fwsyslog_print_counter 1`.**  
   *Documented way to prove heavy logging instead of guessing.*

6. **If memory suspected: CPView memory pages and any supporting monitor view already available.**  
   *Prevents false "memory leak" claims without actual pressure evidence.*

7. **`show spike-detective` or saved spike evidence for intermittent events.**  
   *Supports bursty cases without immediate tuning changes.*

8. **`cpinfo -y all` after the symptom domain is narrowed.**  
   *Preserves escalation-grade evidence after first-pass classification.*

---

## Related Pages
- [[clusterxl-redundancy]] — ClusterXL concepts
- [[sic-trust-communication-issues]] — SIC trust runbook
- [[vpn-tunnel-not-establishing]] — VPN tunnel runbook
- [[performance-degradation-cpu-memory]] — Performance runbook
- [[diagnostics-commands]] — Full diagnostics command reference
