---
title: "More ways to work with Claude"
title_ko: "Claude를 사용하는 다른 방법들"
course: claude-101
lesson: 11
---

## 학습 목표

- Claude.ai 외에 Claude를 쓸 수 있는 주요 제품 구분하기
- **Claude Code · Claude Tag · Claude Design · Claude for Microsoft 365 · Claude in Chrome**을 언제 쓰면 좋은지 이해하기
- 작업 환경에 따라 알맞은 Claude interface를 고르는 기준 세우기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>출발점</strong>Claude는 intelligence, Claude.ai는 interface 하나</li>
  <li><strong>개발·협업</strong>Claude Code · Claude Tag(Slack)</li>
  <li><strong>만들기</strong>Claude Design</li>
  <li><strong>Office·브라우저</strong>Claude for Microsoft 365 · Claude in Chrome</li>
  <li><strong>고르는 기준</strong>작업이 일어나는 곳에 맞추기</li>
</ol>

이 레슨을 한 문장으로 줄이면: **"어떤 Claude가 더 좋은가"가 아니라 "내 작업이 어디에서 일어나는가"로 골라요.**

## 1. Claude는 intelligence, Claude.ai는 한 interface

<div class="oa-eli">
  <span class="oa-eli__head">👩‍⚕️ 쉽게 말하면</span>
  <p>같은 의사라도 <b>진료실, 수술실, 왕진</b>에서 쓸 수 있는 도구와 할 수 있는 일이 달라요. 의사의 실력은 그대로인데 장소가 달라지는 거죠. Claude도 똑같아요. 어디서 만나느냐에 따라 볼 수 있는 맥락과 할 수 있는 action이 달라져요.</p>
</div>

이 강의의 시작에서 Claude는 하나의 **intelligence**라고 했어요. Claude.ai는 Claude와 일하는 대표적인 방법이지만 유일한 방법은 아니에요. Claude는 내가 이미 일하고 있는 환경에 맞춰 여러 specialized tool로 제공돼요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">💻</span><span class="oa-card__title">Claude Code</span>terminal, IDE, browser에서 하는 개발 작업</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">Claude Tag</span>Slack 채널과 thread 안에서 호출</div>
  <div class="oa-card"><span class="oa-card__icon">🎨</span><span class="oa-card__title">Claude Design</span>아이디어를 작동하는 interface prototype으로</div>
  <div class="oa-card"><span class="oa-card__icon">📑</span><span class="oa-card__title">Claude for Microsoft 365</span>Excel · PowerPoint · Word · Outlook sidebar</div>
  <div class="oa-card"><span class="oa-card__icon">🌐</span><span class="oa-card__title">Claude in Chrome</span>browser 안의 웹 탐색과 반복 작업</div>
</div>

중요한 건 **내가 지금 하는 작업이 어디에서 일어나고, 어떤 권한과 맥락이 필요한가**예요.

## 2. Claude Code — 개발자가 일하는 곳에서

<div class="oa-eli">
  <span class="oa-eli__head">🛠️ 쉽게 말하면</span>
  <p>설명서를 읽어 주는 상담원이 아니라, <b>내 작업실에 직접 들어와서 공구를 잡는 동료 개발자</b>예요. 코드를 읽고, 명령을 실행하고, 고친 뒤 정리까지 해요.</p>
</div>

**Claude Code**는 terminal, IDE, browser, 심지어 Slack까지 개발자가 일하는 환경에서 작동하는 **agentic coding tool**이에요. codebase를 이해하고, 명령을 실행하고, 자연어 요청으로 개발 workflow 전체를 처리해요. 소프트웨어 개발을 위한 Claude예요.

이럴 때 써요.

- 자연어로 기능을 설명하면 Claude가 코드를 작성하고, 테스트를 실행하고, commit까지 만들게 하고 싶을 때
- error message를 붙여 넣고 codebase를 분석해 bug를 찾고 고치게 하고 싶을 때
- 낯선 codebase에서 여러 부분이 어떻게 연결되는지 묻고 싶을 때
- lint error 수정, merge conflict 해결, release note 작성 같은 지루한 반복 작업을 자동화하고 싶을 때
- 별도 interface로 옮기지 않고 terminal과 기존 IDE 옆에서 작업하고 싶을 때

