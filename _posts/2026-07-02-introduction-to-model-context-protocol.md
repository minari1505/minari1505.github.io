---
title: "Introduction to Model Context Protocol 정리"
date: 2026-07-02
categories:
  - CCA-F
tags:
  - MCP
  - Claude
  - Anthropic
  - Python
  - Model Context Protocol
excerpt: "Anthropic Academy의 MCP 입문 강의를 CCA-F 시험 대비 관점에서 정리한 노트"
---

Anthropic Academy의 [Introduction to Model Context Protocol](https://anthropic.skilljar.com/introduction-to-model-context-protocol/303756) 강의와 [MCP 공식 문서](https://modelcontextprotocol.io/docs/getting-started/intro)를 기준으로 정리한 노트다. 핵심은 MCP가 AI 애플리케이션과 외부 데이터, 도구, 워크플로를 연결하는 표준 프로토콜이라는 점이다.

## 한 줄 요약

MCP는 Claude 같은 AI 애플리케이션이 파일, DB, API, 사내 서비스 같은 외부 시스템을 매번 별도 통합하지 않고도 같은 방식으로 연결할 수 있게 해주는 표준 인터페이스다.

## 왜 필요한가

AI 앱이 외부 시스템을 쓰려면 보통 서비스마다 별도 연동 코드를 작성해야 한다. 예를 들어 파일 시스템, GitHub, Slack, 데이터베이스, 캘린더를 모두 붙이려면 각 서비스의 인증, API 호출, 응답 포맷, 에러 처리를 앱 안에 계속 추가해야 한다.

MCP는 이 부담을 MCP 서버 쪽으로 옮긴다. AI 애플리케이션은 MCP 클라이언트로 서버와 통신하고, 서버는 자신이 제공할 수 있는 도구와 데이터를 표준 형식으로 노출한다. 그래서 호스트 앱은 여러 서버를 같은 방식으로 발견하고 호출할 수 있다.

## 전체 구조

MCP에는 세 가지 참여자가 있다.

| 구성요소 | 역할 |
| --- | --- |
| MCP Host | 사용자가 실제로 쓰는 AI 애플리케이션. 예: Claude Desktop, Claude Code, IDE |
| MCP Client | Host 안에서 특정 MCP Server 하나와 연결을 유지하는 구성요소 |
| MCP Server | 외부 시스템의 기능이나 데이터를 MCP 형식으로 제공하는 프로그램 |

Host는 여러 MCP Server에 연결할 수 있고, 보통 서버 하나마다 클라이언트 하나를 만든다. 예를 들어 Claude Code가 파일 시스템 서버와 GitHub 서버에 동시에 연결되어 있다면, 내부적으로는 각 서버와 별도 MCP Client 연결을 가진다.

## 프로토콜 레이어

MCP는 크게 데이터 레이어와 전송 레이어로 나눌 수 있다.

| 레이어 | 핵심 |
| --- | --- |
| Data layer | JSON-RPC 2.0 기반 메시지, lifecycle, tools/resources/prompts 같은 primitive |
| Transport layer | 메시지를 실제로 주고받는 방식. 대표적으로 `stdio`, Streamable HTTP |

중요한 흐름은 초기화다. 클라이언트와 서버는 연결 초기에 프로토콜 버전, 지원 기능, 서버/클라이언트 정보를 교환한다. 이 과정을 통해 클라이언트는 서버가 tools, resources, prompts 중 무엇을 지원하는지 알 수 있다.

## 서버가 제공하는 세 가지 primitive

MCP 서버는 주로 tools, resources, prompts를 제공한다. CCA-F 시험 관점에서는 누가 제어하는지까지 같이 외우는 게 좋다.

| Primitive | 의미 | 제어 주체 | 예시 |
| --- | --- | --- | --- |
| Tools | 모델이 호출할 수 있는 실행 함수 | Model-controlled | 파일 수정, API 호출, DB 조회 |
| Resources | 읽기 중심의 컨텍스트 데이터 | Application-controlled | 파일 내용, DB schema, 문서, API 응답 |
| Prompts | 재사용 가능한 프롬프트 템플릿 | User-controlled | 문서 포맷팅, 회의 요약, 코드 리뷰 절차 |

## Tools

Tools는 모델이 실제 행동을 하기 위한 함수다. 서버는 `tools/list`로 사용할 수 있는 도구 목록과 입력 스키마를 제공하고, 클라이언트는 필요할 때 `tools/call`로 특정 도구를 실행한다.

Python SDK를 쓰면 JSON Schema를 직접 길게 쓰기보다 함수 타입 힌트와 설명을 통해 도구 정의를 만들 수 있다.

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("documents")

@mcp.tool()
def read_document(name: str) -> str:
    """Read a document by name."""
    return load_document(name)

if __name__ == "__main__":
    mcp.run(transport="stdio")
```

시험 포인트:

- Tool은 실행 가능한 기능이다.
- 모델이 상황을 보고 호출 여부를 결정한다.
- 입력은 schema로 검증 가능해야 한다.
- 파일 수정, 메시지 전송, DB 변경처럼 부작용이 있는 작업은 사용자 승인과 권한 제어가 중요하다.

## Resources

Resources는 모델에게 줄 컨텍스트 데이터다. 도구처럼 "실행"한다기보다 앱이 필요한 데이터를 읽어 모델 컨텍스트에 넣는 방식에 가깝다.

Resource는 URI로 식별된다. 고정된 URI를 갖는 direct resource도 있고, 파라미터가 들어가는 templated resource도 있다.

예시:

- `file:///docs/guide.md`
- `schema://database/customers`
- `docs://project/{document_name}`

시험 포인트:

- Resource는 주로 읽기 전용 데이터다.
- 앱이 어떤 resource를 읽어 모델에게 줄지 결정한다.
- `resources/list`, `resources/templates/list`, `resources/read` 흐름을 기억한다.
- MIME type을 함께 다루면 JSON, text, binary 성격을 구분할 수 있다.

## Prompts

Prompts는 서버가 제공하는 재사용 가능한 작업 템플릿이다. 사용자가 명시적으로 선택해서 실행하는 흐름에 가깝다.

예를 들어 문서 서버가 다음과 같은 prompt를 제공할 수 있다.

- 문서를 요약해줘
- 문서를 블로그 글 형식으로 바꿔줘
- 문서의 맞춤법과 톤을 검토해줘

시험 포인트:

- Prompt는 user-controlled다.
- `prompts/list`로 찾고 `prompts/get`으로 상세 내용을 가져온다.
- 반복되는 고품질 작업 지시를 서버 안에 넣어두는 방식이다.

## Client 구현 흐름

MCP Client는 단순히 서버에 요청을 보내는 코드가 아니라 Host와 Server 사이에서 capability를 관리하는 구성요소다.

기본 흐름:

1. 서버 프로세스를 시작하거나 원격 서버에 연결한다.
2. `initialize`로 프로토콜 버전과 capabilities를 협상한다.
3. `tools/list`, `resources/list`, `prompts/list`로 서버 기능을 발견한다.
4. Host가 사용자 요청과 서버 기능을 모델에게 전달한다.
5. 모델이 tool 호출을 선택하면 client가 `tools/call`을 수행한다.
6. 필요한 resource를 읽어 컨텍스트에 넣거나 prompt를 가져와 메시지로 변환한다.

강의에서는 MCP 서버뿐 아니라 클라이언트 구현도 다룬다. 즉 "MCP 서버를 만드는 법"만이 아니라 "Claude 같은 Host가 서버 기능을 어떻게 발견하고 사용하는가"까지 보는 과정이다.

## Server Inspector

MCP Server Inspector는 서버를 브라우저에서 테스트하고 디버깅하는 도구다. 클라이언트를 직접 구현하기 전에 서버가 어떤 tools/resources/prompts를 노출하는지 확인하고, tool 호출 결과를 점검할 수 있다.

시험 포인트:

- Inspector는 MCP 서버 개발용 디버깅 도구다.
- 브라우저 UI에서 서버 기능을 확인할 수 있다.
- 서버가 기대한 schema와 응답을 내는지 빠르게 검증할 때 쓴다.

## Tools, Resources, Prompts 선택 기준

헷갈릴 때는 "누가 언제 쓰는가"로 구분한다.

| 상황 | 선택 |
| --- | --- |
| 모델이 필요할 때 행동을 수행해야 한다 | Tool |
| 앱이 모델에게 참고 데이터를 넣어줘야 한다 | Resource |
| 사용자가 정해진 작업 템플릿을 실행하고 싶다 | Prompt |

예를 들어 문서 관리 MCP 서버를 만든다면:

- `read_document`, `edit_document`는 Tool
- 문서 목록, 문서 본문, 프로젝트 메타데이터는 Resource
- "블로그 스타일로 정리", "회의록 요약", "문서 포맷팅"은 Prompt

## 보안과 제어

MCP는 외부 시스템에 접근하는 구조라 권한 제어가 중요하다.

- Tool 호출은 실제 부작용을 만들 수 있으므로 사용자 승인 UI가 필요할 수 있다.
- Resource는 읽기 중심이어도 민감 정보가 포함될 수 있으므로 scope를 제한해야 한다.
- Client는 서버가 요청한 작업을 그대로 실행하기보다 Host의 정책, 사용자 승인, 권한 설정을 거쳐야 한다.
- `stdio` 서버에서 stdout에 로그를 쓰면 JSON-RPC 메시지를 망가뜨릴 수 있으므로 stderr나 logging을 써야 한다.

## 기억할 키워드

- MCP Host, MCP Client, MCP Server
- JSON-RPC 2.0
- Lifecycle initialization
- Capability negotiation
- `stdio`, Streamable HTTP
- Tools: model-controlled
- Resources: application-controlled
- Prompts: user-controlled
- MCP Server Inspector
- Python SDK, decorators, type hints

## 정리

MCP는 AI 앱이 외부 세계와 연결되는 방식을 표준화한다. 강의의 핵심은 Python SDK로 MCP 서버와 클라이언트를 직접 만들어보면서, tools/resources/prompts가 각각 어떤 책임을 갖는지 구분하는 것이다.

시험에서는 세부 코드보다 구조를 먼저 잡는 게 중요하다. Host-Client-Server 관계, 초기 capability 협상, 세 primitive의 제어 주체, 그리고 Inspector의 역할을 정확히 설명할 수 있으면 MCP 입문 범위는 대부분 정리된다.

## 참고 자료

- [Anthropic Academy - Introduction to Model Context Protocol](https://anthropic.skilljar.com/introduction-to-model-context-protocol/303756)
- [MCP 공식 문서 - What is MCP?](https://modelcontextprotocol.io/docs/getting-started/intro)
- [MCP 공식 문서 - Architecture overview](https://modelcontextprotocol.io/docs/learn/architecture)
- [MCP 공식 문서 - Understanding MCP servers](https://modelcontextprotocol.io/docs/learn/server-concepts)
- [MCP 공식 문서 - Understanding MCP clients](https://modelcontextprotocol.io/docs/learn/client-concepts)
- [MCP 공식 문서 - Build an MCP server](https://modelcontextprotocol.io/docs/develop/build-server)
