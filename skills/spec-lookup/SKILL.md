---
name: spec-lookup
description: Retrieves detailed technical information about a specific ONVIF service, WSDL, XSD schema, or specification document, including protocol-level details (SOAP envelope, HTTP endpoint, RTSP integration). Examples: /onvif-pro:spec-lookup Media2, /onvif-pro:spec-lookup analytics.wsdl, /onvif-pro:spec-lookup PTZ, /onvif-pro:spec-lookup GetStreamUri
---

# ONVIF Spec Lookup Skill

Provides detailed technical information about the ONVIF service or specification given in `$ARGUMENTS`.

**Primary source:** https://www.onvif.org/profiles/specifications/
**GitHub specs:** https://github.com/onvif/specs

---

## ONVIF Protocol Stack Reference

All ONVIF services use **SOAP 1.2 over HTTP/HTTPS**. Every operation follows this pattern:

```
HTTP POST http://<device-ip>/<service-path> HTTP/1.1
Content-Type: application/soap+xml; charset=utf-8
SOAPAction: "<namespace>/<operation>"

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope">
  <s:Header>
    <!-- WS-Security (UsernameToken with SHA-1 digest) when required -->
  </s:Header>
  <s:Body>
    <!-- WSDL-defined request element -->
  </s:Body>
</s:Envelope>
```

**Endpoint discovery:** Always call `GetCapabilities` or `GetServices` first — never hard-code service URIs.

**RTSP streaming** is separate from SOAP control: `GetStreamUri` (SOAP) returns an `rtsp://` URI; the client then speaks RTSP directly to that URI.

---

## Official Specification Database (Verified URLs)

### Core Services

**ONVIF Core Specification**
- Spec: https://www.onvif.org/specs/core/ONVIF-Core-Specification.pdf
- WSDL: `device.wsdl` → https://www.onvif.org/ver10/device/wsdl/devicemgmt.wsdl
- XSD: `onvif.xsd` → https://www.onvif.org/ver10/schema/onvif.xsd
- XSD: `common.xsd` → https://www.onvif.org/ver10/schema/common.xsd
- YAML: `storagerenewal.yaml` → https://www.onvif.org/yaml/viewer.php?yaml=storagerenewal.yaml
- Namespace: `http://www.onvif.org/ver10/device/wsdl`
- Typical endpoint: `http://<ip>/onvif/device_service`
- Key operations: `GetCapabilities`, `GetServices`, `GetDeviceInformation`, `GetNetworkInterfaces`, `GetUsers`, `SetNetworkInterfaces`, `SystemReboot`, `GetSystemDateAndTime`

**Event Service**
- WSDL: `event.wsdl` → https://www.onvif.org/ver10/events/wsdl/event.wsdl
- Namespace: `http://www.onvif.org/ver10/events/wsdl`
- Typical endpoint: `http://<ip>/onvif/event_service`
- Key operations: `Subscribe` (WS-BaseNotification pull-point), `CreatePullPointSubscription`, `PullMessages`
- MQTT: Broker endpoint configured via event service; topics follow `onvif/device/<uuid>/...` pattern

---

### Streaming / Data Format

**Streaming Specification**
- Spec: https://www.onvif.org/specs/stream/ONVIF-Streaming-Spec.pdf
- XSD: `metadatastream.xsd` → https://www.onvif.org/ver10/schema/metadatastream.xsd
- Covers: RTSP/RTP/RTCP session setup, RTP payload formats (H.264, H.265, JPEG, G.711, G.726, AAC), metadata RTP payload
- RTSP port: 554 (standard); alternate ports via `GetStreamUri` response

**Media Signing Specification**
- Spec: https://www.onvif.org/specs/stream/ONVIF-MediaSigning-Spec.pdf

**Export File Format Specification** (Profile G)
- Spec: https://www.onvif.org/specs/stream/ONVIF-ExportFileFormat-Spec.pdf

**WebRTC Specification**
- Spec: https://www.onvif.org/specs/stream/ONVIF-WebRTC-Spec.pdf
- Provides browser-accessible streaming via WebRTC (alternative to RTSP)

---

### Media Services (Profile S / T)

