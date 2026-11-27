---
title: "Your first prompt"
title_ko: "첫 prompt 작성하기"
course: claude-code-101
lesson: 4
---

## 학습 목표

- **Manual · Auto-accept · Auto mode**의 차이와 전환 방법(Shift + Tab) 이해하기
- **Plan Mode**가 무엇을 하고 언제 유용한지 파악하기
- **구체적인 prompt**로 복잡한 작업을 안전하게 시작하는 방법 익히기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>permission mode 고르기</strong>Shift + Tab으로 순환</li>
  <li><strong>Plan Mode 진입</strong>status bar에 plan mode on</li>
  <li><strong>구체적인 prompt</strong>무엇을, 어디에, 어떤 기준으로</li>
  <li><strong>계획 검토 · 수락</strong>괜찮으면 수락하고 진행</li>
  <li><strong>결과 확인</strong>무엇을 어떻게 했는지 확인</li>
</ol>

이 레슨을 한 문장으로 줄이면: **얼마나 맡길지 먼저 정하고, 복잡한 일은 Plan Mode에서 구체적인 prompt로 시작해요.**

## 1. permission mode 고르기

<div class="oa-eli">
  <span class="oa-eli__head">🚘 쉽게 말하면</span>
  <p>운전 연수와 비슷해요. 처음엔 강사가 <b>매번 "지금 꺾어도 돼요?"</b> 하고 물어보게 하고, 익숙해지면 간단한 건 알아서 하게 두죠. 어느 단계가 맞는지는 내가 편한 정도에 달렸어요.</p>
</div>

Claude Code와 대화하는 방식은 다른 AI assistant와 같아요. 다만 prompt를 입력하기 전에, Claude가 일하는 동안 **얼마나 지켜볼지** 고를 수 있어요. **Shift + Tab**으로 모드를 순환해요.

| 모드 | 동작 |
|---|---|
| **Manual mode** | 파일을 수정하거나 명령을 실행할 때마다 Claude가 허가를 요청해요 |
| **Auto-accept mode** | 파일 수정과 `mkdir`, `mv`, `rm` 같은 흔한 파일시스템 명령은 자동 승인돼요. **그 외 명령은 여전히 허가가 필요**해요 |
| **Auto mode** | 일상적인 permission prompt 없이 일하고, 백그라운드 safety check가 대부분의 행동을 걸러요. 무언가 막히면 Claude가 보통 더 안전한 방법을 찾거나 진행 여부를 물어봐요 |

정답은 없어요. 자신이 편한 방식으로 고르면 돼요.

> 예전 노트의 "Approval 모드"는 현재 강의에서 **Manual mode**로 불려요. Auto-accept의 범위(흔한 파일시스템 명령 포함)와 **Auto mode**는 현재 강의 본문에서 추가된 내용이에요.

## 2. Plan Mode

<div class="oa-eli">
  <span class="oa-eli__head">📐 쉽게 말하면</span>
  <p>인테리어 공사 전에 <b>도면과 견적서</b>부터 받는 거예요. 벽을 허물고 나서 "이게 아닌데"라고 하면 늦어요. 도면 단계에서 고치는 게 가장 싸요.</p>
</div>

Shift + Tab 메뉴 안에 **Plan Mode**가 있어요. Plan Mode가 하는 일은 이래요.

<ol class="oa-flow">
  <li><strong>prompt를 받아요</strong>하고 싶은 일을 설명해요</li>
  <li><strong>조사해요</strong>파일을 읽고 shell 명령을 실행해 코드베이스를 분석하고, 제안한 구현을 조사해요. <b>소스 파일은 수정하지 않아요</b></li>
  <li><strong>질문해요</strong>중간에 확인 질문을 던져요</li>
  <li><strong>계획을 돌려줘요</strong>실행 가능한 상세 계획</li>
</ol>

