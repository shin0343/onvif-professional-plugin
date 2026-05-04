---
name: diagnose
description: Diagnoses ONVIF interoperability issues, communication errors, and feature mismatches. Covers protocol-layer diagnosis (HTTP/SOAP, RTSP, WS-Discovery), authentication failures, profile mismatches, and service endpoint problems. Examples: /onvif-pro:diagnose camera cannot connect to NVR, /onvif-pro:diagnose PTZ control not working, /onvif-pro:diagnose SOAP 401 error
---

# ONVIF Diagnostics Skill

Systematically diagnoses the ONVIF issue described in `$ARGUMENTS` and provides a resolution path, starting from the transport layer and working up the protocol stack.

## ONVIF Protocol Stack (Diagnostic Reference)

```
Layer 5 — Application:   ONVIF Profile features (PTZ, Recording, Analytics...)
Layer 4 — Service:       SOAP/WSDL operations (GetStreamUri, LockDoor...)
Layer 3 — Messaging:     SOAP 1.2 over HTTP/HTTPS (POST, 200/401/500)
Layer 2 — Streaming:     RTSP/RTP/RTCP (port 554, UDP/TCP)
Layer 1 — Discovery:     WS-Discovery (UDP multicast 239.255.255.250:3702)
Layer 0 — Network:       IP/TCP/UDP, ports 80/443/554/3702/8000
```

Always diagnose from Layer 0 upward — most issues live at layers 0–3.

---

## Diagnostic Framework

### Step 1: Classify the Problem

Extract from `$ARGUMENTS`:
- **Symptoms**: no connection, video dropout, feature not working, 401/500 HTTP error, RTSP DESCRIBE failure, SOAP fault, etc.
- **Devices involved**: camera, NVR, VMS, access control panel, client SDK
- **Related profiles**: which of Profile S/T/G/C/A/D/M is involved
- **When it occurs**: initial setup, after firmware upgrade, specific conditions, intermittent, etc.
- **Error codes / messages**: HTTP status, SOAP fault code, RTSP status

---

### Step 2: ONVIF Diagnostic Tree

**[Category A] Network / Discovery Issues**

*WS-Discovery fails:*
- Multicast blocked → try direct unicast (add device by IP, not via discovery)
- Multicast group: 239.255.255.250, port 3702 (UDP) — check switch IGMP settings
- WS-Discovery uses SOAP over UDP — firewall may be blocking UDP 3702
- Multiple NICs → bind WS-Discovery to correct interface
- VLAN isolation → camera and NVR must be on same VLAN or have multicast routing

*Device reachable by IP but ONVIF not responding:*
- Check port 80 or 8080 is open (HTTP for ONVIF device service)
- Some cameras use port 8000 or custom port — check manufacturer docs
- ONVIF may be disabled by default → enable via camera web UI
- Try `GET http://<ip>/onvif/device_service` — should return SOAP fault (not 404)

---

**[Category B] Authentication / Security Issues**

*HTTP 401 Unauthorized on SOAP requests:*
- Wrong username/password
- WS-Security token expired → nonce must be < 5 minutes old; synchronize clocks (NTP)
- Camera requires WS-Security UsernameToken with PasswordDigest = SHA-1(nonce + created + password)
- Some cameras require HTTP Digest instead of WS-Security — check GetCapabilities Security section
- After password change, re-authenticate in VMS/NVR settings

*HTTPS/TLS failures (TLS Add-on scenarios):*
- Certificate mismatch → verify device certificate CN matches IP/hostname
- Self-signed cert not trusted → import device CA certificate into client trust store
- TLS version mismatch → check advancedsecurity.wsdl `GetSupportedTLSVersions`
- advancedsecurity.wsdl: `GetCertificates`, `UploadCertificateWithPrivateKey`, `SetNetworkProtocols`

---

**[Category C] SOAP / Service Endpoint Issues**

*SOAP 500 Internal Server Error / SOAP Fault:*
- Wrong service endpoint URL → call `GetCapabilities` to discover actual `XAddr` for each service
- Wrong namespace in SOAP envelope → check WSDL namespace (ver10 vs ver20)
- Media vs Media2 confusion → Profile T cameras require Media2 (`/ver20/media/wsdl/media.wsdl`)
- Request body malformed → validate against WSDL schema
- Operation not supported → check `GetCapabilities` or `GetServiceCapabilities` for feature flags

*GetCapabilities returns empty / partial response:*
- Firewall dropping responses → test from device subnet
- MTU fragmentation → SOAP responses can be large; check network MTU (especially over VPN)
- Authentication required for GetCapabilities on some devices (non-standard)

*Service URL mismatch:*
- Never hard-code endpoints — always use `GetCapabilities` XAddr or `GetServices` Address
- Example flow:
  1. `POST /onvif/device_service` → `GetCapabilities` → get Media XAddr
  2. Use Media XAddr for all subsequent media service calls

---

**[Category D] Video Streaming Issues (Profile S / T)**

*RTSP connection failure:*
1. Call `GetStreamUri` via SOAP → get `rtsp://` URI
2. Check RTSP port 554 is open (or alternate port in URI)
3. `DESCRIBE` request to RTSP URI → should return SDP
4. If 401 → RTSP uses same credentials as ONVIF (HTTP Digest for RTSP is common)
5. Firewall between client and camera → open port 554 TCP; for UDP RTP also open ephemeral ports

