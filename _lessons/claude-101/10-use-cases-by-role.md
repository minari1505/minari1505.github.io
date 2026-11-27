---
title: "Use cases by role"
title_ko: "역할별 Claude 활용 사례"
course: claude-101
lesson: 10
---

## 학습 목표

- 바로 시도해 볼 수 있는 claude.ai 활용 사례를 **2~3개 이상** 떠올리기
- 직무별로 Claude가 어떤 업무를 줄이고 보강할 수 있는지 이해하기
- 추가 use case를 찾을 수 있는 **Use Case Gallery** 활용 방향 알기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>공통 업무</strong>status report · feedback 분석 · brand guideline Skill</li>
  <li><strong>직무별 사례</strong>Sales · Marketing · Finance · HR · Legal · Research</li>
  <li><strong>더 찾아보기</strong>Use Case Gallery의 step-by-step guide</li>
  <li><strong>내 업무에 적용</strong>반복 업무 하나를 골라 작게 시작</li>
</ol>

이 레슨을 한 문장으로 줄이면: **거창한 자동화가 아니라, 지금 반복하고 있는 실제 업무 하나를 골라 Claude와 함께 해 보는 것부터 시작해요.**

## 1. Claude를 실제 업무에 적용하기

<div class="oa-eli">
  <span class="oa-eli__head">🍳 쉽게 말하면</span>
  <p>칼질, 불 조절, 간 맞추기를 배웠다면 이제 <b>"오늘 저녁에 뭘 만들지"</b>를 정할 차례예요. 이 레슨은 직무별 레시피 모음이에요. 전부 만들 필요 없이, 내 주방에 맞는 것 두세 개만 골라 가면 돼요.</p>
</div>

지금까지 Claude와 대화하고, project로 맥락을 유지하고, artifact와 skills로 결과물을 만들고, connectors와 research로 Claude의 reach를 넓히는 방법을 배웠어요. 이제 중요한 질문은 이거예요.

> 내 업무에서는 Claude를 어디에 바로 써 볼 수 있을까?

Claude는 특정 직무에만 유용한 도구가 아니에요. 글쓰기, 분석, 자료 정리, 보고서 작성, 반복 업무 자동화처럼 많은 직무에서 업무를 더 빠르고 일관되게 만들어 줘요.

| 역할 | 강의가 소개하는 use case |
|---|---|
| **공통 (General)** | project status report 생성 · user feedback pattern 분석 · brand guideline을 Skill로 패키징 |
| **Sales** | battle card library 만들기 · sales deal 준비 · sales report 생성 |
| **Marketing** | campaign performance 분석 · platform별 content 변환 |
| **Finance** | financial model 만들기 · investment memo 작성 · 물려받은 spreadsheet 이해와 확장 |
| **HR** | new hire onboarding guide 작성 |
| **Legal** | discovery timeline 추적과 pattern 분석 |
| **Research** | literature review 계획 · raw data 통계 검증 |

> 아래 각 use case의 예시 prompt는 이해를 돕기 위한 것이에요. 강의의 각 use case는 **Use Case Gallery**의 상세 guide와 연결되어 있고, 따라 할 수 있는 step-by-step 안내는 거기에 있어요.

## 2. 공통 업무 활용 사례

<div class="oa-eli">
  <span class="oa-eli__head">🧹 쉽게 말하면</span>
  <p>어느 집이든 <b>청소·빨래·설거지</b>는 있죠. 직무가 달라도 "상황 보고하기", "의견 모아 정리하기", "규칙 지키게 하기"는 누구에게나 있는 일이에요.</p>
</div>

많은 직무와 산업에 공통으로 적용할 수 있는 활용 사례예요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📊</span><span class="oa-card__title">Project status report 생성</span>stakeholder에게 명확하고 일관된 업데이트를 전해요</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">User feedback pattern 분석</span>고객 comment, survey response, support ticket에서 insight를 뽑아요</div>
  <div class="oa-card"><span class="oa-card__icon">🎨</span><span class="oa-card__title">Brand guideline을 Skill로</span>brand standard를 적용하는 재사용 가능한 Skill을 만들어요</div>
