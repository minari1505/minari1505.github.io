---
title: "Claude Code Exam Points"
title_ko: "Claude Code 시험 포인트 (도메인 3)"
course: cca-f-exam-prep
lesson: 7
tags:
  - CCA-F
  - Claude Code
  - CLAUDE.md
  - Hooks
---

## 학습 목표

- CLAUDE.md 계층, 200줄 규칙, `.claude/rules/`의 `paths:` lazy-load 알기
- `settings.json`의 permissions와 계층 우선순위, 메모리 4종 알기
- Grep vs Glob, 스킬 vs 커맨드 vs 서브에이전트 vs 훅을 시나리오에서 고르기
- PreToolUse 훅으로 결정론적 차단하는 구조와 CI/CD의 `claude -p` 알기

## 먼저 한 문장으로

**도메인 3은 "어디에 무엇을 두는가"와 "부탁할 일인가, 강제할 일인가"를 묻고, 강제할 일이면 답은 훅입니다.**

Claude Code 기본 사용법, CLAUDE.md, 스킬, 훅, 서브에이전트는 이 사이트의 [Claude Code 101](/courses/claude-code-101/01-what-is-claude-code/), [Introduction to agent skills](/courses/agent-skills/01-what-are-skills/), [Introduction to subagents](/courses/introduction-to-subagents/01-what-are-subagents/) 정리에 있습니다. 여기서는 그 위에 얹히는 시험 판단 포인트만 모았습니다.

## 실행 모드 3가지

| 모드 | 실행 | 용도 |
|---|---|---|
| 대화 | `claude` | 일반 개발 |
| 원샷 | `claude -p "프롬프트"` | 스크립트, CI/CD에서 비대화식 호출 |
| plan | 대화 중 Shift+Tab | 실행 전 계획 리뷰 (HITL 승인 기반의 예) |

"스크립트에서 한 번만 비대화식으로 호출" → `claude -p`. plan 모드는 중요한 리팩터링이나 파괴적 변경 전에 씁니다. `ls` 실행이나 도움말 보기에는 안 씁니다.

## CLAUDE.md 계층과 200줄 규칙

로드 순서: `~/.claude/CLAUDE.md`(사용자) → `<project>/CLAUDE.md`(프로젝트) → 하위 디렉터리 CLAUDE.md(그 아래를 편집할 때만 lazy-load).

CLAUDE.md는 항상 컨텍스트에 올라갑니다. 그래서 **200줄 이내**가 권장입니다. 500줄이 되면 Claude가 지시의 절반을 무시하는 일이 실제로 생깁니다. Git 용량이나 마크다운 파서 한계 때문이 아니라 준수율 때문입니다.

세부 규약은 `.claude/rules/*.md`로 나눕니다.

```markdown
---
paths:
  - "src/components/**"
  - "src/pages/**"
---
# 프론트엔드 규약
...
```

`paths:` 프론트매터가 있으면 그 glob에 맞는 파일을 다룰 때만 로드됩니다. 없으면 모든 세션에 로드됩니다(전역 규약용). "규칙은 `.claude/` 밖 아무 데나 둬도 된다"는 오답이고, 정해진 경로 `.claude/rules/`에 둬야 합니다.

시험 서술형 "500줄 CLAUDE.md를 개선하라"의 답 구조: ① 항상 필요한 것(개요, 커밋 규약, 전체 방침)만 CLAUDE.md에 남김 ② 프론트/백엔드/테스트/CI 규약을 `paths:` 붙은 rules 파일로 분할 ③ CLAUDE.md는 `@.claude/rules/frontend.md` 식으로 참조만. 결과는 상시 로드 50줄 + 필요할 때 관련 규칙 1~2개.

한 가지 알아 둘 동작: `paths` 규칙은 매칭 파일을 Read할 때 주입되고, 새 파일을 Write할 때는 주입되지 않는 경우가 보고되어 있습니다. "새 파일 만들 때 반드시 지킬 규약"은 CLAUDE.md 본문이나 paths 없는 규칙에 두는 편이 안전합니다.

## settings.json

`.claude/settings.json`(프로젝트), `~/.claude/settings.json`(사용자), `.claude/settings.local.json`(로컬, gitignore 권장). 우선순위는 **로컬 > 프로젝트 > 사용자**입니다.

```json
{
  "permissions": {
    "allow": ["Bash(npm test *)", "Bash(git status)", "Bash(git diff *)"],
    "deny": ["Bash(rm -rf *)", "Bash(sudo *)", "Bash(curl * | sh)", "Bash(git push --force *)"]
  },
  "env": { "NODE_ENV": "development" },
  "hooks": { ... }
}
```

- allow에 맞으면 승인 없이 실행, deny에 맞으면 **항상 거부**, 어느 쪽도 아니면 승인 프롬프트.
- deny에 넣을 대표: `rm -rf *`, `curl * | sh`(원격 코드 실행), `git push --force *`, `sudo *`, `npm publish *`.

조직 강제 설정은 `managed-settings.json`을 MDM 등으로 각 머신에 배포하는 방식이고, 사용자가 오버라이드할 수 없습니다.

## 메모리 4종

Claude Code는 `~/.claude/projects/<project_id>/memory/`에 파일로 기억을 저장합니다. `MEMORY.md`는 개별 메모리 파일로 가는 링크를 나열한 **인덱스**입니다.

| 종류 | 내용 |
|---|---|
| user | 사용자의 역할, 취향, 전문 영역 |
| feedback | 사용자가 준 피드백, 채택된 제안 |
| project | 진행 중인 작업, 팀 배경, 마감 |
| reference | 외부 시스템 포인터(Linear, Notion 등) |

세션 시작 시 MEMORY.md는 처음 200줄 또는 25KB까지만 로드됩니다. "Anthropic 클라우드에 저장된다", "Claude가 알아서 다 관리해 준다"는 오답입니다. 로컬 파일이고 사용자가 보고 고칠 수 있습니다.

## 내장 도구: Grep vs Glob

| 도구 | 찾는 대상 | 예 |
|---|---|---|
| Grep | 파일의 **내용**(ripgrep 기반, 정규식) | `calculateTax` 함수 정의 위치 |
| Glob | 파일 **이름/경로** 패턴 | `**/*.test.ts` 나열 |
| Read | 읽기 | 편집 전 대상 파악 |
| Edit | 일부를 문자열 치환 | 기존 파일 부분 수정 (사전에 Read) |
| Write | 신규 생성 / 전체 덮어쓰기 | 새 파일 |
| Bash | 셸 실행 | 테스트, 빌드, git |

"수만 파일에서 함수 정의 위치를 찾는다" → Grep. Glob은 이름만 찾고, 모든 파일을 cat해서 눈으로 보는 건 비효율. 이 내장 도구들이 서브에이전트 `allowedTools`의 기본 세트이기도 합니다.

## 확장점 4가지 고르기

