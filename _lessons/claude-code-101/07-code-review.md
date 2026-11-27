---
title: "Code review"
title_ko: "코드 리뷰"
course: claude-code-101
lesson: 7
---

## 학습 목표

- Claude의 **요약만 믿지 않고 실제 diff를 읽어야 하는 이유** 이해하기
- `/diff`로 변경을 살펴보고 **매번 다시 볼 세 가지**를 알아보기
- `/code-review`로 **깨끗한 context의 second opinion** 받기
- 리뷰 결과(finding)를 **Fix now · Ask why · Leave it**으로 분류하기
- (이전 버전 강의 내용) subagent 리뷰어, `/commit-push-pr`, `--from-pr` 알아 두기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>요약 읽기</strong>Claude가 한 일을 간결하게 보고해요</li>
  <li><strong>실제 diff 확인</strong><code>/diff</code>로 파일별 변경을 직접 봐요</li>
  <li><strong>second opinion</strong><code>/code-review</code>가 깨끗한 context에서 검토해요</li>
  <li><strong>finding 분류</strong>Fix now · Ask why · Leave it</li>
  <li><strong>증거와 함께 수정</strong>고치고, 테스트 출력을 보여 달라고 해요</li>
</ol>

이 레슨을 한 문장으로 줄이면: **요약이 전부 사실이어도 빠진 게 있을 수 있으니, diff를 직접 보고 깨끗한 context의 리뷰를 한 번 더 받아요.**

> 이 레슨은 현재 강의 본문이 크게 바뀌었어요. 1~4장은 **현재 강의 본문** 기준이고, 5장은 예전 노트에 있던 내용을 따로 모아 둔 거예요.

## 1. 요약이 아니라 실제 변경을 보세요

<div class="oa-eli">
  <span class="oa-eli__head">🧾 쉽게 말하면</span>
  <p>이사 업체가 "짐 다 옮겼어요"라고 하면 그 말은 사실이에요. 그래도 <b>박스를 열어 봐야</b> 그릇에 금이 갔는지 알 수 있죠. 요약은 참이어도 전부는 아니에요.</p>
</div>

Claude Code에 작업을 맡기면 Claude는 보통 **간결하게** 보고해요. 그 설명 아래에는 작은 것부터 큰 것까지 다양한 파일 변경이 있을 수 있어요.

중요한 점은 이거예요. **코드를 쓰고 설명한 바로 그 세션은, 그 변경을 판단하기에 가장 좋은 심사자가 아닌 경우가 많아요.** 그래서 좋은 습관은 두 단계예요.

<ol class="oa-flow">
  <li><strong>내가 직접 보기</strong>유지하기 전에 모든 변경을 한 번 살펴봐요</li>
  <li><strong>Claude가 다시 보기</strong>이 세션의 기록이 없는 <b>깨끗한 context</b>에서 다시 리뷰하게 해요</li>
</ol>

### `/diff`로 변경 보기

**diff**는 변경의 before-and-after예요. 파일별로 제거된 줄과 추가된 줄을 보여 줘요.

```text
/diff
```

| `/diff`가 해 주는 것 | 조작 |
|---|---|
| 아직 커밋하지 않은 변경을 보여 주는 **interactive viewer**를 열어요 | 위·아래 화살표로 파일 사이를 이동 |
| Claude의 **각 turn이 무엇을 바꿨는지**도 보여 줄 수 있어요 | Enter로 파일 열기 |

### 매번 다시 봐야 할 세 가지

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🙅</span><span class="oa-card__title">요청하지 않은 변경</span>Claude가 파일에 들어간 김에 고친 config 값, 언급하지 않았는데 다시 쓰인 helper 메서드</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🧪</span><span class="oa-card__title">약해진 테스트</span>건너뛰었거나(skip), 삭제됐거나, 통과할 때까지 느슨해진 테스트</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">📦</span><span class="oa-card__title">새 패키지와 하드코딩 값</span>함수 하나 때문에 추가된 의존성, 코드에 그대로 박힌 URL이나 key</div>
</div>

### 변경 전체가 잘못됐다면 — `/rewind`

<ol class="oa-flow">
  <li><strong><code>/rewind</code> 실행</strong>또는 빈 prompt에서 Esc를 두 번</li>
  <li><strong>prompt 고르기</strong>그 변경을 만든 prompt를 선택</li>
  <li><strong>복원</strong>"Restore code and conversation" 선택</li>
</ol>

> 알아 둘 한계: 패키지 설치처럼 **Claude가 실행한 shell 명령이 바꾼 파일은 되돌려지지 않아요.**

