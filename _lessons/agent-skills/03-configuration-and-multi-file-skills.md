---
title: "Configuration and multi-file skills"
title_ko: "설정과 멀티 파일 Skill"
course: agent-skills
lesson: 3
---

## 학습 목표

- frontmatter 필드로 skill의 동작(호출 제어·도구 권한·실행 컨텍스트) 설정하기
- 인자 전달(`$ARGUMENTS`)과 동적 컨텍스트 주입(`` !`command` ``) 활용하기
- 부속 파일과 스크립트로 컨텍스트 효율이 좋은 멀티 파일 skill 구성하기

## 한눈에 보는 설정

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🚦</span><span class="oa-card__title">호출 제어</span><code>disable-model-invocation</code>, <code>user-invocable</code></div>
  <div class="oa-card"><span class="oa-card__icon">🔑</span><span class="oa-card__title">도구 권한</span><code>allowed-tools</code>, <code>disallowed-tools</code></div>
  <div class="oa-card"><span class="oa-card__icon">📥</span><span class="oa-card__title">입력</span><code>$ARGUMENTS</code>, <code>!`command`</code></div>
  <div class="oa-card"><span class="oa-card__icon">📁</span><span class="oa-card__title">멀티 파일</span>SKILL.md는 얇게, 상세는 부속 파일로</div>
  <div class="oa-card"><span class="oa-card__icon">🧬</span><span class="oa-card__title">격리 실행</span><code>context: fork</code></div>
</div>

## Frontmatter 주요 필드

모든 필드는 선택 사항이고, `description`만 강력 권장이에요.

| 필드 | 역할 |
|---|---|
| `name` | 목록에 표시될 이름 (기본값: 디렉토리명) |
| `description` | 무엇을/언제 — Claude의 자동 호출 판단 기준 |
| `disable-model-invocation: true` | Claude의 자동 호출 금지 — 사용자만 `/이름`으로 실행 |
| `user-invocable: false` | `/` 메뉴에서 숨김 — Claude만 호출 가능 |
| `allowed-tools` | skill 활성화 중 **허가 요청 없이** 쓸 수 있는 도구 목록 |
| `disallowed-tools` | skill 활성화 중 사용 금지할 도구 |
| `model` / `effort` | 이 skill 실행 시 쓸 모델 / 추론 강도 |
| `context: fork` | 서브에이전트(격리된 컨텍스트)에서 실행 |
| `agent` | `context: fork`일 때 사용할 에이전트 타입 |
| `argument-hint` | 자동완성 때 보여줄 인자 힌트 (예: `[issue-number]`) |

## 누가 호출할 수 있는지 제어하기

기본값은 "사용자도, Claude도 호출 가능"이에요. 두 필드로 제한할 수 있어요.

| Frontmatter | 사용자 호출 | Claude 호출 | 컨텍스트 로드 |
|---|---|---|---|
| (기본) | ⭕ | ⭕ | description 상시, 본문은 호출 시 |
| `disable-model-invocation: true` | ⭕ | ❌ | description도 로드 안 됨 |
| `user-invocable: false` | ❌ | ⭕ | description 상시, 본문은 호출 시 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label"><code>disable-model-invocation: true</code></span>
    <ul>
      <li><b>부수효과가 있는 작업</b> (<code>/deploy</code>, <code>/commit</code>, 메시지 전송)</li>
      <li><i>"코드가 준비돼 보인다고 Claude가 멋대로 배포하면 안 되니까!"</i></li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label"><code>user-invocable: false</code></span>
    <ul>
      <li><b>명령이 아닌 배경지식</b> (레거시 시스템 설명 등)</li>
      <li>Claude는 알아야 하지만 <code>/legacy-system-context</code>가 사용자에게 의미 있는 액션은 아니죠.</li>
    </ul>
  </div>
</div>

## 도구 권한: allowed-tools

`allowed-tools`에 적은 도구는 skill이 활성화된 동안 **매번 허가를 묻지 않고** 사용돼요.

<div class="oa-eli">
  <span class="oa-eli__head">🎫 쉽게 말하면</span>
  <p><code>allowed-tools</code>는 출입 금지 목록이 아니라 <b>프리패스</b>예요. 목록에 있는 도구는 묻지 않고 통과시키고, 목록에 없는 도구는 평소처럼 입구에서 허가를 받아요.</p>
</div>

```markdown
---
name: commit
description: Stage and commit the current changes
disable-model-invocation: true
allowed-tools: Bash(git add *) Bash(git commit *) Bash(git status *)
---
```

- 도구를 *제한*하는 게 아니라 *사전 승인*하는 필드예요. 목록에 없는 도구는 평소 권한 설정을 따라요.
- 반대로 도구를 빼고 싶으면 `disallowed-tools`를 쓰세요.

## 인자 전달

```markdown
---
name: fix-issue
description: Fix a GitHub issue
---

Fix GitHub issue $ARGUMENTS following our coding standards.
```

