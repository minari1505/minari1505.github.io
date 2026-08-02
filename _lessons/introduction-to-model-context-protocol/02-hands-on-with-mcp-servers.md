---
title: "Hands-on with MCP servers"
title_ko: "MCP 서버 실습"
course: introduction-to-model-context-protocol
lesson: 2
---

## 학습 목표

- Python MCP SDK로 MCP server를 만드는 기본 흐름 익히기
- decorator 기반으로 MCP tool을 정의하는 방법 이해하기
- `Field` 설명과 type hint가 tool schema 생성에 어떻게 쓰이는지 파악하기
- MCP Inspector로 server tool을 직접 테스트하는 개발 흐름 정리하기

## MCP server를 직접 만들어보기

MCP server를 직접 구현한다고 하면 처음에는 복잡해 보입니다. Claude가 이해할 tool schema를 만들고, tool call을 받아 실행하고, 결과를 다시 돌려줘야 하기 때문입니다.

하지만 공식 Python MCP SDK를 쓰면 이 과정이 훨씬 단순해집니다. JSON schema를 손으로 작성하는 대신, Python 함수와 decorator로 tool을 정의하면 SDK가 필요한 schema와 protocol 처리를 대신 맡아줍니다.

![MCP tool 정의 예시](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849221%2F09_-_004_-_Defining_Tools_with_MCP_00.1749849221750.png)

이번 예시는 간단한 문서 관리 서버입니다.

- 문서들은 메모리 안의 dictionary에 저장
- 문서 ID를 받아 내용을 읽는 tool 제공
- 문서 ID와 문자열을 받아 내용을 수정하는 tool 제공

## MCP server 초기화

Python MCP SDK에서는 `FastMCP`로 서버를 만들 수 있습니다.

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("DocumentMCP", log_level="ERROR")
```

예제 문서는 간단한 dictionary로 둡니다.

```python
docs = {
    "deposition.md": "This deposition covers the testimony of Angela Smith, P.E.",
    "report.pdf": "The report details the state of a 20m condenser tower.",
    "financials.docx": "These financials outline the project's budget and expenditures",
    "outlook.pdf": "This document presents the projected future performance of the system",
    "plan.md": "The plan outlines the steps for the project's implementation.",
    "spec.txt": "These specifications define the technical requirements for the equipment"
}
```

실제 서비스라면 DB, 파일시스템, SaaS API를 연결하겠지만, 실습에서는 MCP server 구조를 보기 위해 메모리 dictionary만 사용합니다.

## decorator로 tool 정의하기

SDK의 핵심은 `@mcp.tool` decorator입니다.

이 decorator를 함수 위에 붙이면 해당 함수가 MCP tool로 등록됩니다. 함수 이름, parameter type hint, `Field` 설명을 바탕으로 SDK가 Claude에게 전달할 tool schema를 자동으로 만듭니다.

### 문서 읽기 tool

첫 번째 tool은 문서 ID를 받아 문서 내용을 문자열로 반환합니다.

```python
from pydantic import Field

