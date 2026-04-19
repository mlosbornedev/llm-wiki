# Check Point fwaccel / SecureXL Kernel Debug Reference

**Source:** SK171943 — Advanced "fw ctl debug" features
**Product:** Quantum Security Gateways
**Versions:** R80.20 (EOS), R80.30 (EOS), R80.40 (EOS), R81 (EOS), R81.10, R81.20, R82, R82.10
**OS:** Gaia
**Last Modified:** 2026-03-30
**Internal note:** Jira ACCL-930
**Preserved from:** Michael's personal notes, 2026-04-01

## fwaccel dbg Command

Controls SecureXL debug. In a cluster, must configure all members the same way. On Scalable Platforms (Maestro/Chassis), connect to the applicable Security Group.

### Syntax (Gaia Clish / Expert / Scalable Platform gCLISH)

```
fwaccel dbg -h
fwaccel dbg -m <module> all | + <flags> | - <flags> | reset | -f {"<5-tuple>"|reset} | list | resetall
```

On Scalable Platform Expert mode: `g_fwaccel dbg` (same syntax).

### Key Parameters

- `-m <module>` — specifies the SecureXL debug module
- `all` — enables all debug flags for the specified module
- `+ <flags>` — enables specified flags (space after + required)
- `- <flags>` — disables specified flags (space after - required)
- `reset` — resets debug flags for specified module to default
- `-f "<5-tuple>"` — debug filter: `"<src_ip>,<src_port>,<dst_ip>,<dst_port>,<protocol>"` (wildcard: `*`)
- `list` — shows all enabled flags in all modules
- `resetall` — resets all debug flags for all modules

### Debug Log File Locations

**Kernel Mode (KPPAK):**
- `$FWDIR/log/fwk.elg` — Firewall module processing
- `/var/log/messages` — additional info

**User Mode (UPPAK):**
- `$FWDIR/log/fwk.elg` — Firewall module
- `/var/log/usim_x86.elg` — SecureXL processing
- `/var/log/messages` — ADP module (NVIDIA ConnectX 100G Cards)

## SecureXL Debug Modules

### Module: default
Flags: `init drv tag lock cpdrv routing kdrv tcp_sv svm iter conn htab del update acct conf stat queue ioctl corr util rngs relations ant conn_app rngs_print infra_ids offload nat`

### Module: db
Flags: `get save del tmpl tmo init ant profile nmr nmt warning`

### Module: api
Flags: `init add update del acct conf stat vpn notif tmpl sv pxl qos gtp infra tmpl_info upd_conf upd_if_inf add_sa del_sa del_all_sas misc get_features get_tab get_stat reset_stat tag long_ver del_all_tmpl get_state upd_link_sel`

### Module: pkt
Flags: `f2f frag spoof acct notif tcp_state tcp_state_pkt sv cpls routing drop pxl qos user deliver vlan pkt nat wrp corr caf bhm geneve sctp`

### Module: infras
Flags: `reorder pm`

### Module: tmpl
Flags: `dtmpl_get dtmpl_notif tmpl`

### Module: vpn
Flags: `vpnpkt linksel routing vpn ls`

### Module: nac
Flags: `db db_get pkt pkt_ex signature offload idnt ioctl nac`

### Module: cpaq
Flags: `init client server exp cbuf opreg transport transport_utils broadcast`

### Module: synatk
Flags: `init conf conn log pkt proxy state msg`

### Module: adp
Flags: `rt nh eth heth wrp inf mbs bpl bplinf mbeinf if drop bond xmode ipsctl ac_print cpfifo qconf qcomm filter packet mcast hw_offload hw_expn rte_api`

### Module: dos
Flags: `fw1-cfg fw1-pkt sim-cfg sim-pkt detailed-pkt detailed-cfg drop cache`

### Module: gtp
Flags: `pkt policy tables api drop notif general`

### Module: usdisp
Flags: `error conn packet api msg state packet_err counter event quota ioctl lock clb uid queue fwstats cachetab vpn temp_conns prio route dumbo`

## Standard Kernel Debug Procedure

Three-action standard:
1. `fw ctl debug -buf <size>`
2. `fw ctl debug -m <module> + <flags>` and `fwaccel dbg -m <module> + <flags>`
3. `fw ctl kdebug -f`

