---
title: "Introduction"
title_ko: "소개 — Claude와 모델 개요"
course: claude-with-the-anthropic-api
lesson: 1
---

## 학습 목표

- 이 강의가 무엇을 다루는지, 어떤 순서로 배우는지 파악하기
- Anthropic과 Claude가 무엇인지 이해하기
- Claude 모델 라인업(속도·지능·비용의 트레이드오프)을 구분하기

## 이 강의에서 배우는 것

**Claude API로 실제 애플리케이션을 만드는 법**을 처음부터 끝까지 다루는 개발자용 강의예요. 큰 흐름은 이렇게 흘러가요.

<ol class="oa-flow">
  <li><strong>요청 보내기</strong>API로 Claude에 요청 (기초)</li>
  <li><strong>평가·엔지니어링</strong>프롬프트를 eval하고 다듬기</li>
  <li><strong>도구 사용</strong>tool use로 능력 부여</li>
  <li><strong>RAG</strong>외부 지식 연결</li>
  <li><strong>고급 기능</strong>thinking, 이미지·PDF, 인용, 캐싱, 코드 실행</li>
  <li><strong>MCP</strong>도구/자원/프롬프트 표준화</li>
  <li><strong>에이전트·워크플로우</strong>설계하기</li>
</ol>

## Anthropic과 Claude

- **Anthropic**은 AI 안전성을 중심에 두고 Claude를 만드는 회사예요.
- **Claude**는 대규모 언어 모델(LLM)로, 텍스트·이미지·문서를 입력받아 텍스트를 생성해요. API를 통해 애플리케이션에 통합할 수 있어요.
- 모든 상호작용은 하나의 엔드포인트 **`POST /v1/messages`** (Messages API)로 이뤄져요. 도구 사용, 구조화 출력 같은 기능도 전부 이 엔드포인트의 옵션이에요 — 별도 API가 아니라는 게 포인트!

## Claude 모델 라인업

<div class="oa-eli">
  <span class="oa-eli__head">🚚 쉽게 말하면</span>
  <p>배송 수단을 고르는 것과 같아요. <b>Opus</b>는 무겁고 까다로운 짐을 맡기는 특수 운송, <b>Sonnet</b>은 대부분의 짐에 쓰는 일반 택배, <b>Haiku</b>는 작은 물건을 빠르고 싸게 보내는 퀵서비스예요. 모든 짐을 특수 운송으로 보낼 필요는 없어요.</p>
</div>

모델은 **지능 ↔ 속도 ↔ 비용**의 트레이드오프로 나뉘어요. 작업에 맞는 모델을 고르는 게 핵심이에요.

| 계열 | 성격 | 이럴 때 |
|---|---|---|
| **Opus** | 가장 지능이 높음 | 복잡한 추론, 장기 에이전트 작업, 어려운 코딩 |
| **Sonnet** | 지능과 속도의 균형 | 대부분의 프로덕션 워크로드 |
| **Haiku** | 가장 빠르고 저렴 | 분류, 단순·대량 작업, 지연이 중요한 경우 |

- 모델은 **정확한 모델 ID 문자열**로 지정해요 (예: `claude-opus-4-8`, `claude-sonnet-...`, `claude-haiku-...`). ID를 추측하면 404가 나요.
- 각 모델의 컨텍스트 윈도우·최대 출력·가격은 다르며, **Models API**(`GET /v1/models`)로 실시간 조회할 수 있어요.
- 비용은 **입력/출력 토큰 100만 개당 달러**로 매겨지고, 출력이 입력보다 비싸요.

> 💡 이 강의의 코드 예시는 대부분 **Python SDK**(`anthropic` 패키지)를 씁니다. TypeScript 등 다른 SDK도 동일한 개념이에요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 도구 사용이나 구조화 출력을 쓰려면 Messages API와 다른 별도 API를 호출해야 하나요?</summary>
  <p><b>정답: 아니에요.</b> 모든 상호작용은 <code>POST /v1/messages</code> 하나로 이뤄지고, 이런 기능은 그 엔드포인트의 옵션이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 대량의 단순 분류 작업에서 지연과 비용이 중요해요. 어느 계열이 맞나요?</summary>
  <p><b>정답: Haiku.</b> 가장 빠르고 저렴해요. 복잡한 추론은 Opus, 대부분의 프로덕션 워크로드는 Sonnet이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 모델 ID를 기억나는 대로 적어 요청했더니 404가 났어요. 어떻게 확인하나요?</summary>
  <p><b>정답: 정확한 모델 ID 문자열을 써야 해요.</b> Models API(<code>GET /v1/models</code>)로 조회할 수 있어요.</p>
</details>

## 정리

- 이 강의는 요청 → 평가/엔지니어링 → 도구 → RAG → 기능 → MCP → 에이전트 순으로 진행돼요.
- 모든 것은 `POST /v1/messages` 하나로 통해요.
- 모델은 Opus(지능) / Sonnet(균형) / Haiku(속도)로 나뉘고, 작업에 맞게 고르는 게 첫 번째 결정이에요.
