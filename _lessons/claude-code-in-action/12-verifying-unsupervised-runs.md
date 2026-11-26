---
title: "Trust It: Verifying Unsupervised Runs"
title_ko: "지켜보지 않은 실행 검증하기"
course: claude-code-in-action
lesson: 12
---

## 학습 목표

- 검증 강도를 **실행을 얼마나 지켜봤는지에 비례해** 정하는 원칙을 설명할 수 있다.
- 요약이 아니라 diff에서 검토를 시작하는 이유를 말할 수 있다.
- 테스트를 "약속"이 아니라 "관문"으로 만드는 hook 구성을 설명할 수 있다.

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>auto 모드 유지</strong>bypass가 아니라 auto</li>
  <li><strong>diff부터</strong>/code-review, git diff</li>
  <li><strong>테스트를 관문으로</strong>Stop hook, PostToolUse hook</li>
  <li><strong>차가운 두 번째 의견</strong>새 세션이나 서브에이전트</li>
</ol>

이 레슨을 관통하는 한 문장: **덜 지켜본 만큼 더 검증합니다.**

<div class="oa-bars">
  <div class="oa-bar"><span>짧은 세션, 직접 지켜봄</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:25%"></span><span class="oa-bar__val">빠르게 훑어보기</span></span></div>
  <div class="oa-bar"><span>긴 무인 실행</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:70%"></span><span class="oa-bar__val">diff 검토 + 테스트 관문</span></span></div>
  <div class="oa-bar"><span>아무도 없는 CI 실행</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:100%"></span><span class="oa-bar__val">사후에 전부 재구성</span></span></div>
  <div class="oa-bars__caption">막대는 필요한 검증의 상대적인 무게를 나타낸 것입니다.</div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면</span>
  <p>집을 비운 사이 인테리어 공사를 맡긴 상황입니다. 옆에서 지켜봤다면 마무리만 확인하면 됩니다. 일주일 동안 자리를 비웠다면 "다 끝났습니다"라는 <b>보고만 듣지 않고</b> 방마다 직접 돌아보고, 수도와 전기를 켜 보고, 다른 전문가에게도 한 번 보여 줍니다.</p>
</div>

## 무인 실행은 auto 모드로

지켜보지 않는 실행은 bypass permissions가 아니라 **auto 모드**로 둡니다. classifier가 대부분의 행동을 위험 여부로 계속 검토하기 때문입니다.

다만 이 안전망의 범위를 분명히 알아야 합니다. classifier는 위험한 행동만 표시하고, **코드가 맞는지는 판단하지 않습니다.** 그러므로 검증 기준은 그대로 유지합니다.

## 요약이 아니라 diff에서 시작

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ Claude의 요약부터 읽기</span>
    <ul>
      <li>요약은 깔끔하게 읽힙니다.</li>
      <li>그런데 실제 diff는 예상하지 못한 파일을 건드렸을 수 있습니다. 요약은 그것을 말해 주지 않습니다.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ diff부터 읽기</span>
    <ul>
      <li><code>/code-review</code>로 변경을 훑으며 문제를 표시</li>
      <li><code>git diff</code>를 직접 눈으로 확인</li>
      <li>계획에 있던 파일을 먼저 읽고, 계획 밖의 변경을 찾습니다.</li>
    </ul>
  </div>
</div>

깔끔한 글이 깔끔한 코드의 증거는 아닙니다.

## 테스트를 약속이 아니라 관문으로

무인 실행의 진짜 관문은 테스트가 통과했는지, 그리고 Claude가 테스트를 **실제로 실행했는지 아니면 실행했다고만 했는지**입니다. 이것을 신뢰에 맡기지 않고 hook으로 연결합니다.

| Hook | 하는 일 |
|---|---|
| Stop hook | 테스트를 실행하고, 실패하면 턴 종료를 거부 |
| PostToolUse hook | 수정할 때마다 lint와 타입 체크 |

핵심은 exit code입니다. hook이 **exit 2**로 끝나면 실패 내용이 Claude에게 그대로 전달되고, Claude가 요청 없이도 읽고 고칩니다. 이 확인은 내가 요청을 기억하든 말든 매 실행마다 작동합니다.

headless 실행은 JSON 결과와 exit code로 검증합니다.

## 차가운 두 번째 의견

PR 전에 돌리는 서브에이전트 코드 리뷰를 무인 실행 결과에도 씁니다. 새 세션이나 서브에이전트를 열어, **코드가 어떻게 만들어졌는지 모르는 상태로** 변경된 코드를 리뷰하게 합니다. 접근 방식에 대한 애착이 없어서, 원래 실행이 스스로 넘어간 부분을 잡아냅니다.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 무인 실행 결과를 검토할 때 Claude의 요약이 아니라 diff부터 보는 이유는 무엇입니까?</summary>
  <p><b>정답: 요약은 예상 밖의 파일 변경을 알려 주지 않기 때문입니다.</b> 계획에 있던 파일을 먼저 읽고, 그 밖의 변경이 있는지 찾습니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. auto 모드의 classifier가 있으니 테스트 검증은 생략해도 됩니까?</summary>
  <p><b>정답: 안 됩니다.</b> classifier는 위험한 행동만 표시하고 코드의 정확성은 판단하지 않습니다. 검증 기준은 그대로입니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Claude가 "테스트 통과"라고 말했지만 실제로 돌렸는지 확신할 수 없습니다. 어떻게 보장합니까?</summary>
  <p><b>정답: Stop hook이 테스트를 실행하게 하고, 실패하면 exit 2로 턴 종료를 거부합니다.</b> 실패 내용이 Claude에게 전달되어 스스로 고칩니다.</p>
</details>

## 핵심 정리

- 검증은 실행을 덜 지켜본 만큼 무겁게 합니다.
- 무인 실행은 auto 모드로 두되, classifier가 정확성을 보지 않는다는 점을 기억합니다.
- diff를 직접 읽고, 테스트를 hook으로 관문화하고, 중요한 변경은 새 세션의 리뷰를 받습니다.
