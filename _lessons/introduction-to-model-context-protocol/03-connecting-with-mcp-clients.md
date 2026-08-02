---
title: "Connecting with MCP clients"
title_ko: "MCP 클라이언트 연결"
course: introduction-to-model-context-protocol
lesson: 3
---

## 학습 목표

- MCP client가 애플리케이션과 MCP server 사이에서 어떤 역할을 하는지 이해하기
- `list_tools()`와 `call_tool()`로 Claude tool use 흐름을 연결하는 방법 정리하기
- MCP resources를 prompt context로 직접 포함하는 방식 이해하기
- MCP prompts를 서버에서 정의하고 client에서 가져와 쓰는 흐름 파악하기

## MCP client를 구현하는 이유

앞에서 MCP server를 만들고 Inspector로 tool을 테스트했습니다. 이제 필요한 것은 애플리케이션 코드가 그 MCP server와 통신할 수 있게 만드는 client입니다.

대부분의 실제 프로젝트에서는 MCP client와 MCP server를 둘 다 만들기보다 둘 중 하나를 구현하는 경우가 많습니다.

- 특정 서비스를 Claude에 노출하고 싶다면 MCP server를 만듭니다.
- 기존 MCP server를 내 앱에서 쓰고 싶다면 MCP client를 붙입니다.

이번 실습에서는 양쪽이 어떻게 맞물리는지 보기 위해 client와 server를 모두 구현합니다.

![MCP client 구현 위치](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849285%2F09_-_006_-_Implementing_a_Client_01.1749849285296.png)

## MCP client 구조

실습의 MCP client는 크게 두 층으로 볼 수 있습니다.

![MCP client와 client session](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849286%2F09_-_006_-_Implementing_a_Client_02.1749849285884.png)

| 구성요소 | 역할 |
|---|---|
| **MCP Client** | 우리가 직접 만드는 wrapper class. 앱 코드에서 쓰기 쉽게 session 접근과 cleanup을 감쌈 |
| **Client Session** | MCP Python SDK가 제공하는 실제 server 연결 객체 |

`ClientSession`은 server와 연결을 유지하므로 resource cleanup을 신경 써야 합니다. 그래서 애플리케이션 코드가 session을 직접 다루기보다, 별도 client class로 감싸서 연결과 종료를 일관되게 관리합니다.

## 애플리케이션 흐름 안에서 client가 쓰이는 지점

CLI 애플리케이션에서 MCP client는 두 지점에서 필요합니다.

![앱 흐름 안의 MCP client](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849286%2F09_-_006_-_Implementing_a_Client_06.1749849286672.png)

1. Claude에게 보낼 사용 가능한 tool 목록을 가져올 때
2. Claude가 tool call을 요청했을 때 MCP server에 실제 실행을 요청할 때

즉, client는 Claude와 MCP server 사이의 bridge입니다.

```text
사용자 질문
  ↓
앱이 MCP client로 tool 목록 조회
  ↓
Claude에게 질문 + tool 목록 전달
  ↓
Claude가 tool call 생성
  ↓
앱이 MCP client로 tool 실행 요청
  ↓
tool result를 Claude에게 전달
  ↓
Claude가 최종 답변 생성
```

## `list_tools()` 구현

먼저 MCP server가 제공하는 tool 목록을 가져오는 함수입니다.

```python
async def list_tools(self) -> list[types.Tool]:
    result = await self.session().list_tools()
    return result.tools
```

흐름은 단순합니다.

1. 현재 MCP session을 가져옵니다.
2. SDK의 `list_tools()`를 호출합니다.
3. result 안의 `tools`를 반환합니다.

이 tool 목록은 Claude API 호출 시 사용 가능한 tool schema로 전달됩니다.

## `call_tool()` 구현

Claude가 어떤 tool을 호출하겠다고 결정하면, 애플리케이션은 해당 tool 이름과 입력값을 MCP server에 전달해야 합니다.

```python
async def call_tool(
    self, tool_name: str, tool_input: dict
) -> types.CallToolResult | None:
    return await self.session().call_tool(tool_name, tool_input)
```

여기서 `tool_name`과 `tool_input`은 Claude의 tool call에서 나온 값입니다.

예를 들어 Claude가 `read_doc_contents`를 호출하고 싶다고 판단하면 다음처럼 연결됩니다.

```text
Claude tool call
  name: read_doc_contents
  input: {"doc_id": "report.pdf"}
        ↓
MCP client.call_tool("read_doc_contents", {"doc_id": "report.pdf"})
        ↓
MCP server의 read_doc_contents 실행
        ↓
CallToolResult 반환
```

