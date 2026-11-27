---
title: "Model Choice and Cost Decisions"
title_ko: "모델 선택과 비용 판단"
course: cca-f-exam-prep
lesson: 2
tags:
  - CCA-F
  - Claude Models
  - Cost Optimization
---

## 학습 목표

- 시나리오를 보고 Opus / Sonnet / Haiku 중 무엇을 고를지 판단하기
- 모델 ID를 프로덕션에서 어떻게 고정하는지 알기
- Messages API 요청 구조와 `stop_reason` 네 가지 값이 에이전트 루프에서 무엇을 뜻하는지 알기
- Batches API, 프롬프트 캐시, temperature, max_tokens에 대한 시험 단골 오해를 걷어 내기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>모델 고르기</strong>요건을 만족하는 가장 싼 모델</li>
  <li><strong>ID 고정</strong>프로덕션은 가장 구체적인 ID</li>
  <li><strong>요청 보내기</strong>system · messages · max_tokens</li>
  <li><strong>응답 읽기</strong><code>stop_reason</code>으로 다음 행동 결정</li>
  <li><strong>비용 줄이기</strong>Batches API · 프롬프트 캐시</li>
</ol>

이 레슨을 한 문장으로 줄이면: **"요건을 만족하는 가장 싼 모델을 고른다"가 모델 선택의 원칙이고, 시험은 이 원칙을 시나리오에 적용할 수 있는지 봐요.**

## 1. 세 모델의 위치

<div class="oa-eli">
  <span class="oa-eli__head">🚚 쉽게 말하면</span>
  <p>편지 한 통을 보내는 데 5톤 트럭을 부르지 않아요. 오토바이 퀵이 더 빠르고 싸요. 반대로 이삿짐을 오토바이로 나를 수는 없죠. 모델 선택도 <b>짐(작업)의 크기에 맞는 가장 작은 차</b>를 고르는 일이에요.</p>
</div>

| 모델 | 추론 능력 | 속도 | 비용 | 잘 맞는 일 |
|---|---|---|---|---|
| Opus | 가장 높음 | 느림 | 높음 | 복잡한 추론, 계획 수립, 오케스트레이터(허브) |
| Sonnet | 중간 | 중간 | 중간 | 일반 애플리케이션, 서브에이전트, 문서 생성 |
| Haiku | 낮음 | 가장 빠름 | 가장 낮음 | 분류, 추출, 대량 배치, 요약 |

이름은 시의 길이에서 왔어요. Opus(장편), Sonnet(14행), Haiku(17음). 규모 순으로 외우면 돼요.

시험에서 자주 섞는 오답: "Haiku는 Opus보다 느리지만 더 복잡한 추론을 한다." 속도와 정확도를 바꿔치기한 문장이에요. 차분히 읽으면 걸러져요.

## 2. 시나리오별 판단

<div class="oa-eli">
  <span class="oa-eli__head">🏥 쉽게 말하면</span>
  <p>병원 접수 창구는 모든 환자를 교수님께 보내지 않아요. 감기는 일반 진료로, 어려운 증상만 전문의에게 보내요. <b>쉬운 일은 싼 모델이 받고, 어려운 일만 비싼 모델로 넘기는 것</b>이 라우팅이에요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🏷️</span><span class="oa-card__title">대량 단순 분류</span>하루 100만 건 리뷰를 긍정/부정/중립으로, 비용 최우선<br>→ <b>Haiku</b></div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🔀</span><span class="oa-card__title">쉬운 것 80% + 어려운 것 20%</span>고객 문의 10만 건, 99%를 3초 안에 응답<br>→ <b>Haiku가 판정, 어려우면 Opus</b></div>
  <div class="oa-card"><span class="oa-card__icon">🗺️</span><span class="oa-card__title">Plan-and-Execute</span>계획을 세우고 실행하는 에이전트<br>→ <b>계획은 Opus, 실행은 Sonnet/Haiku</b></div>
  <div class="oa-card"><span class="oa-card__icon">🕸️</span><span class="oa-card__title">Hub &amp; Spoke</span>멀티 에이전트<br>→ <b>허브는 Opus, 서브는 Sonnet/Haiku</b></div>
</div>

하나씩 근거를 붙이면 이래요.

