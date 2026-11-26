---
title: "Permission Modes"
title_ko: "권한 모드"
course: claude-code-in-action
lesson: 8
---

## 학습 목표

- 여섯 가지 권한 모드가 각각 무엇을 묻지 않고 실행하는지 구분할 수 있다.
- auto 모드의 classifier가 **무엇을 막고 무엇을 못 막는지** 설명할 수 있다.
- 사람이 없는 실행에 맞는 모드를 고를 수 있다.

## 한눈에 보는 여섯 모드

| 모드 | 묻지 않고 실행하는 것 | 맞는 상황 |
|---|---|---|
| Manual | 읽기만 | 하나하나 확인하며 작업 |
| Accept edits | 읽기, 파일 수정, 흔한 파일 시스템 bash 명령 | 수정은 나중에 검토하며 반복 작업 |
| Plan | 읽기(수정 없음) | 조사하고 변경을 제안받을 때 |
| Auto | 전부. 단 대부분의 행동을 별도 classifier 모델이 먼저 검토 | 손을 떼고 맡길 때 |
| Don't ask | 작업 디렉터리 안의 읽기와 사전 승인한 도구. 물어야 할 것은 **자동 거부** | CI, 예약 작업, 야간 배치 |
| Bypass permissions | 전부. 모든 검사를 건너뜀 | 격리된 컨테이너나 VM 안에서만 |

`Shift+Tab`을 누르면 일상적으로 쓰는 모드(manual, accept edits, plan, auto)를 순환합니다. 현재 모드는 아래 상태 표시줄에 나옵니다.

<div class="oa-eli">
  <span class="oa-eli__head">🏢 쉽게 말하면</span>
  <p>건물 출입증의 등급과 같습니다. <b>Manual</b>은 문마다 경비에게 확인받는 방문증, <b>Auto</b>는 대부분 통과하지만 금고 앞에서는 경비가 막는 직원증, <b>Don't ask</b>는 허가된 문만 열리고 나머지는 조용히 잠기는 야간 출입증, <b>Bypass</b>는 모든 문이 열리는 마스터키입니다. 마스터키는 훈련장(격리 환경)에서만 씁니다.</p>
</div>

## Auto 모드가 동작하는 방식

Claude가 스스로 실행하되, 대부분의 행동이 실행되기 전에 **별도의 classifier 모델**이 검토합니다. classifier는 **의도**를 지킵니다. 요청한 범위를 넘어 확대되는 행동을 감시합니다.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">막도록 설계된 것</span>
    <ul>
      <li>프로덕션 배포와 마이그레이션</li>
      <li>force push, 내려받은 코드를 셸로 바로 넘기기</li>
      <li>민감한 데이터를 외부로 보내기</li>
      <li>세션 전부터 있던 파일을 되돌릴 수 없게 삭제하기</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">통과시키는 것</span>
    <ul>
      <li>프로젝트 안의 로컬 수정</li>
      <li>lock 파일 기준 의존성 설치</li>
      <li>읽기 전용 요청</li>
      <li>내 브랜치로 push</li>
    </ul>
  </div>
</div>

차단·허용 목록은 계속 바뀌고 있으므로 최신 내용은 공식 문서에서 확인합니다.

## classifier가 못 하는 일

classifier는 **의도를 볼 뿐 정확성은 보지 않습니다.** 인증 리팩터링을 시켰는데 Claude가 깨진 인증 코드를 써도 통과시킵니다. 깨진 코드는 위험한 행동이 아니기 때문입니다.

그래서 auto 모드는 테스트를 실행하는 **Stop hook**과 짝지어 씁니다.

<ol class="oa-flow">
  <li><strong>실행 중</strong>auto 모드의 classifier가 의도를 지킴</li>
  <li><strong>턴 종료 시</strong>Stop hook이 테스트를 실행</li>
  <li><strong>실패하면</strong>턴을 끝내지 못하고 Claude가 고침</li>
</ol>

하나는 행동 **전에** 의도를, 다른 하나는 행동 **후에** 정확성을 지킵니다.

## Don't ask: 사람이 없는 실행

CI 파이프라인, 예약 작업, 야간 배치처럼 승인해 줄 사람이 없을 때 씁니다. Manual 모드에서 승인 없이 되던 것(작업 디렉터리 안의 읽기)과 사전 승인한 도구는 그대로 실행되고, 물어야 할 것은 자동으로 거부됩니다. 파이프라인이 아무도 답하지 않을 승인을 기다리며 멈추지 않게 하는 것이 목적입니다.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. auto 모드로 리팩터링을 맡겼더니 테스트가 깨진 코드가 나왔습니다. classifier는 왜 막지 않았습니까?</summary>
  <p><b>정답: classifier는 의도만 보고 정확성은 보지 않기 때문입니다.</b> 깨진 코드는 위험한 행동이 아닙니다. 정확성은 테스트를 실행하는 Stop hook으로 지킵니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 야간 CI 작업에 가장 맞는 권한 모드는 무엇입니까?</summary>
  <p><b>정답: Don't ask.</b> 사전 승인한 도구만 실행하고, 승인이 필요한 행동은 자동 거부해서 파이프라인이 멈추지 않습니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Bypass permissions는 어디에서만 써야 합니까?</summary>
  <p><b>정답: 격리된 컨테이너나 가상 머신 안.</b> 모든 검사를 건너뛰는 모드로, <code>--dangerously-skip-permissions</code> 플래그와 같습니다.</p>
</details>

## 핵심 정리

- 권한 모드는 Claude가 묻지 않고 실행할 범위를 한 번에 정합니다.
- Auto는 손을 떼는 모드입니다. classifier가 의도를 지키고, Stop hook이 정확성을 지킵니다.
- 사람이 없는 실행은 Don't ask, Bypass는 격리 환경에서만 씁니다.