### `/diff`에 아무것도 안 보인다면

`/diff`와 `/code-review`는 **git의 변경 기록**을 읽어요. 그래서 프로젝트가 git 저장소 안에 있어야 해요.

| 상황 | 할 일 |
|---|---|
| 아직 git을 안 쓰는 프로젝트 | 다음 작업 전에 Claude에게 git을 설정하고 첫 커밋을 만들어 달라고 해요. 그때부터 모든 변경이 두 명령에 보여요 |
| 지금 눈앞의 변경 | 평범한 말로 리뷰를 요청해요 (2장 참고) |
| 다른 버전 관리 시스템을 쓸 때 | `/diff`와 `/code-review`는 변경을 못 봐요. 그래도 내 도구로 변경을 읽고, Claude가 건드린 파일을 리뷰해 달라고 말로 요청하면 돼요. `/rewind`는 Claude가 자기 편집을 추적하므로 여전히 동작해요 |

### 강의 속 연습 — 요약과 diff 비교하기

signup form 작업 예시예요. 요청은 "이메일 형식을 확인하고 비밀번호를 8자 이상으로 요구하며, inline 오류 메시지를 보여 주는 입력 검증 추가"였어요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">📝 Claude의 요약 (전부 사실)</span>
    <ul>
      <li>제출 전에 이메일 형식과 비밀번호 길이를 확인</li>
      <li>각 필드 아래에 inline 오류 메시지 표시</li>
      <li>모든 테스트 통과</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">📂 실제로 바뀐 파일: 8개</span>
    <ul>
      <li><code>src/components/SignupForm.tsx</code> (+34 −6)</li>
      <li><code>src/lib/validate.ts</code> (new, +14)</li>
      <li><code>src/lib/validate.test.ts</code> (new, +31)</li>
      <li><code>src/components/SignupForm.test.tsx</code> (+2 −8)</li>
      <li><code>src/styles/form.css</code> (+9)</li>
      <li><code>package.json</code> (+1)</li>
      <li><code>package-lock.json</code> (+14)</li>
      <li><code>src/config.ts</code> (+2 −1)</li>
    </ul>
  </div>
</div>

연습은 이 중 **다시 봐야 할 파일 세 개**를 고르는 거예요. 교훈은 **요약이 참이어도, 따져 봐야 할 변경이 빠져 있을 수 있다**는 점이에요.

## 2. second opinion 받기 — `/code-review`

<div class="oa-eli">
  <span class="oa-eli__head">🩺 쉽게 말하면</span>
  <p>큰 수술 전에 <b>다른 병원에서 소견을 한 번 더</b> 받는 것과 같아요. 처음 진단한 의사의 선입견이 없는 사람이 봐야 놓친 게 보여요.</p>
</div>

긴 세션은 그동안 읽고 결정한 모든 것을 짊어지고 있어요. 바로 그 기록이 **리뷰어에게는 없어야 하는 것**이에요. `/code-review`가 그 두 번째 리뷰어예요.

```text
/code-review
```

| 특징 | 내용 |
|---|---|
| **깨끗한 context** | 내 세션의 기록 없이 변경을 리뷰하고, 찾은 것을 보고해요 |
| **수정하지 않아요** | 요청하지 않는 한 아무것도 편집하지 않아요 |
| **백그라운드 실행** | 몇 초에서 몇 분까지 걸리고, 끝나면 결과가 대화에 도착해요 |
| **사용량에 포함** | 다른 작업처럼 사용량에 잡혀요. 다시 볼 가치가 있는 변경에 아껴 쓰세요 |

평범한 말로 요청해도 Claude가 같은 리뷰를 시작할 수 있어요.

```text
Review the changes you just made. Report problems; don't fix anything yet.
```

Claude가 리뷰를 시작하지 않고 그냥 inline으로 답하면, 명령을 직접 실행하세요.

### 더 가볍게 또는 더 깊게

| 명령 | 동작 |
|---|---|
| `/code-review low` | 가장 확신하는 finding만 보고해요. 오탐(false alarm)이 줄어요 |
| `/code-review high` | 더 넓게 훑고, 덜 확실한 finding도 포함할 수 있어요 |

입력한 레벨은 다른 레벨을 입력할 때까지 **이후 리뷰에도 기억**돼요.

### 예시 — signup form 변경의 리뷰 결과

8개 파일을 리뷰해서 finding 4개가 나왔어요.