**하루 100만 건 리뷰를 긍정/부정/중립으로 분류, 비용 최우선.**
→ Haiku. 단순 3클래스 분류에 Opus를 쓰면 수십 배 비용을 내요. Sonnet도 틀린 선택은 아니지만 "비용 최우선" 조건에서는 Haiku가 답이에요.

**고객 문의 10만 건, 80%는 FAQ로 답할 수 있고 20%는 복잡한 조사가 필요. 99%를 3초 안에 응답.**
→ Haiku가 먼저 "FAQ로 답할 수 있는가"를 판정하고, 되면 Haiku가 답하고, 안 되면 Opus로 넘겨요. 이 구조를 라우팅이라고 부르고, 도메인 1의 에이전트 아키텍처에서 다시 나와요. 전부 Opus는 비용과 SLA 둘 다 깨지고, 전부 Haiku는 20%의 품질이 부족해요.

<ol class="oa-flow">
  <li><strong>문의 도착</strong>10만 건</li>
  <li><strong>Haiku가 판정</strong>FAQ로 답할 수 있는가?</li>
  <li><strong>되면 Haiku가 답변</strong>약 80%</li>
  <li><strong>안 되면 Opus로</strong>약 20%, 복잡한 조사</li>
</ol>

**계획을 세우고 실행하는 에이전트(Plan-and-Execute).**
→ 계획은 Opus, 실행 스텝은 Sonnet/Haiku. 계획이 틀리면 뒤가 전부 헛수고라서 계획에 정확도를 써요.

**Hub & Spoke 멀티 에이전트.**
→ 허브(판단·통합)는 Opus, 서브(실행)는 Sonnet/Haiku. 허브는 호출 횟수가 적고 서브는 많으니 이 배치가 비용 효율이 좋아요.

한 가지 함정: **"최신 모델 = 반드시 Opus"가 아니에요.** 세대와 패밀리는 다른 축이에요. 최신 세대의 Haiku가 이전 세대의 Sonnet보다 똑똑한 경우도 있어요.

## 3. 모델 ID 고정

<div class="oa-eli">
  <span class="oa-eli__head">📌 쉽게 말하면</span>
  <p>레시피에 "간장 2큰술"이라고만 쓰면 어느 날 간장 브랜드가 바뀌어 맛이 달라져요. "○○ 양조간장 2큰술"이라고 적어야 늘 같은 맛이 나요. 대신 그 간장이 개선되어도 내 레시피에는 자동으로 반영되지 않아요.</p>
</div>

모델 ID는 `claude-{family}-{major}-{minor}[-{revision}]` 꼴이에요.

```
claude-haiku-4-5-20251001   # 날짜 리비전까지 붙은 형태
claude-opus-4-8             # 4.6 세대부터는 이 자체가 고정 스냅샷
```

시험 문제의 원칙은 단순해요. **프로덕션에서는 가장 구체적으로 지정한 ID를 쓴다.** 에일리어스(짧은 이름)는 나중에 같은 패밀리의 새 버전을 가리킬 수 있어서 재현성이 깨져요.

실제 규칙은 세대에 따라 조금 달라요. 4.6 이전 세대는 날짜 접미사가 붙은 것이 고정 스냅샷이고 없는 것이 에일리어스였어요. 4.6 세대부터는 `claude-opus-4-8` 같은 날짜 없는 ID 자체가 고정 스냅샷이에요. 어느 쪽이든 "가장 구체적인 것을 고른다"는 결론은 같아요.

고정의 대가도 알아 둬요. 고정하면 보안 수정이나 버그 픽스를 자동으로 못 받아요. 그래서 세 가지를 함께 설계해요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🏭</span><span class="oa-card__title">프로덕션</span>가장 구체적인 ID로 고정</div>
  <div class="oa-card"><span class="oa-card__icon">🧪</span><span class="oa-card__title">검증 환경</span>최신 추종</div>
  <div class="oa-card"><span class="oa-card__icon">🔁</span><span class="oa-card__title">운영 주기</span>정기 평가·업데이트 주기를 따로 설계</div>
</div>

## 4. API 요청 한 장으로 보기

<div class="oa-eli">
  <span class="oa-eli__head">🧾 쉽게 말하면</span>
  <p>Claude는 <b>기억이 없는 상담원</b>이에요. 전화를 걸 때마다 "지금까지 나눈 이야기 전부"를 다시 읽어 줘야 이어서 대답해요. 그래서 요청 한 건에 업무 지침(system), 지금까지의 대화(messages), 답변 길이 한도(max_tokens)를 매번 다 담아 보내요.</p>
