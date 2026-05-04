# ONVIF Professional Plugin for Claude Code

> Expert plugin for ONVIF standards in IP-based physical security systems (CCTV / NVR / Access Control)

## Overview

Provides Claude with deep expertise across all ONVIF profiles (S/T/G/C/A/D/M), the complete protocol stack (HTTP + SOAP/WSDL + RTSP), network interface specifications, add-ons, conformance process, and SOAP message construction for IP-based physical security systems.

The agent responds **in the same language you write in** — English or Korean.

---

## ONVIF Protocol Stack

ONVIF는 **HTTP 기반**이며 다음 프로토콜 스택을 사용합니다:

| Layer | Protocol | Purpose |
|-------|----------|---------|
| Transport | HTTP / HTTPS | All ONVIF control messages |
| Service Description | WSDL | Formally defines every ONVIF service |
| Control Messaging | SOAP 1.2 | Encodes all API requests/responses |
| AV Streaming | **RTSP** | Delivers audio/video streams |
| Media Packetization | RTP / RTCP | Media packet transport (UDP/TCP) |
| Discovery | WS-Discovery | Device auto-discovery (UDP multicast) |

> ONVIF is HTTP-based, using **WSDL** (Web Services Description Language) and **SOAP** (Simple Object Access Protocol) for control, and **RTSP** (Real Time Streaming Protocol) for AV stream delivery.

---

## Installation

**Step 1 — Add the marketplace**

```
/plugin marketplace add shin0343/onvif-professional-plugin
```

**Step 2 — Install the plugin**

```
/plugin install onvif-pro@onvif-pro
```

**Step 3 — Verify**

```
/agents
/help
```

You should see **onvif-expert** listed under `/agents` and all `/onvif-pro:*` skills under `/help`.

---

## Available Skills

| Skill | Command | Description |
|-------|---------|-------------|
| Profile analysis | `/onvif-pro:check-profile [device/requirements]` | Recommend profiles + required WSDL services |
| Diagnostics | `/onvif-pro:diagnose [symptoms]` | Protocol-level compatibility and communication diagnostics |
| Spec lookup | `/onvif-pro:spec-lookup [service/operation]` | WSDL/XSD/spec details, SOAP namespaces, endpoint patterns |
| System design | `/onvif-pro:design-system [requirements]` | System architecture with protocol and network design |
| Conformance check | `/onvif-pro:conformance-check [profile]` | Full conformance checklist (Process Spec v5.7) |
| Policy / Add-on | `/onvif-pro:profile-policy [topic]` | Profile Policy, Add-on concepts, Profile Q deprecation |
| Interface Guide | `/onvif-pro:generate-interface-guide [product info]` | Generate conformance Interface Guide XML (DocBook v5.x) |
| SOAP templates | `/onvif-pro:soap-template [service/operation]` | Ready-to-use ONVIF SOAP request templates |

---

## Usage Examples

```bash
# Recommend profiles for a 4K H.265 PTZ IP camera
/onvif-pro:check-profile IP camera H.265 PTZ 4K

# Diagnose camera-NVR connection issues at the protocol level
/onvif-pro:diagnose 3 out of 15 cameras not connecting to NVR, SOAP 401 error

# Look up Media2 service spec (with namespace and endpoint pattern)
/onvif-pro:spec-lookup Media2

# Look up a specific SOAP operation
/onvif-pro:spec-lookup GetStreamUri

# Design a 50-channel integrated security system
/onvif-pro:design-system factory 50-channel CCTV + 20-door access control + biometrics + AI analytics

# Profile T conformance review with Interface Guide requirements
/onvif-pro:conformance-check Profile T camera development

# Understand Add-on concept / Profile Q deprecation / Policy docs
/onvif-pro:profile-policy what is an add-on
/onvif-pro:profile-policy tls add-on v2.0

# Generate Interface Guide XML for conformance submission
/onvif-pro:generate-interface-guide Axis P3245-V Profile T+G firmware 10.12

# Generate SOAP template for locking a door
/onvif-pro:soap-template LockDoor

# Generate SOAP template for RTSP stream setup
/onvif-pro:soap-template GetStreamUri Profile T
```

---

## Local Development & Testing

```bash
# Clone the repo
git clone https://github.com/shin0343/onvif-professional-plugin.git

# Load plugin locally without installing
claude --plugin-dir ./onvif-professional-plugin

# After editing, reload without restarting
/reload-plugins
```

---

## Supported ONVIF Specification Scope

### Profiles