</div>

### Project status report 생성

```text
아래 회의록과 task 상태를 바탕으로 이번 주 project status report를 작성해줘.
진행 상황, 리스크, 다음 액션을 분리해서 정리해줘.
```

반복 보고라면 project instructions나 skill로 format을 고정해 둘 수 있어요.

### User feedback pattern 분석

```text
이 사용자 feedback을 분석해서 반복적으로 등장하는 불만, 요청 기능, 긍정 의견을 category별로 정리해줘.
각 category마다 대표 quote도 2개씩 뽑아줘.
```

### Brand guideline을 Skill로 패키징

매번 brand rule을 설명하지 않고, brand standard를 적용하는 custom skill로 만들 수 있어요.

```text
우리 brand guideline을 기반으로 모든 marketing copy를 검토하는 Skill을 만들고 싶어.
```

## 3. 직무별 활용 사례

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍🤝‍🧑 쉽게 말하면</span>
  <p>같은 만능 조수라도 <b>영업팀에 가면 미팅 준비</b>를, <b>재무팀에 가면 엑셀 해석</b>을 도와요. 조수가 바뀌는 게 아니라, 맡기는 일이 팀마다 다를 뿐이에요.</p>
</div>

### Sales

deal 준비를 앞당기고, 설득력 있는 자료를 만들고, 경쟁 정보를 놓치지 않는 데 써요.

**Battle card library 만들기** — 경쟁사별 강점, 약점, objection handling, positioning을 정리한 battle card로 팀이 deal을 따내도록 도와요.

```text
이 경쟁사 자료와 고객 통화 기록을 바탕으로 sales battle card를 만들어줘.
섹션은 competitor overview, key differentiators, common objections, response talk tracks로 구성해줘.
```

**Sales deal 준비** — 중요한 미팅 전에 prospect를 조사하고 talking point를 정리해요.

```text
Acme Corp과의 미팅을 준비하려고 해.
최근 뉴스, 회사 개요, 우리 제품과 연결될 만한 pain point, 예상 질문을 정리해줘.
```

**Sales report 생성** — pipeline data를 명확하고 실행 가능한 보고서로 바꿔요.

```text
이 pipeline export를 분석해서 이번 분기 forecast, 위험 deal, 다음 액션을 보고서로 정리해줘.
```

### Marketing

성과 데이터를 분석하고, content를 채널에 맞게 효율적으로 재활용하는 데 써요.

**Campaign performance 분석** — campaign metric을 올리고 성과, 개선점, 다음 실험을 도출해요.

```text
이 campaign performance 데이터를 분석해서 가장 성과가 좋았던 channel, conversion bottleneck, 다음 실험 아이디어를 정리해줘.
```

**Platform별 content 변환** — 하나의 long-form content를 LinkedIn, newsletter, blog, X thread 등으로 바꿔요.

```text
이 blog post를 LinkedIn post 3개, newsletter intro 1개, 짧은 X thread로 변환해줘.
각 channel의 톤과 길이에 맞춰줘.
```

### Finance

model 작성, 문서 초안, 복잡한 spreadsheet 이해에 써요.

**Financial model 만들기** — 가정과 입력값을 바탕으로 재무 projection이나 scenario analysis를 만들고 다듬어요.

```text
이 revenue assumption을 바탕으로 3년 financial projection model을 만들어줘.
base, upside, downside scenario를 포함해줘.
```

**Investment memo 작성** — 투자 분석 자료를 구조화하고 핵심 argument를 정리해요.

```text
이 회사 자료와 market research를 바탕으로 investment memo 초안을 작성해줘.
섹션은 company overview, market, financials, risks, recommendation으로 구성해줘.
```

