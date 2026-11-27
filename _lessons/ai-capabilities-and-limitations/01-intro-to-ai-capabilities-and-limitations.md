---
title: "Intro to AI Capabilities and Limitations"
title_ko: "AI 능력과 한계 소개"
course: ai-capabilities-and-limitations
lesson: 1
---

## 학습 목표

- 이 강의가 무엇을 다루고 어떻게 구성되는지 이해하기
- 모델과 제품이 계속 바뀌어도 이 내용이 왜 오래 유효한지 설명하기
- Capabilities & Limitations 프레임워크와 4D 프레임워크가 어떻게 함께 작동하는지 보기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>여기서 말하는 AI</strong>생성형 AI는 다른 AI와 무엇이 다른가</li>
  <li><strong>AI의 학습 과정</strong>pretraining · fine-tuning이 "성격"을 만든다</li>
  <li><strong>네 가지 속성</strong>Next Token Prediction · Knowledge · Working Memory · Steerability</li>
  <li><strong>속성의 충돌</strong>실제 상황에서 속성이 만나면 생기는 일</li>
  <li><strong>다음 단계</strong>안전하고 효과적이고 윤리적으로 쓰기</li>
</ol>

이 코스를 한 문장으로 줄이면: **AI를 "잘 쓰는 법"보다 먼저, AI라는 기계가 어떻게 작동하고 어디서 강하고 약한지를 알아서 신뢰를 보정(calibrated trust)한다.**

## 1. 두 프레임워크는 하나의 시스템이에요

<div class="oa-eli">
  <span class="oa-eli__head">🚗 쉽게 말하면</span>
  <p>운전을 잘하려면 <b>운전 기술</b>(핸들 잡는 법, 차선 바꾸는 법)과 <b>차에 대한 이해</b>(브레이크가 얼마나 밀리는지, 사각지대가 어디인지)가 둘 다 필요해요. 4D 프레임워크가 운전 기술이라면, 이 강의는 차가 어떻게 생겼는지 알려 주는 쪽이에요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🧑 4D 프레임워크 — 사람의 역량</span>
    <ul>
      <li><b>내가</b> AI와 어떻게 협업하는지를 가르쳐요</li>
      <li>human competencies</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🤖 이 강의 — 기계의 속성</span>
    <ul>
      <li><b>AI가</b> 나와 어떻게 함께 일할 수 있는지를 가르쳐요</li>
      <li>machine properties</li>
    </ul>
  </div>
</div>

둘은 따로 노는 내용이 아니라 **하나의 시스템**이에요. 한쪽은 사람의 역량(4D), 다른 쪽은 그 역량이 대응하는 기계의 속성이에요.

즉, AI를 "잘 쓰는 법"만이 아니라 **AI라는 기계가 실제로 어떻게 작동하고 어디서 강하고 약한지**를 이해해서 신뢰를 보정(calibrated trust)하는 것이 이 강의의 목표예요.

## 2. 기계에 대한 멘탈 모델 — 코스 로드맵

<div class="oa-eli">
  <span class="oa-eli__head">🗺️ 쉽게 말하면</span>
  <p>처음 가는 도시에서 골목 이름을 다 외울 필요는 없어요. <b>큰 지도 한 장</b>만 머리에 있으면 길을 잃어도 어디쯤인지 알 수 있죠. 이 강의는 AI에 대한 그 지도 한 장을 만들어 줘요.</p>
</div>

강의는 다음 질문들을 차례로 풀어 가요.

| 구간 | 답하는 질문 |
|---|---|
| **What we mean by AI** | 생성형 AI란 무엇이며 다른 유형의 AI와 어떻게 다른가? |
| **How AI is trained** | pretraining과 fine-tuning은 AI에게 어떻게 "성격"을 부여하는가? |
| **Properties of AI** | **Next Token Prediction, Knowledge, Working Memory, Steerability**는 각각 무엇인가? |
| **Putting it all together** | 실제 상황에서 이 속성들이 충돌하면(properties collide) 어떤 일이 벌어지는가? |
| **Next steps** | 이 지식을 써서 AI를 안전하고, 효과적이고, 윤리적으로 쓰려면? |

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">✍️</span><span class="oa-card__title">Next Token Prediction</span>답이 어디서 오는가</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">Knowledge</span>AI가 실제로 아는 것은 무엇인가</div>
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">Working Memory</span>지금 무엇에 주의를 두는가</div>
  <div class="oa-card"><span class="oa-card__icon">🎛️</span><span class="oa-card__title">Steerability</span>내가 얼마나 통제하는가</div>
</div>

## 3. 왜 오래 유효한가

