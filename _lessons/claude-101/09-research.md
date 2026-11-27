---
title: "Research"
title_ko: "Research로 깊이 조사하기"
course: claude-101
lesson: 9
---

## 학습 목표

- **Research**가 단순 검색이 아니라 체계적인 multi-source investigation이라는 점 설명하기
- 언제 Research를 쓰고, 언제 web search · Thinking · Enterprise Search를 쓸지 구분하기
- Research가 **Thinking**과 함께 조사 계획을 세우고 깊이 있는 report를 만드는 과정 파악하기
- 복잡한 조사에 맞는 **Research prompt** 작성법 익히기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>무엇인가</strong>여러 검색을 이어 가는 agentic 조사</li>
  <li><strong>언제 쓰나</strong>빠른 답보다 깊은 이해가 필요할 때</li>
  <li><strong>어떻게 작동하나</strong>계획 → 여러 검색 → 종합 → citation</li>
  <li><strong>어떻게 켜나</strong><code>+</code> → <code>Research</code> (web search 필요)</li>
  <li><strong>잘 쓰는 법</strong>목표 · 구조 · 제약을 담은 prompt</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Research는 Claude가 스스로 계획을 세워 여러 source를 조사하고, 출처가 달린 report로 정리해 주는 기능이에요.**

## 1. Research란?

<div class="oa-eli">
  <span class="oa-eli__head">🕵️ 쉽게 말하면</span>
  <p>일반 검색은 <b>도서관 안내 데스크에 질문 하나</b> 던지는 거예요. Research는 <b>조사원에게 과제를 맡기는 것</b>이에요. 조사원은 책을 한 권 찾고 끝내지 않고, 읽다가 생긴 궁금증을 따라 다른 서가도 뒤진 뒤, 출처를 단 보고서를 들고 와요. 그동안 나는 내 일을 하면 돼요.</p>
</div>

**Research**는 Claude를 대화형 assistant에서 **체계적인 조사자**로 확장하는 기능이에요. Research를 켜면 Claude는 질문에 바로 답하는 대신, 여러 각도에서 정보를 찾고, web과 connected integrations를 탐색하고, 결과를 종합해 report를 만들어요.

| | 단일 web search | Research |
|---|---|---|
| **검색 방식** | 한 번 검색하고 답해요 | 여러 검색이 **서로 이어져요**. 찾은 내용을 보고 다음에 무엇을 찾을지 정해요 |
| **범위** | 한두 source면 충분한 질문 | 다른 각도를 자동으로 탐색하고 빈틈을 채워요 |
| **결과물** | 빠른 답 | citation이 달린 report |

### Research의 핵심 특징

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🔗</span><span class="oa-card__title">여러 검색이 서로 이어져요</span>한 번 검색하고 끝내지 않아요. 찾은 정보를 바탕으로 다음 조사 대상을 정하고, open question을 체계적으로 좁혀 가요.</div>
  <div class="oa-card"><span class="oa-card__icon">⏱️</span><span class="oa-card__title">시간이 더 걸려요</span>평소 검색보다 오래 걸려요. 질문에 따라 몇 분 또는 그 이상이에요. 한 번의 조회가 아니라 많은 검색을 동시에 보내고, 때로는 수백 개 source에서 찾은 내용을 하나의 답으로 모으기 때문이에요.</div>
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">Thinking과 함께 작동해요</span>검색 전에 접근 방식을 먼저 계획해요. 복잡한 요청을 다룰 수 있는 조각으로 나누고, 조각마다 필요한 정보를 모아요.</div>
  <div class="oa-card"><span class="oa-card__icon">📎</span><span class="oa-card__title">Citation으로 검증해요</span>report에 확인하기 쉬운 source citation이 붙어요. findings를 신뢰하되, 필요하면 원문으로 돌아가 직접 검증할 수 있어요.</div>
</div>

숙련된 research assistant가 정보를 모으고, source를 교차 확인하고, 포괄적인 report를 엮는 동안 나는 내 일을 계속할 수 있어요. 사람이 수동으로 하면 몇 시간이 걸릴 조사예요.

## 2. 언제 Research를 쓸까요?

