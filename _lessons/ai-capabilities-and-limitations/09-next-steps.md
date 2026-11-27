---
title: "Next Steps"
title_ko: "정리와 다음 단계"
course: ai-capabilities-and-limitations
lesson: 9
---

## 학습 목표

- 네 가지 속성과 학습 지문을 하나의 작동하는 멘탈 모델로 종합하기
- Capabilities & Limitations 프레임워크와 4D 프레임워크를 하나의 시스템의 두 반쪽으로 연결하기
- 이번 주에 내 AI 활용에서 바꿀 구체적 변화 하나 정하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>작은 기계 모델</strong>네 속성 = 연속선, 실패 = 속성의 교차</li>
  <li><strong>4D와 연결</strong>속성은 4D 역량이 대응하는 대상</li>
  <li><strong>보정된 신뢰</strong>작업의 위치에 검증·맥락 습관 맞추기</li>
  <li><strong>나의 약속</strong>이번 주에 바꿀 변화 하나</li>
</ol>

이 레슨을 한 문장으로 줄이면: **모든 실패 방식을 외우는 게 아니라, 작고 명료한 기계 모델을 머릿속에 지니고 "어떤 종류의 잘못인지" 알아보는 거예요.**

## 1. 기계에 대한 작은 모델

<div class="oa-eli">
  <span class="oa-eli__head">🧭 쉽게 말하면</span>
  <p>등산할 때 산의 모든 바위를 외우는 사람은 없어요. <b>나침반 하나와 큰 지도 한 장</b>이면, 길을 잃어도 어느 쪽으로 잘못 왔는지 알고 돌아올 수 있죠.</p>
</div>

유창한 AI 활용은 모든 실패 방식을 암기하는 게 아니에요. **머릿속에 작고 명료한 기계 모델을 지녀**, 무언가 잘못됐을 때 "어떤 종류의 잘못인지" 알아보고 그에 맞게 대응하는 것이에요.

네 가지 속성을 능력↔한계 연속선으로 다시 정리하면 이래요. 오른쪽으로 갈수록 더 검증하고 보완해야 해요.

| 속성 | 질문 | 능력 쪽 | 한계 쪽 |
|---|---|---|---|
| **Next Token Prediction** | 답이 어디서 오는가? | 요약·재구성·흔한 개념 설명 | 새롭고 희소한 영역, "참 vs 그럴듯함" |
| **Knowledge** | 실제로 아는 것은? | 자주·최근·일관된 주류 주제 | 드묾·컷오프 이후·niche·논쟁적 |
| **Working Memory** | 지금 주의는 어디에? | 자료가 여유 있고 맥락을 직접 제공 | 아주 긴 문서·대화, 세션 간 연속성(절벽) |
| **Steerability** | 얼마나 통제하는가? | 짧고 구체적·검증 가능한 지시 | 긴 추론, 추상적 요청, 네이티브 정밀도 |

여기에 3강의 **학습 지문**(sycophancy · verbosity · over-caution · 느슨한 확신 보정)과 8강의 **속성 교차**를 더하면 작동하는 멘탈 모델이 완성돼요.

## 2. 두 프레임워크는 하나의 시스템

<div class="oa-eli">
  <span class="oa-eli__head">⛵ 쉽게 말하면</span>
  <p>항해사는 <b>바람과 조류</b>(바꿀 수 없는 바다의 성질)를 읽고 <b>돛과 키</b>(자기 기술)로 대응해요. 속성은 바다의 성질이고, 4D는 항해 기술이에요. 바다를 알아야 기술이 제대로 쓰여요.</p>
</div>

이 프레임워크와 **4D 프레임워크는 한 시스템의 양면**이에요. **속성(machine properties)은 4D 역량이 무엇에 대응하는지를 설명**해요.

| 기계의 속성 | 대응하는 4D 역량 | 뜻 |
|---|---|---|
| 지식의 불균형 (Knowledge) | **Delegation** | 언제 맡기고, 언제 맥락을 직접 주고, 언제 다른 곳으로 갈지 |
| 조종 가능성 · context window (Steerability · Working Memory) | **Description** | 어떻게 말하고, 맥락을 어떻게 구조화할지 |
| 출력이 "생성된" 것임 (Next Token Prediction) | **Discernment** | 어떤 종류의 검증을 적용할지 |
| 책임 있는 사용 | **Diligence** | 책임 있게 쓰는 태도 |

## 3. 보정된 신뢰 (Calibrated trust)

