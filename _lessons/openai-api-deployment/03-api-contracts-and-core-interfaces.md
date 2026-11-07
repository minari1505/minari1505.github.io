---
title: "API Contracts and Core Interfaces"
title_ko: "API 계약과 핵심 인터페이스 — Responses · Chat Completions · Conversations"
course: openai-api-deployment
lesson: 3
---

## 학습 목표

- **Responses API**와 **Chat Completions API**(추론 인터페이스), **Conversations API**(대화 상태 저장소)의 역할 구분하기
- 워크플로우의 상태·도구·스트리밍·비동기·출력 요구로 **인터페이스와 상태 방식**을 각각 고르기
- 요청·응답·스키마·인증·구조화 출력·에러를 **테스트 가능한 계약(contract)**으로 정의하기
- 성공·에러·구조화 출력·상태 동작 테스트를 설계하고 **증명한 것 / 못한 것**을 기록하기
- 결과를 **Core Interface and API Contract Plan**으로 묶어 인수인계하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>인터페이스 비교</strong>Responses vs Chat Completions + 상태(Conversations)</li>
  <li><strong>설계 영향 파악</strong>상태·도구·스트리밍·비동기·재시도</li>
  <li><strong>계약 정의</strong>요청 · 응답 · 스키마 · 에러</li>
  <li><strong>검증 테스트</strong>성공 · 실패 · 권한 · 상태</li>
  <li><strong>계약 플랜 완성</strong>인수인계 가능한 한 장</li>
</ol>

예시 시나리오: 기존에 **Chat Completions 기반 고객지원 챗봇**을 쓰던 고객이, 이번엔 **구조화된 케이스 요약**을 만들어 내부 대시보드에 띄우고, 티켓 상태 조회 도구도 쓰고, 같은 케이스에 대한 후속 질문도 받고 싶어 해요.

## 1. 계약(contract)이란?

<div class="oa-eli">
  <span class="oa-eli__head">📦 쉽게 말하면</span>
  <p>택배 접수 창구를 떠올려 보세요. "<b>무엇을 내야 하는지</b>(주소·무게·내용물), <b>무엇을 받는지</b>(송장번호), <b>문제가 생기면 어떻게 되는지</b>(주소 오류면 반송)"가 정해져 있죠. 이 약속이 있어야 누구든 같은 방식으로 이용하고, 잘못됐을 때 바로 알 수 있어요.</p>
  <p>API 계약도 똑같아요. <b>보낼 것 · 받을 것 · 실패 시 동작 · 확인 방법</b>을 미리 적어 두는 약속이에요.</p>
</div>

## 2. 세 가지 이름, 두 가지 역할

가장 헷갈리는 부분부터 정리해요. **Conversations API는 세 번째 추론 인터페이스가 아니에요.**

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍🍳 쉽게 말하면</span>
  <p><b>Responses API</b>는 요즘 새로 지은 주방(새 프로젝트 권장), <b>Chat Completions</b>는 아직 잘 돌아가는 예전 주방(기존 시스템 호환), <b>Conversations API</b>는 주방이 아니라 <b>단골 손님 장부</b>예요. 장부는 요리를 하지 않지만, "이 손님이 지난번에 뭘 주문했는지"를 기억해 줘요. 그리고 이 장부는 새 주방(Responses)과 함께 써요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">⭐</span><span class="oa-card__title">Responses API</span><b>추론 인터페이스</b> · 새 프로젝트의 권장 출발점. 구조화 출력, 도구, 멀티모달·에이전트 패턴 지원. <code>previous_response_id</code>로 턴 연결도 가능</div>
  <div class="oa-card"><span class="oa-card__icon">🗂️</span><span class="oa-card__title">Chat Completions API</span><b>추론 인터페이스</b> · 여전히 지원됨. 기존 구현·레거시 통합에서 자주 보임 → 새 설계의 기본값이 아니라 <b>호환/마이그레이션 제약</b>으로 취급</div>
  <div class="oa-card"><span class="oa-card__icon">📒</span><span class="oa-card__title">Conversations API</span><b>상태 저장소</b> · Responses와 함께 쓰는 지속 대화 객체(고유 ID). 세션·기기·작업을 넘어 문맥 유지가 정말 필요할 때만</div>
