---
name: soap-template
description: Generates ready-to-use ONVIF SOAP request templates for any ONVIF service operation, including correct namespaces, WS-Security headers, and HTTP headers. Examples: /onvif-pro:soap-template GetStreamUri Profile T, /onvif-pro:soap-template LockDoor, /onvif-pro:soap-template CreateCredential, /onvif-pro:soap-template GetCapabilities
---

# ONVIF SOAP Template Generator Skill

Generates a ready-to-use ONVIF SOAP 1.2 request template for the operation specified in `$ARGUMENTS`.

---

## ONVIF SOAP Protocol Basics

All ONVIF control operations use **SOAP 1.2 over HTTP POST**:

```
POST /onvif/<service-path> HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8
Content-Length: <length>
```

**SOAP Envelope structure:**
```xml
<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:<prefix>="<service-namespace>">
  <s:Header>
    <!-- WS-Security block (when authentication required) -->
  </s:Header>
  <s:Body>
    <!-- ONVIF operation request element -->
  </s:Body>
</s:Envelope>
```

**SOAP Fault response structure (errors):**
```xml
<s:Envelope>
  <s:Body>
    <s:Fault>
      <s:Code>
        <s:Value>s:Sender | s:Receiver</s:Value>
        <s:Subcode><s:Value>ter:InvalidArgVal | ter:NotFound | ...</s:Value></s:Subcode>
      </s:Code>
      <s:Reason><s:Text>human readable</s:Text></s:Reason>
    </s:Fault>
  </s:Body>
</s:Envelope>
```

---

## WS-Security Header Templates

### UsernameToken with PasswordDigest (most common)
```xml
<s:Header>
  <Security xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"
            xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">
    <UsernameToken>
      <Username>admin</Username>
      <!-- PasswordDigest = Base64(SHA-1(nonce + created + password)) -->
      <Password Type="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0#PasswordDigest">
        [Base64(SHA-1(nonce + created_utc + password))]
      </Password>
      <Nonce EncodingType="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0#Base64Binary">
        [Base64(random-16-bytes)]
      </Nonce>
      <wsu:Created>[UTC timestamp: 2024-01-01T00:00:00Z]</wsu:Created>
    </UsernameToken>
  </Security>
</s:Header>
```

> **Note:** `<wsu:Created>` must be within 5 minutes of device clock. Synchronize clocks via NTP.

### HTTP Digest (alternative — common for RTSP)
Used directly in the HTTP layer — not a SOAP header. Standard HTTP `Authorization: Digest ...` header.

---

## Core Service Templates

### GetCapabilities (device.wsdl)
```http
POST /onvif/device_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tds="http://www.onvif.org/ver10/device/wsdl">
  <s:Body>
    <tds:GetCapabilities>
      <tds:Category>All</tds:Category>
    </tds:GetCapabilities>
  </s:Body>
</s:Envelope>
```

### GetServices (device.wsdl)
```http
POST /onvif/device_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tds="http://www.onvif.org/ver10/device/wsdl">
  <s:Body>
    <tds:GetServices>
      <tds:IncludeCapability>true</tds:IncludeCapability>
    </tds:GetServices>
  </s:Body>
</s:Envelope>
```

---

## Media Service Templates (Profile S — media.wsdl)

### GetProfiles
```http
POST /onvif/media_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:trt="http://www.onvif.org/ver10/media/wsdl">
  [WS-Security header]
  <s:Body>
    <trt:GetProfiles/>
  </s:Body>
</s:Envelope>
```

### GetStreamUri (Profile S — RTSP URI for live streaming)
```http
POST /onvif/media_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:trt="http://www.onvif.org/ver10/media/wsdl"
            xmlns:tt="http://www.onvif.org/ver10/schema">
  [WS-Security header]
  <s:Body>
    <trt:GetStreamUri>
      <trt:StreamSetup>
        <tt:Stream>RTP-Unicast</tt:Stream>
        <tt:Transport>
          <tt:Protocol>RTSP</tt:Protocol>
        </tt:Transport>
      </trt:StreamSetup>
      <trt:ProfileToken>MainStreamToken</trt:ProfileToken>
    </trt:GetStreamUri>
  </s:Body>
</s:Envelope>
```
> Response contains: `<tt:Uri>rtsp://&lt;ip&gt;:554/onvif/stream1</tt:Uri>`
> Then: `DESCRIBE rtsp://<ip>:554/onvif/stream1 RTSP/1.0`

