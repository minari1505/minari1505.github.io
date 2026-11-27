---
title: "What is Claude Cowork"
title_ko: "Claude Cowork란"
course: introduction-to-claude-cowork
lesson: 1
---

## 학습 목표

- Claude Cowork를 정의하고, Claude와 일하는 방식으로서 무엇이 다른지 설명하기
- Cowork를 **"내 환경(내 파일, 내 앱, 내 도구) 안에서 일하는 Claude"**로 이해하기
- **Chat · Cowork · Code**를 구분하고 언제 무엇을 써야 할지 알기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>일할 곳을 가리키기</strong>폴더나 project, 그리고 연결한 앱</li>
  <li><strong>원하는 결과 설명</strong>질문이 아니라 outcome을 말해요</li>
  <li><strong>Claude가 계획 · 실행</strong>필요한 도구와 파일을 써서 단계를 밟아요</li>
  <li><strong>실제 결과물</strong>완성된 deliverable을 돌려받아요</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Cowork는 "채팅"이 아니라 "위임"이에요. 답이 아니라 일을 맡겨요.**

## 1. Cowork란 무엇인가요?

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍💼 쉽게 말하면</span>
  <p>옆자리 동료에게 "이거 어떻게 해?"라고 <b>물어보는 것</b>과 "이 건 맡아서 끝내 줘"라고 <b>맡기는 것</b>은 달라요. Chat이 앞쪽이라면, Cowork는 뒤쪽이에요. 자료를 찾고, 정리하고, 파일로 만들어 책상 위에 올려놓는 것까지 해 줘요.</p>
</div>

Cowork는 Claude가 나와 **나란히 작업을 수행**하는 방식이에요. 폴더나 project를 가리키고, 작업이 있는 앱(Gmail, Slack, Google Drive, 캘린더)을 연결한 뒤, 원하는 결과를 설명해요. 그러면 Claude가 계획을 세우고, 단계를 밟고, 필요한 도구와 파일을 쓰고, 실제 결과물을 돌려줘요.

### 어디서 쓰나요?

| 어디서 | 특징 |
|---|---|
| **Claude desktop app** | 작업에 **내 컴퓨터의 파일**이 필요할 때 쓰는 곳이에요. Claude가 파일을 직접 열고 저장할 수 있어요 |
| **웹** | 브라우저에서 Cowork를 쓸 수 있어요 |
| **휴대폰** | Claude 모바일 앱에서도 쓸 수 있어요 |

Cowork에 들어가는 방법은 Claude 앱 버전에 따라 조금 달라요. 어떤 버전에서는 **전용 탭**이 있고, 어떤 버전에서는 평소 대화에 작업을 설명하면 Claude가 그것을 **Cowork task로 바꿔요**. 이 강의의 내용은 어느 쪽이든 똑같이 적용돼요.

### 가장 중요한 멘탈 모델 — 채팅이 아니라 위임

Chat은 여전히 강력해요. 문제를 함께 생각하고, 초안을 다듬고, 브레인스토밍하고, 질문하고, Claude를 sounding board로 쓰는 곳이에요. Cowork는 그걸 대체하지 않아요. 새로 더하는 것은 **하나의 작업 전체**(맥락 수집, 분석, 파일 생성, 도구 사용)를 통째로 맡기고 완성된 결과를 돌려받는 능력이에요.

강의는 이것을 새 Cowork 사용자가 겪는 **가장 큰 전환**이라고 불러요. Chat에 익숙한 사람의 첫 본능은 질문을 치고, 답을 보고, 후속 질문을 치며 반복하는 거예요. Cowork에서도 그렇게 할 수 있지만, 가장 큰 효과는 **평소 남에게 물어보던 일이 아니라, 평소 내가 직접 하던 일**을 맡길 때 나와요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">💬 Chat — 턴 단위</span>
    <ul>
      <li>한 번 주고받는 대화 단위로 움직여요</li>
      <li>도구를 끌어올 수 있지만 <b>단계는 내가 직접 엮어요</b></li>
      <li>짧은 대화와 빠른 사고에 맞게 만들어졌어요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🤝 Cowork — 작업 전체를 지속</span>
    <ul>
      <li>여러 도구를 <b>동시에</b> 끌어와요</li>
      <li>다단계 계획을 끝까지 수행해요</li>
      <li>완성된 산출물을 돌려줘요</li>
    </ul>
  </div>
