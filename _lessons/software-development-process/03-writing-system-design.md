---
title: "Writing System Design"
title_ko: "기본·상세 설계 직접 작성하기"
course: software-development-process
lesson: 3
tags:
  - System Design
  - OpenAPI
  - ADR
  - Traceability
---

## 학습 목표

- 요구사항을 component, data flow, trust boundary로 변환하기
- OpenAPI 3.1로 핵심 endpoint contract 작성하기
- transaction·동시성·재시도·error·logging을 구현 전에 결정하기
- ADR과 traceability matrix로 결정 근거와 검증 연결하기

## ELI15: 기본 설계는 도시 지도, 상세 설계는 건물 도면

도시 지도에는 주거 구역, 병원, 도로처럼 큰 책임과 연결이 보입니다. 건물 도면에는 출입문, 배관, 전기선처럼 실제 시공에 필요한 contract가 보입니다.

- 기본 설계: API, identity provider, database, audit sink가 왜 나뉘며 data가 어디로 흐르는가?
- 상세 설계: endpoint payload, table invariant, transaction, error, retry가 정확히 어떻게 동작하는가?

경계는 조직마다 다릅니다. 중요한 것은 구현자와 reviewer가 같은 결정을 보고 test 가능한 code를 만들 수 있는 수준입니다.

## 1. Context·Component·Trust boundary

```text
[Employee Client]
       │ HTTPS + access token
       ▼
┌──────────────── trust boundary ────────────────┐
│ [Work Request API] ───────► [PostgreSQL]       │
│         │                                      │
│         └── committed event ─► [Audit Sink]    │
└────────────────────────────────────────────────┘
       │ token verification metadata
       ▼
[Corporate Identity Provider]
```

| Component | 책임 | 저장하거나 신뢰하는 data |
|---|---|---|
| Employee Client | 입력·조회 UI, access token 전달 | 장기 credential 저장 안 함 |
| Work Request API | 인증·인가, 상태 전이, validation, transaction | request 처리 중 identity context |
| PostgreSQL | `WorkRequest`, `StatusTransition`, outbox record | 업무 data의 authoritative state |
| Identity Provider | 사용자 인증, token 발급, signing key 제공 | password·MFA 등 credential은 이 경계 안에서 관리 |
| Audit Sink | 변경 event의 검색·장기 보관 | `AuditEvent`; access token은 받지 않음 |

API는 사용자 password를 저장하거나 직접 비교하지 않습니다. Identity provider의 검증된 token claim을 사용하되, 대상 request에 접근 가능한지는 API가 매 요청마다 확인합니다.

## 2. OpenAPI 3.1 contract

다음은 세 endpoint가 공유할 수 있는 축약된 실행 가능 초안입니다. `openapi.yaml`로 저장해 validator와 mock server에서 사용할 수 있습니다.

```yaml
openapi: 3.1.0
info:
  title: Work Request API
  version: 1.0.0
servers:
  - url: /v1
security:
  - bearerAuth: []
paths:
  /requests:
    post:
      operationId: createRequest
      x-requirement-id: REQ-001
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateRequest'
      responses:
        '201':
          description: Created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/WorkRequest'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
    get:
      operationId: listRequests
      x-requirement-id: REQ-004
      parameters:
        - name: status
          in: query
          schema:
            $ref: '#/components/schemas/RequestStatus'
        - name: assigneeId
          in: query
          schema:
            type: string
            format: uuid
        - name: cursor
          in: query
          schema:
            type: string
      responses:
        '200':
          description: A page visible to the caller
          content:
            application/json:
              schema:
                type: object
                required: [items]
                properties:
                  items:
                    type: array
                    items:
                      $ref: '#/components/schemas/WorkRequest'
                  nextCursor:
                    type: [string, 'null']
        '401':
          $ref: '#/components/responses/Unauthorized'
  /requests/{requestId}/status:
    patch:
      operationId: changeRequestStatus
      x-requirement-id: REQ-003
      parameters:
        - name: requestId
          in: path
          required: true
          schema:
            type: string
            format: uuid
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [status, version]
              properties:
                status:
                  $ref: '#/components/schemas/RequestStatus'
                version:
                  type: integer
                  minimum: 1
      responses:
        '200':
          description: Updated
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/WorkRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          description: Caller cannot change this request
        '409':
          description: Invalid transition or version conflict
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
  schemas:
    CreateRequest:
      type: object
      additionalProperties: false
      required: [title, description, priority]
      properties:
        title:
          type: string
          minLength: 1
          maxLength: 120
        description:
          type: string
          minLength: 1
          maxLength: 5000
        priority:
          type: string
          enum: [LOW, NORMAL, HIGH]
    RequestStatus:
      type: string
      enum: [OPEN, IN_PROGRESS, BLOCKED, DONE]
    WorkRequest:
      type: object
      required: [requestId, requesterId, title, priority, status, version, createdAt]
      properties:
        requestId: {type: string, format: uuid}
        requesterId: {type: string, format: uuid}
        assigneeId: {type: [string, 'null'], format: uuid}
        title: {type: string}
        description: {type: string}
        priority: {type: string, enum: [LOW, NORMAL, HIGH]}
        status: {$ref: '#/components/schemas/RequestStatus'}
        version: {type: integer, minimum: 1}
        createdAt: {type: string, format: date-time}
    Problem:
      type: object
      required: [code, message, correlationId]
      properties:
        code: {type: string}
        message: {type: string}
        correlationId: {type: string}
  responses:
    BadRequest:
      description: Invalid request
    Unauthorized:
      description: Missing or invalid access token
```