| 확장점 | 위치 | 컨텍스트 | 실행 |
|---|---|---|---|
| 스킬 | `.claude/skills/<name>/SKILL.md` | 기존과 공유 | 트리거에 맞으면 자동 로드 |
| 슬래시 커맨드 | `.claude/commands/<name>.md` | 기존과 공유 | 사용자가 `/<name>`으로 호출 |
| 서브에이전트 | `.claude/agents/<name>.md` | **독립** | 부모가 위임 |
| 훅 | `settings.json`의 `hooks` | 해당 없음 | harness가 이벤트에 자동 실행 |

판단 흐름: 컨텍스트를 분리해야 하나 → 서브에이전트. 사용자가 직접 부르는 기능인가 → 커맨드. 상황에 맞춰 자동으로 지식이 붙어야 하나 → 스킬. 모델 판단과 무관하게 반드시 실행돼야 하나 → 훅.

시험 서술형 예:

- `/deploy`로 배포 절차 실행 → 슬래시 커맨드
- `migrations/` 편집 시 alembic 사용법 자동 로드 → 스킬(description에 TRIGGER 명시)
- 코드 생성과 병렬로 별도 에이전트가 테스트 작성 → 서브에이전트
- 파일 편집 후 자동 lint → 훅(PostToolUse, matcher `Edit|Write`)

참고: 2026년 4월(v2.1.101)부터 커스텀 슬래시 커맨드는 스킬로 통합됐습니다. 기존 `.claude/commands/`도 동작하지만 신규 작성은 스킬 방식이 권장이고, 같은 이름이면 스킬이 우선합니다. 시험 자료는 커맨드 기준으로 나옵니다.

## 스킬 프론트매터와 Progressive Disclosure

```markdown
---
name: db-migration
description: |
  DB 마이그레이션 작성과 실행.
  TRIGGER: "마이그레이션 작성", "DB 스키마 변경", 또는 migrations/ 아래 편집.
  SKIP: 순수 SELECT만 실행할 때.
allowed-tools: Read, Edit, Bash(alembic *)
---
```

- `allowed-tools`로 스킬 단위 최소 권한.
- SKILL.md에는 핵심 절차만, 상세는 `reference/*.md`에 두고 필요할 때만 읽게 하는 것이 Progressive Disclosure. 컨텍스트 소비를 줄이면서 상세에도 손이 닿습니다.
- 스킬이 안 로드되면 대개 description의 트리거가 모호하거나 name 충돌입니다.

커맨드에서 인자는 `$ARGUMENTS`(전체), `$1 $2`(위치)로 받습니다. 이름은 동사 기반 kebab-case(`/security-review`).

## 훅: 이벤트, matcher, 차단

| 이벤트 | 시점 | 특징 |
|---|---|---|
| PreToolUse | 툴 실행 직전 | **차단(deny)할 수 있는 유일한 지점** |
| PostToolUse | 툴 실행 직후 | 포매팅, 로그, 자동 테스트 |
| UserPromptSubmit | 프롬프트 제출 시 | 로깅, 새니타이징 |
| Stop | 턴 종료 | 알림, 요약 저장 |
| Notification | 알림 전송 시 | 외부 전달 |
| SessionStart | 세션 시작 | 환경 준비 |

훅을 실행하는 주체는 **Claude Code 본체(harness)**입니다. Claude 모델은 훅이 도는지 관여하지 않습니다. 그래서 모델 판단과 독립된 결정론적 자동화가 됩니다.

`matcher`로 대상을 좁힙니다. `"Edit|Write"`, `"Bash"`, `"Bash(git commit *)"`. 넓게 잡으면 이중 실행 같은 오탐이 납니다.

훅에는 툴 이름과 입력이 stdin에 JSON으로 들어옵니다.

```json
{ "tool_name": "Bash", "tool_input": { "command": "rm -rf /tmp/cache" } }
```

**PreToolUse에서 deny하는 법**: exit code **2**로 종료하거나(1은 비차단 오류), JSON으로 `hookSpecificOutput.permissionDecision: "deny"`를 돌려줍니다. stderr 메시지는 Claude에게 피드백으로 전달됩니다.

시험 문제 "`rm -rf`를 모델 판단에 의존하지 않고 확실히 막으려면?" → matcher `Bash`의 PreToolUse 훅이 stdin JSON의 `tool_input.command`를 검사하고 deny. system 프롬프트에 부탁하기(확률적), PostToolUse에 로그 남기기(실행 후라 못 막음), max_iterations 줄이기(무관)는 오답. 레슨 5의 전제 조건 게이트와 같은 발상입니다.

훅은 셸 명령을 그대로 돌리므로 신뢰할 수 없는 설정을 가져다 쓰지 않습니다.

## CI/CD: `claude -p --output-format json`

```bash
if claude -p "테스트를 실행하고 결과를 판단해 줘" --output-format json > result.json; then
  echo ok
else
  exit 1
fi
```

- `--output-format json`은 파이프라인에서 결과를 구조적으로 처리하려고 씁니다(UI, 속도, 과금과 무관).
- 종료 코드 0이 정상. GitHub Actions에서 API 키는 Secrets에 등록하고 `env:`로 참조합니다. YAML에 하드코딩, README에 메모는 오답.
- 실무에서는 Anthropic 공식 `claude-code-action`이 설치·인증·PR 코멘트를 한 번에 처리합니다. 시험은 `claude -p`를 직접 호출하는 구조를 이해하면 됩니다.

## 시크릿, 모니터링, 감사

- 프로덕션 API 키는 Secrets Manager / Vault. `.env` 커밋, 코드 하드코딩, Slack 공유는 오답. 커밋 전 gitleaks 같은 시크릿 스캔을 pre-commit이나 PreToolUse(`Bash(git commit *)`) 훅으로 겁니다.
- 텔레메트리는 OpenTelemetry 호환. 켜는 방법은 `settings.json`의 `"telemetry"` 키가 아니라 환경 변수(`CLAUDE_CODE_ENABLE_TELEMETRY=1`, `OTEL_METRICS_EXPORTER=otlp`, `OTEL_EXPORTER_OTLP_ENDPOINT=...`)이고, settings.json에 넣을 때는 `env` 블록입니다. 특정 제품 연동은 범위 밖, "사용량·비용·이상을 감시하는 구조를 설계할 수 있는가"가 시험 수준입니다.
- 감사 로그는 PII 마스킹 + 규제에 맞는 보관 기간(국내는 개인정보보호법상 접속기록 최소 1년, 고유식별정보·민감정보 처리 시 2년).
- 서브에이전트 권한 분리: `tools: Read, Grep, Glob, Bash(git diff *)`만 가진 읽기 전용 리뷰어와 쓰기 가능한 구현 에이전트를 나눕니다.

## 공식 가이드 v1.1로 보완하기

교재에 없거나 짧게만 나온 내용을 공식 시험 가이드 v1.1(2026년 11월)의 과제 설명(Task Statement 3.1~3.6)에 맞춰 채운 부분입니다.

### 1. "새 팀원에게만 규칙이 안 먹는다" (3.1)

<div class="oa-eli">
  <span class="oa-eli__head">📌 쉽게 말하면</span>
  <p>사무실 게시판에 붙인 공지는 모두가 봅니다. 내 책상 포스트잇에 적은 메모는 나만 봅니다. 팀 규칙을 포스트잇에 적어 두면 새로 온 사람은 그 규칙을 모릅니다.</p>
