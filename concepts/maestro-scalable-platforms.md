---
title: Maestro Scalable Platforms
created: 2026-04-19
updated: 2026-04-19
type: concept
tags: [maestro, scalable-platforms, maestro-orchestrator, cluster, r82, quantum-gateway]
sources: []
---

# Maestro Scalable Platforms

## Overview
Maestro is Check Point's scale-out architecture that combines multiple Quantum Security Gateways into a single logical security device, managed as one. Uses an **Orchestrator** appliance as the control plane.

## Architecture

### Orchestrator Appliances
The brain of the Maestro group. Types:
- **A800** — Entry orchestrator
- **A1000** — Mid-range orchestrator
- **A1200** — High-end orchestrator (R82)

Orchestrator runs the **Security Group** — the logical aggregation of all member appliances.

### Security Group Members (SGM)
The actual firewall appliances. Each SGM contributes:
- Interfaces (up to 16x 100GbE in R82)
- CPU cores for firewall/policy processing
- Memory for state tracking
- Threat Prevention blades (if NGTX equipped)

## Key Capabilities (R82)
- Up to **16 SGM per Security Group** (doubled from R81's 8)
- **Auto-scaling** of throughput by adding SGM
- **Single management IP** for entire group
- **Single policy** for entire group
- Independent failover — any SGM can fail without full group outage
- Full ClusterXL feature parity (HA, LS Unicast, LS Multicast)
- **No VSX support** (unlike standalone appliances)

## Deployment Patterns

### Horizontal Scale (More Members)
Add more SGM to existing orchestrator → more throughput capacity
- Good for: growing bandwidth needs, more concurrent connections

### Vertical Scale (Bigger Members)
Replace SGM with higher-throughput appliances
- Good for: per-connection bandwidth increases

## vs. Traditional ClusterXL
| Feature | ClusterXL | Maestro |
|---------|-----------|---------|
| Max members | 5 | 16 (R82) |
| Management | Per-gateway | Single IP |
| Scaling | Manual | Add SGM |
| Orchestrator | None | Required |
| VSX support | Yes | No |
| Cloud | No | AWS/Azure native |

## Orchestrator CLI
```bash
show orchestrator-group           # Full group status
show orchestrator-group members   # Individual SGM status
show security-group               # Security Group info
show interfaces                    # All SGM interfaces
show all statistics                # Aggregate throughput

# Security Group management
add security-group <name>
set security-group <name> <params>
```

## Related
- [[check-point-company]]
- [[clusterxl-redundancy]]
- [[r82-release]]
- [[vsx-virtual-systems]]
- [[gaia-os]]
