# cpinfo Troubleshooting Runbook

**Source:** Michael's personal triage methodology
**Profile SR:** 6-0004539478 (HPASCACALQ9-SG2-SEC)
**Profile cpinfo:** 70 sections, 237 command outputs, 2084 extracted files

## Purpose
Fast triage guide for parsed cpinfo output produced by `/home/michael/Work/bin/sr-cpinfo-ingest`.

## Intended Layout
- Raw bundle: `/home/michael/Work/<SR>/artifacts/`
- Parsed output: `/home/michael/Work/<SR>/exports/cpinfo-parser/<run-name>/`

## General Triage Order

1. `sections/CP_Status.txt` — blade health, feature status, quick orientation
2. `sections/System_Information.txt` — platform, version, boot/runtime context
3. `commands/Enabled_blades.txt` — which blades/features are in play
4. `sections/FireWall-1_Statistics_(fw_ctl_pstat).txt` — memory/queue/accelerator pressure
5. `sections/CoreXL.txt` — CoreXL workers, affinity, distribution, imbalance hints
6. `sections/FW-1_Accelerator.txt` — SecureXL/acceleration state and datapath behavior
7. `sections/_bin_dmesg.txt` and `files/var/log/dmesg` — kernel and driver errors
8. `files/var/log/messages*` — OS/system events over time
9. `sections/High_Availability.txt` + `commands/*cphaprob*` — ClusterXL/HA state
10. `sections/IP_Interfaces.txt`, `sections/Routed_Information.txt`, `commands/netstat_*`, `commands/ip_route*` — interface/routing path validation

## High-Value Areas by Symptom

### 1. Connection Delay / Forwarding Delay
**Check first:**
- `sections/IP_Interfaces.txt`
- `sections/Routed_Information.txt`
- `sections/FW-1_Accelerator.txt`
- `sections/CoreXL.txt`
- `sections/FireWall-1_Statistics_(fw_ctl_pstat).txt`
- `commands/netstat_-nap.txt`
- `commands/ip_route.txt`
- `files/proc/net/route`
- `files/proc/net/softnet_stat`

**Why:** Confirms route/interface correctness, queueing pressure, acceleration state, and backlog indicators.

### 2. CPU Spike / Worker Contention / Scheduling Issues
**Check first:**
- `sections/CoreXL.txt`
- `sections/FW-1_Accelerator.txt`
- `sections/FireWall-1_Statistics_(fw_ctl_pstat).txt`
- `commands/top_-b_-n_5_-H.txt`
- `commands/ps_auxww.txt`
- `commands/ps_fww_-Ao_psr_uname_pid_ppid_pcpu_pmem_rss_vsz_stat_time_start_cmd.txt`
- `files/proc/ppk/profile_cpu_stat`
- `files/proc/ppk/affinity`
- `files/proc/ppk/drop_statistics`
- `files/var/log/spike_detective/*`

**Why:** Shows worker placement, hot threads, drops, queue pressure, and spike forensics.

### 3. ClusterXL / HA / Sync Problems
**Check first:**
- `sections/High_Availability.txt`
- `commands/High_Availability_State__cphaprob_state_.txt`
- `commands/High_Availability_interfaces__cphaprob_-a_if_.txt`
- `commands/High_Availability_SyncStat__cphaprob_syncstat_.txt`
- `commands/Cluster_failover_history__cphaprob_-h_show_failover_.txt`
- `commands/Bond_interfaces__cphaprob_show_bond_.txt`
- `files/var/log/messages*`
- `files/var/log/dmesg`

**Why:** Establishes whether problem is failover, sync transport, interface state, or downstream symptom of load.

### 4. VSX / VS-Specific Issues
**Check first:**
- `sections/VSX_Information_(CTX_3).txt`
- `commands/VSX_stat.txt`
- `commands/VSX_stat_-l.txt`
- `files/var/log/routed_messages_vs3`
- `files/opt/CPsuite-R82/fw1/CTX/CTX00003/...`
- `files/opt/CPshrd-R82/CTX/CTX00003/...`

**Why:** For VS problems, prioritize matching CTX/VS artifacts before broad global Gaia state.

**Rule:** If the case is about a specific VS, prioritize matching `CTXxxxxx` and `routed_messages_vsX` artifacts before broad global logs.

### 5. Kernel / Driver / Crash Suspicion
**Check first:**
- `sections/_bin_dmesg.txt`
- `files/var/log/dmesg`
- `files/var/log/messages*`
- `sections/Core_Dumps.txt`
- `commands/Kernel_Core_Dumps.txt`
- `commands/User_Mode_Core_Dumps.txt`
- `files/var/log/thread_blocker_device64.log`

**Optional but valuable when present:** `conf_param.elg`, `fw.elg`, other kernel/debug `.elg` logs.

**Why:** Kernel symptoms often show first in dmesg/messages/core indicators.

### 6. Routing / Daemon-Specific Issues
**Check first:**
- `sections/Routed_Information.txt`
- `commands/cpvinfo__bin_routed.txt`
- `files/etc/routed.conf`
- `files/etc/routed*.conf`
- `files/var/log/routed.log`
- `files/var/log/routed_*.log`
- `files/var/log/routed_messages`
- `files/var/log/routed_messages_vs*`

**Why:** Shows daemon config, per-VS routing behavior, and routed-specific errors not visible in generic system logs.

### 7. SecureXL / Affinity / Acceleration Path Issues
**Check first:**
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

**Why:** Fastest path to understanding queue/IRQ/worker placement and acceleration bottlenecks.

## What to De-prioritize Early
- Very large, noisy sections unless symptom specifically points there:
  - `sections/_opt_CPsuite-...tp_collector_cli.txt`
  - `sections/Java_parameters.txt`
  - `sections/Deployment_Agent_(DA)_info.txt`
  - Giant directory listings
- Use them later for deep dives, not as first pass.

## Practical First-Pass Checklist
1. Read: `sections/CP_Status.txt`, `sections/System_Information.txt`, `sections/FireWall-1_Statistics_(fw_ctl_pstat).txt`, `sections/CoreXL.txt`, `sections/FW-1_Accelerator.txt`, `sections/_bin_dmesg.txt`
2. Then branch based on symptom:
   - HA → `High_Availability` + `cphaprob`
   - VSX/VS → `VSX_Information_(CTX_X)` + `routed_messages_vsX`
   - Routing → `Routed_Information` + routed logs
   - CPU/perf → `top`, `ps`, `ppk`, `spike_detective`

## Specific Notes from Profiled cpinfo (SR 6-0004539478)
- Strong VS3/CTX00003 signal present
- `routed_messages_vs3` exists — high value
- `proc/ppk/*` artifacts exist — highly relevant for datapath/scheduling analysis
- `spike_detective` perf logs exist — highly relevant for CPU contention cases
- `conf_param.elg` was NOT present in this cpinfo — treat as optional

## Recommended Next Iteration
Profile 2-3 more cpinfos from different issue types:
- Pure cluster issue
- Pure CPU/drop issue
- Routing/VSX issue

Then convert into a normalized symptom-to-artifact matrix.