</div>

| 위치 | 범위 | git으로 공유 |
|---|---|---|
| `~/.claude/CLAUDE.md` | 내 모든 프로젝트 | ❌ 내 컴퓨터에만 있음 |
| `./CLAUDE.md` 또는 `.claude/CLAUDE.md` | 이 프로젝트 전체 | ✅ |
| 하위 디렉터리의 `CLAUDE.md` | 그 디렉터리 | ✅ |

시험 문제 "기존 팀원에게는 코딩 규칙이 적용되는데 새 팀원에게는 안 된다" → 규칙이 **사용자 수준**에 있어서입니다. 프로젝트 수준으로 옮기는 것이 답입니다.

**`@import`로 쪼개기**: CLAUDE.md 하나에 다 적는 대신 패키지별 표준 파일을 끌어옵니다.

```markdown
# CLAUDE.md
공통 규칙은 여기에 적습니다.

@docs/standards/api-conventions.md
@packages/web/STYLE.md
```

**`/memory`로 확인하기**: 동작이 일관되지 않으면 `/memory`를 실행해 지금 세션에 어떤 메모리 파일이 로드됐는지부터 확인합니다. 기대한 파일이 목록에 없으면 위치나 경로 규칙이 원인입니다.

### 2. 명령과 스킬은 어디에 두나 (3.2)

| | 팀과 공유 | 나만 사용 |
|---|---|---|
| 슬래시 명령 | `.claude/commands/` | `~/.claude/commands/` |
| 스킬 | `.claude/skills/<이름>/SKILL.md` | `~/.claude/skills/<이름>/SKILL.md` |

팀 스킬을 내 방식으로 바꿔 쓰고 싶으면 `~/.claude/skills/`에 **다른 이름**으로 만듭니다. 같은 이름으로 프로젝트 파일을 고치면 팀원 모두에게 영향이 갑니다.

스킬 프론트매터에서 시험에 나오는 세 가지입니다.

```yaml
---
name: review-pr
description: PR 번호를 받아 변경 사항을 리뷰한다
context: fork               # 격리된 서브에이전트에서 실행
allowed-tools: Read, Grep, Glob   # 리뷰 중 파일을 고치지 못하게 제한
argument-hint: "[PR 번호]"   # 인자를 빠뜨리지 않게 안내
---
```

| 키 | 효과 | 쓰는 이유 |
|---|---|---|
| `context: fork` | 스킬을 별도 서브에이전트 컨텍스트에서 실행 | 로그 분석처럼 출력이 긴 작업이 메인 대화를 채우지 않게 함 |
| `allowed-tools` | 스킬 실행 중 쓸 수 있는 도구를 제한 | 읽기 전용 리뷰가 실수로 파일을 고치지 않게 함 |
| `argument-hint` | 필요한 인자를 사용자에게 보여 줌 | 인자 없이 호출해서 생기는 헛돌이를 막음 |

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">스킬에 넣을 것</span>
    <ul>
      <li>필요할 때만 쓰는 작업별 절차</li>
      <li>예: 릴리스 노트 작성, DB 마이그레이션 점검</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">CLAUDE.md에 넣을 것</span>
    <ul>
      <li>항상 적용되는 공통 표준</li>
      <li>예: 테스트 명령, 커밋 규칙, 디렉터리 구조</li>
    </ul>
  </div>
</div>

### 3. 흩어진 파일에는 경로 규칙 (3.3)

테스트 파일이 여러 디렉터리에 흩어져 있다고 해 봅시다.

```text
src/
  components/Button.test.tsx
  hooks/useCart.test.tsx
  pages/checkout/Checkout.test.tsx
```

디렉터리마다 `CLAUDE.md`를 두면 세 곳에 같은 내용을 복사해야 합니다. 디렉터리 `CLAUDE.md`는 한 위치에만 묶이기 때문입니다. 대신 `.claude/rules/`에 glob 패턴으로 한 번만 적습니다.

```markdown
---
paths:
  - "**/*.test.tsx"
---
테스트는 React Testing Library를 쓰고, 구현 세부가 아니라 사용자 동작을 검증한다.
```

이 규칙은 일치하는 파일을 다룰 때만 로드되므로, 관련 없는 작업에서는 컨텍스트를 차지하지 않습니다. 인프라 규칙이라면 `paths: ["terraform/**/*"]`처럼 씁니다.

### 4. 플랜 모드인가, 바로 실행인가 (3.4)

| 작업 | 선택 | 이유 |
|---|---|---|
| 모놀리스를 마이크로서비스로 분리 | 플랜 모드 | 접근법이 여러 개이고 되돌리기 비쌈 |
| 45개 넘는 파일에 걸친 라이브러리 교체 | 플랜 모드 | 여러 파일에 걸친 변경 |
| 함수 하나의 null 체크 버그 수정 | 바로 실행 | 범위가 분명한 단일 파일 변경 |
| 입력 검증 한 줄 추가 | 바로 실행 | 계획할 것이 없음 |

플랜 모드는 파일을 바꾸기 전에 안전하게 탐색할 수 있어서 잘못된 방향으로 구현했다가 다시 하는 비용을 막습니다. 반대로 단순 수정에 플랜 모드를 쓰는 것도 오답입니다.

**Explore 서브에이전트**는 "이 기능과 관련된 파일을 다 찾아 줘" 같은 장황한 탐색을 따로 수행하고 요약만 돌려줍니다. 메인 컨텍스트가 탐색 로그로 차지 않습니다.

두 모드는 섞어 씁니다. 조사와 설계는 플랜 모드로 하고, 방향이 정해지면 바로 실행으로 구현합니다.

### 5. 결과가 흔들릴 때 쓰는 세 가지 방법 (3.5)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧪</span><span class="oa-card__title">입출력 예시 2~3개</span>말로 설명해도 형식이 매번 다르면 "입력 A → 출력 B" 예시를 줍니다.</div>
  <div class="oa-card"><span class="oa-card__icon">✅</span><span class="oa-card__title">테스트 먼저</span>동작, 경계 사례, 성능 기준을 테스트로 쓰고 실패를 하나씩 고치게 합니다.</div>
  <div class="oa-card"><span class="oa-card__icon">🎤</span><span class="oa-card__title">인터뷰 패턴</span>구현 전에 Claude가 질문하게 합니다. 캐시 무효화, 실패 시 동작처럼 놓친 고려 사항이 드러납니다.</div>
</div>

수정 요청을 묶는 기준도 나옵니다.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">한 메시지에 같이 보냄</span>
    <ul>
      <li>서로 영향을 주는 문제</li>
      <li>예: API 응답 형식 변경 + 그 응답을 쓰는 화면 수정</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">하나씩 순서대로 보냄</span>
    <ul>
      <li>서로 독립적인 문제</li>
      <li>예: 로그인 버그 → 그다음 푸터 오타</li>
    </ul>
  </div>
</div>

### 6. CI에서 쓰는 플래그와 리뷰 구조 (3.6)

```bash
claude -p "변경된 파일을 리뷰하고 버그만 보고해" \
  --output-format json \
  --json-schema '{"type":"object","properties":{"findings":{"type":"array"}},"required":["findings"]}'
```

