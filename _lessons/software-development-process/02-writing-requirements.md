---
title: "Writing Requirements"
title_ko: "요구사항 정의서 직접 작성하기"
course: software-development-process
lesson: 2
tags:
  - Requirements Engineering
  - Acceptance Criteria
  - Quality Attributes
---

## 학습 목표

- problem, outcome, scope를 solution과 분리해 한 장으로 정리하기
- 기능 요구사항과 측정 가능한 품질 요구사항 작성하기
- user story, business rule, acceptance criteria의 역할 구분하기
- 승인 이후에도 변경 이유와 영향 범위를 추적하기

## ELI15: 좋은 요구사항은 ‘소원’이 아니라 검사 가능한 약속이다

“검색이 빨라야 한다”는 소원입니다. 누가, 언제, 얼마나 많은 data에서, 몇 초 안에 받아야 하는지 없으므로 성공 여부를 판정할 수 없습니다.

“업무 시간에 동시 사용자 100명이 10만 건 중 목록을 조회할 때, server 응답 시간 p95가 500ms 이하여야 한다”는 검사 가능한 약속입니다. 완벽한 숫자라는 뜻이 아니라, business 영향과 test 방법을 놓고 합의할 수 있다는 뜻입니다.

## 완성 예제: One-page requirement brief

| 항목 | 내용 |
|---|---|
| 문제 | 업무 요청이 메신저·email에 흩어져 담당자, 현재 상태, 변경 이유를 찾는 데 시간이 든다. |
| 목표 outcome | 등록된 요청의 95%가 1영업일 안에 담당자를 가지며, 주간 상태 확인 회의 시간을 50% 줄인다. |
| 사용자 | 요청자, 운영 담당자, 업무 담당자, 감사 담당자 |
| 범위 | 요청 등록, 담당자 배정, 상태 변경, 목록 조회, 상태 변경 감사 기록 |
| 제외 | 일정·근태 관리, 비용 정산, 외부 고객 UI, 담당자 자동 추천 |
| 성공 지표 | 1영업일 내 배정률, 주간 미배정 건수, 상태 조회 소요 시간, API 성공률 |
| 가정 | 사내 identity provider가 사용자 ID와 role을 전달하고, 모든 직원에게 고유 ID가 있다. |
| 제약 | 사내 network에서만 제공하며 변경 이력을 1년 보관한다. |
| 주요 위험 | 기존 channel 병행으로 data가 누락되거나, role mapping 오류로 권한이 과다 부여될 수 있다. |
| 책임자 | Product owner가 범위·우선순위, Security owner가 권한, Service owner가 SLO를 승인한다. |

## 기능 요구사항

각 ID는 하나의 안정적인 식별자입니다. 문장이 바뀌어도 ID를 재사용하면 설계·test·운영 지표에서 변경을 따라갈 수 있습니다.

### REQ-001 요청 등록

인증된 직원은 제목, 설명, 우선순위로 업무 요청을 등록할 수 있다. System은 변경 불가능한 `requestId`, 생성 시각, 요청자 ID, 초기 상태 `OPEN`, version `1`을 반환한다.

### REQ-002 담당자 배정

`OPERATOR` role은 존재하는 직원 한 명을 요청의 담당자로 배정하거나 교체할 수 있다. System은 이전·새 담당자, 수행자, 시각을 감사 event로 기록한다.

### REQ-003 상태 변경

현재 담당자 또는 `OPERATOR` role은 다음 전이만 수행할 수 있다.

```text
OPEN → IN_PROGRESS
IN_PROGRESS → DONE
IN_PROGRESS → BLOCKED
BLOCKED → IN_PROGRESS
```

`DONE`은 terminal state이며, 허용되지 않은 전이는 data를 변경하지 않고 domain error `INVALID_STATUS_TRANSITION`을 반환한다.

### REQ-004 목록 조회

인증된 직원은 자신이 요청했거나 담당한 업무를 최신 생성 순으로 조회할 수 있다. `status`, `assigneeId`, 생성일 범위 filter와 cursor pagination을 지원한다.

## 품질 요구사항: Quality scenario로 쓰기

품질은 “빠르게, 안전하게, 안정적으로”라고 쓰지 않습니다. 자극원, 환경, 대상, response, 측정값을 함께 씁니다.

### NFR-001 조회 latency

- Source·stimulus: 업무 시간의 인증된 사용자 100명이 목록 조회
- Environment: 최근 90일 요청 10만 건, 정상 운영 상태
- Artifact: `GET /requests`
- Response: 권한 filter와 pagination을 적용해 결과 반환
- Measure: API gateway 기준 월간 p95 server latency 500ms 이하, p99 1초 이하
- Verification: production-equivalent dataset의 load test와 운영 metric

### NFR-002 감사 추적

- Source·stimulus: 담당자 또는 상태가 바뀜
- Environment: 정상·재시도 상황
- Artifact: `AuditEvent`
- Response: 대상 ID, 이전·새 값, actor ID, timestamp, correlation ID 기록
- Measure: 성공한 변경의 100%가 같은 transaction outcome에 대응하는 감사 event를 가지며 1년 보관
- Verification: integration test와 일일 누락 reconciliation