**물려받은 spreadsheet 이해와 확장** — 복잡한 workbook을 해독해 formula flow를 설명하고 새 기능을 더해요.

```text
이 spreadsheet의 주요 계산 흐름을 설명하고,
새 cost assumption을 추가하려면 어떤 sheet와 formula를 바꿔야 하는지 알려줘.
```

### HR

더 나은 onboarding 경험과 documentation을 만드는 데 써요.

**New hire onboarding guide 작성** — 직무별 onboarding 자료를 만들고, 역할에 맞는 checklist와 first-week plan을 구성해요.

```text
신규 customer success manager를 위한 onboarding guide를 만들어줘.
첫 30일 계획, 읽어야 할 문서, 만나야 할 사람, 실습 과제를 포함해줘.
```

### Legal

복잡한 timeline을 추적하고 discovery process를 관리하는 데 써요.

**Discovery timeline 추적과 pattern 분석** — 여러 문서에서 사건 timeline을 정리하고 중요한 pattern을 찾아요.

```text
이 legal documents에서 discovery timeline을 정리해줘.
날짜, 관련 인물, 주요 사건, 증거 문서 링크를 표로 만들어줘.
반복되는 pattern이나 누락된 구간도 표시해줘.
```

### Research

literature review를 계획하고 데이터 분석을 검증하는 데 써요.

**Literature review 계획** — 연구 질문과 source를 바탕으로 review 접근 방식을 짜요.

```text
이 연구 주제에 대한 literature review 계획을 세워줘.
핵심 subtopic, 검색 keyword, inclusion/exclusion criteria, synthesis structure를 포함해줘.
```

**Raw data 통계 검증** — 직접 계산한 통계나 분석 결과를 Claude에게 다시 확인하게 해요.

```text
이 raw data와 내가 계산한 summary statistics를 비교해서 오류가 있는지 확인해줘.
계산 과정과 불일치 항목을 설명해줘.
```

## 4. Use Case Gallery 활용하기

<div class="oa-eli">
  <span class="oa-eli__head">📖 쉽게 말하면</span>
  <p>이 레슨이 <b>메뉴판</b>이라면 Use Case Gallery는 <b>재료와 순서가 적힌 요리책</b>이에요. 처음엔 레시피대로 만들어 보고, 익숙해지면 내 입맛에 맞게 바꾸면 돼요.</p>
</div>

여기 소개한 사례는 시작일 뿐이에요. 각 use case는 Anthropic의 **Use Case Gallery**에 있는 상세 guide와 연결되어 있어요. Gallery에서는 이런 걸 찾을 수 있어요.

- 내 직무와 비슷한 활용 사례
- 바로 따라 할 수 있는 step-by-step prompt
- Claude 기능을 조합하는 실제 workflow
- Projects, Artifacts, Skills, Connectors, Research를 함께 쓰는 예시

처음에는 gallery의 prompt를 그대로 따라 해 보고, 그다음 내 업무 맥락에 맞춰 고치는 방식이 좋아요.

## 5. 내 업무에 적용하는 방법

<div class="oa-eli">
  <span class="oa-eli__head">🌱 쉽게 말하면</span>
  <p>운동을 시작할 때 첫날부터 마라톤을 뛰지 않죠. <b>매주 하던 일 하나</b>를 Claude와 같이 해 보고, 잘 되면 그때 루틴으로 굳혀요.</p>
</div>

<ol class="oa-flow">
  <li><strong>업무 고르기</strong>반복해서 하는 업무 하나</li>
  <li><strong>자료 모으기</strong>그 업무에 필요한 입력 자료</li>
  <li><strong>기준 설명하기</strong>원하는 결과 형식과 판단 기준</li>
  <li><strong>개선하기</strong>첫 결과를 보고 follow-up으로 다듬기</li>
  <li><strong>고정하기</strong>반복성이 있으면 Project · Skill · Artifact로</li>
