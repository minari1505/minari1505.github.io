---
title: "What We Mean by AI"
title_ko: "여기서 말하는 AI란"
course: ai-capabilities-and-limitations
lesson: 2
---

## 학습 목표

- 생성형 AI를, 매일 접하는 분류·예측 AI와 구분하기
- 생성형 AI의 속성이 "능력↔한계"의 연속선(continuum) 위에 있음을 이해하기
- 앞으로 깊이 다룰 네 가지 핵심 속성(Next Token Prediction, Knowledge, Working Memory, Steerability) 미리 보기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>AI의 대부분은 생성형이 아니다</strong>스팸 필터 · 추천 · 사기 탐지</li>
  <li><strong>생성형 AI</strong>토큰을 하나씩 만들어 새 콘텐츠 생성</li>
  <li><strong>네 가지 속성</strong>각각 능력↔한계의 스펙트럼</li>
  <li><strong>보정된 신뢰</strong>작업을 스펙트럼 위 어디에 놓을지 정하기</li>
</ol>

이 레슨을 한 문장으로 줄이면: **생성형 AI는 "한결같이 유능"하지도 "한결같이 불안정"하지도 않고, 네 가지 예측 가능한 축을 따라 강하고 약해요.**

## 1. 생성형 AI란

<div class="oa-eli">
  <span class="oa-eli__head">📬 쉽게 말하면</span>
  <p>우체국의 <b>우편물 분류기</b>는 이미 있는 편지를 받아 "이건 서울, 이건 부산"으로 나눠요. <b>편지를 써 주는 대필가</b>는 없던 편지를 새로 만들어요. 둘 다 "똑똑한 일"이지만 하는 일의 종류가 달라요. 이 강의가 다루는 건 대필가 쪽이에요.</p>
</div>

세상 대부분의 AI(스팸 필터, 추천, 사기 탐지)는 **생성형이 아니에요.** 이 강의가 다루는 것은 생성형 — **transformer 기반 텍스트 모델이 토큰을 하나씩 만들어 새 콘텐츠를 생성**하는 종류예요. 기존 콘텐츠를 분류·정렬·순위 매기는 AI와는 근본적으로 달라요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🗂️ 분류·예측 AI (생성형 아님)</span>
    <ul>
      <li>기존 콘텐츠를 <b>분류 · 정렬 · 순위 매기기</b></li>
      <li>스팸 필터</li>
      <li>추천</li>
      <li>사기 탐지</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✍️ 생성형 AI (이 강의의 대상)</span>
    <ul>
      <li><b>새 콘텐츠</b>를 만들어 냄</li>
      <li>transformer 기반 텍스트 모델</li>
      <li>토큰을 하나씩(one token at a time) 생성</li>
    </ul>
  </div>
</div>

## 2. 기계의 멘탈 모델 — 네 가지 속성

<div class="oa-eli">
  <span class="oa-eli__head">🌡️ 쉽게 말하면</span>
  <p>"이 사람 믿을 만해?"는 좋은 질문이 아니에요. "요리는 잘하는데 길은 자주 헷갈려" 같은 식으로 <b>분야별 눈금</b>으로 봐야 정확하죠. AI도 켜짐/꺼짐 스위치가 아니라 네 개의 눈금자로 봐요.</p>
</div>

AI가 나를 위해 할 수 있는 일과 없는 일을 결정하는 네 가지 속성이 있어요. 각각은 **스펙트럼** 위에 있고, 오른쪽으로 갈수록 더 검증하고 보완해야 해요.

{% include capability-spectrum.html title="네 가지 속성" subtitle="오른쪽으로 갈수록 검증·보완이 필요" %}

| 속성 | 질문 | 능력 쪽 (왼쪽) | 한계 쪽 (오른쪽) |
|---|---|---|---|
| **Next Token Prediction** | 답이 어디서 오는가? | 요약·재구성·흔한 개념 설명 같은 익숙한 길 | 새롭고 희소한 영역, "참 vs 그럴듯함" 구분 |
| **Knowledge** | AI가 실제로 아는 것은? | 자주·최근·일관되게 학습된 주류 주제, 인기 언어 | 드물거나 컷오프 이후·niche·지역적·논쟁적 주제 |
| **Working Memory** | 지금 무엇에 주의를 두는가? | 자료가 여유 있게 들어가고, 세션이 최신이며, 맥락을 직접 제공 | 아주 긴 문서·대화, 세션 간 연속성 기대(the cliff) |
| **Steerability** | 내가 얼마나 통제하는가? | 짧고 구체적이고 검증 가능한 지시("표로", "100단어 이하") | 긴 추론 사슬, 추상적 요청, 네이티브 수준 정밀도 |

