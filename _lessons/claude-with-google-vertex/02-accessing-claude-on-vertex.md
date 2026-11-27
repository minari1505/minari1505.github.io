---
title: "Accessing Claude on Vertex AI"
title_ko: "Vertex에서 Claude 접속과 요청"
course: claude-with-google-vertex
lesson: 2
---

> 이 정리는 **Vertex AI 고유 부분**만 다뤄요. 이후 개념(멀티턴·시스템 프롬프트·streaming·tool use·RAG·에이전트 등)은 일반 API와 동일하니 [Building with the Claude API](/courses/claude-with-the-anthropic-api/01-introduction/) 코스를 참고하세요.

## 학습 목표

- Vertex를 통한 **요청 흐름**과 **"서버 경유" 원칙** 이해하기
- `anthropic[vertex]` SDK 설치와 `AnthropicVertex` **클라이언트 생성**하기
- **Vertex 모델 ID 형식**을 알고 첫 요청 만들기 · 응답 파싱하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>SDK 설치</strong>anthropic[vertex]</li>
  <li><strong>클라이언트 생성</strong>AnthropicVertex(region, project_id)</li>
  <li><strong>모델 ID</strong>모델명@버전날짜</li>
  <li><strong>messages.create</strong>model · max_tokens · messages</li>
  <li><strong>응답 추출</strong>message.content[0].text</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Vertex에서는 `AnthropicVertex` 클라이언트만 다르고, 나머지는 익숙한 `messages.create` 그대로예요.**

## 1. 요청 흐름과 서버 경유 원칙

<div class="oa-eli">
  <span class="oa-eli__head">🔑 쉽게 말하면</span>
  <p>집 열쇠를 현관문 앞에 걸어 두지 않죠. <b>비밀 자격증명은 내가 지키는 서버 안에만</b> 두고, 사용자의 브라우저나 앱에는 절대 넣지 않아요.</p>
</div>

사용자 입력이 AI 응답이 되기까지 5단계를 거쳐요.

<ol class="oa-flow">
  <li><strong>Request to Server</strong>사용자 → 내 서버</li>
  <li><strong>Request to Vertex</strong>내 서버 → Vertex</li>
  <li><strong>Model Processing</strong>모델이 응답 생성</li>
  <li><strong>Response to Server</strong>Vertex → 내 서버</li>
  <li><strong>Response to Client</strong>내 서버 → 사용자</li>
</ol>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 클라이언트 코드에서 직접 호출</span>
    <ul>
      <li>요청에는 <b>비밀 자격증명</b>이 필요해요</li>
      <li>클라이언트 코드에 넣으면 누구나 볼 수 있어요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 서버를 중개자로</span>
    <ul>
      <li>내가 통제하고 보호하는 <b>서버</b>가 Vertex에 요청해요</li>
      <li>자격증명은 서버 밖으로 나가지 않아요</li>
    </ul>
  </div>
</div>

서버는 **Anthropic SDK** 또는 Google의 공식 Vertex SDK로 통신해요. Anthropic은 **Python · TypeScript · Go · Ruby**용 공식 SDK를 제공해요.

## 2. SDK 설치와 클라이언트 생성

Jupyter 노트북에서 Vertex 지원이 포함된 Anthropic SDK를 설치해요.

```python
%pip install "anthropic[vertex]"
```

`[vertex]` 부분이 Google Cloud Vertex AI 연결에 필요한 구성 요소를 함께 설치해요.

```python
from anthropic import AnthropicVertex

client = AnthropicVertex(region="global", project_id="your-project-id")
model = "claude-sonnet-4@20250514"
```

| 항목 | 설명 |
|---|---|
| `project_id` | 실제 Google Cloud 프로젝트 ID (콘솔의 프로젝트 선택기에서 확인) |
| `region` | 요청을 보낼 리전 (예시에서는 `"global"`) |
| `model` | Vertex 모델 ID. 변수로 두면 반복 입력을 줄일 수 있어요 |

### Vertex 모델 ID 형식