`servers`는 재사용 가능한 상대 경로로 두고 배포 환경에서 실제 host를 제공합니다. 문서에서 `POST /requests`, `PATCH /requests/{requestId}/status`, `GET /requests`라고 부르는 세 contract가 이제 validator·client generation·contract test의 입력이 됩니다. 담당자 배정 endpoint는 같은 방식으로 `REQ-002`를 확장합니다.

## 3. Data model과 invariant

### WorkRequest

```text
request_id UUID PK
requester_id UUID NOT NULL
assignee_id UUID NULL
title VARCHAR(120) NOT NULL
description TEXT NOT NULL
priority ENUM(LOW, NORMAL, HIGH) NOT NULL
status ENUM(OPEN, IN_PROGRESS, BLOCKED, DONE) NOT NULL
version INTEGER NOT NULL CHECK(version >= 1)
created_at TIMESTAMPTZ NOT NULL
updated_at TIMESTAMPTZ NOT NULL
```

### StatusTransition

```text
transition_id UUID PK
request_id UUID FK → WorkRequest
from_status, to_status
actor_id UUID
request_version INTEGER
correlation_id VARCHAR(64)
created_at TIMESTAMPTZ
UNIQUE(request_id, request_version)
```

### AuditEvent

Audit sink로 전달하는 immutable event입니다. `eventId`, `eventType`, `subjectId`, `actorId`, 이전·새 값, `occurredAt`, `correlationId`를 가집니다. Credential, access token, 요청 본문의 민감한 자유 입력은 복제하지 않습니다.

Normalization은 중복과 update anomaly를 줄이는 설계 도구입니다. Query 성능을 항상 높인다는 법칙은 아닙니다. 실제 access pattern, index, join cost, consistency 요구를 측정하고 필요한 경우 materialized view나 의도적인 denormalization을 선택합니다.

## 4. ADR-001: 상태 전이는 domain service에서 검증한다

```text
Status: Accepted
Context: 상태 변경 규칙이 API, batch, 향후 event consumer에서 동일해야 한다.
Decision: WorkRequest domain service가 현재 상태·목표 상태·actor policy를 검증한다.
          Database constraint는 enum과 version invariant를 방어한다.
Alternatives:
  A. Controller에서 검증 — 시작은 단순하지만 entry point마다 규칙이 복제된다.
  B. DB trigger에서 모두 검증 — 중앙화되지만 application test와 변경 가시성이 낮다.
Consequences:
  + 모든 entry point가 한 정책을 사용한다.
  + unit test로 상태표를 빠르게 검증한다.
  - Database를 우회하는 write path를 금지하고 code review로 통제해야 한다.
```

ADR은 “무엇을 선택했나”뿐 아니라 context, 대안, 결과를 남깁니다. 상황이 변하면 기존 ADR을 지우지 않고 새 ADR로 supersede합니다.

## 5. 상세 contract: 상태 변경 use case