### 같은 메커니즘이 능력과 한계를 동시에 만들어요

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📏</span><span class="oa-card__title">연속선이에요</span>각 속성은 "된다/안 된다"가 아니라 능력에서 한계로 이어지는 눈금이에요</div>
  <div class="oa-card"><span class="oa-card__icon">🪙</span><span class="oa-card__title">동전의 양면</span>능력을 주는 바로 그 메커니즘이 한계도 함께 줘요</div>
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">보정된 신뢰</span>신뢰를 통째로 주거나 거두지 않고, 작업의 위치를 정해요</div>
</div>

> AI는 한결같이 유능하지도, 한결같이 불안정하지도 않아요. **네 가지 예측 가능한 축**을 따라 강하고 약해요.

## 3. 실습 — 생성형일까, 아닐까?

생성형 AI는 스팸을 걸러 주고 다음 영상을 추천하는 AI와 근본적으로 달라요. 이 구분을 내 경험에 직접 써 봐요.

<ol class="oa-flow">
  <li><strong>기능 5개 적기</strong>이번 주에 써 본 AI 기능. 자동완성, 사진 태깅, 스팸 필터, 챗봇 답변, 번역, 상품 추천, 음성 비서 등 폭넓게</li>
  <li><strong>내 판정 적기</strong>새 콘텐츠를 만드나, 기존 콘텐츠를 정렬·순위·분류하나</li>
  <li><strong>AI에게 검토받기</strong>틀리거나 헷갈린 것은 한 문장 설명 요청</li>
  <li><strong>작업 목록에 태그</strong>1강의 작업마다 가장 관련 있어 보이는 속성 질문 붙이기</li>
</ol>

3단계에서 추가로 이렇게 물어봐요: "이 다섯 개 중, 이 강의가 이해를 도와줄 실패 방식을 가질 가능성이 가장 큰 것은?"

4단계에서 붙일 태그는 네 가지 질문 중 하나예요.

| 태그할 질문 | 속성 |
|---|---|
| 답이 어디서 오는가? | Next Token Prediction |
| 무엇을 아는가? | Knowledge |
| 무엇에 주의를 두는가? | Working Memory |
| 내가 얼마나 통제하는가? | Steerability |

> 맞히는 게 목적이 아니에요. 다음 네 레슨에서 **검증해 볼 예측**을 만들어 두는 거예요.

**돌아보기**

- 생성/분류 구분이, 내가 쓰는 어떤 도구에 대한 생각을 바꿨나요?
- 작업 목록에 붙인 태그를 보세요. 둘 이상의 속성에 속할 것 같은 작업이 있었나요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 다음 중 생성형 AI에 해당하는 것은?</summary>
  <p>① 스팸 메일 필터 ② 영상 추천 ③ 카드 사기 탐지 ④ <b>질문에 대한 답을 토큰 단위로 새로 써 내는 텍스트 모델</b></p>
  <p><b>정답: ④</b> — ①~③은 기존 콘텐츠를 분류·정렬·순위 매기는 AI이고, 생성형은 새 콘텐츠를 만들어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 속성과 질문</summary>
  <p><b>Next Token Prediction</b> → 답이 어디서 오는가?<br><b>Knowledge</b> → AI가 실제로 아는 것은?<br><b>Working Memory</b> → 지금 무엇에 주의를 두는가?<br><b>Steerability</b> → 내가 얼마나 통제하는가?</p>
</details>

<details class="oa-quiz">
  <summary>Q3. "보정된 신뢰(calibrated trust)"의 뜻으로 알맞은 것은?</summary>
  <p>① AI를 전적으로 믿기 ② AI를 전혀 믿지 않기 ③ <b>작업을 각 속성의 연속선 위 어디에 놓을지 정하기</b> ④ 최신 모델만 믿기</p>
  <p><b>정답: ③</b> — 신뢰를 통째로 주거나 거두는 것이 아니에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 스펙트럼에서 오른쪽(한계 쪽)으로 갈수록 무엇을 더 해야 하나요?</summary>
  <p><b>정답: 더 검증하고 보완해야 해요.</b> 예를 들어 아주 긴 문서, 컷오프 이후 주제, 긴 추론 사슬이 오른쪽에 있어요.</p>
</details>

## 정리

- 생성형 AI는 기존 콘텐츠를 분류하는 게 아니라 **새 콘텐츠를 생성**해요.
- AI는 한결같이 유능하지도, 한결같이 불안정하지도 않아요. **네 가지 예측 가능한 축**(Next Token Prediction · Knowledge · Working Memory · Steerability)을 따라 강하고 약해요.
- 각 속성은 연속선이에요. **같은 메커니즘이 능력과 한계를 동시에** 만들어요.
- 보정된 신뢰란 신뢰를 통째로 주거나 거두는 게 아니라, 작업을 이 연속선 위 어디에 놓을지 정하는 것이에요.
- 다음 레슨은 네 속성에 들어가기 전에, AI가 **왜 공손하고 도움이 되려 하고 때로 너무 쉽게 동의하는지** — 성격이 만들어지는 과정을 다뤄요.
