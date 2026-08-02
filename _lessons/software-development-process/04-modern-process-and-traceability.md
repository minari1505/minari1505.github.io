---
title: "Modern Process and Traceability"
title_ko: "현대적 개발 프로세스와 추적성"
course: software-development-process
lesson: 4
tags:
  - Software Process
  - Traceability
  - DDD
  - AI Coding
---

## 학습 목표

- phase-gate, waterfall, iterative, incremental, agile의 차이를 feedback 구조로 비교하기
- requirement→decision→component→test→운영 signal을 graph로 추적하기
- architecture와 detailed design의 문서 깊이를 risk로 결정하기
- AI coding에서 specification과 executable evidence를 control loop로 사용하기

## ELI15: 긴 여행 계획과 동네 산책은 다르게 준비한다

학교 앞 문구점에 갈 때 30쪽 계획서는 필요 없습니다. 여러 나라를 거쳐 의약품을 운송한다면 경로, 온도, 책임자, 사고 대응 기록이 필요합니다.

Software도 같습니다. 모든 project에 같은 process를 씌우는 대신 다음을 봅니다.

- 결정을 되돌리기 얼마나 어려운가?
- 틀리면 몇 명과 몇 system이 영향을 받는가?
- feedback을 얼마나 빨리 받을 수 있는가?
- 법·계약·보안상 어떤 evidence가 필요한가?

Process의 목표는 문서 생산량이 아니라 위험한 가정을 늦기 전에 드러내는 것입니다.

## ELIPhD 1: Process model은 feedback topology다

| Model | Work 흐름 | Feedback latency | Batch size | Governance 특징 |
|---|---|---:|---:|---|
| phase-gate | 단계별 기준을 만족해야 다음 단계 진입 | gate 주기에 좌우 | 중~대 | 승인·evidence가 명확 |
| waterfall | 정의된 phase를 주로 순차 수행 | 대체로 김 | 큼 | 계약·계획 baseline에 유리 |
| iterative | 같은 solution을 반복해 정교화 | 짧게 설계 가능 | 작음 | 학습마다 기존 artifact 수정 |
| incremental | usable capability를 조각별 제공 | increment마다 발생 | 작음 | 가치와 integration을 조기 검증 |
| agile | 짧은 iteration·협업·변화 대응 원칙 | 의도적으로 짧음 | 작음 | team autonomy와 지속적 prioritization |

이 용어는 배타적이지 않습니다. 규제 제품도 상위 level phase-gate 안에서 iterative·incremental하게 개발할 수 있습니다. Scrum을 쓴다고 자동으로 feedback이 빨라지지도 않습니다. 큰 story, 느린 review, 수동 test, 늦은 배포가 남으면 실제 batch size와 feedback latency는 큽니다.

변경 비용을 단순화하면 다음 요소의 함수로 볼 수 있습니다.

```text
change cost ≈ affected scope × coordination delay × rework probability × verification cost
```

초기 요구사항이 틀렸을 때 prototype에서 하루 만에 발견하는 것과 production migration 뒤에 발견하는 비용은 다릅니다. 현대적 process는 모든 결정을 미루는 것이 아니라 비싼 가정부터 작은 experiment로 검증합니다.

## ELIPhD 2: Requirements engineering은 한 번의 작성 단계가 아니다

Requirements engineering은 보통 다음 활동이 반복됩니다.

1. Elicitation: stakeholder, 현장, data에서 필요와 제약을 찾는다.
2. Analysis: 충돌, 우선순위, feasibility, risk를 분석한다.
3. Specification: 명확하고 식별 가능한 형태로 기록한다.
4. Validation: 실제 필요와 일치하고 test 가능한지 확인한다.
5. Management: baseline, version, 변경 이유와 trace를 관리한다.

따라서 “요구사항 정의 완료”는 질문이 끝났다는 뜻이 아니라 승인된 현재 baseline이 있다는 뜻입니다. 운영 metric이 가정을 반박하면 다시 elicitation과 analysis로 돌아갑니다.

## Traceability(추적성)를 graph로 보기

여기서 traceability는 요구사항과 구현·검증 evidence 사이의 관계를 양방향으로 따라갈 수 있는 성질입니다.

2·3강의 artifact는 다음 directed graph를 이룹니다.