| Profile | Target | Key WSDL Services | Protocols |
|---------|--------|-------------------|-----------|
| **Profile S** | IP video streaming | `media.wsdl`, `event.wsdl` | SOAP + **RTSP**/RTP |
| **Profile T** | Advanced video (H.265, HTTPS) | `media2.wsdl`, `event.wsdl` | SOAP + **RTSP**/RTP |
| **Profile G** | Edge storage & recording | `recording.wsdl`, `search.wsdl`, `replay.wsdl` | SOAP + **RTSP** (playback) |
| **Profile C** | Door control | `accesscontrol.wsdl`, `doorcontrol.wsdl` | SOAP only |
| **Profile A** | Access control config | `accessrules.wsdl`, `credential.wsdl`, `schedule.wsdl` | SOAP only |
| **Profile D** | Access control peripherals | `credential.wsdl` | SOAP only |
| **Profile M** | Analytics & metadata | `analytics.wsdl`, `event.wsdl` | SOAP + **RTSP** (metadata) + MQTT |
| ~~**Profile Q**~~ | *(Deprecated April 1, 2022)* | Superseded by Profile S/T | — |

### Add-ons

| Add-on | Description | Version |
|--------|-------------|---------|
| **TLS Configuration Add-on** | Encrypted TLS setup via `advancedsecurity.wsdl` | v1.0 (deadline: March 31, 2027) / v2.0 (planned 2027) |

### Network Interface Specifications (30+ services)

Core (`device.wsdl`), Media / Media2, PTZ, Imaging, Streaming (RTSP/RTP/metadatastream.xsd), Analytics, Recording Control, Recording Search, Replay, Access Control, Door Control, Access Rules, Credential, Authentication Behavior, Schedule, Thermal, Device IO, Security (Advanced Security / TLS Add-on), Cloud Integration, Uplink, Application Management, Resource Query, Action Engine, Display, WebRTC, and more.

### Conformance Process

- **Conformance Process Spec**: v5.7 (April 2026) — https://www.onvif.org/profiles/conformance/
- **Interface Guide Spec**: v1.1.2 (April 2023, DocBook v5.x XML) — mandatory for DoC submission
- **Test tools**: Device Test Tool + Client Test Tool (member login required)
- **Submission**: DoC + Interface Guide + Feature List via https://members.onvif.org/

---

## Directory Structure

```
onvif-professional-plugin/
├── .claude-plugin/
│   ├── plugin.json              # Plugin manifest (name, version, metadata)
│   └── marketplace.json         # Marketplace registry (enables /plugin install)
├── agents/
│   └── onvif-expert.md          # ONVIF expert main agent (claude-opus-4-7)
├── skills/
│   ├── check-profile/
│   │   └── SKILL.md             # Profile analysis, recommendation, WSDL mapping
│   ├── diagnose/
│   │   └── SKILL.md             # Protocol-layer diagnostics (HTTP/SOAP/RTSP)
│   ├── spec-lookup/
│   │   └── SKILL.md             # WSDL/spec lookup, namespaces, endpoint patterns
│   ├── design-system/
│   │   └── SKILL.md             # System architecture, network design, protocol flow
│   ├── conformance-check/
│   │   └── SKILL.md             # Conformance checklist (Process v5.7, Interface Guide v1.1.2)
│   ├── profile-policy/
│   │   └── SKILL.md             # Profile Policy & Add-on concept guidance
│   ├── generate-interface-guide/
│   │   └── SKILL.md             # Generate DocBook v5.x XML Interface Guide
│   └── soap-template/
│       └── SKILL.md             # Generate ONVIF SOAP request templates
└── hooks/
    └── hooks.json               # ONVIF keyword detection + protocol stack tip
```

---

## Official ONVIF Resources

- Profiles & Add-ons overview: https://www.onvif.org/profiles-add-ons-specifications/
- Specifications index: https://www.onvif.org/profiles/specifications/
- Conformant products database: https://www.onvif.org/conformant-products/
- Conformance process: https://www.onvif.org/profiles/conformance/
- Interface Guide creation: https://www.onvif.org/profiles/conformance/interface-guide/interface-guide-creation/
- GitHub (specifications): https://github.com/onvif/specs
- TLS Add-on: https://www.onvif.org/profiles/add-on/tls-configuration-add-on/
- Profile Policy v3.5: https://www.onvif.org/wp-content/uploads/2024/10/onvif-profile-policy-v3-5.pdf
- Profile Feature Overview v2.6: https://www.onvif.org/wp-content/uploads/2022/04/onvif-profile-feature-overview.pdf
- Interface Guide Spec v1.1.2: https://www.onvif.org/wp-content/uploads/2023/04/ONVIF-Interface-Guide-Specification-v1-1-2.pdf

---

## License

MIT