---

## Media2 Service Templates (Profile T — media2.wsdl)

### GetProfiles (Media2)
```http
POST /onvif/media2_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tr2="http://www.onvif.org/ver20/media/wsdl">
  [WS-Security header]
  <s:Body>
    <tr2:GetProfiles>
      <tr2:Type>All</tr2:Type>
    </tr2:GetProfiles>
  </s:Body>
</s:Envelope>
```

### GetStreamUri (Media2 — H.265 / HTTPS stream)
```http
POST /onvif/media2_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tr2="http://www.onvif.org/ver20/media/wsdl">
  [WS-Security header]
  <s:Body>
    <tr2:GetStreamUri>
      <tr2:Protocol>RTSP</tr2:Protocol>  <!-- or RTSPS for TLS -->
      <tr2:ProfileToken>MainProfile</tr2:ProfileToken>
    </tr2:GetStreamUri>
  </s:Body>
</s:Envelope>
```

---

## PTZ Service Templates (ptz.wsdl)

### ContinuousMove
```http
POST /onvif/ptz_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tptz="http://www.onvif.org/ver20/ptz/wsdl"
            xmlns:tt="http://www.onvif.org/ver10/schema">
  [WS-Security header]
  <s:Body>
    <tptz:ContinuousMove>
      <tptz:ProfileToken>MainStreamToken</tptz:ProfileToken>
      <tptz:Velocity>
        <tt:PanTilt x="0.5" y="0.0" space="http://www.onvif.org/ver10/tptz/PanTiltSpaces/VelocityGenericSpace"/>
        <tt:Zoom x="0.0" space="http://www.onvif.org/ver10/tptz/ZoomSpaces/VelocityGenericSpace"/>
      </tptz:Velocity>
    </tptz:ContinuousMove>
  </s:Body>
</s:Envelope>
```

### GotoPreset
```http
POST /onvif/ptz_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tptz="http://www.onvif.org/ver20/ptz/wsdl">
  [WS-Security header]
  <s:Body>
    <tptz:GotoPreset>
      <tptz:ProfileToken>MainStreamToken</tptz:ProfileToken>
      <tptz:PresetToken>Preset001</tptz:PresetToken>
    </tptz:GotoPreset>
  </s:Body>
</s:Envelope>
```

---

## Recording Service Templates (Profile G)

### FindRecordings (search.wsdl)
```http
POST /onvif/search_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tse="http://www.onvif.org/ver10/search/wsdl"
            xmlns:tt="http://www.onvif.org/ver10/schema">
  [WS-Security header]
  <s:Body>
    <tse:FindRecordings>
      <tse:Scope>
        <tt:IncludedSources>
          <tt:Token>VideoSource_0</tt:Token>
        </tt:IncludedSources>
        <tt:RecordingInformationFilter>boolean(//Track)</tt:RecordingInformationFilter>
      </tse:Scope>
    </tse:FindRecordings>
  </s:Body>
</s:Envelope>
```

### GetReplayUri (replay.wsdl)
```http
POST /onvif/replay_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:trp="http://www.onvif.org/ver10/replay/wsdl"
            xmlns:tt="http://www.onvif.org/ver10/schema">
  [WS-Security header]
  <s:Body>
    <trp:GetReplayUri>
      <trp:StreamSetup>
        <tt:Stream>RTP-Unicast</tt:Stream>
        <tt:Transport><tt:Protocol>RTSP</tt:Protocol></tt:Transport>
      </trp:StreamSetup>
      <trp:RecordingToken>Recording_001</trp:RecordingToken>
    </trp:GetReplayUri>
  </s:Body>
</s:Envelope>
```
> RTSP playback with time range: `PLAY rtsp://... RTSP/1.0\r\nRange: clock=20240101T120000Z-20240101T130000Z\r\n`

---

## Access Control Templates (Profile C)

### LockDoor (doorcontrol.wsdl)
```http
POST /onvif/doorcontrol_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tdc="http://www.onvif.org/ver10/doorcontrol/wsdl">
  [WS-Security header]
  <s:Body>
    <tdc:LockDoor>
      <tdc:Token>Door_001</tdc:Token>
    </tdc:LockDoor>
  </s:Body>
</s:Envelope>
```

