---
title: "Knowledge"
title_ko: "지식 (Knowledge)"
course: ai-capabilities-and-limitations
lesson: 5
---

## 학습 목표

- AI 모델의 지식이 학습 중 어떻게 형성되고 왜 고정된 컷오프를 갖는지 설명하기
- 어떤 주제가 능력 구간(자주·최근·일관)인지, 가장자리(드묾·컷오프 이후·niche·논쟁적)인지 예측하기
- staleness, 고르지 않은 coverage, 상속된 bias, source amnesia를 특징적 지식 실패로 식별하기
- 이 한계를 다루는 제품 기능(web search, retrieval/RAG, tool use) 인식하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>학습된 것만 안다</strong>실시간 브라우징도, 체험도 없음</li>
  <li><strong>컷오프에서 멈춤</strong>그 이후는 도구 없이는 접근 불가</li>
  <li><strong>고르지 않은 지식</strong>주류는 두텁고, niche·지역·최신은 얇음</li>
  <li><strong>대응</strong>web search · RAG/MCP · tool use로 메우기</li>
</ol>

이 레슨을 한 문장으로 줄이면: **"AI가 이걸 아는가?"가 아니라 "이것이 모델이 읽은 것 안에서 얼마나 잘 대표되어 있었나?"를 물어요.**

## 1. 모델이 읽은 것, 그리고 읽기를 멈춘 시점

<div class="oa-eli">
  <span class="oa-eli__head">🧳 쉽게 말하면</span>
  <p>세상의 책을 엄청나게 읽고 나서 <b>어느 날 외딴섬으로 떠난 사람</b>을 떠올려 보세요. 아는 건 정말 많지만, 떠난 날 이후의 소식은 몰라요. 그리고 많이 읽은 주제는 깊이 알고, 책이 몇 권 없던 주제는 얕게 알아요.</p>
</div>

모델은 **학습 중 노출된 것만** 알아요. 기본적으로 실시간 브라우징도, 체험된 경험도 없고, **knowledge cutoff에서 딱 멈춰요.** 그래서 실용적인 질문은 "AI가 이걸 아는가?"가 아니라 **"이것이 모델이 읽은 것 안에서 얼마나 잘 대표되어 있었나?"**예요.

{% include capability-spectrum.html title="Knowledge" subtitle="What does AI actually know?" %}

> **읽기 전 질문 (강의의 Before you read)**: 지난주에 일어난 뉴스를 AI에게 설명해 달라고 했어요. 결과를 얼마나 꼼꼼히 확인해야 할까요? Trust it → Spot-check → Check details → Verify carefully → High risk 중 한 지점을 먼저 골라 보세요.

## 2. 능력과 한계

<div class="oa-eli">
  <span class="oa-eli__head">📚 쉽게 말하면</span>
  <p>동네 도서관을 생각해 보세요. <b>인기 분야 서가는 빽빽하고</b>, 우리 동네 향토사 서가는 책이 몇 권뿐이에요. 게다가 신간은 아직 안 들어왔고, 책을 읽은 사람은 "어디서 읽었더라?"를 잘 기억하지 못해요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 이것이 가능하게 하는 것 (능력)</span>
    <ul>
      <li>대단히 넓은 일반 지식</li>
      <li>잘 대표된 분야에서의 깊은 역량</li>
      <li>분야를 가로지르는 연결 (embedding: 관련 개념이 가까이 모임)</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⚠️ 특징적으로 실패하는 곳 (한계)</span>
    <ul>
      <li><b>Knowledge cutoff</b>: 학습 종료 이후의 것은 없음</li>
      <li><b>Staleness(진부화)</b>: 학습 시점엔 참이었어도 지금은 아닐 수 있음</li>
      <li><b>고르지 않은 coverage</b>: niche 분야·지역 지식이 취약</li>
      <li><b>상속된 bias</b>: 기본값이 학습 데이터의 사각지대를 반영</li>
      <li><b>Source amnesia</b>: "어디선가 읽었다"는 citation이 아님</li>
    </ul>
  </div>
</div>

상속된 bias는 구체적으로, 무엇을 **"기본값"이나 "정상"으로 취급하느냐**에서 드러나요. source amnesia는 지식이 **어디서 왔는지 귀속하지 못한다**는 뜻이에요.

### 한계를 밀어내는 제품 기능

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🌐</span><span class="oa-card__title">Web search</span>시간에 민감한 질문에서 컷오프를 우회</div>
  <div class="oa-card"><span class="oa-card__icon">📂</span><span class="oa-card__title">Retrieval (RAG) / MCP</span>학습된 적 없는 자료를 끌어옴</div>
  <div class="oa-card"><span class="oa-card__icon">🧮</span><span class="oa-card__title">Tool use</span>실제 계산기·데이터베이스·API 호출</div>
  <div class="oa-card"><span class="oa-card__icon">📅</span><span class="oa-card__title">명시적 cutoff 고지</span>무엇을 재확인할지 알 수 있음</div>