</div>

이 사이트의 [Building with the Claude API](/courses/claude-with-the-anthropic-api/02-accessing-claude-with-the-api/) 정리에 파라미터 설명이 있으니 여기서는 시험이 노리는 지점만 짚어요.

```json
{
  "model": "claude-opus-4-8",
  "max_tokens": 1024,
  "system": "You are a helpful assistant.",
  "messages": [
    {"role": "user", "content": "Hi!"},
    {"role": "assistant", "content": "Hello!"},
    {"role": "user", "content": "How are you?"}
  ],
  "tools": [...],
  "tool_choice": {"type": "auto"}
}
```

| 필드 | 역할 | 시험 포인트 |
|---|---|---|
| `model` | 모델 선택 | 프로덕션은 구체적인 ID로 고정 |
| `max_tokens` | **출력** 토큰 상한 | 입력 상한이 아님 |
| `system` | 역할·제약·출력 형식을 정하는 상위 지시 | 최상위 필드 |
| `messages` | 대화 기록 | **전체 기록을 매번** 보냄 |
| `tools` | 쓸 수 있는 툴 정의 | 레슨 4에서 다룸 |
| `tool_choice` | 툴 선택 전략 | 레슨 4에서 다룸 |

### Claude는 요청 사이에 아무것도 기억하지 않아요

API는 요청 사이의 상태를 서버에 저장하지 않아요. 각 호출은 독립적으로 처리돼요. 그래서 대화를 이어 가려면 호출할 때마다 **전체 대화 기록**을 `messages`에 담아 보내야 해요.

여기서 두 가지 결과가 나와요.

- 직전 턴만 보내면 Claude는 앞의 대화를 몰라요. "챗봇이 앞에서 한 말을 잊는다"는 증상의 원인은 컨텍스트 윈도우 초과보다 **기록을 안 보낸 것**인 경우가 먼저예요.
- 대화가 길어질수록 요청 한 건이 나르는 토큰이 늘어요. 턴이 쌓이면 지연과 비용이 같이 올라가요.

### 메시지 역할

| 역할 | 담는 것 |
|---|---|
| `user` | 사용자 메시지. **툴 결과도 여기**에 `tool_result` 콘텐츠 블록으로 담음 |
| `assistant` | 모델 응답. 툴 사용 요청(`tool_use` 콘텐츠 블록)도 여기에 담김 |

별도의 `tool` 역할은 없어요. 툴 결과는 `user` 역할 메시지 안에 넣어요.

```json
{
  "role": "user",
  "content": [
    {
      "type": "tool_result",
      "tool_use_id": "toolu_01...",
      "content": "..."
    }
  ]
}
```

messages는 첫 메시지가 user, 그 뒤로 user와 assistant가 번갈아야 해요. user가 연속되면 에러예요. `content`는 문자열로 써도 되고 `[{"type": "text", "text": "..."}]` 블록 리스트로 써도 돼요. 둘은 같은 뜻이에요.

### system prompt

- `messages` 배열의 일부가 아니라 `system` 필드로 따로 전달해요.
- 사용자 메시지보다 우선순위가 높아요.
- 한 번 실려서 대화 전체에 적용돼요.
- 역할, 제약, 출력 형식을 정하는 데 써요.

시험에서 중요한 점이 하나 있어요. **system prompt의 표현이 특정 툴 사용을 과하게 유도할 수 있어요.** "항상 고객을 확인하라"라고 쓰면 실제로 필요하지 않은 상황에서도 모델이 `get_customer`를 호출할 가능성이 높아져요.

### 파라미터에 대한 세 가지 오해

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 흔한 오해</span>
    <ul>
      <li>"<code>max_tokens</code>는 입력 상한이다."</li>
      <li>"<code>temperature</code>는 0~2다."</li>
      <li>"system은 messages 안에 <code>role: system</code>으로 넣는다."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 실제</span>
    <ul>
      <li>출력 토큰의 상한이에요. 입력은 컨텍스트 윈도우로 제한돼요.</li>
      <li>Anthropic API는 0~1이에요.</li>
      <li>최상위 <code>system</code> 필드예요.</li>
    </ul>
  </div>
</div>

