# ONVIF Professional Plugin for Claude Code

> Expert plugin for ONVIF standards in IP-based physical security systems (CCTV / NVR / Access Control)
>
> IP 기반 물리 보안 시스템(CCTV / NVR / 출입통제)을 위한 ONVIF 전문가 플러그인

## Overview / 개요

Provides Claude with deep expertise across all ONVIF profiles (S/T/G/C/A/D/M), network interface specifications, add-ons, and the conformance process for IP-based physical security systems.

Claude에 ONVIF 전체 프로필(S/T/G/C/A/D/M), 네트워크 인터페이스 사양, 애드온, 적합성 프로세스에 대한 전문 지식을 제공합니다.

The agent responds **in the same language you write in** — English or Korean.

에이전트는 **작성한 언어로 응답**합니다 — 영어 또는 한국어.

---

## Installation / 설치

### English

**Step 1 — Add the marketplace**

```
/plugin marketplace add shin0343/onvif-professional-plugin
```

**Step 2 — Install the plugin**

```
/plugin install onvif-pro@onvif-pro
```

**Step 3 — Verify**

```
/agents
/help
```

You should see **onvif-expert** listed under `/agents` and all `/onvif-pro:*` skills under `/help`.

---

### 한국어

**1단계 — 마켓플레이스 등록**

```
/plugin marketplace add shin0343/onvif-professional-plugin
```

**2단계 — 플러그인 설치**

```
/plugin install onvif-pro@onvif-pro
```

**3단계 — 설치 확인**

```
/agents
/help
```

`/agents`에서 **onvif-expert**가 보이고, `/help`에서 `/onvif-pro:*` 스킬 목록이 확인되면 정상 설치된 것입니다.

---

## Available Skills / 사용 가능한 스킬

| Skill / 스킬 | Command / 명령어 | Description / 설명 |
|---|---|---|
| Profile analysis / 프로필 분석 | `/onvif-pro:check-profile [device/requirements]` | Recommend profiles for your device type / 장치 유형에 맞는 프로필 추천 |
| Diagnostics / 문제 진단 | `/onvif-pro:diagnose [symptoms]` | Diagnose compatibility and communication issues / 호환성·통신 오류 진단 |
| Spec lookup / 사양 조회 | `/onvif-pro:spec-lookup [service name]` | WSDL/XSD/spec details with verified URLs / 검증된 URL로 WSDL·사양 상세 조회 |
| System design / 시스템 설계 | `/onvif-pro:design-system [requirements]` | System architecture design / 시스템 아키텍처 설계 |
| Conformance check / 적합성 체크 | `/onvif-pro:conformance-check [profile]` | Conformance requirements checklist / 적합성 요건 체크리스트 |
| Policy / Add-on | `/onvif-pro:profile-policy [topic]` | Profile Policy, Add-on concepts, Profile Q deprecation / 프로필 정책·애드온 개념·Profile Q 폐기 안내 |

---

## Usage Examples / 사용 예시

```bash
# Recommend profiles for a 4K H.265 PTZ IP camera
# 4K H.265 PTZ IP 카메라에 적합한 프로필 추천
/onvif-pro:check-profile IP camera H.265 PTZ 4K

# Diagnose camera-NVR connection issues
# 카메라-NVR 연결 문제 진단
/onvif-pro:diagnose 3 out of 15 cameras not connecting to NVR

# Look up Analytics service spec (verified URLs provided)
# Analytics 서비스 사양 조회 (검증된 URL 제공)
/onvif-pro:spec-lookup Analytics

# Design a 50-channel integrated security system
# 50채널 통합 보안 시스템 설계
/onvif-pro:design-system factory 50-channel CCTV + 20-door access control + biometrics + AI analytics

# Profile T conformance review
# Profile T 적합성 검토
/onvif-pro:conformance-check Profile T camera development review

# Understand Add-on concept / Profile Q deprecation / Policy docs
# 애드온 개념 / Profile Q 폐기 이유 / 정책 문서 안내
/onvif-pro:profile-policy what is an add-on
/onvif-pro:profile-policy profile-q deprecation reason
```

---

## Local Development & Testing / 로컬 개발 및 테스트

