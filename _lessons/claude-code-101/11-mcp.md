---
title: "MCP"
title_ko: "MCP (Model Context Protocol)"
course: claude-code-101
lesson: 11
---

## 학습 목표

- **MCP**가 무엇이고 Claude Code를 외부 도구·데이터와 어떻게 연결하는지 이해하기
- MCP 서버 **추가 방법**과 **HTTP / Stdio** 유형 구분하기
- **Local / User / Project** scope 구분하기
- MCP의 **context 비용**과 이를 관리하는 방법 파악하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>MCP란</strong>외부 도구·데이터에 연결하는 개방형 표준</li>
  <li><strong>서버 추가</strong><code>claude mcp add</code> — HTTP 또는 Stdio</li>
  <li><strong>scope 정하기</strong>Local · User · Project</li>
  <li><strong>context 관리</strong><code>/mcp</code>로 확인하고 안 쓰는 건 끄기</li>
</ol>

이 레슨을 한 문장으로 줄이면: **MCP로 코드베이스 밖의 맥락을 연결하되, 안 쓰는 서버는 꺼서 context를 지켜요.**

## 1. MCP란

<div class="oa-eli">
  <span class="oa-eli__head">🔌 쉽게 말하면</span>
  <p><b>USB 포트</b>와 같아요. 규격이 하나로 정해져 있어서 키보드든 프린터든 꽂기만 하면 돼요. MCP도 표준이 하나라서 어떤 도구든 같은 방식으로 Claude Code에 꽂을 수 있어요.</p>
</div>

**Model Context Protocol(MCP)**은 Claude Code가 외부 도구와 데이터 소스에 연결되게 하는 **개방형 표준(open standard)**이에요. 질문을 하면, 그 질문을 더 잘 처리하기 위해 **언제 그 도구를 써야 할지** Claude가 자동으로 이해해요.

많은 맥락이 코드베이스 **밖**에 있어요. MCP가 그 간극을 이어 줘요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🗄️</span><span class="oa-card__title">데이터베이스</span>코드에는 없는 실제 데이터</div>
  <div class="oa-card"><span class="oa-card__icon">📋</span><span class="oa-card__title">생산성 앱</span>이슈, 문서, 일정</div>
  <div class="oa-card"><span class="oa-card__icon">🌍</span><span class="oa-card__title">공개 저장소</span>내 프로젝트 밖의 코드</div>
</div>

## 2. 무엇을 할 수 있나요

<div class="oa-eli">
  <span class="oa-eli__head">📞 쉽게 말하면</span>
  <p>비서에게 <b>회사 전화번호부와 출입증</b>을 주는 거예요. 이제 "그 건 어떻게 됐어?"라고 물으면 비서가 담당 부서에 직접 확인하고 답해 줘요.</p>
</div>

먼저 agentic AI에서 **"tools"** 개념을 이해해야 해요. tool은 Claude Code 같은 agent가 작업을 더 효과적으로 완수하도록 **행동을 수행하는 능력**을 줘요. 텍스트 응답만 돌려받는 일반 AI와 다른 점이에요.

| 상황 | 추가할 MCP 서버 | 얻는 것 |
|---|---|---|
| 팀이 Linear로 프로젝트를 관리해요 | Linear MCP 서버 | 특정 이슈의 세부 내용을 가져와요 |
| 의존성의 최신 문서가 필요해요 | Context7 같은 docs MCP 서버 | 최신 문서를 Claude Code에 제공해요 |

## 3. MCP 서버 추가하기

<div class="oa-eli">
  <span class="oa-eli__head">🍱 쉽게 말하면</span>
  <p>HTTP 서버는 <b>배달 음식</b>(가게가 만들어서 네트워크로 보내 줌), Stdio 서버는 <b>집밥</b>(내 부엌에서 직접 돌아감)이에요.</p>
</div>

`claude mcp add` 명령으로 서버를 추가해요. 주요 유형은 둘이에요.

| 유형 | 용도 | 어디서 실행되나 |
|---|---|---|
| **HTTP 서버** | 원격 서비스 | 서비스 제공자가 호스팅하고 **네트워크로 연결**해요 |
| **Stdio 서버** | 로컬 프로세스 | **내 컴퓨터**에서 돌아요 |

세션 안에서는 `/mcp`로 서버를 관리해요. 무엇이 연결돼 있는지 보고, 상태를 확인하고, 필요 없는 서버를 비활성화할 수 있어요.

## 4. 서버 scope

<div class="oa-eli">
  <span class="oa-eli__head">🗝️ 쉽게 말하면</span>
  <p>열쇠를 <b>이 방 서랍</b>에 둘지(Local), <b>내 열쇠고리</b>에 달지(User), <b>사무실 공용 열쇠함</b>에 걸지(Project)의 차이예요.</p>