</div>

### 인터페이스 결정표

| 대상 | 이럴 때 | 빌드 전 확인할 것 |
|---|---|---|
| **Responses API** | 새 프로젝트, 구조화 출력·도구 등 현재 문서화된 기능 필요 | 필요한 기능 지원 여부, 상태 방식, 스트리밍/백그라운드, SDK 예시, 제한 |
| **Chat Completions API** | 기존 구현 유지·호환 요구 | 고객 제약, 마이그레이션 영향, 수동 상태 관리, 지원 가이드 |
| **Conversations API** (+Responses) | 세션·기기·작업을 넘는 **지속** 대화 상태 | 상태 소유자, 저장·보존 기간, 한도, 개인정보, 복구 동작 |

결정 순서는 **① 추론 인터페이스 먼저 → ② 상태 방식은 따로**. 둘은 관련 있지만 별개의 계약 결정이에요.

### 비슷해 보이지만 다른 선택지 두 개

<div class="oa-vs">
  <div>
    <span class="oa-vs__label">🤖 Agents API = 관리형 실행(runtime)</span>
    <ul>
      <li>도구를 쓰는 실행을 <b>세션 안에서 관리</b>해 줌 (Conversations의 다른 이름 아님)</li>
      <li>세션·이벤트 생명주기가 계약에 들어가야 함</li>
      <li>현재 <b>US 데이터 레지던시만</b>, ZDR 대상 아님 → 도입 전 확인</li>
    </ul>
  </div>
  <div>
    <span class="oa-vs__label">🎯 Decisions API = 타입이 정해진 예측 (public beta)</span>
    <ul>
      <li>"이 티켓은 교체 요청인가?" 같은 <b>고정된 질문</b>에 predicate·choice·score로 답</li>
      <li>요약문을 쓰거나 업무 행동을 실행하지 않음</li>
      <li>"둘 다 구조화된 값을 준다"가 선택 이유가 되면 안 됨</li>
    </ul>
  </div>
</div>

## 3. 인터페이스 선택이 설계 전체를 바꾼다

인터페이스 이름만 정하고 끝내면 안 돼요. 아래 항목들이 줄줄이 따라 바뀝니다.

<div class="oa-svg">
<svg viewBox="0 0 720 260" role="img" aria-label="인터페이스 선택이 영향을 주는 7가지 설계 요소">
  <rect x="270" y="105" width="180" height="50" rx="12" fill="#ffdd7a"/>
  <text x="360" y="128" text-anchor="middle" font-size="15" font-weight="700" fill="#4a3a00">인터페이스</text>
  <text x="360" y="146" text-anchor="middle" font-size="12" fill="#4a3a00">+ 상태 방식 선택</text>
  <g font-size="13" fill="#4a4a4a" text-anchor="middle">
    <rect x="20" y="20" width="150" height="38" rx="10" fill="#fffdf5" stroke="#f1e4b8"/><text x="95" y="44">상태 · 컨텍스트</text>
    <rect x="285" y="10" width="150" height="38" rx="10" fill="#fffdf5" stroke="#f1e4b8"/><text x="360" y="34">도구 · 행동</text>
    <rect x="550" y="20" width="150" height="38" rx="10" fill="#fffdf5" stroke="#f1e4b8"/><text x="625" y="44">스트리밍</text>
    <rect x="20" y="200" width="150" height="38" rx="10" fill="#fffdf5" stroke="#f1e4b8"/><text x="95" y="224">비동기 · 대기</text>
    <rect x="285" y="212" width="150" height="38" rx="10" fill="#fffdf5" stroke="#f1e4b8"/><text x="360" y="236">구조화 출력</text>
    <rect x="550" y="200" width="150" height="38" rx="10" fill="#fffdf5" stroke="#f1e4b8"/><text x="625" y="224">재시도 · 멱등성</text>
    <rect x="565" y="110" width="140" height="38" rx="10" fill="#fffdf5" stroke="#f1e4b8"/><text x="635" y="134">에러 · 검증</text>
  </g>
  <g stroke="#c99400" stroke-width="1.5">
    <line x1="270" y1="118" x2="170" y2="45"/><line x1="360" y1="105" x2="360" y2="48"/><line x1="450" y1="118" x2="550" y2="45"/>
    <line x1="270" y1="142" x2="170" y2="215"/><line x1="360" y1="155" x2="360" y2="212"/><line x1="450" y1="142" x2="550" y2="215"/><line x1="450" y1="130" x2="565" y2="129"/>
  </g>
