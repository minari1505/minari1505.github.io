---
title: "Technical Fluency Orientation"
title_ko: "기술 유창성 오리엔테이션 — 제품 이름보다 먼저 워크플로를 보는 법"
course: openai-technical-practitioner
lesson: 1
---

## 학습 목표

- **제품 친숙도**(이름을 아는 것)와 **실전 기술 판단**(무엇을 물어야 하는지 아는 것)을 구분하기
- **3축 라우팅 모델** — 사용자/제품 surface · 역량/아키텍처 · 배포/운영 — 으로 기회를 분류하기
- 컨텍스트·데이터 흐름·런타임·도구·eval·거버넌스·관측성이 라우팅에 미치는 영향 설명하기
- 확신 수준을 구분해 **과장 없이(overclaiming 없이)** 초기 추천하기
- 나만의 **Technical Fluency Navigator**(1페이지 준비 메모) 만들기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>3축으로 지도 그리기</strong>어디서 · 무엇을 · 어떻게 운영</li>
  <li><strong>4개의 발견 레이어</strong>사용자 · 데이터 · 런타임 · 거버넌스</li>
  <li><strong>판단·검증·에스컬레이션</strong>Level 1 추천 + 트레이드오프</li>
  <li><strong>Navigator 만들기</strong>대화 전에 펼쳐 보는 1장짜리 메모</li>
</ol>

이 코스의 핵심 질문: **"설계를 과하게 하거나, 과장하거나, 엉뚱한 팀에 넘기기 전에 — 이 기술 기회를 책임감 있게 어디로 보낼까?"**

## 1. 왜 기술 유창성(Technical Fluency)이 필요할까

<div class="oa-eli">
  <span class="oa-eli__head">🩺 쉽게 말하면</span>
  <p>환자가 "배가 아파요"라고 하면, 좋은 의사는 바로 약 이름을 말하지 않아요. <b>어디가, 언제부터, 어떻게</b> 아픈지 먼저 묻죠. 약 이름을 많이 아는 것(제품 친숙도)보다 <b>무엇을 물어야 할지 아는 것</b>(기술 판단)이 진짜 실력이에요.</p>
</div>

고객 대화는 보통 막연한 요청으로 시작해요. "운영팀용 AI 어시스턴트가 필요해요", "이 업무 자동화할 수 있나요?", "ChatGPT로 할까요, API로 만들까요?"

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🏷️</span><span class="oa-card__title">Product familiarity</span>OpenAI 제품 이름과 대략 무엇을 하는지 아는 것</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🧭</span><span class="oa-card__title">Practical technical judgment</span>적합성을 가르는 질문, 중요한 트레이드오프, 추천 전에 검증할 것을 아는 것</div>
  <div class="oa-card"><span class="oa-card__icon">🧩</span><span class="oa-card__title">Computational thinking</span>문제를 단계별로 쪼개고, 필요한 정보·접근 가능한 데이터·선택지를 따져 가장 알맞은 것을 고르는 사고</div>
</div>

## 2. 3축 라우팅 모델 — 섞지 말고 나눠서 결정하기

<div class="oa-eli">
  <span class="oa-eli__head">🍕 쉽게 말하면</span>
  <p>피자를 시킬 때 <b>어디서 먹을지</b>(매장·배달), <b>무슨 피자인지</b>(토핑), <b>어떻게 결제·확인할지</b>는 서로 다른 결정이에요. "배달 페퍼로니 카드결제"는 세 결정을 <b>조합한 결과</b>지, 메뉴판의 또 다른 한 줄이 아니죠.</p>
</div>