```text
이 에러가 어디서 발생하는지 찾아서 원인을 설명하고, 최소 수정으로 고쳐줘. 테스트도 실행해줘.
```

## 3. Claude Tag — Slack 안의 Claude

<div class="oa-eli">
  <span class="oa-eli__head">🙋 쉽게 말하면</span>
  <p>회의실에 <b>이미 앉아 있는 동료를 이름으로 부르는 것</b>과 같아요. 대화 내용을 따로 옮겨 적어 설명할 필요 없이, thread에서 tag만 하면 그 자리의 맥락을 보고 도와줘요.</p>
</div>

**Claude Tag**는 Claude를 Slack에 직접 가져와요. 채널과 thread 안에서 도움을 받거나, Slack 맥락을 Claude conversation으로 가져올 수 있어요. 아무 thread에서나 Claude를 tag하면 돼요.

이럴 때 써요.

- Slack을 떠나지 않고 메시지 답변 초안을 쓰거나, 긴 thread를 요약하거나, 복잡한 논의를 정리하고 싶을 때
- 미팅 준비를 위해 workspace의 관련 대화와 공유 문서를 모으고 싶을 때
- 새 팀에 합류해 channel history로 ongoing project를 이해하고 싶을 때
- bug report나 feature discussion에서 바로 coding task를 넘기고 싶을 때 — Claude를 tag하면 주변 맥락을 써서 **Claude Code session**을 띄울 수 있어요
- 대화 중 업계 trend, 기술 개념, 회사 정보를 빠르게 묻고 싶을 때

```text
@Claude 이 thread에서 결정된 내용과 남은 액션 아이템을 요약해줘.
```

## 4. Claude Design — 아이디어를 작동하는 화면으로

<div class="oa-eli">
  <span class="oa-eli__head">✏️ 쉽게 말하면</span>
  <p>냅킨에 그린 스케치를 건네면 <b>눌러 볼 수 있는 시제품</b>으로 돌려주는 디자이너예요. "이런 느낌"을 말로만 설명하지 않고, 바로 만져 보면서 고칠 수 있어요.</p>
</div>

**Claude Design**은 아이디어를 작동하는 interface로 바꿔 줘요. 원하는 걸 평범한 말로 설명하거나 스케치·screenshot에서 시작하면, Claude가 **interactive prototype을 artifact로** 만들어요. 그 artifact는 직접 편집하고, 팀과 공유하고, Claude Code에 넘겨 실제로 만들 수 있어요.

Claude와의 어떤 대화 안에서도, Artifacts tab에서도 작동하고, `claude.ai/design`이라는 전용 공간으로도 제공돼요.

이럴 때 써요.

- 글로 쓴 brief, 스케치, reference screenshot에서 코드 없이 작동하는 UI prototype으로 가고 싶을 때
- 디자인 방향을 탐색하면서 여러 variation을 빠르게 만들어 비교하고 싶을 때
- markup을 건드리지 않고 layout, copy, interaction을 반복해서 다듬고 싶을 때 — canvas에서, comment로, 또는 요청해서
- 팀의 design system을 쓰는 prototype이 필요할 때 — 넘겨주는 결과물이 engineering이 만들 것과 맞도록

> **제공 범위** — 대화 안의 Claude Design은 **beta**이고 유료 plan(Pro, Max, Team, Enterprise)에서 쓸 수 있어요. Pro와 Max는 기본으로 켜져 있어요. Team도 기본으로 켜져 있고 admin이 끌 수 있어요. Enterprise는 admin이 `Organization settings > Artifacts`에서 켜야 해요. 독립된 `claude.ai/design` 공간은 별도 설정을 따라요.

## 5. Claude for Microsoft 365 — Office 안에서 바로