</div>

이것이 Chat에 파일을 업로드하는 것만으로는 완전히 메울 수 없는 간극이에요.

### 좋은 Cowork 작업의 형태

> 지난 분기 가격 정책에 대해 우리가 무엇을 결정했지? Q3 Sales 폴더의 회의록, 세일즈 팀 이메일, 리더십 Slack, 제안 덱을 모두 살펴보고, 임원진에 보낼 1페이지 요약으로 정리해줘.

이 프롬프트 하나에 Cowork다운 작업의 네 가지 특징이 다 들어 있어요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📁</span><span class="oa-card__title">로컬 파일</span>"Q3 Sales 폴더의 회의록"</div>
  <div class="oa-card"><span class="oa-card__icon">🧰</span><span class="oa-card__title">여러 도구</span>이메일, Slack, 제안 덱</div>
  <div class="oa-card"><span class="oa-card__icon">🪜</span><span class="oa-card__title">여러 단계</span>살펴보고 → 모으고 → 정리하고</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📄</span><span class="oa-card__title">실제 결과물</span>임원진에 보낼 1페이지 요약</div>
</div>

로컬 파일 + 여러 도구 + 여러 단계 + 실제 결과물. 이것이 Cowork가 대신 처리해 주는 작업의 모양이에요.

## 2. Cowork는 내 환경 "안에서" 일해요

<div class="oa-eli">
  <span class="oa-eli__head">🏢 쉽게 말하면</span>
  <p>전화로 조언만 해 주는 외부 컨설턴트가 아니라, <b>내 사무실에 출근해서 내 서류함과 내 프로그램으로 일하는 동료</b>예요. 옆에서 구경하는 게 아니라 안에 들어와서 일해요.</p>
</div>

강의의 표현으로는 "내 환경에 인접한 곳이 아니라, 내 환경 안에서"예요.

| 어디서 | Claude가 하는 일 |
|---|---|
| **내 파일에서** | 지정한 폴더의 내용을 읽고, 완성된 출력을 다시 써요 |
| **내 앱에서** | 연결한 이메일 · 캘린더 · 메신저 · 드라이브 · CRM에서 맥락을 끌어와요 |
| **내 브라우저에서** | 커넥터가 없는 웹 도구는 **Claude in Chrome**이 페이지를 직접 읽고 조작해요 (대시보드, 포털, 로그인 뒤의 무언가) |
| **내 도구로** | 무엇을 하라고 설명만 하는 게 아니라 직접 행동해요 |

## 3. Chat vs. Cowork vs. Code

<div class="oa-eli">
  <span class="oa-eli__head">🧭 쉽게 말하면</span>
  <p>같은 사람이라도 <b>회의실에서는 같이 고민하는 상대</b>, <b>사무실에서는 일을 맡아 주는 동료</b>, <b>개발실에서는 같이 코드를 짜는 엔지니어</b>가 될 수 있어요. 어느 방에 들어가느냐는 일의 모양이 정해요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">Chat — 함께 생각하기</span>대화예요. 질문하고, 초안을 쓰고, 브레인스토밍하고, 소리 내어 생각하면 Claude가 chat 창에서 답해요</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🤝</span><span class="oa-card__title">Cowork — 위임하기</span>working session이에요. 폴더나 project와 도구를 가리키고 결과를 설명하면 Claude가 계획하고, 실행하고, 전달해요</div>
  <div class="oa-card"><span class="oa-card__icon">⌨️</span><span class="oa-card__title">Code — 소프트웨어 만들기</span>agentic coding tool이에요. Claude가 코드베이스 안에서 파일을 편집하고, 테스트를 돌리고, commit해요</div>
</div>

| | 이런 일에 가장 좋아요 |
|---|---|
| **Chat** | 질문하고 아이디어 탐색하기 · 내가 직접 다듬을 초안 쓰기 · 한 번의 응답에 들어가는 일 · 한 번에 도구 하나 |
| **Cowork** | 파일과 도구에 걸친 다단계 작업 · 실제 결과물(문서, 덱, 시트) 만들기 · 시작해 두고 나중에 돌아와 확인하고 싶은 일 |
| **Code** | repo 안에서 일하기 · 여러 소스 파일에 걸친 빌드나 리팩터링 · 문서 작업이 아닌 개발 작업 (터미널 · git 접근) |