| 플래그 | 하는 일 | 없으면 |
|---|---|---|
| `-p` (`--print`) | 비대화형으로 한 번 실행하고 끝냄 | 입력을 기다리며 CI 작업이 멈춤 |
| `--output-format json` | 결과를 JSON으로 출력 | 사람이 읽는 텍스트라 파싱이 불안정 |
| `--json-schema` | 출력이 따를 스키마를 지정 | 필드 이름과 구조가 실행마다 달라질 수 있음 |

CI에서 필요한 맥락(테스트 표준, 픽스처 규칙, 리뷰 기준)은 프로젝트 `CLAUDE.md`에 적어 둡니다.

**코드를 만든 세션에게 리뷰를 맡기지 않습니다.** 자기가 내린 판단을 그대로 갖고 있어서 같은 가정을 다시 통과시킵니다.

<ol class="oa-flow">
  <li><strong>세션 A</strong>코드 생성</li>
  <li><strong>세션 B (새 인스턴스)</strong>A의 추론 없이 코드만 보고 리뷰</li>
  <li><strong>결과</strong>PR 코멘트로 게시</li>
</ol>

## 스터디 가이드로 더 채우기

여기부터는 커뮤니티 스터디 가이드(paullarionov/claude-certified-architect)의 설명과 연습 문제 가운데, 위에서 다루지 않았거나 한 줄로만 지나간 내용을 골라 채운 부분입니다.

### 7. 항상 / 파일 따라 / 작업 따라 (3.1~3.3)

<div class="oa-eli">
  <span class="oa-eli__head">🏭 쉽게 말하면</span>
  <p>공장 입구에 붙은 안전 수칙은 누구나 매일 봅니다. 프레스 기계 옆에 붙은 주의 사항은 그 기계 앞에 선 사람만 봅니다. 분기 재고 조사 절차서는 서랍에 있다가 재고 조사를 하는 날에만 꺼냅니다. 지침을 어디에 둘지는 <b>언제 필요한가</b>로 정합니다.</p>
</div>

| 지침의 성격 | 둘 곳 | 로드되는 때 | 예 |
|---|---|---|---|
| 항상 적용 | `CLAUDE.md` | 모든 세션 | 코딩 표준, 테스트 규칙 |
| **파일 유형·위치**에 묶임 | `.claude/rules/` + `paths` | 일치하는 파일을 다룰 때 | `**/*.test.tsx` 테스트 규약, `src/api/**/*` API 규약 |
| **작업 종류**에 묶임 | 스킬 | 그 작업을 할 때(트리거되거나 `/이름`으로 호출) | PR 리뷰 체크리스트, 배포 절차, 마이그레이션 절차 |

`paths` 규칙과 스킬을 가르는 문제가 함정입니다. "API 디렉터리에서 **새 엔드포인트를 만들 때만** 구현 예시 2~3개가 필요하고, 같은 디렉터리에서 디버깅이나 리뷰를 할 때는 필요 없다"면 기준이 파일이 아니라 작업입니다.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 작업용 지침을 경로 규칙에</span>
    <ul>
      <li><code>src/api/**/*</code> 규칙에 엔드포인트 예시를 넣는다</li>
      <li>그 디렉터리에서 디버깅만 해도 예시가 매번 로드됩니다.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 작업용 지침은 스킬로</span>
    <ul>
      <li>예시를 참조하는 스킬을 만들어 필요할 때 호출합니다.</li>
      <li>다른 작업에서는 컨텍스트를 차지하지 않습니다.</li>
    </ul>
  </div>
</div>

위 "500줄 CLAUDE.md를 개선하라" 답안은 프론트·백엔드·테스트처럼 **코드 영역별** 규약을 `paths` 규칙으로 나누는 구조였습니다. 400줄짜리 CLAUDE.md에 PR 리뷰·배포·마이그레이션 같은 **워크플로 절차**가 섞인 경우라면 스터디 가이드 연습 문제의 정답은 "범용 표준은 CLAUDE.md에 두고, 워크플로별 지침은 스킬로"입니다. 문제에서 지침이 파일에 묶이는지 작업에 묶이는지부터 읽습니다.

`paths`가 없는 규칙 파일도 쓸모가 있습니다. 큰 CLAUDE.md를 `testing.md`, `api-conventions.md`, `deployment.md`처럼 주제별 파일로 쪼개 `.claude/rules/`에 두면 찾고 고치기가 쉬워집니다. 이 용도의 오답 선택지로는 파일 패턴을 CLAUDE.md의 절에 매핑하는 `.claude/config.yaml`, 하위 디렉터리의 `README.md`에 지침 나눠 적기가 나옵니다. 둘 다 Claude Code가 지원하는 구성 방식으로 제시되지 않습니다.

`paths` 규칙이 로드되는 시점을 스터디 가이드는 "일치하는 파일을 **편집할 때**"라고 적습니다. 위 본문은 "매칭 파일을 Read할 때 주입"이라고 적었습니다. 표현이 다르니 시험에서는 "일치하는 파일을 다룰 때만 로드되어 컨텍스트와 토큰을 아낀다"는 공통 부분으로 기억합니다.

### 8. `@path` 가져오기 규칙 (3.1)

```markdown
# CLAUDE.md
코딩 표준은 @./standards/coding-style.md 를 따른다.
테스트 요구 사항은 @./standards/testing-requirements.md 에 있다.
프로젝트 개요는 @README.md, 의존성은 @package.json 을 본다.
```

| 규칙 | 내용 |
|---|---|
| 표기 | 파일 경로 **바로 앞**에 `@`를 붙입니다. 사이에 공백을 두지 않습니다 |
| 경로 종류 | 상대 경로와 절대 경로를 모두 씁니다 |
| 상대 경로의 기준 | 작업 디렉터리가 아니라 **그 import를 적은 파일**의 위치 |
| 중첩 | 가져온 파일이 또 가져올 수 있고, 깊이는 최대 5단계 |

패키지마다 CLAUDE.md를 두는 모노레포라면, 각 패키지의 CLAUDE.md가 자기에게 필요한 표준 파일만 `@`로 골라 가져옵니다. 같은 표준을 여러 곳에 복사하지 않아도 됩니다.

### 9. `context: fork`가 막는 것, 그리고 세 가지 문제와 세 가지 키 (3.2)

위 2번에서는 `context: fork`를 "출력이 긴 작업이 메인 대화를 채우지 않게 한다"로 설명했습니다. 연습 문제를 보면 격리는 양쪽으로 작동합니다.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📜</span><span class="oa-card__title">긴 출력</span>코드베이스 분석 스킬을 돌린 뒤 Claude가 원래 하던 일의 맥락을 잃는다 → 분석 출력이 메인 컨텍스트를 채운 것입니다.</div>
  <div class="oa-card"><span class="oa-card__icon">💭</span><span class="oa-card__title">버린 아이디어</span>대안 탐색 스킬을 돌린 뒤 Claude가 기각된 안을 자꾸 언급한다 → 탐색 대화가 메인에 남은 것입니다.</div>
  <div class="oa-card"><span class="oa-card__icon">🧟</span><span class="oa-card__title">앞 대화의 잔재</span>마이그레이션 스킬이 관련 없는 이전 대화의 스키마를 가져다 쓴다 → 메인 대화가 스킬 안으로 새어 든 것입니다.</div>