<div class="oa-eli">
  <span class="oa-eli__head">🪑 쉽게 말하면</span>
  <p>파일을 들고 조수 방까지 갔다 오는 대신, <b>조수가 내 책상 옆자리에 앉는 것</b>이에요. 열어 둔 엑셀·파워포인트·워드·메일을 같이 보면서 그 자리에서 고쳐요.</p>
</div>

Excel, PowerPoint, Word, Outlook 안에 **sidebar**로 Claude를 불러와요. 열려 있는 파일을 그 자리에서 편집하고, 문서 사이에 맥락을 이어 가는 데 잘 맞아요.

| 제품 | 하는 일 |
|---|---|
| **Claude for Excel** | spreadsheet를 대화로 분석하고, 이해하고, 수정해요 |
| **Claude for PowerPoint** | 기존 template과 brand styling을 유지하면서 deck을 작성·편집·재구성해요 |
| **Claude for Word** | 열어 둔 문서를 작성·수정·재구성해요. tracked changes와 comments를 다루고, 연결된 source에서 context를 가져와 근거를 붙여요 |
| **Claude for Outlook** | inbox에서 메일을 분류하고, 관련 thread와 calendar 맥락으로 답장 초안을 쓰고, 긴 email chain을 요약이나 next steps로 정리해요 |

### Claude for Excel

Finance, operations, analysis 업무에서 특히 유용해요.

- 여러 tab으로 구성된 복잡한 workbook에서 formula나 calculation flow를 이해하고 싶을 때
- model의 assumption이나 input을 바꾸되 formula dependency와 관계를 유지하고 싶을 때
- `#REF!`, `#VALUE!`, circular reference 같은 spreadsheet error를 원인까지 추적하고 수정 제안을 받고 싶을 때
- 새 spreadsheet를 만들거나 기존 template에 data를 채우되 formula 구조를 유지하고 싶을 때
- pivot table이나 chart를 빠르게 만들어 data를 시각화하고 싶을 때

```text
이 workbook에서 revenue forecast가 어떤 sheet와 formula를 거쳐 계산되는지 설명해줘.
```

### Claude for PowerPoint

- outline, document, notes를 slide를 하나씩 손으로 만들지 않고 첫 deck 초안으로 바꾸고 싶을 때
- slide copy를 다시 쓰거나 다듬고 싶을 때 — bullet 줄이기, speaker notes 추가, audience에 맞게 tone 조정
- 기존 deck을 재구성하고 싶을 때 — section 순서 바꾸기, dense slide 나누기, 겹치는 slide 합치기
- deck 전체에 title, bullet style, layout formatting을 일관되게 적용하고 싶을 때
- 특정 slide에 어떤 layout이나 chart type이 맞는지 시각적 제안을 받고 싶을 때

```text
이 strategy memo를 12장짜리 executive presentation으로 바꿔줘.
기존 template과 brand style은 유지해줘.
```

### Claude for Word

- outline이나 거친 notes를 팀 template에 맞춘 구조화된 첫 초안으로 바꾸고 싶을 때
- 문서를 떠나지 않고 한 section을 고치고 싶을 때 — 글 다듬기, 특정 독자에 맞게 tone 조정, 구조 재작업
- reviewer comments와 tracked changes에 그 자리에서 대응하고 싶을 때
- 초안이 연결된 source 자료에 근거하게 하고 싶을 때 — 문서의 claim을 출처까지 거슬러 올라갈 수 있도록

### Claude for Outlook

inbox 안의 sidebar에서 메일 분류, 맥락을 반영한 답장 초안, 긴 email chain 요약을 도와요.

> **제공 범위** — Claude for Outlook은 현재 **beta**이고, 다른 Microsoft 365 add-in과 **따로 설치**해요.

## 6. Claude in Chrome — 브라우저 안의 Claude

