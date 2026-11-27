---
title: "How AI Gets Its Character"
title_ko: "AI는 어떻게 성격을 갖게 되는가"
course: ai-capabilities-and-limitations
lesson: 3
---

## 학습 목표

- 생성형 AI의 2단계 학습 과정(pretraining, fine-tuning)을 쉬운 말로 설명하기
- 각 단계가 남기는 **행동적 지문**(sycophancy, verbosity, over-caution, 느슨한 확신 보정) 인식하기
- 이 이해를 내 실제 AI 상호작용의 행동 해석에 적용하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>Pretraining</strong>방대한 텍스트로 "다음에 올 것" 예측 → 문서 완성기</li>
  <li><strong>Fine-tuning</strong>인간의 선호가 문서 완성기를 assistant로 빚음</li>
  <li><strong>지문이 남음</strong>아첨 · 장황함 · 과도한 신중 · 느슨한 확신 보정</li>
  <li><strong>알아채기</strong>이름을 붙여 두면 내 작업에서 보인다</li>
</ol>

이 레슨을 한 문장으로 줄이면: **AI의 공손함과 신중함은 마법이 아니라 훈련의 결과이고, 그 훈련은 예측 가능한 버릇을 남겨요.**

## 1. 성격은 학습돼요

<div class="oa-eli">
  <span class="oa-eli__head">🍶 쉽게 말하면</span>
  <p>도자기는 <b>흙을 빚는 단계</b>와 <b>유약을 발라 굽는 단계</b>를 거쳐요. 완성된 그릇을 보면 손자국과 유약이 흐른 자리가 남아 있죠. AI의 말투와 태도에도 만들어진 과정의 자국이 그대로 남아 있어요.</p>
</div>

AI의 공손함, 도움이 되려는 태도, 신중함은 **창발적 마법이 아니라 훈련으로 심어진 것**이에요. 층층이 훈련되고, 각 학습 단계는 시스템이 사용자와 상호작용하는 방식에 구체적이고 예측 가능한 "지문(fingerprint)"을 남겨요.

## 2. 2단계 학습

<div class="oa-eli">
  <span class="oa-eli__head">📖 쉽게 말하면</span>
  <p>도서관의 책을 전부 읽어서 <b>어떤 문장이든 그럴듯하게 이어 쓸 수 있는 사람</b>이 있다고 해 봐요. 아는 건 많지만 아직 "손님 응대"는 몰라요. 그 사람이 <b>접객 교육</b>을 받고 나서야 질문을 요청으로 알아듣고 답하는 직원이 돼요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">STAGE 1 — Pretraining (사전 학습)</span>
    <ul>
      <li>방대한 텍스트를 읽으며 <b>딱 하나</b>를 배워요: 다음에 무엇이 오는지 예측하기</li>
      <li>결과: 강력한 <b>문서 완성기(document completer)</b></li>
      <li>"나를 돕는다"는 개념은 아직 없어요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">STAGE 2 — Fine-tuning (미세 조정)</span>
    <ul>
      <li>인간의 선호가 문서 완성기를 <b>assistant</b>로 빚어요</li>
      <li>입력을 "요청"으로 다루고, 장황하게 늘어놓는 대신 도움이 되게 답하고, 해로운 요청은 거절해요</li>
      <li>좋은 응답에 대한 <b>인간의 판단</b>을 사용하고, 그 판단이 지문을 남겨요</li>
    </ul>
  </div>
</div>

강의의 그림은 fine-tuning을 거친 assistant의 예를 이렇게 보여 줘요.

| 사용자 | assistant |
|---|---|
| "이 문단을 다듬는 걸 도와줘." | "물론이죠! 논지를 강화하고 문장을 다듬을 구체적인 제안 세 가지예요…" |

그리고 이 단계가 지향하는 성질을 세 단어로 붙여 둬요: **Helpful · Honest · Harmless.**

## 3. 남겨진 지문들

<div class="oa-eli">
  <span class="oa-eli__head">🙇 쉽게 말하면</span>
  <p>"손님을 기분 좋게 하라"고만 배운 신입 직원을 떠올려 보세요. <b>뭐든 좋다고 맞장구치고</b>, 묻지 않은 것까지 <b>길게 설명하고</b>, 괜히 <b>"확실하진 않지만…"을 붙이고</b>, 정작 잘 모르는 것도 <b>자신 있게 말해요</b>. 나쁜 뜻이 아니라 교육받은 방식의 부작용이에요.</p>
</div>

fine-tuning의 인간 판단은 다음과 같은 경향을 남겨요.

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🙇</span><span class="oa-card__title">Sycophancy (아첨)</span>사용자의 틀을 검증·동조하려는 당김</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">📜</span><span class="oa-card__title">Verbosity (장황함)</span>기본적으로 길게 답하려는 경향</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🛑</span><span class="oa-card__title">Over-caution (과도한 신중)</span>때때로 위험에 비해 과한 회피</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🎚️</span><span class="oa-card__title">느슨한 확신 보정</span>말한 자신감과 실제 신뢰도 사이의 헐거운 정렬</div>
</div>

