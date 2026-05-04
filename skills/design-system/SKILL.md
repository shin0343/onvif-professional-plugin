---
name: design-system
description: Designs an ONVIF-based security system architecture from customer requirements, covering device-profile mapping, network design, protocol stack, bandwidth calculation, and security. Examples: /onvif-pro:design-system factory 50-channel CCTV + 10-door access control + AI analytics, /onvif-pro:design-system small office 5 cameras encrypted communication
---

# ONVIF System Design Skill

Analyzes the requirements in `$ARGUMENTS` and produces an optimal ONVIF-based security system architecture.

---

## ONVIF Protocol Stack for System Design

Understanding the protocol stack is essential for network and security design:

| Protocol | Purpose | Ports | Network Requirement |
|----------|---------|-------|---------------------|
| HTTP | SOAP/WSDL control (all ONVIF management) | 80, 8080, 8000 | TCP, low bandwidth |
| HTTPS | Encrypted control (TLS Add-on) | 443 | TCP, TLS cert needed |
| RTSP | AV stream session setup | 554 (standard) | TCP |
| RTP/UDP | Audio/video media payload | Ephemeral (>1024) | UDP, QoS recommended |
| RTP/TCP | RTP over TCP (NAT/firewall fallback) | Same as RTSP | TCP |
| RTSPS | Encrypted RTSP (TLS) | 322 | TCP, TLS |
| WS-Discovery | Device auto-discovery | 3702/UDP | Multicast (239.255.255.250) |
| MQTT | Analytics/event delivery (Profile M) | 1883 / 8883 (TLS) | TCP |

> **Design rule:** Always open port 80 for SOAP control; port 554 for RTSP streaming; port 3702 UDP for WS-Discovery. VLAN segment cameras from management networks.

---

## Design Methodology

### Step 1: Requirements Analysis

Extract from `$ARGUMENTS`:
- **Scale**: number of channels (cameras), doors, readers, analytics nodes
- **Functions**: live streaming, edge/central recording, access control, AI analytics, notifications
- **Technical requirements**: resolution, codec (H.264 vs H.265), PTZ, two-way audio, biometrics, TLS, cloud integration
- **Environment**: network topology, bandwidth constraints, physical security zones, internet connectivity

### Step 2: Per-Layer Profile Selection

**Video devices (cameras):**
| Camera Type | Required Profile | Add-ons | Key WSDL |
|-------------|-----------------|---------|----------|
| Standard IP camera (H.264) | Profile S | — | media.wsdl |
| Advanced camera (H.265, HTTPS) | Profile T | TLS Add-on (if needed) | media2.wsdl |
| Camera with built-in SD card | Profile T + Profile G | — | media2.wsdl + recording.wsdl |
| AI analytics camera | Profile T + Profile M | — | media2.wsdl + analytics.wsdl |
| Full-featured camera | Profile T + G + M | TLS Add-on | All above |

**Video clients (NVR/VMS):**
- Must support the same profiles as connected cameras
- Profile T NVR: implements Media2 client (H.265, metadata)
- Profile G NVR: implements search.wsdl + replay.wsdl for playback

**Access control devices:**
| Device Type | Required Profile | Key WSDL |
|-------------|-----------------|----------|
| Access control panel (door control) | Profile C | accesscontrol.wsdl, doorcontrol.wsdl |
| Biometric/card reader | Profile D | credential.wsdl |
| ACS management software | Profile A | accessrules.wsdl, credential.wsdl, schedule.wsdl |

**Security layer:**
- Encrypted ONVIF communication → TLS Configuration Add-on + advancedsecurity.wsdl
- Cloud integration → Cloud Integration Service (cloudintegration.yaml)
- Remote management → Uplink Service (uplink.wsdl)

### Step 3: Bandwidth Calculation

**Video stream bandwidth reference:**
| Codec | Resolution | Frame Rate | Bandwidth |
|-------|------------|-----------|-----------|
| H.264 | 1080p | 30fps | 4–8 Mbps |
| H.265 | 1080p | 30fps | 2–4 Mbps |
| H.264 | 4K (8MP) | 30fps | 15–25 Mbps |
| H.265 | 4K (8MP) | 30fps | 8–15 Mbps |
| H.265 | 4K (8MP) | 15fps | 4–8 Mbps |
| H.264 | 720p | 30fps | 1.5–4 Mbps |

