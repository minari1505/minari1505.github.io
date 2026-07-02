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

## Frontmatter 주요 필드

모든 필드는 선택 사항이고, `description`만 강력 권장입니다.

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

기본값은 "사용자도, Claude도 호출 가능"입니다. 두 필드로 제한할 수 있어요.

| Frontmatter | 사용자 호출 | Claude 호출 | 컨텍스트 로드 |
|---|---|---|---|
| (기본) | ⭕ | ⭕ | description 상시, 본문은 호출 시 |
| `disable-model-invocation: true` | ⭕ | ❌ | description도 로드 안 됨 |
| `user-invocable: false` | ❌ | ⭕ | description 상시, 본문은 호출 시 |

- **부수효과가 있는 작업** (`/deploy`, `/commit`, 메시지 전송) → `disable-model-invocation: true`. *"코드가 준비돼 보인다고 Claude가 멋대로 배포하면 안 되니까!"*
- **명령이 아닌 배경지식** (레거시 시스템 설명 등) → `user-invocable: false`. Claude는 알아야 하지만 `/legacy-system-context`가 사용자에게 의미 있는 액션은 아니죠.

## 도구 권한: allowed-tools

`allowed-tools`에 적은 도구는 skill이 활성화된 동안 **매번 허가를 묻지 않고** 사용됩니다.

```markdown
---
name: commit
description: Stage and commit the current changes
disable-model-invocation: true
allowed-tools: Bash(git add *) Bash(git commit *) Bash(git status *)
---
```

- 도구를 *제한*하는 게 아니라 *사전 승인*하는 필드예요. 목록에 없는 도구는 평소 권한 설정을 따릅니다.
- 반대로 도구를 빼고 싶으면 `disallowed-tools`를 쓰세요.

## 인자 전달

```markdown
---
name: fix-issue
description: Fix a GitHub issue
---

Fix GitHub issue $ARGUMENTS following our coding standards.
```

- `/fix-issue 123` → `$ARGUMENTS`가 `123`으로 치환됩니다.
- 위치별 접근: `$ARGUMENTS[0]` 또는 축약형 `$0`, `$1` — `/migrate-component SearchBar React Vue`처럼 여러 인자를 받을 때 유용해요.
- 그 외 변수: `${CLAUDE_SKILL_DIR}` (skill 폴더 경로 — 번들 스크립트 참조용), `${CLAUDE_SESSION_ID}`, `${CLAUDE_PROJECT_DIR}` 등.

## 동적 컨텍스트 주입

`` !`command` `` 문법을 쓰면 skill 내용이 Claude에게 전달되기 **전에** 셸 명령이 실행되고, 그 출력이 자리를 대신합니다.

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

실행 순서: ① 각 `` !`command` `` 즉시 실행 → ② 출력이 자리를 치환 → ③ Claude는 실제 데이터가 채워진 완성본을 받음.

**Claude가 실행하는 게 아니라 전처리**라는 점이 핵심입니다. 추측이 아닌 실데이터 기반 응답을 보장해요.

## 멀티 파일 skill

`SKILL.md`는 개요와 내비게이션만 담고, 상세 자료는 별도 파일로 분리합니다.

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
- 스크립트는 **컨텍스트를 쓰지 않고 실행**되는 게 큰 장점 — 대용량 데이터 처리, 결정론적 작업에 적합합니다.
- 서로 배타적인 상황의 자료(예: 폼 채우기 vs 텍스트 추출)는 별도 파일로 나눠야 토큰이 절약됩니다.

## 서브에이전트에서 실행: context: fork

`context: fork`를 붙이면 skill이 **격리된 컨텍스트**에서 실행됩니다. skill 본문이 서브에이전트의 프롬프트가 되고, 대화 히스토리에는 접근하지 못해요.

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

⚠️ `context: fork`는 **명시적인 작업 지시가 있는 skill**에만 의미가 있습니다. "이 컨벤션을 따르라" 같은 가이드라인만 있으면 서브에이전트가 할 일이 없어 빈손으로 돌아와요.

## 정리

- 부수효과 있는 작업엔 `disable-model-invocation: true`, 배경지식엔 `user-invocable: false`.
- `allowed-tools` = 사전 승인, `$ARGUMENTS` = 인자, `` !`cmd` `` = 실행 전 데이터 주입.
- SKILL.md는 얇게(500줄 이하), 상세 자료는 부속 파일로, 무거운 처리는 스크립트로.
