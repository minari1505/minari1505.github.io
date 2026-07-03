---
title: "Let's get started!"
title_ko: "시작하기"
course: model-context-protocol-advanced-topics
lesson: 1
---

## 학습 목표

- 이 심화 강의가 다루는 MCP 고급 주제의 큰 그림 잡기

## 이 강의가 다루는 것

MCP의 기본(client/server, tool·resource·prompt)을 넘어, 이 강의는 **핵심 고급 기능**과 **전송(transport) 계층**을 다룹니다.

**Core MCP features**

- **Sampling** — 서버가 클라이언트를 통해 Claude를 호출하기
- **Log & progress notifications** — 장기 작업 중 실시간 피드백
- **Roots** — 서버에 로컬 파일·폴더 접근 권한 부여

**Transports and communication**

- **JSON message types** — MCP 통신의 메시지 유형
- **STDIO transport** — 로컬 개발용 표준 입출력 전송
- **StreamableHTTP transport** — 원격 호스팅용 HTTP 전송과 그 제약
- **State** — stateless_http / json_response 플래그와 수평 확장

각 개념 레슨에는 실습 코드 walkthrough(다운로드 예제 포함)가 딸려 있어, 개념을 실제 구현으로 이어볼 수 있습니다.

## 핵심 정리

- 이 강의는 MCP의 고급 기능(Sampling·Notifications·Roots)과 전송 계층(STDIO·StreamableHTTP·State)을 다룹니다.
- 개념 → 코드 walkthrough 순으로, 이론과 구현을 함께 익힙니다.
