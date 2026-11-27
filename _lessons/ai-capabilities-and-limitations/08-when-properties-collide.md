---
title: "When Properties Collide"
title_ko: "속성이 충돌할 때"
course: ai-capabilities-and-limitations
lesson: 8
---

## 학습 목표

- 대부분의 AI 실패가 둘 이상의 속성이 상호작용해 생긴다는 점 인식하기
- 흔한 실패 패턴(환각 citation, 긴 대화의 drift, 자신 있게 틀린 수학, 나쁜 전제에 동조)을 어떤 속성이 작용하는지로 진단하기
- 어떤 속성이 제한 요인인지에 따라 표적 처방 적용하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>실패 관찰</strong>무엇을 요청했고 무엇이 왔나</li>
  <li><strong>속성에 이름 붙이기</strong>네 속성 중 어느 둘이 만났나</li>
  <li><strong>표적 처방</strong>검증 · 맥락 재공급 · code 위임 · 반박 초대</li>
  <li><strong>바로 테스트</strong>비슷한 작업에서 처방 확인</li>
</ol>

이 레슨을 한 문장으로 줄이면: **실패는 대개 두 속성이 만나는 지점에서 생기고, 어느 둘인지 이름을 붙이면 꺼낼 처방이 정해져요.**

## 1. 실패는 대개 두 속성의 만남이에요

<div class="oa-eli">
  <span class="oa-eli__head">🚦 쉽게 말하면</span>
  <p>교통사고는 대개 <b>두 길이 만나는 교차로</b>에서 나요. 어느 길과 어느 길이 만난 곳인지 알아야 신호등을 달지, 속도를 줄일지 정할 수 있죠. "사고가 났다"만 알아서는 고칠 수 없어요.</p>
</div>

네 가지 속성은 따로 작동하지 않아요. **실제 실패 대부분은 두 속성이 교차**하는 지점에서 일어나요. 어느 둘인지 이름 붙일 수 있으면, 어떤 처방을 꺼낼지 바로 알 수 있어요.

각 속성을 한 줄로 요약하면 이래요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">✍️</span><span class="oa-card__title">Next Token Prediction</span>그럴듯하게 들리는 것을 생성</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">Knowledge</span>학습된 것을 앎</div>
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">Working Memory</span>가까이 있는 것에 주의</div>
  <div class="oa-card"><span class="oa-card__icon">🎛️</span><span class="oa-card__title">Steerability</span>가장 큰 목소리의 지시를 따름</div>
</div>

## 2. 진단 쌍 (Diagnostic pairs)

