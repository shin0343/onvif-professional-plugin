---
description: ONVIF 장치 간 호환성 문제, 통신 오류, 기능 불일치 등을 진단합니다. 증상을 설명하면 체계적인 진단 트리와 해결책을 제공합니다. 예: /onvif-pro:diagnose 카메라와 NVR 연결 안됨, /onvif-pro:diagnose PTZ 제어 안됨
---

# ONVIF Diagnostics Skill

`$ARGUMENTS`에 설명된 ONVIF 관련 문제를 체계적으로 진단하고 해결책을 제시합니다.

## 진단 프레임워크

### 1단계: 문제 분류
`$ARGUMENTS`에서 다음을 식별하세요:
- **증상**: 연결 안됨, 영상 끊김, 기능 미동작, 인증 실패 등
- **관련 장치**: 카메라, NVR, VMS, 출입통제 패널, 클라이언트 소프트웨어
- **관련 프로필**: Profile S/T/G/C/A/D/M 중 어느 것
- **발생 시점**: 초기 설정, 업그레이드 후, 특정 조건에서만 등

### 2단계: ONVIF 진단 트리

**[범주 A] 디스커버리/연결 문제**
- WS-Discovery 멀티캐스트 네트워크 문제 → 유니캐스트 직접 연결 시도
- 인증 실패 (HTTP Digest, WS-Security) → 자격증명 재확인
- ONVIF 서비스 URL 오류 → GetCapabilities로 실제 엔드포인트 확인
- 네트워크 방화벽 포트 차단 → 포트 80/443/554/8000 확인

**[범주 B] 영상 스트리밍 문제 (Profile S/T 관련)**
- RTSP 연결 실패 → GetStreamUri 재호출, 포트 554 확인
- 코덱 불일치 (H.264 vs H.265) → GetVideoEncoderConfigurations 확인
- 해상도/프레임레이트 협상 실패 → GetVideoEncoderConfigurationOptions 확인
- 멀티캐스트 스트리밍 문제 → 유니캐스트 전환, IGMP 설정 확인

**[범주 C] PTZ 문제 (Profile S/T 관련)**
- PTZ 서비스 없음 → GetCapabilities에서 PTZ URI 확인
- 프리셋 동작 안됨 → GetPresets로 기존 프리셋 확인
- 연속 이동 중 멈춤 → ContinuousMove 대신 AbsoluteMove 사용 고려
- 속도/범위 초과 → GetConfigurationOptions로 허용 범위 확인

**[범주 D] 녹화/재생 문제 (Profile G 관련)**
- 녹화 일정 동작 안됨 → Schedule 서비스 설정 확인
- 녹화 검색 결과 없음 → GetRecordingInformation으로 실제 녹화 확인
- 재생 스트림 오류 → GetReplayUri 재호출
- 내보내기 실패 → 저장 공간 및 Export 서비스 지원 확인

**[범주 E] 출입통제 문제 (Profile C/A/D 관련)**
- 도어 제어 명령 무시 → AccessPoint 상태 및 권한 확인
- 자격증명 인식 안됨 → Credential 서비스 등록 상태 확인
- 일정 적용 안됨 → Schedule 설정 및 AccessRule 연결 확인
- 이벤트 미수신 → Event 서비스 구독 확인

**[범주 F] 분석/메타데이터 문제 (Profile M 관련)**
- 분석 메타데이터 없음 → Analytics 서비스 활성화 및 설정 확인
- MQTT 이벤트 미수신 → 브로커 연결 및 토픽 설정 확인
- 객체 분류 오류 → Analytics 룰 설정 검토

**[범주 G] 적합성(Conformance) 불일치 문제**
- 조건부(Conditional) 기능 동작 불일치 → 해당 기능이 자사 방식으로 지원되는지 확인
- 프로필 버전 불일치 → 장치와 클라이언트의 프로필 버전 일치 여부 확인
- Add-on 버전 불일치 (특히 TLS Add-on) → 버전 호환성 확인

### 3단계: 출력 형식

```
## ONVIF 진단 보고서

**증상:** [입력된 문제 요약]

### 🔍 진단 결과
**예상 원인 (가능성 순):**
1. [주요 원인] — [근거]
2. [부차 원인] — [근거]

### 📋 진단 체크리스트
- [ ] [확인 항목 1] → [확인 방법]
- [ ] [확인 항목 2] → [확인 방법]
- [ ] [확인 항목 3] → [확인 방법]

### 🔧 해결 방법
**즉시 시도:**
1. [단계별 해결 방법]

**근본 해결:**
1. [장기적 해결 방법]

### 📡 관련 ONVIF 사양
- [관련 서비스/WSDL 이름]: [설명]

### ⚡ 예방 방법
- [향후 동일 문제 예방을 위한 조언]
```
