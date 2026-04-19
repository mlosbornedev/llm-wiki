---
title: Performance Degradation — CPU/Memory Runbook
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [troubleshooting, performance, cpu, memory, corexl, r81, r82]
sources: []
---

# Performance Degradation — CPU/Memory Runbook

**Version scope:** R81.20–R82 | **Confidence:** medium-high

## Goal
Determine whether performance degradation is in CPU, memory, logging, or CoreXL distribution — and gather the right evidence to prove it.

---

## Step 1 — Establish Symptom Context
- **Intent:** Classify the performance issue type before investigating.
- **Action:** Capture exact symptom window, impact type (sustained vs spiky), and affected throughput/latency.
- **Expected:** Issue classified as CPU-bound, memory-bound, logging-pressure, or CoreXL imbalance.
- **If not:** Request clarification on whether the issue is continuous or intermittent.

## Step 2 — CPView First-Pass Capture
- **Intent:** Safe first-pass baseline across CPU, memory, disk, and affected blades.
- **Action:** Run `cpview` and capture CPU, memory, disk, and blade statistics.
- **Expected:** Live counters show which resource is under pressure.
- **If not:**
  - If CPU is high in only one path, pivot to CoreXL distribution commands
  - If intermittent, enable CPU Spike Detective
  - If memory counters are abnormal, correlate with SmartView Monitor

## Step 3 — CoreXL Instance Distribution (If CPU Suspected)
- **Intent:** Determine whether pressure is distributed or isolated to one CoreXL instance.
- **Action:** `fw ctl multik stat` — shows per-instance placement and connection load.
- **Expected:** Multiple active instances with non-zero connections; not all work pinned to one.
- **If not:**
  - If few/no instances are active, validate CoreXL state first
  - If one instance carries disproportionate load, check queue utilization next

## Step 4 — CoreXL Queue Utilization
- **Intent:** Detect backlogged instances even when raw connection counts look normal.
- **Action:** `fw ctl multik utilize` — shows per-instance queue depth and utilization %.
- **Expected:** Low queue depth across instances.
- **If not:**
  - If one instance is hot, inspect affinity with `fw ctl affinity -l -r`
  - If utilization is low across instances, bottleneck may be outside CoreXL

## Step 5 — CoreXL Affinity Inspection
- **Intent:** Prove whether affinity or interface layout explains a hot CPU.
- **Action:** `fw ctl affinity -l -r` — shows current affinity for interfaces, user-space processes, and CoreXL instances.
- **Expected:** Affinity layout aligns with expected design; `fwd` not forced onto saturated cores.
- **If not:**
  - If `fwd` is on a saturated core, logging pressure may be contributing
  - If interface/instance affinities look unexpected, stop and confirm design intent before changing

## Step 6 — Logging Pressure Check (If CPU Still Unexplained)
- **Intent:** Prove or rule out heavy logging as a CPU driver.
- **Action:** `fw -i <instance> ctl get int fwsyslog_nlogs_counter` per instance; then `fw ctl set int fwsyslog_print_counter 1` and short `fw ctl zdebug | grep logs` sample.
- **Expected:** Quiet counters if logging is not the driver.
- **If not:**
  - If totals climb rapidly, treat heavy logging as a serious candidate
  - Correlate with `fwd` affinity and CPView CPU views

## Step 7 — Memory Pressure Check
- **Intent:** Rule out or confirm memory as the bottleneck.
- **Action:** CPView memory pages and any supporting monitor view.
- **Expected:** Memory counters within normal range.
- **If not:**
  - If pressure is confirmed, look for memory leak patterns vs. expected peak load
  - Correlate with connection table size

## Step 8 — Intermittent CPU Spikes
- **Intent:** Capture evidence for bursty/intermittent events.
- **Action:** `show spike-detective` to inspect saved spike information, or saved spike evidence.
- **Expected:** Spike history correlates with reported symptom window.
- **If not:** Use `set spike-detective` to arm monitoring if change control allows.

## Step 9 — Escalation Data Collection
- **Intent:** Preserve escalation-grade evidence after narrowing the domain.
- **Action:** `cpinfo -y all` after symptom domain is narrowed.
- **Expected:** Full support bundle tied to the specific symptom.

---

## Minimum Artifact Request Checklist (in order)
1. Exact symptom window, impact type, sustained vs spiky
2. CPView captures for CPU, memory, disk, and affected blades
3. `fw ctl multik stat` and `fw ctl multik utilize`
4. `fw ctl affinity -l -r`
5. Per-instance `fwsyslog_nlogs_counter` and `fw ctl zdebug | grep logs` sample
6. CPView memory pages
7. `show spike-detective` or saved spike evidence for intermittent events
8. `cpinfo -y all` after symptom domain is narrowed

---

## Key Source Notes
- R81.20 and R82 Performance Tuning guides explicitly document `fw ctl multik stat`, `fw ctl multik utilize`, and `fw ctl affinity -l -r`
- CPU Spike Detective does **not** impact performance when monitoring
- `fw ctl zdebug | grep logs` should be narrow and time-bounded; stop with `Ctrl+C` after capture
- Avoid `-d` parameter on `fw ctl multik stat` unless troubleshooting the command itself

---

## Related Pages
- [[securexl-debugging]] — SecureXL debugging layers
- [[gaia-os]] — `cpview`, baseline commands
- [[check-point-sr-workflow-hermes]] — SR evidence methodology
