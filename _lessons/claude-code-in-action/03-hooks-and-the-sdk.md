---
title: "Hooks and the SDK"
title_ko: "Hooks와 SDK"
course: claude-code-in-action
lesson: 3
---

## 학습 목표

- Claude Code hook이 tool call 전후에 어떻게 실행되는지 이해하기
- `PreToolUse`와 `PostToolUse` hook의 차이와 설정 방법 익히기
- 민감 파일 접근 차단, type check, query 중복 검토 같은 실전 hook 패턴 정리하기
- hook 입력 JSON 구조와 exit code 규칙을 이해하기
- Agent SDK로 Claude Code의 agent loop를 스크립트에서 실행하는 방법 파악하기

## Hook이란?

Hook은 Claude Code가 tool을 실행하기 **직전** 또는 **직후**에 내가 지정한 command를 자동으로 실행하는 기능입니다.

기본 흐름은 다음과 같습니다.

```text
사용자 요청 → Claude가 tool call 결정 → Claude Code가 tool 실행 → 결과를 Claude에게 반환
```

Hook을 추가하면 이 흐름 사이에 내 검증 로직이나 자동화 스크립트를 끼워 넣을 수 있습니다.

```text
사용자 요청
  ↓
Claude가 tool call 결정
  ↓
PreToolUse hook 실행
  ↓
Claude Code가 tool 실행
  ↓
PostToolUse hook 실행
  ↓
결과를 Claude에게 반환
```

대표적인 용도는 다음과 같습니다.

- 파일 수정 후 formatter 실행
- 파일 변경 후 test, linter, type checker 실행
- `.env` 같은 민감 파일 접근 차단
- Claude가 수정한 파일 목록 로깅
- 특정 디렉터리에 대한 코드 품질 검증
- naming convention, architecture rule 검사

핵심은 `PreToolUse` hook은 실행 전에 개입해 tool call을 막을 수 있고, `PostToolUse` hook은 실행 이후에 후속 작업과 피드백을 줄 수 있다는 점입니다.

## Hook 설정 위치

Hook은 Claude Code settings 파일에 정의합니다.

| 위치 | 용도 |
|---|---|
| `~/.claude/settings.json` | 모든 프로젝트에 적용되는 global 설정 |
| `.claude/settings.json` | 프로젝트에서 팀과 공유하는 설정 |
| `.claude/settings.local.json` | 개인 로컬 설정. 보통 git에 커밋하지 않음 |

직접 JSON을 편집할 수도 있고, Claude Code 안에서 `/hooks` 명령을 사용해 설정할 수도 있습니다.

## PreToolUse와 PostToolUse

### PreToolUse

`PreToolUse` hook은 tool이 실행되기 전에 실행됩니다. matcher로 어떤 tool을 감시할지 지정합니다.

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Read",
        "hooks": [
          {
            "type": "command",
            "command": "node /absolute/path/to/hooks/read_hook.js"
          }
        ]
      }
    ]
  }
}
```

이 설정은 Claude가 `Read` tool을 호출하려고 할 때마다 `read_hook.js`를 먼저 실행합니다. hook command는 표준 입력으로 tool call 정보를 받고, 실행을 허용할지 차단할지 결정합니다.

### PostToolUse

`PostToolUse` hook은 tool이 이미 실행된 뒤에 실행됩니다.

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": "node /absolute/path/to/hooks/edit_hook.js"
          }
        ]
      }
    ]
  }
}
```

이미 tool call이 끝난 뒤이므로 `PostToolUse` hook은 해당 작업을 막을 수는 없습니다. 대신 다음 일을 할 수 있습니다.

- 방금 수정된 파일 format
- type check나 test 실행
- 결과를 Claude에게 feedback으로 전달
- 변경 내용을 별도 로그로 기록

## Hook 입력 데이터

Hook command는 표준 입력으로 JSON 데이터를 받습니다. 예를 들어 `Read` tool에 대한 `PreToolUse` hook은 다음과 같은 데이터를 받을 수 있습니다.