**"max_tokens는 입력 상한이다."** 아니에요. 출력 토큰의 상한이에요. 입력은 컨텍스트 윈도우로 제한돼요. 크게 잡아도 실제 쓴 만큼만 과금돼요. `stop_reason == "max_tokens"`면 출력이 잘렸다는 뜻이니 이어받기 처리를 검토해요.

**"temperature는 0~2다."** OpenAI 감각이에요. Anthropic API는 0~1이고, 1.5를 넣으면 `invalid_request_error`가 나요. 구조화 출력·분류·추출은 0, 창작은 0.7~1.0.

**"system은 messages 안에 role: system으로 넣는다."** 아니에요. 최상위 `system` 필드예요. messages 안에 넣으면 에러가 나요. OpenAI 코드를 옮길 때 가장 흔한 실수라 시험에 자주 나와요.

> **커뮤니티 가이드는 다르게 적고 있어요.** 커뮤니티 학습 가이드는 `system`을 최상위 필드로 두는 방법 외에, 대화 중간에 지시를 추가하려고 `messages` 안에 `{"role": "system", ...}`을 넣을 수도 있다고 설명해요(`tool_use`와 그에 대응하는 `tool_result` 사이에는 둘 수 없다는 식의 배치 규칙과 함께). 위 교재 설명과 맞지 않는 부분이라 공식 문서로 확인이 필요해요. 시험 답안의 기본 축은 "system prompt는 최상위 `system` 필드"로 잡아 두세요.

## 5. `stop_reason`: 응답이 왜 멈췄는지 읽기

<div class="oa-eli">
  <span class="oa-eli__head">📻 쉽게 말하면</span>
  <p>무전기에서 말을 마칠 때 "오버"(당신 차례)인지 "오버 앤 아웃"(통신 끝)인지 붙여 주죠. <code>stop_reason</code>이 그 신호예요. 말의 내용을 듣고 "끝난 것 같은데?" 하고 짐작하는 것이 아니라, <b>붙어 온 신호를 보고</b> 다음 행동을 정해요.</p>
</div>

응답에는 모델이 왜 생성을 멈췄는지 알려 주는 `stop_reason`이 들어 있어요.

| 값 | 뜻 | 다음 행동 |
|---|---|---|
| `"end_turn"` | 모델이 응답을 마침 | 결과를 사용자에게 보여 줌. 에이전트 루프 **종료** |
| `"tool_use"` | 모델이 툴 호출을 원함 | 툴을 실행하고 결과를 돌려줌. 에이전트 루프 **계속** |
| `"max_tokens"` | 출력 토큰 한도에 닿음 | 응답이 잘린 상태. 한도를 늘리거나 이어받기 처리를 검토 |
| `"stop_sequence"` | 지정한 중지 시퀀스를 만남 | 애플리케이션 로직에 따라 처리 |

에이전트 시스템에서는 `"tool_use"`와 `"end_turn"`이 특히 중요해요. 루프를 계속 돌릴지, 최종 응답으로 끝낼지를 이 값으로 판단해요.

<ol class="oa-flow">
  <li><strong>요청 전송</strong>전체 기록 + 툴 정의</li>
  <li><strong><code>stop_reason</code> 확인</strong><code>tool_use</code>인가?</li>
  <li><strong>툴 실행</strong>결과를 <code>user</code> 메시지의 <code>tool_result</code>로 추가</li>
  <li><strong>다시 요청</strong><code>end_turn</code>이 올 때까지 반복</li>
</ol>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 루프 종료를 이렇게 판단하면 안 돼요</span>
    <ul>
      <li>응답 텍스트에 "완료했습니다" 같은 말이 있는지 찾기</li>
      <li>텍스트 블록이 있으면 끝난 것으로 보기</li>
      <li><code>max_tokens</code>로 잘린 응답을 완성된 답으로 쓰기</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 이렇게 판단해요</span>
    <ul>
      <li><code>stop_reason</code>이 <code>tool_use</code>면 계속</li>
      <li><code>end_turn</code>이면 종료</li>
      <li><code>max_tokens</code>면 잘림으로 따로 처리</li>
    </ul>
  </div>
</div>

`stop_reason`은 모델이 주는 명시적인 구조화 신호예요. 텍스트 내용을 해석해서 종료를 추측하는 방식은 짐작일 뿐이라 가끔 틀려요.

