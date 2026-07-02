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

## MCP 클라이언트가 하는 일

MCP client는 내 애플리케이션과 MCP server 사이의 통신 다리입니다. 외부 도구나 서비스를 쓰고 싶을 때, client가 MCP server와 메시지를 주고받고 프로토콜 세부 사항을 감춰줍니다.

MCP의 장점 중 하나는 **transport agnostic**이라는 점입니다. 클라이언트와 서버가 꼭 한 방식으로만 통신해야 하는 것이 아닙니다. 가장 흔한 개발 환경에서는 같은 머신에서 client와 server가 실행되고, `stdio`로 통신합니다. 하지만 HTTP, WebSocket, SSE 계열 등 네트워크 transport로도 연결할 수 있습니다.

MCP client/server 사이에서 자주 오가는 메시지는 다음과 같습니다.

- `ListToolsRequest` / `ListToolsResult`: 서버가 제공하는 도구 목록을 조회
- `CallToolRequest` / `CallToolResult`: 특정 도구를 인자와 함께 실행하고 결과를 받음

예를 들어 사용자가 "내 GitHub repository가 뭐가 있어?"라고 물었다고 해봅시다.

1. 내 앱은 Claude에게 질문을 보내기 전에 MCP server가 제공하는 tool 목록이 필요함
2. MCP client가 MCP server에 `ListToolsRequest`를 보냄
3. MCP server가 `ListToolsResult`로 사용 가능한 tool 목록을 반환
4. 내 앱이 사용자 질문 + tool 목록을 Claude에게 전달
5. Claude가 필요한 tool을 골라 `tool_use`를 반환
6. 내 앱이 MCP client에게 해당 tool 실행을 요청
7. MCP client가 MCP server에 `CallToolRequest`를 보냄
8. MCP server가 실제 GitHub API를 호출하고 `CallToolResult`를 반환
9. 내 앱이 tool result를 Claude에게 다시 보내고, Claude가 최종 답변 생성

단계는 많지만 책임은 분명합니다. MCP client는 서버 통신과 프로토콜 처리를 맡고, 내 앱은 사용자 경험과 Claude 대화 흐름에 집중할 수 있습니다.

## 실습 프로젝트: CLI 문서 챗봇

강의에서는 MCP client와 MCP server가 어떻게 함께 동작하는지 이해하기 위해 **CLI 기반 문서 챗봇**을 만듭니다.

구성요소는 두 가지입니다.

- **MCP client**: 사용자 입력을 받고 Claude와 MCP server 사이의 흐름을 조정
- **Custom MCP server**: 문서 읽기/수정 같은 작업을 tool로 제공

서버는 두 가지 도구를 제공합니다.

- 문서 내용 읽기
- 문서 내용 업데이트하기

실습에서는 단순화를 위해 문서를 DB에 저장하지 않고 메모리 안의 dictionary에 보관합니다. 실제 프로젝트에서는 보통 MCP client와 server를 모두 직접 만들지는 않습니다. 내 서비스를 다른 AI 앱에서 쓰게 하려면 MCP server를 만들고, 기존 MCP server들을 내 앱에 붙이고 싶다면 MCP client를 만듭니다. 강의에서는 통신 구조를 이해하기 위해 양쪽을 모두 구현합니다.

프로젝트 파일에는 보통 다음이 포함됩니다.

- `main.py`: CLI 앱 진입점
- `mcp_client.py`: MCP client 구현
- `mcp_server.py`: MCP server 구현

실행 전에는 `.env`에 Anthropic API key를 넣고, README에 따라 의존성을 설치합니다. 실행은 다음처럼 합니다.

```bash
# uv 사용 권장
uv run main.py

# 일반 Python 사용
python main.py
```

앱이 뜨면 간단히 `"what's 1+1?"`처럼 물어서 Claude 응답이 정상적으로 오는지 확인합니다. 그다음 MCP tool 연결을 단계적으로 붙입니다.

## 도구 정의 (Defining tools with MCP)

MCP 서버에서 도구를 정의하면, 연결된 어떤 클라이언트든 그 도구를 발견하고 쓸 수 있어요. 앞 레슨의 tool use와 개념은 같지만 — 이름·설명·입력 스키마 — **서버가 표준 방식으로 노출**한다는 점이 달라요.

Python MCP SDK를 쓰면 긴 JSON schema를 직접 작성하지 않아도 됩니다. decorator와 type hint를 바탕으로 SDK가 Claude에게 필요한 schema를 만들어줍니다.