## client 테스트

client 파일에 간단한 테스트 harness가 있다면 직접 실행해 연결을 확인할 수 있습니다.

```bash
uv run mcp_client.py
```

정상 동작하면 MCP server에 연결되고, server가 제공하는 tool 정의가 출력됩니다. 여기에는 tool 이름, 설명, input schema가 포함됩니다.

전체 CLI 애플리케이션은 다음처럼 실행합니다.

```bash
uv run main.py
```

예를 들어 다음 질문을 입력할 수 있습니다.

```text
What is the contents of the report.pdf document?
```

내부적으로는 다음 일이 일어납니다.

1. 앱이 MCP client로 사용 가능한 tool 목록을 가져옵니다.
2. 앱이 사용자 질문과 tool 목록을 Claude에게 보냅니다.
3. Claude가 `read_doc_contents` tool이 필요하다고 판단합니다.
4. 앱이 MCP client로 해당 tool을 실행합니다.
5. MCP server가 문서 내용을 반환합니다.
6. 앱이 tool result를 Claude에게 보내고, Claude가 최종 답변을 작성합니다.

## MCP Resources란?

Tool은 Claude가 필요할 때 호출하는 실행 단위입니다. 반면 **resource**는 server가 노출하는 정보를 prompt에 직접 포함할 수 있게 해주는 방식입니다.

![MCP resource 접근 흐름](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849281%2F09_-_008_-_Accessing_Resources_00.1749849281584.png)

예를 들어 사용자가 `@report.pdf`처럼 resource를 언급하면, 애플리케이션은 이를 resource 요청으로 인식할 수 있습니다.

```text
사용자 입력: @report.pdf 내용 요약해줘
  ↓
앱이 resource URI 인식
  ↓
MCP client가 ReadResourceRequest 전송
  ↓
MCP server가 ReadResourceResult 반환
  ↓
앱이 resource 내용을 prompt에 직접 포함
  ↓
Claude가 별도 tool call 없이 바로 응답
```

Resource는 "Claude가 읽어야 할 컨텍스트를 미리 prompt에 넣는 방식"에 가깝습니다.

## `read_resource()` 구현

resource를 읽기 위해 필요한 import는 다음과 같습니다.

```python
import json
from pydantic import AnyUrl
```

client에는 다음 함수를 구현합니다.

```python
async def read_resource(self, uri: str) -> Any:
    result = await self.session().read_resource(AnyUrl(uri))
    resource = result.contents[0]

    if isinstance(resource, types.TextResourceContents):
        if resource.mimeType == "application/json":
            return json.loads(resource.text)

    return resource.text
```

server는 resource 요청에 대해 `contents` list를 반환합니다. 보통 한 번에 하나의 resource를 읽으므로 첫 번째 요소를 사용합니다.

반환값에는 실제 content와 MIME type이 들어 있습니다.

- `application/json`: 문자열을 JSON으로 parse해서 Python object로 반환
- 그 외 text resource: raw text로 반환

이렇게 하면 JSON 같은 구조화 데이터와 일반 텍스트 문서를 같은 함수로 처리할 수 있습니다.

## Resources와 tools의 차이

| 구분 | Tools | Resources |
|---|---|---|
| 목적 | Claude가 행동을 요청 | 앱이 데이터를 prompt context로 포함 |
| 실행 시점 | Claude가 tool call을 생성한 뒤 | 사용자 입력 처리 단계에서 미리 읽을 수 있음 |
| 예시 | 문서 수정, API 호출, DB query | 문서 본문, 설정 파일, JSON 데이터 |
| 장점 | 동적 행동 가능 | 별도 tool round-trip 없이 빠르게 context 제공 |

문서 내용을 단순히 읽어 Claude에게 주는 작업이라면 resource가 더 자연스러울 수 있습니다. 반면 문서를 수정하거나 외부 API에서 조건에 따라 데이터를 가져와야 한다면 tool이 더 적합합니다.

## MCP Prompts란?

MCP server는 tool과 resource뿐 아니라 **prompt**도 제공할 수 있습니다.

Prompt는 client가 가져다 쓸 수 있는 잘 만들어진 메시지 템플릿입니다. 사용자가 직접 prompt engineering을 잘하지 않아도, server 작성자가 테스트하고 다듬은 고품질 instruction을 재사용할 수 있게 해줍니다.

![MCP prompt 정의](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849287%2F09_-_009_-_Defining_Prompts_00.1749849286857.png)

