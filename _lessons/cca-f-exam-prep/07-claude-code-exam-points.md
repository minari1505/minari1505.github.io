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

<details class="oa-quiz">
  <summary>Q1. 파이프라인에서 Claude Code 단계가 끝나지 않고 타임아웃이 납니다. 가장 먼저 확인할 것은?</summary>
  <p><b>정답: <code>-p</code> 플래그.</b> 대화형 모드로 떠서 입력을 기다리고 있을 가능성이 큽니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 코드베이스 곳곳에 있는 테스트 파일에 같은 규칙을 적용하려 합니다. 가장 알맞은 방법은?</summary>
  <p><b>정답: <code>.claude/rules/</code>에 <code>paths</code> glob을 가진 규칙 파일.</b> 디렉터리별 CLAUDE.md는 위치마다 복사해야 하고, 루트 CLAUDE.md에 넣으면 관련 없는 작업에도 항상 로드됩니다.</p>
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
