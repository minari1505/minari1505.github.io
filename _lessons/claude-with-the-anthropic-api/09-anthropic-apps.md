---
title: "Anthropic Apps — Claude Code & Computer Use"
title_ko: "Claude Code와 Computer Use"
course: claude-with-the-anthropic-api
lesson: 9
---

## 학습 목표

- Claude Code와 Computer Use를 에이전트 사례로 이해하기
- Claude Code의 설치, 프로젝트 초기화, 협업 워크플로우 파악하기
- MCP 서버로 Claude Code를 확장하는 방식 이해하기
- Computer Use가 GUI 환경을 다루는 방식과 주의점 정리하기

이 모듈은 Anthropic이 만든 두 가지 애플리케이션, **Claude Code**와 **Computer Use**를 살펴봅니다. 둘 다 단순한 편의 도구가 아니라, 지금까지 배운 **tool use, MCP, multi-step execution, 환경과의 상호작용**이 실제 제품 안에서 어떻게 조합되는지 보여주는 에이전트 사례입니다.

강의의 흐름은 다음과 같습니다.

1. Claude Code: 터미널에서 동작하는 에이전틱 코딩 assistant
2. Computer Use: Claude가 화면을 보고 마우스/키보드를 조작하는 도구 묶음
3. Agents: 두 사례를 통해 에이전트가 성공적으로 동작하기 위한 조건 이해

## Claude Code

**Claude Code**는 터미널에서 실행되는 코딩 assistant입니다. 프로젝트 폴더 안에서 `claude`를 실행하면, Claude가 코드베이스를 읽고 파일을 수정하고 명령을 실행하며 개발 작업을 도와줍니다.

Claude Code가 할 수 있는 일은 다음과 같습니다.

- 파일 검색, 읽기, 생성, 수정
- 터미널 명령 실행
- 테스트 실행과 실패 원인 분석
- 문서 검색과 코드 예시 확인
- Git 작업 보조
- MCP 서버 연결을 통한 기능 확장

지원 환경은 macOS, Windows WSL, Linux입니다. 핵심은 Claude가 단순히 코드를 "답변"하는 것이 아니라, 프로젝트 안에서 실제 파일과 명령을 다루며 여러 단계의 작업을 수행한다는 점입니다.

### Claude Code setup

설치는 짧습니다. 먼저 Node.js가 필요합니다. 이미 설치되어 있는지 확인하려면 터미널에서 `npm help` 같은 명령을 실행해볼 수 있습니다.

```bash
npm install -g @anthropic-ai/claude-code
claude
```

처음 `claude`를 실행하면 Anthropic 계정으로 로그인하라는 안내가 나옵니다. 로그인 후에는 프로젝트 디렉터리에서 Claude와 대화하면서 개발 작업을 맡길 수 있습니다.

예를 들면 다음처럼 요청할 수 있습니다.

```text
이 버그의 원인을 찾아줘.
테스트를 추가하고 통과하도록 구현해줘.
이 함수가 어디서 호출되는지 정리해줘.
```

## Claude Code in action

Claude Code는 코드 생성기라기보다 프로젝트 전반에서 함께 일하는 개발 파트너에 가깝습니다. 잘 쓰려면 Claude에게 충분한 context와 명확한 작업 흐름을 줘야 합니다.

### `/init`과 `CLAUDE.md`

새 프로젝트에서 Claude Code를 쓰기 시작할 때 가장 먼저 할 일은 `/init`입니다.

```text
/init
```

이 명령은 Claude가 코드베이스를 훑고 프로젝트 구조, 의존성, 빌드 명령, 테스트 명령, 코딩 스타일, 아키텍처 패턴을 파악하게 합니다. 그 결과는 `CLAUDE.md`에 정리됩니다.

`CLAUDE.md`는 이후 대화에 자동으로 포함되는 프로젝트 memory 역할을 합니다. 그래서 Claude가 매번 같은 프로젝트 규칙을 다시 물어보지 않고, 저장된 지침을 참고할 수 있습니다.

