---
title: "Next Token Prediction"
title_ko: "다음 토큰 예측"
course: ai-capabilities-and-limitations
lesson: 4
---

## 학습 목표

- Next Token Prediction이 생성형 AI의 핵심 메커니즘이며, 왜 **유창함과 환각을 동시에** 만드는지 설명하기
- 작업을 Next Token Prediction 연속선(익숙한 길 vs 새로운 영역) 위에 놓기
- **구체성(이름·날짜·인용·통계)**이 조작(fabrication)이 집중되는 구간임을 식별하기
- 이 한계를 완화하는 제품 기능(citation, 불확실성 신호, 제약 생성, generator-verifier 패턴) 인식하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>대규모 자동완성</strong>"무엇 뒤에 무엇이 오는가"로 한 단어씩 작성</li>
  <li><strong>능력</strong>유창함 · 종합 · 익숙한 패턴에서 강함</li>
  <li><strong>한계</strong>환각 · 작화 · 비일관성 · 잘못된 확신</li>
  <li><strong>조작은 구체성에</strong>이름 · 날짜 · 통계 · 인용 · URL</li>
  <li><strong>대응</strong>citation · 불확실성 신호 · 제약 생성 · 검증</li>
</ol>

이 레슨을 한 문장으로 줄이면: **AI를 유창하게 만드는 바로 그 생성 과정이 AI를 지어내게도 만들어요. 그래서 주장이 정밀할수록 더 검증해요.**

## 1. 대규모 자동완성

<div class="oa-eli">
  <span class="oa-eli__head">📱 쉽게 말하면</span>
  <p>휴대폰 키보드가 추천하는 다음 단어만 계속 눌러 문장을 만들어 본 적 있나요? 생성형 AI는 그 놀이의 <b>엄청나게 정교한 버전</b>이에요. 사전에서 답을 "찾아오는" 게 아니라, 어울리는 말을 "이어 쓰는" 거예요.</p>
</div>

생성형 AI는 검색 엔진보다 **엄청나게 정교한 자동완성(autocomplete)**에 가까워요. 무엇 뒤에 무엇이 오는 경향이 있는지를 바탕으로 답을 단어 하나씩 써 내려가요. **바로 이 하나의 속성이 유창함과 환각을 동시에** 줘요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🔎 흔한 오해 — 검색 엔진</span>
    <ul>
      <li>어딘가 저장된 정답을 찾아서 돌려준다</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">⌨️ 실제에 가까운 그림 — 자동완성</span>
    <ul>
      <li>지금까지의 글에 이어질 법한 말을 한 단어씩 생성한다</li>
    </ul>
  </div>
</div>

{% include capability-spectrum.html title="Next Token Prediction" subtitle="Where do AI answers come from?" %}

> **읽기 전 질문 (강의의 Before you read)**: 긴 보고서를 AI에게 요약시켰어요. 결과를 얼마나 꼼꼼히 확인해야 할까요? Trust it → Spot-check → Check details → Verify carefully → High risk 중 한 지점을 먼저 골라 보세요.

## 2. 능력과 한계는 같은 뿌리에서 나와요

<div class="oa-eli">
  <span class="oa-eli__head">🎤 쉽게 말하면</span>
  <p>어떤 주제든 막힘 없이 말하는 <b>즉흥 연설가</b>를 생각해 보세요. 막힘이 없다는 건 장점이지만, 모르는 부분에서도 <b>멈추지 않고 그럴듯하게 이어 간다</b>는 뜻이기도 해요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 이것이 가능하게 하는 것 (능력)</span>
    <ul>
      <li>거의 모든 스타일·형식으로 유창하고 자연스러운 텍스트</li>
      <li>서로 먼 분야의 아이디어를 빠르게 종합</li>
      <li>모델이 이전에 본 것과 닮은 작업에서 강한 성능</li>
      <li>어떤 흐름(이야기, 논증, 코드 블록)이든 일관되게 이어 가기</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⚠️ 특징적으로 실패하는 곳 (한계)</span>
    <ul>
      <li><b>환각(Hallucination)</b>: 그럴듯한 이어짐이 항상 참인 것은 아님</li>
      <li><b>작화(Confabulation)</b>: 빈틈을 표시하지 않고 그럴듯한 내용으로 메움</li>
      <li><b>비일관성(Inconsistency)</b>: 샘플링 때문에 같은 prompt가 다른 출력을 낼 수 있음</li>
      <li><b>잘못된 확신(Misplaced confidence)</b>: 매끄러운 문장이 추측을 감쌀 수 있음</li>
    </ul>
  </div>
</div>

