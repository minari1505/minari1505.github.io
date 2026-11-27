---
title: "Subagents"
title_ko: "서브에이전트"
course: claude-code-101
lesson: 9
---

## 학습 목표

- subagent가 **context 관리**에 어떻게 도움을 주는지 이해하기
- **나만의 subagent를 만드는 흐름**과 저장 위치에 따른 적용 범위 파악하기
- **Persistent memory**, **skill preload** 같은 추가 커스터마이징 옵션 알기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>위임</strong>Claude가 "이 코드베이스를 탐색해 줘" 같은 작업을 subagent에 맡겨요</li>
  <li><strong>격리된 작업</strong>subagent가 자체 context window에서 병렬로 일해요</li>
  <li><strong>요약</strong>끝나면 발견한 내용을 요약해요</li>
  <li><strong>답만 반환</strong>주 context에는 요약만 돌아와요</li>
</ol>

이 레슨을 한 문장으로 줄이면: **무거운 탐색은 subagent에게 맡기고, 주 context에는 답만 돌려받아요.**

## 1. 어떻게 동작하나요

<div class="oa-eli">
  <span class="oa-eli__head">📚 쉽게 말하면</span>
  <p>팀장이 인턴에게 "도서관 가서 이 주제 좀 조사해 와"라고 시키는 거예요. 인턴은 책 수십 권을 뒤지지만, 팀장 책상에 올라오는 건 <b>한 장짜리 요약 보고서</b>뿐이에요. 팀장 책상은 깨끗하게 남아요.</p>
</div>

Claude는 작업을 subagent에게 **위임**할 수 있어요. subagent는 작업을 쪼개고 구성 작업을 **병렬로** 실행해서 context 관리를 개선해 줘요. 각 subagent는 **자기만의 격리된 context window**에서 동작해요.

왜 필요한지부터 볼게요. Claude Code에서 context 관리는 중요한데, context window의 상당 부분이 이런 데 소비돼요.

| context를 많이 쓰는 것 | 문제 |
|---|---|
| 코드베이스를 탐색하는 tool call | 탐색에서 발견한 내용이 정작 개발 중인 **주 기능과 늘 관련 있는 건 아니에요** |
| 조사용 웹 검색 | |

여기서 **subagent**가 등장해요.

