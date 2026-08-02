---
title: "Wrapping up"
title_ko: "정리"
course: model-context-protocol-advanced-topics
lesson: 10
---

## 학습 목표

- MCP 고급 주제 전체를 하나로 되짚기

## 되짚기

이 강의는 MCP의 기본을 넘어 **고급 기능**과 **전송 계층**을 다뤘습니다.

### Core MCP features

- **Sampling** — 서버가 클라이언트를 통해 Claude를 호출해, AI 연동 복잡도·비용을 클라이언트로 이전 (공개 서버에 특히 유용)
- **Log & progress notifications** — Context의 `info()`·`report_progress()`로 장기 작업의 실시간 피드백 제공 (선택 사항)
- **Roots** — 서버에 특정 로컬 파일·폴더 접근을 허가하되 경계로 보안 유지 (`is_path_allowed()` 직접 구현)

### Transports and communication

- **JSON message types** — Request-Result(응답 기대) vs Notification(응답 불필요), MCP는 양방향 프로토콜
- **STDIO transport** — 서브프로세스 + stdin/stdout, 양방향의 이상적 기준선(같은 머신 전용)
- **StreamableHTTP transport** — 원격 서버 연결을 가능하게 하지만 HTTP는 서버→클라이언트 요청이 어려움
- **StreamableHTTP 심화** — SSE와 session ID, dual SSE connection으로 우회
- **State** — 수평 확장 시 `stateless_http`·`json_response` 플래그의 필요성과 트레이드오프

## 핵심 정리

- 고급 기능(Sampling·Notifications·Roots)은 서버↔클라이언트의 양방향 통신 위에서 동작합니다.
- transport 선택(STDIO vs StreamableHTTP)과 상태 플래그가 어떤 기능을 쓸 수 있는지를 결정합니다.
- 서버→클라이언트 통신에 의존하는 기능(sampling·알림)은 stateless HTTP에서 제한되므로, 배포 시나리오에 맞게 transport를 신중히 고르세요.