</div>

세 경우 모두 답은 프론트매터의 `context: fork`입니다. 스킬을 작은 스킬 여러 개로 쪼개기, "결과를 짧게 요약해"라는 지시 추가, 더 싼 모델 지정은 격리를 만들어 주지 않습니다.

세 키가 한 문제에 같이 나오면 증상과 짝을 맞춥니다.

| 증상 | 키 | 프롬프트로 때우는 오답 |
|---|---|---|
| 인자 없이 호출해서 파일 이름이 엉망이 된다 | `argument-hint` | "인자가 유효한지 확인해"라는 지시 |
| 이전 대화 내용이 스킬 실행에 섞인다 | `context: fork` | "이전 대화는 무시해"라는 지시 |
| 도구 권한이 넓어 파괴적인 명령이 실행된다 | `allowed-tools` | 금지 작업 목록을 본문에 나열 |

오른쪽 열은 전부 "부탁"입니다. 왼쪽 세 키는 설정으로 걸립니다. 이 레슨 첫 줄의 "부탁할 일인가, 강제할 일인가"가 스킬에서도 그대로 쓰입니다.

**개인용 변형의 이름**: 위 2번은 "`~/.claude/skills/`에 **다른 이름**으로 만든다"고 적었고, 스터디 가이드의 본문과 도메인 3 노트도 같은 설명입니다. 그런데 같은 가이드의 연습 문제 하나는 "**같은 이름**으로 `~/.claude/skills/commit/SKILL.md`를 만든다"를 정답으로 하고, 개인 스킬이 프로젝트 스킬보다 우선하므로 그 개발자에게만 팀 버전이 가려진다고 해설합니다(다른 이름 안은 "동작은 하지만 더 약한 답"으로 처리). 가이드 안에서도 엇갈리는 지점이라 여기서는 둘 다 적어 둡니다. 양쪽이 일치하는 부분은 이렇습니다. 개인 버전은 `~/.claude/skills/`에 두고, 팀이 공유하는 프로젝트 파일은 건드리지 않습니다. `override: true` 같은 프론트매터 키나 사용자 이름 조건문은 없습니다.

### 10. 플랜 모드, 한 걸음 더 (3.4)

플랜 모드에서 Claude는 Read, Grep, Glob으로 코드베이스를 살펴보고 **구현 계획만** 씁니다. 파일은 바꾸지 않습니다. 사용자가 그 계획을 승인하면 구현으로 넘어갑니다.

<ol class="oa-flow">
  <li><strong>플랜 모드</strong>조사와 설계</li>
  <li><strong>승인</strong>사용자가 계획 확인</li>
  <li><strong>바로 실행</strong>승인된 계획대로 구현</li>
</ol>

위 4번 표에 두 가지 경우를 더합니다.

| 작업 | 선택 | 이유 |
|---|---|---|
| 처음 보는 코드베이스를 고쳐야 한다 | 플랜 모드 | 바꾸기 전에 이해부터 해야 합니다 |
| "Slack 알림 지원 추가"처럼 요구는 한 줄인데 통합 방식이 여러 개(incoming webhook / bot token / Slack App)다 | 플랜 모드 | 방식마다 아키텍처에 미치는 영향이 다르고 요구 사항이 모호합니다 |
| 스택 트레이스가 분명한 단일 파일 수정 | 바로 실행 | 범위와 원인이 이미 드러나 있습니다 |

두 번째 줄이 함정입니다. 기존 코드에 이메일·SMS 채널 패턴이 잘 잡혀 있어도 "기존 패턴대로 일단 만들고 통합 방식은 나중에"는 오답입니다. 방식을 정하는 일이 곧 설계 결정이기 때문입니다.

**탐색 출력이 컨텍스트를 채울 때**: 120개 파일에 걸친 작업을 ① 호출 지점 찾기 ② 접근법 설계 ③ 일관된 구현으로 나눴는데, ①에서 호출 지점 수백 개가 나열되어 컨텍스트가 먼저 찹니다.

| 선택지 | 판정 | 이유 |
|---|---|---|
| ①을 Explore 서브에이전트에 맡겨 요약만 받고, ②③은 메인 대화에서 계속 | ✅ | 설계와 구현에 쓸 메인 컨텍스트를 아낍니다 |
| 전부 메인 대화에서 하고 주기적으로 `/compact` | ❌ | 장황한 출력이 일단 메인에 들어온 뒤 요약되며 세부가 빠집니다 |
| 패턴을 CLAUDE.md에 적고 여러 세션으로 나눠 처리 | ❌ | 세션 사이의 일관성을 메모리 파일에만 기댑니다 |

`/compact`는 이전 기록을 요약해 컨텍스트 공간을 되찾는 명령입니다. 긴 조사에서 도구 출력이 쌓였을 때 쓰지만, 요약 과정에서 **정확한 숫자·날짜·세부 사항이 빠질 수 있다**는 점을 같이 기억합니다(레슨 6).

`/memory`에 대해 스터디 가이드는 위 1번과 다른 면을 설명합니다. 위에서는 "지금 세션에 어떤 메모리 파일이 로드됐는지 확인하는 명령"이라고 적었고, 스터디 가이드는 "CLAUDE.md 파일을 열어 메모·선호·컨텍스트를 저장하는 명령"으로 설명합니다. 여기에 적어 둔 내용은 다음 세션에도 남아 시작할 때 자동으로 로드되므로, 같은 지시를 세션마다 반복하지 않아도 됩니다. 확인과 편집, 두 쓰임을 모두 알아 둡니다.

### 11. CI 리뷰를 다듬는 세 가지 (3.6)

**인라인 PR 댓글로 달려면 스키마에 위치 정보를 넣습니다.** 서술형 문단으로 나온 리뷰는 사람이 손으로 옮겨 붙여야 합니다.

```bash
claude -p "변경된 파일을 리뷰해 줘" \
  --output-format json \
  --json-schema '{
    "type": "object",
    "properties": {
      "findings": {
        "type": "array",
        "items": {
          "type": "object",
          "properties": {
            "file": {"type": "string"},
            "line": {"type": "integer"},
            "severity": {"type": "string"},
            "suggestion": {"type": "string"}
          },
          "required": ["file", "line", "severity", "suggestion"]
        }
      }
    },
    "required": ["findings"]
  }'
```

파이프라인은 이 JSON을 파싱해 발견마다 해당 파일·줄에 댓글을 답니다. CLAUDE.md에 "리뷰 출력 형식" 절을 적어 두기, 프롬프트에 `[FILE:...] [LINE:...]` 템플릿을 지시하기, 서술형 리뷰 뒤에 JSON 요약 단계를 하나 더 두기는 모두 형식을 강제하지 못하는 오답입니다.