- `/fix-issue 123` → `$ARGUMENTS`가 `123`으로 치환돼요.
- 위치별 접근: `$ARGUMENTS[0]` 또는 축약형 `$0`, `$1` — `/migrate-component SearchBar React Vue`처럼 여러 인자를 받을 때 유용해요.
- 그 외 변수: `${CLAUDE_SKILL_DIR}` (skill 폴더 경로 — 번들 스크립트 참조용), `${CLAUDE_SESSION_ID}`, `${CLAUDE_PROJECT_DIR}` 등.

## 동적 컨텍스트 주입

`` !`command` `` 문법을 쓰면 skill 내용이 Claude에게 전달되기 **전에** 셸 명령이 실행되고, 그 출력이 자리를 대신해요.

```markdown
---
name: pr-summary
description: Summarize changes in a pull request
context: fork
agent: Explore
allowed-tools: Bash(gh *)
---

## Pull request context
- PR diff: !`gh pr diff`
- PR comments: !`gh pr view --comments`
- Changed files: !`gh pr diff --name-only`

## Your task
Summarize this pull request...
```

실행 순서는 다음과 같아요.

<ol class="oa-flow">
  <li><strong>명령 실행</strong>각 <code>!`command`</code>를 즉시 실행</li>
  <li><strong>치환</strong>출력이 그 자리를 대신함</li>
  <li><strong>전달</strong>Claude는 실제 데이터가 채워진 완성본을 받음</li>
</ol>

**Claude가 실행하는 게 아니라 전처리**라는 점이 핵심이에요. 추측이 아닌 실데이터 기반 응답을 보장해요.

## 멀티 파일 skill

`SKILL.md`는 개요와 내비게이션만 담고, 상세 자료는 별도 파일로 분리해요.

```text
my-skill/
├── SKILL.md        # 필수 — 개요와 내비게이션
├── reference.md    # 상세 API 문서 — 필요할 때만 로드
├── examples.md     # 사용 예시 — 필요할 때만 로드
└── scripts/
    └── helper.py   # 실행되는 스크립트 — 컨텍스트에 로드되지 않음!
```

```markdown
## Additional resources

- For complete API details, see [reference.md](reference.md)
- For usage examples, see [examples.md](examples.md)
```

- 💡 **`SKILL.md`는 500줄 이하**로 유지하고, 상세 참고 자료는 분리하세요.
- 부속 파일은 SKILL.md에서 링크로 언급해 "언제 뭘 열어볼지" Claude에게 알려주세요.
- 스크립트는 **컨텍스트를 쓰지 않고 실행**되는 게 큰 장점 — 대용량 데이터 처리, 결정론적 작업에 적합해요.
- 서로 배타적인 상황의 자료(예: 폼 채우기 vs 텍스트 추출)는 별도 파일로 나눠야 토큰이 절약돼요.

## 서브에이전트에서 실행: context: fork

`context: fork`를 붙이면 skill이 **격리된 컨텍스트**에서 실행돼요. skill 본문이 서브에이전트의 프롬프트가 되고, 대화 히스토리에는 접근하지 못해요.

```markdown
---
name: deep-research
description: Research a topic thoroughly
context: fork
agent: Explore
---

Research $ARGUMENTS thoroughly:

1. Find relevant files using Glob and Grep
2. Read and analyze the code
3. Summarize findings with specific file references
```

⚠️ `context: fork`는 **명시적인 작업 지시가 있는 skill**에만 의미가 있어요. "이 컨벤션을 따르라" 같은 가이드라인만 있으면 서브에이전트가 할 일이 없어 빈손으로 돌아와요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. <code>/deploy</code> skill을 Claude가 스스로 실행하지 못하게 하려면 어떤 필드를 쓰나요?</summary>
  <p><b>정답: <code>disable-model-invocation: true</code>.</b> 부수효과가 있는 작업은 사용자만 <code>/이름</code>으로 실행하게 해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. <code>allowed-tools</code>에 없는 도구는 skill 실행 중 쓸 수 없나요?</summary>
  <p><b>정답: 쓸 수 있어요.</b> 이 필드는 도구를 제한하는 것이 아니라 사전 승인해요. 목록에 없는 도구는 평소 권한 설정을 따라요. 빼고 싶으면 <code>disallowed-tools</code>를 써요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. "이 컨벤션을 따르라"는 가이드라인만 담긴 skill에 <code>context: fork</code>를 붙이면 어떻게 되나요?</summary>
  <p><b>정답: 서브에이전트가 할 일이 없어 빈손으로 돌아와요.</b> <code>context: fork</code>는 명시적인 작업 지시가 있는 skill에만 의미가 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 번들 스크립트(<code>scripts/helper.py</code>)의 장점은 무엇인가요?</summary>
  <p><b>정답: 컨텍스트를 쓰지 않고 실행돼요.</b> 대용량 데이터 처리나 결정론적 작업에 적합해요.</p>
</details>

## 정리

- 부수효과 있는 작업엔 `disable-model-invocation: true`, 배경지식엔 `user-invocable: false`.
- `allowed-tools` = 사전 승인, `$ARGUMENTS` = 인자, `` !`cmd` `` = 실행 전 데이터 주입.
- SKILL.md는 얇게(500줄 이하), 상세 자료는 부속 파일로, 무거운 처리는 스크립트로.
