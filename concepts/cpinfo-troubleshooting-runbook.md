---
title: cpinfo Troubleshooting Runbook
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [cpinfo, troubleshooting, debugging, runbook, tac, reference]
sources: [raw/transcripts/checkpoint-cpinfo-troubleshooting-runbook-2026-04-19.md]
---

# cpinfo Troubleshooting Runbook

## Purpose
Fast triage guide for parsed cpinfo output. cpinfo bundles contain structured command outputs, log files, and system state captured from a Check Point gateway. This runbook maps symptoms to the specific cpinfo sections and files worth reading first.

**Intake path:** `/home/michael/Work/<SR>/artifacts/` → parsed by `sr-cpinfo-ingest` → output at `/home/michael/Work/<SR>/exports/cpinfo-parser/<run-name>/`

## General Triage Order

| Priority | File | What It Shows |
|----------|------|---------------|
| 1 | `sections/CP_Status.txt` | Blade health, feature status, quick orientation |
| 2 | `sections/System_Information.txt` | Platform, version, boot/runtime context |
| 3 | `commands/Enabled_blades.txt` | Which blades/features are in play |
| 4 | `sections/FireWall-1_Statistics_(fw_ctl_pstat).txt` | Memory/queue/accelerator pressure |
| 5 | `sections/CoreXL.txt` | CoreXL workers, affinity, distribution |
| 6 | `sections/FW-1_Accelerator.txt` | SecureXL/acceleration state |
| 7 | `sections/_bin_dmesg.txt` + `files/var/log/dmesg` | Kernel/driver errors |
| 8 | `files/var/log/messages*` | OS/system events over time |
| 9 | `sections/High_Availability.txt` + `commands/*cphaprob*` | ClusterXL/HA state |
| 10 | Interface/routing sections | Interface/routing path validation |

## Symptom-to-Artifact Mapping

### 1. Connection Delay / Forwarding Delay
**First read:**
- `sections/IP_Interfaces.txt`
- `sections/Routed_Information.txt`
- `sections/FW-1_Accelerator.txt`
- `sections/CoreXL.txt`
- `sections/FireWall-1_Statistics_(fw_ctl_pstat).txt`
- `commands/netstat_-nap.txt`
- `commands/ip_route.txt`
- `files/proc/net/route`
- `files/proc/net/softnet_stat`

**Why:** Confirms route/interface correctness, queueing pressure, acceleration state, backlog indicators.

### 2. CPU Spike / Worker Contention
**First read:**
- `sections/CoreXL.txt`
- `sections/FW-1_Accelerator.txt`
- `sections/FireWall-1_Statistics_(fw_ctl_pstat).txt`
- `commands/top_-b_-n_5_-H.txt`
- `commands/ps_auxww.txt`
- `commands/ps_fww_*` (worker-specific ps output)
- `files/proc/ppk/profile_cpu_stat`
- `files/proc/ppk/affinity`
- `files/proc/ppk/drop_statistics`
- `files/var/log/spike_detective/*`

**Why:** Worker placement, hot threads, drops, queue pressure, spike forensics.

### 3. ClusterXL / HA / Sync Problems
**First read:**
- `sections/High_Availability.txt`
- `commands/High_Availability_State__cphaprob_state_.txt`
- `commands/High_Availability_interfaces__cphaprob_-a_if_.txt`
- `commands/High_Availability_SyncStat__cphaprob_syncstat_.txt`
- `commands/Cluster_failover_history__cphaprob_-h_show_failover_.txt`
- `commands/Bond_interfaces__cphaprob_show_bond_.txt`
- `files/var/log/messages*`
- `files/var/log/dmesg`

**Why:** Establishes whether problem is failover, sync transport, interface state, or downstream load symptom.

### 4. VSX / VS-Specific Issues
**First read:**
- `sections/VSX_Information_(CTX_3).txt`
- `commands/VSX_stat.txt`
- `commands/VSX_stat_-l.txt`
- `files/var/log/routed_messages_vs3`
- `files/opt/CPsuite-R82/fw1/CTX/CTX00003/...`
- `files/opt/CPshrd-R82/CTX/CTX00003/...`

**Rule:** If case is about a specific VS, prioritize matching `CTXxxxxx` and `routed_messages_vsX` artifacts before broad global logs.

### 5. Kernel / Driver / Crash Suspicion
**First read:**
- `sections/_bin_dmesg.txt`
- `files/var/log/dmesg`
- `files/var/log/messages*`
- `sections/Core_Dumps.txt`
- `commands/Kernel_Core_Dumps.txt`
- `commands/User_Mode_Core_Dumps.txt`
- `files/var/log/thread_blocker_device64.log`

**Optional:** `conf_param.elg`, `fw.elg`, other `.elg` logs.

### 6. Routing / Daemon-Specific Issues
**First read:**
- `sections/Routed_Information.txt`
- `commands/cpvinfo__bin_routed.txt`
- `files/etc/routed.conf`
- `files/etc/routed*.conf`
- `files/var/log/routed.log`
- `files/var/log/routed_*.log`
- `files/var/log/routed_messages`
- `files/var/log/routed_messages_vs*`

### 7. SecureXL / Affinity / Acceleration Issues
**First read:**
- `sections/FW-1_Accelerator.txt`
- `commands/SIM_Affinity.txt`
- `commands/fw_affinity_-l_-a_-v.txt`
- `commands/fw_affinity_-l_-x_-flags_tn.txt`
- `commands/Affinity_of_Multi-Queue_IRQs.txt`
- `files/proc/ppk/affinity`
- `files/proc/ppk/cqstats`
- `files/proc/ppk/drop_statistics`
- `files/opt/CPsuite-R82/fw1/CTX/CTX00003/conf/manual.affinity.conf`
- `files/opt/CPsuite-R82/fw1/CTX/CTX00003/conf/fwkall.affinity.conf`

## De-prioritize Early
- `sections/_opt_CPsuite-...tp_collector_cli.txt` — noisy
- `sections/Java_parameters.txt` — noisy
- `sections/Deployment_Agent_(DA)_info.txt` — noisy
- Giant directory listings

Use for deep dives, not first pass.

## Practical First-Pass Checklist

**Always read first:**
1. `sections/CP_Status.txt`
2. `sections/System_Information.txt`
3. `sections/FireWall-1_Statistics_(fw_ctl_pstat).txt`
4. `sections/CoreXL.txt`
5. `sections/FW-1_Accelerator.txt`
6. `sections/_bin_dmesg.txt`

**Then branch:**
- HA → `High_Availability` + `cphaprob*`
- VSX/VS → `VSX_Information_(CTX_X)` + `routed_messages_vsX`
- Routing → `Routed_Information` + routed logs
- CPU/perf → `top`, `ps`, `ppk`, `spike_detective`

## Related
- [[securexl-debugging]]
- [[clusterxl-redundancy]]
- [[vsx-virtual-systems]]
- [[gaia-os]]
- [[check-point-company]]
