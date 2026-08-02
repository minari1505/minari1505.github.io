---
title: "Introduction"
title_ko: "소개"
course: introduction-to-model-context-protocol
lesson: 1
---

## 학습 목표

- MCP가 어떤 문제를 해결하는지 이해하기
- MCP client와 MCP server의 역할 구분하기
- MCP가 tool use와 어떻게 다르고, 어떻게 함께 쓰이는지 정리하기
- tool discovery와 tool execution이 실제 요청 흐름에서 어떻게 연결되는지 파악하기

## MCP란?

**Model Context Protocol(MCP)**은 Claude 같은 모델 기반 애플리케이션이 외부 서비스의 **맥락(context), 도구(tools), 프롬프트(prompts), 자원(resources)**을 표준 방식으로 연결할 수 있게 해주는 통신 계층입니다.

한 문장으로 줄이면, MCP는 외부 시스템 통합 부담을 내 서버에서 덜어내고, 전문화된 **MCP server** 쪽으로 옮기는 방식입니다.

![MCP 기본 구조](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849216%2F09_-_001_-_Introducing_MCP_01.1749849216723.png)

기본 구조는 다음과 같습니다.

```text
내 애플리케이션 / 서버
  ↓ MCP Client
MCP Server
  ↓
GitHub, DB, 파일시스템, 사내 API, SaaS ...
```

여기서 MCP server는 특정 외부 서비스와 연결되는 인터페이스 역할을 합니다. 예를 들어 GitHub MCP server라면 repository, pull request, issue 같은 GitHub 기능을 도구로 노출합니다.

## MCP가 해결하는 문제

예를 들어 사용자가 챗 인터페이스에서 이렇게 묻는다고 해봅시다.

> "내 모든 저장소에서 열려 있는 pull request를 알려줘."

Claude가 이 질문에 답하려면 GitHub API에 접근할 수 있어야 합니다. MCP가 없다면 우리 서버가 GitHub API를 호출하는 함수와 Claude에게 넘길 tool schema를 직접 만들어야 합니다.

![GitHub 데이터를 묻는 사용자 요청](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849217%2F09_-_001_-_Introducing_MCP_03.1749849217761.png)

문제는 GitHub 같은 서비스의 기능 범위가 매우 넓다는 점입니다.

- 저장소 목록 조회
- pull request 조회
- issue 생성/수정
- branch, commit, review, project 조회
- 권한, 인증, pagination, rate limit 처리

이 모든 기능에 대해 tool schema와 실행 함수를 직접 만들고 테스트하고 유지보수하면 통합 코드가 빠르게 커집니다.

![직접 통합할 때 늘어나는 구현 부담](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849218%2F09_-_001_-_Introducing_MCP_05.1749849218279.png)

MCP는 이 부담을 줄입니다. GitHub MCP server가 GitHub와 관련된 도구 정의와 실행을 맡고, 내 애플리케이션은 MCP client를 통해 그 server와 통신합니다.

![MCP server가 통합 부담을 맡는 구조](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849219%2F09_-_001_-_Introducing_MCP_08.1749849219623.png)

## MCP Server의 역할

MCP server는 외부 서비스의 데이터나 기능을 MCP 규격에 맞춰 제공합니다.

![MCP server의 역할](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849221%2F09_-_001_-_Introducing_MCP_10.1749849221341.png)

예를 들어 GitHub MCP server는 내부적으로 GitHub API를 호출하지만, Claude나 내 애플리케이션에는 표준화된 MCP 도구처럼 보입니다.

```text
Claude
  ↓ tool use
내 서버
  ↓ MCP Client
GitHub MCP Server
  ↓ GitHub API
GitHub 데이터
```

중요한 점은 MCP server를 꼭 내가 직접 만들어야 하는 것은 아니라는 겁니다. 누구나 MCP server를 만들 수 있고, 서비스 제공자가 공식 MCP server를 제공할 수도 있습니다. 이 경우 개발자는 외부 API의 세부 구현을 직접 감싸지 않고도 Claude에게 해당 서비스의 기능을 연결할 수 있습니다.

## MCP와 tool use는 같은 것인가?

MCP와 tool use는 연결되어 있지만 같은 개념은 아닙니다.

| 구분 | 역할 |
|---|---|
| **Tool use** | Claude가 어떤 도구를 호출할지 판단하고, tool call을 생성하는 모델-API 기능 |
| **MCP** | 도구, 자원, 프롬프트를 외부 server에서 표준 방식으로 제공하는 통신 규격 |

Tool use만 사용하면 보통 우리 애플리케이션 코드 안에 tool schema와 실행 함수를 직접 둡니다.

MCP를 사용하면 그 tool schema와 실행 구현이 MCP server 쪽에 있습니다. 내 애플리케이션은 MCP client를 통해 "어떤 도구가 있는지" 묻고, Claude가 호출한 도구를 MCP server에 실행 요청합니다.