**커밋이 추가된 뒤의 재리뷰**와 **테스트 제안**에서는 "Claude가 이미 있는 것을 알고 있는가"가 핵심입니다.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 모르는 채로 다시 돌림</span>
    <ul>
      <li>재리뷰: 이미 고친 코드에 지난번 댓글이 또 달립니다.</li>
      <li>테스트 제안: 열 개 중 여섯 개가 이미 있는 시나리오입니다.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 이미 있는 것을 컨텍스트에 넣음</span>
    <ul>
      <li>재리뷰: <b>이전 리뷰 결과</b>를 넣고 "새 이슈와 아직 안 고쳐진 이슈만 보고해"라고 지시합니다.</li>
      <li>테스트 제안: <b>기존 테스트 파일</b>을 넣어 이미 다룬 시나리오를 알게 합니다.</li>
    </ul>
  </div>
</div>

| 솔깃한 선택지 | 왜 정답이 아닌가 |
|---|---|
| 중간 커밋은 건너뛰고 PR 생성 때와 병합 직전에만 리뷰 | 리뷰의 철저함을 포기합니다 |
| 경로와 설명이 지난번과 겹치는 발견을 후처리로 지운다 | 아직 안 고쳐진 이슈까지 지울 수 있습니다 |
| 최근 push에서 바뀐 파일만 리뷰한다 | 이전 커밋의 파일을 놓칩니다 |
| 테스트 제안 개수를 절반으로 줄인다 | 중복이 남고 새 제안만 줄 수 있습니다 |

**자기 점검의 한계**: 생성 세션은 엣지 케이스를 검토하고도 "내 접근이 맞다"고 결론 내립니다. 고치는 방법은 위 6번의 별도 인스턴스이고, 생성 단계에 extended thinking 켜기, "스스로 비판해 봐"라는 자기 리뷰 지시 추가, 테스트 파일과 문서를 더 넣기는 같은 세션의 같은 판단을 그대로 둡니다.

## 합격자 위키로 마지막 점검

