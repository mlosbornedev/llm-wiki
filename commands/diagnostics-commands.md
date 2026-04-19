---
title: Diagnostics & Evidence Commands
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [reference, diagnostics, cpview, corexl, logging, r81, r82]
sources: []
---

# Diagnostics & Evidence Commands

**Version scope:** R81.20–R82 | **Confidence:** high

## `cpview`
- **Purpose:** First-pass built-in performance view on the gateway.
- **What it proves:** Current CPU, memory, disk, and blade statistics in a continuously updated text UI.
- **Expected output:** CPU and memory counters change live. Move from overview into subsystem views to confirm whether load is system-wide or blade-specific. Press `C` to save current page to capture file (`cpview_<pid>.cap<number>`).
- **If not:**
  - Intermittent issue → enable CPU Spike Detective
  - CPU high in one path → pivot to CoreXL distribution commands
  - Memory abnormal → correlate with SmartView Monitor, not guessing at a leak
- **Source:** R81.20 and R82 Performance Tuning guides

## `show spike-detective`
- **Purpose:** Inspect CPU Spike Detective state from Gaia Clish.
- **What it proves:** Whether spike monitoring is configured and available for intermittent CPU spikes.
- **Expected output:** Existing spike-detective configuration and saved spike information.
- **If not:** Use `set spike-detective` to arm monitoring if change control allows. If the problem is continuous, stay in `cpview` and CoreXL checks.
- **Source:** R81.20 and R82 Performance Tuning guides — *does not impact performance when monitoring*

## `fw ctl multik stat`
- **Purpose:** Show per-CoreXL firewall instance placement and current connection load.
- **What it proves:** Whether CoreXL instances are active, which CPU each runs on, and whether connection distribution is balanced.
- **Expected output:** Columns: `ID | Active | CPU | Connections | Peak`. Healthy: multiple active instances with non-zero connections, not all work pinned to one.
- **If not:**
  - Few/no instances active → validate CoreXL state first
  - One instance carries disproportionate connections → check queue utilization and affinity next
- **Caution:** Avoid `-d` parameter unless troubleshooting the command itself; redirect debug output to a file.
- **Source:** R81.20 and R82 Performance Tuning guides

## `fw ctl multik utilize`
- **Purpose:** Show CoreXL queue utilization per firewall instance.
- **What it proves:** Whether a specific instance is backlogged even if raw connection counts look normal.
- **Expected output:** Columns: `ID | Utilize(%) | QueueElements`. Persistent non-trivial queue growth on one instance suggests imbalance or localized pressure.
- **If not:**
  - Low utilization across instances → bottleneck may be outside CoreXL queueing
  - One instance is hot → inspect affinity with `fw ctl affinity -l -r`
- **Source:** R82 Performance Tuning guide; same command family present in R81.20

## `fw ctl affinity -l -r`
- **Purpose:** Show current affinity for interfaces, user-space processes, and CoreXL firewall instances in reverse order.
- **What it proves:** Which CPU cores handle interfaces, which run `fw_*` instances, and where `fwd` is allowed to run.
- **Expected output:** Interface lines (`ethX: CPUY`), CoreXL instance lines (`fw_0: CPU7`), process lines (`fwd: CPU2 3 4 5 6 7`).
- **If not:**
  - `fwd` forced onto saturated core → logging pressure may contribute to CPU symptoms
  - Interface/instance affinities unexpected → stop and confirm design intent before changing
- **Source:** R82 Performance Tuning guide

## `fw -i <instance> ctl get int fwsyslog_nlogs_counter`
- **Purpose:** Show current syslog log count for one CoreXL firewall instance.
- **What it proves:** Whether a specific instance is producing notable syslog logging.
- **Expected output:** Example: `fwsyslog_nlogs_counter= 21`
- **If not:**
  - Counters low while CPU high → logging less likely to be the primary driver
  - One instance much higher than others → correlate with CoreXL distribution and policy/logging design
- **Source:** R82 Logging and Monitoring guide

## `fw ctl zdebug | grep logs` with `fw ctl set int fwsyslog_print_counter 1`
- **Purpose:** Short, focused live check of per-instance syslog counters and total logs sent from kernel.
- **What it proves:** Whether the gateway is under heavy log-generation pressure and how logs are distributed across CoreXL instances.
- **Expected output:** `Number of logs sent from instance0 is ...` / `Total logs sent from kernel (all instances) = ...`
- **If not:**
  - Counter output stays quiet during symptom window → look elsewhere
  - Totals climb rapidly → treat heavy logging as a serious candidate
- **Caution:** Keep narrow and time-bounded. Stop with `Ctrl+C` after capture.
- **Source:** R82 Logging and Monitoring guide

## `cpstat`
- **Purpose:** Inspect feature-specific runtime status from the command line.
- **What it proves:** Subsystem health beyond raw OS counters — especially when CPView suggests one blade or service is driving the issue.
- **Expected output:** Blade-specific status and counters rather than generic OS view.
- **If not:** If CPView shows pressure but `cpstat` is quiet for the suspected blade, re-check whether the issue is platform-level.
- **Source:** R82 Logging and Monitoring guide

## `cpinfo -y all`
- **Purpose:** Collect broad support evidence after first-pass narrowing.
- **What it proves:** Consolidated gateway state for offline escalation and correlation.
- **Expected output:** Large support bundle.
- **If not:** Use after live triage establishes the likely domain — not as a substitute for first-pass isolation.
- **Source:** Ingested CLI references

---

## Practical Low-Risk Diagnostic Order
1. `cpview` (first-pass baseline)
2. `show spike-detective` (if intermittent)
3. `fw ctl multik stat` and `fw ctl multik utilize` (if CPU suspected)
4. `fw ctl affinity -l -r` (if CoreXL imbalance suspected)
5. `fw -i <instance> ctl get int fwsyslog_nlogs_counter` + `fw ctl zdebug | grep logs` (if logging suspected)
6. `cpstat` (blade-specific check)
7. `cpinfo -y all` (escalation data)

---

## Related Pages
- [[performance-degradation-cpu-memory]] — Full runbook context
- [[securexl-debugging]] — SecureXL dual-debug system (fwaccel + fw ctl)
- [[cpinfo-troubleshooting-runbook]] — cpinfo layout and symptom mapping
- [[clusterxl-redundancy]] — ClusterXL diagnostics
