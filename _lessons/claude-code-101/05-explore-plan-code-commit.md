---
title: "The explore → plan → code → commit workflow"
title_ko: "탐색 → 계획 → 코딩 → 커밋 workflow"
course: claude-code-101
lesson: 5
---

## 학습 목표

- **Explore → Plan → Code → Commit** 4단계 workflow의 흐름 이해하기
- 각 단계에서 무엇을 하고 **어디서 방향을 바로잡아야 하는지** 파악하기
- 코딩 단계를 매끄럽게 만드는 **성공 기준 · 도구 · 테스트**의 역할 이해하기
- 커밋 전에 **subagent code reviewer**를 쓰는 이유 설명하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>Explore</strong>프로젝트에 필요한 관련 맥락을 Claude에게 줘요</li>
  <li><strong>Plan</strong>성공을 측정할 기준이 되는 행동 계획을 만들어요</li>
  <li><strong>Code</strong>최종 결과에 이르기까지 사용자와 Claude가 주고받아요</li>
  <li><strong>Commit</strong>검토하고 push해 다음 기능으로 넘어가요</li>
</ol>

이 레슨을 한 문장으로 줄이면: **곧장 코드를 시키지 말고, 탐색하고 계획한 뒤에 코딩하고 검토해서 커밋해요.**

## 1. 이 강의에서 딱 하나만 가져간다면

<div class="oa-eli">
  <span class="oa-eli__head">🧳 쉽게 말하면</span>
  <p>여행 가기 전에 지도를 보고(Explore), 일정을 짜고(Plan), 다녀온 뒤(Code), 사진을 정리해요(Commit). <b>지도도 일정도 없이 출발하면</b> 길을 되돌아오는 데 시간을 더 써요.</p>
</div>

이 과정에서 딱 하나만 가져간다면 바로 이 workflow예요: **Explore, Plan, Code, Commit.**

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 대부분이 하는 방식</span>
    <ul>
      <li>곧장 Claude에게 코드를 작성하라고 시켜요</li>
      <li>→ 나중에 <b>더 많이 되돌려야</b> 해요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 이 workflow</span>
    <ul>
      <li>탐색 → 계획 → 코딩 → 커밋</li>
      <li>→ 코드가 쓰이기 <b>전에</b> 방향을 잡아요</li>
    </ul>
  </div>
</div>

## 2. Explore와 Plan

<div class="oa-eli">
  <span class="oa-eli__head">🏗️ 쉽게 말하면</span>
  <p>건축가는 땅을 먼저 둘러보고 설계도를 그려요. <b>설계도 단계에서 창문 위치를 바꾸는 건 지우개 한 번</b>이면 되지만, 벽을 세운 뒤에는 공사예요.</p>
</div>

앞의 두 단계를 가장 빠르게 처리하는 방법은 **Plan Mode**예요.

| Plan Mode에서 | 내용 |
|---|---|
| **못 하는 것** | 파일 수정 |
| **하는 것** | 파일을 읽고 shell 명령을 실행해 탐색하면서, 구현을 어떻게 할지 정보를 모아요 |

Shift + Tab을 **status bar에 plan mode on이 보일 때까지** 누른 뒤 이렇게 써요.

```text
이미지 업로드 파이프라인에 WebP 변환을 추가해야 해. 파이프라인의 어느 지점에서 처리해야 하는지,
새 의존성이 필요한지, 어떻게 접근할지 파악해줘.
```

<ol class="oa-flow">
  <li><strong>조사</strong>Claude가 관련 파일을 읽고 웹 검색을 해요</li>
  <li><strong>계획 제시</strong>행동 계획을 돌려줘요</li>
  <li><strong>검토</strong>내 기준에 맞는지 판단해요</li>
  <li><strong>수정 요청</strong>아니면 특정 부분을 고쳐 달라고 해요</li>
</ol>

> **코드가 작성되기 전**이라, 여기가 방향을 바로잡기 가장 좋은 지점이에요.

변경할 의도 없이 **코드베이스 요약만** 원한다면, Plan Mode가 아니어도 **explore subagent**를 실행할 수 있어요.

## 3. Code

<div class="oa-eli">
  <span class="oa-eli__head">🎯 쉽게 말하면</span>
  <p>시험을 볼 때 <b>채점 기준표</b>가 있으면 스스로 답을 점검할 수 있죠. Claude도 "무엇이 정답인지"와 "확인할 수단"이 있어야 혼자서 끝까지 갈 수 있어요.</p>
</div>

계획이 좋아 보이면 **"approve"**를 선택해 수락하고, Claude가 항목을 하나씩 처리하게 해요. Claude가 어떻게 진행할지는 permission mode가 정해요.

