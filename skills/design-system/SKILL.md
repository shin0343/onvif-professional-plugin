---
name: design-system
description: Designs an ONVIF-based security system architecture from customer requirements. Examples: /onvif-pro:design-system factory 50-channel CCTV + 10-door access control + AI analytics, /onvif-pro:design-system small office 5 cameras with mandatory encrypted communication
---

# ONVIF System Design Skill

Analyzes the requirements in `$ARGUMENTS` and produces an optimal ONVIF-based security system architecture.

## Design Methodology

### Step 1: Requirements Analysis

Extract the following from `$ARGUMENTS`:
- **Scale**: number of channels, doors, and devices
- **Functions**: streaming, recording, access control, analytics, notifications
- **Technical requirements**: resolution, codec, security level, cloud integration
- **Environment**: network topology, bandwidth constraints

### Step 2: Per-Layer Profile Selection

**Video devices:**
- Standard IP camera → Profile S (required)
- H.265 / advanced camera → Profile T (replaces or supplements S)
- Camera with built-in SD card recording → add Profile G
- AI analytics camera → add Profile M
- NVR/VMS client → match the profiles supported by connected cameras

**Access control devices:**
- Access control panel → Profile C
- Card / biometric reader → Profile D
- ACS management software → Profile A

**Security layer:**
- Encrypted communication required → TLS Configuration Add-on
- Cloud integration → Cloud Integration Service
- Remote management → Uplink Service

### Step 3: Bandwidth Reference Guide

| Codec | Resolution | Bandwidth @ 30fps |
|-------|------------|-------------------|
| H.264 | 1080p | 4–8 Mbps |
| H.265 | 1080p | 2–4 Mbps |
| H.264 | 4K | 15–25 Mbps |
| H.265 | 4K | 8–15 Mbps |

### Step 4: Output Format

Deliver the design proposal in the following format:

```
## ONVIF System Design Proposal

**Requirements summary:** [analysis of input]

### Device Configuration
| Device Type | Qty | Required Profiles | Notes |
|-------------|-----|-------------------|-------|

### Network Design
- Estimated total bandwidth: [calculated value] Mbps
- Switch recommendation: Gigabit / PoE required?
- VLAN segmentation: camera / access control / management networks

### ONVIF Profile Summary
- Required profiles: [list]
- Recommended additions: [list]

### Integration Considerations
1. [Key consideration]

### Security Recommendations
- [Security design advice]

### Implementation Phases
1. Phase 1: [Foundation]
2. Phase 2: [Expansion]

### Verify Compatible Devices
- https://www.onvif.org/conformant-products/
```
