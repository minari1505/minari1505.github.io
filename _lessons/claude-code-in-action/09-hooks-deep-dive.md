---
title: "Hooks"
title_ko: "Hooks 심화 — 결정 JSON과 exit code"
course: claude-code-in-action
lesson: 9
---

## 학습 목표

- 자주 쓰는 hook 이벤트가 agentic loop의 어느 지점에서 실행되는지 말할 수 있다.
- PreToolUse hook이 JSON으로 결정을 돌려주는 방식과 `updatedInput`의 쓰임을 설명할 수 있다.
- exit code 0, 2, 그 밖의 값이 각각 어떻게 처리되는지 구분할 수 있다.

레슨 3의 hook 기초에 이어지는 내용입니다. 레슨 3과 설명이 다른 부분은 이 레슨(개편된 강의) 기준을 따릅니다.

## 한눈에 보는 이벤트

<ol class="oa-flow">
  <li><strong>SessionStart</strong>세션 시작, 환경 준비</li>
  <li><strong>PreToolUse</strong>도구 호출 전 — 막을 수 있음</li>
  <li><strong>PostToolUse</strong>도구 성공 후 — 포맷, lint</li>
  <li><strong>Stop</strong>턴을 끝내려 할 때 — 거부 가능</li>
  <li><strong>PreCompact / PostCompact</strong>compaction 전후</li>
</ol>

이 레슨을 관통하는 한 문장: **hook은 "Claude가 대체로 따른다"를 "Claude가 건너뛸 수 없다"로 바꿉니다.**

## 왜 hook인가

CLAUDE.md에 "수정 후 항상 포맷한다"라고 쓰는 것은 요청이지 보장이 아닙니다. 지켜보지 않는 긴 실행에서는 "대체로"가 부족합니다. hook은 loop의 정해진 지점에서 실행되는 **결정론적 코드**라서 동작을 보장합니다.

## 알아 둘 이벤트

Claude Code는 세션 동안 30개 안팎의 hook 이벤트를 냅니다. 자주 쓰는 것은 몇 개입니다.

| 이벤트 | 시점 | 주로 하는 일 |
|---|---|---|
| `PreToolUse` | 도구 호출 전 | 강제의 기본 수단. 일어나기 전에 막을 수 있음 |
| `PostToolUse` | 도구 호출 성공 후 | 자동 포맷, 자동 lint |
| `Stop` | Claude가 턴을 끝내려 할 때 | 조건이 안 맞으면 "아직 안 끝났다"고 거부. 서브에이전트용 `SubagentStop`도 있음 |
| `PreCompact` / `PostCompact` | compaction 전후 | compaction 전후 처리 |
| `InstructionsLoaded` | CLAUDE.md나 규칙 파일이 로드될 때 | 실제로 무엇이 context에 들어갔는지 감사 |
| `SessionStart` | 세션 시작 | 환경 준비. 새 시작에만 실행하려면 `startup` source 사용 |

**헷갈리기 쉬운 점**: compaction 뒤에 context를 다시 넣으려면 `PostCompact`가 아니라 `compact` matcher를 건 `SessionStart`를 씁니다. 출력이 실제로 대화에 다시 들어가는 것은 이쪽입니다.

## PreToolUse: JSON으로 결정 돌려주기