</div>

이 기능들은 모델이 **학습한 적 없는 정보**에 접근하게 해서 지식의 간극을 메우려고 존재해요.

## 3. 능력 구간 vs 한계 구간

<div class="oa-eli">
  <span class="oa-eli__head">🛣️ 쉽게 말하면</span>
  <p>차가 많이 다니는 <b>큰길</b>은 포장도 잘돼 있고 표지판도 많아요. 사람이 잘 안 다니는 <b>샛길</b>은 지도에 없거나 예전 모습으로 그려져 있죠. 내 질문이 큰길 위에 있는지 샛길에 있는지 먼저 가늠해 봐요.</p>
</div>

| 구간 | 조건 | 예 |
|---|---|---|
| **능력 구간** | 학습 데이터에 **자주 · 최근(학습 기간 안에서) · 일관되게** 나타난 주제 | 주류 주제, 인기 언어 |
| **한계 구간** | 드물거나, 컷오프 이후이거나, niche · 지역적 · 논쟁적인 주제 | 업계 전문 용어, 지역 규제, 작년에 바뀐 것 |

> **4D 연결**: 지식의 불균형은 **Delegation**의 핵심이에요. 모델이 어디서 든든하고 어디서 얄팍한지 알면, 언제 맡기고, 언제 맥락을 직접 주고, 언제 다른 곳으로 갈지 판단할 수 있어요.

## 4. Try it out — 1024차원 공간을 그려 보기

능력 목록의 "embedding: 관련 개념이 가까이 모임"을 직접 만져 보는 체험 페이지예요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🔤 문자열 검색</span>
    <ul>
      <li>"car"를 검색하면 "car"가 들어간 문서만 찾음</li>
      <li>"automobile", "vehicle", "내 Civic 브레이크 교체"는 못 찾음</li>
      <li>동의어 사전, 어간 추출(stemming) 규칙 등을 거의 손으로 이어 붙여야 했음</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">📍 Embedding</span>
    <ul>
      <li>의미를 <b>위치</b>로 바꿈: 텍스트 → 좌표</li>
      <li>비슷한 개념이 서로 가까이 놓임</li>
      <li>이 지도는 손으로 만든 게 아니라 학습 데이터에서 저절로 생김</li>
    </ul>
  </div>
</div>

<ol class="oa-flow">
  <li><strong>Encoding</strong>문서를 축 위의 점으로 놓기 (예: 공룡 관련도 × 롤러코스터 관련도)</li>
  <li><strong>질문도 같은 방식으로</strong>질문을 같은 축 위에 점으로 놓기</li>
  <li><strong>Retrieval</strong>질문에서 가장 가까운 k개 문서 가져오기</li>
  <li><strong>차원 늘리기</strong>축이 늘어도 "가장 가까운 것 찾기"는 그대로</li>
</ol>

체험 페이지가 짚는 점들이에요.

- 예시는 어린이 공룡 책, Velocicoaster 웹 페이지, 백과사전 전체를 2차원에 놓고 "공룡 테마 롤러코스터 중 최고는?"이라는 질문과 가까운 것을 찾아요. 이것이 **similarity search**예요: 키워드 매칭이나 동의어 표 대신 다차원에서의 가까움을 써요.
- 실제 embedding 모델은 **약 천 개의 차원**을 써요. 4차원부터는 그림으로 그릴 수 없지만 계산은 그대로 작동해요. 좌표가 하나 늘고 거리 공식에 항이 하나 더 붙을 뿐이에요.
- 실제 모델의 축은 **누가 정한 게 아니에요.** 각 축의 의미는 학습에서 저절로 생기고(emergent), "847번 차원이 공룡 축"이라고 말할 수 없어요. 그래서 왜 두 텍스트가 가깝게 놓였는지 들여다보기 어려워요.
- embedding 모델은 어떤 문자열이든 받아 **고정 길이의 숫자 목록**을 내놓아요. 체험 페이지의 예시는 VoyageAI의 embedding 모델을 써서 1,024개 값이에요. 입력이 세 단어든 세 문단이든 길이는 같아요.
- 실무에서는 직선 거리 대신 **cosine similarity**를 써요. 벡터가 가리키는 방향이 얼마나 비슷한지를 재고, 값은 -1(반대) · 0(무관) · 1(동일) 사이예요.

## 5. 실습 — 아웃사이더 테스트

모델의 지식은 넓지만 고정돼 있고, 학습 데이터에 있던 것에 따라 모양이 정해져요. 내 분야에서 어디가 든든하고 어디가 얄팍한지 지도를 그려 봐요.

작업 목록에서 작업 하나를 고르고, 그 작업과 관련해 다음을 적어요.