</svg>
</div>

### ① 상태(state)와 컨텍스트

<div class="oa-eli">
  <span class="oa-eli__head">📞 쉽게 말하면</span>
  <p><b>Stateless</b>는 매번 처음 보는 상담원에게 전화하는 것 — 매번 "제 주문번호는…"부터 다 말해야 하지만, 대화가 꼬일 일이 없어요. <b>Stateful</b>은 담당 상담원이 지난 통화를 기억하는 것 — 편하지만 "<b>그 기억이 어디 저장되고, 얼마나 보관되고, 틀리면 어떡하지?</b>"를 정해야 해요.</p>
</div>

| | Stateless 요청 | Stateful 상호작용 |
|---|---|---|
| 특징 | 요청 하나에 필요한 정보가 다 들어 있음 | 턴·작업·세션 사이 연속성에 의존 |
| 장점 | 테스트·거버넌스가 쉬움 | 후속 질문 UX가 좋음 |
| 계약에 적을 것 | 매 요청 필수 필드 (예: `account_id`, `issue_text`, `approved_context_ids`) | 무엇을 · 어디에 · 누가 소유 · 얼마나 보관 · 어떤 정책 · 상태가 없거나/낡았거나/불일치할 때 동작 |
| 테스트 | 숨은 이전 턴 없이 단독으로 동작하는지 | 연속성, 누락·만료·불일치 상태 |

> 저장된 케이스 ID가 있다고 해서 그 케이스에 **접근할 권한까지 생기는 건 아니에요.**

### ② 도구(tool)와 행동(action)

| 계약에 적을 것 | 예: 티켓 상태 조회 도구 |
|---|---|
| 목적 | 현재 티켓 상태 확인 |
| 입력 / 출력 | `ticket_id` → `status` |
| **실행 경계** | 모델은 호출을 *요청*, 실제 실행은 애플리케이션 코드 |
| 권한 | 어떤 시스템·데이터 접근을 가정하는가 |
| 실패 동작 | 도구 장애·불완전 응답 시 어떻게 |
| 승인 | 읽기 전용인가, 시스템을 바꾸는가 |

<div class="oa-vs">
  <div class="oa-vs__good"><span class="oa-vs__label">🔍 읽기 전용 도구</span><ul><li>티켓 상태, 승인된 정책 조회</li><li>위험 낮음</li></ul></div>
  <div class="oa-vs__bad"><span class="oa-vs__label">✍️ 행동하는(action) 워크플로우</span><ul><li>레코드 수정, 메시지 발송, 작업 생성</li><li>더 강한 승인·로깅·검증 필요</li></ul></div>
</div>

Responses API에서는 **앱이 도구 호출을 검증·실행하고 같은 `call_id`로 결과를 돌려줘요.** 이벤트를 *받았다*는 것이 권한이나 완료를 증명하지 않으며, 중복·순서 뒤바뀐 이벤트에도 같은 행동이 두 번 일어나면 안 돼요.