| 결정 축 | 올바른 질문 | 예시 (강의 기준) |
|---|---|---|
| **사용자/제품 surface** | 사람이나 워크플로가 주로 어디서 일하나? | ChatGPT Chat · ChatGPT Work · Codex · API로 만든 앱/프로세스 |
| **역량/아키텍처** | 시스템이 무엇을 해야 하나? | Apps와 도구, Workspace Agents, retrieval, structured outputs, 코드 실행, 자동화 |
| **배포/운영** | 어디서, 어떻게 실행되고 통제되나? | 로컬/클라우드, 호스팅/임베디드, 신원·권한·승인·보존·로깅·사람 검토, 고객 운영 open-weight 모델 평가 |

ChatGPT 안에서도 둘을 구분해요 (강의 기준).

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">💬 ChatGPT Chat</span>
    <ul>
      <li>사용자가 옆에 붙어서 함께 탐색·초안·비평·다듬기</li>
      <li>예: 정책 Q&amp;A를 주고받으며 초안 작성</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">📦 ChatGPT Work</span>
    <ul>
      <li>결과물·맥락·제약·승인 경계·검토 기준을 정해 <b>맡기는</b> 지식 업무</li>
      <li>예: 승인된 자료로 검토용 정책 브리프 만들기</li>
    </ul>
  </div>
</div>

### 초기 적합성 신호 (fit signals)

| 신호 | 그럴듯한 출발점 |
|---|---|
| 함께 탐색·초안·다듬기가 필요 | ChatGPT Chat |
| 결과·맥락·제약·검토 기준을 정의해 맡길 수 있음 | ChatGPT Work |
| 저장소·테스트·리뷰·디버깅·문서화 중심 | Codex |
| 제품·시스템·고객 경험 안에 지능을 **심어야** 함 | API로 만든 앱/프로세스 |
| 승인된 컨텍스트, 검색, 구조화 출력, 도구, 자동화가 필요 | 역량/아키텍처 결정 |
| 신원·권한·승인·보존·로깅·호스팅·운영 주체가 판단을 바꿀 수 있음 | 배포/운영 결정 |

> open-weight 모델은 "데이터가 민감해서"만으로 고르는 게 아니라 **구체적인 배포 요구사항** 때문에 검토하는 것이에요.

### 흔한 라우팅 실수

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">📋</span><span class="oa-card__title">한 줄로 나열</span>ChatGPT·Codex·API·에이전트·open-weight를 동급 선택지 목록처럼 취급</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🤖</span><span class="oa-card__title">에이전트 = 제품?</span>에이전트·자동화는 surface가 아니라 <b>역량/아키텍처</b> 선택</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🏠</span><span class="oa-card__title">open-weight = surface?</span>모델·<b>배포</b> 결정이지 사용자 surface가 아님</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⏰</span><span class="oa-card__title">통제는 나중에</span>권한·승인·로깅·사람 검토·운영 주체를 늦게 따짐</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🌀</span><span class="oa-card__title">"하이브리드"</span>각 축에서 뭘 결정했는지 설명 없이 뭉뚱그림</div>
</div>

## 3. 기본 멘탈 모델 — 4개의 발견 레이어

3축이 "결정"이라면, 4개 레이어는 그 결정을 위한 **증거 수집 도구**예요. 레이어가 3축을 대체하지 않아요.