*Video decoding failure after stream starts:*
- Codec mismatch → call `GetVideoEncoderConfigurations` / Media2 `GetVideoEncoderConfigurations` → verify H.264 vs H.265
- Profile T cameras: client must request H.265 via Media2 — not available via legacy Media service
- Bitrate too high for network → reduce via `SetVideoEncoderConfiguration`
- RTP packet loss → switch from UDP to TCP transport (`SETUP` with `RTP/AVP/TCP`)

*Multicast streaming issues:*
- IGMP snooping misconfigured on switch → disable IGMP snooping or configure multicast routing
- Only one multicast stream can be active per profile — switch to unicast for multiple concurrent clients
- Call `GetMulticastStreamUri` (Profile S) for multicast URI

*HTTPS streaming not working (Profile T):*
- Call `GetStreamUri` via Media2 with `Protocol=HTTPS` → get `rtsps://` URI
- Check port 322 (RTSPS) or 443 is open
- Requires valid TLS certificate on device side

---

**[Category E] PTZ Issues (Profile S / T)**

- PTZ service not in GetCapabilities → camera is fixed-lens; PTZ not supported via ONVIF
- `GetPresets` returns empty list → create presets first via camera web UI or `SetPreset`
- `ContinuousMove` stops immediately → call `Stop` first if previous move is still active
- AbsoluteMove overshoots → use `GetConfigurationOptions` to check PanTiltLimits and ZoomLimits
- Speed parameter rejected → normalize to 0.0–1.0 range (ONVIF normalized speed space)
- PTZ commands accepted but no physical movement → verify ONVIF PTZ service XAddr, not camera's own RS-485 address

---

**[Category F] Recording / Playback Issues (Profile G)**

- Schedule-based recording not triggering → verify Schedule service config AND RecordingJob is active (`CreateRecordingJob` with state=Active)
- `FindRecordings` returns empty → confirm recordings exist via `GetRecordingInformation`
- `GetReplayUri` returns RTSP URI → playback requires RTSP `PLAY` with Range header: `Range: clock=20240101T120000Z-20240101T130000Z`
- Export fails → check storage capacity and `ExportFileFormat` spec compliance
- Time-based search returns wrong results → verify device NTP sync (`GetSystemDateAndTime`)

---

**[Category G] Access Control Issues (Profile C / A / D)**

- Door command `LockDoor` / `UnlockDoor` ignored → check AccessPoint enabled state via `GetAccessPoints`; verify credentials/permissions
- `AccessGranted` event not received → verify WS-BaseNotification subscription is active (`CreatePullPointSubscription` then `PullMessages`)
- Credential not recognized → `GetCredentialList` to verify credential token exists and is Enabled
- Schedule not applied → `GetScheduleList` → verify time ranges; check AccessRule links credential to correct access point
- Profile A ↔ C integration: Profile A manages credentials/schedules; Profile C controls doors — both services must be reachable

---

**[Category H] Analytics / Metadata Issues (Profile M)**

- No analytics metadata in stream → verify Analytics service has active rules (`GetRules`); metadata must be enabled in Media2 `CreateProfile` or `AddMetadataConfiguration`
- MQTT events not received → verify broker IP/port reachable from device; check topic subscription; `GetEventBrokers` for configured broker
- Object classification wrong/missing → check camera analytics capabilities via `GetSupportedRules`; verify rule parameters (sensitivity, ROI, object class filters)
- Metadata XSD validation fails → metadata stream must conform to metadatastream.xsd; parse `tt:MetadataStream` root element

---

**[Category I] Profile / Add-on Conformance Mismatch**

- Device claims Profile T but Media2 not working → run ONVIF Device Test Tool; check if firmware matches claimed conformance version
- Add-on TLS version mismatch → `GetSupportedTLSVersions` (advancedsecurity.wsdl); ensure client and device share at least TLS 1.2
- VMS supports Profile S only but camera is Profile T only → use Profile T camera's Media2 service; many Profile T cameras also expose legacy Media for backward compatibility — check `GetCapabilities`

---

### Step 3: Output Format

```
## ONVIF Diagnostic Report

**Symptoms:** [summary]
**Protocol layer affected:** [Network / Discovery / HTTP-SOAP / RTSP / Application]

### 🔍 Diagnosis
**Probable causes (ordered by likelihood):**
1. [Primary cause] — [rationale based on protocol stack]
2. [Secondary cause] — [rationale]

### 📋 Diagnostic Checklist
- [ ] [Layer 0] Ping device IP → reachable?
- [ ] [Layer 0] Port 80/554/3702 open? (use telnet/nc/nmap)
- [ ] [Layer 1] WS-Discovery Probe sent → XAddrs returned?
- [ ] [Layer 2] GET /onvif/device_service → HTTP 200 or SOAP fault (not 404)?
- [ ] [Layer 3] GetCapabilities → services listed? XAddr values?
- [ ] [Layer 4] Specific service call → SOAP response valid?
- [ ] [Layer 5] Feature working end-to-end?

### 🔧 Resolution
**Immediate steps:**
1. [Step-by-step fix with specific SOAP operations or config changes]

**Root cause resolution:**
1. [Long-term fix / configuration change]

### 📡 Relevant ONVIF Specifications
- [Service name and WSDL]: [what to check]

### ⚡ Prevention
- [Advice to avoid same issue in future: NTP sync, endpoint discovery, profile version checks, etc.]
```