### ③ 스트리밍과 비동기

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📄</span><span class="oa-card__title">한 번에 완성 응답</span>대시보드가 <b>완성된 JSON</b>을 받아야 할 때</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">스트리밍</span>긴 답변을 사람이 <b>조금씩 보는 게</b> 이득일 때. 단, 구조화 결과는 완성 후 검증</div>
  <div class="oa-card"><span class="oa-card__icon">⏳</span><span class="oa-card__title">백그라운드 응답</span>시작해 두고 나중에 상태 확인·결과 회수</div>
  <div class="oa-card"><span class="oa-card__icon">🔀</span><span class="oa-card__title">비동기 도구 호출</span>느린 조회가 도는 동안 모델은 <b>그 결과와 무관한 일만</b> 계속</div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🍕 비동기 도구 예시 — 피자 주문 중 배달 조회</span>
  <p>티켓 상태 조회가 느리다면, 기다리는 동안 모델은 메모를 정리할 수 있지만 <b>상태를 지어내면 안 돼요.</b> 조회가 끝나면 앱이 결과를 <code>function_call_output</code>으로, <b>원래 <code>call_id</code></b>를 붙여 돌려줘요. 조회 실패 시 "빈 상태"를 성공처럼 보여주지 않고 계약의 실패 절차를 따라요.</p>
</div>

**Steering(실행 중 방향 수정)**: 응답이 끝나기 전에 "세 줄로 줄여줘"처럼 지시를 보낼 수 있어요. 핵심은 이벤트를 구분하는 것.

<ol class="oa-flow">
  <li><strong>response.steer</strong>진행 중 응답 ID + 새 지시 전송</li>
  <li><strong>steer.accepted</strong>대기열에 들어갔을 뿐, 반영 확인 아님</li>
  <li><strong>후속(successor) 응답</strong>새 응답 ID를 추적해 실제로 3줄인지 확인</li>
</ol>

`steer.pending`이면 도구 결과·승인이 먼저 필요하고, `steer.failed`면 자동 재적용이 없으니 "바뀌었다"고 말하면 안 돼요. Steering은 **이미 나간 출력이나 이미 시작된 도구를 되돌리지 않아요.**

### ④ 구조화 출력(Structured Outputs)

구조화 출력은 "보기 좋게"가 아니라 **신뢰성·통합 제어 장치**예요.

<div class="oa-vs">
  <div class="oa-vs__bad"><span class="oa-vs__label">JSON mode</span><ul><li>유효한 JSON은 보장</li><li><b>스키마 준수는 보장 안 함</b> → 앱에서 스키마 검증 필요</li></ul></div>
  <div class="oa-vs__good"><span class="oa-vs__label">Structured Outputs (strict)</span><ul><li>지원 스키마 준수를 강제</li><li>단, <b>거절(refusal)·미완성 응답</b>은 별도 처리</li></ul></div>
</div>

> `confidence_note`는 모델이 쓴 **설명 문장**이지, 보정된 신뢰도 점수가 아니에요. 위험도 판단은 결정론적 검사·평가기·사람 리뷰로.

### ⑤ 재시도와 멱등성(idempotency)

<div class="oa-eli">
  <span class="oa-eli__head">🛗 쉽게 말하면</span>
  <p>엘리베이터 버튼은 여러 번 눌러도 한 번만 와요 — 이게 <b>멱등성</b>. 반대로 "송금" 버튼이 두 번 눌리면 두 번 보내지면 큰일이죠. 타임아웃 후 재시도할 때 <b>중복 기록·중복 메시지·중복 고객 행동</b>이 생기지 않게 <code>request_id</code> 같은 중복 제거 장치를 계약에 미리 적어 둬요.</p>
</div>

<div class="oa-bars">
  <div class="oa-bar"><span>읽기 전용 조회</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:20%"></span><span class="oa-bar__val">재시도 위험 낮음</span></span></div>
  <div class="oa-bar"><span>대시보드 항목 생성</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:60%"></span><span class="oa-bar__val">중복 위험 중간</span></span></div>
  <div class="oa-bar"><span>고객 이메일 발송</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:95%"></span><span class="oa-bar__val">중복 위험 높음</span></span></div>
  <p class="oa-bars__caption">막대는 개념적 위험 수준이에요 (강의 수치 아님).</p>
