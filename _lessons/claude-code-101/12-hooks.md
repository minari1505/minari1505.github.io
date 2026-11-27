---
title: "Hooks"
title_ko: "Hooks"
course: claude-code-101
lesson: 12
---

## 학습 목표

- hook이 무엇이고 왜 **"결정론적(deterministic)"**인지 이해하기
- 주요 **hook 이벤트**와 구성 방법 파악하기
- **PreToolUse**로 위험한 작업을 차단하는 원리(exit code) 이해하기
- hook을 **팀과 공유**하는 방법 익히기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>이벤트 고르기</strong>PreToolUse · PostToolUse 등</li>
  <li><strong>matcher 설정</strong>어떤 tool에 적용할지 (선택)</li>
  <li><strong>명령 지정</strong>실행할 command</li>
  <li><strong>매번 실행</strong>예외 없이, 결정론적으로</li>
  <li><strong>팀과 공유</strong><code>.claude/settings.json</code>을 저장소에 체크인</li>
</ol>

이 레슨을 한 문장으로 줄이면: **무언가가 실패 없이 매번 일어나야 한다면, prompt가 아니라 hook에 넣어요.**

## 1. 왜 hook을 쓰나요

<div class="oa-eli">
  <span class="oa-eli__head">🚪 쉽게 말하면</span>
  <p>"나갈 때 문 잠가 주세요"라는 <b>쪽지</b>는 대부분 지켜지지만 가끔 잊혀요. <b>자동 잠금 도어락</b>은 문이 닫히면 무조건 잠겨요. CLAUDE.md가 쪽지라면 hook은 도어락이에요.</p>
</div>

**Hooks**는 Claude Code 생명주기(lifecycle)의 특정 지점에서 명령을 실행하게 해 줘요. 이 과정의 다른 모든 것과 결정적으로 다른 점은, hook은 **결정론적(deterministic)**이라 **항상 실행된다**는 거예요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">📝 CLAUDE.md에 적기</span>
    <ul>
      <li>"파일 편집 후 Prettier를 실행하라"</li>
      <li>대개는 실행돼요</li>
      <li>하지만 <b>가끔은 안 돼요</b></li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🪝 hook으로 걸기</span>
    <ul>
      <li>같은 일을 hook으로 설정</li>
      <li>예외 없이 <b>매번</b> 실행돼요</li>
    </ul>
  </div>
</div>

대표적인 활용은 네 가지예요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">✨</span><span class="oa-card__title">자동 포매팅</span>파일 편집 후 포매터 실행</div>
  <div class="oa-card"><span class="oa-card__icon">📜</span><span class="oa-card__title">로깅</span>규정 준수(compliance)를 위해 실행된 모든 명령을 기록</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⛔</span><span class="oa-card__title">위험한 작업 차단</span>프로덕션 파일 수정 같은 작업을 막기</div>
  <div class="oa-card"><span class="oa-card__icon">🔔</span><span class="oa-card__title">알림</span>Claude가 작업을 끝냈을 때 나에게 알림 보내기</div>
</div>

## 2. 동작 방식

<div class="oa-eli">
  <span class="oa-eli__head">⏰ 쉽게 말하면</span>
  <p>스마트홈 자동화 규칙과 같아요. <b>"언제"</b>(현관문이 열리면) + <b>"무엇에"</b>(거실 조명만) + <b>"무엇을"</b>(불 켜기)을 정해 두면 그대로 돌아가요.</p>
</div>

Hook은 `settings.json`에 구성해요. 정하는 건 세 가지예요.

| 구성 요소 | 뜻 | 필수 여부 |
|---|---|---|
| **이벤트** | 생명주기의 어느 지점에서 실행할지 | 필수 |
| **matcher** | 어떤 tool에 적용할지 | 선택 |
| **command** | 실행할 명령 | 필수 |

가장 흔히 쓰는 이벤트는 다음과 같아요.

| 이벤트 | 실행 시점 |
|---|---|
| **PreToolUse** | tool call **전**에 실행 |
| **PostToolUse** | tool call이 **완료된 후** 실행 |
| **UserPromptSubmit** | prompt를 제출했을 때, Claude가 처리하기 **전**에 실행 |
| **Stop** | Claude가 응답을 **마칠 때** 실행 |
| **Notification** | Claude가 **알림을 보낼 때** 실행 |

이건 일부일 뿐이에요. Claude Code는 훨씬 많은 이벤트를 지원하고, 전체 목록은 hooks reference에 있어요.

### 구성하고 확인하기

| 할 일 | 방법 |
|---|---|
| hook **구성** | `settings.json`을 직접 편집해요 |
| hook **확인** | Claude Code 안에서 `/hooks` 명령. 구성된 hook의 **읽기 전용 목록**이 열려서, 새 hook이 잘 들어갔는지 볼 수 있어요 |

> 예전 노트에는 "`/hooks` 명령으로도 구성한다"고 적혀 있었어요. 현재 강의 본문에서 `/hooks`는 **읽기 전용 목록**이고, 구성은 `settings.json` 편집으로 해요.

## 3. 실전 예시 — 편집 후 자동 포매팅

<div class="oa-eli">
  <span class="oa-eli__head">🧽 쉽게 말하면</span>
  <p>요리가 끝날 때마다 <b>자동으로 돌아가는 식기세척기</b>예요. 누가 요리했든, 무슨 요리든, 끝나면 무조건 설거지가 돼요.</p>
</div>

