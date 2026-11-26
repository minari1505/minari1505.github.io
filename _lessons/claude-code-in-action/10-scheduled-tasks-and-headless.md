---
title: "Scheduled tasks and headless"
title_ko: "예약 작업과 headless 모드"
course: claude-code-in-action
lesson: 10
---

## 학습 목표

- 반복 작업을 넘기는 네 가지 방법(cloud scheduled task, headless, `--bare`, Agent SDK)을 상황에 맞게 고를 수 있다.
- `-p`, `--output-format json`, `--json-schema`, `--resume`으로 스크립트 자동화를 구성할 수 있다.
- cloud scheduled task에 의존하기 전에 알아야 할 제약 세 가지를 말할 수 있다.

## 한눈에 보는 스펙트럼

<ol class="oa-flow">
  <li><strong>Cloud scheduled task</strong>만들 것이 없음. Anthropic 인프라에서 실행</li>
  <li><strong>Headless (-p)</strong>내 파이프라인에서 실행</li>
  <li><strong>--bare</strong>모든 CI 머신에서 같은 설정</li>
  <li><strong>Agent SDK</strong>내 제품 안에 내장</li>
</ol>

왼쪽에서 시작하고, 통제가 더 필요할 때만 오른쪽으로 내려갑니다.

<div class="oa-eli">
  <span class="oa-eli__head">🧺 쉽게 말하면</span>
  <p>빨래를 맡기는 방법과 같습니다. <b>세탁 서비스</b>(cloud scheduled task)는 맡기기만 하면 됩니다. <b>집 세탁기</b>(headless)는 내가 돌리지만 원하는 대로 설정할 수 있습니다. <b>세탁소를 직접 차리는 것</b>(Agent SDK)은 내 사업에 세탁이 들어갈 때만 필요합니다.</p>
</div>

## Cloud scheduled task

예전에 routine이라고 부르던 기능입니다. 스크립트도 서버도 없이, 세 가지를 묶어 클라우드에서 실행합니다.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">프롬프트</span>매 세션에서 할 일</div>
  <div class="oa-card"><span class="oa-card__icon">📁</span><span class="oa-card__title">저장소</span>작업 대상</div>
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">커넥터</span>필요한 외부 연결</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">⏰</span><span class="oa-card__title">트리거</span>cron, HTTP POST, GitHub 이벤트</div>
</div>

맞는 일의 예: 아침 의존성 점검, 새 PR이 올라올 때 실행되는 PR 분류, Sentry 티켓 일일 점검. **같은 프롬프트가 반복 트리거로 실행되는 일**이면 잘 맞습니다.

만드는 방법은 두 가지입니다. 웹(`claude.ai/code/routines`)에서 만들거나, 터미널에서 `/schedule`로 자연어로 설명합니다.

```text
/schedule daily dependency audit at 9am
```

### 의존하기 전에 알아 둘 세 가지

| 제약 | 내용 |
|---|---|
| 리서치 프리뷰 | 동작과 한도가 계속 바뀔 수 있습니다 |
| 주기 | 반복 일정은 **최대 1시간에 한 번**입니다. 더 자주 필요하면 맞지 않습니다 |
| 브랜치 | 실행은 기본 브랜치의 새 clone에서 시작해 `claude/` 브랜치로 push합니다(프롬프트에서 다른 브랜치를 지정하지 않는 한). PR 이벤트로 실행되면 그 PR의 head 커밋에서 시작합니다 |

push 가능한 브랜치를 제한하려면 GitHub의 branch protection rule이나 ruleset을 씁니다. 연결한 GitHub 접근 권한이 우회할 수 있는 규칙은 push를 막지 못합니다.

## Headless 모드: 내 환경이 필요할 때

작업에 내 환경이나, 실행을 감싸는 로직이 필요하면 headless 모드(문서에서는 non-interactive 모드)로 내려갑니다. 핵심은 `-p`(`--print`) 플래그입니다. 대화형 UI 없이 한 번 실행하고, stdin을 읽고 stdout에 쓰므로 다른 셸 도구처럼 파이프로 연결됩니다.

```bash
claude -p "summarize the changes in this diff"
```

`-p`만 쓰면 대화형 세션과 **같은 context**(hook, skill, plugin, MCP 서버, CLAUDE.md)를 로드합니다.

### 구조화 출력 받기