</div>

## 4. 계약 정의 — 요청 · 응답 · 스키마 · 에러

### 요청 계약

| 항목 | 예시 (`POST /case-summary`) |
|---|---|
| 목적 | 계정 매니저용 내부 지원 케이스 요약 생성 |
| 호출자 | 인증된 직원을 대신하는 내부 대시보드 서비스 |
| 필수 필드 | `account_id`, `issue_text`, `approved_context_ids` |
| 선택 필드 | `user_question`, `case_priority_hint` |
| 검증 규칙 | account_id 필수, issue_text 비어있지 않음, context ID는 승인된 소스만 |
| **제외 필드** | 결제 정보 전체, 개인 자격 증명, 운영 secret, 승인 안 된 메모 |

고객 앱의 엔드포인트(`POST /case-summary`)와 그 **뒤에서 쓰는 OpenAI API 인터페이스**는 서로 다른 층이라는 것도 구분해 적어요.

### 응답 계약

| 필드 | 타입 | 의미 |
|---|---|---|
| `issue_summary` | string | 이슈 요약 |
| `urgency` | enum: low / medium / high | 처리 긴급도 |
| `recommended_next_action` | string | 다음 내부 행동 제안 |
| `source_ids` | array (빈 배열 가능) | 승인된 근거 소스. **`[]`이면 근거 없음 → 리뷰 플래그** |
| `confidence_note` | string | 불확실성 설명 (점수 아님) |
| `needs_human_review` | boolean | 사람 검토 필요 여부 |
| `review_reason` | string 또는 null | 리뷰가 true면 반드시 설명 |

### 스키마는 통과했는데 규칙은 어긴 경우

```json
{
  "type": "object",
  "properties": {
    "needs_human_review": {"type": "boolean"},
    "review_reason": {"type": ["string", "null"]}
  },
  "required": ["needs_human_review", "review_reason"],
  "additionalProperties": false
}
```

| 응답 | 스키마 | 앱 규칙 | 결과 |
|---|---|---|---|
| `{"needs_human_review": false, "review_reason": null}` | ✅ | ✅ | 사용 가능 |
| `review_reason` 누락 | ❌ 필수 필드 없음 | — | 실패 |
| `"needs_human_review": "false"` | ❌ 문자열이지 boolean 아님 | — | 실패 |
| `{"needs_human_review": true, "review_reason": null}` | ✅ | ❌ 리뷰인데 이유 없음 | **사람 리뷰로 보냄** |

마지막 줄이 핵심이에요. **스키마 검증 다음에 앱 규칙 검증**이 한 번 더 있어야 해요.

### 에러 상태 — 원인별로 주인이 다르다

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__title">🆔 인증 실패</span>누군지 확인 불가</div>
  <div class="oa-card"><span class="oa-card__title">🚪 인가 실패</span>누군지는 알지만 이 계정 권한 없음 → 데이터 반환 금지, 로그</div>
  <div class="oa-card"><span class="oa-card__title">📝 검증 실패</span>필드 누락·타입·허용값 위반 → 재제출 안내</div>
  <div class="oa-card"><span class="oa-card__title">🔌 의존성 실패</span>소스 접근 불가·타임아웃 → 리뷰 플래그 또는 구조화 실패</div>
  <div class="oa-card"><span class="oa-card__title">🧩 출력 실패</span>필수 필드 생성 불가 → 안전하면 재시도, 아니면 리뷰</div>
  <div class="oa-card"><span class="oa-card__title">🔁 상태 불일치</span>대화 참조가 다른 케이스 → 되묻기 또는 상태 불일치 에러</div>
</div>

에러마다 **조건 · 상태 코드/분류 · 사용자에게 안전한 메시지 · 다음 행동**을 적고, 사용자 메시지엔 내부 정보를 노출하지 않아요. Decisions의 **거절(refusal)**은 false·0·낮은 신뢰도와 다른 별도 결과로 처리해요.

### 모호한 계약 → 테스트 가능한 계약

