---
name: spec-lookup
description: Retrieves detailed information about a specific ONVIF service, WSDL, XSD schema, or specification document. Examples: /onvif-pro:spec-lookup Media2, /onvif-pro:spec-lookup analytics.wsdl, /onvif-pro:spec-lookup PTZ
---

# ONVIF Spec Lookup Skill

Provides detailed technical information about the ONVIF service or specification given in `$ARGUMENTS`.

Source: https://www.onvif.org/profiles/specifications/ (all URLs verified against the official page)

## Official Specification Database (Verified URLs)

### Core Service
**ONVIF Core Specification**
- Spec: https://www.onvif.org/specs/core/ONVIF-Core-Specification.pdf
- WSDL: device.wsdl → https://www.onvif.org/ver10/device/wsdl/devicemgmt.wsdl
- XSD: onvif.xsd → https://www.onvif.org/ver10/schema/onvif.xsd
- XSD: common.xsd → https://www.onvif.org/ver10/schema/common.xsd
- Event: event.wsdl → https://www.onvif.org/ver10/events/wsdl/event.wsdl
- YAML: storagerenewal.yaml → https://www.onvif.org/yaml/viewer.php?yaml=storagerenewal.yaml
- Key features: Device management, network configuration, system info, user management, WS-Discovery

### Streaming / Data Format
**Streaming Specification**
- Spec: https://www.onvif.org/specs/stream/ONVIF-Streaming-Spec.pdf
- XSD: metadatastream.xsd → https://www.onvif.org/ver10/schema/metadatastream.xsd

**Media Signing**
- Spec: https://www.onvif.org/specs/stream/ONVIF-MediaSigning-Spec.pdf

**Export File Format**
- Spec: https://www.onvif.org/specs/stream/ONVIF-ExportFileFormat-Spec.pdf

**WebRTC**
- Spec: https://www.onvif.org/specs/stream/ONVIF-WebRTC-Spec.pdf

### Media Services (Profile S / T)
**Media Service** (for Profile S)
- Spec: https://www.onvif.org/specs/srv/media/ONVIF-Media-Service-Spec.pdf
- WSDL: media.wsdl → https://www.onvif.org/ver10/media/wsdl/media.wsdl

**Media2 Service** (for Profile T)
- Spec: https://www.onvif.org/specs/srv/media/ONVIF-Media2-Service-Spec.pdf
- WSDL: media2.wsdl → https://www.onvif.org/ver20/media/wsdl/media.wsdl
- Improvements over Media: HTTPS streaming, H.265, metadata configuration, OSD

**Imaging Service**
- Spec: https://www.onvif.org/specs/srv/img/ONVIF-Imaging-Service-Spec.pdf
- WSDL: imaging.wsdl → https://www.onvif.org/ver20/imaging/wsdl/imaging.wsdl

**PTZ Service**
- Spec: https://www.onvif.org/specs/srv/ptz/ONVIF-PTZ-Service-Spec.pdf
- WSDL: ptz.wsdl → https://www.onvif.org/ver20/ptz/wsdl/ptz.wsdl

### Recording Services (Profile G)
**Recording Control Service**
- Spec: https://www.onvif.org/specs/srv/rec/ONVIF-RecordingControl-Service-Spec.pdf
- WSDL: recording.wsdl → https://www.onvif.org/ver10/recording.wsdl

**Recording Search Service**
- Spec: https://www.onvif.org/specs/srv/rsrch/ONVIF-RecordingSearch-Service-Spec.pdf
- WSDL: search.wsdl → https://www.onvif.org/ver10/search.wsdl

**Replay Control Service**
- Spec: https://www.onvif.org/specs/srv/replay/ONVIF-ReplayControl-Service-Spec.pdf
- WSDL: replay.wsdl → https://www.onvif.org/ver10/replay.wsdl

**Receiver Service**
- Spec: https://www.onvif.org/specs/srv/rcv/ONVIF-Receiver-Service-Spec.pdf
- WSDL: receiver.wsdl → https://www.onvif.org/ver10/receiver.wsdl

### Access Control Services (Profile C / A / D)
**Access Control Service** (for Profile C)
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-AccessControl-Service-Spec.pdf
- WSDL: accesscontrol.wsdl → https://www.onvif.org/ver10/pacs/accesscontrol.wsdl
- XSD: types.xsd → https://www.onvif.org/ver10/pacs/types.xsd

**Door Control Service** (for Profile C)
- Spec: https://www.onvif.org/specs/srv/door/ONVIF-DoorControl-Service-Spec.pdf
- WSDL: doorcontrol.wsdl → https://www.onvif.org/ver10/pacs/doorcontrol.wsdl

**Access Rules Service** (for Profile A)
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-AccessRules-Service-Spec.pdf
- WSDL: accessrules.wsdl → https://www.onvif.org/ver10/accessrules/wsdl/accessrules.wsdl

