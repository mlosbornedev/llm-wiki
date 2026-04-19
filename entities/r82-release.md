---
title: R82 Release
created: 2026-04-19
updated: 2026-04-19
type: entity
tags: [r82, quantum-gateway, maestro, vsx, clusterxl, gaia-os]
sources: []
---

# R82 Release

## Overview
R82 is Check Point's latest major release (GA: late 2025). Successor to R81.20/R81.10. Available on Quantum Security Gateways, Maestro orchestrators, and virtual appliances.

## Key New Features

### Maestro Enhancements
- Up to 16 Security Group Members per orchestrator group (up from 8 in R81)
- New Security Group deployment workflow
- Improved auto-recovery of failed members
- Full support for 16x 100GbE interfaces

### ClusterXL Improvements
- Active-Active mode improvements (R80.40+ feature extended)
- New `clusterXL_admin` command enhancements
- Better VMAC placement control
- CCP encryption now supported in all ClusterXL modes

### VSX (Virtual Systems)
- VSX now supports 24+ virtual systems per appliance (hardware-dependent)
- New `vsx_util` diagnostics in R82
- VSX capacity improvements for hyperscale deployments

### Security Management
- New R82 WebUI (separate from SmartConsole)
- API-based management replacing some legacy cpstat/cpmish calls
- Domain-level API keys for multi-domain environments

### Performance
- New subscription licensing model
- Firewall throughput improvements (up to 3x vs R81.20 on same hardware)
- DPDK optimization for packet forwarding

## Supported Hardware
- Quantum 6000/24000 series (R82 certified)
- Maestro Orchestrator (A800/A1000/A1200)
- VSX virtual appliances (Google Cloud, AWS, Azure)
- NGTX 5400/5600/5800 series

## Upgrade Path
- R81.10 → R82: Direct upgrade supported
- R80.20.x → R82: Upgrade via R81.10 recommended
- R77.x: Requires fresh install or migration path (no direct upgrade)

## Known Issues (as of R82 GA)
- Some VSX customers report CCP flap during Security Group state transitions
- Active-Active ClusterXL requires manual VMAC configuration on some older 3100/3200 appliances
- Policy installation from R81.20 MDS to R82_DOMAIN requires SmartConsole upgrade

## Related
- [[check-point-company]]
- [[clusterxl-redundancy]]
- [[maestro-scalable-platforms]]
- [[vsx-virtual-systems]]
- [[gaia-os]]