예를 들어 문서 관리 MCP server라면 다음과 같은 prompt를 제공할 수 있습니다.

- 문서를 Markdown으로 재포맷하기
- 문서 요약하기
- 문서에서 action item 추출하기
- 기술 명세를 표로 정리하기

사용자는 "report.pdf를 markdown으로 다시 써줘"라고 직접 입력할 수도 있습니다. 하지만 server author가 미리 잘 설계한 `/format` prompt를 제공하면 더 일관된 결과를 얻을 수 있습니다.

![잘 설계된 prompt를 재사용하는 이유](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849289%2F09_-_009_-_Defining_Prompts_07.1749849288094.png)

## server에서 prompt 정의하기

Prompt도 tool과 비슷하게 decorator로 정의합니다.

```python
from mcp.server.fastmcp.prompts import base
from pydantic import Field

@mcp.prompt(
    name="format",
    description="Rewrites the contents of the document in Markdown format."
)
def format_document(
    doc_id: str = Field(description="Id of the document to format")
) -> list[base.Message]:
    prompt = f"""
Your goal is to reformat a document to be written with markdown syntax.

The id of the document you need to reformat is:
<document_id>
{doc_id}
</document_id>

Add in headers, bullet points, tables, etc as necessary. Feel free to add in structure.
Use the 'edit_document' tool to edit the document.
"""

    return [
        base.UserMessage(prompt)
    ]
```

이 함수는 Claude에게 보낼 message list를 반환합니다. 단일 user message만 반환할 수도 있고, 더 복잡한 대화 흐름이 필요하면 user/assistant message를 여러 개 반환할 수도 있습니다.

Inspector에서 prompt를 테스트하면 변수 interpolation이 적용된 실제 message를 확인할 수 있습니다.

![MCP Inspector에서 prompt 테스트](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849289%2F09_-_009_-_Defining_Prompts_18.1749849289799.png)

## client에서 prompt 가져오기

client 쪽에서는 두 함수를 구현합니다.

먼저 server가 제공하는 prompt 목록을 가져옵니다.

```python
async def list_prompts(self) -> list[types.Prompt]:
    result = await self.session().list_prompts()
    return result.prompts
```

특정 prompt를 가져올 때는 argument dictionary를 함께 전달합니다.

```python
async def get_prompt(self, prompt_name, args: dict[str, str]):
    result = await self.session().get_prompt(prompt_name, args)
    return result.messages
```

예를 들어 `format` prompt가 `doc_id`를 받는다면 다음처럼 호출합니다.

```python
messages = await client.get_prompt("format", {"doc_id": "plan.md"})
```

이 `doc_id` 값은 server의 prompt 함수에 keyword argument로 전달되고, prompt template 안에 삽입됩니다.

## CLI에서 prompt 사용하기

CLI에서는 `/`를 입력하면 사용 가능한 prompt가 command처럼 표시됩니다.

![CLI에서 prompt 선택](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1751999840%2F011_-_Prompts_in_the_Client_11.1751999840520.png)

예를 들어 `/format`을 선택하면 문서 ID를 고르고, client가 MCP server에서 해당 prompt를 가져옵니다. 그런 다음 완성된 message를 Claude에게 보냅니다.

![MCP prompt 사용 흐름](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1751999841%2F011_-_Prompts_in_the_Client_17.1751999841663.png)

이 방식은 복잡한 작업을 안정적인 명령처럼 제공할 때 유용합니다. 사용자에게 긴 지시문을 쓰게 하는 대신, MCP server가 검증된 prompt를 제공하고 client가 이를 불러와 실행합니다.

## 핵심 정리

MCP client는 애플리케이션이 MCP server의 기능을 실제로 쓰게 해주는 연결 계층입니다.

- `list_tools()`는 server가 제공하는 tool 목록을 가져옵니다.
- `call_tool()`은 Claude가 요청한 tool을 MCP server에서 실행합니다.
- `read_resource()`는 server resource를 읽어 prompt context에 직접 포함할 수 있게 합니다.
- `list_prompts()`와 `get_prompt()`는 server가 제공하는 재사용 가능한 prompt template을 가져옵니다.

MCP client를 붙이면 내 애플리케이션은 tool, resource, prompt를 같은 MCP server에서 가져와 Claude workflow에 연결할 수 있습니다. 이 구조 덕분에 외부 서비스 통합을 애플리케이션 내부에 모두 작성하지 않고, MCP server를 통해 재사용 가능한 형태로 분리할 수 있습니다.