Plan Mode가 특히 좋은 상황은 두 가지예요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🧱</span><span class="oa-card__title">복잡한 변경 계획</span>여러 단계로 이루어진 기능 구현을 요청하는 경우가 많은데, 바로 그럴 때 빛을 발해요</div>
  <div class="oa-card"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">안전한 코드 리뷰</span>소스 파일을 건드리지 않으니 안심하고 살펴보게 할 수 있어요</div>
</div>

## 3. 예시 — 다크 모드 토글 추가

<div class="oa-eli">
  <span class="oa-eli__head">🍽️ 쉽게 말하면</span>
  <p>식당에서 "맛있는 거 주세요"보다 <b>"덜 맵게, 면은 곱빼기로"</b>라고 말해야 원하는 게 나와요. prompt도 구체적일수록 결과가 좋아요.</p>
</div>

다크 모드 토글이 필요한 애플리케이션이 있다고 해 볼게요. 프로젝트 루트에서 `claude`를 실행하고, **Shift + Tab을 status bar에 plan mode on이 보일 때까지** 누른 뒤 이렇게 써요.

```text
내 앱 전체에 다크 모드를 구현해야 해. 헤더에 라이트/다크 모드를 전환하는 토글 스위치를 만들어줘.
기존 라이트 테마를 기준으로 대비가 좋은 색을 찾아줬으면 해.
```

이 prompt가 좋은 이유를 뜯어보면 이래요.

| prompt에 담긴 것 | 예시 문장 |
|---|---|
| **범위** | "내 앱 전체에" |
| **무엇을, 어디에** | "헤더에 … 토글 스위치" |
| **판단 기준** | "기존 라이트 테마를 기준으로 대비가 좋은 색" |

그다음 흐름은 이래요.

<ol class="oa-flow">
  <li><strong>계획 세우기</strong>Claude가 계획을 세우게 둬요</li>
  <li><strong>검토</strong>계획이 괜찮은지 확인해요</li>
  <li><strong>수락 · 진행</strong>수락하면 Claude가 작업해요. permission mode에 따라 중간중간 확인을 요청할 수 있어요</li>
  <li><strong>확인</strong>Claude가 무엇을 했고 어떻게 결론에 이르렀는지 볼 수 있어요</li>
</ol>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. permission mode를 바꾸는 단축키는 무엇인가요?</summary>
  <p><b>정답: Shift + Tab.</b> 누를 때마다 모드가 순환하고, Plan Mode도 이 메뉴 안에 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. Auto-accept mode에서도 여전히 허가가 필요한 것은?</summary>
  <p><b>정답: 파일 수정과 흔한 파일시스템 명령(<code>mkdir</code>, <code>mv</code>, <code>rm</code> 등)을 제외한 그 외 명령.</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. Plan Mode에서 Claude가 하지 않는 일은 무엇인가요?</summary>
  <p><b>정답: 소스 파일 수정.</b> 파일을 읽고 shell 명령으로 조사하고, 질문하고, 실행 가능한 계획을 돌려줘요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 여러 단계로 이루어진 기능을 구현해야 해요. 어떻게 시작하는 게 좋을까요?</summary>
  <p><b>정답: Plan Mode에서 구체적인 prompt로 시작해요.</b> 계획을 먼저 검토하고 수락한 뒤 진행하면 코드가 쓰이기 전에 방향을 잡을 수 있어요.</p>
</details>

## 정리

- prompt는 **최대한 구체적으로** 써요.
- 모든 단계에 관여하고 싶으면 **Manual mode**, 편집을 맡기고 싶으면 **Auto-accept mode**, 더 맡기고 싶으면 **Auto mode**를 골라요. **Shift + Tab**으로 전환해요.
- **Plan Mode**는 소스 파일을 수정하지 않고 조사·질문·계획까지 해 줘요.
- 복잡한 변경과 안전한 코드 리뷰에 Plan Mode가 특히 잘 맞아요.
- 코드를 실행하기 전에 Claude가 **원하는 바를 깊게 파고들게** 하려면 Plan Mode를 써요.