```text
change_status(request_id, actor, expected_version, target_status) -> WorkRequest

1. token 검증 결과에서 actor ID·role을 받는다.
2. transaction을 시작하고 WorkRequest를 조회한다.
3. 담당자 또는 OPERATOR인지 object-level authorization을 확인한다.
4. ADR-001 상태표로 transition을 검증한다.
5. UPDATE ... WHERE request_id=? AND version=? 로 optimistic concurrency를 적용한다.
6. 영향 row가 0이면 VERSION_CONFLICT를 반환한다.
7. StatusTransition과 outbox AuditEvent를 같은 transaction에 기록한다.
8. commit 후 outbox worker가 Audit Sink로 전달한다.
```

### Retry와 idempotency

- Client는 timeout 뒤 현재 resource를 조회한 후 재시도합니다.
- 요청 등록에는 `Idempotency-Key` 도입을 검토하고 `(actor_id, key)`를 unique하게 저장합니다.
- Status update는 `expected_version` 덕분에 같은 변경이 중복 확정되지 않습니다.
- Outbox worker는 at-least-once delivery를 사용하고 Audit Sink는 `eventId`로 deduplicate합니다.

### Error mapping

| Domain 결과 | HTTP | Code |
|---|---:|---|
| token 없음·무효 | 401 | `UNAUTHENTICATED` |
| actor 권한 없음 | 403 | `FORBIDDEN` |
| request 없음 | 404 | `REQUEST_NOT_FOUND` |
| 상태 전이 오류 | 409 | `INVALID_STATUS_TRANSITION` |
| version 불일치 | 409 | `VERSION_CONFLICT` |
| 형식 오류 | 400 | `VALIDATION_ERROR` |

Error message에는 내부 stack이나 SQL을 노출하지 않습니다. Log에는 `correlationId`, requirement ID, endpoint, actor의 내부 ID, 결과 code, latency를 구조화해 남기되 token과 자유 입력 본문은 제외합니다.

## 6. Traceability matrix

| Requirement | Design decision·component | Contract·module | Test·operation evidence |
|---|---|---|---|
| REQ-001 | API + WorkRequest | `POST /requests` | TEST-001 contract·integration |
| REQ-002 | authorization + audit outbox | assignment use case | TEST-002 role·audit integration |
| REQ-003 | ADR-001 + optimistic concurrency | `PATCH /requests/{requestId}/status` | TEST-003 transition table·conflict |
| REQ-004 | access policy + cursor index | `GET /requests` | TEST-004 visibility·pagination |
| NFR-001 | query index + telemetry | list query | PERF-001 p95/p99 dashboard |
| NFR-002 | transaction + outbox + Audit Sink | `StatusTransition`, `AuditEvent` | AUDIT-001 reconciliation |
| NFR-003 | Identity Provider + object policy | bearer security scheme | SEC-001 401·403·log scan |

OpenAPI와 migration은 repository에서 version 관리하고 pull request에서 requirement ID를 연결합니다. Contract가 바뀌면 consumer compatibility, matrix, acceptance test를 같은 change set에서 갱신합니다. 그림만 따로 최신화하는 회의를 만드는 것보다 검증 가능한 artifact를 code 가까이에 둡니다.

## 구현 전 review checklist

- [ ] 모든 component의 책임과 trust boundary가 한 문장으로 설명되는가?
- [ ] API schema에 required, enum, length, error가 있는가?
- [ ] Transaction boundary와 partial failure 처리가 정해졌는가?
- [ ] 동시 수정과 retry가 중복 변경을 만들지 않는가?
- [ ] Authorization을 endpoint뿐 아니라 object 단위로 확인하는가?
- [ ] Log와 audit event에서 credential·민감 data를 제외했는가?
- [ ] REQ/NFR이 test 또는 운영 evidence에 연결되는가?

## 핵심 정리

1. 기본 설계는 책임과 경계를, 상세 설계는 구현 가능한 contract와 실패 동작을 다룹니다.
2. OpenAPI, ADR, migration, test는 서로 연결될 때 살아 있는 설계가 됩니다.
3. Transaction, optimistic concurrency, idempotency, outbox는 정상 경로보다 실패·재시도에서 의미가 드러납니다.
4. Traceability matrix는 문서 장식이 아니라 변경 영향과 검증 누락을 찾는 index입니다.

## 참고 자료

- [OpenAPI Specification 3.1](https://spec.openapis.org/oas/v3.1.0.html)
- [The C4 model for visualising software architecture](https://c4model.com/)
- [Michael Nygard — Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