```text
[Outcome: 1일 내 배정률]
          │ satisfies
          ▼
      [REQ-002] ──drives──► [Assignment use case]
          │                         │
       audited by                verified by
          ▼                         ▼
      [NFR-002] ──realized by──► [Outbox] ──► [AUDIT-001]

[REQ-003] ──decided by──► [ADR-001] ──implemented by──► [Status service]
    │                                                    │
    └──────────────verified by──────────────────────► [TEST-003]
                                                           │
                                                     observed by
                                                           ▼
                                                [conflict_rate metric]
```

형식적으로 artifact를 vertex 집합 `V`, 관계를 edge 집합 `E`로 둡니다. 변경 요청 `CR-007`이 `REQ-003`을 바꾸면 outgoing·incoming edge를 traversal해 ADR, endpoint, module, test, dashboard 후보를 찾습니다.

```text
impact(CR-007) = reachable({REQ-003}, relationship whitelist)
```

Graph가 완전하다고 가정하면 안 됩니다. Code ownership, runtime dependency, data lineage처럼 문서 밖 관계가 있을 수 있습니다. Traceability는 impact 후보를 빠르게 만드는 index이며 review와 test를 대체하지 않습니다.

### 최소 trace rule

- Requirement는 최소 한 개의 verification evidence에 연결합니다.
- 중요한 design decision은 자신이 만족시키는 REQ/NFR을 가리킵니다.
- Test는 requirement ID와 endpoint 또는 module을 가리킵니다.
- 운영 alert는 보호하는 NFR/SLO와 owner를 가리킵니다.
- 폐기된 artifact는 삭제 대신 상태와 대체 관계를 기록합니다.

## Architecture와 Detailed design의 깊이 선택

모든 class를 diagram으로 그리거나 아무것도 기록하지 않는 양극단을 피합니다.

| 판단 축 | 낮은 경우 | 높은 경우 |
|---|---|---|
| Reversibility | 쉽게 rollback·교체 | data format·public API처럼 되돌리기 어려움 |
| Blast radius | 한 module·한 사람 | 여러 service·고객·조직 |
| Coordination cost | 한 team 안의 변경 | 여러 team·vendor·승인 기관 |
| Novelty | 익숙한 pattern | 검증되지 않은 기술·domain |
| Compliance | 일반 내부 도구 | 안전·금융·개인정보 evidence 필요 |

오른쪽에 가까울수록 context, 대안, contract, failure mode, migration, 검증 계획을 더 명확히 남깁니다. 단순하고 reversible한 local implementation detail은 code, type, focused test로 충분할 수 있습니다.

## DDD와 Agile이 실제로 말하는 것

Domain-Driven Design의 핵심은 folder 이름을 `domain/`으로 바꾸는 것이 아닙니다. Domain expert와 개발자가 ubiquitous language를 만들고, model과 code가 같은 개념·규칙을 표현하며, model이 달라지는 경계에 bounded context를 두는 접근입니다.

우리 예제에서 `OPEN → IN_PROGRESS` 규칙이 문서, API enum, domain service, test에 같은 언어로 나타나는 것이 model-code alignment입니다. 모든 CRUD system에 aggregate와 event sourcing을 강제하는 것은 DDD의 목표가 아닙니다.

Agile Manifesto 역시 plan·process·documentation을 버리라고 하지 않습니다. 오른쪽 항목에도 가치가 있지만 왼쪽 항목을 더 중시한다고 말합니다. 즉 필요한 evidence를 유지하면서 작동하는 software와 feedback에 더 빨리 연결해야 합니다.

## 원문 예제를 그대로 복사하면 위험한 부분

일본어 원문은 전체 흐름을 빠르게 보는 지도에는 유용하지만, 일부 sample과 FAQ가 AI로 생성되었고 project별 차이가 있다고 저자가 밝힙니다. 다음은 그대로 채택하지 않고 바꿔야 합니다.

### Credential을 application table에 단순 문자열로 설계하지 않는다

가능하면 검증된 corporate identity provider에 인증을 맡기고 application은 stable subject ID만 저장합니다. 직접 password 인증을 운영해야 한다면 OWASP 권고의 modern password hashing, salt, work factor, secret 관리, 재설정·MFA·rate limit까지 별도 threat model로 설계합니다. Column 길이 하나로 보안 요구가 끝나지 않습니다.

