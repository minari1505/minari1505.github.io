---
title: "MCP"
title_ko: "MCP (Model Context Protocol)"
course: claude-code-101
lesson: 11
---

## 학습 목표

- MCP가 무엇이고 Claude Code를 외부 도구·데이터와 어떻게 연결하는지 이해하기
- MCP 서버 추가 방법과 로컬/사용자/프로젝트 scope 구분하기
- MCP의 context 비용과 이를 관리하는 방법 파악하기

## MCP란

**Model Context Protocol(MCP)**은 Claude Code가 외부 도구와 데이터 소스에 연결되게 하는 개방형 표준입니다. 질문을 하면 Claude가 언제 그 도구를 써야 할지 자동으로 이해합니다. 많은 맥락이 코드베이스 밖(데이터베이스, 생산성 앱, 공개 저장소)에 있는데, MCP가 그 간극을 잇습니다.

## 무엇을 할 수 있나

먼저 agentic AI에서 "tools" 개념을 이해해야 합니다. tool은 Claude Code 같은 agent가 작업을 더 효과적으로 완수하도록 행동을 수행하는 능력을 줍니다. 텍스트 응답만 돌려받는 일반 AI와 다른 점입니다.

예를 들어 팀이 Linear로 프로젝트를 관리한다면 Linear MCP 서버를 추가해 특정 이슈의 세부를 가져올 수 있습니다. 의존성의 최신 문서가 필요하면 Context7 같은 docs MCP 서버가 그것을 Claude Code에 제공합니다.

## MCP 서버 추가

`claude mcp add` 명령으로 서버를 추가합니다. 주요 유형은 둘입니다.

- **HTTP 서버** — 원격 서비스용. 서비스 제공자가 호스팅하며 네트워크로 연결.
- **Stdio 서버** — 내 컴퓨터에서 도는 로컬 프로세스용.

세션 안에서 `/mcp`로 서버를 관리해 연결 상태 확인, 필요 없는 서버 비활성화를 할 수 있습니다.

## 서버 scope

- **Local** — 현재 프로젝트에서 나에게만.
- **User** — 내 모든 프로젝트에서.
- **Project** — `.mcp.json` 파일을 버전 관리에 체크인해, 코드베이스를 쓰는 누구나 동일한 서버를 자동으로 갖게 함.

## context 비용

MCP 서버는 쓰지 않을 때도 tool 정의를 context window에 추가합니다. 서버가 많으면 가용 context를 잠식합니다. `/mcp`로 연결 상태를 보고 안 쓰는 것은 비활성화하세요.

- `gh`(GitHub), `aws`(AWS)처럼 **CLI 대체재**가 있으면 CLI가 더 context 효율적입니다. 지속적인 tool 정의를 추가하지 않기 때문입니다.
- **Skill**이 더 나을 수도 있습니다. Skill은 이름과 설명만 context에 로드되고, 필요할 때만 전체를 로드합니다.
- MCP tool이 context window의 10%를 넘으면 Claude Code는 자동으로 **tool search mode**로 전환해 필요한 도구를 온디맨드로 찾습니다 (다만 신뢰성이 다소 떨어질 수 있습니다).

## 핵심 정리

- MCP는 Claude Code를 외부 도구·데이터 소스에 연결합니다.
- `claude mcp add`로 서버를 추가하고, `.mcp.json`으로 프로젝트에 scope하면 팀이 자동으로 같은 서버를 씁니다.
- 안 쓰는 서버는 꺼서 context 사용량을 관리하세요.
