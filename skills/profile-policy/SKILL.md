---
name: profile-policy
description: ONVIF Profile Policy와 Add-on 개념을 설명합니다. 프로필/애드온의 생성·수정·폐기 프로세스, Add-on과 Profile의 차이, Profile Q 폐기 이력 등을 안내합니다. 예: /onvif-pro:profile-policy add-on이란, /onvif-pro:profile-policy profile-q, /onvif-pro:profile-policy 새 프로필 제안
---

# ONVIF Profile Policy & Add-on Concept Skill

`$ARGUMENTS`에 대해 ONVIF Profile Policy 및 Add-on 개념을 설명합니다.

## ONVIF Profile Policy

**공식 문서:** [ONVIF Profile Policy v3.5 (October 2024)](https://www.onvif.org/wp-content/uploads/2024/10/onvif-profile-policy-v3-5.pdf)

### 프로필(Profile) 핵심 원칙
- **고정 기능 세트**: 한번 확정된 프로필의 필수/조건부 기능은 변경 불가
- **완결성**: 프로필 하나만으로 특정 사용 사례를 완전히 커버해야 함
- **적합성 전용 주장**: 등록된 제품만 ONVIF 적합성을 주장할 수 있음
- **다중 지원 가능**: 한 장치가 여러 프로필 지원 가능 (예: 카메라 + Profile T + G)

### 애드온(Add-on) 핵심 원칙 (공식 정의)
출처: https://www.onvif.org/profiles/add-on/

1. **단일 사용 사례**: 하나 이상의 기능으로 구성되며, 단 하나의 사용 사례를 해결
2. **프로필 종속**: 반드시 하나 이상의 기존 비-폐기 프로필과 함께 사용 (단독 사용 불가)
3. **프로필로 승격 불가**: 애드온 자체는 프로필이 되기에 기능이 충분하지 않음
4. **중복 금지**: 애드온의 기능은 기존 비-폐기 프로필에 이미 포함된 기능이어선 안 됨
5. **조건부/선택 요건 제한**:
   - 장치(Device): 조건부 요건 및 선택적 요건 불허
   - 클라이언트(Client): 선택적 요건은 사례별로만 허용
6. **버전 관리**: 기술 변화에 대응하기 위해 기능 추가/제거 가능 (프로필은 불가)
7. **개발 속도**: 프로필보다 사양 및 테스트 도구 개발이 빠름

### 프로필/애드온 생성 프로세스
1. **제안**: 시장 요구사항 및 사용 사례 문서화
2. **검토**: ONVIF 기술 위원회 검토
3. **개발**: 사양 초안 작성
4. **테스트 도구 개발**: 적합성 테스트 도구 제작
5. **승인 및 출시**: 공식 버전 배포
6. **폐기(Deprecation)**: 더 이상 사용되지 않는 프로필 단계적 폐기

## 폐기된 프로필

### Profile Q (Deprecated)
- **폐기일**: 2022년 4월 1일부터 폐기
- **원래 목적**: 기본 장치 구성 (기본 네트워크 설정, 인증 등)
- **폐기 이유**: Profile S/T가 해당 기능을 충분히 커버
- **주의**: Profile Q를 지원 주장하는 신규 제품은 ONVIF 적합성 인정 불가
- **참고**: https://www.onvif.org/profiles/profile-q/

## Add-on vs Profile 비교

| 항목 | Profile | Add-on |
|------|---------|--------|
| 독립 사용 | ✅ 가능 | ❌ 불가 (프로필 필요) |
| 기능 범위 | 포괄적 | 단일 사용 사례 |
| 버전 변경 | ❌ 고정 | ✅ 버전업 가능 |
| 조건부 요건 | 허용 | 장치는 불허 |
| 개발 속도 | 느림 | 빠름 |
| 적합성 단독 | ✅ 가능 | ❌ 불가 (프로필과 함께) |

## 현재 Add-on 목록

### TLS Configuration Add-on (유일한 현행 Add-on)
- **목적**: ONVIF 장치·클라이언트 간 TLS 통신 설정 표준화
- **버전 1.0**: 2027년 3월 31일 적합성 제출 마감
- **버전 2.0**: 출시 예정 (2027년 초)
- **필요 프로필**: S, T, G, C, A, D, M 중 하나 이상
- **공식 페이지**: https://www.onvif.org/profiles/add-on/tls-configuration-add-on/
- **웨비나 자료**: https://www.onvif.org/wp-content/uploads/2024/04/onvif-add-on-webinar-20240425.pdf

## Profile Feature Overview

ONVIF 공식 프로필 기능 비교표:
- **문서**: [ONVIF Profile Feature Overview v2.6 (April 2022)](https://www.onvif.org/wp-content/uploads/2022/04/onvif-profile-feature-overview.pdf)
- **내용**: 모든 프로필의 기능 목록과 필수(M)/조건부(C) 여부를 한눈에 비교

## 출력 형식

```
## ONVIF Profile Policy / Add-on 안내

**질문:** [입력 키워드]

### 📋 관련 정책
[해당 정책 설명]

### ⚖️ 실무적 시사점
[개발자/통합사 관점의 실용적 조언]

### 🔗 공식 참조
- [관련 문서 링크들]
```