```bash
# Clone the repo / 레포 클론
git clone https://github.com/shin0343/onvif-professional-plugin.git

# Load plugin locally without installing / 설치 없이 로컬에서 플러그인 로드
claude --plugin-dir ./onvif-professional-plugin

# After editing, reload without restarting / 수정 후 재시작 없이 리로드
/reload-plugins
```

---

## Supported ONVIF Specification Scope / 지원 ONVIF 사양 범위

### Profiles / 프로필

| Profile | Target / 대상 | Key Features / 주요 기능 |
|---------|--------|-------------|
| **Profile S** | IP video streaming / IP 영상 스트리밍 | H.264, RTSP, PTZ |
| **Profile T** | Advanced video streaming / 고급 영상 스트리밍 | H.265, HTTPS, two-way audio / 양방향 오디오 |
| **Profile G** | Edge storage & recording / 엣지 스토리지·녹화 | SD card recording, replay, export / SD카드 녹화·재생·내보내기 |
| **Profile C** | Door control / 도어 제어 | Lock/unlock, access events / 잠금·해제·출입 이벤트 |
| **Profile A** | Access control config / 출입통제 설정 관리 | Credentials, schedules, access rules / 자격증명·일정·접근규칙 |
| **Profile D** | Access control peripherals / 출입통제 주변기기 | Card readers, biometrics, keypads / 카드리더·생체인식·키패드 |
| **Profile M** | Analytics & metadata / 분석·메타데이터 | AI object classification, MQTT events / AI 객체분류·MQTT 이벤트 |
| ~~**Profile Q**~~ | *(Deprecated April 1, 2022 / 2022년 4월 1일 폐기)* | Superseded by Profile S/T |

### Add-ons / 애드온

| Add-on | Description / 설명 |
|--------|-------------|
| **TLS Configuration Add-on** | Encrypted communication setup (v1.0 / v2.0) / 암호화 통신 설정 표준화 |

### Network Interface Specifications / 네트워크 인터페이스 사양 (30+ services)

Core, Media, Media2, PTZ, Imaging, Streaming, Analytics, Recording Control, Recording Search, Replay, Access Control, Door Control, Access Rules, Credential, Schedule, Authentication Behavior, Thermal, Device IO, Security, Cloud Integration, Uplink, Application Management, Resource Query, Action Engine, Display, WebRTC, and more.

---

## Directory Structure / 디렉토리 구조

```
onvif-professional-plugin/
├── .claude-plugin/
│   ├── plugin.json              # Plugin manifest (name, version, metadata) / 플러그인 메타데이터
│   └── marketplace.json         # Marketplace registry (enables /plugin install) / 마켓플레이스 레지스트리
├── agents/
│   └── onvif-expert.md          # ONVIF expert main agent / ONVIF 전문가 메인 에이전트
├── skills/
│   ├── check-profile/
│   │   └── SKILL.md             # Profile analysis & recommendation / 프로필 분석·추천
│   ├── diagnose/
│   │   └── SKILL.md             # Compatibility diagnostics / 호환성 진단
│   ├── spec-lookup/
│   │   └── SKILL.md             # WSDL/spec lookup with verified URLs / 검증된 URL로 WSDL·사양 조회
│   ├── design-system/
│   │   └── SKILL.md             # System architecture design / 시스템 아키텍처 설계
│   ├── conformance-check/
│   │   └── SKILL.md             # Conformance validation checklist / 적합성 체크리스트
│   └── profile-policy/
│       └── SKILL.md             # Profile Policy & Add-on concept guidance / 프로필 정책·애드온 개념 안내
└── hooks/
    └── hooks.json               # ONVIF keyword detection hooks / ONVIF 키워드 감지 훅
```

---

## Official ONVIF Resources / 공식 ONVIF 리소스

- Profiles overview / 프로필 개요: https://www.onvif.org/profiles-add-ons-specifications/
- Conformant products / 적합성 인증 제품: https://www.onvif.org/conformant-products/
- Conformance process / 적합성 절차: https://www.onvif.org/profiles/conformance/
- GitHub (specifications / 사양): https://github.com/onvif/specs

---

## License

MIT
