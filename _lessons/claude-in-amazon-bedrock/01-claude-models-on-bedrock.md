---
title: "Claude models on Amazon Bedrock"
title_ko: "Bedrock의 Claude 모델과 모델 ID"
course: claude-in-amazon-bedrock
lesson: 1
---

> 이 정리는 **Amazon Bedrock 고유 부분**(모델 ID·접속·요청)만 다뤄요. 프롬프트 엔지니어링·eval·tool use·RAG·에이전트 등 일반 API 개념은 [Building with the Claude API](/courses/claude-with-the-anthropic-api/01-introduction/) 코스와 겹쳐서 여기서는 생략해요.

## 학습 목표

- Claude 모델 계열(**Fable 5 · Opus · Sonnet · Haiku**)과 선택 기준 이해하기
- Bedrock에서 각 모델을 가리키는 **모델 ID** 파악하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>모델 계열</strong>Fable 5 · Opus · Sonnet · Haiku</li>
  <li><strong>선택 기준</strong>지능 · 속도 · 비용의 균형</li>
  <li><strong>모델 ID</strong>Bedrock에서 모델을 가리키는 이름</li>
  <li><strong>요청에 전달</strong>modelId로 넘기기</li>
</ol>

이 레슨을 한 문장으로 줄이면: **가장 센 모델이 아니라 "일에 맞는" 모델을 고르고, Bedrock에서는 그 모델을 모델 ID로 불러요.**

## 1. Claude 모델 계열

<div class="oa-eli">
  <span class="oa-eli__head">🚚 쉽게 말하면</span>
  <p>택배 한 상자를 보내는데 대형 트럭을 부르지 않죠. <b>짐의 크기와 급한 정도에 맞는 차</b>를 고르듯, 작업의 난이도와 속도·비용에 맞춰 모델을 골라요.</p>
</div>

Claude는 세 가지 핵심 계열(**Opus · Sonnet · Haiku**)과, Opus 위에 있는 새 tier **Claude Fable 5**를 제공해요. 모두 텍스트 생성·코딩·이미지 분석 등 핵심 능력을 공유하고, 차이는 **지능 · 속도 · 비용의 균형**이에요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🏔️</span><span class="oa-card__title">Fable 5</span>가장 강력한 tier. Opus를 넘어서는 가장 어려운 과제용</div>
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">Opus</span>핵심 계열 중 가장 유능. 복잡한 추론·계획, 장기 다단계 작업</div>
  <div class="oa-card"><span class="oa-card__icon">⚖️</span><span class="oa-card__title">Sonnet</span>지능 · 속도 · 비용의 균형점. 대부분의 실무에 적합</div>
  <div class="oa-card"><span class="oa-card__icon">⚡</span><span class="oa-card__title">Haiku</span>가장 빠른 모델. 속도와 비용에 최적화</div>
</div>

| 모델 | 강점 | 비용·지연 | 이럴 때 써요 |
|---|---|---|---|
| **Fable 5** | Opus를 넘어서는 최고 성능 | 비용이 상당히 높음 | 그만한 가치가 있는 작업에만 **선별적으로** |
| **Opus** | 복잡한 추론·계획, 장기 다단계 작업 | 중간~높음 | 어렵고 긴 작업 |
| **Sonnet** | 강한 코딩 능력 + 빠른 생성 | 균형 | 대부분의 실무 |
| **Haiku** | 가장 빠름 | 가장 낮은 쪽 | 실시간 사용자 대면 응답 |

> Haiku는 Opus·Sonnet의 reasoning을 지원하지 않아요. 그래서 실시간 사용자 대면에는 적합하지만 **복잡한 문제 해결에는 덜 적합**해요.

<div class="oa-bars">
  <div class="oa-bar"><span>Haiku</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:30%"></span><span class="oa-bar__val">가장 빠르고 저렴</span></span></div>
  <div class="oa-bar"><span>Sonnet</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:55%"></span><span class="oa-bar__val">균형</span></span></div>
  <div class="oa-bar"><span>Opus</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:78%"></span><span class="oa-bar__val">복잡한 추론</span></span></div>
  <div class="oa-bar"><span>Fable 5</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:95%"></span><span class="oa-bar__val">최고 성능 · 최고 비용</span></span></div>
  <p class="oa-bars__caption">막대는 지능(그리고 대체로 비용)이 높아지는 순서를 보여 주는 개념적 표현이에요 (실제 수치 아님).</p>
</div>

## 2. Bedrock 모델 ID

<div class="oa-eli">
  <span class="oa-eli__head">🏷️ 쉽게 말하면</span>
  <p>카페에서 "그 커피 주세요"라고 하면 안 통하고 <b>메뉴판에 적힌 정확한 이름</b>으로 주문해야 하죠. Bedrock에서는 그 정확한 이름이 모델 ID예요.</p>
</div>

Bedrock에서는 모델을 **모델 ID**로 지정해요. 예를 들면:

- Fable 5 → `anthropic.claude-fable-5`

| 궁금한 것 | 답 |
|---|---|
| 정확한 모델 ID는 어디서 확인하나 | AWS Bedrock 콘솔의 **모델 카탈로그** |
| 리전을 넘나드는 호출에 쓰는 ID는 | cross-region inference의 **inference profile** (다음 레슨) |
| 요청에서 어떻게 쓰나 | 이 ID를 `modelId`로 전달 |

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 짝 맞추기 — 상황과 모델</summary>
  <p>"실시간 채팅 응답이 가장 빨라야 한다" → <b>Haiku</b><br>"일반적인 코딩·업무 작업 대부분" → <b>Sonnet</b><br>"길고 복잡한 다단계 추론" → <b>Opus</b><br>"Opus로도 부족한 가장 어려운 과제, 비용을 감수할 가치가 있음" → <b>Fable 5</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. Claude 모델들의 차이를 가장 잘 설명한 것은?</summary>
  <p>① 지원하는 언어가 다르다 ② <b>핵심 능력은 공유하고, 지능·속도·비용의 균형이 다르다</b> ③ Haiku만 이미지를 분석한다 ④ Opus만 코딩을 한다</p>
  <p><b>정답: ②</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. Bedrock에서 요청할 때 모델은 무엇으로 지정하나요?</summary>
  <p><b>모델 ID</b> (예: <code>anthropic.claude-fable-5</code>)를 <code>modelId</code>로 전달해요. 정확한 ID는 Bedrock 콘솔의 모델 카탈로그에서 확인해요.</p>
</details>

## 정리

- Claude는 **Opus · Sonnet · Haiku** 세 핵심 계열과 그 위의 **Fable 5** tier를 제공해요.
- 모든 모델이 핵심 능력을 공유하고, 차이는 **지능 · 속도 · 비용의 균형**이에요.
- **Haiku**는 가장 빠르지만 Opus·Sonnet의 reasoning은 지원하지 않아요.
- **Fable 5**는 비용이 높으니 그만한 가치가 있는 작업에만 써요.
- Bedrock에서는 모델을 **모델 ID**로 지정하고, 요청에 `modelId`로 넘겨요.