예를 들어 강의의 연습 문제 중 하나는 이래요: "Downloads 폴더의 벤더 PDF 다섯 개를 읽고, 가격과 SLA로 비교해서, 결과를 스프레드시트에 넣어줘." 여러 파일을 읽고, 비교하고, 실제 파일을 만들어 내는 일이니 **Cowork**의 모양이에요.

대부분의 지식 노동자는 Chat과 Cowork에서 시간을 보내고, 둘을 매우 다른 용도로 써요.

## 4. 통제권은 사용자에게

<div class="oa-eli">
  <span class="oa-eli__head">🚗 쉽게 말하면</span>
  <p>운전은 맡기지만 <b>목적지를 정하고 브레이크를 밟을 수 있는 건 나</b>예요. 출발 전에 경로를 보여 주고, 중요한 갈림길에서는 물어봐요.</p>
</div>

<ol class="oa-flow">
  <li><strong>계획을 먼저 보여 줘요</strong>시작하기 전에 무엇을 할지 보여 줘요</li>
  <li><strong>중요한 행동 전 확인</strong>기본적으로 전송 · 삭제 · 공유 전에 물어봐요</li>
  <li><strong>언제든 방향 조정</strong>어느 시점에든 끼어들어 steer할 수 있어요</li>
</ol>

### 생각해 보기

최근에 한 일 중 **가치가 "생각"이 아니라 "실행"에 있던 일**을 하나 떠올려 보세요. 여러 도구에 걸쳐 있었거나, 손으로 조립한 결과물로 끝났거나, 매주 반복하는 일일 수 있어요. 그 작업을 기억해 두면 Lesson 3에서 나만의 Cowork 사용 사례를 찾을 때, Lesson 4에서 첫 작업을 위임할 때 다시 쓸 수 있어요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Cowork를 처음 쓰는 사람이 해야 할 가장 큰 사고 전환은 무엇인가요?</summary>
  <p><b>정답: "채팅"에서 "위임"으로.</b> 평소 남에게 물어보던 일이 아니라, 평소 내가 직접 하던 일을 통째로 맡길 때 Cowork의 효과가 가장 커요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. "Downloads 폴더의 벤더 PDF 다섯 개를 읽고 가격과 SLA로 비교해 스프레드시트로 만들어줘." Chat, Cowork, Code 중 무엇이 맞나요?</summary>
  <p><b>정답: Cowork.</b> 로컬 파일 여러 개를 읽고, 여러 단계를 거쳐, 실제 파일(스프레드시트)로 끝나는 작업이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 좋은 Cowork 작업의 네 가지 특징은 무엇인가요?</summary>
  <p><b>정답: 로컬 파일 · 여러 도구 · 여러 단계 · 실제 결과물.</b> 이 모양이 보이면 Cowork에 맡길 만한 일이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 작업에 내 컴퓨터의 파일이 필요하다면 어디에서 Cowork를 써야 하나요?</summary>
  <p><b>정답: Claude desktop app.</b> Cowork는 웹과 휴대폰에서도 쓸 수 있지만, Claude가 내 컴퓨터의 파일을 직접 열고 저장할 수 있는 곳은 desktop app이에요.</p>
</details>

## 정리

- Cowork는 "답"이 아니라 **"작업"**을 위해 만들어진 Claude예요. 로컬 컴퓨터, 클라우드 앱, 브라우저 어디든 내가 일하는 곳에서 함께해요.
- 핵심 전환은 **"채팅"에서 "위임"으로**예요. 결과를 설명하면 Claude가 계획하고, 실행하고, 전체를 전달해요.
- Cowork는 **다단계 · 장시간 · 다중 도구** 작업을 처음부터 끝까지 처리하도록 설계됐고, 실제 결과물로 끝나요.
- **Chat은 생각, Cowork는 위임, Code는 소프트웨어 만들기**예요. 대부분은 Chat과 Cowork를 서로 다른 용도로 써요.
- 통제권은 항상 사용자에게 있어요. Claude는 계획을 먼저 보여 주고, 기본적으로 전송 · 삭제 · 공유 전에 확인하고, 언제든 방향을 바꿀 수 있게 해요.

다음 레슨에서는 Cowork를 설정해요: Claude가 일할 곳을 정하고, 첫 커넥터를 추가하고, 권한 모델을 배워요.