1. **주류 · 문서화 · 안정적인 주제 2개** — 정보에 밝은 동료라면 누구나 알 만한 것
2. **niche · 지역 · 최근 · 급변하는 주제 2개** — 업계 전문 용어, 지역 규제, 지난 1년 사이 바뀐 것
3. **외부인이 자주 틀리는 "기본 가정" 1개** — 전형적인 고객이 누구인지, "표준" 사례가 어떤 모습인지, 언론에 나오는 도구 말고 사람들이 실제로 쓰는 도구가 무엇인지

| Probe | 방법 | 볼 것 |
|---|---|---|
| **1. Coverage** | 주류 주제 하나와 niche 주제 하나를 물어 깊이·정확도 비교 | 둘 사이에 불확실성을 다르게 표시하는가, 둘 다 같은 자신 있는 말투인가 |
| **2. Staleness** | 최근 바뀐 것을 질문: 규제 개정, 도구 출시, 리더십 교체, 개정된 표준 | 컷오프를 인정하는가, 진부한 정보를 최신처럼 제시하는가, 답변을 거절하는가 |
| **3. 기본 가정** | 내 가정을 직접 말하지 않고, AI가 외부인의 관점을 기본값으로 삼는지 드러나는 질문. 예: "전형적인 고객을 묘사해 줘" | AI가 무엇을 "정상"으로 취급하는가 |

실습 뒤에는 작업 목록에 **두 번째 주석**을 달아요: 각 작업에서 모델의 지식에 기댈 수 있는지, 아니면 맥락·문서·검색으로 **내가 지식을 가져와야 하는지** 표시해요.

**도전 과제**: staleness probe를 web search를 켠 채 다시 실행하고 무엇이 달라지는지 비교해요. 이것이 retrieval이 작동하는 모습이에요.

**돌아보기**

- 모델이 알고 있을 거라 가정하지 말고 맥락을 직접 줘야 한다는 걸 이제 깨달은 업무 영역 하나는?
- 기본 가정 probe에서 놀라운 것이 드러났나요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 모델의 지식을 가늠할 때 가장 실용적인 질문은?</summary>
  <p>① "AI가 이걸 아는가?" ② <b>"이것이 모델이 읽은 것 안에서 얼마나 잘 대표되어 있었나?"</b> ③ "이 모델이 최신 버전인가?" ④ "답이 길고 자신 있는가?"</p>
  <p><b>정답: ②</b> — 지식은 학습 데이터에 얼마나 자주·일관되게 나타났는지에 따라 두께가 달라요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 상황과 지식 실패</summary>
  <p>"작년에 개정된 규정을 예전 내용으로 설명한다" → <b>Staleness</b><br>"우리 지역 조례에 대해서는 답이 얕고 부정확하다" → <b>고르지 않은 coverage</b><br>"'전형적인 고객'을 물으면 업계 밖 사람이 떠올리는 모습으로 답한다" → <b>상속된 bias</b><br>"맞는 말 같은데 어디서 나온 정보인지 대지 못한다" → <b>Source amnesia</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 컷오프 이후에 일어난 일을 물어야 할 때 한계를 밀어내는 기능은?</summary>
  <p><b>정답: Web search.</b> 시간에 민감한 질문에서 컷오프를 우회해요. 학습된 적 없는 내부 자료라면 retrieval(RAG)/MCP가, 실제 계산·조회라면 tool use가 맞아요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Knowledge 속성은 4D 중 어느 역량과 가장 직접 연결되나요?</summary>
  <p><b>정답: Delegation.</b> 모델이 어디서 든든하고 어디서 얄팍한지 알아야 언제 맡기고, 언제 맥락을 직접 주고, 언제 다른 곳으로 갈지 정할 수 있어요.</p>
</details>

## 정리

- AI의 지식은 전적으로 학습 데이터에서 오고 **컷오프에서 고정**돼요. 도구 없이는 그 이후 정보에 접근할 수 없어요.
- 능력 구간은 자주·최근·일관되게 나타난 주제, 한계 구간은 드물거나 컷오프 이후·niche·지역·논쟁적 주제예요.
- 특징적 실패: staleness, 고르지 않은 coverage, 상속된 bias("기본값"·"정상"으로 취급하는 것), source amnesia(출처를 귀속하지 못함).
- web search·RAG/MCP·tool use는 학습되지 않은 정보에 접근시켜 이 간극을 메워요.
- embedding은 의미를 위치로 바꿔 관련 개념을 가까이 모으고, retrieval은 그 가까움으로 자료를 찾아요.
- **4D 연결**: 지식의 불균형은 **Delegation**의 핵심이에요 — 모델이 어디서 든든하고 어디서 얄팍한지 알면, 언제 맡기고 언제 맥락을 직접 주고 언제 다른 곳으로 갈지 판단할 수 있어요.