<div class="oa-eli">
  <span class="oa-eli__head">🚗 쉽게 말하면</span>
  <p>길을 말로만 알려 주는 내비게이션이 아니라, <b>조수석에서 같은 화면을 보다가 필요하면 대신 운전도 해 주는 동승자</b>예요. 그래서 큰길(신뢰할 수 있는 사이트)에서, 위험하지 않은 구간(low-risk task)부터 맡기는 게 좋아요.</p>
</div>

**Claude in Chrome**은 Google Chrome에 Claude를 sidebar로 추가하는 browser extension이에요. 내가 작업 중인 화면을 관찰하고, browser 안에서 직접 action을 수행해요.

이럴 때 써요.

- 탐색 중인 article, research paper, web page를 요약하고 싶을 때
- email response 초안 작성이나 inbox 관리 도움을 받고 싶을 때
- 반복적인 form 입력을 자동화하고 싶을 때
- website feature를 테스트하거나, 단계를 하나씩 클릭하지 않고 multi-step workflow를 탐색하게 하고 싶을 때
- tab과 task를 오가도 맥락을 유지하는 browsing assistant가 필요할 때 — niche internal tool, CRM, dashboard처럼 connector가 없는 곳에서 context를 가져오기에 좋아요

```text
이 dashboard에서 지난달 conversion이 떨어진 원인을 찾는 데 필요한 지표를 읽고 요약해줘.
```

### 제공 범위와 주의할 점

Claude in Chrome은 **generally available**이에요.

| Plan | 기본 상태 |
|---|---|
| Pro · Max · Team | 기본으로 켜져 있어요 |
| Enterprise | 기본으로 꺼져 있고, org admin이 켜야 해요 |
| Free | 쓸 수 없어요 |

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">Low-risk task부터</span>Anthropic은 신뢰할 수 있는 website에서 low-risk task에 쓰기를 권장해요</div>
  <div class="oa-card"><span class="oa-card__icon">✋</span><span class="oa-card__title">High-risk action 전 확인</span>구매나 개인 정보 공유 같은 action 전에는 extension이 permission을 요청해요</div>
  <div class="oa-card"><span class="oa-card__icon">🚫</span><span class="oa-card__title">일부 category 차단</span>금융 서비스, adult content 같은 website category는 기본적으로 차단돼요</div>
</div>

## 7. 어떤 Claude를 선택할까요?

<div class="oa-eli">
  <span class="oa-eli__head">🧭 쉽게 말하면</span>
  <p>못은 망치로, 나사는 드라이버로. 도구가 좋고 나쁜 게 아니라 <b>지금 손에 든 일이 무엇이냐</b>가 선택 기준이에요.</p>
</div>

### 도구별 비교

| Tool | 잘 맞는 작업 | 실행 위치 |
|---|---|---|
| **Claude.ai** | 일반 업무, research, writing, analysis, file creation | Web, desktop, mobile apps |
| **Claude Code** | software development, codebase navigation, git workflow | Terminal/command line, IDE, browser |
| **Claude Cowork** | 복잡한 multi-step task: research brief, document creation, file organization, data analysis | Desktop (web과 mobile은 beta, 대상 plan에서) |
| **Claude Tag** | 팀 협업, meeting prep, 맥락 안의 빠른 답변 | Slack workspace |
| **Claude Design** | UI prototype, 디자인 탐색, design system을 반영한 mockup | Claude와의 모든 대화(유료 plan), 그리고 `claude.ai/design` |
| **Claude for Microsoft 365** | 그 자리에서 편집하기, 문서 사이에 맥락 이어 가기 | Excel, PowerPoint, Word, Outlook sidebar |
| **Claude in Chrome** | web research, email management, browser automation | Chrome browser sidebar |

### 작업 환경으로 고르기

- 대화, 글쓰기, 조사, 파일 생성 → **Claude.ai**
- codebase 안에서 개발 작업 → **Claude Code**
- 여러 source를 모으는 긴 지식 작업 → **Claude Cowork**
- 팀 대화 맥락에서 즉시 도움 → **Claude Tag**
- 아이디어를 UI prototype으로 → **Claude Design**
- spreadsheet · slide deck · 문서 · 메일을 그 자리에서 → **Claude for Microsoft 365**
- browser 안의 web workflow → **Claude in Chrome**