<div class="oa-eli">
  <span class="oa-eli__head">🌦️ 쉽게 말하면</span>
  <p>일기예보를 "전부 믿는다 / 전혀 안 믿는다"로 대하지 않죠. <b>오늘 날씨는 꽤 믿고, 열흘 뒤 날씨는 참고만 해요.</b> 그리고 중요한 야외 행사가 있으면 한 번 더 확인하고요. AI도 작업마다 믿는 정도를 맞추는 거예요.</p>
</div>

보정된 신뢰란 작업을 각 속성의 연속선 위 어디에 놓을지 정하고, **검증·맥락 습관을 그 위치에 맞추는 것**이에요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 통째로 정하는 신뢰</span>
    <ul>
      <li>"AI는 믿을 만해" 또는 "AI는 못 믿어"</li>
      <li>모든 작업에 같은 수준의 확인</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 보정된 신뢰</span>
    <ul>
      <li>작업마다 네 연속선 위의 위치를 가늠</li>
      <li>그 위치에 맞춰 검증과 맥락 제공의 강도를 조절</li>
    </ul>
  </div>
</div>

> 모델은 계속 바뀌지만, 이 속성들의 **"모양"**은 정확한 경계가 이동해도 유용하게 남아요. 그러니 가장자리를 계속 시험해 봐요. 속성은 안정적이지만 선이 놓이는 자리는 모델이 좋아지면서 계속 움직여요.

## 4. 실습 — 나의 약속

마지막으로 1강의 작업 목록으로 돌아가요.

<ol class="oa-flow">
  <li><strong>직감으로 표시</strong>각 작업이 속성별 연속선 어디에 놓이는지, 어떤 보완이 필요할지</li>
  <li><strong>작업 하나 고르기</strong>이번 주에 실제로 할 작업</li>
  <li><strong>변화 하나 정하기</strong>아래 넷 중 하나</li>
  <li><strong>적어 두기</strong>글로 남겨야 약속이 돼요</li>
</ol>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔍</span><span class="oa-card__title">검증 단계</span>구체 정보를 확인하는 단계 넣기</div>
  <div class="oa-card"><span class="oa-card__icon">📁</span><span class="oa-card__title">상시 맥락 설정</span>project, 저장된 지침, 참고 문서</div>
  <div class="oa-card"><span class="oa-card__icon">🚏</span><span class="oa-card__title">체크포인트</span>다단계 작업 중간에 멈춰 확인</div>
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">목표 진술 습관</span>형식만이 아니라 목표도 말하기</div>
</div>

**돌아보기**

- 1강에서 지금까지, AI의 행동을 생각하는 방식에서 가장 크게 바뀐 것 하나는?
- 여기서 배운 것으로 4D 중 어느 것이 가장 즉시 날카로워졌다고 느끼나요?

**다음 단계**: 아직 **AI Fluency: Framework & Foundations** 코스를 듣지 않았다면 그것이 자연스러운 다음 걸음이에요. 이 코스가 기계 쪽 맥락을 준 바로 그 **사람 쪽 역량**을 깊이 다뤄요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 유창한 AI 활용에 가장 가까운 것은?</summary>
  <p>① 모든 실패 방식을 암기한다 ② <b>작고 명료한 기계 모델을 지니고, 잘못됐을 때 어떤 종류의 잘못인지 알아본다</b> ③ 최신 모델의 기능 목록을 외운다 ④ AI 출력을 항상 전부 다시 쓴다</p>
  <p><b>정답: ②</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 속성과 4D 역량</summary>
  <p>지식의 불균형 → <b>Delegation</b><br>조종 가능성 · context window → <b>Description</b><br>출력이 "생성된" 것임 → <b>Discernment</b><br>책임 있는 사용 → <b>Diligence</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 모델이 계속 좋아지면 이 프레임워크는 쓸모없어진다. (O / X)</summary>
  <p><b>정답: X</b> — 정확한 경계는 이동하지만 속성의 모양은 유용하게 남아요. 그래서 가장자리를 계속 시험해 보는 것이 좋아요.</p>
</details>

## 정리

- 이제 작동하는 멘탈 모델을 갖췄어요: 네 속성은 연속선, 특징적 실패는 속성의 교차.
- 이 프레임워크와 4D는 하나의 시스템의 두 면 — 속성은 4D 역량이 대응하는 대상을 설명해요.
- 보정된 신뢰 = 작업을 각 연속선 위에 놓고 검증·맥락 습관을 그에 맞추기.
- 이번 주에 바꿀 변화 하나(검증 단계, 상시 맥락, 체크포인트, 목표 진술 습관)를 정해 적어 둬요.
- 모델은 바뀌어도 이 속성들의 모양은 유용하게 남아요. 다음 단계로 **AI Fluency: Framework & Foundations**(사람 쪽 역량)로 이어 가면 좋아요.