</ol>

### 예시 — 매주 status report를 쓴다면

| 단계 | 하는 일 |
|---|---|
| 자료 넣기 | Project에 관련 회의록과 task export를 넣어요 |
| 형식 고정 | Project instructions에 report template을 저장해요 |
| 생성 | Claude에게 이번 주 상태 보고서를 만들게 해요 |
| 발전 | 반복되는 수정 규칙은 Skill로 발전시켜요 |

### 스스로 물어볼 질문

- 내가 매주 반복하는 보고나 정리 작업은 무엇인가요?
- 여러 문서나 대화에서 정보를 모아야 하는 업무는 무엇인가요?
- 초안 작성에 시간이 오래 걸리는 문서는 무엇인가요?
- Claude와 함께 만들면 바로 공유하거나 재사용할 artifact는 무엇인가요?
- Use Case Gallery에서 내 직무와 가장 가까운 guide는 무엇인가요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 짝 맞추기 — 역할과 use case</summary>
  <p>"battle card library 만들기" → <b>Sales</b><br>"platform별 content 변환" → <b>Marketing</b><br>"물려받은 spreadsheet 이해와 확장" → <b>Finance</b><br>"discovery timeline 추적과 pattern 분석" → <b>Legal</b><br>"raw data 통계 검증" → <b>Research</b><br>"new hire onboarding guide 작성" → <b>HR</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. 강의가 소개한 use case를 더 자세히, 단계별로 따라 하고 싶어요. 어디로 가야 할까요?</summary>
  <p>① Connector directory ② Use Case Gallery ③ Settings &gt; Extensions ④ Enterprise Search</p>
  <p><b>정답: ②</b> — 각 use case는 Use Case Gallery의 상세 guide와 연결되어 있고, step-by-step 안내가 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 매번 brand rule을 Claude에게 다시 설명하는 게 번거로워요. 강의가 제안하는 방법은?</summary>
  <p><b>정답: brand guideline을 Skill로 패키징</b>해요. brand standard를 적용하는 재사용 가능한 Skill을 만들면 매번 설명할 필요가 없어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Claude를 업무에 적용하는 첫걸음으로 가장 알맞은 것은?</summary>
  <p>① 팀의 모든 업무를 한 번에 자동화하기 ② 반복해서 하는 업무 하나를 골라 입력 자료를 모으고 Claude와 해 보기 ③ 모든 connector를 먼저 연결하기 ④ Skill부터 여러 개 만들어 두기</p>
  <p><b>정답: ②</b> — 작게 시작해서 follow-up으로 개선하고, 반복성이 확인되면 Project·Skill·Artifact로 고정해요.</p>
</details>

## 정리

- Claude는 특정 직무용 도구가 아니에요. 글쓰기, 분석, 정리, 보고, 반복 업무에서 직무를 가리지 않고 쓸 수 있어요.
- **공통 업무**에서는 status report, feedback analysis, brand guideline Skill이 유용해요.
- **Sales**는 battle card · deal prep · sales report, **Marketing**은 campaign analysis · content repurposing, **Finance**는 model · investment memo · spreadsheet 이해에 써요.
- **HR**은 onboarding guide, **Legal**은 discovery timeline, **Research**는 literature review와 statistics verification에 잘 맞아요.
- 더 많은 사례와 step-by-step guide는 **Use Case Gallery**에 있어요. 그대로 따라 해 본 뒤 내 맥락에 맞게 고쳐요.
- 핵심은 거창한 자동화가 아니라, **지금 반복하는 실제 업무 하나**를 골라 Claude와 더 빠르고 일관되게 처리해 보는 거예요.

다음 레슨에서는 Claude.ai 외에도 Claude Code, Claude Tag, Claude Design, Claude for Microsoft 365, Claude in Chrome처럼 다른 작업 환경에서 Claude를 쓰는 방법을 살펴봐요.