```json
{
  "session_id": "2d6a1e4d-6...",
  "transcript_path": "/Users/sg/...",
  "hook_event_name": "PreToolUse",
  "tool_name": "Read",
  "tool_input": {
    "file_path": "/code/queries/.env"
  }
}
```

command는 이 JSON을 파싱하고, `tool_name`과 `tool_input`을 보고 허용 여부를 결정합니다.

Claude Code에서 어떤 tool들이 있는지는 Claude에게 직접 물어볼 수 있습니다. MCP server를 추가하면 tool 목록이 달라질 수 있으므로, 현재 환경에서 확인하는 것이 안전합니다.

## Exit code 규칙

Hook command는 exit code로 Claude Code에 결과를 전달합니다.

| Exit code | 의미 |
|---|---|
| `0` | 문제 없음. tool call 진행 |
| `2` | tool call 차단. `PreToolUse` hook에서만 의미 있음 |

`PreToolUse` hook에서 exit code `2`로 종료하면 tool call은 실행되지 않습니다. 이때 `stderr`에 쓴 메시지는 Claude에게 feedback으로 전달됩니다.

## 예제: `.env` 읽기 차단

민감한 `.env` 파일을 Claude가 읽지 못하게 막는 hook을 만들어봅니다. 이 예제는 현재 강의 기준으로 `Read` tool만 막습니다. `Grep`이나 `Bash`는 입력 구조가 다르므로 별도 matcher와 검사가 필요합니다.

`.claude/settings.local.json`에 다음 설정을 추가합니다.

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Read",
        "hooks": [
          {
            "type": "command",
            "command": "node $PWD/hooks/read_hook.js"
          }
        ]
      }
    ]
  }
}
```

그리고 `hooks/read_hook.js`를 만듭니다.

```js
process.stdin.setEncoding("utf8");

let input = "";

process.stdin.on("data", (d) => {
  input += d;
});