### 한계를 밀어내는 제품 기능

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔗</span><span class="oa-card__title">Citation · source grounding</span>무엇이 근거 있는지 vs 생성된 것인지 추적</div>
  <div class="oa-card"><span class="oa-card__icon">🚩</span><span class="oa-card__title">불확실성 신호</span>모델이 자신의 불안정함을 스스로 표시</div>
  <div class="oa-card"><span class="oa-card__icon">🧱</span><span class="oa-card__title">제약 생성 · skills</span>constrained generation으로 조작이 스며들 공간을 좁힘</div>
  <div class="oa-card"><span class="oa-card__icon">🔁</span><span class="oa-card__title">Generator-verifier 루프</span>생성과 검증을 나눠 이 한계를 더 멀리 밀어냄</div>
</div>

## 3. 조작은 "구체성"에 집중돼요

<div class="oa-eli">
  <span class="oa-eli__head">🧾 쉽게 말하면</span>
  <p>"그 식당 꽤 유명해"라는 말은 대충 맞기 쉬워요. 하지만 "2019년 3월에 문을 열었고 전화번호는 …"처럼 <b>딱 떨어지는 정보</b>는 하나만 틀려도 틀린 거죠. 말이 정밀해질수록 틀릴 자리가 늘어나요.</p>
</div>

핵심 통찰: **조작은 구체성에서 집중적으로 발생**해요 — 이름, 날짜, 통계, 인용, URL, 인용문. 주장이 정밀할수록 검증이 더 필요해요.

| 구간 | 어떤 작업인가 | 예 |
|---|---|---|
| **능력 구간** | 모델이 여러 번 본 패턴을 닮은 작업 | 요약, 재구성, 흔한 개념 설명 |
| **한계 구간** | 새롭거나 희소한 영역, 그리고 "참"과 "그럴듯함"을 구분해야 하는 모든 곳 | 이름 · 날짜 · 통계 · 인용 · URL · 인용문 |

> **4D 연결**: Next Token Prediction은 **Discernment(분별)**의 토대예요. 출력이 "생성된" 것임을 알면 어떤 종류의 검증을 적용할지 알 수 있어요.

## 4. Try it out — Markov 친구에게 문자 보내기

강의의 체험 페이지는 "100% 해석 가능한 next-token 생성기"를 직접 만들어 보게 해요.

<ol class="oa-flow">
  <li><strong>"학습"</strong>메시지 몇 개에서 단어 사이의 연결을 세어 표로 만들기 (frequency table)</li>
  <li><strong>확률 분포</strong>표의 각 행을 정규화하면 "다음에 올 것"의 확률</li>
  <li><strong>Sampling</strong>지금까지의 내용을 보고 다음 단어를 고르기</li>
  <li><strong>반복</strong>고른 단어가 새 context가 되어 다시 고르기</li>
</ol>

예를 들어 "i think we should probably ship it", "i think that sounds good", "i think we should probably wait" 같은 메시지 다섯 개만으로도 표가 만들어지고, 이 표에서는 "i" 다음에 "think"가 100%예요. **다음 단어를 고르는 이 행위를 sampling이라 부르고, Claude 같은 현대 언어 모델에서도 같은 용어를 써요.**

sampling 규칙은 여러 가지가 가능해요: 항상 최고 확률 단어 고르기, 확률대로 반쯤 무작위로 고르기, 확률대로 고르되 가장 유력한 선택을 더 밀어 주기, 일정 확률 이하 무시하기, 상위 N개만 고려하기. 체험 페이지에 따르면 이런 전략은 개발자가 Claude에 넘기는 sampling 제약과 거의 같아요.

| 단계 | Markov chain | LLM |
|---|---|---|
| **1. context 읽기** | 마지막 단어 하나 | 지금까지의 대화 전체 |
| **2. 분포 계산** | 표에서 한 행 찾기 | 수십억 parameter를 거치는 forward pass (attention, embedding 등) |
| **3. 다음 토큰 sampling** | 확률 분포에서 고르기 | **같은 과정** |

표 조회는 단순하고 설명 가능하고, 신경망의 forward pass는 훨씬 복잡해요. 하지만 **결과물은 같아요: 다음에 올 법한 단어·토큰의 확률 분포.** sampling은 같고, 학습 방식이 근본적으로 달라요. LLM은 단어를 세는 방식의 설명 가능성을 내주고 훨씬 넓은 context와 훨씬 큰 능력을 얻었어요.

> 체험 페이지의 짧은 역사: Markov가 이 아이디어를 발표한 것이 1906년, 2010년 무렵 이런 n-gram 모델이 휴대폰의 다음 단어 예측(SwiftKey, 이후 Apple QuickType)을 움직였고, 2015년경부터 신경망(먼저 RNN, 2017년 transformer)이 표 조회를 학습된 함수로 대체하기 시작했어요.