## 6. Batches API: 동기 호출 vs 배치

<div class="oa-eli">
  <span class="oa-eli__head">👔 쉽게 말하면</span>
  <p>세탁소에 옷을 맡길 때 "당일 급행"은 비싸고 "내일 찾아가기"는 싸요. 내일 아침에만 입으면 되는 옷이라면 급행 요금을 낼 이유가 없어요. 맡길 때 받는 <b>보관표 번호</b>가 <code>custom_id</code>예요. 옷이 맡긴 순서대로 나오지 않아도 번호로 내 옷을 찾아요.</p>
</div>

`client.messages.create()`는 요청하면 그 자리에서 답이 오는 동기 API예요. **Message Batches API**는 여러 요청을 한 덩어리로 넣고, 나중에 결과를 한꺼번에 받는 비동기 API예요.

| 항목 | 내용 |
|---|---|
| 비용 | 동기 API의 약 50% |
| 처리 시간 | 최대 24시간 안에(보통은 훨씬 빠름). 지연 시간 SLA는 보장되지 않음 |
| 결과 대조 | 요청마다 붙인 `custom_id`로 맞춤 |
| 안 맞는 경우 | 실시간 응답, 대화, 멀티턴 툴 호출(요청 1개 = 응답 1개) |

판단 요령은 하나예요. **"사람이 화면 앞에서 기다리는가?"** 기다리면 동기 API, 야간 배치처럼 기다리지 않으면 Batches API가 후보예요.

| 작업 | API | 이유 |
|---|---|---|
| 병합 전 PR 검사 | 동기 | 개발자가 기다리고 있고, 최대 24시간은 받아들일 수 없음 |
| 대화형 코드 리뷰 | 동기 | 즉각 응답이 필요 |
| 야간 기술 부채 보고서 | Batch | 아침까지만 되면 되고 50% 절감 |
| 주간 보안 감사 | Batch | 급하지 않고 50% 절감 |
| 문서 10,000개 처리 | Batch | 대량 처리라 절감 효과가 큼 |

시험 문제: "야간에 100만 건 리뷰 분류, 다음 날 아침까지만 되면 됨, 비용 최소화." → Batches API. 한 건씩 동기 호출하는 선택지, 스트리밍을 쓰는 선택지, temperature를 올리는 선택지는 전부 오답이에요.

두 워크플로가 섞인 문제도 자주 나와요. "차단형 검사와 야간 보고서 둘 다 비용을 줄이고 싶다"면 **기다리는 사람이 없는 쪽만** 배치로 옮겨요. "둘 다 배치로 옮기고 늦으면 동기로 되돌린다"는 선택지는 그럴듯하지만, 지연 시간 보장이 없는 API 위에 차단형 작업을 올린다는 점에서 오답이에요.

### 멀티턴 툴 호출은 배치에 안 맞아요

배치는 요청 하나에 응답 하나가 오는 구조예요. 요청 도중에 모델의 툴 호출을 받아서 실행하고, 그 결과를 돌려줘 분석을 이어 가게 할 방법이 없어요. 5번 섹션의 에이전트 루프(`tool_use` → 실행 → 다시 요청)가 필요한 작업은 동기 호출로 해요.

### `custom_id`

`custom_id`가 필요한 이유도 알아 둬요. 배치는 병렬로 처리되어 결과가 요청 순서대로 오지 않아요. 순서로는 대조가 안 되니 ID로 맞춰요.

```json
{
  "custom_id": "doc-invoice-2024-001",
  "params": {
    "model": "claude-opus-4-8",
    "max_tokens": 1024,
    "messages": [{"role": "user", "content": "Extract data from: ..."}]
  }
}
```

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔗</span><span class="oa-card__title">연결</span>결과를 원본 문서와 이어 줌</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">♻️</span><span class="oa-card__title">부분 재제출</span>실패한 문서만 다시 제출</div>
  <div class="oa-card"><span class="oa-card__icon">🚫</span><span class="oa-card__title">중복 방지</span>성공한 문서를 다시 처리하지 않음</div>
</div>

### 일부만 실패했을 때

<ol class="oa-flow">
  <li><strong>제출</strong>문서 100개를 배치로</li>
  <li><strong>결과</strong>95개 성공, 5개 실패(컨텍스트 한도 초과)</li>
  <li><strong>식별</strong><code>custom_id</code>로 실패한 5개 찾기</li>
  <li><strong>전략 수정</strong>예: 긴 문서를 청크로 분할</li>
  <li><strong>재제출</strong>실패한 5개만</li>
