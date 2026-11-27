---
title: "Steerability"
title_ko: "조종 가능성 (Steerability)"
course: ai-capabilities-and-limitations
lesson: 7
---

## 학습 목표

- steerability가 왜 작동하는지(fine-tuning이 지시 따르기를 가르침)와 왜 한계가 있는지(지시를 이해가 아니라 패턴 매칭으로 따름) 설명하기
- 통제가 가장 강한 곳(짧고 구체적이고 검증 가능한 지시) vs 가장 약한 곳(긴 추론 사슬, 추상적 요청, 네이티브 정밀도) 예측하기
- reasoning drift, letter-over-spirit, 취약한 산술을 특징적 실패로 식별하기
- 이 한계를 다루는 제품 기능(system prompt, code execution, 가시적 추론, 구조화 출력) 인식하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>지시도 패턴 이어가기</strong>이해가 아니라 패턴 매칭으로 따름</li>
  <li><strong>강한 통제</strong>짧고 구체적이고 검증 가능한 지시</li>
  <li><strong>간극</strong>의도한 것 ≠ 도착한 것</li>
  <li><strong>실패</strong>reasoning drift · letter over spirit · prompt injection</li>
  <li><strong>대응</strong>목표 다시 진술 · 체크포인트 · code execution</li>
</ol>

이 레슨을 한 문장으로 줄이면: **지시가 문자 그대로 지켜졌는데 쓸모없다면, 더 세게 반복하지 말고 목표를 다시 말해요.**

## 1. 얼마나 통제하고 있는가

<div class="oa-eli">
  <span class="oa-eli__head">🧞 쉽게 말하면</span>
  <p>소원을 <b>글자 그대로</b> 들어주는 램프의 요정을 떠올려 보세요. "부자가 되게 해 줘"라고 했더니 장난감 돈을 산더미처럼 주는 식이죠. 말은 지켰지만 뜻은 놓쳤어요. 그래서 <b>무엇을 원하는지</b>까지 말해야 해요.</p>
</div>

모델은 다른 모든 것과 같은 방식 — **패턴 이어가기** — 으로 지시를 따라요. 그래서 놀랍도록 잘 조종되지만, **의도한 것과 도착한 것 사이에는 항상 간극**이 있고, 흥미로운 실패 대부분이 그 간극에 살아요.

| | 설명 |
|---|---|
| **왜 작동하나** | fine-tuning이 모델에게 지시 따르기를 가르쳤어요 |
| **왜 한계가 있나** | 지시를 이해해서가 아니라 패턴 매칭으로 따라요. 즉, steerability는 Next Token Prediction으로 지시를 따르는 것이에요 |

{% include capability-spectrum.html title="Steerability" subtitle="How much am I in control?" %}

> **읽기 전 질문 (강의의 Before you read)**: AI에게 "정확히 100단어, 그 이상은 안 돼"라고 글을 시켰어요. 결과를 얼마나 꼼꼼히 확인해야 할까요? Trust it → Spot-check → Check details → Verify carefully → High risk 중 한 지점을 먼저 골라 보세요.

## 2. 능력과 한계

<div class="oa-eli">
  <span class="oa-eli__head">📞 쉽게 말하면</span>
  <p><b>말 전달 게임</b>을 생각해 보세요. "빨간 공 가져와"처럼 짧은 말은 끝까지 그대로 전달돼요. 하지만 긴 이야기는 사람을 거칠 때마다 조금씩 달라져서, 마지막엔 처음과 다른 이야기가 돼요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 이것이 가능하게 하는 것 (능력)</span>
    <ul>
      <li>형식·스타일·길이·톤에 대한 정밀한 통제</li>
      <li>Role-setting: 페르소나·전문성 프레이밍을 채택하고 유지</li>
      <li>명확한 절차를 따르는 다단계 작업 수행</li>
      <li>반복적 개선("더 짧게", "여기 더 구체적으로")</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⚠️ 특징적으로 실패하는 곳 (한계)</span>
    <ul>
      <li><b>Reasoning drift</b>: 긴 사슬에서 작은 오류가 누적됨</li>
      <li><b>Letter over spirit</b>: 지시를 문자 그대로 충족하지만 의도를 놓침</li>
      <li><b>Prompt injection</b>: 문서 속 원치 않는/위험한 지시도 따를 수 있음</li>
    </ul>
  </div>
</div>

학습 목표는 여기에 **취약한 산술(brittle arithmetic)**도 특징적 실패로 꼽아요. 네이티브 수준의 수치·논리 정밀도가 필요한 일은 한계 구간이에요.

