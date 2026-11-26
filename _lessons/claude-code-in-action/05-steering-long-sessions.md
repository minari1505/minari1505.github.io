---
title: "Steering Long Sessions"
title_ko: "긴 세션 조종하기"
course: claude-code-in-action
lesson: 5
---

> 이 레슨부터는 2026년에 개편된 Claude Code in Action(Partner Academy 기준 10개 레슨)을 정리한 내용입니다. 레슨 1~4는 개편 전 버전의 정리입니다.

## 학습 목표

- 긴 작업을 **시작 전에 범위를 정하고, 실행 중에 조종하는** 두 습관을 설명할 수 있다.
- `/compact`에 지시를 붙여 요약 방향을 정하고, rewind 메뉴의 다섯 가지 선택지를 구분할 수 있다.
- `/goal`, `/loop`, worktree가 각각 어떤 상황에 맞는지 고를 수 있다.

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>범위 정하기</strong>plan mode로 계획부터 검토</li>
  <li><strong>조종하기</strong>/compact, rewind</li>
  <li><strong>맡기기</strong>/goal, /loop</li>
  <li><strong>병렬로 돌리기</strong>worktree</li>
</ol>

이 레슨을 관통하는 한 문장: **먼저 범위를 정하고, 그다음 조종합니다.**

<div class="oa-eli">
  <span class="oa-eli__head">🚗 쉽게 말하면</span>
  <p>장거리 운전과 같습니다. 출발 전에 <b>경로를 확인</b>하고(plan mode), 가는 동안 <b>핸들을 조금씩 고쳐 잡습니다</b>(compact, rewind). 길이 익숙해지면 <b>크루즈 컨트롤</b>을 켭니다(goal, loop). 그리고 한 차에 핸들을 두 개 달지 않습니다(worktree).</p>
</div>

## 시작 전: plan mode로 범위 정하기

Claude가 코드를 한 줄도 쓰기 전에 계획을 받습니다. plan mode에서 Claude는 소스 파일을 수정하지 않고 코드를 읽어 무엇을 바꿔야 할지 정리한 계획을 내놓습니다.

핵심은 그 계획을 **훑지 말고 실제로 읽는 것**입니다. 빠진 부분이 있으면 계획 단계에서 추가를 요청합니다. 계획을 고치는 편이, 일단 실행시킨 뒤 어질러진 결과를 치우는 것보다 훨씬 빠릅니다.

## 실행 중: 조종하는 두 가지 도구

### /compact — 요약 방향을 직접 정한다

compact는 대화를 요약해 그 요약을 새 context로 삼고 이전 메시지를 지웁니다. context window가 비워져 작업을 이어갈 수 있지만, 요약에서 중요한 내용이 빠지면 Claude가 방향을 잃습니다.

그래서 `/compact`만 실행하지 않고 **무엇을 남길지 지시를 붙입니다.**

```text
/compact Focus on the --version flag implementation
```

명령 뒤에 쓴 문장이 요약에 남는 내용을 결정합니다.

### Rewind — 잘못 든 길에서 되돌아가기

사용자 프롬프트마다 체크포인트가 만들어집니다. 빈 프롬프트에서 `Escape`를 두 번 누르면 rewind 메뉴가 열립니다.

| 선택지 | 하는 일 | 쓰는 때 |
|---|---|---|
| Restore code and conversation | 코드와 대화를 함께 되돌림 | 방향 자체가 틀렸을 때 |
| Restore conversation | 대화만 되돌림 | 코드는 두고 대화만 정리할 때 |
| Restore code | 파일만 되돌림 | 대화 맥락은 유지하고 변경만 취소할 때 |
| Summarize from here | 체크포인트 **이후**를 요약 | 곁가지 대화를 정리해 공간을 확보할 때 |
| Summarize up to here | 체크포인트 **이전**을 요약 | 긴 준비 단계는 압축하고 구현 부분은 그대로 둘 때 |

## 더 자율적으로: /goal과 /loop

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">/goal — 완료 조건을 건다</span>
    <ul>
      <li>"끝"이 어떤 상태인지 적으면, 빠른 평가기가 조건 충족을 확인할 때까지 여러 턴에 걸쳐 계속 작업합니다.</li>
      <li>취소는 <code>/goal clear</code></li>
      <li>평가기는 <b>대화 기록만</b> 읽습니다. 조건은 테스트 실행 결과처럼 출력으로 확인할 수 있어야 합니다.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">/loop — 주기적으로 실행한다</span>
    <ul>
      <li>프롬프트를 고정 간격이나 자체 조절 간격으로 반복 실행합니다.</li>
      <li>CI 실행이나 배포처럼 <b>외부 상태</b>를 확인하다가 바뀌면 행동할 때 씁니다.</li>
      <li>자체 조절 loop는 대기 중 <code>Escape</code>로 멈춥니다. 고정 간격 loop는 취소를 요청하거나 7일이 지날 때까지 돕니다.</li>
    </ul>
  </div>
</div>

```text
/goal all tests in src/billing pass, and the type checker reports zero errors
```

## 병렬 작업: worktree

같은 코드베이스에서 Claude 세션 두 개가 같은 파일을 건드리면 충돌이 납니다. worktree를 쓰면 세션마다 **독립된 파일 트리**를 가져서 서로의 변경을 덮어쓰지 않습니다. 세션이 끝났을 때 worktree가 깨끗하면 자동으로 제거됩니다.

저장소 루트의 `.worktreeinclude` 파일에는 git이 무시하는 파일 중 worktree마다 복사할 것을 적습니다. 환경 변수 파일이나 로컬 설정처럼 커밋하지는 않지만 매번 필요한 파일에 씁니다.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 디버깅은 끝났고 이제 API 변경만 이어가려 합니다. context를 줄이면서 필요한 내용을 지키려면 어떻게 합니까?</summary>
  <p><b>정답: <code>/compact</code> 뒤에 무엇에 집중할지 지시를 붙입니다.</b> 지시 없이 실행하면 요약에서 중요한 내용이 빠질 수 있습니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. <code>/goal</code>의 조건을 "코드 품질이 좋아질 것"이라고 적었습니다. 무엇이 문제입니까?</summary>
  <p><b>정답: 평가기가 확인할 수 없습니다.</b> 평가기는 대화 기록만 읽으므로, 조건은 테스트 통과나 타입 체크 0건처럼 Claude가 내는 출력으로 확인할 수 있어야 합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 긴 준비 단계를 압축하되 그 뒤의 구현 대화는 그대로 두고 싶습니다. rewind 메뉴에서 무엇을 고릅니까?</summary>
  <p><b>정답: Summarize up to here.</b> 체크포인트 이전을 요약합니다. Summarize from here는 반대로 체크포인트 이후를 요약합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 에이전트 두 개를 같은 저장소에서 동시에 돌리려 합니다. 무엇을 씁니까?</summary>
  <p><b>정답: worktree.</b> 세션마다 독립된 파일 트리를 가지므로 같은 파일을 두고 충돌하지 않습니다.</p>
</details>

## 핵심 정리

- 긴 세션은 **범위 정하기 → 조종하기** 순서입니다. 계획은 훑지 말고 읽습니다.
- `/compact`에는 지시를 붙여 요약이 무엇을 남길지 정합니다.
- 방향이 틀리면 프롬프트로 빠져나오려 하지 말고 rewind로 체크포인트에 돌아갑니다.
- 단계보다 "끝"을 설명하기 쉬우면 `/goal`, 외부 상태를 지켜봐야 하면 `/loop`입니다.
- 병렬 작업은 worktree로 분리합니다.