<div class="oa-svg">
<svg viewBox="0 0 720 230" role="img" aria-label="4개 발견 레이어가 3개 라우팅 축으로 이어지는 그림">
  <defs>
    <marker id="ar1t" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.l1{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.a1{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.t1{font-size:12px;fill:#4a4a4a}.h1{font-size:13px;font-weight:bold;fill:#8a6d00}</style>
  </defs>
  <text x="130" y="18" text-anchor="middle" class="h1">발견 레이어 (증거 모으기)</text>
  <rect x="20" y="30" width="220" height="38" rx="8" class="l1"/><text x="130" y="54" text-anchor="middle" class="t1">👤 사용자/워크플로</text>
  <rect x="20" y="78" width="220" height="38" rx="8" class="l1"/><text x="130" y="102" text-anchor="middle" class="t1">📚 컨텍스트/데이터</text>
  <rect x="20" y="126" width="220" height="38" rx="8" class="l1"/><text x="130" y="150" text-anchor="middle" class="t1">⚙️ 런타임/도구</text>
  <rect x="20" y="174" width="220" height="38" rx="8" class="l1"/><text x="130" y="198" text-anchor="middle" class="t1">🔍 거버넌스/관측성</text>
  <line x1="250" y1="121" x2="420" y2="121" stroke="#c99400" stroke-width="2" marker-end="url(#ar1t)"/>
  <text x="335" y="112" text-anchor="middle" class="t1">근거로 사용</text>
  <text x="565" y="18" text-anchor="middle" class="h1">라우팅 축 (결정하기)</text>
  <rect x="430" y="45" width="270" height="44" rx="8" class="a1"/><text x="565" y="72" text-anchor="middle" class="t1">어디서? 사용자/제품 surface</text>
  <rect x="430" y="99" width="270" height="44" rx="8" class="a1"/><text x="565" y="126" text-anchor="middle" class="t1">무엇을? 역량/아키텍처</text>
  <rect x="430" y="153" width="270" height="44" rx="8" class="a1"/><text x="565" y="180" text-anchor="middle" class="t1">어떻게 통제? 배포/운영</text>
</svg>
</div>

| 레이어 | 묻는 것 |
|---|---|
| **사용자/워크플로** | 누가 도움이 필요한가? 어떤 일, 어떤 결과물? 반복 업무인가, 일회성인가? |
| **컨텍스트/데이터** | AI가 알거나 찾아야 할 것은? 대표성 있고 최신이며 쓸 만한 데이터가 있나? |
| **런타임/도구** | 어디서 실행되나(로컬/클라우드)? 어떤 시스템·도구에 접근하고 어떤 행동이 허용되나? |
| **거버넌스/관측성** | 어떻게 모니터링·통제·평가·개선하나? 중간 단계 기록이 남나? |

<div class="oa-eli">
  <span class="oa-eli__head">✈️ 런타임과 관측성, 쉽게 말하면</span>
  <p><b>런타임</b>은 프로그램이 실제로 돌아가는 "작업장"이에요. 작업장이 어디냐에 따라 어떤 서랍(시스템)을 열 수 있고 어떤 열쇠(권한)가 필요한지가 정해져요.</p>
  <p><b>관측성(observability)</b>은 비행기의 <b>블랙박스</b>예요. 최종 결과만 보는 게 아니라 어떤 단계·도구·출력·에러가 있었는지 기록해서, 문제가 생기면 어디서 틀어졌는지 찾을 수 있게 해 줘요.</p>
</div>

> 레이블(label)은 특정 작업을 위한 **정답 표시**예요. 데이터의 깔끔함·완전성·형식은 **별개의** 품질 기준입니다.

### API는 "보이지 않는 AI"를 만드는 통로

API는 한 소프트웨어가 다른 소프트웨어와 대화하는 방법이에요. 앱이 **요청**(정보·지시·파라미터)을 보내면 AI 서비스가 **응답**을 돌려주고, 앱은 그걸 보여 주거나 저장하거나 다음 시스템으로 넘겨요. 이때 사용자는 AI가 쓰이는지조차 모를 수 있어요.

<ol class="oa-flow">
  <li><strong>티켓 생성</strong>지원 시스템이 정보 수집</li>
  <li><strong>요청</strong>티켓·계정·지식 베이스를 AI 백엔드로</li>
  <li><strong>응답</strong>답변 초안·이슈 분류·다음 행동 제안</li>
  <li><strong>사람 검토</strong>상담원이 확인 후 고객에게 발송</li>
</ol>

판단 질문 하나: **"사용자가 AI와 직접 일하나, 아니면 다른 시스템이 더 큰 워크플로의 일부로 AI를 쓰나?"** → ChatGPT, API, 또는 둘의 조합 중 어디서 시작할지 힌트가 돼요.

### 컨텍스트가 품질을 결정한다

아무리 똑똑한 모델도 **틀린 정보**로 일하면 그럴듯하지만 틀린 답을 내요. 최신 HR 정책을 못 보는 HR 어시스턴트처럼요. 그래서 네 가지를 물어요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📍</span><span class="oa-card__title">출처</span>정보는 어디서 오나?</div>
  <div class="oa-card"><span class="oa-card__icon">🔄</span><span class="oa-card__title">최신성</span>어떻게 최신으로 유지하나?</div>
  <div class="oa-card"><span class="oa-card__icon">🔑</span><span class="oa-card__title">접근</span>누가 볼 수 있나?</div>
  <div class="oa-card"><span class="oa-card__icon">✅</span><span class="oa-card__title">검증</span>출력은 어떻게 검토하나?</div>
</div>

목표는 "AI에게 정보를 더 많이"가 아니라 **맞는 정보를, 맞는 출처에서, 맞는 사람에게**예요.

### Eval과 가드레일은 처음부터

<div class="oa-eli">
  <span class="oa-eli__head">🚗 쉽게 말하면</span>
  <p>한 번 잘 달린 시제품 차와 판매 가능한 차의 차이는 <b>시험 기준을 미리 정했느냐</b>예요. <b>Eval</b>은 "정해진 코스를 몇 번이나 실수 없이 달리나" 시험, <b>가드레일</b>은 도로 옆 난간과 속도 제한이에요.</p>
</div>

| | Eval | Guardrail |
|---|---|---|
| 정의 | AI 솔루션이 기대대로 동작하는지 구조적으로 테스트 | 시스템이 할 일·하면 안 될 일·출력 형태·거절/에스컬레이션 시점을 정하는 통제 |
| 예 | 완전성, 형식 준수, 올바른 출처 사용, 근거 없는 주장 회피, 엣지 케이스 | 데이터 접근, 도구 사용, 출력 형식, 승인 규칙, 정책 경계, 사람 감독 |

Eval은 워크플로를 대표하는 **레이블된 예시**로 시작해요. 학습·파인튜닝 결정은 별도의 데이터 전략과 전문가 검증이 필요하고요. Eval·가드레일은 어떤 경로가 믿을 만한지, 안전하게 테스트 가능한지, 파일럿 너머로 확장 가능한지에 **처음부터** 영향을 줘요.

## 4. 판단하고, 검증하고, 에스컬레이션하기

### 추천과 검증을 분리 — 확신의 3단계

<div class="oa-bars">
  <div class="oa-bar"><span>Level 1 예비 경로 추천</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:33%"></span><span class="oa-bar__val">지금 아는 것 기준의 출발 방향</span></span></div>
  <div class="oa-bar"><span>Level 2 검증된 아키텍처</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:66%"></span><span class="oa-bar__val">핵심 가정을 검증함</span></span></div>
  <div class="oa-bar"><span>Level 3 배포 준비 계획</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:100%"></span><span class="oa-bar__val">요구사항·승인·연동·운영까지 정의</span></span></div>
  <p class="oa-bars__caption">막대 길이는 확신 수준을 보여 주는 개념적 표현이에요.</p>
</div>

초기 고객 대화의 초점은 **Level 1**이에요. 방향을 주되, 왜 그런지와 **무엇을 아직 검증해야 하는지**를 함께 말해요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 과장</span>
    <ul><li>"이게 확실히 맞는 솔루션입니다."</li><li>"ChatGPT 쓰세요." / "에이전트 만드세요."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ Level 1</span>
    <ul><li>"직원용 지식 워크플로라서 ChatGPT가 합리적인 출발점으로 보여요. 확정 전에 정보가 어디 있는지, 지금 어떻게 접근하는지, 보안·승인 요건이 있는지 확인하고 싶어요."</li></ul>
  </div>
</div>

### 경로를 바꾸는 트레이드오프

| 경험 쪽 트레이드오프 | 운영 책임 쪽 트레이드오프 |
|---|---|
| **속도 vs 통제** — 얼마나 빨리? 얼마나 맞춤·거버넌스? | **데이터 접근·보안** — 무슨 데이터, 누가, 어떤 보안 요건? |
| **품질 vs 비용/응답성** — 정확도·추론 깊이 vs 비용·지연(latency) | **거버넌스·신뢰성** — 일관성·정책 준수·감사 가능? |
| **자동화 vs 감독** — AI 단독 vs 사람 승인 | **유지보수·소유** — 출시 후 누가 관리? |
| **단순함 vs 연동 깊이** — 기존 화면으로 충분? 깊은 연결 필요? | **구현 성숙도** — 환경·소유 모델·운영 준비가 됐나? |

### 전문가를 부를 때

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🗂️</span><span class="oa-card__title">데이터·컴플라이언스</span>민감·규제 데이터, 데이터 레지던시, 불명확한 소유권</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⚡</span><span class="oa-card__title">시스템·행동</span>운영 시스템 접근, 레코드 수정, 고객 대상 발송, 중요한 의사결정</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔐</span><span class="oa-card__title">보안·권한</span>신원 관리, 접근 통제, 승인 규칙, 감사, 도구 경계</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🏗️</span><span class="oa-card__title">아키텍처·연동</span>복잡한 연동, 검색·grounding, 커스텀 앱, 에이전트 워크플로</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">📜</span><span class="oa-card__title">제품·정책 질문</span>제품 동작·가용성·로드맵·보안·프라이버시 등 공식 확인이 필요한 것</div>
</div>

### 예시 — "운영팀용 AI 어시스턴트"가 구체적인 경로가 되기까지

<ol class="oa-flow">
  <li><strong>막연한 요청</strong>"운영팀용 AI 어시스턴트"</li>
  <li><strong>질문</strong>어떤 업무? 누가? 어떤 정보? 답만? 행동도? 승인?</li>
  <li><strong>명확해진 요구</strong>승인된 파일로 주간 예외 리뷰 브리프, 레코드 수정 없음, 관리자 검토</li>
  <li><strong>Level 1 추천</strong>ChatGPT Work + 파일 종합, 관리자 확인·샘플 검증</li>
</ol>

추천 문장의 포인트: surface(ChatGPT Work), 역량(제공 파일 종합 — 운영 레코드를 바꾸는 연동 아님), 검증할 것(관리자가 가용성·파일 접근 확인, 운영 리드가 샘플 브리프를 출처와 대조), 책임자(관리자 검토), 그리고 **재평가 조건**(운영 플랫폼 안에서 쓰거나 레코드 수정이 필요해지면 솔루션 아키텍트와 API 경로 재검토).

## 5. 나만의 Technical Fluency Navigator

고객 대화·솔루션 리뷰·인수인계 전에 펼쳐 보는 **1페이지 메모**예요. 최종 추천서나 아키텍처 문서가 아니에요.

| 섹션 | 채울 내용 |
|---|---|
| 1. 내 역할과 대화 초점 | "내 역할은 ___할 때 기술 유창성이 가장 필요하다" |
| 2. 3축 라우팅 단서 | 축마다 예비 메모 1줄 (surface · 역량 · 배포/운영 질문) |
| 3. 다시 쓸 발견 질문 | 워크플로/결과 1개 + 축별 1개씩 |
| 4. 트레이드오프·에스컬레이션 신호 | 기억할 트레이드오프 2개, 전문가를 부를 상황 2개 |

역할별로 쓰임이 달라요: 기술 프리세일즈는 경로 설명·적합성 신호, 솔루션 컨설턴트는 워크플로를 발견 질문으로 번역, 솔루션 아키텍트는 제약·가정·리스크, 구현 담당자는 준비도·인수인계, 개발자/FDE형 빌더는 플랫폼 역량을 구현 방식으로 연결.

<div class="oa-eli">
  <span class="oa-eli__head">📝 Navigator 마무리 한 줄</span>
  <p>"지금까지 파악한 바로는 surface는 ___, 워크플로에는 ___가 필요할 수 있고, 추천으로 확정하기 전에 배포/운영 측면에서 ___를 검증해야 합니다."</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 짝 맞추기 — 고객 요구와 가장 강한 예비 라우팅 단서</summary>
  <p>고객용 제품 안의 AI 출력 → <b>API로 만든 앱/프로세스</b><br>검색·구조화 출력·도구·자동화 → <b>역량/아키텍처 결정</b><br>권한·승인·로깅·보존·사람 검토 → <b>배포/운영 결정</b><br>테스트·문서·PR 지원 → <b>Codex</b><br>사용자가 밀접히 참여하는 정책 Q&amp;A·초안 → <b>ChatGPT Chat</b><br>승인된 자료로 검토용 정책 브리프를 맡겨 만들기 → <b>ChatGPT Work</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 질문이 속한 레이어/축</summary>
  <p>"시스템이 무엇을 해야 하나?" → 역량/아키텍처 결정<br>"실패를 어떻게 모니터링하나?" → 거버넌스/관측성 레이어<br>"사람이나 워크플로가 주로 어디서 일하나?" → 사용자/제품 surface 결정<br>"어떤 레코드에 접근할 수 있나?" → 컨텍스트/데이터 레이어<br>"어디서, 어떻게 실행·통제되나?" → 배포/운영 결정<br>"티켓을 수정할 수 있나?" → 런타임/도구 레이어<br>"누가 어시스턴트를 쓰나?" → 사용자/워크플로 레이어</p>
</details>

<details class="oa-quiz">
  <summary>Q3. "AI가 협력사 리스크 정보를 검토하고 구매 레코드를 자동 업데이트하면 좋겠어요." 가장 강한 판단은?</summary>
  <p>① 여러 단계니까 바로 ChatGPT Work ② "에이전트"를 제품 경로로 보고 지금 자동화 설계 ③ surface·역량·배포/운영 결정을 분리하고, 데이터 접근·권한·검토·감사 가능성·에스컬레이션을 검증 ④ 데이터가 민감할 수 있으니 고객 운영 open-weight 모델</p>
  <p><b>정답: ③</b> — 레코드 쓰기(write-back)가 있으니 권한·감사·검토 검증이 핵심이에요. ④는 "민감하다"만으로 open-weight를 고르는 대표적 실수.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 고객이 "AI 자동화"를 원하지만 워크플로·데이터·사용자·승인 요건은 말하지 않았다. 다음 단계는?</summary>
  <p>① 어떤 모델을 원하는지 묻기 ② 워크플로를 먼저 명확히 하고, 세 축을 분리한 뒤 라우팅 ③ 자동화 = 에이전트이니 에이전트 추천 ④ 가장 빠른 ChatGPT 추천</p>
  <p><b>정답: ②</b></p>
</details>

## 정리

- 기술 라우팅은 **제품 목록이 아니라 워크플로**에서 시작한다.
- **세 결정을 분리**: 어디서 일하나(surface) · 무엇을 하나(역량/아키텍처) · 어떻게 실행·통제하나(배포/운영). 조합된 경로는 **검증할 가설**이다.
- 4개 발견 레이어(사용자 · 컨텍스트/데이터 · 런타임/도구 · 거버넌스/관측성)로 증거를 모은 뒤 축을 결정한다.
- 컨텍스트가 품질을, eval·가드레일이 신뢰를 결정한다 — 둘 다 **처음부터**.
- 초기 대화는 **Level 1**: 방향 + 이유 + 아직 검증할 것. 데이터·행동·보안·연동·정책 질문은 전문가에게.
