---
title: Check Point Software Technologies
created: 2026-04-19
updated: 2026-04-19
type: entity
tags: [company, quantum-gateway, spark-gateway, ngtx-appliance]
sources: []
---

# Check Point Software Technologies

## Overview
Check Point Software Technologies is a network security company founded in 1993 (Jerusalem, Israel) by Gil Shwed, Marius Nacht, and Shlomo Kremer. Headquartered in Tel Aviv, Israel, with US headquarters in San Carlos, CA. NASDAQ: CHKP.

## Core Products

### Quantum Security Gateways
High-end firewall appliances. Current generation (as of R82):
- **1600 Series** — SMB
- **3000 / 3000C Series** — Mid-market
- **5000 / 6000 Series** — Enterprise
- **24000 / 26000 Series** — Data center / hyperscale

### NGTX Appliances (Next Gen Threat Extraction)
Appliances with dedicated NGTP (Next Promotion Threat Prevention) blades:
- **5400-NS** — Mid-range NGTX
- **5600-NS** — High-end NGTX
- **5800-NS** — Top-tier NGTX

### Maestro
Check Point's scale-out security solution using orchestrator appliances:
- **Maestro HyperScale** — single-gateway security with multi-SG engine orchestration
- Supports up to 16 Security Group Members per orchestrator group
- Combines multiple appliances into a single logical gateway

### VSX (Virtual System Extension)
Hardware-based virtualization for creating multiple virtual security gateways on a single appliance. See [[vsx-virtual-systems]].

## Software Platform: Gaia OS
All modern Check Point appliances run Gaia OS (based on CentOS/rhel). See [[gaia-os]].

## Management Architecture
- **Security Management Server (SMS)** — formerly SmartCenter
  - R80.x / R81.x / R82.x management
  - Single management vs. Multi-domain (MDM)
- **SmartConsole** — Windows-based management client
- **WebUI / CLISH** — Gaia management interfaces

## Key Releases
- **R77** — Legacy, still prevalent in many envs
- **R80.x** — Introduction of Policy Layers, ThreatCloud
- **R80.20+** — VSX improvements, ClusterXL upgrades
- **R81** — New SmartConsole, ThreatCloud v2
- **R81.10** — Current long-term
- **R81.20** — New management UI, Security Group improvements
- **R82** — Latest release, Maestro enhancements, cloud-native integrations

## TAC Organization
Check Point TAC is organized by product line:
- **High End Products Team** — Scalable Platforms, Maestro, VSX, ClusterXL
- **SMB / 600 Series Team** — Lower-end appliances
- **Management Team** — SMS, SmartConsole, logging
- **Threat Prevention Team** — SandBlast, Threat Emulation, Threat Extraction

## Related
- [[r82-release]]
- [[clusterxl-redundancy]]
- [[vsx-virtual-systems]]
- [[maestro-scalable-platforms]]
