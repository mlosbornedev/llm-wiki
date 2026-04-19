---
title: Identity Awareness Command Reference
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [reference, identity-awareness, r81, r82, vsx]
sources: []
---

# Identity Awareness Command Reference

**Version scope:** R81.20–R82 | **Confidence:** high

## Operator-First Usage Boundary
- Commands are for observing Identity Awareness collection/distribution state.
- In clusters, configure members consistently.
- On Scalable Platforms (Maestro/Chassis), run applicable commands in Expert mode on the applicable Security Group.
- In R82, the guide explicitly adds a VSX caveat: run in the context of the applicable Virtual Gateway / Legacy Virtual System with `vsenv <VS ID>`.

---

## `adlog a statistics` / `adlog l statistics`
- **Purpose:** Show NT Event log processing statistics for AD Query.
- **What it proves:** Whether adlog is actually receiving and processing event-log identity input — per IP and in total — and whether identities are being learned at all.
- **Expected output:** Per-IP and total counters are present; shows number of identified IP addresses. In healthy AD Query collection, counters are non-empty and consistent with expected DC/log-server activity.
- **If not:**
  - Counters stay empty during a known logon window → first prove identity acquisition is missing before blaming policy
  - Verify identity-source selection and AD Query prerequisites before moving to PEP/PDP distribution checks
- **Scope note:** `a` = gateway-side AD Query context; `l` = log-server context. In R82 VSX/VSNext/Legacy VSX mode, run in applicable `vsenv` context.
- **Source:** R81.20 and R82 Identity Awareness Administration Guides

## `pdp status show`
- **Purpose:** Show PDP status information (start time, configuration time).
- **What it proves:** Whether the PDP control-plane process is up and has a coherent current configuration baseline.
- **Expected output:** Command returns PDP information; start/configuration timestamps appear reasonable for the incident timeline.
- **If not:**
  - PDP status unavailable or clearly stale after recent changes → treat PDP health/configuration as a primary branch
  - Continue with `pdp tasks_manager status` if a task or rollout appears stuck
- **Source:** R81.20 and R82 Identity Awareness Administration Guides

## `pdp tasks_manager status`
- **Purpose:** Show current, previous, and pending PDP tasks.
- **What it proves:** Whether identity-related background tasks are progressing or stuck.
- **Expected output:** Task list is readable and consistent with recent identity configuration activity; no unexplained buildup of pending work in steady state.
- **If not:**
  - Tasks stuck or continuously pending → do not assume the issue is only on the PEP side
  - Correlate with recent identity-source changes and capture timestamps before restart/reset discussions
- **Source:** R81.20 and R82 Identity Awareness Administration Guides

## `pep show pdp all`
- **Purpose:** Show the communication channel between the PEP and PDP.
- **What it proves:** Whether the enforcement point can see and talk to the identity decision/distribution plane.
- **Expected output:** Visible PEP↔PDP communication data; output enumerates PDP communication details for the current context.
- **If not:**
  - Path broken → do not jump directly to Access Control rule debugging; verify PDP status and network registration state first
- **Source:** R81.20 and R82 Identity Awareness Administration Guides

## `pep show network pdp`
- **Purpose:** Show the Network-to-PDP mapping table.
- **What it proves:** Which networks are registered to which PDP — crucial when identities appear to exist but enforcement happens on the wrong path or context.
- **Expected output:** Mapping table entries exist for the relevant network segments; mappings align with the gateway/VS expected to enforce identity-based access.
- **If not:**
  - Mappings missing or unexpected → investigate topology/registration before changing policy
- **Note:** In VSX environments, wrong context can make healthy identity data look absent.
- **Source:** R81.20 and R82 Identity Awareness Administration Guides

## `pep show user query ...`
- **Purpose:** Query PEP-side user/session mappings using filters such as `usr`, `cid`, `mchn`, `ugrp`, `role`, or `uid`.
- **What it proves:** Whether a specific user/machine/IP currently exists in the enforcement-side identity mapping table.
- **Expected output:** Targeted query returns the expected session or no session. Useful filter examples: username, computer, group, role, PDP, client IP.
- **If not:**
  - Known active user absent → identity collection may exist upstream but not be enforced locally
  - Compare with `adlog ... statistics` and `pdp status show` before resetting anything
- **Caution:** Query in the exact enforcement context; wrong VS can produce a false-negative result.
- **Source:** R81.20 and R82 Identity Awareness Administration Guides

---

## Practical Low-Risk Order
1. `adlog a statistics` or `adlog l statistics` — is AD Query receiving events?
2. `pdp status show` — is PDP up and configured?
3. `pdp tasks_manager status` — are background tasks progressing?
4. `pep show pdp all` — can PEP see PDP?
5. `pep show network pdp` — are networks registered correctly?
6. `pep show user query ...` — is the specific user/session present?

---

## Related Pages
- [[vsx-virtual-systems]] — VSX context and `vsenv` usage
- [[gaia-os]] — Baseline Gaia commands
- [[check-point-sr-workflow-hermes]] — SR evidence methodology
