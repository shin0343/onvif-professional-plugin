---
name: diagnose
description: Diagnoses ONVIF interoperability issues, communication errors, and feature mismatches between devices. Describe the symptoms and receive a systematic diagnostic tree with solutions. Examples: /onvif-pro:diagnose camera cannot connect to NVR, /onvif-pro:diagnose PTZ control not working
---

# ONVIF Diagnostics Skill

Systematically diagnoses the ONVIF issue described in `$ARGUMENTS` and provides a resolution path.

## Diagnostic Framework

### Step 1: Classify the Problem
Identify the following from `$ARGUMENTS`:
- **Symptoms**: no connection, video drops, feature not working, authentication failure, etc.
- **Devices involved**: camera, NVR, VMS, access control panel, client software
- **Related profiles**: which of Profile S/T/G/C/A/D/M is involved
- **When it occurs**: during initial setup, after firmware upgrade, only under certain conditions, etc.

### Step 2: ONVIF Diagnostic Tree

**[Category A] Discovery / Connection Issues**
- WS-Discovery multicast network problem → try direct unicast connection
- Authentication failure (HTTP Digest, WS-Security) → re-verify credentials
- Incorrect ONVIF service URL → call GetCapabilities to discover actual endpoint
- Firewall blocking ports → check ports 80 / 443 / 554 / 8000

**[Category B] Video Streaming Issues (Profile S/T)**
- RTSP connection failure → re-call GetStreamUri, verify port 554
- Codec mismatch (H.264 vs H.265) → check GetVideoEncoderConfigurations
- Resolution/frame rate negotiation failure → check GetVideoEncoderConfigurationOptions
- Multicast streaming problem → switch to unicast, check IGMP settings

**[Category C] PTZ Issues (Profile S/T)**
- PTZ service not found → check PTZ URI in GetCapabilities response
- Preset not executing → call GetPresets to verify existing presets
- Stops during ContinuousMove → consider switching to AbsoluteMove
- Speed/range exceeded → check allowed range via GetConfigurationOptions

**[Category D] Recording / Playback Issues (Profile G)**
- Schedule-based recording not triggering → check Schedule service configuration
- Recording search returns no results → call GetRecordingInformation to confirm recordings exist
- Playback stream error → re-call GetReplayUri
- Export failure → check available storage and Export service support

**[Category E] Access Control Issues (Profile C/A/D)**
- Door control commands ignored → check AccessPoint status and permissions
- Credential not recognized → check Credential service registration state
- Schedule not applied → verify Schedule settings and AccessRule linkage
- Events not received → verify Event service subscription

**[Category F] Analytics / Metadata Issues (Profile M)**
- No analytics metadata → activate Analytics service and check configuration
- MQTT events not received → check broker connection and topic configuration
- Object classification errors → review Analytics rule settings

**[Category G] Conformance Mismatch Issues**
- Conditional feature behavior differs between devices → confirm whether the feature is implemented in a proprietary way
- Profile version mismatch between device and client → verify both sides support the same profile version
- Add-on version mismatch (especially TLS Add-on) → check version compatibility

### Step 3: Output Format

```
## ONVIF Diagnostic Report

**Symptoms:** [summary of reported issue]

### 🔍 Diagnosis
**Probable causes (ordered by likelihood):**
1. [Primary cause] — [rationale]
2. [Secondary cause] — [rationale]

### 📋 Diagnostic Checklist
- [ ] [Item 1] → [How to verify]
- [ ] [Item 2] → [How to verify]
- [ ] [Item 3] → [How to verify]

### 🔧 Resolution
**Immediate steps:**
1. [Step-by-step fix]

**Root cause resolution:**
1. [Long-term fix]

### 📡 Relevant ONVIF Specifications
- [Service/WSDL name]: [description]

### ⚡ Prevention
- [Advice to avoid the same issue in the future]
```