| # | 위치 | 태그 | 내용 |
|---|---|---|---|
| 1 | `src/components/SignupForm.test.tsx:18` | correctness | 테스트 하나가 skip되고 assertion이 약해졌어요. 빈 값을 제출하는 form도 통과해요 |
| 2 | `src/lib/validate.ts:4` | correctness | `isValidEmail`이 앞뒤 공백을 trim하지 않아, 공백이 붙은 주소가 거부돼요 |
| 3 | `src/config.ts:1` | correctness | `API_URL`이 `localhost:3000`으로 하드코딩돼, 프로덕션 빌드가 개발 주소로 signup을 보내요 |
| 4 | `src/styles/form.css:1` | style | 오류 텍스트가 프로젝트의 `--color-danger` 변수 대신 hex 색상을 써요 |

## 3. finding을 어떻게 처리할까요

<div class="oa-eli">
  <span class="oa-eli__head">🚑 쉽게 말하면</span>
  <p>응급실의 <b>분류(triage)</b>와 같아요. 당장 치료할 환자, 검사를 더 해 봐야 할 환자, 나중에 외래로 와도 되는 환자를 나누죠. 모든 지적을 바로 고칠 필요는 없어요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔧</span><span class="oa-card__title">Fix now</span>진짜 문제이고, 중요하고, 반드시 고쳐야 해요</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">❓</span><span class="oa-card__title">Ask why</span>검증하기 어렵거나 뭔가 이상해 보이는 finding. 코드를 처음 읽는 리뷰어도 놓치는 게 있을 수 있어요</div>
  <div class="oa-card"><span class="oa-card__icon">📌</span><span class="oa-card__title">Leave it</span>진짜 문제지만 작거나 사소해요. 나중에 한 세션에서 묶어서 고치면 돼요</div>
</div>

**Ask why**는 finding을 Claude에게 그대로 인용하고 다시 확인해 달라고 하면 돼요. 위 2번 finding이라면 이렇게 써요.

```text
You reported that isValidEmail doesn't trim spaces, but line 4 calls trim(). Check again and tell me whether the finding stands.
```

수정을 요청할 때는 **증거도 함께** 요청하는 게 좋아요.

```text
Fix the first finding: don't skip the empty-email test and restore the original assertion. Then run the tests and show me the output.
```

> 수정이 커져서 그 자체로 큰 변경이 되면, **리뷰를 다시** 돌리세요.

### 강의 속 연습 — 네 finding 분류해 보기

강의는 각 finding에 코드를 보여 주고 세 더미 중 하나를 고르게 해요.

| # | 강의가 보여 주는 코드 | 노트 작성자의 풀이 |
|---|---|---|
| 1 | `it(...)`이 `it.skip(...)`으로 바뀌고, `toHaveBeenCalledWith({...})`가 `toHaveBeenCalled()`로 약해짐 | **Fix now** — 약해진 테스트는 매번 다시 볼 항목이에요 |
| 2 | 4번째 줄이 `return isEmail(value.trim());` | **Ask why** — finding과 달리 코드가 이미 `trim()`을 호출해요 |
| 3 | `import.meta.env.VITE_API_URL`이 `"http://localhost:3000"`으로 바뀜 | **Fix now** — 하드코딩 값이 프로덕션에 영향을 줘요 |
| 4 | `.field-error { color: #c0392b; ... }` | **Leave it** — 진짜지만 작은 스타일 문제예요 |

> 풀이 열은 강의 본문의 분류 기준과 예시 문장을 근거로 한 추론이에요. 연습 문제의 채점 결과를 직접 확인한 건 아니에요.

## 4. 언제 더 꼼꼼히 리뷰할까요

<div class="oa-eli">
  <span class="oa-eli__head">⚖️ 쉽게 말하면</span>
  <p>편의점 영수증은 훑어보고 말지만, <b>전세 계약서</b>는 한 줄씩 읽고 전문가에게도 보여 주죠. 리뷰의 깊이도 변경의 무게에 맞춰요.</p>
</div>

| 변경의 성격 | 리뷰 수준 |
|---|---|
| 단순한 한 줄 변경 | diff를 빠르게 훑는 것으로 충분한 경우가 많아요 |
| 머릿속에 다 담기 어려울 만큼 큰 변경 | 사람 리뷰 + Claude 리뷰 |
| 민감한 것을 건드리거나 파괴적인 일을 하는 변경 | 사람 리뷰 + Claude 리뷰 |
| 팀원에게 작업을 넘기기 전 | 사람 리뷰 + Claude 리뷰 |