<div class="oa-eli">
  <span class="oa-eli__head">🧰 쉽게 말하면</span>
  <p>못 하나 박는 데 공사팀을 부르진 않죠. <b>주소 하나 확인</b>은 web search, <b>머리로 풀 문제</b>는 Thinking, <b>회사 안 이야기</b>는 Enterprise Search, <b>여러 곳을 뒤져 보고서를 써야 할 때</b>가 Research예요.</p>
</div>

Research는 빠른 답보다 **깊이 있는 이해**가 필요할 때 맞아요.

| Research가 필요한 상황 | Research가 잘하는 작업 |
|---|---|
| 여러 source를 종합한 comprehensive report가 필요할 때 | Market analysis와 competitive research |
| web과 Google Workspace 같은 connected integrations를 함께 분석해야 할 때 | team offsite나 product launch 같은 복잡한 project planning |
| 수동으로 몇 시간 걸릴 thorough investigation이 필요할 때 | email, calendar, documents에 흩어진 정보 종합 |
| competitor나 vendor option을 비교 분석해야 할 때 | 여러 source를 참고하는 technical documentation 작성 |
| citation이 포함된 검증 가능한 report가 필요할 때 | 최신 정보와 검증된 source가 필요한 briefing 준비 |

### 다른 기능이 더 맞는 경우

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">⚡</span><span class="oa-card__title">Web search</span>빠르고 구체적인 사실 하나가 필요할 때 — 오늘의 주가, 회사 주소, 특정 사건의 날짜. 한두 source면 충분하고, <b>속도가 포괄성보다 중요</b>할 때.</div>
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">Thinking</span>외부 정보 수집보다 <b>깊은 reasoning</b>이 필요할 때 — 수학 문제, code debugging, 논리 분석, 외부 자료가 필요 없는 복잡한 전략 문제.</div>
  <div class="oa-card"><span class="oa-card__icon">🏢</span><span class="oa-card__title">Enterprise Search</span><b>조직 내부 지식</b>에서 답을 찾을 때 — 회사 정책과 프로세스, Slack thread·email·회의록·내부 문서, 과거 의사결정, onboarding 질문.</div>
</div>

> Research는 **public web과 connected integrations를 활용한 깊은 조사**에 강하고, Enterprise Search는 **조직 내부 지식 검색**에 특화되어 있어요. 질문이 public web이 아니라 우리 회사에만 해당한다면 Enterprise Search 쪽이에요.

## 3. Research가 작동하는 방식

<div class="oa-eli">
  <span class="oa-eli__head">🗺️ 쉽게 말하면</span>
  <p>여행 계획을 잘 짜는 친구를 떠올려 보세요. 먼저 <b>뭘 알아봐야 할지 목록</b>을 만들고, 숙소를 찾다가 교통편이 걸리면 그것도 알아보고, 마지막에 <b>링크가 달린 일정표</b>로 정리해 주죠. 내가 단계마다 시키지 않아도요.</p>
</div>

Research를 켜면 단순한 web search를 훨씬 넘어서는 **agentic, multi-step process**가 시작돼요. Claude는 이미 찾은 내용을 바탕으로 다음에 무엇을 검색할지 **스스로 결정**해요. 단계마다 내가 지시할 필요가 없어요.

<ol class="oa-flow">
  <li><strong>1. 접근 방식 계획</strong>검색 전에 먼저 생각해요. 요청을 쪼개고, 어떤 정보가 필요한지, 어떤 각도를 조사할지 계획</li>
  <li><strong>2. 여러 검색 수행</strong>단일 query가 아니에요. 첫 결과를 보고 다음 조사 내용을 정하고, 유망한 단서를 따라가며 빈틈을 채워요</li>
  <li><strong>3. Findings 종합</strong>web과 connected integrations(Gmail, Google Calendar, Google Drive 등)의 정보를 잘 정리된 하나의 report로</li>
  <li><strong>4. Citation 제공</strong>report의 claim마다 source가 연결돼서, 출처를 확인하고 더 깊이 들어갈 수 있어요</li>
</ol>

## 4. Research 사용 방법

