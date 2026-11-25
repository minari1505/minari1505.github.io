---
title: "What are skills?"
title_ko: "Skill이란 무엇인가?"
course: agent-skills
lesson: 1
---

## 학습 목표

- Skill이 무엇인지, 어떤 문제를 해결하는지 설명하기
- Claude가 skill을 발견하고 사용하는 두 가지 방식 구분하기
- Skill의 핵심 설계 원리인 **점진적 공개(progressive disclosure)** 이해하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>① 메타데이터</strong>name + description은 세션 시작 시 항상 로드</li>
  <li><strong>관련성 판단</strong>Claude가 description만 보고 판단</li>
  <li><strong>② SKILL.md 본문</strong>필요할 때 로드</li>
  <li><strong>③ 부속 파일</strong>해당 상황에서만 로드</li>
</ol>

이 레슨을 관통하는 한 문장: **필요한 만큼만 읽기 때문에, 많이 담아도 평소에는 가볍습니다.**

## Skill = 재사용 가능한 지시서 폴더

**Skill은 Claude가 특정 작업을 더 잘하도록 만들어주는, 재사용 가능한 마크다운 지시서**입니다. 폴더 하나에 `SKILL.md`(필수) + 참고 문서·스크립트 같은 부속 파일을 담는 구조예요.

<div class="oa-eli">
  <span class="oa-eli__head">📒 쉽게 말하면</span>
  <p>새 직원에게 주는 <b>온보딩 가이드</b>에 비유됩니다. 범용 에이전트(Claude)에게 우리 팀의 절차와 도메인 지식을 얹어, 특정 작업에 특화된 에이전트로 만드는 방법이에요.</p>
</div>

Claude는 skill을 두 가지 방식으로 사용합니다.

| 방식 | 동작 |
|---|---|
| **자동 호출** | 대화 내용이 skill의 `description`과 관련 있다고 판단되면 Claude가 알아서 로드 |
| **직접 호출** | 사용자가 `/skill-name`을 입력해서 명시적으로 실행 |

## 언제 skill을 만들까?

이 두 가지 신호가 보이면 skill을 만들 타이밍입니다.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📋</span><span class="oa-card__title">신호 1</span>같은 지시사항·체크리스트·여러 단계 절차를 <b>채팅에 반복해서 붙여넣고 있을 때</b></div>
  <div class="oa-card"><span class="oa-card__icon">📈</span><span class="oa-card__title">신호 2</span>CLAUDE.md의 한 섹션이 "사실(fact)"이 아니라 <b>"절차(procedure)"로 커져버렸을 때</b></div>
</div>

CLAUDE.md와 달리 skill 본문은 **사용될 때만 컨텍스트에 로드**되기 때문에, 긴 참고 자료를 넣어도 평소에는 토큰 비용이 거의 들지 않습니다.

## 핵심 설계: 점진적 공개 (Progressive Disclosure)

Skill이 토큰을 아끼면서도 방대한 지식을 담을 수 있는 비결은, 정보를 **3단계로 나눠 필요한 만큼만 로드**하는 구조입니다.

<div class="oa-eli">
  <span class="oa-eli__head">📚 쉽게 말하면</span>
  <p>책장을 떠올리면 됩니다. 평소에는 <b>책등의 제목</b>(메타데이터)만 봅니다. 필요한 책만 꺼내 <b>본문</b>을 읽고, 더 자세한 내용이 필요할 때만 <b>부록</b>을 펼칩니다. 책장에 책이 아무리 많아도 책상 위는 복잡해지지 않습니다.</p>
</div>

| 단계 | 내용 | 로드 시점 |
|---|---|---|
| ① 메타데이터 | frontmatter의 `name` + `description` | 세션 시작 시 항상 (매우 가벼움) |
| ② SKILL.md 본문 | 실제 지시사항 | Claude가 필요하다고 판단할 때 |
| ③ 부속 파일 | reference.md, 스크립트 등 | 해당 상황에서만 |

- 1단계 메타데이터만 시스템 프롬프트에 상시 로드되므로, Claude는 **전체 내용을 읽지 않고도** 이 skill이 지금 관련 있는지 판단할 수 있어요.
- 덕분에 skill에는 사실상 **무제한 분량의 자료**를 담을 수 있습니다. 에이전트가 한꺼번에 전부 로드하지 않으니까요.

### 실제 예시: Anthropic의 PDF skill

- `SKILL.md` 본문: PDF 다루기의 일반적인 가이드
- 별도 파일(`forms.md` 등): 폼 채우기 같은 특수 작업의 상세 절차
- 번들 Python 스크립트: PDF 전체를 컨텍스트에 올리지 않고 폼 필드를 **결정론적으로 추출**

## MCP와의 관계

Skill은 MCP(Model Context Protocol)를 대체하는 게 아니라 **보완**합니다.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">MCP</span>
    <ul>
      <li>외부 시스템(Slack, DB 등)과 연결해 <b>도구를 제공</b></li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">Skill</span>
    <ul>
      <li>그 도구들을 활용하는 <b>복잡한 워크플로우를 가르침</b> (내부 절차 지식)</li>
    </ul>
  </div>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 세션 시작 시 항상 컨텍스트에 로드되는 것은 skill의 어느 부분입니까?</summary>
  <p><b>정답: frontmatter의 <code>name</code>과 <code>description</code>(메타데이터).</b> 본문은 Claude가 필요하다고 판단할 때, 부속 파일은 해당 상황에서만 로드됩니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. CLAUDE.md의 한 섹션이 긴 절차로 커졌습니다. 어떻게 하는 것이 좋습니까?</summary>
  <p><b>정답: skill로 옮깁니다.</b> skill 본문은 사용될 때만 로드되므로 긴 참고 자료를 넣어도 평소 토큰 비용이 거의 들지 않습니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Skill과 MCP는 어떤 관계입니까?</summary>
  <p><b>정답: 보완 관계.</b> MCP는 외부 시스템과 연결해 도구를 제공하고, Skill은 그 도구를 활용하는 워크플로우를 가르칩니다.</p>
</details>

## 정리

- Skill = `SKILL.md` 중심의 재사용 가능한 지시서 폴더. Claude가 자동으로, 또는 사용자가 `/이름`으로 호출.
- 반복해서 붙여넣는 절차가 있다면 skill로 만들 것.
- 점진적 공개(메타데이터 → 본문 → 부속 파일) 덕분에 토큰 걱정 없이 깊은 지식을 담을 수 있다.
