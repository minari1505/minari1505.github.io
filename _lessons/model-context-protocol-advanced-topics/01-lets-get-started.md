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

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">Sampling</span>서버가 클라이언트를 통해 Claude를 호출하기</div>
  <div class="oa-card"><span class="oa-card__icon">📊</span><span class="oa-card__title">Log & progress notifications</span>장기 작업 중 실시간 피드백</div>
  <div class="oa-card"><span class="oa-card__icon">📂</span><span class="oa-card__title">Roots</span>서버에 로컬 파일·폴더 접근 권한 부여</div>
</div>

**Transports and communication**

<ol class="oa-flow">
  <li><strong>JSON message types</strong>MCP 통신의 메시지 유형</li>
  <li><strong>STDIO transport</strong>로컬 개발용 표준 입출력 전송</li>
  <li><strong>StreamableHTTP transport</strong>원격 호스팅용 HTTP 전송과 그 제약</li>
  <li><strong>State</strong>stateless_http / json_response 플래그와 수평 확장</li>
</ol>

각 개념 레슨에는 실습 코드 walkthrough(다운로드 예제 포함)가 딸려 있어, 개념을 실제 구현으로 이어볼 수 있습니다.

<div class="oa-eli">
  <span class="oa-eli__head">🧭 쉽게 말하면</span>
  <p>앞의 세 가지는 <b>"서버가 할 수 있는 일을 넓히는 기능"</b>이고, 뒤의 네 가지는 <b>"메시지가 오가는 길"</b>에 관한 이야기입니다. 길(transport)을 어떻게 고르느냐에 따라 앞의 기능을 쓸 수 있는지가 정해집니다.</p>
</div>

## 핵심 정리

- 이 강의는 MCP의 고급 기능(Sampling·Notifications·Roots)과 전송 계층(STDIO·StreamableHTTP·State)을 다룹니다.
- 개념 → 코드 walkthrough 순으로, 이론과 구현을 함께 익힙니다.