### NFR-003 인증·인가

- Source·stimulus: API 요청이 access token을 제시
- Environment: 정상 운영과 만료·위조 token 상황
- Artifact: 모든 `/requests` endpoint
- Response: identity provider의 서명·issuer·audience·expiry를 확인하고 role 및 object-level policy 적용
- Measure: 인증되지 않은 요청은 data 변경 없이 `401`, 권한 없는 요청은 `403`; access token과 credential은 application log에 기록하지 않음
- Verification: security integration test와 log sampling

수치는 초안입니다. 실제 traffic, 사용자가 감내하는 지연, 장애 비용, 예산을 근거로 owner가 조정해야 합니다.

## User story, Business rule, Acceptance criteria

### User story

> 담당자로서, 업무 진행 상태를 변경하고 싶다. 그래야 요청자가 현재 상황을 알 수 있다.

User story는 사용자와 가치를 기억하게 하지만 권한, 허용 전이, 동시 수정, error response를 모두 담지 못합니다. 따라서 requirement 전체를 대체하지 않습니다.

### Business rules

- BR-001: 현재 담당자 또는 `OPERATOR`만 상태를 변경한다.
- BR-002: `REQ-003`의 전이 표에 없는 변경은 거부한다.
- BR-003: client가 보낸 `version`이 현재 version과 다르면 동시 수정 충돌로 거부한다.
- BR-004: 성공한 상태 변경은 `NFR-002` 감사 event와 함께 확정된다.

### Acceptance criteria

| ID | Given | When | Then |
|---|---|---|---|
| AC-003-01 | Given 담당자이고 요청 상태가 `OPEN`, version이 `3`일 때 | When `IN_PROGRESS`, version `3`으로 변경하면 | Then 상태와 version `4`, 감사 event를 반환한다. |
| AC-003-02 | Given 요청자가 담당자도 운영자도 아닐 때 | When 상태 변경을 요청하면 | Then `403 FORBIDDEN`을 반환하고 상태·감사 data를 바꾸지 않는다. |
| AC-003-03 | Given 상태가 `DONE`일 때 | When `IN_PROGRESS`로 변경하면 | Then `409 INVALID_STATUS_TRANSITION`을 반환하고 data를 바꾸지 않는다. |
| AC-003-04 | Given 현재 version이 `4`일 때 | When version `3`으로 변경하면 | Then `409 VERSION_CONFLICT`와 현재 version을 반환한다. |

이 표는 Given–When–Then을 사람이 읽는 예제로 쓴 것입니다. 자동 test에서는 fixture와 API 호출로 같은 조건을 실행 가능하게 만듭니다.

## 요구사항 review 질문

- 각 문장이 “system은 …한다” 또는 관찰 가능한 품질 response로 쓰였는가?
- 해결 방법을 성급하게 고정한 문장이 있는가?
- 용어와 상태 값이 다른 문서와 같은가?
- 정상뿐 아니라 권한 실패·잘못된 입력·동시 수정이 있는가?
- NFR에 workload, 환경, 측정 지점과 threshold가 있는가?
- acceptance test와 운영 metric으로 실제 확인할 수 있는가?
- scope 밖의 항목과 책임자가 명시됐는가?

## 승인과 변경 관리

승인은 문서를 영원히 동결하는 절차가 아니라 특정 version의 baseline을 만든 기록입니다.

```yaml
requirement_set: work-request-api
version: 1.0
approved_at: 2026-08-02
owners:
  product: product-owner
  security: security-owner
  service: service-owner
change:
  id: CR-007
  reason: "BLOCKED 상태와 복귀 흐름이 필요함"
  affected: [REQ-003, ADR-001, API-STATUS, TEST-003]
  decision: approved
```

변경 요청에는 최소한 이유, 영향받는 ID, 대안, 승인자, 적용 version을 남깁니다. 긴 문서보다 작은 trace link가 change impact 분석에 더 유용할 수 있습니다.

## 복사해서 시작하는 빈 양식

```markdown
# 기능 이름
- 문제:
- 목표 outcome·metric:
- 사용자·stakeholder:
- 범위 / 제외:
- 가정·제약·위험:

## Functional requirements
- REQ-001:

## Quality scenarios
- NFR-001: source/stimulus, environment, artifact, response, measure, verification

## Acceptance criteria
- AC-001-01: Given ..., When ..., Then ...

## Approval & change
- version / owner / change ID / affected trace IDs:
```

## 핵심 정리

1. 좋은 요구사항은 solution 설명보다 문제·outcome·검증 기준을 먼저 고정합니다.
2. 기능 요구사항은 행위를, quality scenario는 조건과 측정 가능한 품질을 정의합니다.
3. User story에는 business rule과 acceptance criteria가 함께 필요합니다.
4. Requirement ID와 change record가 설계·test·운영까지 이어지는 추적성의 시작입니다.

## 참고 자료

- [ISO/IEC/IEEE 29148:2018 — Requirements engineering](https://www.iso.org/standard/72089.html)
- [IEEE Computer Society — SWEBOK](https://www.computer.org/education/bodies-of-knowledge/software-engineering)
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