### 이게 "Claude Code Review"와 같은 건가요?

아니에요. **Claude Code Review**는 팀을 위한 별도 제품이에요. Claude organization의 Owner가 GitHub 저장소에 켜면 pull request에 finding을 게시해요. 이 레슨의 내용에는 그 제품이 필요하지 않아요.

리뷰어가 **같은 문제를 여러 번** 지적한다면, Claude가 매 세션 시작 때 읽는 규칙으로 적어 두는 게 좋아요. 그 파일이 다음 레슨의 **CLAUDE.md**예요.

## 5. 예전 노트에 있던 내용 (이전 버전 강의)

<div class="oa-eli">
  <span class="oa-eli__head">🗂️ 쉽게 말하면</span>
  <p>교과서가 개정되면서 빠진 단원을 <b>부록</b>으로 남겨 둔 거예요. 현재 강의 본문에는 없으니, 쓰기 전에 공식 문서에서 지금도 유효한지 확인하세요.</p>
</div>

### subagent로 리뷰하기

PR을 push하기 전에 Claude에게 subagent로 변경 사항을 검토하게 하세요. subagent는 **자체 context window에서 새로운 시선**으로 동작하고, 방금 세션 내내 코드를 작성한 main agent의 편향을 갖지 않아요.

- code-reviewer subagent를 만들 때는 **읽기 전용 도구로 제한**하세요. 리뷰어는 문제를 지적해야지 파일을 수정하면 안 돼요.
- subagent 설정을 저장소에 체크인하면 **팀 전체가 같은 리뷰어**를 써요.

### `/commit-push-pr` skill

`/commit-push-pr` skill은 **커밋, push, PR 생성을 한 단계**로 처리해요. 각각을 수동으로 하는 대신 skill을 실행하면 Claude가 알아서 해 줘요.

CLAUDE.md에 채널이 명시된 Slack MCP 서버가 구성돼 있다면, PR 링크가 팀 채널에 자동으로 게시돼요.

### `--from-pr`로 세션 연결

Claude가 `gh pr create`로 PR을 만들면 세션이 그 PR에 자동으로 연결돼요. 나중에 리뷰 코멘트를 처리하거나 실패한 빌드를 고치러 돌아와야 하면 다음을 실행해요.

```text
claude --from-pr <PR_NUMBER>
```

떠났던 지점에서 그대로 이어져요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Claude의 요약이 전부 사실인데도 diff를 직접 봐야 하는 이유는?</summary>
  <p><b>정답: 참인 요약도 따져 봐야 할 변경을 빠뜨릴 수 있어요.</b> 그리고 코드를 쓴 세션은 그 변경의 가장 좋은 심사자가 아닌 경우가 많아요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. diff에서 매번 다시 봐야 할 세 가지는?</summary>
  <p><b>정답: 요청하지 않은 변경, 약해진 테스트, 새 패키지와 하드코딩 값.</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. <code>/code-review</code>가 같은 세션에게 "다시 봐 줘"라고 하는 것보다 나은 점은?</summary>
  <p><b>정답: 세션의 기록이 없는 깨끗한 context에서 리뷰해요.</b> 요청하지 않으면 아무것도 수정하지 않고 보고만 해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 리뷰어의 finding이 코드와 안 맞아 보여요. 어떻게 하나요?</summary>
  <p><b>정답: Ask why.</b> finding을 그대로 인용하고 다시 확인해 달라고 해요. 코드를 처음 읽는 리뷰어도 틀릴 수 있어요.</p>
</details>

## 정리

- 요약만 믿지 말고 **실제 diff**를 읽어요. `/diff`로 요청하지 않은 변경, 약해진 테스트, 새 패키지·하드코딩 값을 찾아요.
- 변경 전체가 잘못됐으면 `/rewind`로 되돌려요. 단, shell 명령이 바꾼 파일은 되돌려지지 않아요.
- `/code-review`(또는 평범한 말)로 **깨끗한 context의 second opinion**을 받아요. 리뷰어는 보고만 하고, 요청하지 않으면 수정하지 않아요.
- 각 finding을 **Fix now · Ask why · Leave it**으로 나누고, 수정을 요청할 때는 **증거**도 함께 요청해요.
- 변경이 크거나, 민감하거나, 파괴적이거나, 팀원에게 넘기기 전이라면 더 꼼꼼히 리뷰해요.
- 이전 버전 강의에서 다룬 subagent 리뷰어, `/commit-push-pr`, `--from-pr`은 현재 본문에는 없어요.