**Credential Service** (for Profile A / D)
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-Credential-Service-Spec.pdf
- WSDL: credential.wsdl → https://www.onvif.org/ver10/credential/wsdl/credential.wsdl

**Authentication Behavior Service**
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-AuthenticationBehavior-Service-Spec.pdf
- WSDL: authenticationbehavior.wsdl → https://www.onvif.org/ver10/authenticationbehavior/wsdl/authenticationbehavior.wsdl

**Schedule Service** (for Profile A / C)
- Spec: https://www.onvif.org/specs/srv/sched/ONVIF-Scheduler-Service-Spec.pdf
- WSDL: schedule.wsdl → https://www.onvif.org/ver10/schedule/wsdl/schedule.wsdl

### Analytics Service (Profile M)
**Analytics Service**
- Spec: https://www.onvif.org/specs/srv/analytics/ONVIF-Analytics-Service-Spec.pdf
- WSDL: analytics.wsdl → https://www.onvif.org/ver20/analytics/wsdl/analytics.wsdl
- XSD: rules.xsd → https://www.onvif.org/ver20/analytics/rules.xsd
- XSD: humanbody.xsd → https://www.onvif.org/ver20/analytics/humanbody.xsd
- XSD: humanface.xsd → https://www.onvif.org/ver20/analytics/humanface.xsd

### Security Services
**Security Service**
- Spec: https://www.onvif.org/specs/srv/security/ONVIF-Security-Service-Spec.pdf
- WSDL: advancedsecurity.wsdl → https://www.onvif.org/ver10/advancedsecurity/wsdl/advancedsecurity.wsdl

**Security Baseline**
- Spec: https://www.onvif.org/specs/srv/security/ONVIF-SecurityBaseline-Spec.pdf

### Other Services
**Thermal Service**
- Spec: https://www.onvif.org/specs/srv/thermal/ONVIF-Thermal-Service-Spec.pdf
- WSDL: thermal.wsdl → https://www.onvif.org/ver10/thermal/wsdl/thermal.wsdl
- XSD: radiometry.xsd → https://www.onvif.org/ver20/analytics/radiometry.xsd

**Device IO Service**
- Spec: https://www.onvif.org/specs/srv/io/ONVIF-DeviceIo-Service-Spec.pdf
- WSDL: deviceio.wsdl → https://www.onvif.org/ver10/deviceio.wsdl

**Display Service**
- Spec: https://www.onvif.org/specs/srv/disp/ONVIF-Display-Service-Spec.pdf
- WSDL: display.wsdl → https://www.onvif.org/ver10/display.wsdl

**Action Engine Service**
- Spec: https://www.onvif.org/specs/srv/act/ONVIF-ActionEngine-Service-Spec.pdf
- WSDL: actionengine.wsdl → https://www.onvif.org/ver10/actionengine.wsdl

**Application Management Service**
- Spec: https://www.onvif.org/specs/srv/appmgmt/ONVIF-ApplicationManagement-Service-Spec.pdf
- WSDL: appmgmt.wsdl → https://www.onvif.org/ver10/appmgmt/wsdl/appmgmt.wsdl

**Cloud Integration Service**
- Spec: https://www.onvif.org/specs/srv/cloudint/ONVIF-CloudIntegration-Service-Spec.pdf
- YAML: cloudintegration.yaml → https://www.onvif.org/yaml/viewer.php?yaml=cloudintegration.yaml

**Uplink Service**
- Spec: https://www.onvif.org/specs/srv/uplink/ONVIF-Uplink-Spec.pdf
- WSDL: uplink.wsdl → https://www.onvif.org/ver10/uplink/wsdl/uplink.wsdl

**Provisioning Service**
- Spec: https://www.onvif.org/specs/srv/ptz/ONVIF-Provisioning-Service-Spec.pdf
- WSDL: provisioning.wsdl → https://www.onvif.org/ver10/provisioning/wsdl/provisioning.wsdl

**Resource Query Service**
- Spec: https://www.onvif.org/specs/srv/res/ONVIF-ResourceQuery-Spec.pdf
- (No WSDL — query-only service)

## Lookup Procedure

1. Identify the service name, WSDL name, or keyword from `$ARGUMENTS`
2. Find the matching entry in the database above
3. Return detailed information in the following format:

```
## ONVIF Spec Lookup: [Service Name]

### 📄 Overview
- **Service name**: [official name]
- **Related profiles**: [Profile X, Y, ...]
- **Spec PDF**: [URL]
- **WSDL/Schema**: [URLs]

### 🔧 Key Features & Operations
[List of core features and main SOAP operations]

### 📡 Service Endpoint Pattern
[Typical service URI pattern]

### 💡 Implementation Notes
[Key considerations for real-world usage]

### 🔗 Related Services
[Other ONVIF services that relate to this one]
```

4. If the latest version of a spec document is needed, use the WebFetch tool to retrieve it directly:
   - Official list: https://www.onvif.org/profiles/specifications/
   - GitHub specifications: https://github.com/onvif/specs