<div class="oa-eli">
  <span class="oa-eli__head">🩺 쉽게 말하면</span>
  <p>의사는 "아파요"만 듣고 약을 주지 않아요. <b>열이 있는데 기침도 한다</b>처럼 증상의 조합을 보고 병명을 붙이고, 병명이 정해지면 처방이 따라오죠. AI 실패도 "틀렸네"에서 멈추지 말고 조합에 이름을 붙여요.</p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 200" role="img" aria-label="두 진단 쌍: Next Token Prediction과 Knowledge가 만나면 환각된 구체 정보, Working Memory와 Steerability가 만나면 긴 대화의 drift">
  <defs>
    <marker id="ar_c8" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.b8{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.w8{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.h8{font-size:14px;font-weight:700;fill:#333}.t8{font-size:12px;fill:#4a4a4a}.p8{font-size:20px;font-weight:700;fill:#c99400}</style>
  </defs>
  <rect class="b8" x="10" y="14" width="200" height="64" rx="10"/>
  <text class="h8" x="110" y="40" text-anchor="middle">Next Token Prediction</text>
  <text class="t8" x="110" y="62" text-anchor="middle">그럴듯한 것을 생성</text>
  <text class="p8" x="230" y="53" text-anchor="middle">+</text>
  <rect class="b8" x="250" y="14" width="200" height="64" rx="10"/>
  <text class="h8" x="350" y="40" text-anchor="middle">Knowledge</text>
  <text class="t8" x="350" y="62" text-anchor="middle">학습된 것을 앎</text>
  <line x1="454" y1="46" x2="492" y2="46" stroke="#c99400" stroke-width="2" marker-end="url(#ar_c8)"/>
  <rect class="w8" x="500" y="14" width="210" height="64" rx="10"/>
  <text class="h8" x="605" y="40" text-anchor="middle">환각된 구체 정보</text>
  <text class="t8" x="605" y="62" text-anchor="middle">예: 존재하지 않는 citation</text>
  <rect class="b8" x="10" y="114" width="200" height="64" rx="10"/>
  <text class="h8" x="110" y="140" text-anchor="middle">Working Memory</text>
  <text class="t8" x="110" y="162" text-anchor="middle">가까이 있는 것에 주의</text>
  <text class="p8" x="230" y="153" text-anchor="middle">+</text>
  <rect class="b8" x="250" y="114" width="200" height="64" rx="10"/>
  <text class="h8" x="350" y="140" text-anchor="middle">Steerability</text>
  <text class="t8" x="350" y="162" text-anchor="middle">가장 큰 목소리의 지시를 따름</text>
  <line x1="454" y1="146" x2="492" y2="146" stroke="#c99400" stroke-width="2" marker-end="url(#ar_c8)"/>
  <rect class="w8" x="500" y="114" width="210" height="64" rx="10"/>
  <text class="h8" x="605" y="140" text-anchor="middle">긴 대화의 drift</text>
  <text class="t8" x="605" y="162" text-anchor="middle">초반 지시가 흐려짐</text>
</svg>
</div>

| 진단 쌍 | 생기는 실패 | 예 |
|---|---|---|
| **Next Token Prediction + Knowledge** | **환각된 구체 정보(hallucinated specifics)** | 존재하지 않는 citation을 자신 있게 생성 |
| **Working Memory + Steerability** | **긴 대화의 drift** | 초반 지시가 긴 대화 속에서 흐려짐 |

왜 이런 조합이 되는지는 한 줄 요약을 이어 보면 보여요. 지식이 얇은 자리에서도 생성은 멈추지 않고 그럴듯한 것을 만들어 내고(첫 번째 쌍), 지시는 window에서 멀어질수록 가까이 있는 내용에 밀려요(두 번째 쌍).

## 3. 이름을 붙이면 처방이 나와요

<div class="oa-eli">
  <span class="oa-eli__head">🧰 쉽게 말하면</span>
  <p>물이 새는데 망치를 들면 소용없어요. <b>어디가 문제인지</b> 알아야 맞는 공구를 꺼내죠. 속성에 이름을 붙이는 일이 바로 공구함에서 맞는 공구를 고르는 일이에요.</p>
</div>

작용하는 속성에 이름을 붙이면 처방이 바로 나와요 — **구체 정보를 검증**하거나, **맥락을 재공급**하거나, **code execution에 위임**하거나, **반박을 초대**하는 것이에요.

| 흔한 실패 패턴 | 꺼낼 처방 | 앞 레슨과의 연결 |
|---|---|---|
| 환각 citation | **구체 정보 검증** | 4강: 조작은 구체성(이름·날짜·통계·인용·URL)에 집중 |
| 긴 대화의 drift | **맥락 재공급** | 6강: 핵심 맥락을 다시 넣기, 앞배치 |
| 자신 있게 틀린 수학 | **code execution에 위임** | 7강: 수학은 실제 인터프리터에 |
| 나쁜 전제에 동조 | **반박 초대** | 3강: sycophancy 지문 |

> 이 진단 행위 자체가 **Discernment의 적용**이에요. 어떤 종류의 "틀림"인지 알면 더 잘 평가해요.

## 4. 실습 — 실패 진단

AI 출력이 정말 실망스럽거나 놀라웠던 경험 2~3개를 떠올려요(이 코스에서 관찰한 것도 포함). 각각을 한두 문장으로 적어요: 무엇을 요청했고, 무엇을 받았고, 무엇이 실망스럽거나 놀라웠는지.

<ol class="oa-flow">
  <li><strong>AI와 함께 되짚기</strong>상황을 설명하고 "네 가지 속성 중 어느 것이 작용했고 왜인지" 묻기</li>
  <li><strong>진단 평가하기</strong>내 지식으로 평가하고, 틀렸다면 반박하기</li>
  <li><strong>표적 처방 묻기</strong>"그 진단이라면 가장 표적화된 처방은?" 가능하면 비슷한 작업에서 바로 테스트</li>
</ol>

1단계에서 쓸 질문이에요.

```text
네 가지 속성(Next Token Prediction, Knowledge, Working Memory, Steerability)을 기준으로,
여기서 어느 것들이 작용했을 가능성이 크고 왜 그런가요?
```

2단계에서는 3강의 **sycophancy 지문**을 기억해요. AI가 내 틀에 너무 쉽게 동조할 수 있어요. 틀렸다고 생각하면 그렇다고 말해요.

마지막으로 1강의 작업 목록과 그동안 쌓인 주석을 모두 펼쳐요.

| 레슨 | 쌓인 주석 |
|---|---|
| 2강 | 속성 태그 |
| 4강 | 검증 점수 |
| 5강 | 지식 플래그 (기댈 수 있나, 내가 가져와야 하나) |
| 6강 | 맥락 필요 여부 |
| 7강 | 목표 문장 |

가장 애먹인 작업들에 대해 **어느 두 속성이 충돌했는지** 이름을 붙이고, 그 진단을 작업 옆에 적어요.

**돌아보기**

- 속성 쌍에 이름을 붙인 것이 꺼낼 처방을 바꿨나요? 이 코스 전이라면 다른(덜 효과적인) 처방을 시도했을까요?
- 일상 업무에서 가장 자주 만날 것 같은 속성 쌍은 무엇인가요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. AI가 존재하지 않는 논문을 저자명과 연도까지 붙여 자신 있게 인용했다. 어느 두 속성의 만남인가요?</summary>
  <p><b>정답: Next Token Prediction + Knowledge.</b> 환각된 구체 정보예요. 처방은 구체 정보를 하나하나 검증하는 것이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 대화 초반에 "항상 존댓말로, 300자 이내"라고 정했는데 대화가 길어지자 지켜지지 않는다. 진단과 처방은?</summary>
  <p><b>정답: Working Memory + Steerability (긴 대화의 drift), 처방은 맥락 재공급.</b> 초반 지시가 긴 대화 속에서 흐려진 것이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 짝 맞추기 — 실패 패턴과 처방</summary>
  <p>환각 citation → <b>구체 정보 검증</b><br>긴 대화의 drift → <b>맥락 재공급</b><br>자신 있게 틀린 수학 → <b>code execution에 위임</b><br>나쁜 전제에 동조 → <b>반박 초대</b></p>
</details>

<details class="oa-quiz">
  <summary>Q4. AI에게 실패 원인을 진단시킬 때 조심할 점은?</summary>
  <p><b>정답: AI가 내 설명의 틀에 너무 쉽게 동조할 수 있어요 (sycophancy 지문).</b> 진단을 내 지식으로 평가하고, 틀렸다고 생각하면 반박해요.</p>
</details>

## 정리

- 실제 실패는 보통 한 속성이 아니라 **두 속성의 상호작용**이에요.
- 기억할 진단 쌍: NTP+Knowledge(환각된 구체 정보), Working Memory+Steerability(긴 대화 drift).
- 작용하는 속성에 이름을 붙이면 처방(구체 정보 검증·맥락 재공급·code 위임·반박 초대)이 바로 나와요.
- AI에게 진단을 맡길 때도 sycophancy를 기억하고, 진단은 내가 평가해요.
- 이 진단 행위 자체가 **Discernment의 적용**이에요 — 어떤 종류의 "틀림"인지 알면 더 잘 평가해요.
