---
title: "The CLAUDE.md file"
title_ko: "CLAUDE.md 파일"
course: claude-code-101
lesson: 8
---

## 학습 목표

- CLAUDE.md가 **해결하는 문제와 동작 방식** 이해하기
- **프로젝트 수준**과 **사용자 수준** memory 파일의 계층 파악하기
- CLAUDE.md를 **간결하고 유용하게** 유지하는 팁 세 가지 익히기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>문제</strong>CLAUDE.md가 없으면 매번 처음부터 시작해요</li>
  <li><strong>해결</strong>세션 시작 때마다 자동으로 읽히는 Markdown 파일</li>
  <li><strong>공유</strong>프로젝트 수준은 팀과, 사용자 수준은 나만</li>
  <li><strong>키우기</strong>없이 시작 → 반복 지적 관찰 → <code>/init</code></li>
</ol>

이 레슨을 한 문장으로 줄이면: **CLAUDE.md는 Claude Code에게 프로젝트에 대한 지속적인 기억을 주는, 코드베이스의 온보딩 스크립트예요.**

## 1. 어떤 문제를 해결하나요

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍💼 쉽게 말하면</span>
  <p>매일 아침 <b>기억이 초기화되는 신입사원</b>을 상상해 보세요. 매번 회사 구조부터 다시 설명해야 하죠. 책상 위에 <b>인수인계 문서</b> 한 장을 올려 두면 출근하자마자 읽고 바로 일을 시작해요.</p>
</div>

CLAUDE.md는 Claude Code에서 가장 유용한 기능 중 하나예요. Claude Code에 **프로젝트에 대한 지속적인 기억(persistent memory)**을 줘요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ CLAUDE.md 없이</span>
    <ul>
      <li>매번 <b>처음부터</b> 시작해요</li>
      <li>코드베이스를 다시 탐색해요</li>
      <li>필요한 의존성을 다시 파악해요</li>
      <li>이미 구현된 기능을 다시 이해해야 해요</li>
      <li>때로는 <b>가정</b>을 세워서, 방향을 잡기 더 어려워져요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ CLAUDE.md가 있으면</span>
    <ul>
      <li>프로젝트 루트에 두는 Markdown 파일</li>
      <li>세션 시작 때마다 Claude Code가 <b>자동으로</b> 읽어요</li>
      <li>코드베이스의 <b>온보딩 스크립트</b> 역할</li>
      <li>내용은 prompt에 덧붙여져요(appended)</li>
    </ul>
  </div>
</div>

## 2. 예시로 보는 CLAUDE.md

<div class="oa-eli">
  <span class="oa-eli__head">🍜 쉽게 말하면</span>
  <p>단골집에 붙여 둔 메모예요. <b>"늘 먹던 걸로, 고수는 빼고"</b>가 적혀 있으면 매번 설명하지 않아도 그대로 나와요.</p>
</div>

전형적인 CLAUDE.md는 이렇게 생겼어요.

```text
# Project
This is a Next.js 15 app using the App Router, Tailwind, and Drizzle ORM.

# Commands
- Dev server: `pnpm dev`
- Run tests: `pnpm test`
- Lint: `pnpm lint`

# Code Style
- Use 2-space indentation
- Prefer named exports
- All API routes go in app/api/
- Use server actions instead of API routes where possible
```

간단하죠. 세 부분으로 나뉘어요.

| 섹션 | 담는 것 | 예시 |
|---|---|---|
| **Project** | 스택 | Next.js 15, App Router, Tailwind, Drizzle ORM |
| **Commands** | 자주 쓰는 명령 | `pnpm dev`, `pnpm test`, `pnpm lint` |
| **Code Style** | 코드 관례 | 2-space, named exports, `app/api/`, server actions 우선 |

이제 React 컴포넌트를 만들라고 하면, Claude Code는 **Tailwind로 스타일링하고 코드 관례를 따라야 한다**는 걸 이미 알아요.

## 3. CLAUDE.md는 팀을 위한 것

<div class="oa-eli">
  <span class="oa-eli__head">🏢 쉽게 말하면</span>
  <p>회사 <b>공용 게시판</b>과 <b>내 책상 포스트잇</b>의 차이예요. 게시판 내용은 모두가 보고, 포스트잇은 나만 보지만 내가 어디에 앉든 따라와요.</p>
</div>

CLAUDE.md는 버전 관리에 **커밋해서** 팀이 함께 혜택을 받게 할 수 있고, 그렇게 해야 해요. 대상에 따라 memory 파일에는 계층이 있어요.