**Media Service** (Profile S)
- Spec: https://www.onvif.org/specs/srv/media/ONVIF-Media-Service-Spec.pdf
- WSDL: `media.wsdl` → https://www.onvif.org/ver10/media/wsdl/media.wsdl
- Namespace: `http://www.onvif.org/ver10/media/wsdl`
- Typical endpoint: `http://<ip>/onvif/media_service`
- Key operations: `GetProfiles`, `GetStreamUri` (→ RTSP URI), `GetVideoEncoderConfigurations`, `SetVideoEncoderConfiguration`, `GetSnapshotUri`, `GetAudioEncoderConfigurations`, `GetVideoSources`
- RTSP URI pattern: `rtsp://<ip>:554/onvif/stream1`

**Media2 Service** (Profile T — mandatory)
- Spec: https://www.onvif.org/specs/srv/media/ONVIF-Media2-Service-Spec.pdf
- WSDL: `media2.wsdl` → https://www.onvif.org/ver20/media/wsdl/media.wsdl
- Namespace: `http://www.onvif.org/ver20/media/wsdl`
- Typical endpoint: `http://<ip>/onvif/media2_service` or `/onvif/Media2`
- Key improvements over Media: HTTPS streaming URI, H.265 configuration, metadata streaming configuration, OSD overlay management
- Key operations: `GetProfiles`, `GetStreamUri`, `GetVideoEncoderConfigurations`, `GetMetadataConfigurations`, `CreateOSD`, `SetOSD`

**Imaging Service**
- Spec: https://www.onvif.org/specs/srv/img/ONVIF-Imaging-Service-Spec.pdf
- WSDL: `imaging.wsdl` → https://www.onvif.org/ver20/imaging/wsdl/imaging.wsdl
- Namespace: `http://www.onvif.org/ver20/imaging/wsdl`
- Key operations: `GetImagingSettings`, `SetImagingSettings`, `GetOptions` (brightness, contrast, WB, IR, exposure)

**PTZ Service**
- Spec: https://www.onvif.org/specs/srv/ptz/ONVIF-PTZ-Service-Spec.pdf
- WSDL: `ptz.wsdl` → https://www.onvif.org/ver20/ptz/wsdl/ptz.wsdl
- Namespace: `http://www.onvif.org/ver20/ptz/wsdl`
- Typical endpoint: `http://<ip>/onvif/ptz_service`
- Key operations: `ContinuousMove`, `AbsoluteMove`, `RelativeMove`, `Stop`, `GetPresets`, `GotoPreset`, `SetPreset`, `GetConfigurationOptions`
- Move spaces: PanTiltSpace (normalized -1..1), ZoomSpace (normalized 0..1)

---

### Recording Services (Profile G)

**Recording Control Service**
- Spec: https://www.onvif.org/specs/srv/rec/ONVIF-RecordingControl-Service-Spec.pdf
- WSDL: `recording.wsdl` → https://www.onvif.org/ver10/recording.wsdl
- Typical endpoint: `http://<ip>/onvif/recording_service`
- Key operations: `CreateRecording`, `CreateTrack`, `CreateRecordingJob`, `GetRecordings`, `DeleteRecording`

**Recording Search Service**
- Spec: https://www.onvif.org/specs/srv/rsrch/ONVIF-RecordingSearch-Service-Spec.pdf
- WSDL: `search.wsdl` → https://www.onvif.org/ver10/search.wsdl
- Key operations: `FindRecordings`, `GetRecordingInformation`, `FindEvents`, `FindMetadata`, `GetSearchState`, `EndSearch`

**Replay Control Service**
- Spec: https://www.onvif.org/specs/srv/replay/ONVIF-ReplayControl-Service-Spec.pdf
- WSDL: `replay.wsdl` → https://www.onvif.org/ver10/replay.wsdl
- Key operations: `GetReplayUri` (→ RTSP playback URI with seek), `GetReplayConfiguration`

**Receiver Service**
- Spec: https://www.onvif.org/specs/srv/rcv/ONVIF-Receiver-Service-Spec.pdf
- WSDL: `receiver.wsdl` → https://www.onvif.org/ver10/receiver.wsdl

---

### Access Control Services (Profile C / A / D)

