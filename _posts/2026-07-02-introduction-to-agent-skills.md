---
title: "[CCA-F] Introduction to Agent Skills 정리 ✏️"
date: 2026-07-02
categories:
  - CCA-F
tags:
  - Agent-Skills
  - Claude-Code
  - Anthropic
---

Anthropic Academy(Skilljar)의 [Introduction to agent skills](https://anthropic.skilljar.com/introduction-to-agent-skills) 강의를 정리한 글이에요. CCA-F 자격증 준비 첫 번째 강의! 🎓

강의는 총 6개 모듈로 구성돼요.

1. What are skills?
2. Creating your first skill
3. Configuration and multi-file skills
4. Skills vs. other Claude Code features
5. Sharing skills
6. Troubleshooting skills

---

## 1. Skill이란? (What are skills?)

**Skill = Claude가 특정 작업을 더 잘하도록 만들어주는, 재사용 가능한 마크다운 지시서 폴더**예요.

- 폴더 하나에 `SKILL.md`(필수) + 참고 문서, 스크립트 같은 부속 파일을 담아요.
- Claude가 대화 내용을 보고 **관련 있을 때 알아서 불러 쓰거나**, 사용자가 `/skill-name`으로 직접 호출할 수 있어요.
- 새 직원에게 주는 **온보딩 가이드**에 비유돼요. 범용 에이전트에게 도메인 전문 지식을 얹어 특화 에이전트로 만드는 방법이에요.

### 언제 Skill을 만들까?

- 같은 지시사항/체크리스트/여러 단계 절차를 **채팅에 반복해서 붙여넣고 있을 때**
- CLAUDE.md의 한 섹션이 "사실(fact)"이 아니라 **"절차(procedure)"로 커져버렸을 때**

CLAUDE.md와 달리 skill 본문은 **사용될 때만 컨텍스트에 로드**되기 때문에, 긴 참고 자료를 넣어도 평소에는 토큰 비용이 거의 없어요.

### 핵심 설계: 점진적 공개 (Progressive Disclosure)

Skill이 토큰을 아끼는 비결은 3단계로 정보를 나눠 로드하는 구조예요.

| 단계 | 내용 | 로드 시점 |
|---|---|---|
| ① 메타데이터 | frontmatter의 `name` + `description` | 세션 시작 시 항상 (매우 가벼움) |
| ② SKILL.md 본문 | 실제 지시사항 | Claude가 필요하다고 판단할 때 |
| ③ 부속 파일 | reference.md, 스크립트 등 | 해당 상황에서만 |

덕분에 skill에는 사실상 **무제한 분량의 자료**를 담을 수 있어요. 한꺼번에 로드하지 않으니까요.

---

## 2. 첫 Skill 만들기 (Creating your first skill)

### 어디에 만들까?

| 위치 | 경로 | 적용 범위 |
|---|---|---|
| Enterprise | managed settings | 조직 전체 |
| Personal | `~/.claude/skills/<이름>/SKILL.md` | 내 모든 프로젝트 |
| Project | `.claude/skills/<이름>/SKILL.md` | 해당 프로젝트만 |
| Plugin | `<plugin>/skills/<이름>/SKILL.md` | 플러그인 활성화된 곳 |

이름이 겹치면 **Enterprise > Personal > Project** 순으로 우선해요. 플러그인 skill은 `plugin-name:skill-name` 네임스페이스를 쓰니 충돌하지 않아요.

### 최소 구성

```bash
mkdir -p ~/.claude/skills/summarize-changes
```

```markdown
---
description: Summarizes uncommitted changes and flags anything risky.
  Use when the user asks what changed, wants a commit message,
  or asks to review their diff.
---

## Current changes

!`git diff HEAD`

## Instructions

Summarize the changes above in two or three bullet points,
then list any risks you notice.
```

- `---` 사이 **YAML frontmatter**: Claude가 언제 이 skill을 쓸지 판단하는 정보
- 그 아래 **마크다운 본문**: skill이 실행될 때 Claude가 따르는 지시사항
- **디렉토리 이름이 곧 명령어**가 돼요 → `/summarize-changes`

### description이 제일 중요해요 ⭐

Claude는 오직 `description`을 보고 skill을 자동으로 쓸지 결정해요. 좋은 description의 조건:

- **무엇을 하는지 + 언제 쓰는지**를 모두 담기
- 사용자가 실제로 말할 법한 **자연스러운 키워드** 포함하기 ("what changed", "commit message" 등)
- 핵심 사용 사례를 **맨 앞에** 쓰기 (skill 목록에서 1,536자로 잘리기 때문)

### 테스트 방법 2가지

1. **자동 호출**: description에 맞는 질문을 던져보기 → "What did I change?"
2. **직접 호출**: `/summarize-changes` 입력

---

## 3. 설정과 멀티 파일 Skill (Configuration and multi-file skills)

### Frontmatter 주요 필드

모든 필드는 선택 사항이고, `description`만 강력 권장이에요.

| 필드 | 역할 |
|---|---|
| `name` | 목록에 표시될 이름 (기본값: 디렉토리명) |
| `description` | 무엇을/언제 — Claude의 자동 호출 판단 기준 |
| `disable-model-invocation: true` | Claude의 자동 호출 금지, 사용자만 `/이름`으로 실행 |
| `user-invocable: false` | `/` 메뉴에서 숨김, Claude만 호출 가능 |
| `allowed-tools` | skill 활성화 중 **허가 없이** 쓸 수 있는 도구 목록 |
| `disallowed-tools` | skill 활성화 중 사용 금지할 도구 |
| `model` / `effort` | 이 skill 실행 시 사용할 모델/추론 강도 |
| `context: fork` | 서브에이전트(격리된 컨텍스트)에서 실행 |
| `agent` | `context: fork`일 때 사용할 에이전트 타입 |
| `argument-hint` | 자동완성 때 보여줄 인자 힌트 |

### 누가 호출할 수 있는지 정리

| Frontmatter | 사용자 호출 | Claude 호출 | 컨텍스트 로드 |
|---|---|---|---|
| (기본) | ⭕ | ⭕ | description 상시, 본문은 호출 시 |
| `disable-model-invocation: true` | ⭕ | ❌ | description도 로드 안 됨 |
| `user-invocable: false` | ❌ | ⭕ | description 상시, 본문은 호출 시 |

- 배포(`/deploy`), 커밋처럼 **부수효과가 있는 작업** → `disable-model-invocation: true` ("코드가 준비돼 보인다고 Claude가 멋대로 배포하면 안 되니까!")
- 레거시 시스템 설명처럼 **명령이 아닌 배경지식** → `user-invocable: false`

### 인자 전달

```markdown
---
name: fix-issue
description: Fix a GitHub issue
---

Fix GitHub issue $ARGUMENTS following our coding standards.
```

- `/fix-issue 123` → `$ARGUMENTS`가 `123`으로 치환
- `$0`, `$1`(= `$ARGUMENTS[0]`, `$ARGUMENTS[1]`)로 위치별 접근도 가능
- `${CLAUDE_SKILL_DIR}` (skill 폴더 경로), `${CLAUDE_SESSION_ID}` 같은 변수도 지원

### 동적 컨텍스트 주입

`` !`command` `` 문법을 쓰면 **Claude가 읽기 전에** 셸 명령이 먼저 실행되고, 그 출력이 본문에 삽입돼요.

```markdown
## Pull request context
- PR diff: !`gh pr diff`
- PR comments: !`gh pr view --comments`
```

Claude가 명령을 실행하는 게 아니라 **전처리**라는 점이 포인트! Claude는 완성된 결과만 봐요. 추측이 아닌 실제 데이터에 근거한 답을 받을 수 있어요.

### 멀티 파일 구조

```text
my-skill/
├── SKILL.md        # 필수 — 개요와 내비게이션
├── reference.md    # 상세 API 문서 — 필요할 때만 로드
├── examples.md     # 사용 예시 — 필요할 때만 로드
└── scripts/
    └── helper.py   # 실행되는 스크립트 — 컨텍스트에 로드 안 됨!
```

- `SKILL.md`는 **500줄 이하**로 유지, 상세 자료는 별도 파일로
- 부속 파일은 SKILL.md에서 링크로 언급해 "언제 뭘 열어볼지" 알려주기
- 스크립트는 **컨텍스트를 쓰지 않고 실행**되는 게 장점 — 예: PDF 폼 필드 추출 스크립트는 PDF 전체를 컨텍스트에 올리지 않고 결정론적으로 처리

---

## 4. Skill vs 다른 Claude Code 기능들

이 강의에서 가장 시험에 나올 것 같은 부분! 🔥 언제 뭘 쓰는지 구분하는 게 핵심이에요.

| 기능 | 성격 | 로드 방식 | 이럴 때 |
|---|---|---|---|
| **CLAUDE.md** | 항상 알아야 할 사실·규칙 | 매 세션 전체 로드 | 프로젝트 구조, 코딩 컨벤션 같은 "사실" |
| **Skill** | 특정 작업의 절차·전문지식 | 필요할 때만 본문 로드 | 반복하는 절차, 긴 참고 자료 |
| **Hooks** | 결정론적 자동화 | 도구 이벤트에 항상 실행 | "저장할 때마다 반드시 lint" 같은 강제 규칙 |
| **Subagents** | 격리된 컨텍스트의 별도 에이전트 | 위임 시 생성 | 메인 대화를 오염시키면 안 되는 대규모 탐색 |
| **MCP** | 외부 시스템 연동 (도구 제공) | 서버 연결 | Slack, DB 등 **외부 통합** |

기억하기 쉬운 구분법:

- **CLAUDE.md**: "우리 팀은 이래" (사실) — 항상 로드되니 짧게
- **Skill**: "이 작업은 이렇게 해" (절차) — 쓸 때만 로드되니 길어도 OK
- **Hook**: "무조건 이렇게 돼야 해" (강제) — LLM 판단에 안 맡기고 결정론적으로 실행
- **MCP**: 외부 도구 연결, **Skill**: 그 도구를 쓰는 복잡한 워크플로우를 가르치기 → 서로 보완 관계!

참고로 **custom slash commands는 skill로 통합**됐어요. `.claude/commands/deploy.md`와 `.claude/skills/deploy/SKILL.md` 둘 다 `/deploy`를 만들고 동일하게 동작해요. skill 쪽이 부속 파일, 호출 제어 같은 추가 기능이 있어서 권장돼요.

### Skill + Subagent 조합

두 방향으로 결합할 수 있어요.

1. **Skill에 `context: fork`**: skill 본문이 서브에이전트의 프롬프트가 됨 (대화 히스토리와 격리)

    ```markdown
    ---
    name: deep-research
    description: Research a topic thoroughly
    context: fork
    agent: Explore
    ---

    Research $ARGUMENTS thoroughly...
    ```

2. **Subagent에 `skills` 필드**: 서브에이전트 시작 시 skill 전체 내용을 미리 로드해서 참고 자료로 사용

⚠️ `context: fork`는 **명시적인 작업 지시가 있는 skill**에만 의미가 있어요. "이 컨벤션을 따라라" 같은 가이드라인만 있으면 서브에이전트가 할 일이 없어서 빈손으로 돌아와요.

---

## 5. Skill 공유하기 (Sharing skills)

배포 범위에 따라 3가지 방법이 있어요.

| 방법 | 대상 | 하는 법 |
|---|---|---|
| **Project skills** | 같은 저장소를 쓰는 팀원 | `.claude/skills/`를 git에 커밋 |
| **Plugins** | 여러 팀/저장소 | 플러그인의 `skills/` 디렉토리에 포함, 마켓플레이스로 배포 |
| **Managed (Enterprise)** | 조직 전체 | managed settings로 강제 배포 |

- 프로젝트 skill은 저장소를 clone한 팀원 모두에게 자동 적용 (워크스페이스 신뢰 승인 후)
- 플러그인 skill은 `plugin-name:skill-name`으로 네임스페이스 분리
- 🔒 **보안 주의**: skill은 지시문 + 코드로 새 능력을 부여하는 만큼, **신뢰할 수 있는 출처의 skill만 설치**하고 코드·의존성·외부 네트워크 접근 지시가 없는지 검토해야 해요. 프로젝트 skill의 `allowed-tools`는 저장소를 신뢰(trust)한 후에만 발효돼요.

---

## 6. 트러블슈팅 (Troubleshooting skills)

### Skill이 트리거되지 않을 때

1. description에 사용자가 **자연스럽게 말할 키워드**가 들어있는지 확인
2. "What skills are available?"라고 물어서 skill이 목록에 있는지 확인
3. description에 가깝게 요청을 바꿔 말해보기
4. `/skill-name`으로 직접 호출해보기

frontmatter YAML이 깨져 있으면 본문은 로드되지만 description이 비어서 자동 매칭이 안 돼요. `claude --debug`로 파싱 에러를 확인할 수 있어요.

### Skill이 너무 자주 트리거될 때

1. description을 **더 구체적으로** 좁히기
2. 수동 호출만 원하면 `disable-model-invocation: true` 추가

### description이 잘려 보일 때

- skill이 많으면 컨텍스트 예산(기본: 컨텍스트 윈도우의 1%)에 맞춰 description이 잘리거나 드롭돼요
- `/doctor`로 어떤 skill이 영향받는지 확인
- 핵심 사용 사례를 description **맨 앞에** 쓰는 습관이 여기서도 도움돼요

### 잘 만든 skill인지 평가하기

트리거됐다는 것 ≠ 의도대로 동작한다는 것! 두 가지를 따로 측정해요.

1. **트리거 정확도**: 원하는 프롬프트에서 호출되는가 (+ 원치 않는 데서 호출 안 되는가)
2. **출력 품질**: 호출됐을 때 결과물이 기대에 맞는가

방법은 **베이스라인 비교**: 현실적인 프롬프트 몇 개를 skill 있는 세션 / 없는 세션에서 각각 돌려 비교. 공식 `skill-creator` 플러그인이 이 평가 루프(테스트 케이스 → 격리 실행 → 채점 → 벤치마크 → A/B 비교)를 자동화해줘요.

---

## 마무리 요약 🌸

- **Skill = 재사용 가능한 지시서 폴더.** `SKILL.md` 하나면 시작할 수 있다.
- **점진적 공개(progressive disclosure)** 덕분에 방대한 자료도 토큰 걱정 없이 담는다.
- **description이 생명.** 무엇을/언제를 키워드와 함께, 핵심을 맨 앞에.
- **사실은 CLAUDE.md, 절차는 Skill, 강제는 Hook, 외부 연동은 MCP.**
- 부수효과 있는 작업엔 `disable-model-invocation: true`, 배경지식엔 `user-invocable: false`.
- 공유는 **프로젝트 커밋 → 플러그인 → 조직 managed settings** 순으로 범위 확장.
- 안 되면: description 키워드 점검 → 목록 확인 → `/이름` 직접 호출 → `--debug`.

다음 강의 정리에서 만나요! 👋
