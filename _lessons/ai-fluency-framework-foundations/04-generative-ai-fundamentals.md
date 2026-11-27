---
title: "Generative AI fundamentals"
title_ko: "생성형 AI 기초"
course: ai-fluency-framework-foundations
lesson: 4
---

## 학습 목표

- **생성형 AI**를 정의하고 다른 유형의 AI와 어떻게 다른지 구분하기
- 생성형 AI의 핵심 특징과 **기술적 토대** 알아보기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>생성형 AI</strong>분석이 아니라 새 콘텐츠를 만듦</li>
  <li><strong>세 가지 토대</strong>transformer · 데이터 · 연산 능력</li>
  <li><strong>Pre-training</strong>수십억 예시에서 패턴 학습</li>
  <li><strong>Fine-tuning</strong>지시를 따르고 도움이 되게 응답</li>
</ol>

이 레슨을 한 문장으로 줄이면: **LLM은 방대한 글에서 패턴을 배운 뒤, 사람의 지시에 잘 답하도록 다듬어진 "새 콘텐츠 생성기"예요.**

> 이 레슨은 **Deep Dive 1: What is Generative AI?**의 첫 번째 파트예요. 다음 레슨(능력과 한계)과 한 묶음이에요.

## 1. 생성형 AI란

<div class="oa-eli">
  <span class="oa-eli__head">🎨 쉽게 말하면</span>
  <p>미술 평론가는 이미 있는 그림을 <b>분석</b>하고, 화가는 새 그림을 <b>그려요</b>. 생성형 AI는 화가 쪽이에요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🔍 기존에 있는 것을 분석하는 AI</span>
    <ul>
      <li>이미 존재하는 데이터를 분류·판별·예측</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✨ 생성형 AI (generative AI)</span>
    <ul>
      <li><b>새로운 콘텐츠</b>를 만들어 내요</li>
      <li>텍스트 · 이미지 · 코드</li>
    </ul>
  </div>
</div>

Claude 같은 **large language model(LLM)**이 생성형 AI의 대표적인 예예요.

## 2. LLM을 가능하게 한 세 가지

<div class="oa-eli">
  <span class="oa-eli__head">🍞 쉽게 말하면</span>
  <p>맛있는 빵에는 <b>좋은 레시피, 충분한 재료, 큰 오븐</b>이 다 필요해요. 하나만 빠져도 지금 같은 LLM은 나올 수 없었어요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🧩</span><span class="oa-card__title">알고리즘·아키텍처 돌파구</span>특히 <b>transformer</b> 아키텍처 (레시피)</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">방대한 학습 데이터</span>디지털로 쌓인 엄청난 양의 데이터 (재료)</div>
  <div class="oa-card"><span class="oa-card__icon">🖥️</span><span class="oa-card__title">강력한 연산 능력</span>급격히 늘어난 컴퓨팅 파워 (오븐)</div>
</div>

## 3. 어떻게 배우나 — 두 단계 학습

<div class="oa-eli">
  <span class="oa-eli__head">🎓 쉽게 말하면</span>
  <p>학교에서 폭넓게 책을 읽으며 세상을 배우는 게 <b>pre-training</b>, 입사 후 "손님에게는 이렇게 응대해요"를 배우는 신입 교육이 <b>fine-tuning</b>이에요.</p>
</div>

| 단계 | 무엇을 하나 | 결과 |
|---|---|---|
| **Pre-training (사전 학습)** | 수십억 개의 예시에서 **패턴을 분석** | 언어와 지식의 폭넓은 패턴을 익힘 |
| **Fine-tuning (미세 조정)** | **지시를 따르고 도움이 되는 응답**을 하도록 학습 | 쓸모 있는 assistant로 다듬어짐 |

### 함께 알아 둘 개념

| 개념 | 뜻 |
|---|---|
| **Context window** | 모델이 한 번에 참조할 수 있는 정보의 양 |
| **Emergent capabilities (창발적 능력)** | 명시적으로 가르치지 않았는데 규모가 커지면서 나타나는 능력 |

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 생성형 AI가 다른 유형의 AI와 구분되는 핵심은?</summary>
  <p>① 인터넷에 연결돼 있다 ② <b>기존 데이터를 분석하는 데 그치지 않고 새로운 콘텐츠를 만든다</b> ③ 항상 정확하다 ④ 학습 데이터가 필요 없다</p>
  <p><b>정답: ②</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. 현대 LLM을 가능하게 한 세 가지 발전은?</summary>
  <p><b>transformer 등 알고리즘·아키텍처 돌파구 · 방대한 학습 데이터 · 강력한 연산 능력</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 짝 맞추기 — 학습 단계</summary>
  <p><b>Pre-training</b> → 수십억 예시에서 패턴 분석<br><b>Fine-tuning</b> → 지시를 따르고 도움이 되게 응답하도록 학습</p>
</details>

## 정리

- 생성형 AI는 기존 데이터를 분석하는 데 그치지 않고 **새 콘텐츠(텍스트·이미지·코드)**를 만들어요.
- LLM은 **transformer 아키텍처 · 방대한 데이터 · 강력한 연산 능력**이 합쳐져 가능해졌어요.
- 학습은 **pre-training → fine-tuning** 두 단계예요.
- **Context window**와 **창발적 능력**은 이후 레슨에서도 계속 나오는 핵심 개념이에요.
