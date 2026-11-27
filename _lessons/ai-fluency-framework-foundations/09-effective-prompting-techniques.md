---
title: "Effective prompting techniques"
title_ko: "효과적인 프롬프트 기법"
course: ai-fluency-framework-foundations
lesson: 9
---

## 학습 목표

- **prompt engineering**이 무엇이고 생산적인 AI 협업에 왜 중요한지 이해하기
- **여섯 가지 기초 프롬프트 기법**을 적용해 AI와의 상호작용 개선하기
- 성공적인 상호작용으로 이어지는 **공통 패턴** 알아보기
- 응답이 기대에 못 미칠 때 프롬프트를 **진단하고 다듬기**

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>정의</strong>AI를 위한 효과적인 지시 설계</li>
  <li><strong>6가지 기법</strong>맥락 · 예시 · 제약 · 단계 · 생각 · 역할</li>
  <li><strong>비밀 무기</strong>AI에게 프롬프트 개선을 부탁</li>
  <li><strong>반복</strong>결과를 보고 다듬기</li>
</ol>

이 레슨을 한 문장으로 줄이면: **좋은 프롬프트는 마법 주문이 아니라, 사람에게 일을 잘 부탁하는 방법에 AI 특유의 요령을 더한 것이에요.**

> 이 레슨은 **Deep Dive 2: Effective prompting techniques**예요. 앞 레슨의 Description 역량을 실전 기술로 풀어낸 내용이에요.

## 1. Prompt engineering이란

<div class="oa-eli">
  <span class="oa-eli__head">📝 쉽게 말하면</span>
  <p>후배에게 일을 부탁하는 <b>업무 요청서</b>를 잘 쓰는 것과 같아요. 배경, 예시, 분량, 순서를 적어 주면 한 번에 원하는 결과가 오죠.</p>
</div>

**Prompt engineering**은 AI system을 위한 효과적인 지시를 설계하는 일이에요. 익숙한 **사람 사이의 소통 원칙**에 **AI 특유의 고려 사항**을 합친 것이에요.

## 2. 여섯 가지 기초 기법

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧾</span><span class="oa-card__title">① 맥락 주기</span>무엇을, 왜 원하는지와 관련 배경을 구체적으로</div>
  <div class="oa-card"><span class="oa-card__icon">🖼️</span><span class="oa-card__title">② 예시 보여주기</span>원하는 출력의 스타일이나 형식을 직접 보여주기</div>
  <div class="oa-card"><span class="oa-card__icon">📏</span><span class="oa-card__title">③ 제약 지정하기</span>형식, 길이 등 출력 요구 사항을 분명히</div>
  <div class="oa-card"><span class="oa-card__icon">🪜</span><span class="oa-card__title">④ 단계로 나누기</span>복잡한 작업은 여러 단계의 추론으로 안내</div>
  <div class="oa-card"><span class="oa-card__icon">🤔</span><span class="oa-card__title">⑤ 먼저 생각하게 하기</span>AI가 과정을 풀어 갈 여유를 주기</div>
  <div class="oa-card"><span class="oa-card__icon">🎭</span><span class="oa-card__title">⑥ 역할·톤 정하기</span>AI가 어떻게 소통할지 지정</div>
</div>

| 기법 | 이런 말을 더해요 (예시) |
|---|---|
| **Give context** | "다음 주 신입 교육에서 쓸 자료야. 듣는 사람은 비전공자야." |
| **Show examples** | "아래 예시와 같은 말투와 형식으로 써 줘." |
| **Specify constraints** | "표로, 5행 이내, 300자 이하로." |
| **Break into steps** | "먼저 요약하고, 다음에 비교하고, 마지막에 추천해 줘." |
| **Ask the AI to think first** | "답하기 전에 먼저 단계별로 생각해 봐." |
| **Define the AI's role or tone** | "꼼꼼한 편집자처럼, 친근한 말투로." |

*(오른쪽 열의 문장은 이해를 돕기 위한 예시예요.)*

## 3. 응답이 기대와 다를 때

<div class="oa-eli">
  <span class="oa-eli__head">🧂 쉽게 말하면</span>
  <p>요리는 한 번에 간이 맞지 않아요. <b>맛을 보고, 조금 고치고, 다시 맛보는</b> 과정이 정상이에요. 그리고 옆에 있는 요리사(AI)에게 "뭐가 부족할까?" 하고 물어봐도 돼요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">😕 흔한 대응</span>
    <ul>
      <li>한 번 써 보고 "AI는 별로네" 하고 포기</li>
      <li>같은 프롬프트를 그대로 다시 실행</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">💪 강의가 권하는 대응</span>
    <ul>
      <li><b>비밀 무기</b>: AI에게 내 프롬프트를 개선해 달라고 부탁</li>
      <li>프롬프트는 <b>반복(iterative)</b> 작업 — 결과를 보고 접근을 다듬기</li>
      <li>AI와 <b>함께</b> 다듬어도 좋아요</li>
    </ul>
  </div>
</div>

### 성공하는 프롬프트의 공통 패턴

| 패턴 | 내용 |
|---|---|
| **명확한 작업 개요** | 무엇을 하려는지 한눈에 |
| **형식 지정** | 결과물이 어떤 모양이어야 하는지 |
| **명시적 제약** | 길이, 범위 등 지켜야 할 것 |
| **관련 배경 정보** | AI가 알아야 할 맥락 |

### 돌아보기 (Reflection)

- 여섯 기법 중 지금 내 AI 사용을 가장 크게 개선할 것은?
- 기대에 못 미친 최근 대화에서, 어떤 기법이 결과를 바꿨을까요?
- 이 기법들은 4D의 **Description** 역량과 어떻게 이어지나요?

원한다면 지난 레슨의 **Bad Prompt Makeover**를 다시 해 보며 이 원칙들을 연습해 보세요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 짝 맞추기 — 요청 문장과 기법</summary>
  <p>"이 글은 고객사 임원에게 보낼 거야" → <b>맥락 주기</b><br>"아래 샘플처럼 써 줘" → <b>예시 보여주기</b><br>"세 문단, 각 3문장 이내" → <b>제약 지정하기</b><br>"답하기 전에 차근차근 생각해 봐" → <b>먼저 생각하게 하기</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. 강의가 말하는 프롬프트의 "비밀 무기"는?</summary>
  <p>① 더 긴 프롬프트 ② 영어로 쓰기 ③ <b>AI 자체에게 프롬프트 개선을 도와 달라고 하기</b> ④ 같은 요청 반복하기</p>
  <p><b>정답: ③</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 잘 쓴 프롬프트는 한 번에 완벽한 결과를 내야 한다. (O / X)</summary>
  <p><b>정답: X</b> — 프롬프트는 반복적인 과정이에요. 결과를 보고 다듬는 것을 당연하게 여기세요.</p>
</details>

## 정리

- 효과적인 프롬프트는 **명확한 소통 원칙 + AI 특유의 기법**이에요.
- 여섯 기법: **맥락 주기 · 예시 보여주기 · 제약 지정 · 단계로 나누기 · 먼저 생각하게 하기 · 역할·톤 정하기.**
- **비밀 무기**: AI에게 프롬프트 개선을 부탁하기.
- 프롬프트는 **반복적**이고, AI와 함께 다듬을 수도 있어요.
- 성공 패턴: 명확한 작업 개요, 형식 지정, 명시적 제약, 관련 배경 정보.