<div class="oa-eli">
  <span class="oa-eli__head">🏷️ 쉽게 말하면</span>
  <p>같은 사람도 회사마다 사번 형식이 다르듯, <b>같은 Claude 모델도 플랫폼마다 부르는 이름 형식이 달라요.</b></p>
</div>

| 플랫폼 | 모델 ID 형식 | 예 |
|---|---|---|
| **Vertex AI** | `모델명@버전날짜` | `claude-sonnet-4@20250514` |
| **Amazon Bedrock** | `anthropic.`으로 시작 | `anthropic.claude-...` |

## 3. 요청 만들고 응답 꺼내기

`create` 함수의 핵심 파라미터는 세 가지예요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🏷️</span><span class="oa-card__title">model</span>사용할 모델 ID</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📏</span><span class="oa-card__title">max_tokens</span>응답 길이의 <b>상한</b>. 목표가 아니라 예산이에요</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">messages</span>대화 기록</div>
</div>

```python
message = client.messages.create(
    model=model,
    max_tokens=1000,
    messages=[
        {"role": "user", "content": "What is quantum computing? Answer in one sentence"}
    ]
)
```

각 메시지는 `role`(`"user"` 또는 `"assistant"`)과 `content`(실제 텍스트)를 가진 딕셔너리예요.

응답은 메타데이터가 많은 복잡한 객체예요. Claude가 생성한 텍스트만 얻으려면:

```python
message.content[0].text
```

### Bedrock과 나란히 보기

| | Vertex AI | Amazon Bedrock |
|---|---|---|
| **클라이언트** | `AnthropicVertex(region=..., project_id=...)` | `boto3.client("bedrock-runtime", region_name=...)` |
| **요청** | `client.messages.create(model, max_tokens, messages)` | `client.converse(modelId, messages)` |
| **텍스트 추출** | `message.content[0].text` | `response["output"]["message"]["content"][0]["text"]` |

*(Bedrock 쪽은 [Claude with Amazon Bedrock](/courses/claude-in-amazon-bedrock/02-accessing-claude-on-bedrock/) 정리 기준이에요.)*

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 브라우저(클라이언트) 코드에서 Vertex API를 직접 호출하면 안 되는 이유는?</summary>
  <p>① 속도가 느려서 ② <b>요청에 필요한 비밀 자격증명이 누구에게나 노출되기 때문</b> ③ Vertex가 브라우저를 차단해서 ④ 비용이 더 들어서</p>
  <p><b>정답: ②</b> — 항상 내가 통제하는 서버를 중개자로 둬요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. Vertex 모델 ID로 올바른 형식은?</summary>
  <p>① <code>anthropic.claude-sonnet-4</code> ② <b><code>claude-sonnet-4@20250514</code></b> ③ <code>vertex/claude-sonnet-4</code> ④ <code>claude-sonnet-4:latest</code></p>
  <p><b>정답: ②</b> — <code>모델명@버전날짜</code> 형식이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. <code>max_tokens=1000</code>은 "1000 토큰짜리 답을 써라"라는 뜻이다. (O / X)</summary>
  <p><b>정답: X</b> — 응답 길이의 <b>상한(예산)</b>이지 목표가 아니에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 응답에서 생성된 텍스트를 꺼내는 코드는?</summary>
  <p><code>message.content[0].text</code></p>
</details>

## 정리

- 항상 **서버를 중개자**로 두고, 서버가 Anthropic SDK로 Vertex에 요청해요. 클라이언트에서 직접 호출하지 않아요.
- 설치는 `%pip install "anthropic[vertex]"`, 클라이언트는 `AnthropicVertex(region=..., project_id=...)`로 만들어요.
- Vertex 모델 ID는 `claude-sonnet-4@20250514`처럼 **`모델명@버전날짜`** 형식이에요.
- 요청은 `client.messages.create(model, max_tokens, messages)`, 텍스트는 `message.content[0].text`로 꺼내요.
- `max_tokens`는 목표가 아니라 **응답 길이의 상한**이에요.