<div class="oa-eli">
  <span class="oa-eli__head">🎛️ 쉽게 말하면</span>
  <p>세탁기의 <b>"표준" 대신 "불림 + 강력" 코스</b>를 고르는 것과 같아요. 버튼 하나로 모드를 바꾸면, 시간은 더 걸리지만 훨씬 꼼꼼하게 돌아가요.</p>
</div>

<ol class="oa-flow">
  <li><strong>+ 버튼</strong>chat interface 왼쪽 아래의 <code>+</code> 클릭</li>
  <li><strong>Research 선택</strong>메뉴에서 <code>Research</code> 선택 — 활성화되면 강조 표시</li>
  <li><strong>Prompt 제출</strong>prompt를 입력하고 제출</li>
  <li><strong>기다리기</strong>Claude가 background에서 검색·분석하고 progress indicator를 보여 줘요</li>
</ol>

> **주의** — Research가 작동하려면 **web search가 켜져 있어야** 해요. 꺼져 있다면 같은 `+` 메뉴에서 켤 수 있어요.

## 5. 효과적인 Research prompt 작성법

<div class="oa-eli">
  <span class="oa-eli__head">📝 쉽게 말하면</span>
  <p>조사원에게 "전기차 좀 알아봐"라고만 하면 뭘 들고 올지 몰라요. <b>"투자 판단용으로, 이 세 가지를, 이 목차로"</b>라고 의뢰서를 써 주면 원하는 보고서가 와요. 한 번 돌리는 데 몇 분씩 걸리니까 의뢰서에 공을 들일 가치가 있어요.</p>
</div>

Research 한 번은 초 단위가 아니라 분 단위로 걸려요. 그래서 prompt를 다듬는 데 쓴 시간이 그만큼 돌아와요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🎯</span><span class="oa-card__title">① 목표를 구체적으로</span>무엇을, 왜 알고 싶은지 써요</div>
  <div class="oa-card"><span class="oa-card__icon">🗂️</span><span class="oa-card__title">② 원하는 구조 지정</span>Claude는 내가 준 구조에 맞춰 findings를 정리해요</div>
  <div class="oa-card"><span class="oa-card__icon">📏</span><span class="oa-card__title">③ 제약 조건 포함</span>budget, timeline, geography 등</div>
  <div class="oa-card"><span class="oa-card__icon">🤝</span><span class="oa-card__title">④ Claude와 prompt 다듬기</span>Research를 켜기 전에 먼저 물어봐요</div>
</div>

### ① 목표를 구체적으로 쓰기

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 너무 넓은 prompt</span>
    <ul>
      <li>"EV market에 대해 알려줘."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 목표가 보이는 prompt</span>
    <ul>
      <li>"전기차 배터리 시장을 분석해줘. 주요 player, 기술 trend, 투자 의사결정에 영향을 줄 수 있는 supply chain challenge를 식별해줘."</li>
    </ul>
  </div>
</div>

```text
전기차 배터리 시장을 분석해줘.
주요 player, 기술 trend, 투자 의사결정에 영향을 줄 수 있는 supply chain challenge를 식별해줘.
```

### ② 원하는 구조 지정하기

```text
Team offsite 장소 후보를 비교해줘.
다음 섹션으로 정리해줘:
1. 위치와 접근성
2. meeting space와 amenities
3. catering options
4. 가격 고려사항
```

### ③ 제약 조건 포함하기

budget, timeline, geography, audience, required format 같은 제약을 넣으면 Research가 더 관련성 높은 방향으로 좁혀져요.

### ④ Research prompt 자체를 Claude와 다듬기

조사 질문을 어떻게 잡아야 할지 모르겠다면, Research를 켜기 **전에** Claude에게 prompt를 같이 다듬어 달라고 해요.

```text
이 주제에 대해 Research mode로 조사하려고 해.
좋은 Research prompt로 바꿔줘.
```

## 6. Connected integrations와 함께 쓰기

<div class="oa-eli">
  <span class="oa-eli__head">🧳 쉽게 말하면</span>
  <p>조사원이 <b>바깥 도서관만</b> 다니는 게 아니라 <b>내 서랍과 수첩도</b> 같이 볼 수 있게 되는 거예요. "우리 쪽 자료는 이렇고, 바깥 사례는 이래요"를 한 보고서에서 볼 수 있어요.</p>
</div>

