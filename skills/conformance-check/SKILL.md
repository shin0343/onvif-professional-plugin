---
description: 특정 ONVIF 장치나 시스템의 ONVIF 적합성(Conformance) 준수 여부를 검토합니다. 예: /onvif-pro:conformance-check Profile T 카메라, /onvif-pro:conformance-check 출입통제 시스템 Profile C+A
---

# ONVIF Conformance Check Skill

`$ARGUMENTS`에 지정된 장치/시스템의 ONVIF 적합성 요건을 체크리스트 형태로 제공합니다.

## 적합성 선언 기본 요건 (모든 프로필 공통)

ONVIF 적합성을 주장하려면 반드시:
1. ONVIF 멤버사여야 함
2. 최소 1개 이상의 프로필 지원
3. ONVIF 적합 제품 데이터베이스에 등록
4. 해당 펌웨어/소프트웨어 버전 명시

## 프로필별 체크리스트

### Profile S 체크리스트
**필수(Mandatory) 기능:**
- [ ] WS-Discovery 지원 (장치 검색)
- [ ] GetCapabilities 응답
- [ ] GetProfiles, GetStreamUri 구현
- [ ] RTSP/RTP 스트리밍 (H.264 필수)
- [ ] GetVideoEncoderConfigurations
- [ ] 이벤트 서비스 기본 구독

**조건부(Conditional) 기능:**
- [ ] PTZ 지원 시 → PTZ 서비스 ONVIF 방식 구현
- [ ] 오디오 지원 시 → AudioEncoder 설정
- [ ] 멀티스트림 지원 시 → 다중 VideoSource 구성

### Profile T 체크리스트
**Profile S 외 추가 필수:**
- [ ] H.265(HEVC) 비디오 인코딩
- [ ] Media2 서비스 구현
- [ ] HTTPS 기반 스트리밍 (혹은 HTTP)
- [ ] 메타데이터 스트리밍 설정

**조건부:**
- [ ] 양방향 오디오 지원 시 → Backchannel 구현
- [ ] OSD 지원 시 → OSD 설정 서비스

### Profile G 체크리스트
**필수:**
- [ ] Recording Control 서비스
- [ ] Recording Search 서비스
- [ ] Replay Control 서비스
- [ ] 일정 기반 녹화 설정

**조건부:**
- [ ] 내보내기 지원 시 → Export File Format 준수

### Profile C 체크리스트
**필수:**
- [ ] Door Control 서비스
- [ ] Access Control 서비스 (기본)
- [ ] 이벤트 서비스 (출입 이벤트)
- [ ] 사이트 정보 조회

### Profile A 체크리스트
**필수:**
- [ ] Access Rules 서비스
- [ ] Credential 서비스
- [ ] Schedule 서비스
- [ ] 인증 방식 설정

### Profile D 체크리스트
**필수:**
- [ ] 자격증명 식별자 전송
- [ ] 접근 요청/응답
- [ ] 잠금/해제 액션 수행

**조건부:**
- [ ] 생체인식 지원 시 → 해당 인식 방식 ONVIF 방식으로 구현

### Profile M 체크리스트
**필수:**
- [ ] Analytics 서비스
- [ ] 메타데이터 스트리밍 (metadatastream.xsd 준수)
- [ ] 객체 분류 (최소 1개: 차량/인체/번호판/얼굴)
- [ ] 이벤트 인터페이스

**조건부:**
- [ ] MQTT 지원 시 → MQTT 브로커 설정 ONVIF 방식
- [ ] 지리적 위치 지원 시 → GeoLocation 메타데이터 포함

## 테스트 도구 안내

```
## ONVIF 적합성 검토 결과: [장치/시스템]

### ✅ 충족된 요건
- [충족 항목들]

### ❌ 미충족 또는 확인 필요 요건
- [미충족 항목]: [조치 방법]

### 📋 테스트 도구
- ONVIF 공식 테스트 도구 다운로드:
  - Device Test Tool: https://www.onvif.org/profiles/conformance/device-test-2/
  - Client Test Tool: https://www.onvif.org/profiles/conformance/client-test/

### 📝 적합성 신청 절차
1. 테스트 도구 통과
2. Declaration of Conformance (DoC) 작성
3. Interface Guide 준비
4. Feature List 파일 생성
5. Member Tools(https://www.onvif.org/member-tools/)에 제출

### ⚠️ 주의사항
- TLS Add-on v1.0: 2027년 3월 31일 제출 마감
- 적합성은 특정 펌웨어 버전에 귀속됨
- 제품 검색: https://www.onvif.org/conformant-products/
```