가장 흔한 hook은 편집 후 자동 포매팅이에요.

<ol class="oa-flow">
  <li><strong>이벤트</strong>PostToolUse</li>
  <li><strong>matcher</strong><code>"Edit|Write"</code> — Claude가 Edit 또는 Write tool을 쓴 뒤에 발동</li>
  <li><strong>command</strong>파일 확장자를 확인해 맞는 포매터 실행</li>
</ol>

| 파일 종류 | 포매터 |
|---|---|
| TypeScript | Prettier |
| Go | gofmt |
| 그 외 | 프로젝트가 쓰는 포매터 |

> 예전 노트의 matcher는 `"Edit|MultiEdit|Write"`였어요. 현재 강의 본문은 `"Edit|Write"`예요.

## 4. PreToolUse로 차단하기

<div class="oa-eli">
  <span class="oa-eli__head">🛂 쉽게 말하면</span>
  <p>공항 <b>보안 검색대</b>예요. 통과(0), "이건 반입 금지예요, 이유는 이래요"라며 돌려보내기(2), 경고만 하고 통과(그 외)가 있어요.</p>
</div>

PreToolUse hook은 tool call이 **실행되기 전에 차단**할 수 있어요.

<ol class="oa-flow">
  <li><strong>입력</strong>hook이 tool 이름과 입력을 JSON으로 stdin에서 받아요</li>
  <li><strong>판단</strong>스크립트가 규칙에 맞는지 검사해요</li>
  <li><strong>exit code</strong>종료 코드가 동작을 결정해요</li>
</ol>

| exit code | 동작 | 누구에게 무엇이 전달되나 |
|---|---|---|
| **0** | 정상 진행 | — |
| **2** | 작업 **차단** | stderr 메시지가 **Claude에게 피드백**으로 전달돼요. 왜 막혔는지 알고 조정할 수 있어요 |
| **그 외** | 차단하지 않는 오류(non-blocking error) | **사용자에게** 표시되지만 아무것도 멈추지 않아요 |

이렇게 **하드 규칙**을 강제해요.

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🗂️</span><span class="oa-card__title">프로덕션 config 쓰기 차단</span>프로덕션 config 디렉터리에 쓰는 것을 막아요</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">💣</span><span class="oa-card__title"><code>rm -rf</code> 차단</span><code>rm -rf</code>가 포함된 bash 명령을 막아요</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🌿</span><span class="oa-card__title">main 커밋 차단</span>main에 커밋하는 것을 막아요</div>
</div>

팀에 필요한 것을 **"권고(suggested)"가 아닌 "보장(guaranteed)"**으로 만드는 방법이에요.

## 5. 팀과 hook 공유하기

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면</span>
  <p>내 방에만 붙인 규칙이 아니라 <b>건물 관리 규약</b>에 넣는 거예요. 새로 이사 온 사람도 자동으로 같은 규칙을 적용받아요.</p>
</div>

| 항목 | 내용 |
|---|---|
| **위치** | `.claude/settings.json`에 구성한 hook은 **프로젝트 수준**이에요 |
| **공유** | 저장소에 체크인할 수 있어요. 팀 전체가 같은 hook을 자동으로 갖게 돼요 |
| **스크립트 참조** | 명령에서 `CLAUDE_PROJECT_DIR` 환경 변수를 써서 프로젝트에 저장된 스크립트를 참조해요 |

`CLAUDE_PROJECT_DIR`을 쓰면 **Claude의 현재 작업 디렉터리와 무관하게** 스크립트가 동작해요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. "파일 편집 후 Prettier 실행"을 CLAUDE.md가 아니라 hook에 넣어야 하는 이유는?</summary>
  <p><b>정답: hook은 결정론적이라 예외 없이 매번 실행되기 때문이에요.</b> CLAUDE.md에 적으면 대개는 실행되지만 가끔은 안 돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. PreToolUse hook이 exit code 2로 끝나면 어떻게 되나요?</summary>
  <p><b>정답: 작업이 차단되고, stderr 메시지가 Claude에게 피드백으로 전달돼요.</b> Claude가 왜 막혔는지 알고 조정할 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. hook 스크립트가 exit code 1로 끝났어요. tool call은 막히나요?</summary>
  <p><b>정답: 아니요.</b> 0과 2가 아닌 exit code는 차단하지 않는 오류예요. 사용자에게 표시되지만 아무것도 멈추지 않아요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 팀 전체가 같은 hook을 쓰게 하려면 어디에 구성하나요?</summary>
  <p><b>정답: <code>.claude/settings.json</code>에 구성하고 저장소에 체크인해요.</b> 스크립트 경로는 <code>CLAUDE_PROJECT_DIR</code> 환경 변수로 참조해요.</p>
</details>

## 정리

- Hook은 Claude Code 동작에 **결정론적 통제**를 줘요. 항상 실행돼요.
- **PostToolUse**는 자동 포매팅·로깅에, **PreToolUse**는 위험한 작업 차단에 써요.
- PreToolUse에서 **exit code 0은 진행, 2는 차단(stderr가 Claude에게 전달), 그 외는 차단하지 않는 오류**예요.
- `settings.json`에서 구성하고, `/hooks`로 구성된 목록을 확인해요.
- `.claude/settings.json`을 저장소에 체크인해 팀과 공유하고, 스크립트는 `CLAUDE_PROJECT_DIR`로 참조해요.
- 무언가가 실패 없이 매번 일어나야 한다면, prompt가 아니라 **hook**에 넣으세요.