<div class="oa-vs">
  <div class="oa-vs__bad"><span class="oa-vs__label">❌ 모호함</span><ul><li>"텍스트 좀 보내면 요약해 줘"</li><li>"적절한 컨텍스트 사용"</li><li>"에러는 우아하게 처리"</li></ul></div>
  <div class="oa-vs__good"><span class="oa-vs__label">✅ 테스트 가능</span><ul><li>account_id·issue_text·approved_context_ids를 받아 7개 필드 반환</li><li>승인 컨텍스트가 없으면 근거 있는 척 하지 않고 <code>needs_human_review=true</code> 또는 구조화 실패</li></ul></div>
</div>

## 5. 검증 테스트 설계

<div class="oa-bars">
  <div class="oa-bar"><span>성공 테스트 1개</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:15%"></span><span class="oa-bar__val">해피 패스만</span></span></div>
  <div class="oa-bar"><span>+ 에러·권한 테스트</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:50%"></span><span class="oa-bar__val">실패 경로 커버</span></span></div>
  <div class="oa-bar"><span>+ 구조화 출력·상태</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:80%"></span><span class="oa-bar__val">계약 대부분 커버</span></span></div>
  <p class="oa-bars__caption">개념적 커버리지 비교 — 성공 하나는 준비 완료를 증명하지 않아요.</p>
</div>

| 테스트 | 요청 | 기대 결과 | 증명하는 것 |
|---|---|---|---|
| 정상 요약 | 필수 필드 모두 유효 | 7개 필드, 유효 구조 | 필수 필드 반환 가능 |
| 필드 누락 | `account_id` 없음 | 정의된 누락 에러 | 실행 전 검증 |
| 인증 실패 | 잘못된 인증 | 처리 안 함 + 안전한 에러 | 인증 경계 |
| 인가 실패 | 권한 없는 계정 | 계정 데이터 미반환 | 인증과 인가 분리 |
| 소스 불가 | 승인 컨텍스트 없음 | 지어내지 않음 + 리뷰 | 근거 없는 주장 방지 |
| 구조화 실패 | 거절·미완성 | 통제된 실패 or 리뷰 | 비스키마 경로 처리 |
| 상태 불일치 | 다른 케이스 참조 | 되묻기 / 불일치 에러 | 잘못된 문맥 사용 방지 |
| 중복 재시도 | 타임아웃 후 재전송 | 중복 항목 없음 | 멱등성 요구 |

비동기 도구라면 **잘못되거나 모르는 `call_id`**, 타임아웃도, steering이라면 **accepted → successor 확인 / pending / failed**도 테스트해요. 증거에는 기대값과 실제값을 따로 적고, **실행 안 한 테스트는 pass가 아니에요.**

## 6. Core Interface and API Contract Plan

| 섹션 | 담을 내용 |
|---|---|
| 워크플로우 요구 | 이 API가 지원하는 업무 |
| 추론 인터페이스 + 근거 | Responses / Chat Completions / 기타 — 왜 맞는지 |
| 상태 방식 | stateless · 앱 관리 · chained Responses · Conversations |
| 도구·스트리밍·비동기·재시도 | 실행 주체, 대기 상태, call 매칭, 실패·재시도 조건 |
| 런타임 계약 | 앱 소유 루프 vs 관리형 런타임, 승인·복구·최종 상태 증거 |
| 타입 예측 계약 (선택 시) | 질문·답 타입·거절 경로·임계값 규칙 — 안 쓰면 "미사용" 명시 |
| 구조화 출력 | Structured Outputs vs JSON mode + 앱 검증 |
| 요청 / 응답 / 에러 / 테스트 | 위 4·5절 |
| 열린 질문 | 빌드 전에 확인할 것 |

### 오래가는 설계 vs 금방 바뀌는 사실

