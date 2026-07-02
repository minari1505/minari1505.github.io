---
title: "Skills vs. other Claude Code features"
title_ko: "Skill vs 다른 Claude Code 기능"
course: agent-skills
lesson: 4
---

## 학습 목표

- Skill·CLAUDE.md·Hooks·Subagents·MCP가 각각 어떤 문제를 푸는지 구분하기
- "어떤 상황에 뭘 쓸까"를 판단하는 기준 세우기
- Skill과 subagent를 결합하는 두 가지 방향 이해하기

## 한눈에 비교

| 기능 | 성격 | 로드/실행 방식 | 이럴 때 |
|---|---|---|---|
| **CLAUDE.md** | 항상 알아야 할 사실·규칙 | 매 세션 전체 로드 | 프로젝트 구조, 코딩 컨벤션 같은 "사실" |
| **Skill** | 특정 작업의 절차·전문지식 | 필요할 때만 본문 로드 | 반복하는 절차, 긴 참고 자료 |
| **Hooks** | 결정론적 자동화 | 도구 이벤트마다 항상 실행 | "저장할 때마다 반드시 lint" 같은 강제 규칙 |
| **Subagents** | 격리된 컨텍스트의 별도 에이전트 | 위임 시 생성 | 메인 대화를 오염시키면 안 되는 대규모 탐색 |
| **MCP** | 외부 시스템 연동 (도구 제공) | 서버 연결 | Slack·DB 같은 **외부 통합** |

기억하기 쉬운 구분법:

- **CLAUDE.md** — "우리 팀은 이래" (사실). 항상 로드되니 짧게.
- **Skill** — "이 작업은 이렇게 해" (절차). 쓸 때만 로드되니 길어도 OK.
- **Hook** — "무조건 이렇게 돼야 해" (강제). LLM 판단에 맡기지 않고 결정론적으로 실행.
- **MCP** — 외부 도구를 *연결*, **Skill** — 그 도구를 쓰는 워크플로우를 *가르침*. 서로 보완 관계!

## Slash command는 skill로 통합됐어요

**Custom slash commands는 skill과 하나로 합쳐졌습니다.**

- `.claude/commands/deploy.md`와 `.claude/skills/deploy/SKILL.md`는 둘 다 `/deploy`를 만들고 동일하게 동작합니다.
- 기존 `.claude/commands/` 파일도 계속 작동하고 같은 frontmatter를 지원해요.
- 다만 skill 쪽이 추가 기능이 있어 권장됩니다: **부속 파일 디렉토리**, **호출 제어**(누가 부를 수 있는지), **관련 상황에서의 자동 로드**.
- 같은 이름이 겹치면 skill이 command보다 우선합니다.

## Skill 컨텍스트 생명주기

Skill이 호출되면 렌더링된 `SKILL.md` 내용이 대화에 **하나의 메시지로 들어가 세션 내내 유지**됩니다.

- 이후 턴에서 파일을 다시 읽지 않으므로, 작업 내내 적용될 지침은 "일회성 단계"가 아니라 **상시 지침**으로 써야 해요.
- 본문은 간결하게! 로드된 skill의 모든 줄은 **매 턴 반복되는 토큰 비용**입니다.
- 자동 컴팩션 시에는 최근 호출된 skill들이 일정 토큰 예산 안에서 요약 뒤에 다시 붙습니다. 오래된 skill은 밀려날 수 있으니, 중요한 skill은 컴팩션 후 재호출하세요.

## Skill + Subagent: 두 방향의 결합

| 방식 | 시스템 프롬프트 | 작업(task) | 함께 로드 |
|---|---|---|---|
| Skill에 `context: fork` | 에이전트 타입에서 | **SKILL.md 내용** | CLAUDE.md (Explore/Plan 제외) |
| Subagent에 `skills` 필드 | 서브에이전트 본문 | Claude의 위임 메시지 | **미리 로드된 skill** + CLAUDE.md |

1. **Skill → Subagent** (`context: fork`): skill 본문이 서브에이전트를 움직이는 프롬프트가 됩니다. `agent` 필드로 실행 환경(Explore, Plan, general-purpose, 커스텀)을 고르세요.
2. **Subagent → Skill** (`skills` 필드): 서브에이전트 시작 시 skill **전체 내용**이 미리 주입됩니다. 일반 세션에서 description만 로드되는 것과 다른 점!

## 선택 가이드 요약

상황별로 정리하면:

- 프로젝트의 **불변 사실** → CLAUDE.md
- **반복 절차**, 긴 참고 자료 → Skill
- **예외 없이 강제**할 규칙 → Hook
- 외부 서비스 **연동** → MCP (+ 그 사용법은 Skill로)
- 컨텍스트 **격리**가 필요한 큰 작업 → Subagent (+ Skill로 지식 주입)

## 정리

- 사실은 CLAUDE.md, 절차는 Skill, 강제는 Hook, 외부 연동은 MCP, 격리는 Subagent.
- Slash commands ⊂ Skills — 이제 하나의 개념.
- Skill 본문은 세션 내내 컨텍스트에 남는다 → 간결하게 쓰기.