**ONVIF control overhead:** Negligible (SOAP/HTTP bursts only during configuration/commands)
**RTSP control overhead:** ~10–50 Kbps per stream (RTCP, keep-alive)
**MQTT/event overhead:** Negligible (event messages, typically < 1KB each)

**Formula:** Total bandwidth = (cameras × per-camera bitrate) × 1.15 (overhead factor)

### Step 4: Network Architecture Design

**VLAN segmentation (recommended):**
- **VLAN 10 — Camera network**: IP cameras only; RTSP streams flow here
- **VLAN 20 — Management network**: NVR/VMS, ONVIF SOAP control, WS-Discovery
- **VLAN 30 — Access control network**: ACS panels, readers, door controllers
- **VLAN 99 — Server network**: Analytics servers, MQTT broker, recording storage

**Switch requirements:**
- Gigabit PoE switch for cameras (IEEE 802.3bt/at)
- PoE budget: typical IP camera 5–15W; PTZ camera up to 25W
- Enable IGMP snooping for multicast-efficient WS-Discovery
- QoS: mark RTP/UDP traffic with DSCP EF (46) or AF41

**Firewall rules (minimum):**
- Camera VLAN → NVR VLAN: TCP 554 (RTSP), TCP 80 (SOAP), UDP ephemeral (RTP)
- NVR VLAN → Camera VLAN: TCP 80 (SOAP control), UDP 3702 (WS-Discovery)
- ACS VLAN → Management VLAN: TCP 80 (SOAP)

### Step 5: Output Format

```
## ONVIF System Design Proposal

**Requirements summary:** [analysis of input]

### Device Configuration
| Device Type | Qty | Required Profiles | Key WSDL Services | Notes |
|-------------|-----|-------------------|-------------------|-------|

### Network Design
- **Estimated total bandwidth**: [N] Mbps (at [codec]/[resolution]/[fps])
- **Switch recommendation**: Gigabit PoE, [N]× 802.3at/bt ports
- **VLAN segmentation**: Camera / Management / ACS / Server networks
- **Ports to open**: 80/TCP (SOAP), 554/TCP (RTSP), 3702/UDP (Discovery), ephemeral UDP (RTP)

### ONVIF Protocol Flow
- **Control**: SOAP/WSDL over HTTP → camera endpoint discovered via GetCapabilities
- **Streaming**: RTSP URI from GetStreamUri → RTP/UDP to NVR (or RTP/TCP through NAT)
- **Access events**: WS-BaseNotification pull-point → SOAP PullMessages from ACS panel
- **Analytics events**: MQTT broker ← camera (Profile M) OR WS-Notification pull-point

### ONVIF Profile Summary
- Required profiles: [list with device type]
- Recommended additions: [list with justification]
- Add-ons: [TLS / none]

### Security Recommendations
- Enable TLS Configuration Add-on for all ONVIF SOAP control (HTTPS)
- Use WS-Security UsernameToken with PasswordDigest (never send plaintext passwords)
- Change default ONVIF credentials on all devices before deployment
- Place cameras on isolated VLAN; restrict RTSP/SOAP access to NVR only
- Enable ONVIF → advancedsecurity.wsdl for certificate management

### Integration Considerations
1. Verify all camera models in ONVIF conformant products database
2. Profile T cameras: confirm NVR supports Media2 (H.265 Profile T)
3. Edge storage (Profile G): confirm SD card capacity for retention period
4. Analytics (Profile M): confirm MQTT broker reachable from camera VLAN

### Implementation Phases
1. **Phase 1 — Foundation**: Network infrastructure, VLAN config, core cameras (Profile S/T)
2. **Phase 2 — Storage & Access Control**: NVR integration (Profile G), ACS panels (Profile C/A/D)
3. **Phase 3 — Intelligence**: AI analytics cameras (Profile M), MQTT event platform, TLS hardening

### Verify Compatible Devices
- https://www.onvif.org/conformant-products/
- Use `/onvif-pro:conformance-check [profile]` for per-profile checklist
```
