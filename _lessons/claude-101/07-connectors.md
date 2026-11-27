---
title: "Connectors"
title_ko: "Connectors로 Claude 확장하기"
course: claude-101
lesson: 7
---

## 학습 목표

- **Connectors**가 무엇이고 Claude와의 작업에서 왜 중요한지 설명하기
- **Connector directory**에서 도구를 찾고 첫 연결을 설정하기
- 연결된 도구를 Claude 대화에서 효과적으로 사용하기
- **Web connectors**와 **desktop extensions**의 차이 구분하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>왜 필요한가</strong>업무 맥락은 chat 밖 여러 도구에 흩어져 있어요</li>
  <li><strong>무엇인가</strong>MCP 기반으로 Claude와 도구를 잇는 연결</li>
  <li><strong>두 가지 유형</strong>Web connectors · Desktop extensions</li>
  <li><strong>연결하기</strong>Directory → Connect → 로그인 → 권한 승인 → 테스트</li>
  <li><strong>안전하게 쓰기</strong>범위 제한 · 내가 보는 것만 · 언제든 해제</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Connector는 복사·붙여넣기 없이 Claude가 내 도구의 실제 정보로 일하게 해 주는 연결이에요.**

## 1. Connectors란? 왜 중요한가요?

<div class="oa-eli">
  <span class="oa-eli__head">🔑 쉽게 말하면</span>
  <p>새로 온 동료가 아무리 똑똑해도 사내 드라이브·메신저·업무 툴 계정이 없으면 매번 자료를 출력해서 건네줘야 하죠. Connector는 그 동료에게 <b>내가 쓰는 도구의 출입증</b>을 주는 거예요. 단, 내 출입증으로 열 수 있는 문만 열려요.</p>
</div>

**Connectors**는 Claude가 내 실제 업무 도구, 데이터, 맥락에 접근할 수 있게 해 주는 연결 기능이에요. Claude를 "대답하는 assistant"에서 **내가 매일 쓰는 도구와 같은 정보를 보는 collaborator**로 바꿔 줘요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ Connector가 없을 때</span>
    <ul>
      <li>매번 정보를 복사해서 Claude에게 붙여넣어요.</li>
      <li>대화를 시작할 때마다 맥락을 처음부터 설명해요.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ Connector가 있을 때</span>
    <ul>
      <li>Claude가 Google Drive, Notion, Slack, Asana 같은 서비스에서 필요한 정보를 <b>직접</b> 찾아요.</li>
      <li>권한에 따라 연결된 앱 안에서 <b>작업까지</b> 수행해요.</li>
    </ul>
  </div>
</div>

Claude가 더 좋은 답을 하려면 맥락이 필요한데, 업무 맥락은 보통 chat 안에만 있지 않아요.

| 맥락의 종류 | 실제로 있는 곳 |
|---|---|
| 문서 | Google Drive, Notion |
| 의사결정 | Slack thread |
| task | Asana, Linear, Jira |
| 고객·영업 정보 | HubSpot 같은 CRM |
| 결제·매출 데이터 | Stripe, PayPal |

### Connectors가 할 수 있는 일

Connector와 내가 부여한 권한에 따라 Claude는 대화 안에서 이런 일을 할 수 있어요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔎</span><span class="oa-card__title">파일 검색</span>연결된 서비스에서 필요한 파일 찾기</div>
  <div class="oa-card"><span class="oa-card__icon">📄</span><span class="oa-card__title">문서 가져오기</span>문서를 불러와 대화에서 활용</div>
  <div class="oa-card"><span class="oa-card__icon">📊</span><span class="oa-card__title">데이터 분석</span>도구에 있는 데이터를 바로 분석</div>
  <div class="oa-card"><span class="oa-card__icon">✍️</span><span class="oa-card__title">새 content 생성</span>연결된 앱 안에 새 내용 만들기</div>
  <div class="oa-card"><span class="oa-card__icon">🔄</span><span class="oa-card__title">record 업데이트</span>기존 기록 수정</div>
  <div class="oa-card"><span class="oa-card__icon">⚡</span><span class="oa-card__title">task 실행</span>연결된 앱 안에서 작업 수행</div>
</div>

> 모든 connector가 같은 권한을 갖는 건 아니에요. 어떤 connector는 **읽기 중심**이고, 어떤 connector는 **작성·수정 action**도 지원해요.

## 2. MCP — Connectors를 움직이는 표준

