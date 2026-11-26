---
title: "A CLAUDE.md That Follows"
title_ko: "지켜지는 CLAUDE.md 만들기"
course: claude-code-in-action
lesson: 6
---

## 학습 목표

- CLAUDE.md가 **강제 설정이 아니라 지침**이라는 점과, 파일이 길어질수록 잘 안 지켜지는 이유를 설명할 수 있다.
- CLAUDE.md가 놓이는 네 위치와 `@` import의 실제 효과를 구분할 수 있다.
- 규칙이 지켜지게 만드는 문장 작성법 세 가지를 적용할 수 있다.

## 한눈에 보는 원칙

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚧</span><span class="oa-card__title">강한 규칙은 hook으로</span>절대 넘으면 안 되는 선은 CLAUDE.md에 두지 않습니다.</div>
  <div class="oa-card"><span class="oa-card__icon">🗂️</span><span class="oa-card__title">import는 정리용</span>파일을 나눠도 읽는 양은 줄지 않습니다.</div>
  <div class="oa-card"><span class="oa-card__icon">✅</span><span class="oa-card__title">구체적이고 확인 가능하게</span>지켰는지 볼 수 있는 문장으로 씁니다.</div>
  <div class="oa-card"><span class="oa-card__icon">🔊</span><span class="oa-card__title">강조는 예산</span>두세 개 규칙에만 씁니다.</div>
</div>

이 레슨을 관통하는 한 문장: **파일이 간결할수록 Claude가 더 많이 따릅니다.**

## 왜 긴 CLAUDE.md는 무시되는가

문제가 생길 때마다 규칙을 하나씩 추가하다 보면 파일이 거대해지고, Claude가 일부를 지키지 않기 시작합니다. 버그가 아니라 파일의 성격 때문입니다.

CLAUDE.md는 **강제되는 설정이 아니라 지침**입니다. 모든 줄이 Claude의 주의를 두고 다른 줄과 경쟁합니다. 파일이 길수록 서로 경쟁이 심해지고, 규칙 하나하나가 지켜질 가능성이 떨어집니다.

<div class="oa-eli">
  <span class="oa-eli__head">📋 쉽게 말하면</span>
  <p>사무실 벽에 공지가 <b>3장</b> 붙어 있으면 모두 읽습니다. <b>300장</b>이 붙어 있으면 아무도 읽지 않습니다. 공지를 더 붙이는 것이 아니라 정말 필요한 것만 남겨야 읽힙니다.</p>
</div>

## 먼저 물을 것: CLAUDE.md가 맞는 도구인가

규칙을 쓰기 전에 그 규칙이 지침인지, 절대 넘으면 안 되는 선인지 구분합니다.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">CLAUDE.md에 "never push to main"</span>
    <ul>
      <li>Claude가 읽고 지켜 주기를 <b>기대</b>하는 것입니다.</li>
      <li>대부분 지키지만, 위험한 규칙에 "대부분"은 부족합니다.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">PreToolUse hook으로 차단</span>
    <ul>
      <li>hook은 행동 전에 실행되는 코드라서 <b>실제로 막습니다</b>.</li>
      <li>Claude가 push를 시도해도 hook이 멈춥니다.</li>
    </ul>
  </div>
</div>

강한 규칙은 hook으로 옮기고, CLAUDE.md는 부드러운 컨벤션을 맡깁니다.

## CLAUDE.md의 네 위치

네 위치의 파일은 시작할 때 **모두 함께 로드되어 쌓입니다.** 어느 하나가 빠지지 않습니다.

| 위치 | 누구의 것 | 특징 |
|---|---|---|
| Managed policy | 조직(플랫폼 팀) | 제외할 수 없습니다. 조직 정책이 항상 적용됩니다 |
| User | 나 | 내 컴퓨터의 모든 프로젝트에 따라옵니다 |
| Project | 팀 | 저장소에 커밋해 공유합니다 |
| Local | 나, 이 저장소만 | git이 무시합니다 |

