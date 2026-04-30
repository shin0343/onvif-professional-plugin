# ONVIF Professional Plugin for Claude Code

> Expert plugin for ONVIF standards in IP-based physical security systems (CCTV / NVR / Access Control)

## Overview

Provides Claude with deep expertise across all ONVIF profiles (S/T/G/C/A/D/M), network interface specifications, add-ons, and the conformance process for IP-based physical security systems.

The agent responds **in the same language you write in** — English or Korean.

## Installation & Testing

```bash
# Load plugin locally for testing
claude --plugin-dir ./onvif-professional-plugin

# Verify agent is active
/agents

# List available skills
/help
```

## Directory Structure

```
onvif-professional-plugin/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── agents/
│   └── onvif-expert.md          # ONVIF expert main agent
├── skills/
│   ├── check-profile/
│   │   └── SKILL.md             # Profile analysis & recommendation
│   ├── diagnose/
│   │   └── SKILL.md             # Compatibility diagnostics
│   ├── spec-lookup/
│   │   └── SKILL.md             # WSDL/spec document lookup (verified URLs)
│   ├── design-system/
│   │   └── SKILL.md             # System architecture design
│   ├── conformance-check/
│   │   └── SKILL.md             # Conformance validation checklist
│   └── profile-policy/
│       └── SKILL.md             # Profile Policy & Add-on concept guidance
├── hooks/
│   └── hooks.json               # ONVIF keyword detection hooks
├── settings.json                # Default agent settings
└── onvif-spec-summary.md        # ONVIF specification reference summary
```

## Available Skills

| Skill | Command | Description |
|-------|---------|-------------|
| Profile analysis | `/onvif-pro:check-profile [device/requirements]` | Recommend profiles for your device type |
| Diagnostics | `/onvif-pro:diagnose [symptoms]` | Diagnose compatibility and communication issues |
| Spec lookup | `/onvif-pro:spec-lookup [service name]` | WSDL/XSD/spec details with verified URLs |
| System design | `/onvif-pro:design-system [requirements]` | System architecture design |
| Conformance check | `/onvif-pro:conformance-check [profile]` | Conformance requirements checklist |
| Policy / Add-on | `/onvif-pro:profile-policy [topic]` | Profile Policy, Add-on concepts, Profile Q deprecation |

## Usage Examples

```bash
# Recommend profiles for a 4K H.265 PTZ IP camera
/onvif-pro:check-profile IP camera H.265 PTZ 4K

# Diagnose camera-NVR connection issues
/onvif-pro:diagnose 3 out of 15 cameras not connecting to NVR

# Look up Analytics service spec (verified URLs provided)
/onvif-pro:spec-lookup Analytics

# Design a 50-channel integrated security system
/onvif-pro:design-system factory 50-channel CCTV + 20-door access control + biometrics + AI analytics

# Profile T conformance review
/onvif-pro:conformance-check Profile T camera development review

# Understand Add-on concept / Profile Q deprecation / Policy docs
/onvif-pro:profile-policy what is an add-on
/onvif-pro:profile-policy profile-q deprecation reason
```

## Supported ONVIF Specification Scope

### Profiles
- **Profile S** — IP video streaming (H.264, RTSP, PTZ)
- **Profile T** — Advanced video streaming (H.265, HTTPS, two-way audio)
- **Profile G** — Edge storage and recording retrieval
- **Profile C** — Access control door management and events
- **Profile A** — Access control credentials/schedules/rules management
- **Profile D** — Access control peripherals (card readers, biometrics)
- **Profile M** — Analytics metadata and events (AI object classification)
- ~~**Profile Q**~~ — Deprecated April 1, 2022 (no new conformance claims accepted)

### Add-ons
- **TLS Configuration Add-on** — Encrypted communication setup (v1.0 / v2.0)

### Network Interface Specifications (30+ services)
Core, Media, Media2, PTZ, Imaging, Streaming, Analytics, Recording Control, Recording Search, Replay, Access Control, Door Control, Access Rules, Credential, Schedule, Authentication Behavior, Thermal, Device IO, Security, Cloud Integration, Uplink, Application Management, Resource Query, Action Engine, Display, WebRTC, and more.

## Official ONVIF Resources

- Profiles overview: https://www.onvif.org/profiles-add-ons-specifications/
- Conformant products: https://www.onvif.org/conformant-products/
- Conformance process: https://www.onvif.org/profiles/conformance/
- GitHub (specifications): https://github.com/onvif/specs

## License

MIT
