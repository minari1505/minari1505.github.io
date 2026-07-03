---
title: "StreamableHTTP in depth"
title_ko: "StreamableHTTP 심화"
course: model-context-protocol-advanced-topics
lesson: 8
---

## 학습 목표

- StreamableHTTP가 Server-Sent Events(SSE)로 HTTP 제약을 어떻게 우회하는지 이해하기
- session ID와 dual SSE connection 모델 파악하기
- 어떤 메시지가 어느 연결로 라우팅되는지 알기

## 핵심 문제

Sampling·notification·logging 같은 일부 MCP 기능은 **서버가 클라이언트에 요청을 시작**해야 하는데, HTTP는 클라이언트→서버 요청용으로 설계됐습니다. StreamableHTTP는 **Server-Sent Events(SSE)**를 이용한 영리한 우회로 이를 해결합니다.

## 초기 연결 설정

MCP 연결처럼 시작합니다.

1. 클라이언트가 **Initialize Request** 전송
2. 서버가 특별한 **`mcp-session-id` 헤더**를 포함한 **Initialize Result**로 응답
3. 클라이언트가 그 session ID로 **Initialized Notification** 전송

이 **session ID는 클라이언트를 고유하게 식별**하며, 이후 모든 요청에 포함돼야 합니다.

## SSE 우회

초기화 후, 클라이언트가 **GET 요청**으로 SSE 연결을 엽니다. 이는 서버가 언제든 메시지를 스트리밍할 수 있는 **오래 유지되는 HTTP 응답**을 만듭니다. 이 SSE 연결이 **서버→클라이언트 통신**의 핵심입니다.

![StreamableHTTP SSE — session id를 가진 기본 SSE 연결(서버→클라이언트용)과 도구 호출별 SSE 연결](/assets/images/courses/model-context-protocol-advanced-topics/streamablehttp-in-depth.png)

## Dual SSE Connection

도구를 호출하면 **두 개의 별도 SSE 연결**이 생깁니다.

- **Primary SSE Connection** — 서버 시작 요청용, 무기한 열려 있음
- **Tool-Specific SSE Connection** — 각 도구 호출마다 생성되고, 도구 결과가 전송되면 자동으로 닫힘

### 메시지 라우팅

- **Progress notification** → Primary SSE connection
- **Logging 메시지·도구 결과** → Tool-specific SSE connection

## 우회를 깨는 플래그

- **`stateless_http`**, **`json_response`** 를 `True`로 설정하면 SSE 우회 메커니즘이 깨집니다. 특정 시나리오에서 켤 수 있지만, 서버→클라이언트 통신에 의존하는 전체 MCP 기능이 제한됩니다.

## 핵심 정리

- StreamableHTTP는 HTTP 제약을 SSE로 우회해, 서버→클라이언트 통신을 가능하게 합니다.
- 초기화 후 받은 **session ID는 이후 모든 요청에 필수**입니다.
- 시스템은 **primary SSE(서버 시작 요청)**와 **tool-specific SSE(도구별)** 두 연결을 자동 관리합니다.
