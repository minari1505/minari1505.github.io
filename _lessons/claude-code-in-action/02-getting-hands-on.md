---
title: "Getting hands on"
title_ko: "직접 실습하기"
course: claude-code-in-action
lesson: 2
---

## 학습 목표

- 실습용 프로젝트를 준비하고 Claude Code로 열어보는 흐름 이해하기
- `CLAUDE.md`, `/init`, 파일 mention으로 Claude에게 올바른 context를 주는 방법 익히기
- screenshot, planning mode, `/effort`로 변경 작업을 더 정확하게 진행하기
- `/compact`, `/clear`, `/rewind`, `/memory`로 긴 대화의 context를 관리하기
- custom commands, MCP server, GitHub integration으로 Claude Code를 확장하는 방법 정리하기

원문 레슨: [Claude Code in Action](https://anthropic.skilljar.com/claude-code-in-action/301615)

## 왜 실습 프로젝트가 필요한가?

Claude Code는 실제 코드베이스가 있을 때 훨씬 흥미롭게 배울 수 있습니다. 파일을 읽고, 프로젝트 구조를 파악하고, 명령을 실행하고, 코드를 수정하는 과정을 직접 볼 수 있기 때문입니다.

강의에서는 작은 **UI generation app**을 실습 프로젝트로 사용합니다. 이전 영상에서 보여준 것과 같은 앱이며, Claude Code가 코드베이스를 탐색하고 변경하는 흐름을 연습하기에 적당한 크기입니다.

다만 이 프로젝트를 반드시 실행해야 하는 것은 아닙니다. 이미 작업 중인 코드베이스가 있다면 그 프로젝트로 따라가도 됩니다. 핵심은 Claude Code가 실제 파일과 명령을 다룰 수 있는 환경을 준비하는 것입니다.

## 실습 프로젝트 준비

프로젝트 실행에는 약간의 setup이 필요합니다.

1. 로컬에 **Node.js**가 설치되어 있는지 확인합니다. 설치가 필요하면 [Node.js download page](https://nodejs.org/en/download)를 참고합니다.
2. 강의에 첨부된 `uigen.zip` 파일을 다운로드하고 압축을 풉니다.
3. 프로젝트 디렉터리로 이동합니다.
4. 다음 명령으로 의존성을 설치하고 로컬 SQLite 데이터베이스를 준비합니다.

```bash
npm run setup
```

이 단계가 끝나면 프로젝트의 dependency와 로컬 개발용 데이터베이스가 준비됩니다.

### Claude Code 실행

프로젝트가 준비되면 프로젝트 루트에서 Claude Code를 실행합니다.

```bash
claude
```

처음에는 바로 수정을 요청하기보다, Claude Code가 프로젝트를 이해하게 하는 질문부터 시작하는 편이 좋습니다.

```text
이 프로젝트 구조를 먼저 파악해줘.
package.json과 주요 source file을 읽고 실행 방법을 정리해줘.
아직 코드는 수정하지 말고, 앱이 어떤 구조인지 설명해줘.
```

이런 식으로 context를 먼저 모으게 하면 Claude Code가 파일 구조, npm script, 데이터베이스 setup, API key 사용 위치를 파악한 뒤 더 안정적으로 작업할 수 있습니다.

## Anthropic API key 설정

이 프로젝트는 Anthropic API를 통해 Claude를 호출해 UI component를 생성할 수 있습니다. 앱의 실제 생성 기능까지 테스트하려면 Anthropic API key가 필요합니다.

API key 설정은 선택 사항입니다. key를 넣지 않아도 앱은 정적인 fake code를 생성하면서 기본 흐름을 확인할 수 있습니다.

실제 API 호출까지 테스트하려면 다음 순서로 설정합니다.

1. [Anthropic Console](https://console.anthropic.com/)에서 API key를 발급받습니다.
2. 프로젝트의 `.env` 파일을 엽니다.
3. 파일 안의 `your-api-key-here`라는 텍스트를 발급받은 key로 교체합니다.

```text
ANTHROPIC_API_KEY=your-api-key-here
```

실제 key는 공개 저장소에 커밋하면 안 됩니다. `.env` 파일이 `.gitignore`에 포함되어 있는지 확인하고, 코드 블록의 `your-api-key-here` placeholder만 문서나 예시에 남겨야 합니다.

## 프로젝트 실행

setup이 끝나면 다음 명령으로 개발 서버를 시작합니다.

```bash
npm run dev
```

이후 브라우저에서 로컬 개발 서버를 열어 앱을 확인할 수 있습니다. API key가 설정되어 있으면 Claude를 통한 UI 생성 기능을 테스트할 수 있고, 설정하지 않았다면 static fake code 생성 흐름으로 앱 구조를 살펴볼 수 있습니다.

## Context 추가하기

Claude Code를 잘 쓰려면 "많은 context"가 아니라 **맞는 context**를 주는 것이 중요합니다. 프로젝트에 파일이 수십, 수백 개 있더라도 Claude가 지금 작업에 필요한 파일과 규칙을 빠르게 찾을 수 있어야 합니다. 관련 없는 context가 너무 많으면 오히려 성능이 떨어질 수 있습니다.

### `/init`과 `CLAUDE.md`

새 프로젝트에서 가장 먼저 실행할 만한 명령은 `/init`입니다.

```text
/init
```

이 명령은 Claude가 코드베이스를 분석해 다음 정보를 파악하게 합니다.

- 프로젝트 목적과 아키텍처
- 중요한 명령과 핵심 파일
- 코드 패턴과 구조

분석이 끝나면 Claude는 요약을 `CLAUDE.md` 파일에 작성합니다. 이 파일은 프로젝트에 대한 persistent system prompt처럼 동작합니다. Claude가 이후 요청마다 프로젝트 규칙, 명령, 아키텍처 정보를 참고할 수 있게 해줍니다.

`CLAUDE.md`는 보통 다음 세 위치에서 사용됩니다.

| 파일 | 용도 |
|---|---|
| `CLAUDE.md` | `/init`으로 생성. source control에 포함해 팀과 공유 |
| `CLAUDE.local.md` | 개인 설정. 다른 엔지니어와 공유하지 않음 |
| `~/.claude/CLAUDE.md` | 내 머신의 모든 프로젝트에 적용할 공통 지침 |

Claude가 코드를 너무 많이 주석 처리한다면 `CLAUDE.md`에 다음처럼 적을 수 있습니다.

```text
Use comments sparingly. Only comment complex code.
```

예전 강의 영상에는 `#` shortcut으로 memory를 추가하는 방식이 나올 수 있지만, 현재는 `/memory`를 사용하거나 `CLAUDE.md`를 직접 수정하는 방식이 맞습니다.

### 파일 mention: `@`

특정 파일을 Claude에게 확실히 보여주고 싶다면 `@`를 사용합니다.

```text
How does the auth system work? @auth
```

Claude Code는 관련 파일 후보를 보여주고, 선택한 파일 내용을 대화 context에 포함합니다. 이미 중요한 파일을 알고 있다면 `@src/lib/prompts/generation.tsx`처럼 직접 경로를 지정하는 편이 빠릅니다.

`CLAUDE.md` 안에서도 `@` mention을 사용할 수 있습니다. 예를 들어 데이터 구조를 자주 참조해야 한다면:

```text
The database schema is defined in the @prisma/schema.prisma file.
Reference it anytime you need to understand the structure of data stored in the database.
```

이렇게 하면 Claude가 매번 스키마 파일을 새로 찾지 않아도 됩니다. 이미 `AGENTS.md`를 쓰는 repo라면 `CLAUDE.md` 첫 줄에 `@AGENTS.md`를 추가해 기존 지침을 재사용할 수도 있습니다.

## 변경 작업하기

Claude Code로 실제 변경을 만들 때는 텍스트만으로 설명하는 것보다 더 정확한 방법들이 있습니다.

### Screenshot으로 정확히 지시하기

UI의 특정 영역을 고치고 싶을 때는 screenshot이 매우 효과적입니다. 화면을 붙여 넣으면 Claude가 "어느 부분을 말하는지"를 훨씬 정확히 이해합니다.

Claude Code에 screenshot을 붙여 넣을 때는 macOS에서도 `Cmd+V`가 아니라 **`Ctrl+V`**를 사용합니다. 붙여 넣은 뒤 다음처럼 요청할 수 있습니다.

```text
이 screenshot에서 카드 제목과 버튼 간격이 너무 좁아 보여.
해당 영역의 spacing을 조정해줘.
```

### Planning Mode

여러 파일을 읽고 넓게 조사해야 하는 작업은 Planning Mode가 좋습니다.

```text
/plan
```

또는 `Shift + Tab`을 두 번 누르면 Planning Mode를 켤 수 있습니다. 이미 auto-accept edits 상태라면 한 번으로 전환될 수 있습니다.

Planning Mode에서 Claude는 바로 코드를 수정하지 않고 다음을 먼저 수행합니다.

- 프로젝트 파일을 더 넓게 읽음
- 구현 계획을 자세히 작성
- 어떤 파일을 어떻게 바꿀지 보여줌
- 사용자 승인을 기다림

계획을 검토할 때 `Ctrl+G`를 누르면 text editor에서 계획을 열 수 있습니다. 필요한 부분을 직접 고쳐 제출하면 Claude는 수정된 계획을 기준으로 구현합니다.

### `/effort`와 `ultrathink`

강의 영상에는 오래된 `"think harder"`류 keyword가 나올 수 있지만, 현재는 효과가 없습니다. 세션의 reasoning 강도를 조절하려면 `/effort`를 사용합니다.

```text
/effort
```

`low`는 더 빠르고 저렴하며, `max`는 어려운 문제에서 더 오래 reasoning합니다. 기본값은 모델과 plan에 따라 다르므로 `/effort`에서 현재 값을 확인하는 것이 좋습니다.

한 번의 prompt에서만 더 깊게 생각하라는 신호를 주고 싶다면 `ultrathink`를 사용할 수 있습니다. 이는 세션의 effort level을 바꾸지는 않고, 해당 turn에서 더 깊게 reasoning하라는 신호입니다.

```text
ultrathink. 이 버그의 가능한 원인을 단계별로 좁혀가며 분석해줘.
```

Planning Mode와 effort level은 다른 종류의 복잡도를 다룹니다.

| 기능 | 적합한 상황 |
|---|---|
| Planning Mode | 코드베이스 전반 이해, 다단계 구현, 여러 파일 변경 |
| 높은 `/effort` | 복잡한 로직, 어려운 디버깅, 알고리즘 문제 |

넓은 코드베이스 조사와 깊은 추론이 모두 필요하다면 둘을 함께 쓸 수 있습니다. 다만 둘 다 token과 시간이 더 들 수 있습니다.

## 대화 흐름 제어하기

긴 작업에서는 Claude가 잘못된 방향으로 가거나, 이전 context가 다음 작업을 방해할 수 있습니다. Claude Code에는 이를 제어하는 명령들이 있습니다.

### Escape로 중단하기

Claude가 너무 넓게 작업하거나 잘못된 방향으로 가기 시작하면 `Escape`를 눌러 중단할 수 있습니다. 중단 후 더 좁은 요청으로 다시 지시합니다.

예를 들어 여러 함수 테스트를 한꺼번에 쓰려다가 범위가 커진다면:

```text
일단 useAuth 하나만 대상으로 테스트를 작성해줘.
다른 hook은 아직 건드리지 마.
```

반복적으로 같은 실수를 한다면 `Escape`로 멈춘 뒤 `/memory`를 실행하거나 `CLAUDE.md`를 직접 수정해 올바른 규칙을 남길 수 있습니다.

### `/rewind`

대화가 길어지면 불필요한 디버깅 과정이나 실패한 시도들이 context에 남습니다. 이럴 때는 `Escape`를 두 번 누르거나 `/rewind`를 입력해 이전 시점으로 돌아갈 수 있습니다.

```text
/rewind
```

이 기능은 Claude가 코드베이스를 이해한 유용한 context는 유지하되, 방해되는 최근 대화만 제거하고 싶을 때 좋습니다.

### `/compact`

`/compact`는 긴 대화 전체를 요약해 중요한 정보만 남깁니다.

```text
/compact
```

Claude가 현재 작업과 프로젝트에 대해 많이 배웠고, 이어지는 관련 작업을 계속하고 싶을 때 사용합니다. 긴 대화의 핵심을 보존하면서 context를 줄이는 방법입니다.

### `/clear`

완전히 다른 작업으로 넘어갈 때는 `/clear`가 더 적합합니다.

```text
/clear
```

이 명령은 fresh context로 새 대화를 시작합니다. 이전 conversation은 session history에서 사라지는 것이 아니며, 필요하면 `/resume`으로 돌아갈 수 있습니다.

정리하면:

- 관련 작업을 계속한다 → `/compact`
- 다른 작업으로 전환한다 → `/clear`
- 최근 흐름만 되돌린다 → `/rewind`
- Claude가 잘못 가고 있다 → `Escape`

## Custom commands

Claude Code에는 slash command가 기본 제공되지만, 프로젝트별 반복 작업을 직접 command로 만들 수도 있습니다.

프로젝트 루트에서 다음 구조를 만듭니다.

```text
.claude/
  commands/
    audit.md
```

파일 이름이 command 이름이 됩니다. 즉 `audit.md`는 `/audit` 명령이 됩니다. Claude Code가 자동으로 인식하므로 보통 재시작이 필요 없습니다.

예를 들어 dependency 취약점 점검 command는 다음 workflow를 담을 수 있습니다.

1. `npm audit`으로 취약한 package 확인
2. `npm audit fix`로 가능한 업데이트 적용
3. 테스트 실행으로 업데이트가 깨뜨린 부분이 없는지 확인

### 인자 받기

custom command는 `$ARGUMENTS` placeholder를 사용해 인자를 받을 수 있습니다.

```markdown
Write comprehensive tests for: $ARGUMENTS

Testing conventions:
* Use Vitest with React Testing Library
* Place test files in a __tests__ directory in the same folder as the source file
* Name test files as [filename].test.ts(x)
* Use @/ prefix for imports

Coverage:
* Test happy paths
* Test edge cases
* Test error states
```

이 파일을 `.claude/commands/write_tests.md`로 저장하면 다음처럼 사용할 수 있습니다.

```text
/write_tests the use-auth.ts file in the hooks directory
```

custom command는 반복 workflow를 자동화하고, 팀의 테스트/배포/보일러플레이트 규칙을 Claude에게 일관되게 전달하는 데 유용합니다.

## MCP servers with Claude Code

Claude Code는 MCP(Model Context Protocol) server를 연결해 기능을 확장할 수 있습니다. MCP server는 로컬 또는 원격에서 실행되며, Claude에게 기본 기능에는 없는 tools와 abilities를 제공합니다.

대표 예시는 Playwright MCP server입니다. 브라우저를 조작할 수 있으므로 웹 개발 workflow에 특히 유용합니다.

터미널에서 다음 명령을 실행해 Playwright MCP server를 추가합니다. Claude Code 내부가 아니라 일반 terminal에서 실행합니다.

```bash
claude mcp add playwright npx @playwright/mcp@latest
```

이 명령은 두 가지를 설정합니다.

- MCP server 이름을 `playwright`로 등록
- 로컬에서 server를 시작할 command를 `npx @playwright/mcp@latest`로 지정

### 권한 관리

처음 MCP tool을 사용할 때 Claude는 권한을 묻습니다. 반복 prompt가 번거롭다면 `.claude/settings.local.json`에서 허용할 수 있습니다.

```json
{
  "permissions": {
    "allow": ["mcp__playwright"],
    "deny": []
  }
}
```

여기서 `mcp__playwright`의 double underscore를 주의해야 합니다.

### 실전 예시

Playwright MCP를 붙이면 Claude가 실제 브라우저에서 앱을 보고 개선할 수 있습니다.

```text
Navigate to localhost:3000, generate a basic component, review the styling,
and update the generation prompt at @src/lib/prompts/generation.tsx
to produce better components going forward.
```

Claude는 브라우저를 열고, 앱에서 component를 생성하고, 시각적 결과를 본 뒤, prompt 파일을 수정할 수 있습니다. 코드만 보는 것보다 실제 visual output을 관찰할 수 있기 때문에 UI 품질 개선에 더 강합니다.

예를 들어 generic purple-blue gradient 대신 다음 같은 방향을 prompt에 반영할 수 있습니다.

- warm sunset gradients
- ocean depth themes
- asymmetric layouts
- overlapping elements
- 더 창의적인 spacing과 구성

Playwright 외에도 MCP 생태계에는 database, API testing, monitoring, cloud service, 개발 도구 자동화 관련 server가 있습니다. 필요한 toolchain에 맞춰 Claude Code를 확장할 수 있습니다.

## GitHub integration

Claude Code는 GitHub Actions 안에서 Claude를 실행하는 공식 GitHub integration을 제공합니다. 크게 두 가지 workflow가 있습니다.

- issue/PR에서 `@claude` mention으로 작업 요청
- pull request가 열릴 때 자동 code review

### 설치

Claude Code에서 다음 명령을 실행합니다.

```text
/install-github-app
```

이 명령은 설치 과정을 안내합니다.

- GitHub에 Claude Code app 설치
- API key 추가
- workflow file을 포함한 pull request 자동 생성

PR을 merge하면 `.github/workflows`에 GitHub Actions 파일이 추가됩니다.

### 기본 Action

**Mention Action**은 issue나 PR에서 `@claude`를 mention하면 실행됩니다.

- 요청 분석과 task plan 생성
- codebase 접근 후 작업 수행
- issue/PR comment로 결과 응답

**Pull Request Action**은 PR이 생성될 때 자동으로 변경사항을 검토합니다.

- 수정 영향 분석
- 잠재 문제 확인
- 상세 report를 PR에 comment

### Workflow 커스터마이징

프로젝트에 맞게 setup step을 추가할 수 있습니다.

```yaml
- name: Project Setup
  run: |
    npm run setup
    npm run dev:daemon
```

Claude에게 프로젝트 상황을 알려주는 custom instruction도 줄 수 있습니다.

```yaml
custom_instructions: |
  The project is already set up with all dependencies installed.
  The server is already running at localhost:3000. Logs from it
  are being written to logs.txt. If needed, you can query the
  db with the 'sqlite3' cli. If needed, use the mcp__playwright
  set of tools to launch a browser and interact with the app.
```

MCP server도 workflow에서 설정할 수 있습니다.

```yaml
mcp_config: |
  {
    "mcpServers": {
      "playwright": {
        "command": "npx",
        "args": [
          "@playwright/mcp@latest",
          "--allowed-origins",
          "localhost:3000;cdn.tailwindcss.com;esm.sh"
        ]
      }
    }
  }
```

GitHub Actions에서는 local Claude Code와 달리 tool 권한을 명시적으로 나열해야 합니다.

```yaml
allowed_tools: "Bash(npm:*),Bash(sqlite3:*),mcp__playwright__browser_snapshot,mcp__playwright__browser_click"
```

MCP server tool도 각각 허용해야 합니다. local 환경처럼 한 번에 쉽게 승인하는 shortcut이 없으므로, 필요한 tool 목록을 workflow에 정확히 적어야 합니다.

GitHub integration은 Claude를 로컬 assistant에서 자동화된 팀원에 가깝게 확장합니다. issue 처리, PR 리뷰, 간단한 코드 변경을 GitHub 안에서 직접 수행할 수 있게 해줍니다.

## 이 모듈에서 이어질 내용

`Getting hands on` 섹션에서는 프로젝트를 준비한 뒤 Claude Code로 실제 작업을 해봅니다.

- Claude Code setup
- Project setup
- Adding context
- Making changes
- Controlling context
- Custom commands
- MCP servers with Claude Code
- GitHub integration

즉, 이번 레슨은 Claude Code가 작업할 playground를 준비하는 단계입니다. 이후 레슨에서는 이 코드베이스를 대상으로 context를 추가하고, 변경을 만들고, Claude Code의 동작을 더 세밀하게 제어하는 방법을 다룹니다.

## 정리

- Claude Code는 실제 코드베이스에서 써볼 때 학습 효과가 큽니다. 실습 프로젝트는 `uigen.zip`으로 제공되는 UI generation app입니다.
- Node.js를 준비한 뒤 `npm run setup`으로 의존성과 SQLite 데이터베이스를 준비하고, `npm run dev`로 실행합니다.
- `CLAUDE.md`, `/init`, `/memory`, `@file` mention을 사용해 Claude에게 필요한 context를 정확히 제공합니다.
- screenshot, Planning Mode, `/effort`, `ultrathink`는 변경 작업의 정확도와 reasoning 깊이를 높이는 도구입니다.
- `Escape`, `/rewind`, `/compact`, `/clear`는 긴 대화의 context를 정리하고 흐름을 제어하는 데 사용합니다.
- custom commands는 반복 workflow를 project-specific slash command로 만드는 방법입니다.
- MCP server와 GitHub integration을 붙이면 Claude Code가 브라우저, 외부 도구, GitHub Actions까지 다룰 수 있습니다.