### Normalization과 performance는 별개 축으로 측정한다

Normalization은 redundancy와 update anomaly를 줄입니다. Join이 늘거나 access pattern과 맞지 않으면 read cost가 커질 수도 있습니다. Consistency 요구로 logical model을 정한 뒤 query plan·index·workload를 측정하고, 필요할 때 cache·materialized view·denormalization을 명시적으로 선택합니다.

### Availability 숫자는 표에서 고르는 등급이 아니다

가용성 목표는 사용자 영향, 허용 downtime, dependency capability, 운영 인력, 비용에서 나옵니다. Measurement window, planned maintenance, partial failure, error budget을 함께 정의해야 `99.9%`가 engineering decision이 됩니다.

### DDD·Agile은 상세 설계를 생략하는 면허가 아니다

설계의 형태가 upfront 문서에서 code·test·ADR·짧은 diagram으로 이동할 수는 있습니다. 그러나 transaction boundary, security policy, retry semantics처럼 잘못되면 비싼 결정은 여전히 명시하고 review해야 합니다.

## AI coding을 위한 specification control loop

AI가 code를 빠르게 생성할수록 “무엇이 맞는가”를 판단할 oracle이 더 중요합니다.

```text
Requirement ID
   ↓
API schema + invariant + acceptance example
   ↓
AI가 작은 change 생성
   ↓
format/type/unit/contract/security test
   ↓
human review: intent·threat·migration·operability
   ↓
staging/production telemetry
   └────────── feedback ──────────► requirement·design update
```

### AI에게 주는 작업 단위 예시

```text
목표: REQ-003 상태 변경 endpoint 구현
Contract: OpenAPI operationId changeRequestStatus
Rules: BR-001~004, ADR-001
완료 조건: AC-003-01~04와 TEST-003 통과
금지: 인증 우회, token logging, schema 임의 변경
변경 범위: status application/domain/adapter와 관련 test
출력: 변경 파일, 선택한 대안, 실행한 검증, 남은 risk
```

Specification은 prompt 한 번으로 고정되는 진실이 아닙니다. Generated code가 드러낸 ambiguity를 requirement와 ADR에 되돌리고, executable acceptance test와 contract test를 regression oracle로 유지합니다. Human review는 business intent, security boundary, migration과 운영 가능성을 책임집니다.

## Process 선택 checklist

- [ ] Decision을 되돌릴 수 있는가? Data·public contract migration이 필요한가?
- [ ] 실패의 blast radius와 규제·계약 evidence는 어느 정도인가?
- [ ] Team 간 handoff와 ownership 경계가 몇 개인가?
- [ ] 사용자·운영 feedback을 며칠 안에 받을 수 있는가?
- [ ] Change frequency와 배포 빈도에 맞게 문서를 갱신할 수 있는가?
- [ ] Requirement마다 test나 운영 evidence가 연결되는가?
- [ ] Architecture decision의 context·대안·결과가 남아 있는가?
- [ ] AI가 만든 code를 판정할 executable contract와 review owner가 있는가?

## 핵심 정리

1. Process model의 실질적인 차이는 이름보다 batch size, feedback latency, governance에 있습니다.
2. Traceability graph는 변경된 requirement에서 design·code·test·운영 signal의 영향 후보를 찾게 합니다.
3. 문서 깊이는 reversibility, blast radius, coordination, novelty, compliance로 결정합니다.
4. DDD와 Agile은 설계를 없애지 않고 model과 feedback을 code 가까이 가져옵니다.
5. AI coding에서는 명확한 contract, executable test, human review, observability가 하나의 control loop여야 합니다.

## 참고 자료

- [IEEE Computer Society — SWEBOK](https://www.computer.org/education/bodies-of-knowledge/software-engineering)
- [Manifesto for Agile Software Development](https://agilemanifesto.org/)
- [Principles behind the Agile Manifesto](https://agilemanifesto.org/principles.html)
- [Domain Language — Bounded Context](https://martinfowler.com/bliki/BoundedContext.html)
- [OpenAPI Specification 3.1](https://spec.openapis.org/oas/v3.1.0.html)
- [The C4 model](https://c4model.com/)
- [Michael Nygard — Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [ISO/IEC/IEEE 29148:2018](https://www.iso.org/standard/72089.html)