process.stdin.on("end", () => {
  const toolArgs = JSON.parse(input);
  const readPath = toolArgs.tool_input?.file_path || "";

  if (readPath.includes(".env")) {
    console.error("You cannot read the .env file");
    process.exit(2);
  }

  process.exit(0);
});
```

동작 방식은 단순합니다.

1. stdin으로 들어온 JSON을 읽음
2. `tool_input.file_path`를 확인
3. 경로에 `.env`가 포함되어 있으면 `stderr`에 이유를 쓰고 `process.exit(2)`
4. 아니면 `process.exit(0)`으로 허용

Claude Code 세션에서 `.env` 파일을 읽어보라고 요청하면 다음 메시지가 Claude에게 전달됩니다.

```text
You cannot read the .env file
```

다른 파일은 정상적으로 읽을 수 있습니다.

### 왜 Read만 막는가?

각 tool은 서로 다른 입력 구조를 가집니다.

| Tool | 입력 예 |
|---|---|
| `Read` | `{ "file_path": "..." }` |
| `Grep` | `{ "pattern": "...", "path": "..." }` |
| `Bash` | `{ "command": "..." }` |

위 hook은 `file_path`만 검사하므로 `Read`에는 동작하지만, `grep API_KEY`나 `cat .env` 같은 Bash command는 막지 못합니다. 더 강한 보호가 필요하면 tool별 hook을 따로 작성하거나 `permissions.deny` 규칙을 함께 사용합니다.

예를 들어 다음처럼 deny rule을 병행할 수 있습니다.

```json
{
  "permissions": {
    "deny": ["Read(**/.env)"]
  }
}
```

## Hook 설정의 경로 문제

Claude Code hook 보안 권장사항 중 하나는 script 경로에 상대 경로보다 **절대 경로**를 쓰는 것입니다. 상대 경로는 path interception이나 binary planting 공격에 취약할 수 있기 때문입니다.

하지만 절대 경로는 팀과 공유하기 어렵습니다. 내 프로젝트 경로와 다른 사람의 프로젝트 경로가 다르기 때문입니다.

강의 실습 프로젝트는 이를 해결하기 위해 `settings.example.json`에 `$PWD` placeholder를 둡니다. `npm run setup`을 실행하면 `scripts/init-claude.js`가 실행되고 다음 일을 합니다.

1. `settings.example.json` 안의 `$PWD`를 현재 프로젝트의 절대 경로로 치환
2. 결과를 `.claude/settings.local.json`으로 복사
3. 각 로컬 환경에서 절대 경로를 쓰는 hook 설정 생성

이 방식은 설정 파일을 공유하면서도 실제 실행은 보안 권장사항에 맞는 절대 경로를 사용하게 해줍니다.

## 다른 hook 종류와 입력 구조 확인

이번 강의에서 주로 다룬 hook은 `PreToolUse`와 `PostToolUse`지만, Claude Code에는 다른 hook도 있습니다.

- `Notification`: Claude Code가 알림을 보낼 때 실행
- `Stop`: Claude Code가 응답을 끝냈을 때 실행
- `SubagentStop`: subagent 작업이 끝났을 때 실행
- `PreCompact`: 수동 또는 자동 compact 직전에 실행
- `UserPromptSubmit`: 사용자가 prompt를 제출한 뒤 Claude가 처리하기 전에 실행
- `SessionStart`: session 시작 또는 재개 시 실행
- `SessionEnd`: session 종료 시 실행

주의할 점은 hook 종류와 matcher에 따라 stdin 입력 구조가 크게 달라진다는 것입니다.

예를 들어 `TodoWrite` tool을 감시하는 `PostToolUse` hook은 `tool_input`과 `tool_response`를 받을 수 있습니다.

```json
{
  "session_id": "9ecf22fa-edf8-4332-ae85-b6d5456eda64",
  "transcript_path": "<path_to_transcript>",
  "hook_event_name": "PostToolUse",
  "tool_name": "TodoWrite",
  "tool_input": {
    "todos": [
      { "content": "write a readme", "status": "pending", "id": "1" }
    ]
  },
  "tool_response": {
    "oldTodos": [],
    "newTodos": [
      { "content": "write a readme", "status": "pending", "id": "1" }
    ]
  }
}
```

반면 `Stop` hook은 tool 정보 없이 다음처럼 들어올 수 있습니다.

```json
{
  "session_id": "af9f50b6-f042-4773-b3e2-c3a4814765ce",
  "transcript_path": "<path_to_transcript>",
  "hook_event_name": "Stop",
  "stop_hook_active": false
}
```

입력 구조를 정확히 모를 때는 임시 logging hook을 만드는 것이 좋습니다.

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "jq . > post-log.json"
          }
        ]
      }
    ]
  }
}
```

이렇게 하면 실제 hook에 들어오는 JSON을 `post-log.json`으로 확인할 수 있습니다. hook을 작성할 때 가장 먼저 해볼 만한 디버깅 방법입니다.

## 유용한 hook 패턴

### TypeScript type check hook

Claude가 함수 signature를 바꿀 때, 정의는 수정하지만 모든 call site를 놓칠 수 있습니다. 예를 들어 `schema.ts`의 함수에 `verbose` parameter를 추가했지만 `main.ts`의 호출부를 고치지 않으면 type error가 생깁니다.

이를 막기 위해 파일 편집 후 TypeScript compiler를 실행하는 `PostToolUse` hook을 만들 수 있습니다.

- `Write`, `Edit`, `MultiEdit` 이후 `tsc --noEmit` 실행
- type error가 있으면 stderr/stdout을 Claude에게 feedback으로 전달
- Claude가 다른 파일의 호출부까지 수정하도록 유도

typed language라면 type checker를, untyped language라면 test나 linter를 비슷한 방식으로 사용할 수 있습니다.

### Query 중복 방지 hook

큰 프로젝트에서는 Claude가 기존 query를 재사용하지 않고 비슷한 SQL 함수를 새로 만들 수 있습니다.

예를 들어 이미 `getPendingOrders()`가 있는데, "3일 이상 pending인 주문을 Slack으로 알리는 integration을 만들어줘"라고 요청하면 Claude가 새 query를 작성할 수 있습니다.

이를 막는 hook은 다음처럼 동작합니다.