```bash
claude -p "Extract the exported function names from src/core/style.js" \
  --output-format json \
  --json-schema '{"type":"object","properties":{"functions":{"type":"array","items":{"type":"string"}}},"required":["functions"]}' \
  | jq '.structured_output.functions'
```

스키마에 맞는 객체는 JSON 응답의 `structured_output` 필드에 들어옵니다. `jq`로 꺼내 다음 스크립트나 데이터베이스로 넘깁니다.

### 여러 단계 자동화: 세션 이어가기

JSON 출력에서 세션 ID를 저장해 두었다가 나중에 재개합니다. 첫 실행이 계획을 만들고 두 번째 실행이 수행하는 구성에 맞습니다.

```bash
claude --resume "$(jq -r .session_id /tmp/plan.json)"
```

### CI에서 일관된 실행: --bare

`--bare`는 hook, skill, plugin, MCP 서버, CLAUDE.md의 **자동 탐색을 건너뜁니다.** Bash, 파일 읽기, 파일 수정 도구와 `--settings`, `--mcp-config` 같은 플래그로 넘긴 것만 가집니다.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">-p만</span>
    <ul>
      <li>대화형과 같은 context를 로드</li>
      <li>머신마다 설정이 다르면 결과도 달라질 수 있음</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">-p --bare</span>
    <ul>
      <li>모든 머신이 같은 설정에서 시작, 시작도 빠름</li>
      <li>구독 로그인을 쓰지 않으므로 <code>ANTHROPIC_API_KEY</code>를 먼저 설정</li>
    </ul>
  </div>
</div>

## Agent SDK: 내 앱 안의 Claude Code

Claude Code를 TypeScript나 Python 애플리케이션에 내장하는 라이브러리입니다. 두 언어 모두 `query` 함수와 CLI와 같은 기본 요소를 제공합니다. 프롬프트와 함께 `allowedTools`, system prompt, permission mode 같은 옵션을 넘기고, Claude가 스트리밍하는 메시지를 순회하며 처리합니다. CLI와 같은 엔진을 제품 안에서 호출하는 것입니다.

## 무엇을 고를까

| 상황 | 선택 |
|---|---|
| 반복 작업의 기본값. 호스팅할 것이 없기를 원함 | Cloud scheduled task |
| 작업이 내 파이프라인을 필요로 하고 스크립트로 데이터를 넘김 | Headless `-p` |
| CI의 모든 머신에서 같은 설정이 필요함 | `--bare` |
| 작업이 내 제품 안에 들어가야 함 | Agent SDK |

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. <code>--json-schema</code>로 받은 결과는 JSON 응답의 어느 필드에 들어 있습니까?</summary>
  <p><b>정답: <code>structured_output</code>.</b> <code>--output-format json</code>과 함께 쓰고 <code>jq</code>로 꺼냅니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. CI 머신마다 Claude Code의 동작이 조금씩 다릅니다. 무엇으로 맞춥니까?</summary>
  <p><b>정답: <code>--bare</code>.</b> hook, skill, plugin, MCP 서버, CLAUDE.md의 자동 탐색을 건너뛰어 모든 머신이 같은 설정에서 시작합니다. <code>ANTHROPIC_API_KEY</code>가 필요합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 10분마다 실행해야 하는 작업을 cloud scheduled task로 만들 수 있습니까?</summary>
  <p><b>정답: 없습니다.</b> 반복 일정은 최대 1시간에 한 번입니다. 더 자주 필요하면 headless 모드를 내 스케줄러에서 돌립니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 첫 스크립트가 계획을 만들고 다음 스크립트가 그 맥락을 이어받아 실행하게 하려면 어떻게 합니까?</summary>
  <p><b>정답: 첫 실행의 JSON 출력에서 <code>session_id</code>를 저장하고 <code>--resume</code>으로 재개합니다.</b></p>
</details>

## 핵심 정리

- 반복 작업은 cloud scheduled task에서 시작하고, 통제가 더 필요할 때만 headless, `--bare`, Agent SDK로 내려갑니다.
- `-p`는 대화형과 같은 context를 로드하고, `--bare`는 자동 탐색을 건너뜁니다.
- 구조화 출력은 `--output-format json` + `--json-schema`, 결과는 `structured_output` 필드입니다.
- cloud scheduled task는 리서치 프리뷰이고, 최대 시간당 1회이며, 기본적으로 `claude/` 브랜치로 push합니다.