<div class="oa-eli">
  <span class="oa-eli__head">🔌 쉽게 말하면</span>
  <p>예전엔 기기마다 충전기가 달랐지만 지금은 USB-C 하나로 다 꽂죠. <b>MCP는 AI를 위한 USB-C</b>예요. 도구마다 다른 연결 방식을 만들 필요 없이, 하나의 규격으로 Claude에 꽂을 수 있어요.</p>
</div>

Connectors는 **Model Context Protocol(MCP)**을 기반으로 해요. 하나의 일관된 interface로 Claude가 여러 application과 연결돼요.

<div class="oa-svg">
<svg viewBox="0 0 720 220" role="img" aria-label="Claude가 MCP 기반 Connector를 거쳐 여러 도구와 연결되는 구조">
  <defs>
    <marker id="ar_c101_07" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.b1{fill:#f6f3ea;stroke:#e5dfcf;stroke-width:1.5}.b2{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.b3{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.h1{font-size:15px;font-weight:700;fill:#333}.t1{font-size:12px;fill:#4a4a4a}.s1{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect class="b1" x="10" y="60" width="150" height="80" rx="10"/>
  <text class="h1" x="85" y="96" text-anchor="middle">Claude</text>
  <text class="s1" x="85" y="118" text-anchor="middle">대화 안에서 요청</text>
  <line x1="164" y1="100" x2="232" y2="100" stroke="#c99400" stroke-width="2" marker-end="url(#ar_c101_07)"/>
  <rect class="b2" x="240" y="50" width="190" height="100" rx="10"/>
  <text class="h1" x="335" y="86" text-anchor="middle">MCP 기반 Connector</text>
  <text class="t1" x="335" y="110" text-anchor="middle">하나의 일관된 interface</text>
  <text class="s1" x="335" y="130" text-anchor="middle">open standard</text>
  <line x1="434" y1="85" x2="502" y2="50" stroke="#c99400" stroke-width="2" marker-end="url(#ar_c101_07)"/>
  <line x1="434" y1="115" x2="502" y2="150" stroke="#c99400" stroke-width="2" marker-end="url(#ar_c101_07)"/>
  <rect class="b3" x="510" y="10" width="200" height="80" rx="10"/>
  <text class="h1" x="610" y="40" text-anchor="middle">Web connectors</text>
  <text class="t1" x="610" y="62" text-anchor="middle">Google Drive · Notion</text>
  <text class="t1" x="610" y="79" text-anchor="middle">Slack · Asana …</text>
  <rect class="b3" x="510" y="110" width="200" height="80" rx="10"/>
  <text class="h1" x="610" y="140" text-anchor="middle">Desktop extensions</text>
  <text class="t1" x="610" y="162" text-anchor="middle">로컬 파일 · native app</text>
  <text class="s1" x="610" y="179" text-anchor="middle">Claude Desktop app 필요</text>
  <text class="s1" x="335" y="212" text-anchor="middle">도구가 달라도 Claude 쪽 연결 방식은 하나예요</text>
</svg>
</div>

MCP가 **open standard**라서 개발자는 어떤 도구든 connector를 만들 수 있고, Claude는 그 connector들을 일관된 방식으로 써요.

## 3. Connector의 두 가지 유형

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면</span>
  <p>Web connector는 <b>인터넷 너머 클라우드 서비스로 가는 다리</b>, desktop extension은 <b>내 컴퓨터 안 서랍을 여는 열쇠</b>예요. 어디에 있는 자료를 쓰느냐에 따라 골라요.</p>
</div>

| 구분 | Web connectors | Desktop extensions |
|---|---|---|
| **연결 대상** | cloud service | 내 컴퓨터의 local files, native applications, macOS/Windows 기능 |
| **예시** | Gmail, Notion, Slack, Asana, Linear, Stripe, Google Drive | 로컬 파일 접근(문서 읽기·정리), browser control(웹 작업 자동화), Figma 같은 native application integration |
| **어디서 쓰나** | 브라우저 기반 Claude에서도 사용 가능 | 웹 인터페이스가 아니라 **Claude Desktop app** 필요 |
| **연결 방식** | 서비스 로그인 + 권한 승인 | `Settings > Extensions`에서 설치 |
| **잘 맞는 작업** | cloud에 있는 문서·대화·데이터 | local environment와 가까운 작업 |

## 4. Connector 찾고 연결하기

<div class="oa-eli">
  <span class="oa-eli__head">📱 쉽게 말하면</span>
  <p>스마트폰에 앱을 깔 때 <b>앱스토어에서 찾고 → 설치하고 → 권한을 허용</b>하죠. Connector도 똑같아요. Directory가 앱스토어 역할을 해요.</p>
</div>

### 찾는 곳

Anthropic은 추천 connector directory를 제공해요.

```text
claude.ai/directory
```

chat window 왼쪽 아래 `+` 버튼을 누른 뒤 `Connectors`를 선택해도 돼요. Directory는 두 탭으로 나뉘어요.

| 탭 | 설명 |
|---|---|
| **Web** | Gmail, Notion, Slack, Asana, Linear, Stripe 같은 cloud service |
| **Desktop extensions** | Claude Desktop app을 통해 로컬에서 실행되는 도구 |

> Directory는 개별 앱이 아니라 **connector 단위**로 나열돼요. 하나의 항목이 관련 도구 여러 개를 담당할 수 있어요. 예를 들어 **Atlassian Rovo** connector는 Jira와 Confluence를 모두 다루니까, 앱 이름 대신 Atlassian으로 찾아야 해요. 필요한 도구가 목록에 없으면 **custom connector**로 추가할 수 있어요.

### Web connector 설정하기

<ol class="oa-flow">
  <li><strong>찾기</strong><code>claude.ai/directory</code> 또는 chat에서 <code>+ &gt; Connectors</code></li>
  <li><strong>Connect</strong>추가할 connector를 고르고 <code>Connect</code> 클릭</li>
  <li><strong>로그인</strong>해당 서비스 login page에서 기존 계정으로 로그인</li>
  <li><strong>권한 승인</strong>Claude가 요청하는 권한을 검토하고 승인</li>
  <li><strong>테스트</strong>Claude로 돌아와 간단한 요청으로 확인</li>
</ol>

테스트는 이런 식으로 해요.

```text
내 Google Drive에 접근할 수 있어?
```

연결 후에는 부여한 권한에 따라 Claude가 해당 서비스에서 검색, 읽기, 일부 action 수행을 할 수 있어요.

### Desktop extension 설치하기

<ol class="oa-flow">
  <li><strong>앱 설치</strong>Claude Desktop app을 내려받아 설치</li>
  <li><strong>메뉴 이동</strong>앱에서 <code>Settings &gt; Extensions</code></li>
  <li><strong>Install</strong>extensions를 둘러보고 원하는 것을 <code>Install</code></li>
  <li><strong>추가 설정</strong>extension별 추가 설정 완료</li>
</ol>

Desktop extension은 로컬 파일이나 native app과 연결되니까, web connector보다 local environment에 가까운 작업에 잘 맞아요.

## 5. 업무에서 Connectors 사용하기

<div class="oa-eli">
  <span class="oa-eli__head">🗣️ 쉽게 말하면</span>
  <p>자료를 들고 와서 "이거 봐 줘"라고 하는 대신, <b>"거기 있는 거 찾아서 봐 줘"</b>라고 말하면 돼요. Connector를 연결하면 Claude가 대화 중에 관련 도구를 스스로 고려해요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📋</span><span class="oa-card__title">Project management</span>Asana, Linear, Jira</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">Communication</span>Slack, Gmail</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">Documentation</span>Notion, Google Drive, Confluence</div>
  <div class="oa-card"><span class="oa-card__icon">💼</span><span class="oa-card__title">Business tools</span>Stripe, PayPal, HubSpot</div>
</div>

### Project management (Asana, Linear, Jira)

```text
이번 주 마감인 가장 우선순위 높은 task가 뭐야?
```

```text
Q4 budget proposal review task를 새로 만들어줘.
```

```text
제품 launch project의 현재 상태를 요약해줘.
```

### Communication (Slack, Gmail)

대화와 이메일 맥락을 찾을 수 있어요.

```text
vendor contract에 대해 논의한 email thread를 찾아줘.
```

```text
#marketing 채널의 최신 메시지에 답장 초안을 써줘.
```

```text
어제 논의에서 timeline에 대해 팀이 결정한 내용을 요약해줘.
```

### Documentation (Notion, Google Drive, Confluence)

문서 기반 작업이 쉬워져요.

```text
우리 brand voice guideline을 documentation에서 찾아줘.
```

```text
지난주 product review 회의록을 요약해줘.
```

```text
우리 style guide는 contraction 사용에 대해 뭐라고 해?
```

### Business tools (Stripe, PayPal, HubSpot)

business data에 접근할 때 유용해요.

```text
지난 분기 revenue trend를 보여줘.
```

```text
Acme Corp opportunity 상태가 어떻게 돼?
```

```text
$1,000를 넘는 최근 transaction을 나열해줘.
```

## 6. 보안과 권한

<div class="oa-eli">
  <span class="oa-eli__head">🪪 쉽게 말하면</span>
  <p>Claude에게 주는 건 마스터키가 아니라 <b>내 사원증 복사본</b>이에요. 내가 못 들어가는 방은 Claude도 못 들어가고, 필요 없어지면 언제든 회수할 수 있어요.</p>
</div>

Connector를 연결한다는 건 Claude에게 외부 서비스의 데이터를 **읽거나, 경우에 따라 수정할** 권한을 주는 거예요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">Scoped access</span>권한은 connector가 필요한 범위로 제한돼요. 각 application menu에서 개별 권한을 켜고 끌 수 있어요.</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">👀</span><span class="oa-card__title">Claude sees what you see</span>Claude는 <b>내가 접근 권한을 가진 데이터만</b> 봐요. work email을 연결해도 CEO의 inbox는 못 봐요. 내 것만 보여요.</div>
  <div class="oa-card"><span class="oa-card__icon">🔓</span><span class="oa-card__title">언제든 해제 가능</span>Claude settings나 third-party service의 security settings에서 연결을 끊을 수 있어요.</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⚠️</span><span class="oa-card__title">신뢰할 수 있는 connector만</span>Skills처럼 custom connector도 찾거나 만들 수 있어요. 같은 수준으로 조심해서, 신뢰할 수 있는 source의 것만 설치하고 어떤 권한을 요청하는지 확인해요.</div>
</div>

### 스스로 물어볼 질문

- 내가 매일 쓰는 도구 중 Claude와 연결하면 가장 가치가 큰 건 무엇인가요?
- 지금 복사해서 붙여넣는 정보 중 connector가 자동으로 처리할 수 있는 건 무엇인가요?
- 여러 source의 데이터를 합쳐야 해서 시간이 오래 걸리는 workflow는 무엇인가요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 강의가 MCP를 "AI를 위한 USB-C"에 비유한 이유는?</summary>
  <p>① Claude의 답변 속도를 높여 줘서 ② 하나의 일관된 표준 interface로 여러 application을 연결할 수 있어서 ③ 유료 plan에서만 쓸 수 있어서 ④ 로컬 파일만 다뤄서</p>
  <p><b>정답: ②</b> — MCP는 open standard라서 개발자가 어떤 도구든 connector를 만들 수 있고, Claude는 같은 방식으로 써요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — Web connector와 Desktop extension</summary>
  <p>"Gmail·Notion·Slack 같은 cloud service에 로그인해 권한을 승인" → <b>Web connector</b><br>"내 컴퓨터의 로컬 파일을 읽고 정리" → <b>Desktop extension</b><br>"Figma 같은 native application과 연동" → <b>Desktop extension</b><br>"Claude Desktop app의 <code>Settings &gt; Extensions</code>에서 설치" → <b>Desktop extension</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. work email을 Claude에 연결했어요. Claude가 볼 수 있는 범위는?</summary>
  <p>① 회사 전체 메일함 ② CEO를 포함한 상급자의 inbox ③ 내가 접근 권한을 가진 데이터만 ④ 연결한 순간 이후에 받은 메일만</p>
  <p><b>정답: ③</b> — "Claude sees what you see." Claude는 내가 볼 수 있는 것만 볼 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Directory에서 Jira를 검색했는데 Jira라는 항목이 안 보여요. 어떻게 해야 할까요?</summary>
  <p><b>정답: Atlassian으로 찾아봐요.</b> Directory는 앱이 아니라 connector 단위로 나열되고, Atlassian Rovo connector가 Jira와 Confluence를 함께 다뤄요. 정말 항목이 없는 도구라면 custom connector로 추가할 수 있어요.</p>
</details>

## 정리

- **Connectors**는 Claude가 실제 업무 도구와 데이터에 접근하게 해 줘요. 복사·붙여넣기가 줄고, 권한에 따라 action까지 수행해요.
- Connectors는 **MCP**라는 open standard를 기반으로 해서, 다양한 도구를 일관된 방식으로 Claude에 붙일 수 있어요.
- **Web connectors**는 cloud service와, **Desktop extensions**는 Claude Desktop app을 통해 로컬 도구와 연결해요.
- 연결은 `claude.ai/directory` 또는 `+ > Connectors`에서 시작해요. Directory는 connector 단위라 하나의 항목이 여러 앱을 담당할 수 있어요.
- 권한은 connector별로 관리되고, Claude는 **내가 접근 가능한 데이터만** 볼 수 있어요.
- 연결은 언제든 해제할 수 있고, custom connector는 신뢰할 수 있는 source의 것만 써요.

다음 레슨에서는 조직 지식을 검색하고 종합하는 Claude for Work 기능인 Enterprise Search를 다뤄요.
