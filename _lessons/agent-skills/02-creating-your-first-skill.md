---
title: "Creating your first skill"
title_ko: "첫 Skill 만들기"
course: agent-skills
lesson: 2
---

## 학습 목표

- Skill을 저장하는 4가지 위치와 우선순위 구분하기
- `SKILL.md`의 두 구성 요소(frontmatter + 본문) 작성하기
- Claude가 자동 호출 판단에 쓰는 `description` 잘 쓰는 법 익히기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>위치 고르기</strong>위치가 곧 적용 범위</li>
  <li><strong>디렉토리 만들기</strong>디렉토리 이름 = 명령어</li>
  <li><strong>SKILL.md 작성</strong>frontmatter + 본문</li>
  <li><strong>테스트</strong>자동 호출과 직접 호출</li>
</ol>

## Skill은 어디에 저장할까?

저장 위치가 곧 **적용 범위**를 결정합니다.

<div class="oa-eli">
  <span class="oa-eli__head">🗂️ 쉽게 말하면</span>
  <p>메모를 어디에 붙이느냐와 같습니다. <b>내 수첩</b>(Personal)에 적으면 어디를 가든 나만 봅니다. <b>프로젝트 회의실 벽</b>(Project)에 붙이면 그 프로젝트 사람들만 봅니다. <b>회사 공지</b>(Enterprise)는 모두에게 적용되고 가장 우선합니다.</p>
</div>

| 위치 | 경로 | 적용 범위 |
|---|---|---|
| Enterprise | managed settings로 배포 | 조직 전체 |
| Personal | `~/.claude/skills/<이름>/SKILL.md` | 내 모든 프로젝트 |
| Project | `.claude/skills/<이름>/SKILL.md` | 해당 프로젝트만 |
| Plugin | `<plugin>/skills/<이름>/SKILL.md` | 플러그인이 활성화된 곳 |

- 이름이 겹치면 **Enterprise > Personal > Project** 순으로 우선합니다.
- Plugin skill은 `plugin-name:skill-name` 네임스페이스를 쓰므로 다른 레벨과 충돌하지 않아요.
- 같은 이름의 skill은 번들 skill(`/code-review` 등)도 덮어씁니다.

## 실습: 변경사항 요약 skill 만들기

git 저장소의 커밋 안 된 변경사항을 요약하고 위험 요소를 짚어주는 skill을 만들어 봅니다.

**1단계 — 디렉토리 만들기** (개인 skill이므로 모든 프로젝트에서 사용 가능)

```bash
mkdir -p ~/.claude/skills/summarize-changes
```

**2단계 — `SKILL.md` 작성**

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
then list any risks you notice such as missing error handling,
hardcoded values, or tests that need updating.
If the diff is empty, say there are no uncommitted changes.
```

구조는 딱 두 부분입니다.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">YAML frontmatter (<code>---</code> 사이)</span>
    <ul>
      <li>Claude가 <i>언제</i> 이 skill을 쓸지 판단하는 정보</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">마크다운 본문 (그 아래)</span>
    <ul>
      <li>skill이 실행될 때 Claude가 따르는 지시사항</li>
    </ul>
  </div>
</div>

**디렉토리 이름이 곧 명령어**가 됩니다 → `/summarize-changes`

> `` !`git diff HEAD` `` 줄은 **동적 컨텍스트 주입**입니다. Claude가 skill을 읽기 전에 Claude Code가 이 명령을 먼저 실행하고 출력으로 치환해요. 그래서 Claude는 추측이 아닌 **실제 diff**를 보고 답합니다. (자세한 건 레슨 03에서!)

**3단계 — 테스트** (두 가지 방법)

1. **자동 호출**: description에 맞는 질문 던지기 → `What did I change?`
2. **직접 호출**: `/summarize-changes` 입력

어느 쪽이든 변경사항 요약 + 위험 요소 목록이 나오면 성공! 🎉

## description이 제일 중요합니다 ⭐

Claude는 오직 `description`을 보고 skill을 자동 호출할지 결정합니다. 잘 쓰는 요령:

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">무엇 + 언제</span><b>무엇을 하는지 + 언제 쓰는지</b>를 모두 담기</div>
  <div class="oa-card"><span class="oa-card__icon">🗣️</span><span class="oa-card__title">자연스러운 키워드</span>사용자가 실제로 말할 법한 키워드 포함하기 — "what changed", "commit message", "review diff"</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">⬆️</span><span class="oa-card__title">핵심을 맨 앞에</span>skill 목록에서 description은 1,536자로 잘리기 때문</div>
  <div class="oa-card"><span class="oa-card__icon">✍️</span><span class="oa-card__title">명시적으로 쓰기</span>생략하면 본문 첫 문단이 대신 쓰이지만, 명시적으로 쓰는 걸 권장</div>
</div>

## 알아두면 좋은 것

- **실시간 변경 감지**: skill 파일을 추가·수정·삭제하면 세션 재시작 없이 바로 반영됩니다. (skills 디렉토리 자체를 새로 만든 경우만 재시작 필요)
- 프로젝트 skill은 `.claude/skills/`를 git에 커밋하면 팀원 모두가 쓸 수 있어요. (레슨 05에서 자세히)

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. <code>~/.claude/skills/summarize-changes/SKILL.md</code>로 만든 skill은 어떤 명령어로 직접 호출합니까?</summary>
  <p><b>정답: <code>/summarize-changes</code>.</b> 디렉토리 이름이 곧 명령어가 됩니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 같은 이름의 skill이 Personal과 Project에 모두 있습니다. 어느 쪽이 적용됩니까?</summary>
  <p><b>정답: Personal.</b> 이름이 겹치면 Enterprise &gt; Personal &gt; Project 순으로 우선합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Claude가 skill을 자동 호출할지 결정할 때 보는 것은 무엇입니까?</summary>
  <p><b>정답: 오직 <code>description</code>.</b> 그래서 무엇을 하는지와 언제 쓰는지를 키워드와 함께, 핵심을 맨 앞에 씁니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 본문의 <code>!`git diff HEAD`</code> 줄은 누가 언제 실행합니까?</summary>
  <p><b>정답: Claude가 skill을 읽기 전에 Claude Code가 먼저 실행하고 출력으로 치환합니다.</b> 그래서 Claude는 추측이 아닌 실제 diff를 보고 답합니다.</p>
</details>

## 정리

- 위치가 범위: Personal(`~/.claude/skills/`)은 모든 프로젝트, Project(`.claude/skills/`)는 그 저장소만.
- `SKILL.md` = frontmatter(언제 쓸지) + 본문(뭘 할지). 디렉토리명 = `/명령어`.
- description에는 "무엇을 + 언제"를 키워드와 함께, 핵심을 맨 앞에.