Google Workspace나 다른 integrations가 연결되어 있으면 Research가 더 강력해져요. Claude는 web research와 함께 email, calendar, documents의 context를 가져올 수 있어요.

```text
Project X에 대해 email과 Slack에서 논의된 내용을 요약하고,
비슷한 initiative에 대한 industry best practices를 조사해줘.
```

```text
다음 주 내 calendar commitments를 검토하고,
내가 만날 각 회사에 대해 조사해줘.
```

```text
우리 pricing strategy 관련 내부 문서를 모두 찾고,
경쟁사 positioning과 비교해줘.
```

원하는 source가 있다면 prompt에 직접 말해서 방향을 잡아 줄 수 있어요.

```text
내 Google Drive에서 관련 context를 가져와줘.
```

```text
이 주제에 대한 최근 email의 insight도 포함해줘.
```

### 스스로 물어볼 질문

- 내 업무에서 여러 source를 모아야 하는 research task는 무엇인가요?
- Google Workspace 같은 connected integrations와 Research를 결합하면 workflow가 어떻게 달라질까요?
- 조사 시간이 너무 오래 걸려 미뤄 둔 복잡한 질문은 무엇인가요?
- 빠른 web search로 충분한 질문과 Research가 필요한 질문을 어떻게 구분할 수 있을까요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Research가 단일 web search와 가장 다른 점은?</summary>
  <p>① 검색 결과를 더 빨리 보여 줘요 ② 찾은 내용을 바탕으로 다음 검색을 스스로 정하며 여러 검색을 이어 가요 ③ citation 없이 요약만 줘요 ④ 내부 문서만 검색해요</p>
  <p><b>정답: ②</b> — Research는 agentic하게 작동해요. 단서를 따라가고 빈틈을 채운 뒤 citation이 달린 report로 종합해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 상황과 알맞은 기능</summary>
  <p>"오늘 A사 주가가 얼마야?" → <b>Web search</b> (빠른 사실 하나)<br>"이 코드가 왜 무한 루프에 빠지는지 분석해줘" → <b>Thinking</b> (외부 정보 없이 깊은 추론)<br>"우리 회사 expense report 제출 절차가 뭐야?" → <b>Enterprise Search</b> (조직 내부 지식)<br>"vendor 세 곳을 비교한 출처 있는 report가 필요해" → <b>Research</b> (여러 source 종합)</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Research를 켰는데 작동하지 않아요. 가장 먼저 확인할 것은?</summary>
  <p><b>정답: web search가 켜져 있는지</b>예요. Research는 web search가 켜져 있어야 작동하고, 같은 <code>+</code> 메뉴에서 켤 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 다음 중 가장 좋은 Research prompt는?</summary>
  <p>① "AI 시장 알려줘." ② "offsite 장소 추천해줘." ③ "team offsite 장소 후보를 위치와 접근성, meeting space와 amenities, catering options, 가격 고려사항 섹션으로 비교해줘." ④ "최대한 길게 조사해줘."</p>
  <p><b>정답: ③</b> — 목표가 구체적이고 원하는 구조가 지정되어 있어요. 여기에 budget이나 지역 같은 제약까지 넣으면 더 좋아요.</p>
</details>

## 정리

- **Research**는 단일 검색이 아니라 여러 검색을 이어 가는 **agentic investigation**이에요. Claude가 다음에 무엇을 조사할지 스스로 정해요.
- **Thinking**과 함께 작동해서, 검색 전에 요청을 쪼개고 조사 계획을 세워요.
- 평소 검색보다 오래 걸려요(몇 분 또는 그 이상). 많은 검색을 동시에 보내고 때로는 수백 개 source를 종합하기 때문이에요.
- report에는 **citation**이 붙어서 findings를 검증하기 쉬워요.
- web과 connected integrations를 함께 활용할 수 있어요. `+` 메뉴에서 켜고, web search가 켜져 있어야 해요.
- 빠른 사실 확인은 web search, 외부 정보 없는 깊은 추론은 Thinking, 조직 내부 지식 검색은 Enterprise Search가 더 맞아요.

다음 섹션에서는 지금까지 배운 기능이 실제 역할별 use case에서 어떻게 조합되는지 살펴봐요.