### 한계를 밀어내는 제품 기능

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📌</span><span class="oa-card__title">System prompt / 커스텀 지침</span>희석되지 않는 상시 지시</div>
  <div class="oa-card"><span class="oa-card__icon">🧮</span><span class="oa-card__title">Code execution</span>수학을 실제 인터프리터에 위임</div>
  <div class="oa-card"><span class="oa-card__icon">👀</span><span class="oa-card__title">가시적 추론 (visible reasoning)</span>최종 답이 아니라 2단계에서 drift를 잡음</div>
  <div class="oa-card"><span class="oa-card__icon">🧱</span><span class="oa-card__title">구조화 출력 모드</span>letter-over-spirit 방황을 줄임</div>
</div>

이 기능들은 모두 **내 의도가 희석되지 않게** 하려고 존재해요.

## 3. 지시가 아니라 "의도"로 프롬프트하기

<div class="oa-eli">
  <span class="oa-eli__head">💇 쉽게 말하면</span>
  <p>미용실에서 "짧게 잘라 주세요"만 말하면 생각보다 훨씬 짧아질 수 있어요. "<b>면접이 있어서 단정해 보이고 싶어요</b>"까지 말하면 미용사가 길이를 알아서 맞춰 줘요. 방법(지시)에 목적(목표)을 얹는 거예요.</p>
</div>

핵심 실천: **지시가 문자 그대로 지켜졌지만 쓸모없을 때, 지시를 더 세게 반복하지 말고 목표(goal)를 다시 진술**해요. "말하는 것"과 "의미하는 것" 사이 간극이 대부분의 실패가 사는 곳이에요. 지시를 더 강하게 되풀이해도 그 간극은 닫히지 않아요.

| 구간 | 조건 | 예 |
|---|---|---|
| **능력 구간** | 짧고 구체적이고 검증 가능한 지시 | 형식 규격, 길이 제한, 명시적 역할 |
| **한계 구간** | 긴 추론 사슬, 추상적·모호한 지시, 네이티브 수준의 수치·논리 정밀도가 필요한 것 | 4~5단계가 서로 의존하는 작업, "더 전문적으로" |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">📐 형식 (format)</span>
    <ul>
      <li>"불릿 세 개"</li>
      <li>결과물이 어떻게 <b>보여야</b> 하는지</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🎯 목표 (goal)</span>
    <ul>
      <li>"이 일정이 현실적이라고 우리 팀을 설득하기"</li>
      <li>실제로 <b>이루려는</b> 것</li>
    </ul>
  </div>
</div>

> **4D 연결**: Steerability는 **Description**을 강력하게 만들면서 동시에 그 한계를 정해요. 말과 의도의 간극을 알면 프롬프트 방식과 체크포인트 위치가 달라져요.

## 4. Try it out — Letter vs. Spirit

체험 페이지의 한 줄 요약: **지시는 이해가 아니라 패턴 매칭으로 지켜지고, 내 말과 내 의도 사이에는 항상 틈이 있어요.** "Make it shorter", "Make it more professional", "Add more detail" 세 가지 지시를 차례로 살펴봐요.

첫 장면은 이래요. 동료가 이메일 하나를 전달하면서 "더 짧게 해 줘"라고 해요. 이메일의 내용은 다음과 같아요.

| 항목 | 내용 |
|---|---|
| **보낸 사람 → 받는 사람** | Jordan Reyes → Marcus Chen |
| **제목** | Q2 로드맵 후속 논의 |
| **핵심** | 세 가지 방향을 검토해 권고안에 도달: 리소스 제약을 고려하면 **플랫폼 통합(platform consolidation)**이 가장 타당 |
| **tradeoff** | 모바일 리디자인을 Q3로 미뤄야 함 |
| **요청** | 금요일에 팀 전체와 확정하기 전에 **sign-off**를 받고 싶음. 통화가 편하면 통화도 가능 |

"더 짧게"라는 지시는 글자 수만 줄이면 문자 그대로는 충족돼요. 체험 페이지는 이 지시를 그대로 따른 결과(literal result)를 보여 주면서, 말은 지켜졌지만 의도는 놓칠 수 있다는 점을 확인하게 해요.

## 5. 실습 — 목표 다시 쓰기

말하는 것과 의미하는 것 사이의 간극이 대부분의 steerability 실패가 사는 곳이에요. 지시만이 아니라 **의도에서** 프롬프트하는 연습이에요.

1강 목록에서 여러 단계가 있거나 특정 출력 형식이 필요한 작업을 골라요. 그리고 **목표를 한 문장으로** 적어요. 결과물의 모양이 아니라 실제로 이루려는 것이에요.