@mcp.tool(
    name="read_doc_contents",
    description="Read the contents of a document and return it as a string."
)
def read_document(
    doc_id: str = Field(description="Id of the document to read")
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")

    return docs[doc_id]
```

여기서 중요한 부분은 세 가지입니다.

- `name`: Claude와 client가 보게 될 tool 이름
- `description`: tool이 언제, 무엇을 위해 쓰이는지 설명
- `Field(description=...)`: parameter가 어떤 값인지 설명

`doc_id`가 존재하지 않으면 `ValueError`를 던집니다. 이렇게 명확한 error를 주면 Claude나 client가 실패 원인을 이해하기 쉽습니다.

## 문서 수정 tool 만들기

두 번째 tool은 단순한 find-and-replace 방식으로 문서를 수정합니다.

```python
@mcp.tool(
    name="edit_document",
    description="Edit a document by replacing a string in the documents content with a new string."
)
def edit_document(
    doc_id: str = Field(description="Id of the document that will be edited"),
    old_str: str = Field(description="The text to replace. Must match exactly, including whitespace."),
    new_str: str = Field(description="The new text to insert in place of the old text.")
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")

    docs[doc_id] = docs[doc_id].replace(old_str, new_str)
```

이 tool은 세 값을 받습니다.

| 인자 | 의미 |
|---|---|
| `doc_id` | 수정할 문서 ID |
| `old_str` | 찾아서 바꿀 기존 문자열. 공백까지 정확히 일치해야 함 |
| `new_str` | 기존 문자열 대신 넣을 새 문자열 |

실무에서는 `old_str`이 문서 안에 실제로 존재하는지 확인하고, 여러 번 replace될 가능성도 제어해야 합니다. 하지만 MCP server 구조를 익히는 실습에서는 기본적인 replace만으로 충분합니다.

## SDK 방식의 장점

Python MCP SDK를 쓰면 다음 작업을 직접 구현하지 않아도 됩니다.

- tool schema를 JSON으로 손수 작성
- parameter type과 설명을 별도 schema로 중복 관리
- MCP protocol message를 직접 파싱
- tool 등록과 목록 제공 로직 구현

대신 Python 함수 하나에 type hint와 설명을 붙이면 됩니다.

```text
Python 함수
  ↓
@mcp.tool decorator
  ↓
SDK가 MCP tool schema 생성
  ↓
MCP client가 tool list 조회
  ↓
Claude가 tool use로 호출
```

이 방식은 tool이 많아질수록 유지보수 차이가 커집니다. 함수의 parameter와 설명이 실제 구현 옆에 있기 때문에, schema와 코드가 따로 어긋날 가능성이 줄어듭니다.

## MCP Inspector로 테스트하기

MCP server를 만들었다면 바로 Claude나 전체 애플리케이션에 붙이기 전에 server만 따로 테스트하는 편이 좋습니다. Python MCP SDK에는 browser 기반의 **MCP Inspector**가 포함되어 있습니다.

터미널에서 다음 명령을 실행합니다.

```bash
mcp dev mcp_server.py
```

그러면 개발 서버가 실행되고, 보통 다음과 비슷한 로컬 URL이 표시됩니다.

```text
http://127.0.0.1:6274
```

브라우저에서 이 URL을 열면 MCP Inspector를 사용할 수 있습니다. UI는 계속 개발 중이라 화면이 조금씩 달라질 수 있지만, 핵심 흐름은 같습니다.

1. `Connect` 버튼으로 MCP server에 연결
2. `Tools` 탭으로 이동
3. `List Tools`를 눌러 server가 제공하는 tool 확인
4. 원하는 tool을 선택해 입력값을 넣고 실행
5. 성공 여부와 반환 값을 확인

![MCP Inspector에서 tool 테스트](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849212%2F09_-_005_-_The_Server_Inspector_10.1749849212534.png)

예를 들어 `read_doc_contents` tool을 테스트하려면 다음처럼 하면 됩니다.

1. `read_doc_contents` tool 선택
2. `doc_id`에 `deposition.md` 입력
3. `Run Tool` 클릭
4. 반환된 문서 내용 확인

## tool 상호작용 테스트

Inspector는 단일 tool만 확인하는 용도가 아닙니다. server 상태가 유지되기 때문에 여러 tool을 순서대로 실행해 상호작용을 확인할 수 있습니다.

예를 들어 다음 흐름을 테스트할 수 있습니다.

1. `read_doc_contents`로 `plan.md`의 현재 내용 확인
2. `edit_document`로 특정 문자열 수정
3. 다시 `read_doc_contents`를 실행해 수정 결과 확인

이렇게 하면 전체 애플리케이션을 연결하지 않고도 MCP server의 핵심 동작을 빠르게 검증할 수 있습니다.

## 개발 흐름

MCP server를 만들 때는 다음 루프가 효율적입니다.

```text
tool 함수 작성
  ↓
mcp dev mcp_server.py 실행
  ↓
Inspector에서 List Tools 확인
  ↓
정상 입력과 edge case 테스트
  ↓
필요하면 함수와 설명 수정
```

특히 다음을 Inspector에서 먼저 확인하는 것이 좋습니다.

- 존재하는 문서 ID를 넣었을 때 정상 반환되는가?
- 없는 문서 ID를 넣었을 때 명확한 error가 나는가?
- 수정 tool이 실제로 상태를 바꾸는가?
- `old_str`가 정확히 일치하지 않을 때 어떻게 동작하는가?
- tool 설명과 parameter 설명이 Claude가 이해하기 충분한가?

## 핵심 정리

MCP Python SDK는 MCP server 구현을 "protocol 작성"이 아니라 "Python 함수 작성"에 가깝게 만들어줍니다.

- `FastMCP`로 server를 초기화합니다.
- `@mcp.tool`로 Python 함수를 MCP tool로 등록합니다.
- type hint와 `Field` 설명이 tool schema 생성에 쓰입니다.
- error는 Python exception으로 자연스럽게 표현할 수 있습니다.
- MCP Inspector로 Claude나 전체 앱 없이도 tool을 테스트할 수 있습니다.

MCP server 개발의 핵심은 좋은 tool 함수를 만들고, Inspector로 빠르게 검증하는 것입니다. 이 루프가 잡히면 외부 API, DB, 파일시스템 같은 실제 시스템도 같은 패턴으로 MCP server에 연결할 수 있습니다.
