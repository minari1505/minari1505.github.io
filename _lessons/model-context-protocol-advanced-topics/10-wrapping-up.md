---
title: "Wrapping up"
title_ko: "정리"
course: model-context-protocol-advanced-topics
lesson: 10
---

## 학습 목표

- MCP 고급 주제 전체를 하나로 되짚기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>Core MCP features</strong>Sampling · Notifications · Roots</li>
  <li><strong>메시지와 방향</strong>Request-Result vs Notification, 양방향 프로토콜</li>
  <li><strong>Transport</strong>STDIO vs StreamableHTTP</li>
  <li><strong>State</strong>수평 확장과 <code>stateless_http</code>·<code>json_response</code></li>
</ol>

이 강의를 한 문장으로 줄이면: **고급 기능은 서버↔클라이언트 양방향 통신 위에서 동작하므로, transport와 상태 플래그 선택이 쓸 수 있는 기능을 결정합니다.**

## 되짚기

이 강의는 MCP의 기본을 넘어 **고급 기능**과 **전송 계층**을 다뤘습니다.

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면</span>
  <p>앞 절반은 집에 <b>어떤 가전을 들일지</b>(Sampling, Notifications, Roots)였고, 뒤 절반은 그 가전이 돌아가게 하는 <b>배선과 배관</b>(transport, state)이었습니다. 배선이 한쪽 방향만 지원하면, 양쪽으로 신호를 주고받아야 하는 가전은 켤 수 없습니다.</p>
</div>

### Core MCP features

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">Sampling</span>서버가 클라이언트를 통해 Claude를 호출해, AI 연동 복잡도·비용을 클라이언트로 이전 (공개 서버에 특히 유용)</div>
  <div class="oa-card"><span class="oa-card__icon">📊</span><span class="oa-card__title">Log &amp; progress notifications</span>Context의 <code>info()</code>·<code>report_progress()</code>로 장기 작업의 실시간 피드백 제공 (선택 사항)</div>
  <div class="oa-card"><span class="oa-card__icon">🔑</span><span class="oa-card__title">Roots</span>서버에 특정 로컬 파일·폴더 접근을 허가하되 경계로 보안 유지 (<code>is_path_allowed()</code> 직접 구현)</div>
</div>

### Transports and communication

| 주제 | 핵심 |
|---|---|
| **JSON message types** | Request-Result(응답 기대) vs Notification(응답 불필요), MCP는 양방향 프로토콜 |
| **STDIO transport** | 서브프로세스 + stdin/stdout, 양방향의 이상적 기준선(같은 머신 전용) |
| **StreamableHTTP transport** | 원격 서버 연결을 가능하게 하지만 HTTP는 서버→클라이언트 요청이 어려움 |
| **StreamableHTTP 심화** | SSE와 session ID, dual SSE connection으로 우회 |
| **State** | 수평 확장 시 `stateless_http`·`json_response` 플래그의 필요성과 트레이드오프 |

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">STDIO</span>
    <ul>
      <li>서브프로세스 + stdin/stdout</li>
      <li>양방향 통신의 이상적 기준선</li>
      <li>같은 머신 전용</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">StreamableHTTP</span>
    <ul>
      <li>원격 서버 연결 가능</li>
      <li>서버→클라이언트 요청이 어려워 SSE·session ID·dual SSE connection으로 우회</li>
      <li>수평 확장 시 상태 플래그의 트레이드오프를 따져야 함</li>
    </ul>
  </div>
</div>

## 기능별로 필요한 통신 방향

| 기능 | 서버 → 클라이언트 통신 필요 | stateless HTTP에서 |
|---|---|---|
| 도구 호출과 결과 | 아니오 | 동작 |
| Sampling | 예 | 사용 불가 |
| Progress·Logging 알림 | 예 | 사용 불가 |
| 리소스 업데이트 구독 | 예 | 사용 불가 |

> 서버→클라이언트 통신에 의존하는 기능을 쓸 계획이라면, 배포 시나리오에 맞게 transport와 상태 플래그를 먼저 정합니다.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 공개 MCP 서버가 모든 사용자의 AI 비용을 떠안지 않게 하는 기능은 무엇입니까?</summary>
  <p><b>정답: Sampling.</b> 서버가 클라이언트를 통해 Claude를 호출해 비용과 복잡도를 클라이언트로 옮깁니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. sampling과 실시간 알림에 의존하는 서버를 수평 확장하려 합니다. 무엇을 다시 검토해야 합니까?</summary>
  <p><b>정답: transport와 상태 플래그.</b> <code>stateless_http=True</code>에서는 서버→클라이언트 통신에 의존하는 기능이 제한됩니다.</p>
</details>

## 핵심 정리

- 고급 기능(Sampling·Notifications·Roots)은 서버↔클라이언트의 양방향 통신 위에서 동작합니다.
- transport 선택(STDIO vs StreamableHTTP)과 상태 플래그가 어떤 기능을 쓸 수 있는지를 결정합니다.
- 서버→클라이언트 통신에 의존하는 기능(sampling·알림)은 stateless HTTP에서 제한되므로, 배포 시나리오에 맞게 transport를 신중히 고르세요.
