---
title: "Model Context Protocol"
title_ko: "MCP (Model Context Protocol)"
course: claude-with-the-anthropic-api
lesson: 8
---

## 학습 목표

- MCP가 무엇이고 어떤 문제를 푸는지 이해하기
- MCP의 3가지 핵심 제공물(도구·자원·프롬프트) 구분하기
- 클라이언트-서버 구조와 연결 방식 파악하기
- 로컬/원격 MCP 서버와 Claude API의 MCP connector 관계 이해하기

## MCP란?

**Model Context Protocol(MCP)**은 LLM 애플리케이션이 외부 시스템의 **도구·데이터·프롬프트**에 연결하는 **표준 프로토콜**이에요. "AI를 위한 USB-C 포트"라고 생각하면 돼요 — 서비스마다 제각각 통합하는 대신, MCP라는 공통 규격으로 연결해요.

Claude 같은 AI 앱이 실제 업무에 유용해지려면 로컬 파일, GitHub, Jira, Notion, DB, 사내 API 같은 외부 시스템을 읽고 조작할 수 있어야 합니다. MCP가 없으면 각 앱과 각 서비스가 서로 다른 방식으로 직접 통합해야 해요. 앱이 5개, 서비스가 10개면 통합 지점이 폭발합니다.

MCP는 이 문제를 공통 규격으로 줄입니다.

```text
AI 앱 / 에이전트
      ↓ MCP 클라이언트
MCP 서버들
      ↓
파일시스템, DB, GitHub, Notion, Jira, 내부 API ...
```

한 번 MCP 서버를 만들면 MCP를 지원하는 여러 클라이언트에서 재사용할 수 있습니다. 반대로 AI 앱은 MCP 클라이언트만 구현하면 다양한 서버에 붙을 수 있어요.

## 왜 MCP가 필요한가?

앞에서 배운 tool use는 "Claude에게 도구 스키마를 주고, Claude가 호출하면 내 코드가 실행한다"는 구조였습니다. MCP는 이 아이디어를 더 넓은 생태계 규격으로 만든 것입니다.

예를 들어 다음 작업을 생각해볼 수 있습니다.

- Claude가 로컬 파일을 읽고 수정
- Claude Code가 Figma 디자인을 보고 웹앱 생성
- 사내 챗봇이 여러 DB를 조회해 분석
- 개인 비서가 Google Calendar와 Notion을 읽고 일정/작업을 조정

이런 연결을 앱마다 새로 만들면 비용이 큽니다. MCP는 **도구와 데이터 연결 방식을 표준화**해서 개발자는 서버 하나를 만들고, 클라이언트는 같은 방식으로 여러 서버를 붙일 수 있게 합니다.

## 기본 구성: Host, Client, Server

MCP 문서에서는 보통 세 역할을 구분합니다.

- **Host**: 사용자가 실제로 쓰는 AI 애플리케이션. 예: Claude Desktop, IDE, 내 챗앱
- **MCP Client**: 특정 MCP 서버와 1:1 연결을 유지하는 구성요소. Host 안에 들어 있음
- **MCP Server**: 도구·자원·프롬프트를 제공하는 프로그램

하나의 host는 여러 MCP server에 연결될 수 있고, 보통 서버마다 별도의 MCP client 연결이 생깁니다.

```text
Claude Desktop / IDE / 내 앱 (Host)
  ├─ MCP Client ─ filesystem MCP Server
  ├─ MCP Client ─ GitHub MCP Server
  └─ MCP Client ─ DB MCP Server
```

MCP server는 실행 위치에 따라 크게 두 종류로 볼 수 있습니다.

- **Local MCP server**: 내 컴퓨터에서 실행. 보통 `stdio` transport 사용. 예: 로컬 파일시스템 서버
- **Remote MCP server**: 외부 서비스에서 실행. 보통 HTTP 기반 transport 사용. 예: Sentry, GitHub, 사내 API 서버

이 구분은 보안과 배포 방식에 중요합니다. 로컬 서버는 내 머신 자원에 접근하기 쉽고, 원격 서버는 인증·권한·네트워크 노출을 더 신경 써야 합니다.

## MCP가 제공하는 3가지

| 제공물 | 무엇인가 | 비유 |
|---|---|---|
| **Tools (도구)** | Claude가 호출해 실행하는 함수 | tool use의 표준화 버전 |
| **Resources (자원)** | 읽어올 수 있는 데이터/문서 | 파일·DB 레코드 |
| **Prompts (프롬프트)** | 재사용 가능한 프롬프트 템플릿 | 미리 만든 지시문 |

조금 더 풀어보면:

- **Tools**: 모델이 호출할 수 있는 실행 단위입니다. 파일 조작, API 호출, DB query, 계산처럼 "행동"에 가깝습니다.
- **Resources**: 클라이언트가 읽을 수 있는 데이터입니다. 파일 내용, DB record, API response처럼 "맥락"에 가깝습니다.
- **Prompts**: 자주 쓰는 지시문 템플릿입니다. 팀이 검증한 system prompt, few-shot 예시, 특정 workflow 지시문을 재사용할 수 있습니다.

