---
title: "What are subagents?"
title_ko: "서브에이전트란?"
course: introduction-to-subagents
lesson: 1
---

## 학습 목표

- 서브에이전트가 무엇이고 왜 context 관리에 도움이 되는지 이해하기
- 내장 서브에이전트(General purpose, Explore, Plan)와 커스텀 서브에이전트 구분하기
- 서브에이전트가 주는 세 가지 이점 파악하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>메인이 위임</strong>작업 설명을 써서 넘김</li>
  <li><strong>별도 context에서 작업</strong>파일 읽기·검색·tool call</li>
  <li><strong>요약만 반환</strong>중간 과정은 돌아오지 않음</li>
  <li><strong>대화 폐기</strong>메인 context는 깨끗하게 유지</li>
</ol>

## 서브에이전트란

**서브에이전트(subagent)**는 Claude Code가 작업을 위임할 수 있는 특화된 도우미입니다. 각 서브에이전트는 **자체 대화 context window**에서 실행되어 일을 하고, **요약만** 메인 스레드로 돌려줍니다. 파일 읽기·검색·tool call 같은 중간 단계는 격리된 채 메인 대화를 어지럽히지 않습니다.

<div class="oa-eli">
  <span class="oa-eli__head">📚 쉽게 말하면</span>
  <p>도서관 사서에게 "환불 규정이 어느 책에 있는지 찾아 주세요"라고 부탁하는 것과 같습니다. 사서는 서가를 돌며 책 수십 권을 펼쳐 보지만, 내 책상에는 <b>"3층 B열, 142쪽"이라는 쪽지 한 장</b>만 올라옵니다. 내 책상(메인 context)은 어질러지지 않습니다.</p>
</div>

## 왜 중요한가

Claude Code와 대화할 때마다 모든 tool call·파일 읽기·검색 결과가 메인 context window에 쌓입니다. 그 공간은 유한하고, 가득 차면 Claude가 대화 앞부분을 놓치기 시작합니다.

서브에이전트는 **별도의 context window**를 띄워 이를 해결합니다. 서브에이전트는 두 가지를 받습니다.

- 설정 파일의 **커스텀 system prompt** — 역할과 행동을 정의
- 부모 에이전트가 요청에 맞춰 쓴 **작업 설명(task description)**

서브에이전트는 스스로 파일을 읽고 검색·편집한 뒤, 끝나면 **요약만** 돌려주고 그 대화 전체는 폐기됩니다. 덕분에 메인 context는 깨끗하게 유지됩니다. 트레이드오프는 서브에이전트가 어떻게 결론에 도달했는지에 대한 **가시성을 잃는다**는 점입니다.

### 실용 예시

낯선 코드베이스에서 "환불을 어느 서비스가 처리하지?"를 알고 싶다고 합시다.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">서브에이전트 없이</span>
    <ul>
      <li>Claude가 15개 파일을 읽고 여러 번 검색하며 함수 호출을 추적합니다.</li>
      <li>그 모든 내용이 메인 context에 쌓입니다.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">Explore 서브에이전트 사용</span>
    <ul>
      <li>파고드는 작업이 서브에이전트의 context에서 이뤄집니다.</li>
      <li>메인에는 질문과 요약만 기록됩니다.</li>
    </ul>
  </div>
</div>

## 내장 서브에이전트

Claude Code는 바로 쓸 수 있는 내장 서브에이전트를 제공합니다.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧰</span><span class="oa-card__title">General purpose</span>탐색과 실행이 모두 필요한 다단계 작업용</div>
  <div class="oa-card"><span class="oa-card__icon">🔎</span><span class="oa-card__title">Explore</span>코드베이스의 빠른 검색·탐색용</div>
  <div class="oa-card"><span class="oa-card__icon">🗺️</span><span class="oa-card__title">Plan</span>plan 모드에서 계획 제시 전 코드베이스 조사·분석용</div>
</div>

## 커스텀 서브에이전트

내장 옵션을 넘어, 커스텀 system prompt와 도구 접근을 가진 나만의 서브에이전트를 만들 수 있습니다 — code reviewer, test writer, documentation generator 등 워크플로에 맞춘 특화 에이전트.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 서브에이전트가 작업을 마치면 메인 스레드로 돌아오는 것은 무엇입니까?</summary>
  <p><b>정답: 요약만.</b> 파일 읽기, 검색, tool call 같은 중간 단계는 서브에이전트의 context에 격리되고 대화 전체는 폐기됩니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 서브에이전트를 쓸 때 감수해야 하는 트레이드오프는 무엇입니까?</summary>
  <p><b>정답: 가시성을 잃습니다.</b> 서브에이전트가 어떻게 결론에 도달했는지 메인에서는 볼 수 없습니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 낯선 코드베이스에서 특정 기능이 어디에 구현됐는지 빠르게 찾으려 합니다. 알맞은 내장 서브에이전트는 무엇입니까?</summary>
  <p><b>정답: Explore.</b> 빠른 검색과 탐색용입니다. General purpose는 탐색과 실행이 모두 필요한 다단계 작업, Plan은 plan 모드의 사전 조사에 씁니다.</p>
</details>

## 핵심 정리

서브에이전트의 세 가지 이점:

- **작업을 집중된 조각으로 나눔** — 각 서브에이전트가 특정 작업에 집중
- **메인 context window를 깨끗하게 유지** — 모든 중간 작업을 격리
- **필요한 정보만 간결한 요약으로 반환**

메인 context에 소음이 적을수록 더 길고 효과적으로 작업할 수 있습니다.
