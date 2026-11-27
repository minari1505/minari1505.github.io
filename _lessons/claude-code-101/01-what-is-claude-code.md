---
title: "What is Claude Code?"
title_ko: "Claude Code란?"
course: claude-code-101
lesson: 1
---

## 학습 목표

- Claude Code가 **Claude.ai와 어떻게 다른지** 설명하기
- **AI Agent**의 개념과 agentic loop의 기본 아이디어 파악하기
- Claude Code가 실제로 **할 수 있는 일 4가지** 알기
- 효과적으로 쓰기 위한 **세 가지 전제**(context window · permissions · 실수 가능성) 이해하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>Claude Code란</strong>코드베이스를 이해하고 직접 일하는 agentic coding tool</li>
  <li><strong>Claude.ai와 차이</strong>파일 · 터미널 · 코드베이스에 직접 접근</li>
  <li><strong>Agent라는 정체</strong>LLM이 loop 안에서 도구를 써 가며 목표 달성</li>
  <li><strong>할 수 있는 일</strong>읽기 · 수정 · 명령 실행 · 웹 검색</li>
  <li><strong>잘 쓰는 전제</strong>context · 권한 · 실수 가능성</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Claude Code는 "답을 알려 주는 채팅"이 아니라 "내 코드베이스에 들어와 직접 일하는 agent"예요.**

## 1. Claude Code란?

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍🔧 쉽게 말하면</span>
  <p>전화로 수리 방법을 알려 주는 상담원과, 집에 직접 와서 고쳐 주는 기사님의 차이예요. Claude Code는 <b>내 프로젝트에 직접 들어와서 손을 대는 쪽</b>이에요.</p>
</div>

Claude Code는 코드베이스를 이해하고, 파일을 수정하고, 명령을 실행하고, 기존 개발 도구와 연동해 작업을 더 빠르게 끝내 주는 **agentic coding tool**이에요.

쓸 수 있는 곳은 다섯 군데예요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">⌨️</span><span class="oa-card__title">터미널</span><code>claude</code> 명령으로 실행</div>
  <div class="oa-card"><span class="oa-card__icon">🧩</span><span class="oa-card__title">Visual Studio Code</span>확장으로 설치</div>
  <div class="oa-card"><span class="oa-card__icon">🛠️</span><span class="oa-card__title">JetBrains IDE</span>플러그인으로 설치</div>
  <div class="oa-card"><span class="oa-card__icon">🖥️</span><span class="oa-card__title">Claude Desktop app</span>데스크톱 앱 안에서</div>
  <div class="oa-card"><span class="oa-card__icon">🌐</span><span class="oa-card__title">웹</span>브라우저에서</div>
</div>

## 2. Claude.ai와 무엇이 다른가요?

<div class="oa-eli">
  <span class="oa-eli__head">📋 쉽게 말하면</span>
  <p>Claude.ai에서는 내가 <b>심부름꾼</b>이에요. 코드를 복사해 붙여 넣고, 답을 다시 복사해 에디터로 가져가죠. Claude Code에서는 Claude가 직접 다녀와요.</p>
</div>

Claude.ai를 써 봤다면 차이가 궁금할 수 있어요. 가장 큰 차이는 Claude Code가 **파일, 터미널, 코드베이스 전체에 직접 접근**한다는 점이에요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">💬 Claude.ai</span>
    <ul>
      <li>코드를 복사·붙여넣기 하며 오가야 해요</li>
      <li>내 파일과 터미널을 직접 만지지 못해요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🤖 Claude Code</span>
    <ul>
      <li>파일 · 터미널 · 코드베이스 전체에 <b>직접 접근</b></li>
      <li>직접 들어가서 작업을 수행해요</li>
      <li><b>AI Agent</b>로 동작해요 (핵심 차별점)</li>
    </ul>
  </div>
</div>

### Agent란?

AI Agent는 **자신의 환경과 상호작용하며, 정해진 목표를 달성하기 위해 행동을 수행하는 소프트웨어**예요. 근본적으로는 **large language model이 실시간 loop 안에서 동작**하는 방식이에요.

Agent는 목표를 이루기 위해 세 가지에 접근할 수 있어요.

| 접근 대상 | 뜻 |
|---|---|
| **tools** | 파일 읽기, 명령 실행 같은 행동 수단 |
| **외부 서비스** | 코드베이스 밖의 서비스 |
| **다른 AI Agent** | 일을 나눠 맡길 또 다른 agent |

## 3. Claude Code가 할 수 있는 일