| Probe | 방법 | 볼 것 |
|---|---|---|
| **1. 강한 통제** | "3열 표로", "정확히 5개 불릿", "처음부터 끝까지 2인칭으로" 같은 짧고 검증 가능한 지시 | 정확히 지켜지는가. 패턴 매칭으로 완벽히 맞출 만큼 단순한 지시 = 능력 구간 |
| **2. Reasoning drift** | 4~5단계가 서로 의존하는 작업을 시키고 단계별 검토 → "2단계 결과를 먼저 보여 주고 멈춰 줘"로 체크포인트를 넣어 다시 실행 | 초기의 작은 오류가 끝까지 가는가. 체크포인트 유무의 차이 |
| **3. Letter vs spirit** | 문자 그대로 충족되지만 쓸모없을 수 있는 지시 → 목표를 함께 명시해 다시 요청 | 두 결과의 차이 |

Probe 3에서 쓸 수 있는 지시의 예예요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 지시만</span>
    <ul>
      <li>"더 짧게 해 줘." (진짜 문제는 구조인 초안에)</li>
      <li>"더 전문적으로 해 줘." (진짜 문제는 요청 사항이 묻혀 있는 이메일에)</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 지시 + 목표</span>
    <ul>
      <li>"더 짧게 해 줘. 내 목표는 2쪽의 핵심 결론까지 임원의 주의를 붙잡는 거야."</li>
    </ul>
  </div>
</div>

실습 뒤에는 작업 목록으로 돌아가요. 다단계 작업에는 **체크포인트를 넣을 자리**를 적고, 형식만으로 프롬프트해 온 작업에는 다음에 덧붙일 **목표 문장** 초안을 써 둬요.

**돌아보기**

- 목표 없이 형식만 말한 적이 얼마나 자주 있었나요? 둘 다 넣으면 무엇이 달라지나요?
- 이번 주부터 중간 체크포인트를 넣을 반복 작업 하나는?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 모델이 지시를 따르는 방식을 가장 잘 설명한 것은?</summary>
  <p>① 지시의 의도를 완전히 이해하고 실행한다 ② <b>다른 모든 것과 같이 패턴을 이어 가는 방식으로 따른다</b> ③ 규칙 엔진이 지시를 해석한다 ④ 가장 최근 지시만 따른다</p>
  <p><b>정답: ②</b> — 그래서 잘 조종되지만, 의도한 것과 도착한 것 사이에 항상 간극이 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. "더 짧게"라고 했더니 글자 수는 줄었지만 정작 핵심 결론이 빠졌다. 다음 행동으로 가장 좋은 것은?</summary>
  <p>① "더 짧게!!"를 강조해서 반복한다 ② 같은 지시를 대문자로 다시 쓴다 ③ <b>지시와 함께 목표를 다시 진술한다</b> ④ 모델을 바꾼다</p>
  <p><b>정답: ③</b> — letter over spirit 실패예요. 지시를 더 세게 반복해도 간극은 닫히지 않아요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 짝 맞추기 — 실패와 한계를 밀어내는 기능</summary>
  <p>"긴 계산에서 숫자가 틀린다" → <b>Code execution</b><br>"5단계 추론의 2단계에서 생긴 오류가 끝까지 간다" → <b>가시적 추론 · 체크포인트</b><br>"대화가 길어지자 처음 정한 규칙이 흐려진다" → <b>System prompt / 커스텀 지침</b><br>"출력 형식이 요청과 다르게 제멋대로다" → <b>구조화 출력 모드</b></p>
</details>

<details class="oa-quiz">
  <summary>Q4. 다음 중 steerability의 능력 구간에 있는 지시는?</summary>
  <p>① "더 설득력 있게 써 줘" ② <b>"3열 표로 답해 줘"</b> ③ "이 다섯 단계 재무 모델을 암산으로 풀어 줘" ④ "알아서 잘해 줘"</p>
  <p><b>정답: ②</b> — 짧고 구체적이고 검증 가능한 지시가 가장 잘 지켜져요.</p>
</details>

## 정리

- Steerability는 모델이 **Next Token Prediction으로 지시를 따른다**는 것이에요.
- 능력 구간은 짧고 구체적이고 검증 가능한 지시(형식 규격, 길이 제한, 명시적 역할), 한계 구간은 긴 추론·추상적 지시·네이티브 정밀도가 필요한 것이에요.
- 특징적 실패: reasoning drift(작은 오류 누적), letter-over-spirit(지시는 지켰으나 의도를 놓침), 그리고 문서 속 지시를 따라 버리는 prompt injection.
- system prompt·code execution·가시적 추론·구조화 출력이 의도의 희석을 막아요.
- 지시가 문자대로 지켜졌지만 쓸모없으면, **더 세게 반복하지 말고 목표를 다시 진술**해요.
- **4D 연결**: Steerability는 **Description**을 강력하게 만들면서 동시에 그 한계를 정해요 — 말과 의도의 간극을 알면 프롬프트 방식과 체크포인트 위치가 달라져요.
