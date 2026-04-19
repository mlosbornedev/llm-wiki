---
title: Policy Install Failures — Gateway Side
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [troubleshooting, policy-install, sic, security-management, r81, r82]
sources: []
---

# Policy Install Failures — Gateway Side Runbook

**Version scope:** R81.20–R82 | **Confidence:** medium

## Goal
Determine whether a gateway-side policy install failure is caused by SIC/trust issues, connectivity problems, or policy content/application failures.

---

## Step 1 — Capture Exact Error and Failure Point
- **Intent:** First split: SIC/trust, connectivity, or policy-content failure.
- **Action:** Capture exact SmartConsole / install-policy error text and failure timestamp.
- **Expected:** Clear error classification.
- **If not:** Request exact wording before proposing remediation.

## Step 2 — Establish Version Context
- **Intent:** Confirm release/Jumbo context before comparing behavior.
- **Action:** `show version all` from the gateway.
- **Expected:** Known release baseline.
- **If not:** Resolve identity mismatch first.

## Step 3 — Check SIC Status
- **Intent:** Use the management guide's own state model.
- **Action:** From SmartConsole gateway object, record SIC status: `Communicating`, `Unknown`, or `Not Communicating`.
- **Expected:** Current trust state is known.
- **If not:** Gather screenshots from the Communication window.
- **Note:** Quantum Security Management guides tie SIC directly to policy install capability.

## Step 4 — Check for Recent Changes
- **Intent:** Identify common triggers before deeper investigation.
- **Action:** Document any recent: host replacement, re-IP, certificate/trust work, or upgrade.
- **Expected:** Change history is known.
- **If not:** These are common triggers for broken trust or mismatched identity.

## Step 5 — Verify Prerequisites (Same as SIC Runbook)
- **Intent:** Eliminate initialization blockers before reset workflows.
- **Action:** Verify:
  - Connectivity between gateway and management server
  - Same SIC activation key on both sides
  - Correct rules/anti-spoofing if management is behind a gateway
  - Correct hostname/IP in `/etc/hosts` on gateway (including public IP if NATed)
  - Time synchronization on both systems
- **Expected:** Prerequisites are clean.

## Step 6 — Gather Focused SIC Evidence
- **Intent:** Inspect the source-documented evidence location.
- **Action:** Review `sic_info.elg` excerpt or support-bundle reference around the failure window.
- **Expected:** Trust handshake problem is identified or ruled out.
- **If not:** Widen collection only after preserving focused SIC evidence.

## Step 7 — Escalate to Full Support Data
- **Intent:** Gather escalation-grade evidence when initial checks are inconclusive.
- **Action:** `cpinfo`/`cpreport` and `cpstat mg` references if already collected.
- **Expected:** Full gateway state available for correlation.
- **If not:** Continue with narrowed collection based on prior steps.

---

## Minimum Artifact Request Checklist (in order)
1. Exact SmartConsole/install-policy error text and failure timestamp
2. `show version all` from the gateway
3. SIC status (`Communicating` / `Unknown` / `Not Communicating`)
4. Gateway-side name/IP resolution and recent change history
5. Focused SIC evidence: `sic_info.elg` excerpt or support-bundle reference
6. Confirmation of clock sync and `/etc/hosts`/NAT handling
7. `cpstat mg` / support-bundle references if already collected
8. `cpinfo`/`cpreport` only if above still does not isolate trust, path, or service failure

---

## Key Caveats
- **SIC is required for policy installation** — policy install failures can be a trust problem before they are a policy-content problem.
- **SIC reset revokes the certificate** — do not use as first step.
- After trust reset, a policy install step distributes the updated CRL.
- Time sync and activation-key alignment are explicitly called out in the guides.

---

## Related Pages
- [[sic-trust-communication-issues]] — Full SIC trust troubleshooting
- [[check-point-sr-workflow-hermes]] — Michael's SR methodology
- [[cpinfo-troubleshooting-runbook]] — cpinfo symptom-to-artifact triage