</ol>

배치 전체를 그대로 다시 제출하는 것은 오답이에요. 성공한 95개에 비용을 한 번 더 내고, 실패 원인은 그대로라 5개는 또 실패해요.

### SLA 계획

<div class="oa-eli">
  <span class="oa-eli__head">🚌 쉽게 말하면</span>
  <p>버스가 "최대 24시간 안에 도착"한다면, 30시간 뒤 약속에 맞추려면 <b>앞으로 6시간 안에는 타야</b> 해요. 배치도 마감에서 24시간을 거꾸로 빼서 제출 마감을 정해요.</p>
</div>

결과가 30시간 안에 필요하고 배치 처리에 최대 24시간이 걸릴 수 있다면 이렇게 계산해요.

| 항목 | 값 |
|---|---|
| 결과가 필요한 시점 | 30시간 뒤 |
| 배치 최대 처리 시간 | 24시간 |
| 제출할 수 있는 시간 창 | 30 − 24 = **6시간** |

즉, 마감 최소 24시간 전에는 배치를 제출해야 해요. 자주 제출하는 구조라면 4시간 창 단위로 나누는 방식이 안전해요.

## 7. 프롬프트 캐시: 개념과 배치 순서

<div class="oa-eli">
  <span class="oa-eli__head">📚 쉽게 말하면</span>
  <p>매 수업마다 교과서 전체를 칠판에 다시 베껴 쓰는 대신, <b>교과서는 책상에 펴 두고</b> 오늘 질문만 새로 받아요. 단, 책의 앞쪽 페이지가 한 장이라도 바뀌면 그 뒤는 처음부터 다시 읽어야 해요. 그래서 안 바뀌는 것을 앞에 둬요.</p>
</div>

같은 10K 토큰짜리 시스템 프롬프트를 하루 100만 번 보내면 100억 토큰을 매번 정가로 내요. 프롬프트 캐시를 켜면 두 번째부터는 같은 부분을 크게 할인된 가격으로 재사용해요.

```python
system=[{
    "type": "text",
    "text": very_long_system_prompt,
    "cache_control": {"type": "ephemeral"},
}]
```

시험에서 묻는 것은 두 가지예요.

1. **비용 문제의 정답은 캐시다.** 시스템 프롬프트를 100 토큰으로 압축한다(품질 손실), user 메시지에 매번 넣는다(캐시 효과 없음)는 오답이에요.
2. **캐시는 앞부분부터 일치하는 접두사(prefix)에만 작동한다.** 그래서 정적인 큰 문서는 앞(system)에 두고, 날짜나 사용자 질문처럼 매번 바뀌는 부분은 뒤(user)에 둬요. 동적인 내용을 앞에 두면 매번 캐시 미스가 나요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 매번 캐시 미스</span>
    <ul>
      <li>앞: 오늘 날짜, 사용자 질문</li>
      <li>뒤: 큰 정적 문서</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 캐시 적중</span>
    <ul>
      <li>앞(system): 큰 정적 문서</li>
      <li>뒤(user): 오늘 날짜, 사용자 질문</li>
    </ul>
  </div>
</div>

요금 숫자(첫 쓰기 1.25배, 읽기 10~20%)와 TTL(기본 5분, 1시간 옵션)은 배경 지식이에요. 시험 범위 밖이니 외우지 않아도 되지만, "정적인 문맥을 재사용해 비용을 줄인다"는 개념은 반드시 알아야 해요.

## 8. 429가 오면

`rate_limit_error`는 "조금 기다리라"는 신호예요. 지수 백오프(1초, 2초, 4초, 8초…)로 재시도해요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 전부 오답</span>
    <ul>
      <li>연속으로 10번 다시 때리기</li>
      <li>전부 중단하기</li>
      <li>다른 API 키로 바꿔서 즉시 재시도하기</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 정답</span>
    <ul>
      <li>지수 백오프로 재시도</li>
      <li>SDK의 <code>max_retries</code> 옵션 사용</li>
      <li>직접 짤 때는 지터(랜덤 흔들림) 추가</li>
    </ul>
  </div>
</div>