**Access Control Service** (Profile C)
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-AccessControl-Service-Spec.pdf
- WSDL: `accesscontrol.wsdl` → https://www.onvif.org/ver10/pacs/accesscontrol.wsdl
- XSD: `types.xsd` → https://www.onvif.org/ver10/pacs/types.xsd
- Namespace: `http://www.onvif.org/ver10/accesscontrol/wsdl`
- Typical endpoint: `http://<ip>/onvif/accesscontrol_service`
- Key operations: `GetAccessPoints`, `GetAccessPointInfoList`, `GetAreaInfoList`

**Door Control Service** (Profile C)
- Spec: https://www.onvif.org/specs/srv/door/ONVIF-DoorControl-Service-Spec.pdf
- WSDL: `doorcontrol.wsdl` → https://www.onvif.org/ver10/pacs/doorcontrol.wsdl
- Namespace: `http://www.onvif.org/ver10/doorcontrol/wsdl`
- Typical endpoint: `http://<ip>/onvif/doorcontrol_service`
- Key operations: `AccessDoor`, `LockDoor`, `UnlockDoor`, `DoubleLockDoor`, `BlockDoor`, `LockDownDoor`, `LockOpenDoor`, `GetDoorInfoList`, `GetDoorState`

**Access Rules Service** (Profile A)
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-AccessRules-Service-Spec.pdf
- WSDL: `accessrules.wsdl` → https://www.onvif.org/ver10/accessrules/wsdl/accessrules.wsdl
- Key operations: `CreateAccessProfile`, `ModifyAccessProfile`, `GetAccessProfileList`, `DeleteAccessProfile`

**Credential Service** (Profile A / D)
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-Credential-Service-Spec.pdf
- WSDL: `credential.wsdl` → https://www.onvif.org/ver10/credential/wsdl/credential.wsdl
- Key operations: `CreateCredential`, `GetCredentialList`, `ModifyCredential`, `DeleteCredential`, `EnableCredential`, `DisableCredential`, `GetCredentialIdentifiers`
- Credential identifier types: Card (Wiegand, OSDP), PIN, Fingerprint, Face, Iris, QR Code

**Authentication Behavior Service**
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-AuthenticationBehavior-Service-Spec.pdf
- WSDL: `authenticationbehavior.wsdl` → https://www.onvif.org/ver10/authenticationbehavior/wsdl/authenticationbehavior.wsdl

**Schedule Service** (Profile A / C)
- Spec: https://www.onvif.org/specs/srv/sched/ONVIF-Scheduler-Service-Spec.pdf
- WSDL: `schedule.wsdl` → https://www.onvif.org/ver10/schedule/wsdl/schedule.wsdl
- Key operations: `CreateSchedule`, `GetScheduleList`, `ModifySchedule`, `DeleteSchedule`, `CreateSpecialDays`, `GetSpecialDayGroupList`

---

### Analytics Service (Profile M)

**Analytics Service**
- Spec: https://www.onvif.org/specs/srv/analytics/ONVIF-Analytics-Service-Spec.pdf
- WSDL: `analytics.wsdl` → https://www.onvif.org/ver20/analytics/wsdl/analytics.wsdl
- Namespace: `http://www.onvif.org/ver20/analytics/wsdl`
- Typical endpoint: `http://<ip>/onvif/analytics_service`
- XSD: `rules.xsd` → https://www.onvif.org/ver20/analytics/rules.xsd
- XSD: `humanbody.xsd` → https://www.onvif.org/ver20/analytics/humanbody.xsd
- XSD: `humanface.xsd` → https://www.onvif.org/ver20/analytics/humanface.xsd
- Key operations: `GetSupportedRules`, `CreateRule`, `GetRules`, `ModifyRule`, `DeleteRule`, `GetAnalyticsModules`, `CreateAnalyticsModules`
- Metadata stream: analytics objects embedded in RTSP RTP payload per metadatastream.xsd

---

### Security Services

**Security Service (Advanced Security)**
- Spec: https://www.onvif.org/specs/srv/security/ONVIF-Security-Service-Spec.pdf
- WSDL: `advancedsecurity.wsdl` → https://www.onvif.org/ver10/advancedsecurity/wsdl/advancedsecurity.wsdl
- Used by: TLS Configuration Add-on
- Key operations: TLS configuration, certificate management (upload, delete, get), CRL management

**Security Baseline Specification**
- Spec: https://www.onvif.org/specs/srv/security/ONVIF-SecurityBaseline-Spec.pdf
- Covers: minimum security requirements for all ONVIF devices

