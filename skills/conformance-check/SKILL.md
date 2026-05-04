---
name: conformance-check
description: Reviews ONVIF conformance requirements for a specific device or system, covering per-profile mandatory/conditional checklists, Interface Guide preparation (DocBook v5.x XML), and the full submission process per Conformance Process Spec v5.7. Examples: /onvif-pro:conformance-check Profile T camera, /onvif-pro:conformance-check access control system Profile C+A
---

# ONVIF Conformance Check Skill

Provides a complete conformance checklist for the device or system specified in `$ARGUMENTS`, based on the ONVIF Conformance Process Specification v5.7 (April 2026).

**Reference:** https://www.onvif.org/profiles/conformance/

---

## Base Conformance Requirements (All Profiles)

To claim ONVIF conformance, a product must:
1. ✅ Be from an **ONVIF member organization**
2. ✅ Support at least **one ONVIF profile** (or approved add-on paired with a profile)
3. ✅ Be registered in the **ONVIF conformant products database**
4. ✅ Specify the exact **firmware/software version** (conformance is version-specific, valid indefinitely for that version)
5. ✅ Pass the **ONVIF Device Test Tool** and/or **Client Test Tool**
6. ✅ Submit **three artefacts** via Member Tools (https://members.onvif.org/):
   - Declaration of Conformance (DoC)
   - ONVIF Interface Guide (XML, DocBook v5.x)
   - Feature List file

---

## Per-Profile Checklists

### Profile S — Video Streaming
**Mandatory features:**
- [ ] WS-Discovery support (UDP multicast to 239.255.255.250:3702)
- [ ] `GetCapabilities` response includes Media service endpoint
- [ ] `GetProfiles` implemented (media.wsdl)
- [ ] `GetStreamUri` implemented → returns valid RTSP URI
- [ ] RTSP/RTP streaming active on returned URI (port 554 typical)
- [ ] H.264 video encoding mandatory (`GetVideoEncoderConfigurations`)
- [ ] HTTP Digest or WS-Security UsernameToken authentication
- [ ] Basic Event service subscription (event.wsdl, WS-BaseNotification)

**Conditional features:**
- [ ] If PTZ supported → full PTZ service (ptz.wsdl) exposed via ONVIF
- [ ] If audio supported → AudioEncoder configuration via media.wsdl
- [ ] If multi-stream → multiple VideoSource/VideoEncoder configurations
- [ ] If multicast → `GetMulticastStreamUri` implemented

### Profile T — Advanced Video Streaming
**Additional mandatory (beyond Profile S / replaces some S requirements):**
- [ ] **Media2 service** (media2.wsdl) mandatory — `GetProfiles`, `GetStreamUri` via Media2
- [ ] **H.265 (HEVC)** video encoding supported
- [ ] Metadata streaming configured (metadatastream.xsd compliant)
- [ ] HTTPS endpoint available for streaming (TLS not required, but HTTPS URI must be obtainable)
- [ ] `GetVideoEncoderConfigurations` via Media2

**Conditional:**
- [ ] If two-way audio → Backchannel (audio back-channel via RTSP) implemented
- [ ] If OSD supported → OSD configuration service (Media2)
- [ ] If radiometry (thermal) → radiometry.xsd metadata supported

### Profile G — Edge Storage and Retrieval
**Mandatory:**
- [ ] Recording Control service (recording.wsdl) — create/manage recordings
- [ ] Recording Search service (search.wsdl) — `FindRecordings`, `GetRecordingInformation`
- [ ] Replay Control service (replay.wsdl) — `GetReplayUri` → valid RTSP playback URI
- [ ] Schedule service (schedule.wsdl) — schedule-based recording configuration
- [ ] `GetRecordings` returns existing recording tokens

**Conditional:**
- [ ] If export supported → Export File Format Spec (ONVIF-ExportFileFormat-Spec.pdf) followed
- [ ] If receiver supported → receiver.wsdl implemented

### Profile C — Door Control
**Mandatory:**
- [ ] Access Control service (accesscontrol.wsdl) — `GetAccessPoints`, access point status
- [ ] Door Control service (doorcontrol.wsdl) — `AccessDoor`, `LockDoor`, `UnlockDoor`, `DoubleLockDoor`
- [ ] Event service (event.wsdl) — access events (AccessGranted, AccessDenied, DoorLocked, etc.)
- [ ] Site information retrieval — `GetAreaInfoList`

**Conditional:**
- [ ] If tamper detection → tamper events via event.wsdl

### Profile A — Access Control Configuration
**Mandatory:**
- [ ] Access Rules service (accessrules.wsdl) — `CreateAccessProfile`, `GetAccessProfileList`
- [ ] Credential service (credential.wsdl) — `CreateCredential`, `GetCredentialList`, `EnableCredential`, `DisableCredential`
- [ ] Schedule service (schedule.wsdl) — `CreateSchedule`, `GetScheduleList`
- [ ] Authentication Behavior service (authenticationbehavior.wsdl)
- [ ] Identity/entity queries returning valid structures

### Profile D — Access Control Peripherals
**Mandatory:**
- [ ] Credential identifier transmission (credential.wsdl subset)
- [ ] Access request submission (AccessRequest message)
- [ ] Lock/unlock action execution based on received access decision

**Conditional:**
- [ ] If biometrics supported → biometric recognition method type exposed via ONVIF
- [ ] If mobile credential → mobile credential terminal behavior implemented

### Profile M — Metadata & Analytics
**Mandatory:**
- [ ] Analytics service (analytics.wsdl) — `GetSupportedRules`, `CreateRule`, `GetAnalyticsModules`
- [ ] Metadata streaming via RTSP (metadatastream.xsd compliant payload)
- [ ] Object classification (at least one: Vehicle / Human / License Plate / Face)
- [ ] Event interface (event.wsdl) — analytics events delivered

**Conditional:**
- [ ] If MQTT → MQTT broker configuration via event.wsdl; events published to MQTT topics
- [ ] If geolocation → GeoLocation metadata included in metadatastream
- [ ] If facial recognition → humanface.xsd metadata structures
- [ ] If vehicle analytics → license plate/vehicle class metadata

---

## Interface Guide Requirements (v1.1.2, April 2023)

**Specification:** https://www.onvif.org/wp-content/uploads/2023/04/ONVIF-Interface-Guide-Specification-v1-1-2.pdf
**Format:** XML conforming to **DocBook v5.x** standard
**Template:** Available in ONVIF Member Portal (https://members.onvif.org/) under "Resources"

### Mandatory Sections
- [ ] **Overview** — Standard template text + Product type (device/client/both)
- [ ] **Product Information** — Product Name AND Version Number (must exactly match DoC and Feature List)
  - [ ] For product families: ALL product names listed (as in DoC)
- [ ] **Supported ONVIF Profiles** — Explicit list of all claimed profiles
- [ ] **Support Information** — Company name, address (street, city, country), technical support URL
- [ ] **Prerequisites** — Hardware/OS requirements to interact with the product
- [ ] **Installation** — Power source, network connection, software installation steps
- [ ] **Default Network Settings** — How to obtain IP address; DHCP behavior; default IP (device: mandatory)
- [ ] **Default Login** — Default username/password, default access URL (device: mandatory)
- [ ] **Local Configuration** — Navigation to settings/configuration page
- [ ] **Enabling ONVIF** — Whether ONVIF is enabled by default; how to enable if not
- [ ] **Querying Capabilities** — How to query capabilities from client (GetCapabilities / GetServices); discovery procedure

### Optional Sections
- [ ] **Remote Configuration** — How to configure media stream, recording, access point via client
- [ ] Company Logo

> **Note:** As of April 19, 2023, only Interface Guide Spec v1.1.2 is accepted for DoC submissions.

---

## Output Format

```
## ONVIF Conformance Review: [Device/System]

### ✅ Requirements Met
- [Item]: [Evidence / how it is satisfied]

### ❌ Unmet or Unverified Requirements
- [Item]: [Remediation action required]

### 📋 Mandatory Test Tools
- Device Test Tool: https://www.onvif.org/profiles/conformance/device-test-2/
- Client Test Tool: https://www.onvif.org/profiles/conformance/client-test/
- Device Test Specs: https://www.onvif.org/profiles/conformance/device-test/
- Client Test Specs: https://www.onvif.org/profiles/conformance/client-test/
- (Tool download: https://members.onvif.org/ — member login required)

### 📝 Conformance Submission Checklist
1. [ ] Pass all ONVIF Test Tool routines for claimed profile(s)
2. [ ] Prepare Declaration of Conformance (DoC)
3. [ ] Prepare Interface Guide (XML, DocBook v5.x, per Spec v1.1.2)
4. [ ] Generate Feature List file
5. [ ] Submit all three artefacts via Member Tools: https://members.onvif.org/

### ⚠️ Important Notes
- Conformance Process Spec: **v5.7 (April 2026)**
- Interface Guide Spec: **v1.1.2 (April 2023)** — only this version accepted
- Conformance is tied to a specific firmware/software version
- TLS Add-on v1.0: conformance submission deadline March 31, 2027
- Search conformant products: https://www.onvif.org/conformant-products/
- Report improper conformance claims: https://www.onvif.org/profiles/conformance/
```
