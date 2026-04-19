---
title: CP5400-NS Appliance
created: 2026-04-19
updated: 2026-04-19
type: entity
tags: [ngtx-appliance, hardware, r81, r82, quantum-gateway]
sources: []
---

# CP5400-NS Appliance

## Overview
Check Point Next Gen Threat Extraction (NGTX) mid-range appliance. Part of the 5400 series lineup. Supports full threat prevention suite including Threat Emulation (sandboxing) and Threat Extraction (document sanitization).

## Hardware Specs
- **Form Factor:** 1U Rackmount
- **CPU:** Intel Xeon D-2100 series (multi-core)
- **Memory:** Up to 256GB RAM
- **Storage:** SSD (logs, snapshots)
- **Network:** 8x 1GbE copper + 2x 10GbE SFP+
- **NGTX Module:** Dedicated processing card for Threat Emulation/Extraction

## Position in Lineup
```
CP3200-NS (entry)
CP4400-NS (mid-low)
CP5400-NS (mid) ← this page
CP5600-NS (mid-high)
CP5800-NS (high)
```

## Software
- **OS:** Gaia OS (R81.20 JHF recommended)
- **JHF:** Jump Hotfixes — R81.20 JHF 122 is current as of this writing
- **Management:** Standalone or under SMS/MD

## Use Cases
- Perimeter firewall with sandboxing
- Branch office threat prevention
- Mid-market unified threat management
- Can serve as VSX host (up to 5 virtual systems)

## ClusterXL Support
- **HA Mode:** ✓ Full support
- **LS Unicast:** ✓ Full support
- **LS Multicast:** ✓ Full support
- **Active-Active:** ✓ (R80.40+) — no VSX in Active-Active

## Known Configurations
- This unit (serial: 00:1C:7F:AA:D4:3B) is configured for passthrough mode for [[sr-6-0004564689-golfview]]
- Passthrough mode: appliance operates as pure L2 bridge, no routing/NAT

## Related
- [[check-point-company]]
- [[clusterxl-redundancy]]
- [[vsx-virtual-systems]]
- [[sr-scaffold-workflow]]