즉, tool use는 **Claude가 도구를 사용하는 방식**이고, MCP는 **도구를 어디서 어떻게 공급받고 실행할지에 대한 표준 연결 방식**입니다.

## MCP Client의 역할

MCP client는 내 애플리케이션과 MCP server 사이의 통신 다리입니다. 내 서버는 MCP client를 통해 MCP server가 제공하는 도구 목록을 가져오고, 특정 도구 실행을 요청합니다.

![MCP client와 server 통신](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849223%2F09_-_002_-_MCP_Clients_01.1749849223245.png)

MCP는 특정 transport에 묶이지 않습니다. 흔한 구성은 같은 머신에서 MCP client와 MCP server가 `stdio`로 통신하는 방식이지만, 상황에 따라 HTTP, WebSocket 같은 다른 transport도 사용할 수 있습니다.

![MCP transport 방식](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849224%2F09_-_002_-_MCP_Clients_03.1749849223875.png)

## MCP의 주요 메시지 흐름

MCP client와 server는 MCP 사양에 정의된 메시지를 주고받습니다. 입문 단계에서 가장 중요한 것은 두 가지입니다.

### 1. ListToolsRequest / ListToolsResult

내 서버가 Claude에게 도구를 알려주려면 먼저 MCP server가 어떤 도구를 제공하는지 알아야 합니다.

![ListToolsRequest와 ListToolsResult](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849224%2F09_-_002_-_MCP_Clients_05.1749849224367.png)

```text
MCP Client → MCP Server: 어떤 tool을 제공하나요?
MCP Server → MCP Client: 이런 tool들이 있습니다.
```

이 결과를 바탕으로 내 애플리케이션은 Claude에게 사용 가능한 도구 목록을 전달할 수 있습니다.

### 2. CallToolRequest / CallToolResult

Claude가 특정 도구를 호출하겠다고 판단하면, 내 서버는 MCP client를 통해 MCP server에 실제 실행을 요청합니다.

![CallToolRequest와 CallToolResult](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849225%2F09_-_002_-_MCP_Clients_07.1749849224854.png)

```text
MCP Client → MCP Server: 이 tool을 이 인자로 실행해 주세요.
MCP Server → 외부 서비스: 실제 API 호출 또는 작업 수행
MCP Server → MCP Client: 실행 결과 반환
```

## 전체 요청 흐름

사용자가 "내 GitHub 저장소가 뭐가 있지?"라고 묻는 상황을 보면 MCP 흐름이 더 분명해집니다.

![MCP client 전체 요청 흐름](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849232%2F09_-_002_-_MCP_Clients_19.1749849231568.png)

1. 사용자가 내 서버에 질문을 보냅니다.
2. 내 서버는 MCP client를 통해 MCP server의 사용 가능한 도구를 조회합니다.
3. MCP server는 `ListToolsResult`로 도구 목록을 반환합니다.
4. 내 서버는 사용자 질문과 도구 목록을 Claude에게 보냅니다.
5. Claude는 질문에 답하려면 GitHub 도구가 필요하다고 판단합니다.
6. Claude가 tool call을 생성합니다.
7. 내 서버는 MCP client를 통해 MCP server에 `CallToolRequest`를 보냅니다.
8. MCP server는 GitHub API를 호출합니다.
9. GitHub 응답이 MCP server와 client를 거쳐 내 서버로 돌아옵니다.
10. 내 서버는 tool result를 Claude에게 전달합니다.
11. Claude는 GitHub 데이터를 바탕으로 최종 답변을 만듭니다.

단계가 많아 보이지만, 각 컴포넌트의 책임은 명확합니다.

- 내 애플리케이션: 사용자 요청, Claude 호출, 대화 흐름 관리
- MCP client: MCP server와의 통신 담당
- MCP server: 외부 서비스별 도구 정의와 실행 담당
- Claude: 어떤 도구를 쓸지 판단하고 최종 응답 생성

## 핵심 정리

MCP는 외부 서비스 통합을 매번 직접 구현하지 않기 위한 표준 연결 방식입니다.

- MCP server는 도구, 자원, 프롬프트를 제공합니다.
- MCP client는 내 애플리케이션과 MCP server 사이에서 메시지를 주고받습니다.
- Claude는 tool use를 통해 MCP server가 제공한 도구를 호출할 수 있습니다.
- MCP는 tool use를 대체하는 것이 아니라, tool use에 필요한 도구 공급과 실행을 표준화합니다.

결국 MCP의 가치는 통합 코드를 줄이는 데 있습니다. GitHub, DB, 파일시스템, SaaS 같은 외부 시스템을 Claude에 연결할 때, 매번 tool schema와 실행 함수를 직접 만들지 않고 MCP server를 재사용할 수 있게 해줍니다.