843/1000점으로 합격한 수험자가 공개한 학습 위키([hong-chu/claude-certified-architect-foundations-llm-wiki](https://github.com/hong-chu/claude-certified-architect-foundations-llm-wiki), CC BY 4.0)에서, 위에 없거나 한 줄로만 지나간 판단 기준을 골라 다시 정리한 부분입니다. 위 내용과 설명이 다른 곳은 그 자리에 차이를 적었습니다.

<div class="oa-eli">
  <span class="oa-eli__head">🎒 쉽게 말하면</span>
  <p>두꺼운 교과서 한 권을 얇은 책 여섯 권으로 나눠도, 매일 여섯 권을 다 가방에 넣으면 가방 무게는 그대로입니다. 찾기만 쉬워졌을 뿐입니다. 가방이 가벼워지려면 <b>오늘 수업에 필요한 책만</b> 넣어야 합니다. <code>@import</code>는 책을 나누는 일이고, <code>paths</code> 규칙은 필요한 책만 넣는 일입니다.</p>
</div>

### 12. `@import`는 정리, `paths` 규칙은 절감 (3.1, 3.3)

둘 다 "커진 CLAUDE.md를 쪼갠다"는 점은 같지만, 로드되는 양이 줄어드는 것은 한쪽뿐입니다.

| | `@import` | `.claude/rules/` + `paths` |
|---|---|---|
| 하는 일 | 내용을 여러 파일로 나누고 import로 다시 끌어옴 | 일치하는 파일을 다룰 때만 규칙을 로드 |
| 로딩 | **즉시(eager)**. 가져온 내용은 항상 로드됨 | **지연(lazy)**. 관련 있을 때만 로드됨 |
| 컨텍스트 토큰이 줄어드나 | 아니요 | 예 |
| 쓰는 때 | 정리와 재사용이 목적일 때 | 경로별로 로드되는 컨텍스트를 줄여야 할 때 |

"토큰을 아끼려고 `@import`로 나눴다"는 선택지는 오답입니다. 유지보수가 목적이라면 `@import`로 충분합니다.

**위 본문과의 차이**: 위 "500줄 CLAUDE.md를 개선하라" 답안 구조의 ③은 `paths`를 붙인 rules 파일을 CLAUDE.md에서 `@.claude/rules/frontend.md` 식으로 참조하고, 그 결과가 "상시 로드 50줄 + 필요할 때 관련 규칙"이라고 적었습니다. 위키 설명대로라면 `@`로 가져온 내용은 항상 로드되므로, 컨텍스트를 줄여 주는 것은 `@` 참조가 아니라 `paths` 프론트매터입니다. 시험에서는 "나누기(정리)"와 "덜 싣기(절감)"를 따로 묻는다고 기억합니다.

지침을 두는 층도 셋으로 갈립니다. 위 7번 표가 "항상 / 파일 따라 / 작업 따라"였다면, 이 표는 파일 쪽을 한 번 더 나눈 것입니다.

| 지침의 범위 | 둘 곳 |
|---|---|
| 항상 적용되는 표준 | `CLAUDE.md` |
| 저장소 곳곳에 흩어진 특정 경로·파일 유형 | `.claude/rules/` + `paths` |
| **폴더 하나** 전체에 항상 적용 | 그 디렉터리의 `CLAUDE.md` |

하위 디렉터리 `CLAUDE.md`는 규약이 폴더 하나에 깔끔하게 대응할 때만 맞습니다. 그리고 규칙의 `paths`에 쓰는 glob **패턴**은 내장 도구 **Glob**과 이름만 같을 뿐 다른 것입니다.

### 13. 한 단어로 외우는 배치표 (3.1~3.2)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📜</span><span class="oa-card__title">Rule</span>정책</div>
  <div class="oa-card"><span class="oa-card__icon">🔘</span><span class="oa-card__title">Command</span>버튼</div>
  <div class="oa-card"><span class="oa-card__icon">🧭</span><span class="oa-card__title">Skill</span>워크플로</div>
  <div class="oa-card"><span class="oa-card__icon">💡</span><span class="oa-card__title">CLAUDE.md</span>항상 켜져 있음</div>
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">MCP</span>외부 도구·데이터</div>
</div>

"팀이면 프로젝트, 개인이면 사용자"라는 규칙은 지침·명령·스킬·MCP 서버에 똑같이 적용됩니다. 스코프가 어긋난 오답은 양쪽 방향으로 나옵니다.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 팀 것을 개인 자리에</span>
    <ul>
      <li>팀 표준을 <code>~/.claude/CLAUDE.md</code>에 적는다</li>
      <li>새 팀원에게는 규칙이 적용되지 않습니다.</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 개인 것을 팀 자리에</span>
    <ul>
      <li>혼자 실험하는 스킬을 프로젝트 <code>.claude/skills/</code>에 커밋한다</li>
      <li>API 토큰을 환경 변수 대신 <code>.mcp.json</code>에 적어 커밋한다</li>
    </ul>
  </div>
</div>

### 14. 작업 방식의 증상 → 원인 → 고칠 곳 (3.4~3.6)

| 증상 | 원인 | 고칠 곳 |
|---|---|---|
| 구현이 특정 입력에서 틀리는데 "더 주의해"만 반복함 | 무엇이 틀렸는지 전달되지 않음 | **실패하는 테스트 케이스**를 입력·기대값·실제값과 함께 공유 |
| 버그 하나를 고치면 다른 쪽이 깨짐 | 서로 얽힌 문제를 하나씩 따로 고침 | 한 메시지에 상세히 묶어 상호작용까지 같이 추론하게 함 |
| 계획이 확정됐는데 계속 플랜 모드에 머묾 | 모드 전환을 안 함 | 계획이 분명해지면 바로 실행으로 전환 |
| 복잡성 때문에 문제가 터진 뒤에야 플랜 모드로 바꿈 | 전환이 너무 늦음 | 구조적 변경은 **처음부터** 플랜 모드 |
| 모든 스킬에 `context: fork`를 붙임 | 격리가 필요 없는 스킬까지 격리 | 장황하거나 오래 걸리는 작업을 하고 깔끔한 요약만 돌려줄 스킬에만 |
| 비용을 줄이려고 머지 전 리뷰를 배치로 돌림 | CI가 결과를 기다리고 있음 | 머지 전 작업은 동기 호출 (레슨 3) |

플랜 모드 판단은 네 가지를 봅니다. 복잡도(구조적인가), 범위(파일이 여러 개인가), 의존성(먼저 탐색해야 하는가), 섣부른 수정의 위험(큰가). 넷 다 낮으면 바로 실행입니다. Explore 서브에이전트는 스킬의 `context: fork`와 같은 발상을 탐색에 적용한 것으로 보면 됩니다. 지저분한 발견 과정을 메인 컨텍스트 밖에서 합니다.

이 도메인에서 "더 주의해서 해", temperature 올리기, 모호한 산문 지시는 어느 증상에도 정답이 되지 않습니다.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 파이프라인에서 Claude Code 단계가 끝나지 않고 타임아웃이 납니다. 가장 먼저 확인할 것은?</summary>
  <p><b>정답: <code>-p</code> 플래그.</b> 대화형 모드로 떠서 입력을 기다리고 있을 가능성이 큽니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 코드베이스 곳곳에 있는 테스트 파일에 같은 규칙을 적용하려 합니다. 가장 알맞은 방법은?</summary>
  <p><b>정답: <code>.claude/rules/</code>에 <code>paths</code> glob을 가진 규칙 파일.</b> 디렉터리별 CLAUDE.md는 위치마다 복사해야 하고, 루트 CLAUDE.md에 넣으면 관련 없는 작업에도 항상 로드됩니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. CLAUDE.md가 400줄을 넘었습니다. 코딩 표준, 테스트 규칙, 상세한 PR 리뷰 체크리스트, 배포 절차, DB 마이그레이션 절차가 한 파일에 있습니다. 코딩 표준과 테스트 규칙은 항상 지켜지고, 나머지 셋은 그 작업을 할 때만 적용되면 좋겠습니다. 가장 효과적인 재구성은?</summary>
  <p>① 지침을 전부 스킬로 옮기고 CLAUDE.md에는 프로젝트 소개만 남긴다 ② 전부 CLAUDE.md에 두되 <code>@</code> import로 범주별 파일로 나눈다 ③ 전부 <code>.claude/rules/</code>로 옮기고 <code>paths</code> glob으로 파일 유형별로 로드한다 ④ 범용 표준은 CLAUDE.md에 두고, PR 리뷰·배포·마이그레이션 지침은 각각 스킬로 만든다</p>
  <p><b>정답: ④</b> — CLAUDE.md는 모든 세션에 로드되고 스킬은 필요할 때만 불립니다. ①은 항상 필요한 표준까지 빼 버리고, ②는 파일만 나뉠 뿐 여전히 전부 로드되며, ③은 파일 유형이 아니라 작업 종류에 묶인 지침에는 맞지 않습니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 팀이 만든 <code>/migration</code> 스킬에 문제가 셋 있습니다. (1) 인자 없이 실행해서 파일 이름이 엉망이 된다 (2) 관련 없는 이전 대화의 스키마 내용을 가져다 쓴다 (3) 도구 권한이 넓어서 누군가 실수로 테스트 데이터를 지우는 명령을 실행했다. 셋을 모두 해결하는 구성은?</summary>
  <p>① <code>$ARGUMENTS</code> 대신 <code>$1</code>, <code>$2</code>를 쓰고, 스키마 파일을 <code>@</code>로 참조하고, description에 파괴적 작업 경고를 적는다 ② 프론트매터에 <code>argument-hint</code>, <code>context: fork</code>, 파일 쓰기 작업으로 좁힌 <code>allowed-tools</code>를 넣는다 ③ 스킬을 생성용과 적용용 둘로 나누고 이름이 빠지면 물어보라는 지시를 넣는다 ④ SKILL.md 본문에 인자 검증 지시, 이전 대화 무시 지시, 금지 작업 목록을 적는다</p>
  <p><b>정답: ②</b> — 문제 하나에 설정 키 하나가 대응합니다. ①③④는 인자 누락, 컨텍스트 누수, 넓은 권한 가운데 일부를 지시문에 맡겨 강제하지 못합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q5. "결제 수단에 간편결제 추가"라는 한 줄짜리 작업을 받았습니다. 기존 코드에는 카드·계좌이체 패턴이 잘 잡혀 있습니다. 그런데 간편결제 쪽은 리다이렉트 방식, SDK 내장 방식, 정기 결제용 빌링 키 방식이 있고 방식마다 구조에 미치는 영향이 다릅니다. 어떻게 시작해야 합니까?</summary>
  <p>① 가장 단순한 리다이렉트 방식으로 바로 구현을 시작한다 ② 플랜 모드로 통합 방식과 구조상 영향을 조사하고, 구현 전에 권장안을 제시한다 ③ 기존 패턴대로 클래스 뼈대부터 만들고 방식 결정은 뒤로 미룬다 ④ 나중을 대비해 기능이 가장 많은 빌링 키 방식으로 바로 구현한다</p>
  <p><b>정답: ②</b> — 접근법이 여러 개이고 요구 사항이 모호하면 플랜 모드입니다. ①④는 확인하지 않은 가정으로 방식을 정하고, ③은 방식에 따라 달라질 구조를 먼저 굳힙니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q6. 첫 자동 리뷰가 발견 12건을 달았고 개발자가 수정 커밋을 올렸습니다. 리뷰를 다시 돌렸더니 이미 고친 코드에 지난번과 같은 댓글이 여러 건 또 달렸습니다. 철저함은 유지하면서 중복을 없애려면?</summary>
  <p>① 중간 커밋은 건너뛰고 PR 생성 때와 병합 직전에만 리뷰한다 ② 파일 경로와 설명이 지난번과 겹치는 발견을 게시 전에 걸러 내는 후처리 필터를 둔다 ③ 리뷰 범위를 가장 최근 push에서 바뀐 파일로 좁힌다 ④ 이전 리뷰 결과를 컨텍스트에 넣고 새 이슈와 아직 해결되지 않은 이슈만 보고하라고 지시한다</p>
  <p><b>정답: ④</b> — 지난 결과를 알아야 고쳐진 것과 남은 것을 구분합니다. ①은 철저함을 버리고, ②는 안 고쳐진 이슈까지 지울 수 있으며, ③은 이전 커밋의 파일을 놓칩니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q7. 파일 120개에 걸쳐 외부 API 호출마다 오류 처리 래퍼를 씌우려 합니다. 순서는 (1) 호출 지점과 패턴 전부 찾기 (2) 처리 방식 함께 설계 (3) 일관되게 구현입니다. (1)에서 호출 지점 수백 개가 나열되면서 찾기가 끝나기도 전에 컨텍스트가 빠르게 찹니다. 가장 효과적인 방법은?</summary>
  <p>① (1)은 Explore 서브에이전트에 맡겨 요약만 받고, (2)와 (3)은 메인 대화에서 이어 간다 ② 전부 메인 대화에서 하면서 주기적으로 <code>/compact</code>를 쓴다 ③ 비대화형 모드로 바꿔 호출마다 요약을 직접 넘겨 가며 묶음 처리한다 ④ 패턴을 CLAUDE.md에 적어 두고 여러 세션에 나눠 파일을 처리한다</p>
  <p><b>정답: ①</b> — 장황한 탐색 출력을 별도 컨텍스트에 가두고, 맥락이 중요한 설계와 구현에 메인 컨텍스트를 씁니다. ②는 출력이 이미 메인에 들어온 뒤의 수습이고, ③④는 세션이 끊겨 구현 일관성을 지키기 어렵습니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q8. 600줄짜리 CLAUDE.md 때문에 컨텍스트 사용량이 크다는 지적이 나왔습니다. 팀이 내용을 주제별 파일 여섯 개로 나누고 CLAUDE.md에서 <code>@</code>로 모두 가져오게 바꿨지만 세션의 토큰 사용량은 그대로입니다. 프론트엔드 규약은 <code>src/components/</code>를 다룰 때만, 인프라 규약은 <code>terraform/</code>을 다룰 때만 필요합니다. 어떻게 고칩니까?</summary>
  <p>① import를 2단계로 중첩해 파일을 더 잘게 나눈다 ② 해당 절을 <code>.claude/rules/</code>로 옮기고 <code>paths</code> 프론트매터를 붙인다 ③ 세션마다 <code>/compact</code>를 실행한다 ④ 규약을 <code>~/.claude/CLAUDE.md</code>로 옮긴다</p>
  <p><b>정답: ②</b> — <code>@import</code>는 파일을 정리할 뿐 가져온 내용을 항상 로드합니다. 일치하는 파일을 다룰 때만 로드되는 것은 <code>paths</code> 규칙입니다. ①은 같은 양을 더 잘게 나눌 뿐이고, ③은 이미 로드된 뒤의 수습이며, ④는 팀 규약을 개인 스코프로 옮겨 공유까지 깨뜨립니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q9. Claude Code가 만든 할인 계산 함수가 쿠폰 두 개가 겹칠 때만 금액을 잘못 냅니다. 개발자가 "엣지 케이스를 더 꼼꼼히 봐 줘"라고 두 번 요청했지만 같은 버그가 남습니다. 가장 효과적인 다음 요청은?</summary>
  <p>① "이번에는 정말 신중하게 다시 작성해 줘"라고 요청한다 ② temperature를 올려 다른 구현이 나오게 한다 ③ 실패하는 테스트 케이스를 입력, 기대값, 실제값과 함께 준다 ④ 함수를 처음부터 다시 생성하게 한다</p>
  <p><b>정답: ③</b> — 깨진 구현은 무엇이 틀렸는지 구체적으로 보여 줘야 고쳐집니다. ①은 이미 두 번 안 통한 모호한 지시이고, ②와 ④는 무엇이 틀렸는지 알려 주지 않은 채 다시 뽑기만 합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q10. 리팩터링 뒤 문제가 둘 생겼습니다. 캐시 키 형식을 고치면 세션 조회가 깨지고, 세션 조회를 고치면 캐시 키가 다시 틀어집니다. 개발자가 메시지를 따로 보내 하나씩 고치게 했더니 두 수정이 서로를 계속 되돌립니다. 어떻게 요청해야 합니까?</summary>
  <p>① 지금처럼 하나씩 보내되 매번 "다른 쪽은 건드리지 마"를 덧붙인다 ② 두 문제와 둘의 관계를 한 메시지에 상세히 적어 함께 고치게 한다 ③ 문제마다 새 세션을 열어 따로 고친다 ④ 리팩터링을 되돌리고 플랜 모드 없이 다시 실행한다</p>
  <p><b>정답: ②</b> — 서로 영향을 주는 문제는 한 번에 보여 줘야 상호작용까지 같이 추론합니다. ①과 ③은 얽힌 문제를 계속 떼어 놓는 방식이고, ④는 원인을 다루지 않습니다. 서로 독립적인 문제였다면 반대로 하나씩 순서대로가 맞습니다.</p>
</details>

## 핵심 정리

- 비대화식은 `claude -p`, 계획 리뷰는 plan 모드.
- CLAUDE.md는 사용자 → 프로젝트 → 하위, 200줄 이내, 세부는 `.claude/rules/*.md` + `paths:`.
- settings.json: allow는 무승인, deny는 항상 거부, 나머지는 프롬프트. 우선순위 로컬 > 프로젝트 > 사용자.
- 메모리는 로컬 파일, MEMORY.md는 인덱스, user/feedback/project/reference.
- 내용은 Grep, 이름은 Glob, 고치기 전에 Read.
- 자동 지식은 스킬, 사용자 호출은 커맨드, 독립 컨텍스트는 서브에이전트, 결정론적 실행은 훅.
- 확실히 막아야 하면 PreToolUse 훅(exit 2 또는 permissionDecision deny). 훅은 harness가 실행한다.
- CI는 `claude -p --output-format json`, 키는 Secrets, 텔레메트리는 OTel 환경 변수.
- 가이드 보완: 팀 규칙은 프로젝트 수준에 둔다(사용자 수준은 공유 안 됨). 로드 확인은 `/memory`, 흩어진 파일은 `paths` 규칙, 스킬은 `context: fork`·`allowed-tools`·`argument-hint`, CI는 `-p --output-format json --json-schema`, 리뷰는 별도 인스턴스.
- 스터디 가이드 보완: 항상 적용은 CLAUDE.md, 파일에 묶이면 `paths` 규칙, 작업에 묶이면 스킬. `@path`의 상대 경로는 import를 적은 파일 기준이고 중첩은 최대 5단계. 스킬 문제는 증상별로 `argument-hint`·`context: fork`·`allowed-tools`를 짝짓는다. 요구가 모호하고 방식이 여러 개면 플랜 모드, 장황한 탐색은 Explore 서브에이전트. 재리뷰에는 이전 결과를, 테스트 제안에는 기존 테스트 파일을 컨텍스트에 넣는다.
- 합격자 위키 점검: `@import`는 정리만 하고 항상 로드된다. 컨텍스트를 줄이는 것은 `paths` 규칙이다. Rule = 정책, Command = 버튼, Skill = 워크플로, CLAUDE.md = 항상 켜짐, MCP = 외부 도구·데이터. 깨진 구현에는 실패하는 테스트 케이스(입력·기대값·실제값), 얽힌 문제는 한 메시지에, 계획이 정해지면 플랜 모드에서 나온다.
