---
name: check-profile
description: Analyzes a device type or set of requirements and recommends the optimal ONVIF profile and add-on combination, including required WSDL services and protocol details. Examples: /onvif-pro:check-profile IP camera H.265 PTZ, /onvif-pro:check-profile access control biometrics, /onvif-pro:check-profile edge AI analytics MQTT
---

# ONVIF Profile Check Skill

Analyzes the device type or requirements in `$ARGUMENTS` and recommends the optimal combination of ONVIF profiles and add-ons, with the specific WSDL services and protocols each profile requires.

---

## ONVIF Protocol Stack (Context)

ONVIF is **HTTP 기반**:
- **Control**: All profile feature calls use **SOAP/WSDL over HTTP** (`POST /onvif/<service>`)
- **Streaming**: AV streams delivered via **RTSP** (obtained via `GetStreamUri` SOAP call)
- **Discovery**: WS-Discovery (SOAP over UDP multicast 239.255.255.250:3702)

Profile selection determines which WSDL services must be implemented and which protocols are exercised.

---

## Analysis Procedure

### Step 1: Classify the Device / Requirements

Extract from `$ARGUMENTS`:
- **Device type**: camera, NVR/VMS client, access control panel, card reader, biometric device, analytics server, hybrid device
- **Key functions**: video streaming, edge recording, access control, analytics, secure communication
- **Technical requirements**: H.265, PTZ, two-way audio, biometrics, MQTT, TLS, cloud integration, AI analytics

### Step 2: Profile Mapping Logic

**Video / Streaming — Device side (camera):**
| Requirement | Profile | Mandatory WSDL Services |
|-------------|---------|------------------------|
| Basic IP camera streaming | **Profile S** | `device.wsdl`, `media.wsdl`, `event.wsdl` |
| H.265, HTTPS streaming, advanced features | **Profile T** | `device.wsdl`, `media2.wsdl`, `event.wsdl` |
| Built-in SD card / edge recording | + **Profile G** | + `recording.wsdl`, `search.wsdl`, `replay.wsdl` |
| AI analytics metadata streaming | + **Profile M** | + `analytics.wsdl`, `event.wsdl` (MQTT optional) |

**Video / Streaming — Client side (NVR / VMS):**
- Must implement matching profiles to the cameras it connects to
- Profile S client: `media.wsdl` consumer
- Profile T client: `media2.wsdl` consumer (RTSP with H.265)
- Profile G client: `search.wsdl` + `replay.wsdl` consumer

**Access Control — Device side:**
| Requirement | Profile | Mandatory WSDL Services |
|-------------|---------|------------------------|
| Door lock/unlock control panel | **Profile C** | `device.wsdl`, `accesscontrol.wsdl`, `doorcontrol.wsdl`, `event.wsdl` |
| Credential / schedule management | + **Profile A** | + `accessrules.wsdl`, `credential.wsdl`, `schedule.wsdl`, `authenticationbehavior.wsdl` |
| Card / biometric readers | **Profile D** | `device.wsdl`, `credential.wsdl` (identifier subset) |

**Secure Communications:**
| Requirement | Add-on | Required WSDL |
|-------------|--------|---------------|
| Standardized TLS setup & cert management | **TLS Configuration Add-on** | `advancedsecurity.wsdl` |
| Must pair with at least one profile | (cannot be standalone) | |

**Profile Q is deprecated** (April 1, 2022) — do not recommend for new products.

### Step 3: Key Protocol Considerations Per Profile

**Profile S/T:**
- RTSP stream URI obtained via SOAP `GetStreamUri` — URI changes per profile token
- Authentication: HTTP Digest (RTSP) + WS-Security UsernameToken (SOAP)
- Profile T: Media2 service replaces Media for advanced features; H.265 requires Media2

**Profile G:**
- Playback via RTSP with Range header (time-based seek)
- `FindRecordings` (SOAP) → get recording tokens → `GetReplayUri` → RTSP playback

**Profile C/A/D:**
- All door/credential operations are SOAP — no RTSP involved
- Events delivered via WS-BaseNotification pull-point (SOAP `PullMessages`)

**Profile M:**
- Analytics metadata rides inside RTSP RTP stream (metadatastream.xsd payload)
- MQTT as an alternative event transport (configure via `event.wsdl`)

### Step 4: Output Format

```
## ONVIF Profile Analysis Result

**Input device/requirements:** [summary]

### ✅ Required Profiles
| Profile | Reason | Key WSDL Services | Protocol |
|---------|--------|-------------------|---------|
| Profile X | ... | service.wsdl, ... | SOAP + RTSP |

### 📦 Recommended Additional Profiles / Add-ons
| Profile/Add-on | Condition | Additional WSDL Services |
|----------------|-----------|--------------------------|
| Profile Y | Add if [condition] | ... |

### 🔌 Protocol Summary
- **Control protocol**: SOAP 1.2 over HTTP (POST to ONVIF service endpoints)
- **Streaming protocol**: RTSP (URI from GetStreamUri), RTP/RTCP for media
- **Discovery**: WS-Discovery multicast UDP 239.255.255.250:3702
- **Events**: WS-BaseNotification (pull) or MQTT (Profile M)
- **Authentication**: WS-Security UsernameToken (SOAP) + HTTP Digest (RTSP)

### ⚠️ Notes
- Conditional features: [list]
- Profile T note: Media2 service is mandatory — legacy Media service optional for backward compat
- Conformance verification: https://www.onvif.org/conformant-products/

### 🏗️ Example System Configuration
[How devices connect and which profiles each device implements]

### 📌 Next Steps
- Verify manufacturer conformance: https://www.onvif.org/conformant-products/
- Use `/onvif-pro:conformance-check [profile]` for detailed checklist
- Use `/onvif-pro:design-system [requirements]` for full system architecture
```

### Step 5: Add Practical Advice
- Real-world integration considerations for the recommended profile combination
- Protocol-level interoperability risks (e.g., Media vs Media2 client support)
- How to verify manufacturer compatibility via onvif.org/conformant-products/
- Test approach: ONVIF Device Test Tool for device-side; Client Test Tool for VMS/NVR
