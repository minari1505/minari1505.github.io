---
title: "Standing context: Global instructions and projects"
title_ko: "상시 맥락: global instructions와 projects"
course: introduction-to-claude-cowork
lesson: 6
---

## 학습 목표

- **global instructions**로 Claude가 모든 세션을 "내가 어떻게 일하는지" 아는 상태로 시작하게 하기
- 어떤 작업이 **project**에 속하는지 판단하기
- project를 시작하는 **세 가지 방법** 고르기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>Global instructions 작성</strong>나에 대한 상시 브리프</li>
  <li><strong>교정이 반복되면 추가</strong>자꾸 하는 말이 후보예요</li>
  <li><strong>반복되는 작업 흐름 발견</strong>고객 · 반복 결과물 · 런칭</li>
  <li><strong>Project 만들기</strong>instructions · scheduled tasks · context</li>
  <li><strong>Memory가 쌓여요</strong>다음 작업이 아는 상태로 시작</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Global instructions는 "나"를, project는 "내가 지금 하는 일"을 커버해요.**

## 1. 새 협업자 온보딩하기

<div class="oa-eli">
  <span class="oa-eli__head">📒 쉽게 말하면</span>
  <p>눈치 빠른 동료는 같이 일하며 알아서 배우지만, <b>확실히 전해지길 바라는 건 인수인계 문서로 써 주죠.</b> 저절로 쌓이는 기억과, 내가 일부러 마련해 두는 맥락은 따로예요.</p>
</div>

Claude는 대화에서 스스로 무언가를 기억하고, 그 memory는 Cowork 작업에도 적용돼요. 하지만 **작업에서 작업으로 확실하게 이어지는 맥락**은 사용자가 일부러 설정한 맥락이에요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🙋</span><span class="oa-card__title">Global instructions</span>모든 Cowork 세션에 적용되는 상시 브리프</div>
  <div class="oa-card"><span class="oa-card__icon">🗂️</span><span class="oa-card__title">Projects</span>그 안에서 나눈 대화로부터 Claude가 <b>자동으로</b> memory를 쌓는 워크스페이스</div>
</div>

대부분은 global instructions부터 시작하고, 반복되는 작업이 드러나면 project를 더해요.

## 2. Global instructions — 모든 세션에 적용되는 브리프

<div class="oa-eli">
  <span class="oa-eli__head">☕ 쉽게 말하면</span>
  <p>단골 카페에서 <b>"늘 먹던 걸로요"</b>가 통하는 이유는 사장님이 내 취향을 알기 때문이에요. Global instructions는 그 취향을 한 번 적어 두는 메모예요.</p>
</div>

한 번 작성하는 상시 브리프예요. Settings에서 쓰면 Claude가 **모든 세션**(모든 chat, 모든 예약 작업)에서 참조해요.

### 설정 방법

<ol class="oa-flow">
  <li><strong>Settings 열기</strong>Claude의 Settings</li>
  <li><strong>Global instructions 찾기</strong>앱 버전에 따라 "Instructions for Claude"로 표시돼요</li>
  <li><strong>브리프 작성 · 저장</strong>몇 문장이면 충분해요</li>
</ol>

처음 정리할 때 강의는 이 경로를 `Settings → Cowork → Global instructions 옆 Edit`으로, 적용 범위를 "모든 chat, 예약 작업, Dispatch"로 안내했어요. 현재 강의는 위처럼 설명해요.

### 무엇을 넣나요?

| 넣을 것 | 왜 |
|---|---|
| **내가 누구이고 무슨 일을 하는가** | 모든 작업이 내 역할에 맞춰 시작돼요 |
| **내가 쓰는 약어 · 줄임말** | Claude가 "QBR 덱"이 뭔지 안 물어도 돼요 |
| **출력을 어떻게 받고 싶은가** | 형식 · 길이 · 톤 등 |

강의는 역할별 예시(Product Manager, Finance, Legal)를 보여 줘요. 예를 들어 Product Manager용 브리프를 넣으면 이런 기본 동작이 생겨요.

- 따로 말하지 않아도 PRD와 brief 형식을 기본으로 써요
- 해결책보다 user problem을 먼저 말해요
- 모든 metric에 출처 문서를 밝혀요

### 처음부터 완벽할 필요 없어요

반복해서 주는 교정이 곧 global-instruction 후보예요.

- "결론부터 말해줘" (bottom line up front)
- "옥스퍼드 콤마 쓰지 마"