## 5. 실습 — 검증 테스트

AI를 유창하게 만드는 생성 과정이 AI를 지어내게도 만든다는 걸, **내가 잡아낼 수 있는 분야**에서 직접 확인해요.

작업 목록에서 내가 가장 전문성 있는 주제를 골라, 검증 가능한 구체적 사실 5개(직함, 출판일, 통계, 제품 사양, 직접 인용, URL)를 준비해요. 내가 정확하다고 알고 있고 독립적으로 확인할 수 있는 것들이어야 해요. 그리고 세 가지를 시도해요.

| Probe | 방법 | 볼 것 |
|---|---|---|
| **1. 능력 구간** | 잘 알려진 개념을 설명·요약시키고 내용을 spot-check | 매끄럽고, 자신 있고, 대체로 정확하다는 느낌 |
| **2. 구체성의 압박** | 출처 3개, 저자명, 정확한 수치, URL을 요구하고 하나하나 검증. 5점 만점 채점 | 몇 개가 완전히 정확한가. 지어냈다면 얼마나 자신 있게 말했나 |
| **3. 샘플링 확인** | 같은 요청을 새 대화에서 다시 실행해 두 출력을 비교 | 무엇이 유지되고 무엇이 바뀌는가 — 그 차이가 sampling이 작동한 흔적 |

**도전 과제**: Probe 2를 citation이 켜진 도구(예: Claude의 Research 모드)에서 다시 실행하고 다시 채점해요. 확인할 출처가 있으면 점수가 달라지나요?

**돌아보기**

- 잘 모르는 분야였다면 이 조작을 잡아낼 수 있었을까요?
- 작업 목록을 보세요. 어떤 작업이 주로 능력 구간에 있고, 어떤 작업이 검증이 필요한 구체성 쪽으로 밀려가나요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 생성형 AI가 유창함과 환각을 "동시에" 갖는 이유는?</summary>
  <p><b>정답: 둘 다 같은 메커니즘에서 나오기 때문이에요.</b> "무엇 뒤에 무엇이 오는가"로 한 단어씩 이어 쓰는 방식은 자연스러운 글을 만들지만, 그럴듯한 이어짐이 항상 참인 것은 아니에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 다음 중 가장 꼼꼼히 검증해야 하는 출력은?</summary>
  <p>① 잘 알려진 개념의 쉬운 설명 ② 내가 준 글의 형식 바꾸기 ③ 내가 준 문서의 요약 ④ <b>저자명, 출판 연도, URL이 포함된 참고 문헌 목록</b></p>
  <p><b>정답: ④</b> — 조작은 이름·날짜·통계·인용·URL 같은 구체성에 집중돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 짝 맞추기 — 실패 방식과 뜻</summary>
  <p><b>Hallucination</b> → 그럴듯한 이어짐이 참이 아님<br><b>Confabulation</b> → 빈틈을 표시하지 않고 그럴듯한 내용으로 메움<br><b>Inconsistency</b> → 같은 prompt가 다른 출력을 냄 (sampling)<br><b>Misplaced confidence</b> → 매끄러운 문장이 추측을 감쌈</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Markov chain과 LLM은 무엇이 같고 무엇이 다른가요?</summary>
  <p><b>정답: 다음 토큰의 확률 분포에서 sampling한다는 점은 같아요.</b> 다른 점은 context의 범위(마지막 단어 vs 대화 전체)와 분포를 계산하는 방식(표 조회 vs 신경망 forward pass)이에요.</p>
</details>

## 정리

- Next Token Prediction은 생성형 AI가 "무엇 뒤에 무엇이 오는지"를 바탕으로 단어 하나씩 답을 쓴다는 사실이에요.
- 능력 구간은 익숙한 패턴(요약·재구성·흔한 개념), 한계 구간은 새롭거나 희소한 영역과 "참 vs 그럴듯함" 구분이 필요한 곳이에요.
- **조작은 구체성(이름·날짜·통계·인용·URL·인용문)에 집중**돼요 — 정밀한 주장일수록 검증해요.
- citation·불확실성 신호·제약 생성·generator-verifier 루프는 이 한계를 밀어내는 장치예요.
- 같은 prompt가 다른 답을 내는 것은 고장이 아니라 **sampling**의 결과예요.
- **4D 연결**: Next Token Prediction은 **Discernment(분별)**의 토대예요 — 출력이 "생성된" 것임을 알면 어떤 종류의 검증을 적용할지 알 수 있어요.