<div class="oa-eli">
  <span class="oa-eli__head">👩‍💻 쉽게 말하면</span>
  <p>옆자리에 앉은 동료 개발자를 떠올려 보세요. 코드를 읽고, 고치고, 테스트를 돌려 보고, 모르면 문서를 찾아봐요. Claude Code도 <b>이 네 가지를 스스로</b> 해요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📖</span><span class="oa-card__title">코드베이스 읽고 이해하기</span>특정 기능을 설명해 달라거나, 버그를 코드 전반에서 추적해 달라고 요청할 수 있어요</div>
  <div class="oa-card"><span class="oa-card__icon">✏️</span><span class="oa-card__title">프로젝트 전반의 파일 수정</span>함수를 리팩터링하고, 그 함수를 참조하는 모든 파일을 함께 업데이트해요</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">⚙️</span><span class="oa-card__title">터미널 명령 실행</span>빌드 스크립트 실행, 테스트 수행, 패키지 설치를 하고 <b>그 출력을 보고 다음 행동을 결정</b>해요</div>
  <div class="oa-card"><span class="oa-card__icon">🔎</span><span class="oa-card__title">웹 검색</span>문서나 최신 API 레퍼런스가 필요하면 직접 찾아봐요</div>
</div>

## 4. 효과적으로 쓰기 위한 세 가지

<div class="oa-eli">
  <span class="oa-eli__head">🚗 쉽게 말하면</span>
  <p>운전 보조 기능이 있는 차와 비슷해요. 한 번에 볼 수 있는 범위에 한계가 있고(context), 얼마나 맡길지는 내가 정하고(permissions), 그래도 <b>핸들에서 손을 완전히 떼면 안 돼요</b>(실수 가능성).</p>
</div>

| 개념 | 내용 | 기억할 점 |
|---|---|---|
| **context window** | Claude의 **작업 기억**이에요. 많은 것을 담을 수 있지만 한 번에 전부는 아니에요 | 여기서 "agentic"의 의미가 드러나요. Claude는 코드베이스 전체를 context에 올리지 않고도 답을 찾는 **전략적인 방법**을 스스로 찾아요 |
| **permissions는 내가 정해요** | permission mode로 얼마나 hands-on 할지 골라요. 명령 실행·변경 전에 **매번 물어보게** 할 수도 있고, 백그라운드 safety check가 대부분의 행동을 걸러 주는 **auto mode**로 돌릴 수도 있어요 | hands-on이든 hands-off든 **통제권은 항상 사용자**에게 있어요 |
| **실수할 수 있어요** | 다른 도구처럼 완벽하지 않아요. 의도를 오해하거나, 버그를 넣거나, 과하게 설계(over-engineer)할 수 있어요 | **과정에 계속 참여(stay in the loop)**하면 일찍 잡아낼 수 있어요 |

> 권한 설명은 현재 강의 본문 기준이에요. 예전 노트의 "기본적으로 실행 전에 확인한다"는 설명에 더해, 이제는 **auto mode**라는 선택지가 함께 소개돼요. 자세한 모드는 다음 레슨에서 다뤄요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Claude Code와 Claude.ai의 가장 큰 차이는 무엇인가요?</summary>
  <p><b>정답: Claude Code는 파일 · 터미널 · 코드베이스 전체에 직접 접근해요.</b> 코드를 복사·붙여넣기 하며 오가는 대신 직접 들어가 작업하고, AI Agent로 동작해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 강의가 설명하는 AI Agent의 핵심 동작 방식은?</summary>
  <p><b>정답: large language model이 실시간 loop 안에서 동작하는 것.</b> 목표 달성을 위해 tools, 외부 서비스, 다른 AI Agent에 접근할 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. context window에서 "agentic"의 의미가 드러난다는 건 무슨 뜻인가요?</summary>
  <p><b>정답: 코드베이스 전체를 context에 올리지 않아도, Claude가 답을 찾는 전략적인 방법을 스스로 찾는다는 뜻이에요.</b> 작업 기억에는 한계가 있기 때문이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Claude Code가 저지를 수 있는 실수 세 가지와 대처법은?</summary>
  <p><b>정답: 의도 오해, 버그 유입, 과한 설계.</b> 과정에 계속 참여하면 일찍 잡아낼 수 있어요.</p>
</details>

## 정리

- Claude Code는 코드베이스를 읽고, 파일을 수정하고, 명령을 실행하고, 외부 도구와 연결해 더 빠르게 출시하도록 돕는 **agentic coding tool**이에요.
- Claude.ai와 달리 파일·터미널·코드베이스에 **직접 접근**하고, AI Agent로서 loop 안에서 도구를 활용해요.
- 할 수 있는 일은 **읽기 · 수정 · 명령 실행 · 웹 검색** 네 가지로 요약돼요.
- **context window**는 유한한 작업 기억이고, **permissions**는 내가 정하며, Claude도 **실수할 수 있어요.**
- 터미널, VS Code, JetBrains, Claude Desktop app에서 바로 쓸 수 있어요 (웹에서도 가능해요).
