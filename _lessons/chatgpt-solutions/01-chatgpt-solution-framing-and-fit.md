---
title: "ChatGPT Solution Framing & Fit"
title_ko: "ChatGPT 적합성 판단 — 고객 문제를 먼저 정의하고, 맞는 길을 고르기"
course: chatgpt-solutions
lesson: 1
---

## 학습 목표

- "챗봇", "AI 비서", "코파일럿" 같은 **포장지 단어** 뒤에 숨은 실제 업무 문제를 찾아내기
- ChatGPT Chat · ChatGPT Work · Codex · API · 혼합 경로 중 **어디서 시작할지** 판단하기
- 7가지 질문(problem frame)으로 고객 문제를 **두세 문장의 문제 정의**로 만들기
- 이해관계자(고통을 느끼는 사람·성과 책임자·속도를 늦출 수 있는 사람·우선순위를 만드는 사람)를 지도처럼 그리기
- 3가지 **fit check**로 "계속 추진할 가치가 있나? 다음에 무엇을 검증할까?"를 짧게 판단하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>적합 패턴 알아보기</strong>라벨이 아니라 업무 고통에서 출발</li>
  <li><strong>경로 고르기</strong>Chat · Work · Codex · API · 혼합</li>
  <li><strong>문제 정의</strong>7가지 질문 → 2~3문장</li>
  <li><strong>구매자·검증 신호</strong>누가 관련되고, 무엇이 비었나</li>
  <li><strong>Fit-check 판단</strong>추진 여부 + 다음 검증 공백</li>
</ol>

이 코스를 관통하는 한 문장: **ChatGPT 제안은 제품 기능이 아니라 "업무의 고통(workflow pain)"에서 시작한다.**

## 1. ChatGPT에 맞는 기회 알아보기

<div class="oa-eli">
  <span class="oa-eli__head">🩺 쉽게 말하면</span>
  <p>환자가 병원에 와서 "진통제 주세요"라고 해도, 좋은 의사는 바로 약을 주지 않아요. "어디가, 언제부터, 어떻게 아파요?"를 먼저 묻죠. 고객이 "AI 비서가 필요해요"라고 말하는 것도 <b>처방(제품) 요청</b>일 뿐, <b>증상(업무 문제)</b>은 아직 모르는 상태예요.</p>
</div>

### 핵심 질문: primary-output test

> **"이 업무에서 AI가 업무 담당자에게 무엇을 돌려주거나, 바꾸거나, 완성해 줘야 하나?"**

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 부족한 표현</span>
    <ul>
      <li>"챗봇을 원해요."</li>
      <li>"영업 코파일럿을 원해요."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 더 나은 표현</span>
    <ul>
      <li>"고객지원 매니저가 티켓 메모에서 <b>에스컬레이션 주제 요약</b>을 받아 주간 개선안을 준비해야 해요."</li>
      <li>"어카운트 팀이 통화 메모·계정 계획·CRM 자료로 <b>임원 리뷰 전 계정 브리핑</b>을 만들어야 해요."</li>
    </ul>
  </div>
</div>

좋은 표현에는 **사용자 · 업무 · 정보 출처 · 결과물** 네 가지가 다 들어 있어요.

### 강한 신호 vs 약한 신호

| 강한 적합 신호 | 아직 약한 신호 |
|---|---|
| "매주 같은 수작업 준비에 시간이 너무 들어요" | "HR용 챗봇이 필요해요" (어떤 업무인지 모름) |
| "정보가 문서·회의록·시스템에 흩어져 있어요" | 영향받는 사용자 그룹을 말 못 함 |
| "사람마다 같은 결과물을 다르게 만들어 품질이 들쭉날쭉해요" | 업무 고통·비즈니스 영향이 불분명 |
| "접근 권한·승인된 출처·검토에 대한 가드레일이 필요해요" | 데이터 제약·다음 검증 단계가 없음 |

### 4가지 대표 패턴

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">Knowledge access</span>승인된 자료에서 믿을 만한 답·요약·비교·브리핑 만들기 (예: 사내 정책을 쉬운 말로 요약)</div>
  <div class="oa-card"><span class="oa-card__icon">⚡</span><span class="oa-card__title">Work acceleration</span>초안·분석·계획·메시지 등 검토 가능한 결과물 빨리 만들기 (예: 회의록으로 후속 메일 초안)</div>
  <div class="oa-card"><span class="oa-card__icon">📏</span><span class="oa-card__title">Team consistency</span>사람마다 다른 결과물을 표준화·정책에 맞추기 (예: 고객 후속 요약 양식 통일)</div>
  <div class="oa-card"><span class="oa-card__icon">🧭</span><span class="oa-card__title">Decision support</span>비교·추천·다음 단계 안내 (예: 평가표로 공급사 비교)</div>