| permission mode | 진행 방식 |
|---|---|
| 매번 묻기 | 단계마다 사용자에게 물어봐요 |
| auto-accept | 파일 수정을 자동 수락해요 |
| auto mode | 백그라운드 safety check가 대부분의 행동을 걸러 주는 상태로 진행해요 |

Claude는 계획을 "완료"로 보기 전에 최선을 다해 문제를 해결하지만, 때로는 **사람이 개입**해야 해요. 여기서 Plan Mode의 장점이 드러나요. 실행이 끝난 뒤에도 **결과에 이르게 된 맥락**을 함께 갖고 있어서, Claude의 다음 결정을 잘 이끌 수 있어요.

### 코딩 단계를 매끄럽게 만드는 팁

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🎯</span><span class="oa-card__title">성공 기준을 정의하기</span>Claude가 결과를 확신하려면 "무엇이 올바른가"가 분명해야 해요. <b>계획을 쓸 때 명시</b>하세요</div>
  <div class="oa-card"><span class="oa-card__icon">🧰</span><span class="oa-card__title">도구를 추가하기</span>목표 달성을 돕는 도구는 왕복을 크게 줄여요. 웹 UI를 만든다면 Claude in Chrome 확장을 설치해, Claude Code가 브라우저 탭을 제어하며 UI를 직접 테스트하게 할 수 있어요</div>
  <div class="oa-card"><span class="oa-card__icon">🧪</span><span class="oa-card__title">테스트 스위트를 포함하기</span>Claude가 계속 검증할 수 있는 테스트를 주세요. Claude가 테스트를 대신 작성할 수도 있어요</div>
</div>

> 테스트를 넘기기 전에, **거짓 양성(false positive)**을 피하도록 그 테스트가 믿을 만한 기준(source of truth)인지 확인하세요.

> 팁: Claude가 같은 문제에 계속 부딪힌다면, 그 해결책을 **CLAUDE.md 파일에 저장**하라고 요청하세요.

## 4. Commit

<div class="oa-eli">
  <span class="oa-eli__head">👀 쉽게 말하면</span>
  <p>내가 쓴 글의 오타는 내 눈에 잘 안 보여요. <b>처음 읽는 사람</b>이 훨씬 잘 찾죠. subagent 리뷰어가 바로 그 "처음 읽는 사람"이에요.</p>
</div>

<ol class="oa-flow">
  <li><strong>직접 테스트</strong>변경을 내가 테스트해 만족스러운지 확인해요</li>
  <li><strong>subagent code reviewer</strong>커밋 전에 작업을 검토하게 해요</li>
  <li><strong>커밋 메시지 생성</strong>Claude가 내 스타일에 맞게 써 줘요</li>
  <li><strong>반복</strong>다음 기능으로 넘어가 같은 흐름을 되풀이해요</li>
</ol>

subagent는 코드베이스를 **새로운 시선(fresh pair of eyes)**으로 봐요. 세션 내내 쌓인 main agent의 **편향을 갖고 있지 않아요.**

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 이 workflow에서 방향을 바로잡기 가장 좋은 지점은 어디인가요?</summary>
  <p><b>정답: Plan 단계에서 계획을 검토할 때.</b> 아직 코드가 작성되기 전이라 수정 비용이 가장 낮아요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 변경할 생각 없이 코드베이스 요약만 필요해요. 꼭 Plan Mode여야 하나요?</summary>
  <p><b>정답: 아니요.</b> Plan Mode가 아니어도 explore subagent를 실행할 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Claude가 작성한 테스트를 검증 기준으로 넘기기 전에 확인할 것은?</summary>
  <p><b>정답: 테스트가 믿을 만한 기준인지.</b> 그렇지 않으면 거짓 양성이 생겨 "통과했지만 틀린" 결과가 나올 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 커밋 전에 main agent가 아니라 subagent에게 리뷰를 맡기는 이유는?</summary>
  <p><b>정답: subagent는 세션 내내 쌓인 main agent의 편향이 없어서</b> 코드베이스를 새로운 시선으로 봐요.</p>
</details>

## 정리

- Claude Code를 효과적으로 쓰려면 **Explore → Plan → Code → Commit** workflow를 따라요.
- **Explore**는 프로젝트에 필요한 관련 맥락을 Claude에게 줘요.
- **Plan**은 Claude가 성공을 측정할 기준이 되는 행동 계획을 만들어요. 코드가 쓰이기 전이라 방향을 고치기 가장 좋아요.
- **Code**는 최종 결과에 도달하기까지 사용자와 Claude가 주고받는 과정이에요. 성공 기준 · 도구 · 테스트가 이 단계를 매끄럽게 해요.
- **Commit**은 코드를 검토·push해 다음 기능으로 넘어가게 해요. 커밋 전에 subagent code reviewer를 돌려요.
- 같은 문제가 반복되면 해결책을 **CLAUDE.md**에 저장하게 하세요.