</div>

| scope | 어디서 쓸 수 있나 | 누가 쓰나 |
|---|---|---|
| **Local** | 현재 프로젝트에서만 | 나만 |
| **User** | 내 모든 프로젝트에서 | 나만 |
| **Project** | `.mcp.json` 파일을 버전 관리에 체크인 | 코드베이스를 쓰는 **누구나** 똑같은 서버를 자동으로 갖게 돼요 |

## 5. context 비용

<div class="oa-eli">
  <span class="oa-eli__head">📱 쉽게 말하면</span>
  <p>안 쓰는 앱도 깔려 있으면 <b>홈 화면 자리</b>를 차지하죠. 앱 본체가 안 열려 있어도 아이콘은 늘 거기 있어요. 안 쓰는 건 지워야 화면이 깔끔해요.</p>
</div>

MCP 서버는 **실제로 쓰지 않을 때도** tool 이름과 instructions를 context window에 추가해요. 연결된 서버마다 얼마간의 공간을 차지하니, `/mcp`로 연결 상태를 보고 안 쓰는 건 비활성화하세요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">⌨️</span><span class="oa-card__title">CLI 대체재</span><code>gh</code>(GitHub), <code>aws</code>(AWS)처럼 CLI가 있으면 CLI가 더 context 효율적이에요. tool마다 목록(listing)을 추가하지 않기 때문이에요</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📖</span><span class="oa-card__title">Skill과 짝짓기</span>서버의 tool을 잘 쓰는 법을 가르치는 Skill을 함께 두면 좋아요. Skill은 이름과 설명만 로드되고, 필요할 때만 전체가 로드돼요</div>
  <div class="oa-card"><span class="oa-card__icon">🔎</span><span class="oa-card__title">Tool search</span>기본으로 켜져 있어요. 각 MCP tool의 전체 정의를 Claude가 필요로 할 때까지 context 밖에 둬요</div>
</div>

### 예전 노트와 달라진 점

| 항목 | 예전 설명 | 현재 강의 본문 |
|---|---|---|
| 상시 올라가는 것 | tool 정의 | tool **이름과 instructions** |
| CLI가 효율적인 이유 | 지속적인 tool 정의를 추가하지 않아서 | tool마다 **listing**을 추가하지 않아서 |
| Skill | MCP보다 "더 나을 수도 있는" 대안 | MCP 서버와 **짝지어** 쓰는 것 |
| tool search | MCP tool이 context window의 10%를 넘으면 자동 전환 (신뢰성이 다소 떨어질 수 있음) | **기본으로 켜져** 있음 |

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. MCP가 잇는 "간극"은 무엇인가요?</summary>
  <p><b>정답: 코드베이스 밖에 있는 맥락.</b> 데이터베이스, 생산성 앱, 공개 저장소 같은 곳에 있는 정보를 Claude Code에 연결해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 팀원 모두가 똑같은 MCP 서버를 자동으로 갖게 하려면 어떤 scope를 쓰나요?</summary>
  <p><b>정답: Project.</b> <code>.mcp.json</code> 파일을 버전 관리에 체크인해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 내 컴퓨터에서 도는 로컬 프로세스를 MCP 서버로 연결하려면 어떤 유형인가요?</summary>
  <p><b>정답: Stdio 서버.</b> HTTP 서버는 서비스 제공자가 호스팅하는 원격 서비스용이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. GitHub 작업에 MCP 서버 대신 <code>gh</code> CLI를 쓰면 context 면에서 왜 유리한가요?</summary>
  <p><b>정답: CLI는 tool마다 listing을 context에 추가하지 않기 때문이에요.</b> MCP 서버는 쓰지 않을 때도 tool 이름과 instructions를 올려요.</p>
</details>

## 정리

- MCP는 Claude Code를 **외부 도구·데이터 소스**에 연결하는 개방형 표준이에요.
- `claude mcp add`로 서버를 추가해요. 원격 서비스는 **HTTP**, 로컬 프로세스는 **Stdio**예요.
- scope는 **Local · User · Project**이고, `.mcp.json`으로 프로젝트에 scope하면 팀이 자동으로 같은 서버를 써요.
- 연결된 서버마다 context 공간을 차지하니, `/mcp`로 확인하고 **안 쓰는 서버는 끄세요.**
- CLI 대체재가 있으면 CLI가 더 효율적이고, Skill을 짝지으면 tool을 더 잘 쓰게 돼요. Tool search는 기본으로 켜져 있어요.