Short version: `fw ctl zdebug` — resets pre-existing debug flags and filters, uses 1024K buffer by default, does not support all advanced options.

### Full Debug Procedure (Step-by-Step)

1. `expert` — enter expert mode
2. `fw ctl debug 0` — reset to defaults
3. `fwaccel dbg resetall` — reset all SecureXL flags
4. `fw ctl set int simple_debug_filter_off 1` — disable existing filters
5. Configure filters (5-tuple, host IP, or VPN peer)
6. `fw ctl debug -buf 8200` — allocate kernel buffer (size 8200)
7. Verify buffer allocation
8. Enable kernel module flags: `fw ctl debug -m <module> + <flags>`
9. Enable SecureXL flags: `fwaccel dbg -m <module> + <flags>`
10. Verify settings with `fw ctl debug -m <module>` and `fwaccel dbg list`
11. Start capture: `fw ctl ndebug ... -o /path/file` or `fw ctl kdebug -f -o /var/log/kernel_debug.txt`
12. Reproduce issue
13. Stop output (CTRL+C)
14. `fw ctl debug 0` — restore defaults
15. `fwaccel dbg resetall` — reset SecureXL
16. `fw ctl set int simple_debug_filter_off 1` — disable filters
17. Verify defaults restored
18. Collect: `/var/log/kernel_debug.txt`, `/var/log/messages*`, `$FWDIR/log/fwk.elg*`, `/var/log/usim_x86.elg*`

### Boot-Time Debug Flags

From R81.20, SecureXL debug can start at boot. Configure via:
- `$FWDIR/conf/fwaccel_dbg_flags.cfg`
- `$FWDIR/conf/fwaccel6_dbg_flags.cfg`

Format: one line per module. Example: `echo "pkt tcp_state routing" >> $FWDIR/conf/fwaccel_dbg_flags.cfg`

Kernel boot-time flags: `$FWDIR/modules/fwkern.conf` (kernel parameters).

## Advanced Debug Options

- `-d` — include/exclude strings in debug collection
- `-s` — stop debug when string appears
- `-v` and `-k` — VSX / VSNext support
- `-F` and `-H` — tuple / host debug filters
- `-e` — INSPECT filter
- `-f` and `-t` — message type / frequency thresholds
- `-buf` and `-b` — kernel/user-space buffer sizing
- `fw ctl kdebug -m / -s` — cyclic output files
- `-T` — microsecond timestamps
- `-x` — avoid unless explicitly needed

## Kernel Debug Filters

### 5-Tuple Filter
Configure via `-F` or kernel parameters:
```
fw ctl set str simple_debug_filter_saddr_1 "192.168.20.30"
fw ctl set str simple_debug_filter_daddr_1 "172.16.40.50"
fw ctl set int simple_debug_filter_dport_1 80
```

Up to 5 tuple filters simultaneously.

### Host IP Filter
Via `-H` or `simple_debug_filter_addr_<N>` kernel parameters. Up to 3 host-IP filters.

### VPN Peer Filter
Via `simple_debug_filter_vpn_<N>`. Up to 2 VPN-peer filters.

### Disable All Filters
`fw ctl set int simple_debug_filter_off 1`

Filters apply to both accelerated and non-accelerated traffic.

## Connection Life Cycle Debug

Start: `conn_life_cycle.sh -a start -o /var/log/kernel_debug.txt -T -f "<5-tuple>"`
Stop: `conn_life_cycle.sh -a stop -o /var/log/kernel_debug_formatted.txt`

Output is Ruby-style hierarchical text. Up to 5 filters. 0 means any for ports/protocol.

Example: `fw ctl debug -m fw + conn drop` then `conn_life_cycle.sh -a start -o /var/log/kernel_debug.txt -T -f "172.20.168.15,0,192.168.3.53,22,6"`

## Best Practices

- Use `fw ctl debug 0` to reset to default
- Avoid `fw ctl debug -x` unless explicitly needed
- Allocate kernel buffer with `fw ctl debug -buf 8200`
- Allocate user-space buffer at least 8200 when needed
- Use `-T` for microsecond timestamps
- Write output under `/var/log/`
- Consider cyclic files for long captures
- Use maintenance window — debug increases CPU load
- Prefer console access
- On R82 and high-core-count systems, new kernel debug behavior applies