---

### Other Services

**Thermal Service**
- Spec: https://www.onvif.org/specs/srv/thermal/ONVIF-Thermal-Service-Spec.pdf
- WSDL: `thermal.wsdl` → https://www.onvif.org/ver10/thermal/wsdl/thermal.wsdl
- XSD: `radiometry.xsd` → https://www.onvif.org/ver20/analytics/radiometry.xsd
- Key operations: `GetConfiguration`, `SetConfiguration`, `GetConfigurationOptions`

**Device IO Service**
- Spec: https://www.onvif.org/specs/srv/io/ONVIF-DeviceIo-Service-Spec.pdf
- WSDL: `deviceio.wsdl` → https://www.onvif.org/ver10/deviceio.wsdl
- Covers: digital I/O (relay outputs, digital inputs), serial port, video outputs, audio outputs

**Display Service**
- Spec: https://www.onvif.org/specs/srv/disp/ONVIF-Display-Service-Spec.pdf
- WSDL: `display.wsdl` → https://www.onvif.org/ver10/display.wsdl

**Action Engine Service**
- Spec: https://www.onvif.org/specs/srv/act/ONVIF-ActionEngine-Service-Spec.pdf
- WSDL: `actionengine.wsdl` → https://www.onvif.org/ver10/actionengine.wsdl

**Application Management Service**
- Spec: https://www.onvif.org/specs/srv/appmgmt/ONVIF-ApplicationManagement-Service-Spec.pdf
- WSDL: `appmgmt.wsdl` → https://www.onvif.org/ver10/appmgmt/wsdl/appmgmt.wsdl
- Covers: on-device app lifecycle (install, start, stop, remove) — relevant for edge AI apps

**Cloud Integration Service**
- Spec: https://www.onvif.org/specs/srv/cloudint/ONVIF-CloudIntegration-Service-Spec.pdf
- YAML: `cloudintegration.yaml` → https://www.onvif.org/yaml/viewer.php?yaml=cloudintegration.yaml

**Uplink Service**
- Spec: https://www.onvif.org/specs/srv/uplink/ONVIF-Uplink-Spec.pdf
- WSDL: `uplink.wsdl` → https://www.onvif.org/ver10/uplink/wsdl/uplink.wsdl
- Covers: remote management channel — device connects out to cloud/VMS (reverse tunnel)

**Provisioning Service**
- Spec: https://www.onvif.org/specs/srv/ptz/ONVIF-Provisioning-Service-Spec.pdf
- WSDL: `provisioning.wsdl` → https://www.onvif.org/ver10/provisioning/wsdl/provisioning.wsdl

**Resource Query Service**
- Spec: https://www.onvif.org/specs/srv/res/ONVIF-ResourceQuery-Spec.pdf
- REST/YAML based — no WSDL; query-only service for device resource enumeration

---

## Lookup Procedure

1. Identify the service name, WSDL name, operation name, or keyword from `$ARGUMENTS`
2. Find the matching entry in the database above
3. If `$ARGUMENTS` names a specific SOAP operation (e.g., `GetStreamUri`, `LockDoor`), identify which service owns it
4. Return detailed information in this format:

```
## ONVIF Spec Lookup: [Service / Operation Name]

### 📄 Overview
- **Service name**: [official name]
- **Related profiles**: [Profile X, Y, ...]
- **Spec PDF**: [URL]
- **WSDL / Schema**: [URLs]
- **Namespace**: [WSDL target namespace]
- **Typical HTTP endpoint**: [http://<ip>/onvif/...]

### 🔧 Key Operations & SOAP Actions
| Operation | SOAPAction | Description |
|-----------|-----------|-------------|
| OperationName | "namespace/OperationName" | what it does |

### 📡 Protocol Notes
[How this service relates to RTSP / WS-Discovery / MQTT if applicable]
[Authentication requirements for this service]

### 💡 Implementation Notes
[Key considerations for real-world usage; common pitfalls; conditional feature flags]

### 🔗 Related Services
[Other ONVIF services that interact with this one]
```

5. If the latest spec version is needed, fetch it directly:
   - Specifications index: https://www.onvif.org/profiles/specifications/
   - GitHub: https://github.com/onvif/specs