### 시작용 템플릿

```text
나는 [회사]의 [역할]이고, [주요 작업 흐름 1~2개]를 하고 있어. 알아 둬야 할 [약어/줄임말]은 다음과 같아. 내 결과물은 대부분 [문서, 덱, 브리프, 모델 등]이야. 업데이트와 브리프는 간결하게 받고 싶어 — 권고안부터 말하고, 배경은 한 문단으로. 최종 문서 파일을 만들기 전에 .md 미리보기를 먼저 검토하고 싶어.
```

## 3. Projects — 하나의 작업 흐름을 위한 워크스페이스

<div class="oa-eli">
  <span class="oa-eli__head">🗃️ 쉽게 말하면</span>
  <p>고객사마다 <b>전용 바인더</b>를 두는 것과 같아요. 지침, 자료, 지난 회의 메모가 한곳에 있어서 바인더를 펼치면 "어디까지 했더라?"를 다시 설명할 필요가 없어요.</p>
</div>

Global instructions가 "나"를 커버한다면, project는 "내가 지금 하는 일"을 커버해요. 특정 작업 흐름(고객, 반복 결과물, 런칭)에 묶인 워크스페이스예요. 안에는 **내가 설정하는 세 가지**와 **Claude가 쌓아 주는 한 가지**가 있어요.

| 구성 요소 | 누가 | 내용 |
|---|---|---|
| **Instructions** | 내가 설정 | global과 같지만 이 프로젝트에 한정돼요. (예: "이 프로젝트는 주간 transformation 팀 회의용이야. 조직 전반의 데이터를 모아 매주 슬라이드 덱으로 압축하는 게 목적이야.") |
| **Scheduled tasks** | 내가 설정 | 프로젝트에 속한 반복 실행: 금요일 status pull, 회의 전 brief, 월말 rollup. 프로젝트 안의 대화에서 설정하고, 매번 프로젝트 맥락으로 실행돼요 |
| **Context** | 내가 설정 | Claude가 작업할 하나 이상의 폴더나 링크. 프로젝트의 모든 대화가 접근해요 |
| **Memory** | Claude가 쌓음 | 프로젝트 안 대화에서 Claude가 배우는 것. 시간이 지나며 쌓이고, 직접 쓰지 않아요 |

### Memory가 project의 차이예요

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">📭 project 밖</span>
    <ul>
      <li>매 세션이 global instructions만 빼고 <b>새로 시작</b>해요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">📬 project 안</span>
    <ul>
      <li>모든 대화가 Claude가 아는 것에 <b>더해져요</b></li>
      <li>다음 작업이 고객 상황 · 지난주 결정 · 미결 사항을 이미 손에 쥔 채 시작돼요</li>
    </ul>
  </div>
</div>

### 예시 — 고객 계정 project (강의의 "Northwind — account")

| 구성 요소 | 예시 내용 |
|---|---|
| **Instructions** | 격식 있는 톤. EB(Sarah Chen, VP Ops)를 이름으로 부르기. 항상 "draft for review"로 끝내기 |
| **Scheduled** | 월요일 오전 8시: CRM에서 주말 Northwind 활동을 끌어와 3줄 계정 업데이트 초안 · 갱신 30일 전: renewal-prep brief 초안 |
| **Context** | 내 컴퓨터의 `Clients/Northwind/`, `QBR-prep/` · 링크: Salesforce의 Northwind 계정, Notion의 고객 프로필 |
| **Memory** | `eb-prefs-sarah-chen.md`, `renewal-march-15.md`, `qbr-price-sensitivity.md` |

고객 계정 하나에 폴더 하나, EB 한 명, guardrail 한 세트. Cowork가 매번 "이건 누구용이에요?"라고 묻지 않게 돼요.

### project로 좋은 작업 흐름

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🤝</span><span class="oa-card__title">고객 · 계정</span>회의록과 결과물 폴더, 그 계정의 상시 지침. 회의 준비 · 후속 조치 · 리뷰 초안이 모두 project 안에서 일어나고, Claude가 관계를 익혀 가요</div>
  <div class="oa-card"><span class="oa-card__icon">🔁</span><span class="oa-card__title">반복 결과물</span>월간 보고서, 분기 리뷰, 주간 리더십 업데이트. 새 주기마다 같은 project 안의 새 대화이고, 지난번 인사이트 위에 자동으로 쌓여요</div>
  <div class="oa-card"><span class="oa-card__icon">🚀</span><span class="oa-card__title">런칭 · 이니셔티브</span>리디자인, 마이그레이션, 새 프로그램 롤아웃. 브리프 · 결정 · 현황 업데이트가 출시될 때까지 한곳에</div>