`CLAUDE.md`는 범위별로 나눠 사용할 수 있습니다.

- **Project memory**: 팀 전체가 공유하는 프로젝트 지침
- **Local memory**: git에 올리지 않는 개인 작업 지침
- **User memory**: 여러 프로젝트에서 공통으로 쓰는 개인 지침

작업 중 빠르게 memory를 추가하고 싶을 때는 `#` 명령을 사용할 수 있습니다.

```text
# Always use descriptive variable names
```

이렇게 입력하면 해당 지침을 project, local, user memory 중 어디에 저장할지 선택할 수 있습니다.

### 일반적인 작업 흐름

Claude Code는 바로 "구현해줘"라고 시키는 것보다, context와 계획을 먼저 주는 편이 안정적입니다.

1. 관련 파일을 먼저 읽게 한다
2. 아직 코드를 쓰지 말고 해결 계획을 세우게 한다
3. 계획을 검토한 뒤 구현을 요청한다
4. 테스트를 실행하고 실패하면 반복해서 수정한다

예시는 다음과 같습니다.

```text
Read the math.py and document.py files.

Plan to implement document_path_to_markdown tool.
Do not write code yet. Only explain the implementation plan.

Implement the plan and run the tests.
```

이 흐름이 좋은 이유는 Claude가 기존 코드 패턴을 먼저 보고, 바로 손대기 전에 어떤 파일을 바꿀지 정리하기 때문입니다. 특히 큰 코드베이스에서는 이 단계가 결과 품질에 큰 차이를 만듭니다.

### TDD 방식으로 쓰기

더 안정적인 방식은 테스트 주도 흐름입니다.

1. 관련 파일과 기존 테스트를 읽게 한다
2. 새 기능을 검증할 테스트 케이스를 생각하게 한다
3. 필요한 테스트를 먼저 작성하게 한다
4. 테스트가 통과하도록 구현하게 한다

Claude에게 성공 조건을 테스트로 먼저 주면, 구현이 더 명확해집니다. "좋아 보이는 코드"가 아니라 "검증 가능한 코드"를 목표로 반복할 수 있기 때문입니다.

### 추가 명령

자주 쓰는 Claude Code 명령은 다음과 같습니다.

- `/clear`: 현재 대화 context를 비우고 새로 시작
- `/init`: 코드베이스를 분석하고 `CLAUDE.md` 생성
- `#`: `CLAUDE.md` memory에 새 지침 추가

Claude Code는 테스트 실행, dependency 설치, git stage/commit 같은 반복적인 개발 작업도 처리할 수 있습니다. 다만 실제 코드 변경이나 명령 실행은 프로젝트에 영향을 주므로, 중요한 변경은 diff와 테스트 결과를 확인하는 습관이 필요합니다.

### MCP 서버로 확장

Claude Code에는 MCP client가 내장되어 있습니다. 그래서 MCP server를 연결하면 Claude Code의 기본 기능을 외부 시스템과 도구로 확장할 수 있습니다.

MCP server는 세 가지를 제공할 수 있습니다.

- **Tools**: Claude가 호출해 실행하는 함수
- **Resources**: Claude가 읽을 수 있는 데이터
- **Prompts**: 재사용 가능한 프롬프트 템플릿

Claude Code에 MCP server를 추가하는 기본 명령은 다음과 같습니다.

```bash
claude mcp add [server-name] [command-to-start-server]
```

예를 들어 문서 처리 MCP server가 `uv run main.py`로 실행된다면 다음처럼 등록합니다.

```bash
claude mcp add documents uv run main.py
```

등록 후 Claude Code를 시작하면 해당 MCP server에 연결할 수 있고, 서버가 제공하는 tool/resource/prompt를 사용할 수 있습니다.

### 예시: 문서 처리 도구

실습 예시는 `document_path_to_markdown` 같은 도구입니다. 이 tool은 PDF나 Word 문서 경로를 받아 내용을 markdown으로 변환합니다.

