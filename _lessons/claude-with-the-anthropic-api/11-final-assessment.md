---
title: "Final Assessment & Wrap Up"
title_ko: "마무리 & 정리"
course: claude-with-the-anthropic-api
lesson: 11
---

## 학습 목표

- 강의 전체를 한눈에 복습하기
- 배운 개념들이 어떻게 연결되는지 정리하기

## 강의 전체 요약 🗺️

이 강의는 Claude API로 **단순 호출에서 시작해 에이전트까지** 쌓아 올리는 여정이었어요.

| 모듈 | 핵심 한 줄 |
|---|---|
| 소개 | 모든 건 `POST /v1/messages` 하나로. 모델은 Opus/Sonnet/Haiku로 구분 |
| API 접근 | 키는 환경 변수, 멀티턴은 전체 기록 재전송, 시스템 프롬프트·스트리밍·구조화 출력 |
| 프롬프트 평가 | 개선을 **측정**하라 — 코드 기반 vs 모델 기반 채점 |
| 프롬프트 엔지니어링 | 명확·구체·XML 태그·예시(few-shot) |
| 도구 사용 | Claude가 요청, 내 코드가 실행, `tool_result`로 반환 |
| RAG | 관련 문서를 검색해 프롬프트에 삽입 — 의미+어휘 하이브리드 |
| Claude 기능 | thinking·이미지·PDF·인용·캐싱·코드 실행 |
| MCP | 도구·자원·프롬프트를 잇는 표준 프로토콜 |
| Claude Code & Computer Use | 도구+에이전틱 루프가 실제 제품이 된 사례 |
| 에이전트 & 워크플로우 | 워크플로우(내 코드 제어) vs 에이전트(Claude 제어) |

## 개념들이 연결되는 방식

```
단일 호출 (messages.create)
   │  프롬프트 엔지니어링으로 품질 ↑ / eval로 검증
   ▼
+ 도구 사용 → Claude에 외부 능력 부여
   │
   ├─ + RAG → 외부 지식 연결
   ├─ + MCP → 도구/자원/프롬프트 표준화
   ▼
워크플로우 (내 코드가 여러 호출을 조율)
   ▼
에이전트 (Claude가 스스로 도구를 써서 목표 달성)
```

<ol class="oa-flow">
  <li><strong>단일 호출</strong>프롬프트 엔지니어링 + eval</li>
  <li><strong>도구 사용</strong>외부 능력 (+ RAG, MCP)</li>
  <li><strong>워크플로우</strong>내 코드가 조율</li>
  <li><strong>에이전트</strong>Claude가 조율</li>
</ol>

<div class="oa-eli">
  <span class="oa-eli__head">🪜 쉽게 말하면</span>
  <p>사다리를 한 칸씩 오르는 것과 같아요. 아래 칸(단일 호출)으로 해결되면 거기서 멈춰요. 위 칸으로 갈수록 할 수 있는 일은 늘지만 비용과 불확실성도 함께 늘어나요.</p>
</div>

## 실전 체크리스트

- [ ] 작업에 맞는 **모델** 선택 (Opus/Sonnet/Haiku)
- [ ] 프롬프트를 **명확·구체**하게, 필요하면 XML 태그·예시
- [ ] 변경은 **eval로 측정**하며 반복
- [ ] 긴 출력은 **스트리밍**, 형식이 필요하면 **구조화 출력**
- [ ] 반복되는 큰 컨텍스트는 **프롬프트 캐싱**
- [ ] 외부 능력은 **도구 사용**, 외부 지식은 **RAG**
- [ ] 복잡도는 **단일 호출 → 워크플로우 → 에이전트** 순으로만 올리기

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 새 기능을 만들 때 복잡도는 어떤 순서로 올리나요?</summary>
  <p><b>정답: 단일 호출 → 워크플로우 → 에이전트.</b> 아래 단계로 충분하면 올리지 않아요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 외부 능력과 외부 지식은 각각 무엇으로 연결하나요?</summary>
  <p><b>정답: 외부 능력은 도구 사용, 외부 지식은 RAG.</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 프롬프트를 고쳤어요. 정말 나아졌는지 어떻게 확인하나요?</summary>
  <p><b>정답: eval로 측정해요.</b> 느낌이 아니라 테스트셋 점수로 이전 버전과 비교해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 같은 긴 문서를 두고 요청을 반복해요. 비용과 속도를 줄이려면 무엇을 쓰나요?</summary>
  <p><b>정답: 프롬프트 캐싱.</b></p>
</details>

## 최종 평가 (Final Assessment)

강의 마지막엔 배운 내용을 확인하는 **최종 평가**가 있어요. 각 모듈의 퀴즈를 복습하고, 위 체크리스트를 스스로 설명할 수 있으면 준비 완료예요.

---

🎉 **Building with the Claude API** 강의 정리 끝! 이제 Claude API로 나만의 애플리케이션을 만들 준비가 됐어요.