간단한 MCP server는 다음처럼 시작할 수 있습니다.

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("DocumentMCP", log_level="ERROR")
```

실습에서는 문서를 메모리 dictionary에 저장합니다.

```python
docs = {
    "deposition.md": "This deposition covers the testimony of Angela Smith, P.E.",
    "report.pdf": "The report details the state of a 20m condenser tower.",
    "financials.docx": "These financials outline the project's budget and expenditure",
    "plan.md": "The plan outlines the steps for the project's implementation.",
}
```

### 문서 읽기 도구

```python
@mcp.tool(
    name="read_doc_contents",
    description="Read the contents of a document and return it as a string.",
)
def read_document(
    doc_id: str = Field(description="Id of the document to read"),
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")

    return docs[doc_id]
```

`@mcp.tool` decorator가 tool 정의를 MCP 형식으로 노출하고, `Field` description은 Claude가 인자의 의미를 이해하는 데 도움을 줍니다.

### 문서 수정 도구

```python
@mcp.tool(
    name="edit_document",
    description="Edit a document by replacing a string in the documents content with a new string.",
)
def edit_document(
    doc_id: str = Field(description="Id of the document that will be edited"),
    old_str: str = Field(description="The text to replace. Must match exactly, including whitespace."),
    new_str: str = Field(description="The new text to insert in place of the old text."),
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")

    docs[doc_id] = docs[doc_id].replace(old_str, new_str)
```

이 도구는 문서 ID, 정확히 일치해야 하는 기존 문자열, 새 문자열을 받아 Python의 `replace()`로 수정합니다. 존재하지 않는 문서 ID가 들어오면 `ValueError`를 던져 Claude가 문제를 이해하고 다시 시도하거나 사용자에게 설명할 수 있게 합니다.

SDK 방식의 장점은 명확합니다.

- Python type hint 기반 schema 자동 생성
- Pydantic `Field`를 통한 인자 설명/검증
- 수동 JSON schema 작성보다 적은 boilerplate
- 일반 Python 함수처럼 읽히고 테스트하기 쉬움

## 서버 인스펙터 (Server inspector)

MCP 서버를 개발할 때 **인스펙터** 도구로 서버가 노출한 도구·자원·프롬프트를 직접 확인하고 테스트할 수 있어요. 클라이언트를 붙이기 전에 서버가 잘 동작하는지 점검해요.

Python MCP SDK에는 브라우저 기반 inspector가 포함되어 있습니다. 전체 앱을 붙이기 전에 MCP server의 tool, resource, prompt를 독립적으로 테스트할 수 있습니다.

프로젝트의 Python 환경을 활성화한 뒤 다음 명령을 실행합니다.

```bash
mcp dev mcp_server.py
```

그러면 보통 6277번 포트에서 개발 서버가 뜨고, 브라우저에서 열 수 있는 로컬 URL이 출력됩니다. Inspector UI는 계속 발전 중이라 화면은 강의 스크린샷과 다를 수 있지만 핵심 흐름은 비슷합니다.

기본 사용 흐름은 다음과 같습니다.

1. 왼쪽에서 `Connect` 클릭
2. `Tools` 섹션으로 이동
3. `List Tools`로 서버가 노출한 도구 확인
4. 테스트할 tool 선택
5. 필요한 parameter 입력
6. `Run Tool`로 실행하고 결과 확인

예를 들어 `read_doc_contents` 도구를 테스트하려면 `deposition.md` 같은 문서 ID를 넣고 실행합니다. 결과로 문서 내용이 반환되는지 확인합니다. 이후 `edit_document`로 문자열을 바꾸고, 다시 `read_doc_contents`를 실행해 변경이 반영됐는지 확인할 수 있습니다.

Inspector는 개발 루프를 짧게 만듭니다.

- MCP server 코드를 수정
- Inspector에서 개별 tool만 빠르게 테스트
- 전체 Claude 앱 연결 없이 결과 확인
- 문제를 server 단에서 분리해 디버깅

## 클라이언트 구현 (Implementing a client)

클라이언트는 서버에 연결해서:

1. 서버가 제공하는 도구·자원·프롬프트 목록을 받아오고
2. Claude에게 그것들을 도구로 넘겨 사용하게 하고
3. Claude가 도구를 호출하면 서버에 실행을 위임하고 결과를 되돌려요

실습 코드에서는 MCP Python SDK의 **Client Session**을 직접 쓰기보다, 이를 감싸는 커스텀 `MCPClient` 클래스를 만듭니다.

- **Client Session**: MCP server와 실제로 연결된 세션. SDK가 제공
- **MCPClient**: 앱 코드가 쓰기 쉬운 메서드로 감싸고, 연결 정리까지 책임지는 wrapper

세션은 사용 후 정리가 필요하므로 `async with` 형태로 다루는 것이 안전합니다.

```python
async with MCPClient(
    command="uv",
    args=["run", "mcp_server.py"],
) as client:
    tools = await client.list_tools()
    print(tools)
```

앱에서 MCP client가 제공해야 하는 핵심 메서드는 우선 두 가지입니다.

### `list_tools()`

서버가 제공하는 tool 목록을 가져옵니다.

```python
async def list_tools(self) -> list[types.Tool]:
    result = await self.session().list_tools()
    return result.tools
```

이 결과를 Claude API의 `tools` 인자로 변환해 넘기면, Claude가 어떤 tool을 사용할 수 있는지 알 수 있습니다.

### `call_tool()`

Claude가 특정 tool을 호출하겠다고 하면, 앱은 MCP client를 통해 MCP server에 실행을 위임합니다.

```python
async def call_tool(
    self,
    tool_name: str,
    tool_input: dict,
) -> types.CallToolResult | None:
    return await self.session().call_tool(tool_name, tool_input)
```

전체 흐름은 다음과 같습니다.

1. 앱이 `list_tools()`로 MCP server의 tool 목록을 가져옴
2. 사용자 질문과 tool 목록을 Claude에게 보냄
3. Claude가 `read_doc_contents` 같은 tool use를 반환
4. 앱이 `call_tool()`로 MCP server에 실행 요청
5. tool 결과를 다시 Claude에게 보내 최종 답변 생성

예를 들어 사용자가 `"What is the contents of the report.pdf document?"`라고 물으면 Claude는 문서 읽기 tool이 필요하다고 판단하고, 앱은 MCP client를 통해 `read_doc_contents`를 실행합니다.

## 자원과 프롬프트 (Resources & Prompts)

- **자원 접근**: 서버가 노출한 파일·데이터를 URI로 읽어와 프롬프트에 넣을 수 있어요.
- **프롬프트**: 서버가 제공하는 프롬프트 템플릿을 클라이언트에서 불러 사용해요. 팀이 검증한 프롬프트를 재사용하기 좋아요.

## 리소스 정의와 접근

MCP의 **resources**는 tool처럼 "행동"을 수행하기보다, 서버가 가진 데이터를 클라이언트가 읽을 수 있게 노출하는 기능입니다. 일반 HTTP 서버의 GET handler와 비슷하게 생각하면 됩니다.

문서 챗봇 예시에서는 `@document_name` 멘션 기능을 만들 수 있습니다.

- 사용자가 `@`를 입력하면 사용 가능한 문서 목록을 보여줌
- 사용자가 `@report.pdf`를 언급하면 해당 문서 내용을 가져와 Claude 프롬프트에 직접 삽입

이 경우 Claude가 tool call로 문서를 읽게 할 수도 있지만, resource를 쓰면 앱이 먼저 문서를 읽어서 Claude에게 context로 넣을 수 있습니다. 단순 조회 데이터라면 tool 호출보다 더 직접적입니다.

### 리소스 종류

MCP resource는 크게 두 가지입니다.

- **Direct resource**: 고정 URI. 예: `docs://documents`
- **Templated resource**: 파라미터가 있는 URI. 예: `docs://documents/{doc_id}`

Templated resource에서는 URI의 `{doc_id}`가 함수 인자로 자동 전달됩니다.

### 문서 목록 리소스

```python
@mcp.resource(
    "docs://documents",
    mime_type="application/json",
)
def list_docs() -> list[str]:
    return list(docs.keys())
```

문서 목록은 구조화 데이터이므로 `application/json` MIME type을 사용합니다. Python SDK가 반환값을 알아서 직렬화하므로 직접 JSON 문자열로 바꿀 필요는 없습니다.

### 문서 내용 리소스

```python
@mcp.resource(
    "docs://documents/{doc_id}",
    mime_type="text/plain",
)
def fetch_doc(doc_id: str) -> str:
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")

    return docs[doc_id]
```

문서 본문은 일반 텍스트이므로 `text/plain`을 사용합니다. 리소스도 Inspector에서 테스트할 수 있고, Resources에는 direct resource가, Resource Templates에는 파라미터가 있는 resource가 표시됩니다.

### 클라이언트에서 리소스 읽기

MCP client에는 resource URI를 받아 서버에서 내용을 읽어오는 메서드가 필요합니다.

```python
import json
from pydantic import AnyUrl
```

```python
async def read_resource(self, uri: str) -> Any:
    result = await self.session().read_resource(AnyUrl(uri))
    resource = result.contents[0]

    if isinstance(resource, types.TextResourceContents):
        if resource.mimeType == "application/json":
            return json.loads(resource.text)

        return resource.text
```

서버 응답의 `contents`는 리스트입니다. 보통 첫 번째 요소에 실제 resource 데이터와 MIME type 같은 메타데이터가 들어 있습니다. MIME type을 보고 JSON은 Python 객체로 파싱하고, plain text는 문자열로 반환합니다.

CLI에서는 `"What's in the @report.pdf document?"`처럼 입력했을 때 다음 흐름이 됩니다.

1. 앱이 사용 가능한 resources를 autocomplete로 보여줌
2. 사용자가 `@report.pdf`를 선택
3. 앱이 MCP client의 `read_resource()`로 문서 내용을 가져옴
4. 가져온 내용을 Claude에게 보낼 prompt에 직접 포함

이 방식의 장점은 Claude가 문서 내용을 얻기 위해 별도 tool call을 하지 않아도 된다는 점입니다. 앱이 필요한 context를 미리 넣어주므로 더 빠르고 단순한 흐름을 만들 수 있습니다.

## MCP 프롬프트

MCP의 **prompts**는 서버가 클라이언트에게 제공하는 재사용 가능한 메시지 템플릿입니다. 단순 문자열이 아니라, user/assistant 메시지들의 묶음으로 볼 수 있습니다.

좋은 MCP prompt는 다음 조건을 만족해야 합니다.

- 서버의 목적과 직접 관련 있음
- 충분히 테스트되어 있음
- 명확하고 구체적인 지시를 포함
- 서버가 제공하는 tools/resources와 잘 맞음
- 필요한 인자를 사용자가 제공할 수 있게 설계됨

### 프롬프트 목록 조회

클라이언트에서 서버가 제공하는 prompt 목록을 가져옵니다.

```python
async def list_prompts(self) -> list[types.Prompt]:
    result = await self.session().list_prompts()
    return result.prompts
```

CLI에서는 사용자가 `/`를 입력했을 때 사용 가능한 prompt를 command처럼 보여줄 수 있습니다.

### 개별 프롬프트 가져오기

특정 prompt를 가져올 때는 필요한 인자를 함께 넘깁니다.

```python
async def get_prompt(self, prompt_name, args: dict[str, str]):
    result = await self.session().get_prompt(prompt_name, args)
    return result.messages
```

서버 쪽 prompt 함수가 `doc_id` 같은 인자를 받는다면:

```python
def format_document(doc_id: str):
    ...
```

클라이언트는 다음처럼 인자를 전달합니다.

```python
messages = await client.get_prompt(
    "format",
    {"doc_id": "report.pdf"},
)
```

MCP server는 이 인자를 prompt 함수에 keyword argument로 전달하고, 값이 삽입된 메시지 목록을 반환합니다. 이 메시지 목록은 Claude에게 그대로 보낼 수 있습니다.

CLI 흐름은 다음과 같습니다.

1. 사용자가 `/format` 같은 prompt를 선택
2. 앱이 필요한 인자, 예를 들어 문서 ID를 물어봄
3. `get_prompt()`로 인자가 반영된 메시지 목록을 가져옴
4. 그 메시지를 Claude에게 보내 작업 시작
5. Claude는 필요하면 MCP tool을 사용해 추가 데이터를 가져오고 작업을 완료

Prompt는 "완전히 고정된 명령"과 "매번 새로 쓰는 자유 입력" 사이의 좋은 절충안입니다. 팀이 검증한 workflow를 제공하면서도, `doc_id` 같은 인자로 동적으로 사용할 수 있습니다.

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
- client는 `list_tools()`/`call_tool()`로 tool 흐름을 연결하고, `read_resource()`/`get_prompt()`로 context와 템플릿을 가져옵니다.
- local server는 보통 `stdio`, remote server는 HTTP 기반 transport를 사용.
- 인스펙터로 서버를 점검하고, 클라이언트나 Claude API MCP connector로 Claude에 연결해요.
