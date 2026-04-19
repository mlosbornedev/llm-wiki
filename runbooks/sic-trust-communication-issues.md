---
title: SIC Trust / Communication Issues Runbook
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [troubleshooting, sic, security-management, r81, r82]
sources: []
---

# SIC Trust / Communication Issues Runbook

**Version scope:** R81.20–R82 | **Confidence:** medium

## Goal
Determine whether management-to-gateway failure is caused by missing secure trust, ordinary network reachability, or incorrect identity/time prerequisites.

---

## Step 1 — Capture the Exact Failing Operation
- **Intent:** Separate "cannot communicate" from "cannot install policy" or "cannot initialize SIC."
- **Action:** Capture exact error text, failing operation, and timestamps.
- **Expected:** Symptom framed as initialization failure, lost communication, or management operation failure.
- **If not:** Request exact operation and wording before proposing remediation.

## Step 2 — Establish Version and Target Identity
- **Intent:** Confirm correct object and release family.
- **Action:** `show version all`, plus hostname / management-server name-IP context.
- **Expected:** Gateway identity and release context are known.
- **If not:** Resolve identity mismatch first.

## Step 3 — Read Documented SIC Status from Management
- **Intent:** Use the management guide's own state model.
- **Action:** From gateway object in SmartConsole, record SIC status: `Communicating`, `Unknown`, or `Not Communicating`.
- **Expected:** Current state is known.
- **If not:** Gather screenshots/text from the Communication window.

## Step 4 — Verify Low-Risk Prerequisites Before Any Reset
- **Intent:** Eliminate documented initialization blockers first.
- **Action:** Verify all of:
  - Connectivity between Security Gateway and Security Management Server
  - Same SIC activation key on both sides when initializing
  - Correct rules / anti-spoofing if management is behind a gateway
  - Correct management hostname/IP mapping in `/etc/hosts` on the gateway, including public IP if management is statically NATed
  - Correct date/time on both systems
- **Expected:** Prerequisites are clean.
- **If not:** Correct the failing prerequisite and retry SIC before any reset workflow.

## Step 5 — Use Focused SIC Logging Next
- **Intent:** Inspect the source-documented evidence location.
- **Action:** Review `$CPDIR/log/sic_info.elg` on both the Security Management Server and the Security Gateway around the failure time.
- **Expected:** Evidence shows handshake/trust problem or leaves trust unproven.
- **If not:** Widen collection only after preserving focused SIC evidence.

## Step 6 — Permissive Test Flow (Controlled Troubleshooting Step)
- **Intent:** Follow the guide's escalation sequence safely.
- **Action:** If management is behind policy and prerequisites look correct, temporarily unload local policy on the gateway with `fw unloadlocal`, then retry SIC.
- **Expected:** SIC either initializes or remains broken.
- **If not:** Restore normal state per change control and continue with trust-focused analysis.

## Step 7 — Reset Trust Only When Evidence Shows It Is Compromised
- **Intent:** Avoid unnecessary certificate revocation.
- **Action:** If trust state is compromised or object/host identity changed: document risk, reset trust, ensure same one-time activation password is configured on gateway, and quickly re-establish SIC.
- **Expected:** Certificate is reissued and SIC returns to `Communicating`.
- **If not:** Escalate instead of looping repeated resets.

---

## Minimum Artifact Request Checklist (in order)
1. Exact error text + failing operation + timestamp
2. `show version all` and hostname/IP context
3. SIC status (`Communicating` / `Unknown` / `Not Communicating`)
4. Recent change history: re-IP, rename, upgrade, NAT change, host replacement, or prior SIC reset
5. Focused `sic_info.elg` evidence or support-bundle reference
6. Confirmation of clock sync and `/etc/hosts` / NAT handling

---

## Key Caveats
- **Time synchronization** is a documented requirement for SIC initialization.
- If management is statically NATed, the remote gateway may need the public management IP in `/etc/hosts` resolving to the management hostname.
- Resetting trust **revokes the certificate** — do not use as first troubleshooting step.
- SIC is required for secure management communication, policy installation, and log delivery — policy install failures can be a trust problem before they are a policy-content problem.
- After trust reset, the guides call out a policy install step to distribute the updated CRL.

---

## Related Pages
- [[policy-install-failures-gateway-side]] — Gateway-side policy install failures
- [[clusterxl-redundancy]] — ClusterXL failover that may accompany trust issues
- [[gaia-os]] — Gaia OS baseline commands