</div>

## 4. Project를 시작하는 세 가지 방법

<div class="oa-eli">
  <span class="oa-eli__head">📦 쉽게 말하면</span>
  <p>새 사무실을 꾸리는 방법은 셋이에요. <b>빈 방에서 시작</b>하거나, <b>이미 쓰던 서류함을 통째로 옮겨 오거나</b>, <b>다른 방에서 쓰던 세팅을 그대로 가져오는</b> 거예요. 자료가 이미 어디 있느냐가 답을 정해요.</p>
</div>

| 방법 | 이럴 때 | 동작 |
|---|---|---|
| **처음부터 (from scratch)** | 아직 자료가 없을 때 | 빈 상태로 시작해 instructions · context를 더해 가요 |
| **내 컴퓨터의 기존 폴더에서** | 이미 작업하는 폴더가 있을 때 | 그 폴더를 가리키면 프로젝트의 working directory가 돼요 |
| **기존 project에서** | Claude와의 대화에서 이미 키워 온 project가 있을 때 | 그 project에서 시작해 instructions와 knowledge가 이어져요 |

만들기: 사이드바에서 **Projects → New project**를 누르고, 설정이 가장 쉬운 옵션을 골라요. 작업 폴더 · instructions · 커넥터는 언제든 바꿀 수 있어요.

> **이전 버전 노트와의 차이**: 처음 정리할 때 강의는 세 번째 방법을 "Chat 프로젝트에서"로 설명하고 **단방향**(Cowork에서의 변경은 Chat으로 역동기화되지 않음)이라고 안내했어요. 현재 강의 본문에는 단방향 설명이 없어서, 지금도 그런지는 확인하지 못했어요.

### 지금 해보기

두 층을 모두 놓아 보세요.

1. **global instructions 초안 쓰기**: 너무 고민하지 말고 몇 문장으로 시작해요. 위의 시작용 템플릿을 써도 좋아요.
2. **작업 흐름 하나에 project 만들기**: 반복되는 것(고객 계정, 월간 결과물, 진행 중인 이니셔티브)을 골라요. 자료가 이미 있는 폴더를 가리키거나, 아직 없으면 빈 상태로 시작해요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Claude에게 "결론부터 말해줘"라는 교정을 계속 반복하고 있어요. 어디에 넣어야 하나요?</summary>
  <p><b>정답: Global instructions.</b> 반복해서 주는 교정이 곧 global-instruction 후보예요. 한 번 써 두면 모든 세션에 적용돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. Project의 네 가지 구성 요소 중 사용자가 직접 쓰지 않는 것은?</summary>
  <p><b>정답: Memory.</b> Instructions · scheduled tasks · context는 내가 설정하고, memory는 프로젝트 안의 대화에서 Claude가 스스로 쌓아요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Project 안의 대화와 밖의 세션은 무엇이 다른가요?</summary>
  <p><b>정답: project 밖에서는 global instructions만 빼고 매번 새로 시작하지만, 안에서는 모든 대화가 Claude의 지식에 더해져요.</b> 그래서 다음 작업이 지난 결정과 미결 사항을 아는 상태로 시작돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 이미 고객 자료가 정리된 폴더가 내 컴퓨터에 있어요. project를 어떻게 시작하는 게 가장 쉬운가요?</summary>
  <p><b>정답: 기존 폴더에서 시작해요.</b> 그 폴더를 가리키면 프로젝트의 working directory가 돼요.</p>
</details>

## 정리

- 두 층을 함께 두세요: **나를 위한 global instructions, 작업 흐름을 위한 project.**
- Global instructions는 Settings에서 한 번 쓰는 **상시 브리프**예요. 역할, 약어, 출력 선호를 넣고, 반복하는 교정이 생기면 추가해요.
- Project는 **instructions · scheduled tasks · context · memory**로 이루어진 범위 좁힌 워크스페이스예요.
- Project의 차이는 **memory**예요. 안에서 나눈 모든 대화가 쌓여 다음 작업의 출발점이 돼요.
- Project는 **처음부터 / 기존 폴더에서 / 기존 project에서** 세 방법으로 시작할 수 있어요.

다음 레슨에서는 나와 팀이 의존하는 프로세스를 Claude에게 가르치는 skills를 만나요.