SDK에는 `max_retries` 옵션이 있고, 직접 짤 때는 지터(랜덤 흔들림)를 더해 여러 클라이언트가 동시에 재시도하는 문제를 피해요.

## 9. 컨텍스트 윈도우 ≠ 성능

<div class="oa-eli">
  <span class="oa-eli__head">🗂️ 쉽게 말하면</span>
  <p>책상이 아무리 넓어도 서류 500장을 한꺼번에 펼쳐 놓으면 가운데 깔린 한 장은 못 찾아요. <b>책상 크기(윈도우)와 찾는 능력은 별개</b>예요. 필요한 서류만 골라 올리는 편이 나아요.</p>
</div>

컨텍스트 윈도우는 모델이 한 번에 볼 수 있는 전체 입력 크기이고, 토큰으로 계산해요. 여기에 들어가는 것은 네 가지예요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📜</span><span class="oa-card__title">system prompt</span>매 요청에 실림</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">전체 메시지 기록</span>턴이 쌓일수록 커짐</div>
  <div class="oa-card"><span class="oa-card__icon">🧰</span><span class="oa-card__title">툴 정의</span>등록한 툴 전부</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">📦</span><span class="oa-card__title">툴 결과</span>호출할 때마다 누적</div>
</div>

Opus 계열은 최대 1M 토큰까지 받아요. 그렇다고 500K 토큰짜리 사내 문서를 통째로 넣고 질문하면 기대한 답이 안 나올 수 있어요. 창을 넘긴 것이 아니에요(1M 안에 들어가요). 중간에 있는 정보를 놓치는 **Lost in the Middle** 현상과 컨텍스트 열화 때문이에요. 대책은 RAG, 요약, 청크+랭킹이고, 레슨 6에서 다뤄요.

컨텍스트 윈도우에서 자주 생기는 문제 세 가지예요.

| 문제 | 무슨 일이 생기나 | 대응 방향 |
|---|---|---|
| 중간 손실(Lost in the Middle) | 긴 입력에서 시작과 끝은 비교적 잘 반영되지만 중간의 세부는 놓칠 수 있음 | 중요한 정보는 입력의 앞이나 뒤쪽에 모아 둠 |
| 툴 결과 누적 | 툴을 부를 때마다 결과가 컨텍스트에 쌓임. 40개 필드를 돌려주는데 실제로 필요한 것이 5개뿐이면 나머지는 낭비 | 필요한 필드만 남김 |
| 점진적 요약 | 대화 기록을 압축하다 보면 숫자, 백분율, 날짜 같은 정확한 값이 빠지거나 "대략", "거의", "몇 개"처럼 흐려짐 | 정확한 값은 요약에 맡기지 않고 따로 보존 |

"더 큰 컨텍스트 윈도우를 가진 모델로 바꾼다"는 선택지는 대개 오답이에요. 창이 커져도 중간을 놓치는 문제는 그대로이기 때문이에요.

## 10. RSP와 ASL, 이 정도만