<div class="oa-svg">
<svg viewBox="0 0 720 230" role="img" aria-label="main agent가 subagent에 작업을 맡기고 요약만 돌려받는 구조">
  <defs>
    <marker id="ar_cc109" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.m9{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.s9{fill:#f6f3ea;stroke:#e5dfcf;stroke-width:1.5}.k9{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.h9{font-size:15px;font-weight:700;fill:#333}.t9{font-size:12px;fill:#4a4a4a}.c9{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect class="m9" x="10" y="30" width="250" height="150" rx="10"/>
  <text class="h9" x="135" y="56" text-anchor="middle">main agent의 context</text>
  <rect class="k9" x="30" y="72" width="210" height="34" rx="6"/>
  <text class="t9" x="135" y="94" text-anchor="middle">지금 개발 중인 주 기능</text>
  <rect class="k9" x="30" y="116" width="210" height="34" rx="6"/>
  <text class="t9" x="135" y="138" text-anchor="middle">subagent가 돌려준 요약</text>
  <text class="c9" x="135" y="170" text-anchor="middle">깨끗하게 유지돼요</text>
  <line x1="264" y1="78" x2="452" y2="78" stroke="#c99400" stroke-width="2" marker-end="url(#ar_cc109)"/>
  <text class="c9" x="358" y="68" text-anchor="middle">"이 코드베이스를 탐색해 줘"</text>
  <line x1="456" y1="134" x2="268" y2="134" stroke="#c99400" stroke-width="2" marker-end="url(#ar_cc109)"/>
  <text class="c9" x="358" y="152" text-anchor="middle">요약만 반환</text>
  <rect class="s9" x="460" y="30" width="250" height="150" rx="10"/>
  <text class="h9" x="585" y="56" text-anchor="middle">subagent의 context</text>
  <text class="t9" x="585" y="86" text-anchor="middle">파일 읽기 · tool call</text>
  <text class="t9" x="585" y="108" text-anchor="middle">웹 검색 · 탐색 과정 전체</text>
  <text class="t9" x="585" y="130" text-anchor="middle">→ 발견한 내용 요약</text>
  <text class="c9" x="585" y="170" text-anchor="middle">병렬로 실행, 별도의 context window</text>
  <text class="c9" x="360" y="215" text-anchor="middle">답에 이르는 "여정"은 오른쪽에 남고, 왼쪽에는 "답"만 와요</text>
</svg>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ subagent 없이</span>
    <ul>
      <li>탐색 tool call과 웹 검색 결과가 전부 주 context에 쌓여요</li>
      <li>주 기능과 무관한 내용이 공간을 차지해요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ subagent로</span>
    <ul>
      <li>원하던 <b>답만</b> 얻어요</li>
      <li>답에 이르기까지의 여정 전체가 주 context를 어지럽히지 않아요</li>
    </ul>
  </div>
</div>

## 2. 나만의 subagent 만들기

<div class="oa-eli">
  <span class="oa-eli__head">📇 쉽게 말하면</span>
  <p>전문가 <b>명함</b>을 만들어 두는 거예요. 이름, 하는 일, 일하는 방식을 적어 두면, Claude가 "이 일은 이 사람한테 맡기면 되겠네" 하고 알아서 불러요.</p>
</div>

subagent는 **YAML frontmatter가 있는 Markdown 파일**로 정의돼요. 가장 쉬운 시작은 Claude가 하나 생성하게 하는 거예요. 원하는 subagent와 저장할 위치를 설명해요.

```text
Create a code-reviewer subagent in .claude/agents/ that reviews code for bugs and readability. Make it read-only.
```

Claude가 subagent의 **이름 · 설명 · prompt**를 생성해요. 이 정보는 사용자가 준 prompt를 보고 **언제 그 subagent를 호출할지**도 Claude에게 알려 줘요.

### 저장 위치에 따른 적용 범위

| 위치 | 적용 범위 |
|---|---|
| `.claude/agents/` | **현재 프로젝트**에서 동작해요 |
| `~/.claude/agents/` | **내 모든 프로젝트**에서 동작해요 |

> 고른 폴더가 세션을 시작할 때 **없었다면**, 새 subagent를 로드하도록 Claude Code를 재시작하세요.

### 이전 버전 강의의 방법 — `/agents`

예전 노트는 아래 명령으로 만드는 흐름을 소개했어요. 현재 강의 본문은 위처럼 **prompt로 설명해서 만드는 방식**을 안내해요.

```text
/agents
```

그런 뒤 "Create new agent"를 선택해요. 에이전트의 범위 선택, 목적 정의, 접근 도구 선택, 색상 지정 같은 단계를 거쳐요.

## 3. 추가 커스터마이징

<div class="oa-eli">
  <span class="oa-eli__head">🎓 쉽게 말하면</span>
  <p>외주 전문가에게 <b>업무 수첩</b>(지난번 일을 기억)과 <b>교육 자료</b>(미리 익혀 올 매뉴얼)를 쥐여 주는 거예요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">Persistent memory</span>subagent가 <b>대화를 넘어</b> 메모리를 유지해요. 같은 프로젝트에 꾸준히 쓸 때 좋아요</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📦</span><span class="oa-card__title">Skill preload</span><code>skills</code> 키를 추가하고 skill 이름을 나열해 미리 로드해요</div>
</div>

skill preload에서 주의할 점이 하나 있어요.

| | skill이 context에 로드되는 방식 |
|---|---|
| **주 대화의 skill** | 필요할 때만 전체가 로드돼요 |
| **subagent에 preload한 skill** | **skill 전체**가 context에 로드돼요 |

> 예전 노트에는 키 이름이 `skill`로 적혀 있었어요. 현재 강의 본문의 표기는 **`skills`**예요.

더 깊이 알고 싶다면 전용 강의 *Introduction to subagents*를 참고하세요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. subagent가 탐색을 마치면 main agent에게 무엇이 돌아오나요?</summary>
  <p><b>정답: 발견한 내용의 요약.</b> 탐색 과정 전체는 subagent의 별도 context window에 남아서 주 context를 어지럽히지 않아요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. subagent를 내 모든 프로젝트에서 쓰려면 어디에 저장하나요?</summary>
  <p><b>정답: <code>~/.claude/agents/</code>.</b> <code>.claude/agents/</code>는 현재 프로젝트에서만 동작해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Claude가 생성한 subagent의 이름 · 설명 · prompt는 어떤 역할도 하나요?</summary>
  <p><b>정답: 언제 그 subagent를 호출할지 Claude에게 알려 줘요.</b> 사용자가 준 prompt를 보고 판단하는 기준이 돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. subagent에 skill을 preload할 때 주 대화의 skill과 다른 점은?</summary>
  <p><b>정답: skill 전체가 context에 로드돼요.</b> <code>skills</code> 키에 이름을 나열해 preload해요.</p>
</details>

## 정리

- context window를 깨끗하게 유지하는 것이 Claude Code에서 생산성을 지키는 최고의 방법 중 하나예요.
- subagent는 **격리된 자체 context window**에서 **병렬로** 일하고, 끝나면 **요약만** 돌려줘요.
- subagent는 YAML frontmatter가 있는 Markdown 파일이고, 원하는 것을 설명해 **Claude가 생성**하게 하는 게 가장 쉬워요.
- `.claude/agents/`는 현재 프로젝트, `~/.claude/agents/`는 모든 프로젝트에 적용돼요.
- **Persistent memory**와 **skill preload**(`skills` 키)로 더 다듬을 수 있어요. preload한 skill은 전체가 로드돼요.