MCP 서버는 이 세 가지를 모두 제공할 수도 있고, 하나만 제공할 수도 있습니다. 예를 들어 날씨 서버는 `get_forecast` 같은 tool만 제공할 수 있고, 문서 서버는 읽기 전용 resource 중심으로 동작할 수 있습니다.

## MCP의 계층 구조

MCP는 크게 두 계층으로 나눠 생각하면 이해하기 쉽습니다.

### Data layer

클라이언트와 서버가 어떤 메시지를 주고받는지 정의하는 계층입니다. JSON-RPC 2.0 기반으로 동작하며 다음을 포함합니다.

- 연결 초기화와 capability negotiation
- 서버가 제공하는 tools/resources/prompts 목록 조회
- tool 호출과 결과 반환
- resource 읽기
- prompt template 조회
- progress, notification 같은 보조 메시지

### Transport layer

실제로 메시지가 어떤 통로로 오가는지 정의하는 계층입니다.

- **stdio**: 같은 머신의 로컬 프로세스끼리 표준 입출력으로 통신
- **Streamable HTTP / SSE 계열**: 원격 서버와 HTTP 기반으로 통신

핵심은 transport가 달라도 위의 data layer 메시지 구조는 같은 방식으로 유지된다는 점입니다.

## 도구 정의 (Defining tools with MCP)

MCP 서버에서 도구를 정의하면, 연결된 어떤 클라이언트든 그 도구를 발견하고 쓸 수 있어요. 앞 레슨의 tool use와 개념은 같지만 — 이름·설명·입력 스키마 — **서버가 표준 방식으로 노출**한다는 점이 달라요.

## 서버 인스펙터 (Server inspector)

MCP 서버를 개발할 때 **인스펙터** 도구로 서버가 노출한 도구·자원·프롬프트를 직접 확인하고 테스트할 수 있어요. 클라이언트를 붙이기 전에 서버가 잘 동작하는지 점검해요.

## 클라이언트 구현 (Implementing a client)

클라이언트는 서버에 연결해서:

1. 서버가 제공하는 도구·자원·프롬프트 목록을 받아오고
2. Claude에게 그것들을 도구로 넘겨 사용하게 하고
3. Claude가 도구를 호출하면 서버에 실행을 위임하고 결과를 되돌려요

## 자원과 프롬프트 (Resources & Prompts)

- **자원 접근**: 서버가 노출한 파일·데이터를 URI로 읽어와 프롬프트에 넣을 수 있어요.
- **프롬프트**: 서버가 제공하는 프롬프트 템플릿을 클라이언트에서 불러 사용해요. 팀이 검증한 프롬프트를 재사용하기 좋아요.

## MCP와 API의 관계

Claude API에는 **MCP connector**가 있어, 원격 MCP 서버에 Claude가 직접 연결하도록 Messages API 요청에 서버를 선언할 수 있습니다. 이 경우 별도의 MCP client를 직접 구현하지 않고도 MCP tool을 Messages API에서 사용할 수 있어요.

개념적으로는 두 가지를 요청에 넣습니다.

1. `mcp_servers`: 어떤 MCP 서버에 연결할지. URL, 이름, 인증 정보 등
2. `tools`: 그 서버의 MCP toolset 중 무엇을 Claude에게 노출할지

```python
response = client.beta.messages.create(
    model="claude-opus-4-8",
    max_tokens=1000,
    messages=[{"role": "user", "content": "What tools do you have available?"}],
    mcp_servers=[
        {
            "type": "url",
            "url": "https://example-server.modelcontextprotocol.io/sse",
            "name": "example-mcp",
            "authorization_token": "YOUR_TOKEN",
        }
    ],
    tools=[
        {"type": "mcp_toolset", "mcp_server_name": "example-mcp"}
    ],
    betas=["mcp-client-2025-11-20"],
)
```

단, Claude API의 MCP connector는 MCP 전체 기능 중 tool call 중심으로 제공됩니다. local `stdio` 서버를 API에 직접 붙이는 방식이 아니라, HTTP로 노출된 remote MCP server를 연결하는 구조라고 이해하면 됩니다.

로컬 MCP 서버나 더 세밀한 제어가 필요하면 SDK의 MCP client/helper로 직접 연결하는 방식이 더 적합합니다.

## 정리

- MCP = 도구·자원·프롬프트를 연결하는 **표준 프로토콜** ("AI의 USB-C").
- Host 안의 MCP client가 MCP server에 연결하고, server가 기능을 제공하는 구조.
- 3가지 제공물: **Tools**(실행), **Resources**(데이터), **Prompts**(템플릿).
- local server는 보통 `stdio`, remote server는 HTTP 기반 transport를 사용.
- 인스펙터로 서버를 점검하고, 클라이언트나 Claude API MCP connector로 Claude에 연결해요.