### AccessDoor (doorcontrol.wsdl — grant temporary access)
```http
POST /onvif/doorcontrol_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tdc="http://www.onvif.org/ver10/doorcontrol/wsdl">
  [WS-Security header]
  <s:Body>
    <tdc:AccessDoor>
      <tdc:Token>Door_001</tdc:Token>
    </tdc:AccessDoor>
  </s:Body>
</s:Envelope>
```

---

## Credential Service Templates (Profile A / D)

### CreateCredential (credential.wsdl)
```http
POST /onvif/credential_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tcr="http://www.onvif.org/ver10/credential/wsdl"
            xmlns:tt="http://www.onvif.org/ver10/pacs">
  [WS-Security header]
  <s:Body>
    <tcr:CreateCredential>
      <tcr:Credential>
        <tt:Description>Employee Badge - John Doe</tt:Description>
        <tt:CredentialIdentifier>
          <tt:Type>
            <tt:Name>com.onvif.wiegand26</tt:Name>
            <tt:FormatType>pt:HexString</tt:FormatType>
          </tt:Type>
          <tt:Value>0A1B2C</tt:Value>  <!-- Wiegand card number -->
        </tt:CredentialIdentifier>
        <tt:Enabled>true</tt:Enabled>
      </tcr:Credential>
    </tcr:CreateCredential>
  </s:Body>
</s:Envelope>
```

---

## Event Subscription Templates (event.wsdl)

### CreatePullPointSubscription (Profile S/T/C/M)
```http
POST /onvif/event_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tev="http://www.onvif.org/ver10/events/wsdl"
            xmlns:wsnt="http://docs.oasis-open.org/wsn/b-2">
  [WS-Security header]
  <s:Body>
    <tev:CreatePullPointSubscription>
      <tev:Filter>
        <!-- Filter to specific topic (optional) -->
        <wsnt:TopicExpression Dialect="http://www.onvif.org/ver10/tev/topicExpression/ConcreteSet">
          tns1:RuleEngine/FieldDetector/ObjectsInside
        </wsnt:TopicExpression>
      </tev:Filter>
      <tev:InitialTerminationTime>PT1H</tev:InitialTerminationTime>
    </tev:CreatePullPointSubscription>
  </s:Body>
</s:Envelope>
```

### PullMessages
```http
POST /onvif/pullpoint_subscription_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tev="http://www.onvif.org/ver10/events/wsdl">
  [WS-Security header]
  <s:Body>
    <tev:PullMessages>
      <tev:Timeout>PT10S</tev:Timeout>
      <tev:MessageLimit>100</tev:MessageLimit>
    </tev:PullMessages>
  </s:Body>
</s:Envelope>
```

---

## Template Generation Procedure

1. Identify the **service** and **operation** from `$ARGUMENTS`
2. Look up the service namespace and typical endpoint path
3. Include the WS-Security header block if authentication is required for that operation
4. Output the complete HTTP request + SOAP envelope
5. Add notes on expected response structure and common errors

```
## ONVIF SOAP Template: [Operation Name]

### 📋 Service Details
- **WSDL**: [service.wsdl URL]
- **Namespace**: [xmlns prefix="namespace"]
- **Typical endpoint**: [http://<ip>/onvif/...]
- **Authentication**: [Required: WS-Security UsernameToken | Optional | Not required]
- **Related profiles**: [Profile X, Y]

### 📤 Request Template
[Complete HTTP request with SOAP envelope]

### 📥 Expected Response
[Key elements in the SOAP response body]

### ⚠️ Common Errors
| HTTP / SOAP Fault | Cause | Fix |
|-------------------|-------|-----|
| 401 Unauthorized | Wrong credentials or expired nonce | Re-authenticate; sync NTP |
| SOAP Fault ter:NotFound | Token not found | Verify token via GetProfiles/GetDoorInfoList |
| SOAP Fault ter:InvalidArgVal | Invalid parameter value | Check allowed ranges via GetConfigurationOptions |

### 💡 Implementation Notes
[Protocol flow context; what to call before/after this operation]
```