사용자는 Claude Code에 다음처럼 요청할 수 있습니다.

```text
Convert the tests/fixtures/mcp_docs.docx file to markdown.
```

그러면 Claude는 직접 파일 내용을 추측하지 않고, MCP server의 `document_path_to_markdown` tool을 호출해 문서를 읽고 변환된 markdown을 반환합니다.

### 자주 쓰이는 MCP 통합

MCP 생태계에는 개발 워크플로우에 붙이기 좋은 서버들이 많습니다.

- `sentry-mcp`: Sentry에 기록된 production 오류 조회
- `playwright-mcp`: 브라우저 자동화와 E2E 테스트 보조
- `figma-context-mcp`: Figma 디자인 context 제공
- `mcp-atlassian`: Jira, Confluence 접근
- `firecrawl-mcp-server`: 웹 스크래핑
- `slack-mcp`: Slack 메시지 작성과 thread 응답

여러 MCP server를 함께 붙이면 Claude Code는 단순 코딩 assistant를 넘어, 팀의 실제 개발 환경에 연결된 에이전트가 됩니다. 예를 들어 Jira 티켓을 읽고, Sentry 오류를 확인하고, 코드를 수정하고, 테스트를 돌린 뒤 Slack에 결과를 공유하는 흐름을 만들 수 있습니다.

## Computer Use (컴퓨터 사용)

**Computer Use**는 Claude가 화면을 보고 마우스와 키보드를 조작해 GUI 애플리케이션을 다루는 도구 묶음입니다. 텍스트 API나 전용 SDK가 없는 웹사이트·데스크톱 앱도, 사람이 쓰는 것처럼 화면을 보며 단계적으로 조작할 수 있게 합니다.

```
스크린샷 촬영 → Claude가 화면 파악 → 클릭/입력 좌표 지시 → 내 환경이 실행 → 다시 스크린샷 → 반복
```

Computer Use가 할 수 있는 일은 다음과 같습니다.

- 웹사이트 접속과 탐색
- 데스크톱 애플리케이션 조작
- 버튼 클릭, 텍스트 입력, 화면 상태 확인
- 시각적 UI를 기반으로 한 multi-step task 수행

중요한 점은 실행 환경을 애플리케이션 개발자가 제공한다는 것입니다. Claude는 스크린샷을 해석하고 다음 행동을 제안하지만, 실제 마우스/키보드 실행과 sandbox 구성은 클라이언트 쪽 책임입니다.

따라서 운영 환경에서는 다음을 신경 써야 합니다.

- 격리된 가상 데스크톱이나 sandbox 사용
- 민감한 작업 전 사용자 확인 단계 추가
- 로그인 세션, 결제, 삭제 같은 위험 작업 제한
- 실패했을 때 중단할 수 있는 명확한 guardrail 마련

Computer Use는 Claude Code와 마찬가지로 에이전트의 핵심 요소를 잘 보여줍니다. 모델이 환경을 관찰하고, 다음 행동을 선택하고, 실행 결과를 다시 관찰하면서 여러 단계의 작업을 이어갑니다.

## 정리

- **Claude Code**는 터미널 기반 에이전틱 코딩 assistant입니다. 파일, 셸, 테스트, git 작업을 다루며 `/init`과 `CLAUDE.md`로 프로젝트 context를 유지합니다.
- 좋은 Claude Code 워크플로우는 context 제공 → 계획 수립 → 구현 → 테스트 반복입니다. TDD 방식으로 쓰면 성공 조건이 명확해집니다.
- Claude Code는 MCP client를 내장하고 있어, `claude mcp add ...`로 외부 도구·자원·프롬프트를 연결할 수 있습니다.
- **Computer Use**는 Claude가 화면을 보고 GUI를 조작하는 방식입니다. API가 없는 환경을 자동화할 수 있지만, sandbox와 확인 절차가 중요합니다.
- 두 사례 모두 tool use, MCP, 환경 관찰, 반복 실행이 결합된 실제 에이전트 구현입니다.