</div>

같은 계정 브리핑이라도 **대화하며 함께 다듬을지**, **맡기고 결과만 검토할지**, **한 사람이 계속 챙길지**, **팀이 공동으로 운영할지**에 따라 필요한 도구·권한·검토 단계가 달라져요.

## 2. ChatGPT냐, 다른 OpenAI 경로냐

<div class="oa-eli">
  <span class="oa-eli__head">🚲 쉽게 말하면</span>
  <p>"이동 수단이 필요해요"라는 말만으로 자전거·버스·트럭 중 뭘 살지 못 정해요. <b>누가 타고, 어디를 가고, 뭘 싣는지</b>를 알아야죠. OpenAI 제품 선택도 똑같아요.</p>
</div>

### 경로별 한 줄 규칙 (강의 기준)

| 경로 | 이럴 때 먼저 고려 | 예시 |
|---|---|---|
| **ChatGPT Chat** | 사람이 AI와 대화하며 생각·작성·분석·다듬기 | 지원 매니저가 대화로 주간 요약 작성 |
| **ChatGPT Work** | 결과물·맥락·한계를 정해 주고 **여러 단계 작업을 맡긴 뒤 검토** | 검토 가능한 주간 요약 준비를 위임 |
| **Codex** | 주 사용자가 개발자, 코드 작성·리뷰·테스트·디버깅 | PR 리뷰, 레거시 리팩터링 |
| **API** | AI가 기존 제품·포털·시스템 **안에 들어가야** 함 (사용자는 ChatGPT를 안 열 수도) | 은행 어드바이저 포털에 리서치 기능 내장 |
| **혼합(Hybrid)** | 사용자 그룹·시스템·팀이 여러 개 걸쳐 있음 | ChatGPT로 가치 검증 → 나중에 CRM에 API로 내장 |

> "챗봇"이 자동으로 ChatGPT를, "자동화"가 자동으로 API를, "개발 생산성"이 자동으로 Codex를 뜻하지 않아요. **너무 빨리 경로를 정하지 마세요.**

### 3개 층으로 나눠서 생각하기

