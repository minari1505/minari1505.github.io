---
title: "Accessing Claude on Amazon Bedrock"
title_ko: "Bedrock에서 Claude 접속과 요청"
course: claude-in-amazon-bedrock
lesson: 2
---

> 이 정리는 **Amazon Bedrock 고유 부분**만 다뤄요. 이후 개념(멀티턴·시스템 프롬프트·streaming·tool use·RAG·에이전트 등)은 일반 API와 동일하니 [Building with the Claude API](/courses/claude-with-the-anthropic-api/01-introduction/) 코스를 참고하세요.

## 학습 목표

- Bedrock을 통한 **요청 흐름** 이해하기
- boto3 `bedrock-runtime` 클라이언트로 **첫 요청** 만들기
- **모델 ID의 리전 가용성** 문제를 **inference profile(cross-region inference)**로 해결하기
- `converse` API의 **메시지 구조와 응답 파싱** 익히기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>클라이언트 만들기</strong>boto3 bedrock-runtime</li>
  <li><strong>모델 고르기</strong>모델 ID 또는 inference profile</li>
  <li><strong>메시지 만들기</strong>role + content 리스트</li>
  <li><strong>converse 호출</strong>modelId + messages</li>
  <li><strong>응답 파싱</strong>output → message → content → text</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Bedrock에서는 boto3 클라이언트로 `converse`를 부르고, 리전 문제는 inference profile에 맡겨요.**

## 1. 요청은 어떤 길로 가나

<div class="oa-eli">
  <span class="oa-eli__head">🍽️ 쉽게 말하면</span>
  <p>손님(사용자)은 주방에 직접 들어가지 않아요. <b>웨이터(내 서버)</b>가 주문을 받아 주방(Bedrock)에 전달하고, 음식이 나오면 손님에게 가져다줘요.</p>
</div>

<ol class="oa-flow">
  <li><strong>사용자</strong>웹 인터페이스로 메시지 제출</li>
  <li><strong>내 서버</strong>그 텍스트를 담은 요청 수신</li>
  <li><strong>Bedrock 요청</strong>Bedrock 클라이언트로 사용자 메시지 + 모델 ID 전송</li>
  <li><strong>모델</strong>선택한 모델이 텍스트 생성</li>
  <li><strong>Bedrock 응답</strong>assistant 메시지로 반환</li>
  <li><strong>내 서버</strong>응답을 사용자 브라우저로 전달</li>
</ol>

## 2. Bedrock 클라이언트 설정

`boto3`로 Bedrock runtime 서비스에 연결하는 클라이언트를 만들어요.

```python
import boto3

client = boto3.client("bedrock-runtime", region_name="us-west-2")
```

첫 요청에 필요한 세 가지예요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">Bedrock Runtime Client</span>Bedrock으로 가는 연결</div>
  <div class="oa-card"><span class="oa-card__icon">🏷️</span><span class="oa-card__title">Model ID</span>실행할 모델</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">User Message</span>입력 텍스트</div>
</div>

## 3. 모델 ID와 리전 가용성

<div class="oa-eli">
  <span class="oa-eli__head">🏪 쉽게 말하면</span>
  <p>모든 지점에 모든 메뉴가 있는 건 아니에요. 없는 메뉴를 시키면 "그런 메뉴 없어요"라는 답만 돌아오죠. <b>inference profile은 그 메뉴를 파는 지점으로 주문을 알아서 넘겨 주는 본사 콜센터</b>예요.</p>
</div>