Anthropic은 모델 능력이 올라갈수록 보안 대책을 강화하는 Responsible Scaling Policy를 공개하고 있고, AI Safety Level(ASL-1~4)을 정의해요. 현재 주력 모델은 ASL-3 기준으로 배포돼요. 시험 범위 밖이지만, "운영상의 보안 책임은 이용자 쪽에도 있다"는 문장은 알아 둬요. API 키 관리, 외부 공개 출력의 최종 확인, 권한 관리는 이용자 몫이에요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. CI에서 두 가지 분석을 Claude로 돌려요. (가) PR 병합을 막고 있는 스타일 검사, (나) 매일 새벽에 도는 테스트 커버리지 보고서. API 비용을 줄이려면?</summary>
  <p>① 둘 다 Batches API로 옮기고 완료를 폴링한다 ② (나)만 Batches API로 옮기고 (가)는 동기 호출로 둔다 ③ 둘 다 Batches API로 옮기고 늦으면 동기 호출로 되돌린다 ④ 결과 순서가 섞일 수 있으니 둘 다 동기 호출로 둔다</p>
  <p><b>정답: ②</b> — 배치는 약 50% 싸지만 최대 24시간이 걸리고 지연 보장이 없어요. 사람이 기다리는 (가)는 동기, 기다리는 사람이 없는 (나)만 배치예요. 순서 문제는 <code>custom_id</code>로 풀려요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 코드 리뷰 작업은 Claude가 변경 파일을 본 뒤 툴로 관련 파일을 몇 번 더 읽어 오고 나서 최종 의견을 내요. 이 작업을 요청 하나로 Batches API에 넣으려 할 때 가장 큰 걸림돌은?</summary>
  <p>① 배치 결과에는 요청과 맞춰 볼 ID가 없다 ② 요청 도중에 툴을 실행해 결과를 돌려주는 왕복을 할 수 없다 ③ 배치는 시스템 프롬프트를 받지 않는다 ④ 24시간 지연만 감수하면 문제없다</p>
  <p><b>정답: ②</b> — 배치는 요청 1개에 응답 1개예요. <code>tool_use</code> → 실행 → 재요청을 반복하는 멀티턴 툴 호출은 동기 호출로 해야 해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 에이전트 루프가 가끔 툴 결과를 받기도 전에 끝나 버려요. 코드를 보니 "응답에 텍스트 블록이 있으면 종료"로 되어 있어요. 어떻게 고칠까요?</summary>
  <p>① 응답 텍스트에 "완료"라는 단어가 있는지 검사한다 ② 반복 횟수를 3회로 고정한다 ③ <code>stop_reason</code>이 <code>tool_use</code>면 계속하고 <code>end_turn</code>이면 끝낸다 ④ <code>max_tokens</code>를 늘린다</p>
  <p><b>정답: ③</b> — 모델은 툴을 요청하면서 텍스트를 함께 내기도 해요. 종료 여부는 모델이 주는 명시적 신호인 <code>stop_reason</code>으로 판단해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 계약서 200건을 배치로 추출했는데 12건이 컨텍스트 한도 초과로 실패했어요. 가장 알맞은 다음 단계는?</summary>
  <p>① 200건 전체를 같은 설정으로 다시 제출한다 ② 실패한 12건을 <code>custom_id</code>로 찾아 긴 문서를 나눈 뒤 그 12건만 다시 제출한다 ③ 200건을 전부 동기 호출로 바꾼다 ④ 결과 순서를 보고 실패한 문서를 추정한다</p>
  <p><b>정답: ②</b> — <code>custom_id</code>가 있으면 실패한 것만 골라 원인을 고쳐 다시 넣을 수 있어요. 전체 재제출은 성공한 188건에 비용을 또 내고 원인도 그대로예요.</p>
</details>

<details class="oa-quiz">
  <summary>Q5. 상담 챗봇이 세 번째 턴에서 사용자가 첫 턴에 말한 주문 번호를 전혀 모르는 것처럼 답해요. 대화는 아주 짧아요. 가장 그럴듯한 원인은?</summary>
  <p>① 컨텍스트 윈도우를 넘었다 ② temperature가 너무 높다 ③ 매 요청에 직전 메시지만 보내고 전체 기록을 보내지 않았다 ④ 모델이 Haiku라서 기억력이 약하다</p>
  <p><b>정답: ③</b> — API는 요청 사이에 상태를 저장하지 않아요. 호출할 때마다 <code>messages</code>에 전체 대화 기록을 담아야 해요.</p>
</details>

## 정리

- 요건을 만족하는 가장 싼 모델. 계획·허브는 Opus, 실행·서브는 Sonnet/Haiku, 대량 단순 작업은 Haiku.
- 프로덕션 모델 ID는 가장 구체적인 것으로 고정. 고정하면 업데이트 주기를 따로 설계해요.
- API는 상태를 저장하지 않아요. 매번 전체 기록을 보내고, 툴 결과는 `user` 메시지의 `tool_result`로, 루프 종료는 `stop_reason`(`tool_use`면 계속, `end_turn`이면 끝)으로 판단해요.
- 사람이 안 기다리는 대량 처리는 Batches API(약 50%, 최대 24시간, 멀티턴 툴 호출 불가). `custom_id`로 결과를 대조하고 실패한 것만 다시 제출해요. 마감에서 24시간을 빼서 제출 창을 잡아요.
- 프롬프트 캐시는 접두사 일치. 정적인 것 앞, 동적인 것 뒤.
- max_tokens는 출력 상한, temperature는 0~1, system은 최상위 필드, 429는 지수 백오프. 큰 창에 다 넣는다고 잘 찾는 게 아니에요(Lost in the Middle).