<div class="oa-svg">
<svg viewBox="0 0 720 230" role="img" aria-label="경로 판단의 세 층">
  <defs>
    <marker id="c1a" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.c1q{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.c1g{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.c1t{font-size:13px;fill:#4a4a4a;font-weight:bold}.c1s{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="20" width="210" height="80" rx="10" class="c1q"/><text x="115" y="48" text-anchor="middle" class="c1t">① 사용자·제품 표면</text><text x="115" y="68" text-anchor="middle" class="c1s">사람·시스템이 어디서 AI를 만나나</text><text x="115" y="84" text-anchor="middle" class="c1s">Chat / Work / Codex / API</text>
  <rect x="255" y="20" width="210" height="80" rx="10" class="c1q"/><text x="360" y="48" text-anchor="middle" class="c1t">② 필요한 능력</text><text x="360" y="68" text-anchor="middle" class="c1s">협업·위임·연결 앱·검색·</text><text x="360" y="84" text-anchor="middle" class="c1s">액션·일정·자동화</text>
  <rect x="500" y="20" width="210" height="80" rx="10" class="c1q"/><text x="605" y="48" text-anchor="middle" class="c1t">③ 운영 모델</text><text x="605" y="68" text-anchor="middle" class="c1s">어떻게 연결·통제·운영하나</text><text x="605" y="84" text-anchor="middle" class="c1s">권한·승인·담당자·유지</text>
  <line x1="220" y1="60" x2="251" y2="60" stroke="#c99400" stroke-width="1.5" marker-end="url(#c1a)"/>
  <line x1="465" y1="60" x2="496" y2="60" stroke="#c99400" stroke-width="1.5" marker-end="url(#c1a)"/>
  <line x1="360" y1="100" x2="360" y2="136" stroke="#c99400" stroke-width="1.5" marker-end="url(#c1a)"/>
  <rect x="200" y="140" width="320" height="70" rx="10" class="c1g"/><text x="360" y="168" text-anchor="middle" class="c1t">검증 경로 (validation path)</text><text x="360" y="190" text-anchor="middle" class="c1s">이 조합을 다음에 믿을 만하게 시험해 볼 수 있나?</text>
</svg>
</div>

"에이전트", "연결 앱", "마이그레이션" 같은 것은 **제품 표면이 아니라 능력·운영 모델 요구**예요. 먼저 어디서 일할지를 고르고, 그다음 필요한 능력과 운영 방식을 따져요.

### 비교 기준 7가지

| # | 기준 | 핵심 질문 |
|---|---|---|
| 1 | 주요 비즈니스 결과물 | 라벨 뒤의 진짜 결과물은? (챗봇 → 승인된 정책 기반의 믿을 만한 답) |
| 2 | 공유 결과물과 유지 | 공유 페이지를 **누가 보고**, **누가 최신으로 유지**하나? |
| 3 | 사용자 경험 | 대화형? 위임형? 포털 안? 개발 도구? |
| 4 | 업무 소유권 | 일회성 작업 / 개인이 계속 챙김 / 팀 공동 운영 중 무엇? |
| 5 | 운영 맥락 | 필요한 문서·시스템·권한·검토·거버넌스는? |
| 6 | 통합 지점 | AI 기능이 어디에 살아야 하나? 지원되는 경험으로 충분한가? |
| 7 | 검증 경로 | 다음에 무엇을 시험할 수 있나? |

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📄</span><span class="oa-card__title">ChatGPT Space (강의 기준)</span>편집 가능한 페이지와 파일을 한곳에 모아 동료와 함께 작업. 페이지에 복사된 정보는 그 페이지 독자 모두가 볼 수 있으니 <b>독자 범위</b>를 확인.</div>
  <div class="oa-card"><span class="oa-card__icon">👤</span><span class="oa-card__title">dot vs Team Tasks (강의 기준)</span><b>dot</b>: 한 사람을 위해 계속 일하는 개인 에이전트. <b>Team Tasks</b>: 팀이 함께 관리하는 반복 작업(일정·이벤트, 팀 서비스 계정). 만든 사람이 떠나면 연결 계정은?</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🔌</span><span class="oa-card__title">"앱이 필요해요" ≠ API (강의 기준)</span>플러그인 확장, MCP Events, ChatGPT Sites처럼 <b>지원되는 경험</b>으로 충분할 수 있어요. 그걸 넘어설 때 API 빌드를 검토.</div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">⏱️ 시간 절약 계산의 함정</span>
  <p>답이 화면에 빨리 뜨기 시작해도, 추론·도구 작업이 끝나고 <b>검토자가 확인·수정을 마친 시점</b>까지가 진짜 소요 시간이에요. 모델·추론 수준·속도 등급 비교도 "결과 품질 + 수정 시간 + 비용"으로 해야 공정해요.</p>
</div>

## 3. 솔루션보다 고객 문제를 먼저 정의하기

<div class="oa-eli">
  <span class="oa-eli__head">🧩 쉽게 말하면</span>
  <p>"공부가 안 돼요"는 너무 넓어요. "매주 월요일 수학 숙제를 할 때, 교과서·필기·인강을 왔다 갔다 하느라 2시간이 걸려서 다른 과목을 못 해요"라고 하면 해결책이 보이기 시작하죠.</p>
</div>

### Problem frame — 7가지 질문

<ol class="oa-flow">
  <li><strong>① 트리거</strong>언제 문제가 생기나? (주간 보고, 회의 전)</li>
  <li><strong>② 구체적 고통</strong>무엇이 느리고·중복되고·들쭉날쭉한가?</li>
  <li><strong>③ 영향받는 사람</strong>누가 일하고, 누가 결과물을 쓰나?</li>
  <li><strong>④ 현재 우회책</strong>지금은 어떻게 버티나? 어디서 깨지나?</li>
  <li><strong>⑤ 비즈니스 결과</strong>안 고치면 무엇이 늦어지고 안 보이나?</li>
  <li><strong>⑥ 걸린 결정·산출물</strong>무엇을 만들고 누가 다음에 쓰나?</li>
  <li><strong>⑦ "더 나은" 상태</strong>무엇이 빨라지고 일관돼야 하나?</li>
</ol>

### 문제 정의 문장 템플릿

```text
[역할/팀]은 [트리거] 때 [구체적 업무 문제]로 어려움을 겪는다.
지금은 [현재 우회책]을 쓰고 있어 [비즈니스 결과]가 생긴다.
고객은 [더 나은 상태 / 걸린 산출물]을 원한다.
```

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 문제 정의</span>
    <ul>
      <li>"고객은 영업에서 시간을 아끼려고 ChatGPT를 쓰고 싶어 한다."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 문제 정의</span>
    <ul>
      <li>"어카운트 매니저는 분기 리뷰 전 일관된 임원 브리핑을 준비하기 어렵다. 통화 메모·CRM·계정 계획을 손으로 뒤져 준비가 느리고, 리더십이 갱신 위험을 비교하기 어렵다. 고객은 더 빠르고 일관된 리뷰용 요약을 원한다."</li>
    </ul>
  </div>
</div>

## 4. 구매자 맥락과 초기 검증 신호

### 같은 기회, 다른 관심사

| 구매자 | 주로 신경 쓰는 것 | 이렇게 말하면 통해요 |
|---|---|---|
| **비즈니스 리더** | 가치·일관성·속도·품질·영향 | "리더십이 계정 위험을 더 잘 보게 돼요" |
| **IT·보안** | 거버넌스·접근·데이터 경계·리스크 | "어떤 승인된 출처·접근·검토 단계가 안전한 사용을 결정하는지가 먼저예요" |
| **실무자(operator)** | 일이 쉬워지고 쓸 만한가 | "리뷰 전에 정보 이어 붙이는 시간을 줄여요" |

### 이해관계자 지도 — 4가지 역할

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">😣</span><span class="oa-card__title">고통을 느끼는 사람</span>매일 그 일을 하는 실무자. 지금 어디서 막히는지 설명 가능</div>
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">성과 책임자</span>비즈니스 결과에 책임지는 사람 (예: 영업운영 책임자)</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚧</span><span class="oa-card__title">속도를 좌우하는 사람</span>IT·보안·법무·구매·컴플라이언스. 늦게 부르면 블로커가 됨</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🚀</span><span class="oa-card__title">우선순위를 만드는 사람</span>이 문제를 회사 목표와 연결해 급하게 만들어 줄 임원</div>
</div>

작은 팀에서 한 사람이 여러 역할을 맡아도, **누가 접근을 승인하고 누가 결과물을 받아들이는지**는 따로 확인해야 해요.

### 검증 신호 분류 — 3단계 라벨

검토할 신호: 명확한 문제 · 이해관계자 · 결과물 언어로 된 사용 사례 · 고객 참여 신호(합의된 다음 단계) · 가치 가설 · 필요한 자료 · 거버넌스/검토 요구 · 성공 지표.

<div class="oa-bars">
  <div class="oa-bar"><span>결과물 언어로 된 사용 사례</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:90%"></span><span class="oa-bar__val">Enough to pursue</span></span></div>
  <div class="oa-bar"><span>문제·이해관계자·자료</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:55%"></span><span class="oa-bar__val">Needs validation</span></span></div>
  <div class="oa-bar"><span>거버넌스·성공 지표·참여 신호</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:12%"></span><span class="oa-bar__val">Missing</span></span></div>
  <p class="oa-bars__caption">강의의 "갱신 전 고객성공 매니저 지원" 예시를 분류한 개념적 표현이에요 (실제 수치 아님). → 다음 단계는 핸드오프가 아니라 <b>검증</b>.</p>
</div>

## 5. 3가지 ChatGPT fit check

<div class="oa-eli">
  <span class="oa-eli__head">👟 쉽게 말하면</span>
  <p>신발 살 때 ① 내가 할 운동에 맞나(용도), ② 이 브랜드·모델이 맞나(제품), ③ 신어 보고 걸어 볼 수 있나(시착)를 확인하죠. Fit check도 <b>용도 → 경로 → 검증</b> 세 단계예요.</p>
</div>

| Fit check | 질문 | 찾아야 할 것 |
|---|---|---|
| **① Workflow & output fit** | 분명한 지식 업무 문제가 있고, 결과물을 이름 붙일 수 있나? | 사용자 그룹, 반복 업무, 결과물, 그 업무가 중요한 이유 |
| **② Solution-path fit** | Chat으로 함께? Work로 위임? 계속되는 일이면 개인/팀 소유? | 거버넌스 환경의 업무 사용자, 소유자·독자, 내장 필요 없음, 승인·검토·중단 권한자 |
| **③ Validation fit** | 계속 추진할 만큼 아나? 가장 중요한 검증 공백은? | 다음 단계 합의, 가치 가설, 자료·권한 파악, 성공 지표 |

첫 테스트는 **대표 작업 1개 + 승인된 입력 + 합격 기준**을 정하고, 결과 품질·사람의 수정 시간·완료 시간·실제 비용으로 비교해요.

### Fit-check 판단 문장 템플릿

```text
[기회]는 [업무·결과물 적합] 때문에 ChatGPT 기회로 추진할 가치가 있다/아직 없다.
[Chat / Work / 다른 경로]가 [표면·능력·운영 모델 적합] 때문에 첫 경험으로 맞다/안 맞다.
다음 검증 공백은 [검증 적합]이다.
```

<div class="oa-eli">
  <span class="oa-eli__head">🏭 예시 (제조사 QBR 준비)</span>
  <p>어카운트 팀이 회의록·티켓·갱신 이력·사용량·메일로 분기 리뷰(QBR) 브리핑을 만드는 데 계정당 몇 시간이 걸리고 품질이 사람마다 달라요.<br>→ 추진 가치 있음. 초안 위임이면 <b>Work</b>, 직접 분석하며 다듬으면 <b>Chat</b>. 내장이 필요해지면 나중에 API, Codex는 해당 없음. 반복 프로세스로 돌리려면 개인 dot보다 <b>Team Tasks</b>와 그 연결을 점검. 다음 검증: 사용 가능한 출처, 브리핑 검토자, 공유 페이지 독자와 유지 담당.</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 고객이 "재무팀용 코파일럿이 필요해요"라고 한다. 적합성 판단에 가장 도움이 되는 후속 질문은?</summary>
  <p>① 어떤 재무 업무가 지금 힘들고, 누가 하며, 어떤 결과물이 필요한가? ② 커스텀 앱부터 만들까? ③ 라이선스 몇 개 살까? ④ 최신 모델을 쓸까?</p>
  <p><b>정답: ①</b> — 라벨 뒤의 업무·사용자·결과물(primary output)을 묻는 질문이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. "엔지니어의 PR 리뷰, 디버깅, 코드 품질 개선을 돕고 싶어요." 먼저 볼 경로는?</summary>
  <p><b>정답: Codex</b> — 소프트웨어 개발·코드·디버깅·리뷰가 주된 업무이기 때문. (모든 AI가 API여야 한다 ✗ / 모든 엔지니어링은 혼합 ✗ / 무조건 ChatGPT부터 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q3. ChatGPT를 제안하기 전 problem frame에 들어가야 할 질문은?</summary>
  <p><b>정답:</b> "AI가 어떤 결과물을 만들고·개선하고·찾고·검토하고·종합하도록 도와야 하나?" (인터페이스 색상, 관리자 설정, 요금제 질문은 아직 아님)</p>
</details>

<details class="oa-quiz">
  <summary>Q4. HR 정책 질문 지원 기회에서 고통 당사자·성과 책임자는 확인됐지만 승인된 자료와 검토 기대치가 없다. 어떻게 분류?</summary>
  <p><b>정답: Missing</b> — 운영 맥락과 거버넌스 기대치가 아직 불분명하기 때문. ("ChatGPT는 어떤 정책 질문이든 답한다"거나 "영업에서 다룰 주제가 아니다"는 오답)</p>
</details>

<details class="oa-quiz">
  <summary>Q5. 주간 리더십 업데이트 준비 지원 — 가장 강한 fit-check 판단은?</summary>
  <p><b>정답:</b> "지원 매니저가 티켓 메모로 주간 업데이트를 만들어야 하므로 추진 가치가 있다. ChatGPT가 맞을 수 있지만 출처와 검토 단계는 검증이 필요하다." — 간결·구체적·증거 기반. (API 먼저 구축 ✗ / 바로 배포 준비 완료 ✗ / 세부는 나중에 ✗)</p>
</details>

## 정리

- **라벨이 아니라 업무 고통**에서 출발하고, primary-output test로 결과물을 이름 붙인다.
- 경로 판단은 **표면 → 필요한 능력 → 운영 모델 → 검증 경로** 순으로. "앱"·"자동화"·"에이전트"라는 단어만으로 경로를 정하지 않는다.
- 계속되는 일이면 **개인(dot) vs 팀(Team Tasks)** 소유와 공유 결과물의 **독자·유지 담당**을 따로 확인한다.
- 7가지 질문으로 2~3문장 문제 정의를 만들고, 이해관계자 4역할을 지도로 그린다.
- 3가지 fit check로 "추진할 가치 + 맞는 첫 경험 + 다음 검증 공백"을 한 단락으로 판단한다.