> 같은 Claude intelligence라도, interface가 달라지면 접근할 수 있는 **context와 action**이 달라져요.

### 스스로 물어볼 질문

- 내 업무 대부분은 web chat으로 충분한가요, 아니면 특정 tool 안에서 Claude가 필요할까요?
- 개발 관련 작업은 Claude.ai보다 Claude Code가 더 맞지 않을까요?
- Slack 대화에서 바로 요약하거나 action item을 뽑으면 좋은 thread는 무엇인가요?
- Excel, PowerPoint, Word에서 반복적으로 하는 수작업은 무엇인가요?
- browser에서 반복 클릭하거나 여러 페이지를 탐색하는 workflow가 있나요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 짝 맞추기 — 상황과 알맞은 도구</summary>
  <p>"error message를 붙여 넣고 codebase에서 bug를 찾아 고치게 하고 싶어요" → <b>Claude Code</b><br>"긴 Slack thread에서 결정 사항을 바로 요약받고 싶어요" → <b>Claude Tag</b><br>"스케치를 눌러 볼 수 있는 UI prototype으로 만들고 싶어요" → <b>Claude Design</b><br>"workbook의 <code>#REF!</code> error를 원인까지 추적하고 싶어요" → <b>Claude for Excel</b><br>"connector가 없는 사내 dashboard의 내용을 읽어 요약받고 싶어요" → <b>Claude in Chrome</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. Claude in Chrome에 대한 설명으로 맞는 것은?</summary>
  <p>① Free plan에서도 쓸 수 있어요 ② 구매 같은 high-risk action도 묻지 않고 진행해요 ③ 신뢰할 수 있는 website의 low-risk task에 쓰기를 권장하고, high-risk action 전에는 permission을 요청해요 ④ 모든 website category에서 제한 없이 작동해요</p>
  <p><b>정답: ③</b> — Free plan에서는 쓸 수 없고, 금융 서비스나 adult content 같은 category는 기본적으로 차단돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Claude interface를 고를 때 강의가 제시하는 기준은?</summary>
  <p>① 가장 최근에 나온 제품 ② 가장 기능이 많은 제품 ③ 작업이 어디에서 일어나고 어떤 권한과 맥락이 필요한가 ④ 항상 Claude.ai</p>
  <p><b>정답: ③</b> — 같은 intelligence라도 interface에 따라 접근할 수 있는 context와 action이 달라지기 때문이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Slack의 bug report thread에서 Claude를 tag하면 할 수 있는 일은?</summary>
  <p><b>정답:</b> thread 주변 맥락을 써서 Claude Code session을 띄우고 coding task를 바로 넘길 수 있어요. Slack을 떠나 맥락을 다시 설명할 필요가 없어요.</p>
</details>

## 정리

- Claude는 하나의 **intelligence**이고, Claude.ai는 여러 interface 중 하나예요.
- **Claude Code**는 개발 workflow에, **Claude Tag**는 Slack의 팀 대화 맥락에 특화되어 있어요.
- **Claude Design**은 brief·스케치·screenshot을 편집 가능한 interactive prototype으로 만들어요. 대화 안 기능은 beta이고 유료 plan에서 쓸 수 있어요.
- **Claude for Microsoft 365**는 Excel, PowerPoint, Word, Outlook sidebar에서 열어 둔 파일을 그 자리에서 다뤄요. Outlook은 beta이고 따로 설치해요.
- **Claude in Chrome**은 browser 안의 정보와 action을 Claude에 연결해요. 신뢰할 수 있는 website의 low-risk task부터 써요.
- 고르는 기준은 **작업이 일어나는 곳**과 필요한 context·action이에요.

다음 단계에서는 이 강의를 짧게 recap하고, completion badge를 받기 위한 quiz로 마무리해요.