PreToolUse는 도구 호출을 실행 전에 막을 수 있습니다. JSON을 출력하고 exit 0으로 끝내면 Claude에게 결정을 전달합니다.

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "deny",
    "permissionDecisionReason": "...",
    "updatedInput": { "command": "..." }
  }
}
```

| `permissionDecision` | 의미 |
|---|---|
| `allow` | 호출을 통과시킴 |
| `deny` | 호출을 막음 |
| `ask` | 사용자에게 결정을 넘김 |
| `defer` | 비대화형 `-p` 실행에서 호출 프로세스가 도구를 멈췄다가 나중에 재개할 때만 쓰임. 드물게 사용 |

### 막는 대신 고쳐 쓰기: updatedInput

`updatedInput`을 돌려주면 호출을 막지 않고 **입력을 바꿔서** 실행시킬 수 있습니다.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">deny — 차단</span>
    <ul>
      <li>비밀 키가 든 명령을 통째로 거부</li>
      <li>작업이 멈춥니다.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">updatedInput — 가리고 실행</span>
    <ul>
      <li><code>sk_live_</code> 패턴을 찾아 자리표시자로 바꿈</li>
      <li>명령은 실행되고, 비밀 값은 통과하지 못합니다.</li>
    </ul>
  </div>
</div>

주의할 점: `updatedInput`은 **입력 객체 전체를 대체**합니다. 바꾸지 않는 필드도 그대로 다시 적어야 사라지지 않습니다.

## exit code 규칙

JSON을 쓰지 않는 단순한 hook은 exit code로 충분합니다.

| exit code | 의미 | 처리 |
|---|---|---|
| `0` | 성공 | stdout이 JSON이면 파싱. 일반 텍스트는 대부분 무시되지만 `SessionStart`, `UserPromptSubmit`, `UserPromptExpansion`에서는 context에 추가됨 |
| `2` | 차단 오류 | stderr가 Claude에게 context로 전달됨. 거의 모든 곳에서 차단용 코드 |
| 그 밖 | 비차단 | stderr는 로그에 남고 Claude는 계속 진행 |

<div class="oa-eli">
  <span class="oa-eli__head">⚠️ 가장 흔한 실수: exit 1</span>
  <p>exit code <b>1</b>은 오류처럼 느껴지지만 <b>막지 않습니다</b>. Claude는 명령을 그대로 실행합니다. 무언가를 멈추려 했다면 1이 아니라 <b>2</b>로 끝내야 합니다.</p>
</div>

몇 가지 예외가 있습니다.

- exit 2는 `Stop`도 막을 수 있습니다. Claude에게 아직 안 끝났다고 알리는 방법입니다.
- `PostToolUse`는 도구가 이미 실행된 뒤라, 막아도 호출 자체는 되돌릴 수 없습니다. 다만 텍스트를 Claude에게 전달할 수는 있습니다.
- `Notification`과 `SessionStart`는 차단을 무시하고 계속 진행합니다.

## compaction을 넘어 상태 지키기

긴 대화를 compact하면 세부 내용이 많이 빠집니다. `compact` matcher를 건 `SessionStart` hook이 compaction 직후 실행되어, 작업 중이던 파일의 짧은 요약을 출력하게 합니다. 이 출력이 context에 다시 들어가서 Claude가 이어서 작업합니다.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 위험한 명령을 막으려고 hook에서 <code>exit 1</code>을 했는데 명령이 실행됐습니다. 왜 그렇습니까?</summary>
  <p><b>정답: exit 1은 비차단입니다.</b> stderr만 로그에 남고 Claude는 계속 진행합니다. 막으려면 exit 2를 씁니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. compaction 뒤에 "작업 중이던 파일 목록"을 context에 다시 넣으려 합니다. 어떤 hook을 씁니까?</summary>
  <p><b>정답: <code>compact</code> matcher를 건 <code>SessionStart</code>.</b> <code>PostCompact</code>가 아닙니다. 출력이 대화에 다시 들어가는 것은 SessionStart 쪽입니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. bash 명령에서 비밀 키만 가리고 명령은 실행시키고 싶습니다. PreToolUse hook에서 무엇을 돌려줍니까?</summary>
  <p><b>정답: <code>updatedInput</code>.</b> 입력을 고쳐 써서 실행시킵니다. 입력 객체 전체를 대체하므로 바꾸지 않는 필드도 다시 적어야 합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 테스트가 실패하면 Claude가 턴을 끝내지 못하게 하려면 어떤 이벤트를 씁니까?</summary>
  <p><b>정답: <code>Stop</code>.</b> hook이 exit 2로 끝나면 턴 종료를 거부하고 실패 내용이 Claude에게 전달됩니다.</p>
</details>

## 핵심 정리

- hook은 정해진 지점에서 실행되는 코드라 동작을 보장합니다.
- PreToolUse는 `permissionDecision`(allow, deny, ask)으로 결정을 돌려주고, `updatedInput`으로 입력을 고쳐 쓸 수 있습니다.
- 막으려면 exit 2입니다. exit 1은 막지 않습니다.
- compaction 뒤 상태 복원은 `compact` matcher를 건 `SessionStart`로 합니다.