<div class="oa-eli">
  <span class="oa-eli__head">🌊 쉽게 말하면</span>
  <p>해안선은 밀물과 썰물에 따라 매일 조금씩 달라지지만, <b>바다와 육지가 있다는 사실</b>은 그대로예요. 모델이 좋아지면 경계선은 움직여도, 속성 자체는 남아요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🔄 계속 바뀌는 것</span>
    <ul>
      <li>모델 버전</li>
      <li>제품 기능</li>
      <li>능력과 한계의 정확한 경계선</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🪨 그대로 남는 것</span>
    <ul>
      <li>"토큰을 하나씩 예측해 만든다"는 근본 메커니즘</li>
      <li>거기서 나오는 능력·한계의 패턴</li>
      <li>네 가지 속성 그 자체</li>
    </ul>
  </div>
</div>

그래서 특정 기능의 사용법보다 **이 기계의 속성을 이해하는 것**이 더 오래가요. 강의의 표현으로는, 경계는 이동하지만 속성은 그대로예요.

## 4. 실습 — 지금 내 AI 활용 지도 그리기

이 실습은 뒤따르는 모든 레슨 실습의 **토대**예요. 여기서 만든 작업 목록을 매 레슨마다 다시 꺼내요.

<ol class="oa-flow">
  <li><strong>작업 4~6개 적기</strong>최근 2주간 AI로 실제 한 일. 아직 많이 안 썼다면 해 보고 싶은 일</li>
  <li><strong>한 줄 메모</strong>첫 시도에 쓸 만했나, 손봐야 했나 (가벼운 직감이면 충분)</li>
  <li><strong>AI에게 묻기</strong>"각 작업에서, 내가 주의하지 않으면 잘못될 수 있는 방식 하나씩은?"</li>
  <li><strong>목록 보관</strong>매 레슨마다 돌아와 주석을 덧붙이기</li>
</ol>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 너무 넓은 작업 이름</span>
    <ul>
      <li>"글쓰기"</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 구체적인 작업 이름</span>
    <ul>
      <li>"프로젝트 지연을 설명하는 고객 이메일 초안 작성"</li>
    </ul>
  </div>
</div>

AI가 말한 실패 방식이 내 경험과 맞지 않으면 그냥 넘기지 말고 반박해요: "그건 내 경험과 달라요. 실제로는 이런 일이 있었어요…"

**돌아보기**

- 적은 작업 중 어떤 것이 AI에게 맡기기 "안전하게" 느껴지고, 어떤 것이 위험하게 느껴지나요? 그 이유를 아직 말로 설명할 수 있나요?
- 그때는 설명할 수 없었던 AI의 행동(좋은 것이든 나쁜 것이든) 하나는 무엇인가요?

> 강의 자료로 **AI Capabilities & Limitations Framework Overview** PDF가 함께 제공돼요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 4D 프레임워크와 이 강의의 관계로 가장 알맞은 것은?</summary>
  <p>① 이 강의가 4D를 대체한다 ② 서로 관련 없는 별개 주제다 ③ <b>4D는 사람의 역량, 이 강의는 그 역량이 대응하는 기계의 속성을 다루는 한 시스템의 두 반쪽이다</b> ④ 4D는 개발자용, 이 강의는 비개발자용이다</p>
  <p><b>정답: ③</b> — 한쪽은 human competencies, 다른 쪽은 machine properties예요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 모델이 계속 바뀌는데도 이 강의 내용이 오래 유효한 이유는?</summary>
  <p><b>정답: 경계는 이동해도 속성은 그대로이기 때문이에요.</b> 모델이 좋아지면 능력과 한계의 경계선은 움직이지만, 네 가지 속성과 근본 메커니즘은 유지돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 첫 실습에서 작업을 "글쓰기"가 아니라 "프로젝트 지연을 설명하는 고객 이메일 초안"처럼 적으라고 하는 이유는?</summary>
  <p><b>정답: 구체적이어야 무언가를 알려 주기 때문이에요.</b> 이 목록은 이후 모든 레슨에서 속성별로 다시 살펴보는 재료라서, 뭉뚱그린 이름으로는 분석할 수 없어요.</p>
</details>

## 정리

- 이 강의는 "기계로서의 AI"가 어떻게 작동하는지를 다루고, 4D 프레임워크(사람의 역량)와 짝을 이뤄요. 4D가 사람의 역량을 설명한다면, 이 강의는 그 역량이 대응하는 **기계의 속성**을 설명해요.
- 생성형 AI의 능력·한계는 네 가지 속성(**Next Token Prediction · Knowledge · Working Memory · Steerability**)으로 예측 가능하게 설명돼요.
- 이 내용이 오래가는 이유: 모델이 좋아져도 **속성은 그대로**이고 경계만 이동해요.
- 목표는 신뢰를 통째로 주거나 거두는 것이 아니라, 작업을 능력↔한계 스펙트럼 위에 정확히 놓는 **보정된 신뢰**예요.
- 첫 실습에서 만든 **작업 목록**은 코스 내내 다시 쓰는 재료예요.