> ⚠️ **모든 모델이 모든 AWS 리전에 있는 것은 아니에요.** 선택한 리전에 없는 모델을 실행하려 하면 "모델이 존재하지 않는다"는 **모호한 오류**가 나요. 예: Sonnet이 `us-west-2`에는 있는데 `us-east-1`에서 요청하면 실패해요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 모델 ID를 리전에 직접 지정</span>
    <ul>
      <li>어느 모델이 어느 리전에 있는지 직접 추적해야 해요</li>
      <li>리전이 안 맞으면 "모델이 없다"는 모호한 오류</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ Inference profile 사용</span>
    <ul>
      <li>모델이 실제로 호스팅된 리전으로 <b>자동 라우팅</b></li>
      <li>여러 리전(예: <code>us-west-2</code>, <code>us-east-2</code>)에 걸쳐 알아서 올바른 리전으로 보내요</li>
    </ul>
  </div>
</div>

> inference profile ID는 AWS Bedrock 콘솔의 메인 모델 카탈로그가 아니라 **"Cross-region inference"** 아래에서 찾아 복사해요.

## 4. 메시지 만들고 요청 보내기

### User 메시지 구조

```python
user_message = {
    "role": "user",
    "content": [
        {"text": "What's 1+1?"}
    ]
}
```

`content`가 **리스트**인 이유는 한 메시지에 텍스트·이미지 등 여러 유형을 담을 수 있기 때문이에요(멀티모달 요청 가능).

### 요청 보내기 — `converse`

```python
response = client.converse(
    modelId=model_id,
    messages=[user_message]
)
```

응답에는 메타데이터가 많아요. 생성된 텍스트만 얻으려면 구조를 따라 들어가요.

```python
response["output"]["message"]["content"][0]["text"]
```

<ol class="oa-flow">
  <li><strong>response</strong>메타데이터가 담긴 전체 응답</li>
  <li><strong>["output"]["message"]</strong>assistant 메시지</li>
  <li><strong>["content"][0]</strong>첫 번째 콘텐츠 블록</li>
  <li><strong>["text"]</strong>생성된 텍스트</li>
</ol>

### 메시지 유형

| 유형 | role | 내용 |
|---|---|---|
| **User 메시지** | `"user"` | 모델에 넣을 내용 |
| **Assistant 메시지** | `"assistant"` | 모델이 생성한 내용 |

둘 다 **role + content 리스트**로 같은 구조예요. Bedrock이 돌려주는 assistant 메시지도 user 메시지와 같은 형식이라, user/assistant를 번갈아 이어 붙이면 **멀티턴 대화**를 쉽게 만들 수 있어요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. <code>us-east-1</code>에서 요청했더니 "모델이 존재하지 않는다"는 오류가 났다. 가장 먼저 의심할 것은?</summary>
  <p>① API key 만료 ② <b>그 모델이 해당 리전에 없음 — inference profile(cross-region inference)로 해결</b> ③ 메시지 형식 오류 ④ boto3 버전</p>
  <p><b>정답: ②</b> — 모델마다 리전 가용성이 달라요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 메시지의 <code>content</code>가 문자열이 아니라 리스트인 이유는?</summary>
  <p><b>한 메시지에 텍스트·이미지 등 여러 유형의 콘텐츠를 담을 수 있게</b> 하기 위해서예요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. <code>converse</code> 응답에서 생성된 텍스트를 꺼내는 코드는?</summary>
  <p><code>response["output"]["message"]["content"][0]["text"]</code></p>
</details>

<details class="oa-quiz">
  <summary>Q4. inference profile ID는 어디에서 찾나요?</summary>
  <p>Bedrock 콘솔의 메인 모델 카탈로그가 아니라 <b>"Cross-region inference"</b> 아래에서 찾아요.</p>
</details>

## 정리

- Bedrock 접속은 `boto3.client("bedrock-runtime", region_name=...)`로 시작해요.
- 모델은 **리전마다 가용성이 달라요.** **inference profile(cross-region inference)**로 자동 라우팅해 문제를 피해요.
- 요청은 `converse(modelId, messages=[...])`로 보내고, 텍스트는 `response["output"]["message"]["content"][0]["text"]`로 꺼내요.
- user/assistant 메시지는 **같은 구조**라 멀티턴 대화를 쉽게 구성할 수 있어요.
