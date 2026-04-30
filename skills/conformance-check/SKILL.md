---
name: conformance-check
description: Reviews ONVIF conformance requirements for a specific device or system. Examples: /onvif-pro:conformance-check Profile T camera, /onvif-pro:conformance-check access control system Profile C+A
---

# ONVIF Conformance Check Skill

Provides a checklist of ONVIF conformance requirements for the device or system specified in `$ARGUMENTS`.

## Base Conformance Requirements (All Profiles)

To claim ONVIF conformance, a product must:
1. Be an ONVIF member organization
2. Support at least one ONVIF profile
3. Be registered in the ONVIF conformant products database
4. Specify the exact firmware/software version

## Per-Profile Checklists

### Profile S Checklist
**Mandatory features:**
- [ ] WS-Discovery support (device discovery)
- [ ] GetCapabilities response
- [ ] GetProfiles and GetStreamUri implemented
- [ ] RTSP/RTP streaming (H.264 required)
- [ ] GetVideoEncoderConfigurations
- [ ] Basic Event service subscription

**Conditional features:**
- [ ] If PTZ supported → PTZ service implemented via ONVIF
- [ ] If audio supported → AudioEncoder configuration
- [ ] If multi-stream supported → multiple VideoSource configurations

### Profile T Checklist
**Additional mandatory (beyond Profile S):**
- [ ] H.265 (HEVC) video encoding
- [ ] Media2 service implemented
- [ ] HTTPS-based streaming (or HTTP)
- [ ] Metadata streaming configuration

**Conditional:**
- [ ] If two-way audio supported → Backchannel implemented
- [ ] If OSD supported → OSD configuration service

### Profile G Checklist
**Mandatory:**
- [ ] Recording Control service
- [ ] Recording Search service
- [ ] Replay Control service
- [ ] Schedule-based recording configuration

**Conditional:**
- [ ] If export supported → Export File Format specification followed

### Profile C Checklist
**Mandatory:**
- [ ] Door Control service
- [ ] Access Control service (basic)
- [ ] Event service (access events)
- [ ] Site information retrieval

### Profile A Checklist
**Mandatory:**
- [ ] Access Rules service
- [ ] Credential service
- [ ] Schedule service
- [ ] Authentication behavior configuration

### Profile D Checklist
**Mandatory:**
- [ ] Credential identifier transmission
- [ ] Access request/response
- [ ] Lock/unlock action execution

**Conditional:**
- [ ] If biometrics supported → recognition method implemented via ONVIF

### Profile M Checklist
**Mandatory:**
- [ ] Analytics service
- [ ] Metadata streaming (compliant with metadatastream.xsd)
- [ ] Object classification (at least one: vehicle/human/license plate/face)
- [ ] Event interface

**Conditional:**
- [ ] If MQTT supported → MQTT broker configuration via ONVIF
- [ ] If geolocation supported → GeoLocation metadata included

## Output Format

```
## ONVIF Conformance Review: [Device/System]

### ✅ Requirements Met
- [Met items]

### ❌ Unmet or Unverified Requirements
- [Item]: [Remediation action]

### 📋 Test Tools
- Official ONVIF test tool downloads:
  - Device Test Tool: https://www.onvif.org/profiles/conformance/device-test-2/
  - Client Test Tool: https://www.onvif.org/profiles/conformance/client-test/

### 📝 Conformance Submission Steps
1. Pass all test tools
2. Prepare Declaration of Conformance (DoC)
3. Prepare Interface Guide
4. Generate Feature List file
5. Submit via Member Tools: https://www.onvif.org/member-tools/

### ⚠️ Important Notes
- TLS Add-on v1.0: conformance submission deadline is March 31, 2027
- Conformance is tied to a specific firmware/software version
- Search conformant products: https://www.onvif.org/conformant-products/
```
