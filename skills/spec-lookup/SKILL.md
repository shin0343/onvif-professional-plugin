---
description: 특정 ONVIF 서비스, WSDL, XSD 스키마, 또는 사양 문서에 대한 상세 정보를 조회합니다. 예: /onvif-pro:spec-lookup Media2, /onvif-pro:spec-lookup analytics.wsdl, /onvif-pro:spec-lookup PTZ
---

# ONVIF Spec Lookup Skill

`$ARGUMENTS`에 지정된 ONVIF 서비스 또는 사양에 대한 상세 기술 정보를 제공합니다.

출처: https://www.onvif.org/profiles/specifications/ (공식 확인된 URL 목록)

## 공식 사양 데이터베이스 (검증된 URL)

### 코어 서비스
**ONVIF Core Specification**
- Spec: https://www.onvif.org/specs/core/ONVIF-Core-Specification.pdf
- WSDL: device.wsdl → https://www.onvif.org/ver10/device/wsdl/devicemgmt.wsdl
- XSD: onvif.xsd → https://www.onvif.org/ver10/schema/onvif.xsd
- XSD: common.xsd → https://www.onvif.org/ver10/schema/common.xsd
- Event: event.wsdl → https://www.onvif.org/ver10/events/wsdl/event.wsdl
- YAML: storagerenewal.yaml → https://www.onvif.org/yaml/viewer.php?yaml=storagerenewal.yaml
- 주요 기능: 장치 관리, 네트워크 설정, 시스템 정보, 사용자 관리, WS-Discovery

### 스트리밍 / 데이터 형식
**Streaming Specification**
- Spec: https://www.onvif.org/specs/stream/ONVIF-Streaming-Spec.pdf
- XSD: metadatastream.xsd → https://www.onvif.org/ver10/schema/metadatastream.xsd

**Media Signing**
- Spec: https://www.onvif.org/specs/stream/ONVIF-MediaSigning-Spec.pdf

**Export File Format**
- Spec: https://www.onvif.org/specs/stream/ONVIF-ExportFileFormat-Spec.pdf

**WebRTC**
- Spec: https://www.onvif.org/specs/stream/ONVIF-WebRTC-Spec.pdf

### Media 서비스 (Profile S/T)
**Media Service** (Profile S용)
- Spec: https://www.onvif.org/specs/srv/media/ONVIF-Media-Service-Spec.pdf
- WSDL: media.wsdl → https://www.onvif.org/ver10/media/wsdl/media.wsdl

**Media2 Service** (Profile T용)
- Spec: https://www.onvif.org/specs/srv/media/ONVIF-Media2-Service-Spec.pdf
- WSDL: media2.wsdl → https://www.onvif.org/ver20/media/wsdl/media.wsdl
- 향상점: HTTPS 스트리밍, H.265, 메타데이터 설정, OSD

**Imaging Service**
- Spec: https://www.onvif.org/specs/srv/img/ONVIF-Imaging-Service-Spec.pdf
- WSDL: imaging.wsdl → https://www.onvif.org/ver20/imaging/wsdl/imaging.wsdl

**PTZ Service**
- Spec: https://www.onvif.org/specs/srv/ptz/ONVIF-PTZ-Service-Spec.pdf
- WSDL: ptz.wsdl → https://www.onvif.org/ver20/ptz/wsdl/ptz.wsdl

### 녹화 서비스 (Profile G)
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

### 출입통제 서비스 (Profile C/A/D)
**Access Control Service** (Profile C용)
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-AccessControl-Service-Spec.pdf
- WSDL: accesscontrol.wsdl → https://www.onvif.org/ver10/pacs/accesscontrol.wsdl
- XSD: types.xsd → https://www.onvif.org/ver10/pacs/types.xsd

**Door Control Service** (Profile C용)
- Spec: https://www.onvif.org/specs/srv/door/ONVIF-DoorControl-Service-Spec.pdf
- WSDL: doorcontrol.wsdl → https://www.onvif.org/ver10/pacs/doorcontrol.wsdl

**Access Rules Service** (Profile A용)
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-AccessRules-Service-Spec.pdf
- WSDL: accessrules.wsdl → https://www.onvif.org/ver10/accessrules/wsdl/accessrules.wsdl

**Credential Service** (Profile A/D용)
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-Credential-Service-Spec.pdf
- WSDL: credential.wsdl → https://www.onvif.org/ver10/credential/wsdl/credential.wsdl

**Authentication Behavior Service**
- Spec: https://www.onvif.org/specs/srv/access/ONVIF-AuthenticationBehavior-Service-Spec.pdf
- WSDL: authenticationbehavior.wsdl → https://www.onvif.org/ver10/authenticationbehavior/wsdl/authenticationbehavior.wsdl

**Schedule Service** (Profile A/C용)
- Spec: https://www.onvif.org/specs/srv/sched/ONVIF-Scheduler-Service-Spec.pdf
- WSDL: schedule.wsdl → https://www.onvif.org/ver10/schedule/wsdl/schedule.wsdl

### 분석 서비스 (Profile M)
**Analytics Service**
- Spec: https://www.onvif.org/specs/srv/analytics/ONVIF-Analytics-Service-Spec.pdf
- WSDL: analytics.wsdl → https://www.onvif.org/ver20/analytics/wsdl/analytics.wsdl
- XSD: rules.xsd → https://www.onvif.org/ver20/analytics/rules.xsd
- XSD: humanbody.xsd → https://www.onvif.org/ver20/analytics/humanbody.xsd
- XSD: humanface.xsd → https://www.onvif.org/ver20/analytics/humanface.xsd

### 보안 서비스
**Security Service**
- Spec: https://www.onvif.org/specs/srv/security/ONVIF-Security-Service-Spec.pdf
- WSDL: advancedsecurity.wsdl → https://www.onvif.org/ver10/advancedsecurity/wsdl/advancedsecurity.wsdl

**Security Baseline**
- Spec: https://www.onvif.org/specs/srv/security/ONVIF-SecurityBaseline-Spec.pdf

### 기타 서비스
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
- (WSDL 없음 — 쿼리 전용 서비스)

## 조회 절차

1. `$ARGUMENTS`에서 서비스명, WSDL명, 또는 키워드를 식별
2. 위 데이터베이스에서 매칭되는 항목 찾기
3. 해당 항목의 상세 정보를 다음 형식으로 제공:

```
## ONVIF 사양 조회: [서비스명]

### 📄 기본 정보
- **서비스명**: [공식 이름]
- **관련 프로필**: [Profile X, Y...]
- **Spec PDF**: [URL]
- **WSDL/Schema**: [URL들]

### 🔧 주요 기능 및 오퍼레이션
[핵심 기능과 주요 SOAP 오퍼레이션 목록]

### 📡 서비스 엔드포인트 패턴
[일반적인 서비스 URI 패턴]

### 💡 구현 시 주의사항
[실제 사용 시 주요 고려사항]

### 🔗 관련 서비스
[연관된 다른 ONVIF 서비스들]
```

4. 정확한 스펙 문서가 필요한 경우 WebFetch 도구로 공식 URL에서 최신 정보 확인
   - 공식 목록: https://www.onvif.org/profiles/specifications/
   - GitHub 사양: https://github.com/onvif/specs