<div class="oa-vs">
  <div class="oa-vs__good"><span class="oa-vs__label">🪨 오래가는 설계 논리</span><ul><li>워크플로우 요구, 상태·도구 필요 여부</li><li>요청·응답 필드, 에러 상태, 테스트가 증명할 것</li></ul></div>
  <div class="oa-vs__bad"><span class="oa-vs__label">🌊 변하는 세부사항 (빌드 전 재확인)</span><ul><li>정확한 인터페이스 이름, 기능 조합, 한도</li><li>모델명, SDK 예시, 스트리밍·비동기 동작, 가격</li></ul></div>
</div>

변하는 세부사항은 **문서 확인 날짜와 함께 업데이트 가능한 메모**로 분리해 두면 계획이 오래 정확하게 유지돼요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 기존 Chat Completions 챗봇을 가진 고객이 구조화 요약·읽기 전용 도구·후속 질문을 원한다. 가장 강한 권고는?</summary>
  <p><b>정답:</b> 구조화 출력·도구 요구 때문에 <b>Responses API를 평가</b>하고, Chat Completions는 <b>호환 제약</b>으로 다루며, 지속 상태가 정말 필요할 때만 Responses와 함께 <b>Conversations API</b>를 쓴다.</p>
  <p>✗ Conversations를 추론 인터페이스로 사용 / ✗ Responses가 구조·도구·상태를 자동으로 다 해결한다고 가정 / ✗ 지금 잘 되니 Chat Completions 유지</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 설계 가정이 만드는 계약 요구는?</summary>
  <p>재시도 있는 쓰기 작업 → <b>멱등성</b> 요구 정의<br>타입 예측 → 질문·답 타입·거절 경로·앱 결정 규칙<br>도구 사용 → 도구 입력·결과 처리·권한·실패 동작<br>Stateful 상호작용 → 무엇을·어디에·누가·보존/개인정보 제약<br>관리형 실행 세션 → 대기 중 행동·상관관계·복구·최종 업무 상태 증거</p>
</details>

<details class="oa-quiz">
  <summary>Q3. "프로젝트 업데이트를 팀 트래커용 할 일 목록으로 바꿔줘"라는 인수인계 메모에 빠진 것은?</summary>
  <p><b>정답:</b> 호출자가 보내는 것, API가 돌려줄 할 일 필드, 트래커가 기대하는 구조, 업데이트가 불완전·잘못됐을 때의 동작. (롤아웃 계획, 화면 디자인, 정확한 모델·SDK 버전은 계약의 핵심이 아님)</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 정상 요청 1건이 모든 필드를 잘 반환했다. 계획서엔 뭐라고 쓸까?</summary>
  <p><b>정답:</b> "<b>성공 경로는 검증됨.</b> 누락 필드·무권한 접근·소스 불가·잘못된 구조화 출력·(상태를 쓴다면) 낡은 상태 테스트를 추가한다." — 나머지를 위험으로만 넘기거나 일부만 우선하는 건 계약 검증을 미루는 것.</p>
</details>

<details class="oa-quiz">
  <summary>Q5. 인수인계에 가장 강한 계약 플랜은?</summary>
  <p><b>정답:</b> 검증 대기 상태의 <b>Responses API</b> + 정의된 상태 방식 + 스키마 동작 + 에러 + 테스트 + 열린 질문. (에러를 나중에 정의 ✗ / Conversations를 추론에 사용 ✗ / 필드는 대시보드가 나중에 ✗)</p>
</details>

## 정리

- **추론 인터페이스**(Responses 권장 / Chat Completions 호환)와 **상태 방식**(stateless ~ Conversations)은 따로 결정한다.
- Agents API는 관리형 실행, Decisions API는 타입 예측 — 둘 다 "요약 생성"의 대체물이 아니다.
- 인터페이스 선택은 상태·도구·스트리밍·비동기·구조화 출력·재시도·검증을 모두 바꾼다.
- 계약 = 요청 + 응답 + 스키마 + **앱 규칙** + 에러(원인별) — 테스트할 수 있을 만큼 구체적으로.
- 성공 1건 ≠ 준비 완료. 실패·권한·구조·상태 테스트로 증명하고, 변하는 제품 사실은 빌드 전에 재확인.
