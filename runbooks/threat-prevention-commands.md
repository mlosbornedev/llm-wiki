---
title: Threat Prevention Command Reference
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [reference, threat-prevention, threat-emulation, r81, r82]
sources: []
---

# Threat Prevention Command Reference

**Version scope:** R81.20–R82 | **Confidence:** medium-high

## Operator-First Boundary
- Guides are heavier on deployment workflow than gateway CLI troubleshooting.
- Most explicit operator-grade commands are for policy installation and manual file submission to Threat Emulation.
- Threat Prevention policy installation is separable from Access Control policy installation when the source says to do so.

---

## `mgmt_cli install-policy <options>`
- **Purpose:** Install the Threat Prevention policy on specified Security Gateways from the Security Management Server.
- **What it proves:** Whether management can push the intended Threat Prevention policy to the selected targets.
- **Expected output:** Command run on Security Management Server; install targets explicitly selected; selected gateways receive the updated Threat Prevention policy on success.
- **If not:**
  - Install fails on a cluster member → treat as gateway/cluster install problem, not proof that a protection itself is bad
  - Collect exact install failure text and pivot to policy-install/SIC runbooks
- **Note:** Threat Prevention has a dedicated policy and can be installed separately from Access Control. For clusters, policy installation is effectively all-members-or-fail for that cluster target.
- **Source:** R81.20 and R82 Threat Prevention Administration Guides

## `te_add_file -f=<file path>` / `te_add_file -d=<directory path>`
- **Purpose:** Manually send one file or a directory of files for Threat Emulation from Expert mode.
- **What it proves:** Whether ted accepts the file submission and what verdict/action is returned for the sample.
- **Expected output:** Connection progress (`Trying to connect to ted...`, `Connected to ted...Ready to send...`); response contains `:event_id`, `:action`, `:confidence`, `:done`, `:file_path`, `:md5_string`. `:done (1)` indicates the command completed its processing cycle for the shown sample.
- **If not:**
  - Cannot connect to ted → investigate Threat Emulation service/path health
  - Verdict output incomplete → capture raw command output and correlate with management policy state
- **Caution:** Must be run from Expert mode. Evidence collection and sample submission only — not a substitute for validating that the intended Threat Prevention policy is installed.
- **Source:** R81.20 and R82 Threat Prevention Administration Guides

---

## Threat Prevention Policy Install Workflow (from guides)
- Threat Prevention has a **dedicated policy** and can be installed separately from Access Control.
- Installing only the Threat Prevention policy is explicitly recommended to **minimize performance impact** during this workflow.
- For clusters, policy installation is effectively all-members-or-fail for that cluster target.
- `Install the Threat Prevention policy` is framed as a distinct step after configuration in the corpus.

---

## Practical Low-Risk Order
1. Confirm whether the issue is policy deployment, verdicting, or file-submission behavior.
2. If deployment-related: `mgmt_cli install-policy <options>` on management server; capture exact target/failure details.
3. If sample-behavior-related: `te_add_file` from Expert mode; save returned event/action/confidence fields.
4. If still ambiguous: correlate with existing policy-install and SIC runbooks before suggesting changes.

---

## Related Pages
- [[policy-install-failures-gateway-side]] — Policy install failure runbook
- [[sic-trust-communication-issues]] — SIC trust issues
- [[check-point-sr-workflow-hermes]] — SR evidence methodology
