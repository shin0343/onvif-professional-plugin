# ONVIF Professional Plugin for Claude Code

> Expert plugin for ONVIF standards in IP-based physical security systems (CCTV/NVR/Access Control)

[한국어 문서 보기](#korean)

---

## Overview

Provides Claude with deep expertise in all ONVIF profiles (S/T/G/C/A/D/M), network interface specifications, Add-ons, and Conformance Process for IP-based physical security systems.

The agent responds **in the same language you write in** — English or Korean.

## Installation & Testing

```bash
# Load plugin locally for testing
claude --plugin-dir ./onvif-professional-plugin

# Verify agent is active
/agents

# List available skills
/help
```

## Directory Structure

```
onvif-professional-plugin/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── agents/
│   └── onvif-expert.md          # ONVIF expert main agent
├── skills/
│   ├── check-profile/
│   │   └── SKILL.md             # Profile analysis & recommendation
│   ├── diagnose/
│   │   └── SKILL.md             # Compatibility diagnostics
│   ├── spec-lookup/
│   │   └── SKILL.md             # WSDL/spec document lookup
│   ├── design-system/
│   │   └── SKILL.md             # System architecture design
│   ├── conformance-check/
│   │   └── SKILL.md             # Conformance validation checklist
│   └── profile-policy/
│       └── SKILL.md             # Profile Policy & Add-on guidance
├── hooks/
│   └── hooks.json               # ONVIF keyword detection hooks
└── settings.json                # Default agent settings
```

## Available Skills

| Skill | Command | Description |
|-------|---------|-------------|
| Profile analysis | `/onvif-pro:check-profile [device/requirements]` | Recommend profiles for your device type |
| Diagnostics | `/onvif-pro:diagnose [symptoms]` | Diagnose compatibility and communication issues |
| Spec lookup | `/onvif-pro:spec-lookup [service name]` | WSDL/XSD/spec details with verified URLs |
| System design | `/onvif-pro:design-system [requirements]` | System architecture design |
| Conformance check | `/onvif-pro:conformance-check [profile]` | Conformance requirements checklist |
| Policy/Add-on | `/onvif-pro:profile-policy [topic]` | Profile Policy, Add-on concepts, Profile Q deprecation |

## Usage Examples

```bash
# Check profile for H.265 PTZ 4K IP camera
/onvif-pro:check-profile IP camera H.265 PTZ 4K

# Diagnose camera-NVR connection issues
/onvif-pro:diagnose 3 out of 15 cameras not connecting to NVR

# Look up Analytics service spec (verified URLs provided)
/onvif-pro:spec-lookup Analytics

# Design a 50-channel integrated security system
/onvif-pro:design-system factory 50-channel CCTV + 20-door access control + biometrics + AI analytics

# Profile T conformance check
/onvif-pro:conformance-check Profile T camera development review

# Add-on concepts / Profile Q deprecation / Policy docs
/onvif-pro:profile-policy what is add-on
/onvif-pro:profile-policy profile-q deprecation reason
```

## Supported ONVIF Specification Scope

### Profiles
- **Profile S** — IP video streaming (H.264, RTSP, PTZ)
- **Profile T** — Advanced video streaming (H.265, HTTPS, two-way audio)
- **Profile G** — Edge storage and recording retrieval
- **Profile C** — Access control door management and events
- **Profile A** — Access control credentials/schedules/rules management
- **Profile D** — Access control peripherals (card readers, biometrics)
- **Profile M** — Analytics metadata and events (AI object classification)
- ~~**Profile Q**~~ — Deprecated April 1, 2022 (no new conformance claims accepted)

### Add-ons
- **TLS Configuration Add-on** — Encrypted communication setup (v1.0/v2.0)

### Network Interface Specifications (30+ services)
Core, Media, Media2, PTZ, Imaging, Streaming, Analytics, Recording Control, Recording Search, Replay, Access Control, Door Control, Access Rules, Credential, Schedule, Authentication Behavior, Thermal, Device IO, Security, Cloud Integration, Uplink, Application Management, Resource Query, Action Engine, Display, WebRTC, and more.

## Official ONVIF Resources

- Profiles overview: https://www.onvif.org/profiles-add-ons-specifications/
- Conformant products: https://www.onvif.org/conformant-products/
- Conformance process: https://www.onvif.org/profiles/conformance/
- GitHub (specifications): https://github.com/onvif/specs

## License

MIT

---

<a name="korean"></a>

# ONVIF Professional Plugin for Claude Code (한국어)

> IP 기반 물리 보안 시스템(CCTV/NVR/출입통제)의 ONVIF 표준 전문가 플러그인

## 개요

ONVIF(Open Network Video Interface Forum) 표준의 모든 프로필(S/T/G/C/A/D/M), 네트워크 인터페이스 사양, Add-on, 그리고 Conformance Process에 대한 전문 지식을 Claude에 제공합니다.

에이전트는 **입력한 언어와 동일한 언어로 응답**합니다 — 영어 또는 한국어.

## 설치 및 테스트

```bash
# 로컬 테스트
claude --plugin-dir ./onvif-professional-plugin

# 플러그인 로드 후 에이전트 활성화 확인
/agents

# 스킬 목록 확인
/help
```

## 사용 가능한 스킬

| 스킬 | 명령어 | 설명 |
|------|--------|------|
| 프로필 분석 | `/onvif-pro:check-profile [장치/요구사항]` | 장치 유형에 맞는 프로필 추천 |
| 문제 진단 | `/onvif-pro:diagnose [증상 설명]` | 호환성/통신 문제 진단 트리 제공 |
| 사양 조회 | `/onvif-pro:spec-lookup [서비스명]` | WSDL/XSD/스펙 문서 상세 정보 (공식 URL 검증) |
| 시스템 설계 | `/onvif-pro:design-system [요구사항]` | 시스템 아키텍처 설계 |
| 적합성 검증 | `/onvif-pro:conformance-check [프로필]` | 적합성 요건 체크리스트 |
| Policy/Add-on | `/onvif-pro:profile-policy [주제]` | Profile Policy, Add-on 개념, Profile Q 폐기 안내 |

## 사용 예시

```bash
# IP 카메라 H.265 지원 여부 확인
/onvif-pro:check-profile IP카메라 H.265 PTZ 4K

# 카메라-NVR 연결 문제 진단
/onvif-pro:diagnose 카메라 15대 중 3대가 NVR에 연결 안됨

# Analytics 서비스 사양 조회 (검증된 URL 제공)
/onvif-pro:spec-lookup Analytics

# 50채널 통합 보안 시스템 설계
/onvif-pro:design-system 공장 50채널 CCTV + 출입통제 20도어 + 생체인식 + AI 분석

# Profile T 적합성 체크
/onvif-pro:conformance-check Profile T 카메라 개발 검토

# Add-on 개념 / Profile Q 폐기 이유 / Policy 문서 안내
/onvif-pro:profile-policy add-on이란
/onvif-pro:profile-policy profile-q 폐기 이유
```

## 지원하는 ONVIF 사양 범위

### 프로필
- **Profile S** — IP 기반 영상 스트리밍 (H.264, RTSP, PTZ)
- **Profile T** — 고급 영상 스트리밍 (H.265, HTTPS, 양방향 오디오)
- **Profile G** — 엣지 스토리지 및 녹화 검색
- **Profile C** — 출입통제 도어 제어 및 이벤트
- **Profile A** — 출입통제 자격증명/일정/규칙 관리
- **Profile D** — 출입통제 주변기기 (카드리더기, 생체인식)
- **Profile M** — 분석 메타데이터 및 이벤트 (AI 객체 분류)
- ~~**Profile Q**~~ — 2022년 4월 1일 폐기 (더 이상 신규 적합성 주장 불가)

### Add-on
- **TLS Configuration Add-on** — 암호화 통신 설정 (v1.0/v2.0)

### 네트워크 인터페이스 사양 (30+ 서비스)
Core, Media, Media2, PTZ, Imaging, Streaming, Analytics, Recording Control, Recording Search, Replay, Access Control, Door Control, Access Rules, Credential, Schedule, Authentication Behavior, Thermal, Device IO, Security, Cloud Integration, Uplink, Application Management, Resource Query, Action Engine, Display, WebRTC 등

## ONVIF 공식 리소스

- 프로필 개요: https://www.onvif.org/profiles-add-ons-specifications/
- 적합 제품 검색: https://www.onvif.org/conformant-products/
- 적합성 프로세스: https://www.onvif.org/profiles/conformance/
- GitHub (사양): https://github.com/onvif/specs

## 라이선스

MIT