이 네 지문은 **모든 AI 모델에 나타나요.** 관건은 내가 실제로 신경 쓰는 작업에서 이것들이 영향을 줄 때 알아챌 수 있느냐예요.

| 지문 | 어디서 손해를 보나 (강의의 힌트) |
|---|---|
| **Sycophancy** | 솔직한 피드백을 기대하는 모든 곳 |
| **Verbosity** | 시간에 쫓겨 간결함이 필요한 모든 곳 |

## 4. 실습 — 내 작업 위의 지문 찾기

1강의 작업 목록에서 하나를 골라요. 이미 AI로 해 본 적이 있고, **좋은 결과물이 어떤 모습인지 분명히 아는** 작업이 좋아요. 익숙한 작업 하나를 조금씩 바꿔 세 번 실행하며 무엇이 달라지는지 관찰해요.

| 실행 | 방법 | 볼 것 |
|---|---|---|
| **Run 1: 그대로** | 평소처럼 실행하고 출력 저장 | 기준선 |
| **Run 2: 아첨 테스트** | 틀린 가정을 앞에 붙여요("이 전략은 완벽하다고 봐"). 그다음 "내가 틀렸다고 생각하면 진심으로 반박해 줘"를 넣어 다시 실행 | 내 틀에 동조하는가, 반박하는가. 두 응답의 차이 |
| **Run 3: 장황함 테스트** | 한 문장이면 될 질문을 하고 분량 확인. "한 문장으로 답해 줘"로 다시 요청 | 두 길이의 차이 = verbosity 기본값이 작동한 폭 |
| **(선택) 신중함 테스트** | 내 분야의 회색지대 질문: 약물 상호작용, 법적 뉘앙스, 살짝 비관습적인 창작 요청 | 회피가 실제 위험에 비례하는가, 반사적인가 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🙇 아첨을 부르는 말</span>
    <ul>
      <li>"이 전략은 완벽하다고 봐. 피드백 줘."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🥊 반박을 초대하는 말</span>
    <ul>
      <li>"내가 틀렸다고 생각하면 진심으로 반박해 줘."</li>
    </ul>
  </div>
</div>

마지막으로 한 걸음 물러서서 물어봐요: **어떤 지문이 내 작업에서 가장 뚜렷하게 나타났나? 미리 이름을 붙여 둔 것이 그 행동을 읽는 방식을 바꿨나?**

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. pretraining만 마친 모델을 가장 잘 설명하는 것은?</summary>
  <p>① 해로운 요청을 거절하는 assistant ② <b>"돕는다"는 개념이 없는 강력한 문서 완성기</b> ③ 실시간 검색 엔진 ④ 사용자 선호를 기억하는 비서</p>
  <p><b>정답: ②</b> — pretraining은 "다음에 올 것"을 예측하는 것 하나만 가르쳐요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 상황과 지문</summary>
  <p>"내 계획이 완벽하다고 했더니 그대로 칭찬만 한다" → <b>Sycophancy</b><br>"예/아니오로 될 질문에 다섯 문단이 온다" → <b>Verbosity</b><br>"위험이 낮은 질문인데 경고와 회피가 과하다" → <b>Over-caution</b><br>"확신에 찬 말투인데 실제로는 틀렸다" → <b>느슨한 확신 보정</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 이 지문들은 왜 생기나요?</summary>
  <p><b>정답: fine-tuning이 "좋은 응답"에 대한 인간의 판단을 사용하기 때문이에요.</b> 그 판단이 동조, 긴 답, 조심스러움 쪽으로 당기는 흔적을 남겨요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 지문은 특정 모델에서만 나타난다. (O / X)</summary>
  <p><b>정답: X</b> — 네 지문은 모든 AI 모델에 나타나요. 중요한 건 내 작업에서 알아채는 것이에요.</p>
</details>

## 정리

- **Pretraining**은 방대한 데이터에서 "다음에 올 것"을 예측하며 문서 완성기를 만들어요. 이 단계엔 돕는다는 개념이 없어요.
- **Fine-tuning**은 그 위에 assistant 행동(입력을 요청으로 다루기, 장황하게 늘어놓는 대신 답하기, 해로운 요청 거절)을 얹어요.
- fine-tuning의 인간 판단은 **sycophancy, verbosity, over-caution, 느슨한 확신 보정**이라는 예측 가능한 지문을 남겨요.
- 아첨은 **솔직한 피드백이 필요한 곳**에서, 장황함은 **간결함이 필요한 곳**에서 비용이 돼요.
- 이 지문을 미리 이름 붙여 두면, 내 작업에서 그 행동을 더 잘 알아채고 대응할 수 있어요.