| 수준 | 위치 | 누구를 위한 것 | 무엇을 넣나 |
|---|---|---|---|
| **프로젝트 수준 CLAUDE.md** | 프로젝트 루트 디렉터리 | 팀과 공유 | 스택, 명령, 코드 관례 |
| **사용자 수준 CLAUDE.md** | 설정(configuration) 폴더 | 나만. **내 모든 프로젝트**에 적용 | 개인 선호 |

## 4. 간결하게 유지하는 팁

<div class="oa-eli">
  <span class="oa-eli__head">🌱 쉽게 말하면</span>
  <p>규칙집을 미리 두껍게 써 두지 말고, <b>같은 잔소리를 두 번 하게 될 때</b> 한 줄씩 적어 넣는 거예요. 그래야 꼭 필요한 것만 남아요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">💾</span><span class="oa-card__title">수정 사항을 memory에 저장</span>같은 지적을 반복한다면 그 규칙을 저장하라고 명시적으로 요청해요</div>
  <div class="oa-card"><span class="oa-card__icon">📎</span><span class="oa-card__title">프로젝트 문서 참조</span><code>@</code> 기호와 파일 경로로 문서를 가리켜요</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🚶</span><span class="oa-card__title">없이 시작하기</span>먼저 관찰하고, 준비되면 <code>/init</code>으로 생성해요</div>
</div>

### 수정 사항을 memory에 저장

"항상 API route 대신 server action을 써라"처럼 **같은 지적을 반복**한다면, 그 규칙을 memory에 저장하라고 Claude에게 명시적으로 요청하세요. 다음에 프로젝트를 열면 기억하고 있어요.

### 프로젝트 문서 참조

Claude가 참조하길 원하는 문서가 프로젝트에 있으면, `@` 기호와 파일 경로를 써요.

```text
## README.md
Please read if you need more info: @README.md
```

### 없이 시작하기

<ol class="oa-flow">
  <li><strong>CLAUDE.md 없이 시작</strong>일단 프로젝트를 시작해요</li>
  <li><strong>관찰</strong>어디서 반복적으로 방향을 바로잡아야 하는지 봐요</li>
  <li><strong><code>/init</code></strong>준비되면 Claude가 하나 생성하게 해요</li>
  <li><strong>키워 가기</strong>진행하면서 필요에 따라 추가해요</li>
</ol>

이렇게 하면 CLAUDE.md가 **꼭 필요한 정보에만 집중**되어 간결해져요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. CLAUDE.md가 없으면 Claude Code는 세션마다 어떻게 시작하나요?</summary>
  <p><b>정답: 매번 처음부터 시작해요.</b> 코드베이스를 다시 탐색하고, 의존성과 구현된 기능을 다시 파악하고, 때로는 가정을 세워요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 내 모든 프로젝트에 적용할 개인 선호는 어디에 넣나요?</summary>
  <p><b>정답: 사용자 수준 CLAUDE.md.</b> 설정 폴더에 있고 나만을 위한 파일이에요. 프로젝트 수준 파일은 팀과 공유해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. CLAUDE.md에서 README 같은 프로젝트 문서를 참조하게 하려면?</summary>
  <p><b>정답: <code>@</code> 기호와 파일 경로를 써요.</b> 예: <code>@README.md</code></p>
</details>

<details class="oa-quiz">
  <summary>Q4. 강의가 처음에는 CLAUDE.md 없이 시작하라고 권하는 이유는?</summary>
  <p><b>정답: 어디서 반복적으로 방향을 바로잡는지 관찰할 수 있어서요.</b> 그래야 꼭 필요한 정보만 담긴 간결한 파일이 돼요. 준비되면 <code>/init</code>으로 생성해요.</p>
</details>

## 정리

- 답답한 세션과 생산적인 세션의 차이는 대개 **맥락**에서 갈리고, CLAUDE.md가 그 맥락을 제공하는 방법이에요.
- CLAUDE.md는 프로젝트 루트에 두는 Markdown 파일이고, **세션 시작 때마다 자동으로** 읽혀 prompt에 덧붙여져요.
- **프로젝트 수준**은 버전 관리에 커밋해 팀과 공유하고, **사용자 수준**은 내 모든 프로젝트에 적용되는 개인 선호를 담아요.
- 반복되는 지적은 memory에 저장하게 하고, 문서는 `@`로 참조해요.
- **스택, 선호, 명령**으로 시작해 진행하며 필요에 따라 키워 가세요. `/init`이 초안을 만들어 줘요.