1. Claude가 `./queries` 디렉터리 파일을 수정
2. hook이 별도 Claude Code instance를 Agent SDK로 실행
3. 두 번째 Claude가 변경 내용을 review하고 기존 query와 중복되는지 확인
4. 중복이 있으면 원래 Claude에게 feedback 제공
5. 원래 Claude가 새 query를 제거하고 기존 함수를 재사용

이 패턴은 강력하지만 비용이 있습니다. 별도 Claude instance를 매번 띄우므로 시간과 API 사용량이 증가합니다. 따라서 모든 디렉터리가 아니라, 중복이 특히 문제가 되는 핵심 디렉터리에만 적용하는 편이 좋습니다.

## Agent SDK

Agent SDK는 Claude Code의 agent loop를 내 애플리케이션이나 스크립트에서 직접 실행할 수 있게 해줍니다. CLI에서 쓰는 파일 읽기, 편집, tool use 흐름을 코드로 제어할 수 있습니다.

TypeScript와 Python에서 사용할 수 있으며, 여기서는 TypeScript 예시를 정리합니다.

### 설치

새 디렉터리를 만들고 SDK package를 설치합니다.

```bash
mkdir sdk-demo
cd sdk-demo
npm init -y
npm install @anthropic-ai/claude-agent-sdk
```

현재 package 이름은 `@anthropic-ai/claude-agent-sdk`입니다. 이름이 비슷한 `@anthropic-ai/claude-code`는 CLI 자체라 import해서 쓰는 SDK가 아닙니다.

### 최소 예제

`index.mjs` 파일을 만들고 다음 코드를 넣습니다.

```js
import { query } from "@anthropic-ai/claude-agent-sdk";

const prompt = "List the files in the current directory";

for await (const message of query({ prompt })) {
  console.log(JSON.stringify(message, null, 2));
}
```

실행합니다.

```bash
node index.mjs
```

그러면 CLI에서 보던 대화 event가 JSON stream으로 출력됩니다. 여기에는 tool call, tool result, Claude의 text message가 포함됩니다.

### Tool 제한

기본적으로 SDK는 전체 tool set에 접근할 수 있습니다. 특정 tool만 허용하려면 `allowedTools`를 넘깁니다.

```js
import { query } from "@anthropic-ai/claude-agent-sdk";

const prompt = "List the markdown files in this project";

for await (const message of query({
  prompt,
  options: {
    allowedTools: ["Read", "Glob"],
  },
})) {
  console.log(JSON.stringify(message, null, 2));
}
```

이는 CLI의 `--allowedTools` flag에 해당합니다.

Agent SDK는 custom system prompt, MCP server, hooks, subagent, session resumption 같은 Claude Code 기능도 지원합니다. 따라서 hook 안에서 별도 Claude instance를 실행하거나, 프로젝트 전용 자동화 script를 만드는 기반으로 사용할 수 있습니다.

## 정리

- Hook은 Claude Code의 tool call 전후에 내 command를 실행하는 확장 지점입니다.
- `PreToolUse`는 실행 전 hook이라 tool call을 차단할 수 있고, `PostToolUse`는 실행 후 hook이라 formatting, testing, feedback에 적합합니다.
- hook command는 stdin으로 JSON을 받고, exit code `0` 또는 `2`로 Claude Code에 결과를 전달합니다.
- `.env` 차단 예시는 `Read` tool의 `tool_input.file_path`를 검사하지만, Grep/Bash까지 막으려면 각 tool의 입력 구조를 따로 처리하거나 permission rule을 병행해야 합니다.
- hook 입력 구조는 hook 종류와 tool마다 다르므로, `jq . > post-log.json` 같은 logging hook으로 실제 payload를 먼저 확인하는 것이 좋습니다.
- 유용한 hook으로는 type checker 실행, query 중복 검토, formatter/test/lint feedback 등이 있습니다.
- Agent SDK는 Claude Code를 CLI 밖에서 programmatically 실행하게 해주며, `@anthropic-ai/claude-agent-sdk` package를 사용합니다.