Local은 놓치기 쉽지만 유용합니다. 내 브랜치에서 리팩터링하는 동안 Claude가 기억해야 할 설계 결정은 팀 전체에 영향을 주는 Project 파일이 아니라 Local에 둡니다.

## import로 큰 파일 나누기

```markdown
@.claude/conventions/code-style.md
@.claude/conventions/testing.md
@.claude/conventions/workflow.md
```

여기서 오해하기 쉬운 점이 있습니다. Claude는 시작할 때 import한 파일을 **그 자리에 펼쳐서** 읽습니다. 정리에는 도움이 되지만 모든 내용이 처음부터 로드됩니다.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">import가 해 주는 것</span>
    <ul><li>파일을 주제별로 정돈</li></ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">import가 해 주지 않는 것</span>
    <ul><li>Claude가 읽는 context의 양을 줄이기</li></ul>
  </div>
</div>

## 규칙이 지켜지게 쓰는 법

| 원칙 | ❌ | ✅ |
|---|---|---|
| 구체적이고 확인 가능하게 | "API route는 모범 사례를 따른다" | "새 API route는 `src/api/handlers`에 파일 하나당 하나씩 둔다" |
| 금지만 하지 말고 대안을 말한다 | "default export를 쓰지 않는다" | "default export 대신 named export를 쓴다" |
| 강조는 예산처럼 쓴다 | 모든 규칙에 "IMPORTANT" | 어기면 정말 아픈 두세 개에만 |

"IMPORTANT", "YOU MUST" 같은 말은 규칙의 우선순위를 올리지만, 주변의 조용한 규칙과 비교해서만 효과가 있습니다. 모든 규칙이 소리치면 아무것도 눈에 띄지 않습니다.

## 파일은 계속 고친다

CLAUDE.md는 완성되지 않는 파일입니다. Claude가 잘못 행동하면 손으로 고치고 넘어가지 말고 **CLAUDE.md에 대한 버그 리포트**로 다룹니다. "그 내용을 CLAUDE.md에 추가해"라고 말하면 Claude가 규칙을 써 줍니다. 그리고 설명할 수 없는 줄은 지웁니다.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. "절대 main에 push하지 않는다"는 규칙은 어디에 둬야 합니까?</summary>
  <p><b>정답: PreToolUse hook.</b> CLAUDE.md는 지침이라 대부분 지켜질 뿐입니다. hook은 행동 전에 실행되는 코드라 실제로 차단합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. CLAUDE.md가 너무 길어 <code>@</code> import로 세 파일로 나눴습니다. Claude가 읽는 context가 줄어듭니까?</summary>
  <p><b>정답: 줄지 않습니다.</b> import한 파일은 시작할 때 그 자리에 펼쳐져 모두 로드됩니다. import는 정리를 위한 것입니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 내 브랜치 작업 중에만 필요한 메모를 팀에 영향 없이 두려면 어느 위치입니까?</summary>
  <p><b>정답: Local.</b> git이 무시하는, 이 저장소만을 위한 개인 파일입니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 모든 규칙 앞에 "IMPORTANT"를 붙였더니 효과가 없습니다. 왜 그렇습니까?</summary>
  <p><b>정답: 강조는 주변보다 클 때만 효과가 있기 때문입니다.</b> 가장 중요한 두세 개 규칙에만 씁니다.</p>
</details>

## 핵심 정리

- CLAUDE.md는 지침입니다. 길어질수록 각 규칙이 덜 지켜집니다.
- 넘으면 안 되는 선은 hook으로 옮깁니다.
- 네 위치(Managed policy, User, Project, Local)는 모두 로드되어 쌓입니다.
- import는 정리용이고 context를 줄이지 않습니다.
- 규칙은 구체적이고 확인 가능하게, 대안을 명시해서, 강조는 아껴서 씁니다.
