---
title: "Platform Capabilities and Developer Tools"
title_ko: "플랫폼 역량과 개발자 도구 — 기능 목록이 아니라 역할(role)로 고르기"
course: openai-technical-practitioner
lesson: 4
---

## 학습 목표

- 플랫폼 역량을 **상호작용·컨텍스트·행동·조율·평가·개발자 워크플로·거버넌스** 같은 역할로 구분하기
- API 인터페이스, **런타임 소유(누가 조율 소프트웨어를 돌리나)**, 출력 계약, 연동 경계 구분하기
- 검색·도구·에이전트·eval·개발자 도구가 **언제 워크플로에 맞는지** 판단하기
- 각 역량이 가져오는 **의존성·통제·트레이드오프·에스컬레이션 신호** 짚기
- 첫 버전에 들어갈 **최소 책임 역량 세트(minimum viable capability set)** 고르기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>역할로 생각하기</strong>기능 이름 대신 "이 역량이 맡는 일"</li>
  <li><strong>상호작용·컨텍스트</strong>무엇을 보내고, 무엇을 받고, 어떤 근거로?</li>
  <li><strong>도구·MCP·경계</strong>읽기 전용 vs 행동 — 통제가 달라진다</li>
  <li><strong>조건부 역량</strong>Realtime·멀티모달·에이전트·Codex는 필요할 때만</li>
  <li><strong>최소 책임 세트</strong>4단 결정 구조 + 경계를 밝히는 말투</li>
</ol>

이 코스 내내 하나의 예시가 따라다녀요: **영업팀용 "어카운트 브리프(account brief)" 어시스턴트** — 고객사 미팅 전에 승인된 메모·통화 요약으로 브리프를 만들어 주는 도구예요.

## 1. 기능 목록이 아니라 역량의 역할로

<div class="oa-eli">
  <span class="oa-eli__head">🧱 쉽게 말하면</span>
  <p>레고로 집을 지을 때 "빨간 블록 주세요"부터 말하지 않죠. "문이 필요해, 창문이 필요해"가 먼저고, 그다음에 맞는 블록을 찾아요. <b>플랫폼 역량 = 재사용 가능한 블록</b>이고, 블록은 <b>집의 어느 부분을 맡는지</b>가 분명할 때 쓸모가 생겨요.</p>
</div>

고객은 종종 이렇게 말해요: "검색(retrieval)이 필요해요", "에이전트가 필요해요", "Codex 쓰고 싶어요". 하지만 **기능 이름만으로는 워크플로가 무엇을 요구하는지 설명이 안 돼요.** 기능부터 고르면 사용자 요구·원천 데이터·출력 형식·행동 범위·리스크를 확인하기도 전에 복잡도만 늘어나요 → 과잉 설계, 검증 어려움, 인수인계 어려움.

좋은 추천은 각 블록에 대해 네 가지를 말해요:

| 질문 | 예: 브리프 어시스턴트의 "구조화 출력" |
|---|---|
| 어떤 **역할**? | 모든 브리프를 같은 형식으로 |
| 왜 **지금** 필요? | 영업 사용자가 매번 같은 섹션을 기대 |
| 어떤 **의존성·통제·트레이드오프**? | 브리프 형식이 먼저 정의돼 있어야 함 |
| 추천 전 무엇을 **검증**? | 형식이 맞아도 내용(담당자 등)이 틀릴 수 있음 → 기록과 대조 |

### OpenAI 역량을 "하는 일"로 보면 (강의 기준)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">API 역량</span>제품·운영·고객 경험에 AI를 넣는 빌더/연동 경로</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">ChatGPT 경험</span>직원용 업무: Chat(함께 생각·작성), Work(위임형 지식 업무, 가능·활성화 시)</div>
  <div class="oa-card"><span class="oa-card__icon">💻</span><span class="oa-card__title">Codex·개발자 도구</span>소프트웨어 개발·엔지니어링 업무</div>
  <div class="oa-card"><span class="oa-card__icon">🤖</span><span class="oa-card__title">에이전트</span>답하기에서 "일하기"로 — 컨텍스트·도구·가드레일·승인과 함께</div>
</div>

### 7가지 역량 역할

| 역할 | 무엇을 하나 | 브리프 어시스턴트 예시 |
|---|---|---|
| **Interaction** | 사용자/시스템이 솔루션과 만나는 방식 | 사내 앱·워크스페이스에서 브리프 요청 |
| **Context** | 필요한 정보에 접근 | 승인된 어카운트 메모·기회 정보·통화 요약 |
| **Action** | 도구로 다른 시스템에 영향 | CRM 상태 확인, 후속 태스크 생성 |
| **Orchestration** | 여러 단계 조율 | 요청 → 검색 → 누락 확인 → 초안 → 검토 |
| **Evaluation** | 품질·신뢰성 테스트 | 완전성·근거·형식·유용성 eval |
| **Developer workflow** | 만들고 테스트하고 유지 | Codex로 구현·테스트·코드 리뷰 |
| **Governance·Observability** | 통제·모니터링·개선 | 권한·승인 지점·로그·에스컬레이션 경로 |

### 누가 작업을 소유하고, 누가 결과를 보나 (강의 기준)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🟢</span><span class="oa-card__title">dot (개인 에이전트)</span>대화 사이에도 계속 일하고, 허용된 컨텍스트·연결 앱을 써서 <b>본인</b>에게 결과를 돌려줌</div>
  <div class="oa-card"><span class="oa-card__icon">👥</span><span class="oa-card__title">Team Task (팀 프로세스)</span>공유 지시로 일정·이벤트에 따라 실행. <b>팀 서비스 계정</b>과 설정된 연결로 동작 → 한 사람에게 의존하지 않음. 각 연결의 외부 계정이 접근 범위를 정함</div>
  <div class="oa-card"><span class="oa-card__icon">📄</span><span class="oa-card__title">Page (공유 산출물)</span>함께 보는 편집 가능 문서. 공유한다고 <b>최신 유지 프로세스</b>가 생기지는 않음. 복사해 넣은 정보는 원본 파일 권한과 별개로 독자에게 보임</div>
</div>

> 브리프라면: **누가 프로세스를 유지하나 · 어느 계정이 원천에 접근하나 · 누가 완성본을 읽어도 되나**를 따로 확인해요.

### 최소 책임 역량 세트

<div class="oa-eli">
  <span class="oa-eli__head">🎒 쉽게 말하면</span>
  <p>당일치기 등산에 텐트·버너·침낭까지 다 챙기면 무거워서 오히려 위험해요. 그렇다고 물과 비옷을 빼면 안 되죠. "가장 작은 데모"가 아니라 <b>안전하게 다녀올 수 있는 가장 가벼운 짐</b>이 최소 책임 세트예요.</p>
</div>

**사용자 요구를 채우기에 충분한 역량 + 리스크를 다룰 충분한 통제 + 다음 결정을 뒷받침할 충분한 증거.** 이걸로 "지금 필요 / 나중에 추가 / 리스크·비용·운영 부담 / 데이터·권한·시스템 준비에 의존 / 전문가 검증 필요"를 나눠요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 브리프 첫 버전에 필요할 것</span>
    <ul>
      <li>상호작용 계층(요청 surface)</li>
      <li>승인된 어카운트 메모 검색</li>
      <li>구조화 출력(일정한 브리프 형식)</li>
      <li>eval + 사람 검토</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⏸️ 워크플로가 요구하지 않으면 빼기</span>
    <ul>
      <li>Realtime 음성, 멀티모달 입력</li>
      <li>MCP 스타일 연결</li>
      <li>Codex, 에이전트 도구</li>
    </ul>
  </div>
</div>

각 블록엔 의존성이 있어요: 검색 ← 승인·최신·접근 가능한 원천 / 구조화 출력 ← 정의된 형식 / 도구 ← 시스템 접근·권한·실패 처리·소유자 / eval ← 성공 기준·대표 케이스 / 사람 검토 ← 사용처와 리스크.

## 2. 상호작용과 컨텍스트

### 상호작용 계층 = 워크플로의 "인수인계 지점"

<div class="oa-eli">
  <span class="oa-eli__head">🍔 쉽게 말하면</span>
  <p>햄버거 가게 주문 창구를 떠올려요. 손님이 주문(요청)하고, 점원이 레시피(지시)대로 재료(입력·컨텍스트)를 넣어 포장(출력 형식)해서 건네면, 손님이 확인(검토)해요. 창구가 어떻게 생겼냐보다 <b>무엇을 주고받는지</b>가 핵심이에요.</p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 150" role="img" aria-label="요청, 지시, 입력, 컨텍스트, 출력, 검토 또는 행동으로 이어지는 상호작용 계층 흐름">
  <defs>
    <marker id="ar4a" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.b4{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.g4{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.t4{font-size:12px;fill:#4a4a4a}.s4{font-size:10px;fill:#8a8a8a}</style>
  </defs>
  <rect x="5" y="30" width="100" height="70" rx="10" class="b4"/>
  <text x="55" y="60" text-anchor="middle" class="t4">요청</text>
  <text x="55" y="80" text-anchor="middle" class="s4">어카운트 ID</text>
  <line x1="107" y1="65" x2="121" y2="65" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar4a)"/>
  <rect x="125" y="30" width="100" height="70" rx="10" class="b4"/>
  <text x="175" y="60" text-anchor="middle" class="t4">지시</text>
  <text x="175" y="80" text-anchor="middle" class="s4">브리프 작성 규칙</text>
  <line x1="227" y1="65" x2="241" y2="65" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar4a)"/>
  <rect x="245" y="30" width="100" height="70" rx="10" class="b4"/>
  <text x="295" y="60" text-anchor="middle" class="t4">입력</text>
  <text x="295" y="80" text-anchor="middle" class="s4">사용자 메모</text>
  <line x1="347" y1="65" x2="361" y2="65" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar4a)"/>
  <rect x="365" y="30" width="100" height="70" rx="10" class="b4"/>
  <text x="415" y="60" text-anchor="middle" class="t4">컨텍스트</text>
  <text x="415" y="80" text-anchor="middle" class="s4">승인된 원천</text>
  <line x1="467" y1="65" x2="481" y2="65" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar4a)"/>
  <rect x="485" y="30" width="100" height="70" rx="10" class="g4"/>
  <text x="535" y="60" text-anchor="middle" class="t4">출력</text>
  <text x="535" y="80" text-anchor="middle" class="s4">정해진 형식</text>
  <line x1="587" y1="65" x2="601" y2="65" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar4a)"/>
  <rect x="605" y="30" width="110" height="70" rx="10" class="g4"/>
  <text x="660" y="60" text-anchor="middle" class="t4">검토 / 행동</text>
  <text x="660" y="80" text-anchor="middle" class="s4">영업 사용자 확인</text>
  <text x="360" y="135" text-anchor="middle" class="s4">출력이 검토·저장·도구 호출·다음 단계 중 무엇을 일으키는지까지 정해야 상호작용 계층이 완성돼요</text>
</svg>
</div>

- **API 인터페이스**(소프트웨어가 AI에 요청·결과를 주고받는 방식) ≠ **사용자 인터페이스**(사람이 보는 채팅·패널·앱).
- 새 프로젝트엔 Chat Completions보다 **Responses**를 권장(강의 기준). 단, 이건 **누가 조율 소프트웨어를 돌리는지**, 결과를 어떻게 표시·저장·검토할지는 정해 주지 않아요.

### 런타임 소유 — 누가 조율하나 (강의 기준)

| 방식 | 누가 조율 소프트웨어를 돌리나 | 메모 |
|---|---|---|
| **애플리케이션 소유** (Responses API, 선택적으로 Agents SDK) | 팀의 앱 | 요청·도구 실행·상태·승인·복구를 팀이 결정. Responses가 호스팅 도구·턴 간 정보 제공 가능 |
| **관리형** (Agents API, public beta) | OpenAI 관리 하네스 | 세션 진행 유지, 컨텍스트 압축, 끊김 후 복구. 그래도 권한·검토·eval은 팀이 정의 |
| **ChatGPT 경험** (관리자 활성화) | ChatGPT | 커스텀 빌드 없이 충분할 수도 |

<div class="oa-eli">
  <span class="oa-eli__head">📮 "요청했다" ≠ "실행됐다"</span>
  <p>친구에게 "편지 부쳐 줘"라고 부탁했다고 편지가 도착한 건 아니죠. 모델이 CRM 조회를 <b>요청</b>해도, 앱이 요청과 권한을 검증하고, 워커가 실제로 조회하고, 결과를 원래 호출에 돌려줘야 해요. <b>실행·추적·확인은 둘러싼 애플리케이션의 책임</b>이에요.</p>
</div>

- **Async tool calling**: 앱이 돌리는 도구가 진행 중일 때 모델이 독립적인 일을 계속(지원 시). 백그라운드 응답 생성과는 다름.
- **Mid-turn steering**(Responses WebSocket): 응답 도중 요구를 추가·변경. 이미 시작된 행동·도구 호출을 **되돌리지 않음**.
- 커스텀 API 연동 신호: 커스텀 앱 경험, 구조화 출력, 도구 사용, 다단계, 검색, 시스템 간 워크플로, API 수준 통제.

### 출력은 "워크플로에서 무엇이 되어야 하나"부터

"모델이 뭘 생성해야 하지?"가 아니라 **"이 출력이 워크플로에서 무엇이 되어야 하지?"**를 물어요.

| 필요 | 방식 (강의 기준) | 주의 |
|---|---|---|
| 읽을 수 있는 브리프 | 생성된 텍스트 | 전체 브리프는 여전히 생성 필요 |
| 담당자·다음 단계 같은 필드 | **Structured Outputs** + JSON Schema | 형식이 맞아도 담당자가 틀릴 수 있음 → 기록과 대조 |
| 먼저 레코드 조회 | **Function calling** | 앱이 검증·인가 후 실행. 요청 ≠ 완료 |
| "전문가 검토 필요?" | **Decisions API** predicate (GPT-6 Luna) | 조건이 참일 추정 확률 |
| "어느 검토 큐로?" | Decisions choice | 팀이 준 카테고리 중 하나 |
| "어카운트 정보가 얼마나 완전?" | Decisions score | 정의된 단계 기준 점수(사이값 가능) |
| 차트·계산기 등 대화형 답 | **Intelligent UI** (Chat 응답 내) | Work·Voice 미지원. 검토된 브리프 보관처(Page)는 따로 필요 |

> 카테고리나 점수는 검토를 **안내**할 뿐, 행동을 하거나 허가를 주지 않아요. 그리고 **브리프를 만들면 안 되는 경우**도 정해요: 차단·실패·부분·취소된 실행엔 안전한 상태 메시지를 보여 주고, 증거는 따로 남겨요. 영업용 브리프(요약·출처·리스크·다음 단계)와 운영자용 실행 기록(호출·결과 ID, 검증 실패, 승인, 상태)은 **분리**해요.

### 컨텍스트 계층 — "검색 넣을까?"가 아니라

<div class="oa-eli">
  <span class="oa-eli__head">📚 쉽게 말하면</span>
  <p>숙제할 때 선생님이 프린트를 직접 주면 도서관에 갈 필요가 없어요. 하지만 매주 다른 주제를 조사해야 하면 도서관(검색)이 필요하죠. 그때도 <b>책이 최신인지, 내가 그 서가에 들어갈 수 있는지</b>부터 확인해야 해요.</p>
</div>

핵심 질문: **"이 워크플로는 어떤 승인된 컨텍스트가 필요하고, 그 컨텍스트는 믿을 만한 출력을 뒷받침할 준비가 됐나?"**

<ol class="oa-flow">
  <li><strong>직접 제공</strong>사용자가 메모 몇 개를 붙여 넣는 일회성 초안</li>
  <li><strong>검색</strong>정해진 원천에서 메모·기회·통화 요약을 가져오는 반복형</li>
  <li><strong>구조화 지식 + 권한</strong>사용자마다 접근 권한이 다르면 권한 검증 먼저</li>
</ol>

**검색 준비도 = 콘텐츠 준비 + 접근 준비.** 확인할 것: 진실의 원천(source of truth)은? 누가 관리? 최신·완전한가? 사용자 그룹별 권한 차이? 인용 필요? 누락·충돌 시 처리? 검색 품질은 어떻게 테스트?

- 회의 메모를 매번 직접 주는 프로젝트 업데이트 앱 → 검색 불필요할 수 있음
- 사내 IT 정책 Q&A → 승인된 정책 원천 검색 + 최신성 확인 + 접근 검증 + 충돌 검토 프로세스

## 3. 도구, MCP, 그리고 경계

### 읽기 전용 vs 행동하는 도구

<div class="oa-eli">
  <span class="oa-eli__head">🏦 쉽게 말하면</span>
  <p>은행 앱에서 <b>잔액 조회</b>와 <b>송금</b>은 같은 앱 안이지만 보안 절차가 완전히 다르죠. 송금엔 비밀번호 재확인·한도·알림이 붙어요. AI 도구도 같아요 — <b>같은 CRM이어도 읽기와 쓰기는 다른 경계</b>예요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">👀 읽기 전용 (lookup)</span>
    <ul>
      <li>조회·확인·계산 — 기록을 바꾸지 않음</li>
      <li>예: CRM 기회 상태 조회, 파이프라인 계산</li>
      <li>그래도 접근 권한·원천 검증·로깅·검색 품질 테스트 필요</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">✍️ 행동 (action-taking)</span>
    <ul>
      <li>생성·수정·삭제·발송·승인·예약·트리거</li>
      <li>예: CRM 필드 수정, 후속 태스크 생성, 이메일 발송</li>
      <li>승인 게이트·제한된 행동·추적성·실패 처리·소유자·테스트</li>
    </ul>
  </div>
</div>

<div class="oa-bars">
  <div class="oa-bar"><span>답변만 생성</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:25%"></span><span class="oa-bar__val">출력 검토</span></span></div>
  <div class="oa-bar"><span>읽기 전용 도구</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:50%"></span><span class="oa-bar__val">+ 접근 권한·원천 검증·로깅</span></span></div>
  <div class="oa-bar"><span>행동 도구</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:95%"></span><span class="oa-bar__val">+ 승인·제한·실패 처리·소유·테스트·에스컬레이션</span></span></div>
  <p class="oa-bars__caption">개념적 표현 — 조회에서 행동으로 갈수록 필요한 통제가 쌓여요.</p>
</div>

**직접(direct) vs 프로그램(programmatic) 도구 호출** (강의 기준): 모델이 도구를 고르고 결과를 보고 다음을 판단해야 하면 직접 호출. 대량 조회·조인·집계·변환처럼 **예측 가능·한정된 처리**에서 모델이 중간 결과를 다 볼 필요 없을 때만 프로그램 호출.

### 도구 경계 질문 체크리스트

- 어떤 도구가 필요? 그중 읽기 전용은? 행동은?
- 허용/제한되는 행동은? 사람 승인이 필요한 건?
- 호출이 실패하면? 무엇을 로깅? 무엇을 테스트? 무엇을 에스컬레이션?
- 누가 연동을 소유·유지? **어떤 사용자/서비스 계정이 실행**하고, **누가 출력을 보며**, 어떤 읽기·쓰기가 인가됐나?

앱이 CRM 조회를 돌릴 땐: 완전한 도구 요청을 기다림 → 인자 검증 → 호출자 권한 확인 → 상태 기록(대기·완료·실패·차단) → 결과를 호출에 매칭. **조회 실패 ≠ 레코드 없음.** 결과에 의존하지 않는 일은 계속하되, 결과나 미승인 승인에 의존하는 단계는 기다려요.

<div class="oa-eli">
  <span class="oa-eli__head">🎫 로그인 ≠ 권한 (강의 기준)</span>
  <p><b>Sign in with ChatGPT</b>는 신원 확인이에요. 참여 앱에서 Go·Plus·Pro 사용자가 Work/Codex 플랜 사용량을 쓰도록 선택할 수 있지만(API 키 없이), 채팅·메모리를 공유하지 않고 API 과금과도 별개예요. 그리고 <b>로그인·플랜 사용 허용이 CRM 조회·수정을 인가하지 않아요.</b> CRM 연결엔 자체 권한이 필요해요. <b>OpenAI Marketplace</b>는 커밋 일부를 적격 파트너 제품에 쓰는 구매 방식일 뿐, 실행 소프트웨어나 보안 승인이 아니에요.</p>
</div>

### MCP 스타일 연결

<div class="oa-eli">
  <span class="oa-eli__head">🔌 쉽게 말하면</span>
  <p>USB-C처럼 <b>공통 규격 콘센트</b>예요. 한 번 만든 MCP 서버를 여러 호환 AI 앱에 꽂을 수 있어서 커스텀 연동이 줄어요. 하지만 콘센트가 맞는다고 아무 기기나 꽂아도 되는 건 아니죠 — <b>어떤 데이터·행동을 허용할지는 별도 결정</b>이에요.</p>
</div>

| 구성 요소 (강의 기준) | 하는 일 |
|---|---|
| **Plugin 확장** | 사용자용 인터페이스(대화 옆 패널, 파일 뷰어) |
| **MCP tools** | 인가된 연결로 에이전트가 호출하는 작업(예: 어카운트 레코드 읽기) |
| **MCP Events** | 구독한 변화가 생기면 연결된 서비스가 ChatGPT에 알림 → 사용자의 지시대로 작업. 이벤트는 입력, 지시가 할 일을 정함 |
| **ChatGPT Sites — 방문자 연결** | 방문자가 자기 계정으로 읽기 전용 접근 인가. 제작자 연결 상속 X, 방문 중에만, 예약 작업 X |
| **ChatGPT Sites — Site 호스팅 plugin** | Site가 자기 데이터 읽기/변경 도구를 ChatGPT·Codex에 노출. Site 공유 ≠ plugin 공유 |

- 패널도 이벤트도 **CRM 수정 권한을 주지 않아요.** 브리프 파일럿은 연결 원천을 읽기 전용으로, 쓰기는 별도 승인된 경로로.
- 제3자 MCP 서버로 보낸 데이터는 **그 서비스의 보존·거주 정책**을 따르고, OpenAI API 설정이 자동 적용되지 않아요.
- MCP를 검토할 신호: 여러 도구·시스템, 재사용 연결, 에이전트 워크플로, 플랫폼 팀 참여, 도구 접근 거버넌스. CRM 필드 하나 확인이면 단순 도구 호출로 충분.

### 실제 예: 구매(procurement) 어시스턴트

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">읽기 전용 버전</span>
    <ul><li>업체 승인 여부·계약 활성 여부·정책 메모를 조회해 요약 → 검토</li></ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">행동 버전</span>
    <ul><li>티켓 생성·업체 레코드 수정·승인 요청 발송 → 승인 게이트·제한·로깅·소유·테스트·실패 처리·에스컬레이션</li></ul>
  </div>
</div>

중요한 건 업체 시스템 자체가 아니라 **조회만 하느냐, 바꿀 수 있느냐**예요.

## 4. 조건부 역량 — 정당할 때만

### 상호작용 모양(interaction shape)

<div class="oa-eli">
  <span class="oa-eli__head">✉️ 쉽게 말하면</span>
  <p>생일 축하는 문자로 해도 충분하지만, 길을 잃은 친구에겐 <b>지금 전화</b>가 필요해요. 브리프는 미팅 전에 만들어 검토하면 되니 "문자형(비동기)"이면 충분하고, 실시간 고객 상담은 "전화형(Realtime)"이 필요할 수 있어요.</p>
</div>

모양: 비동기 처리 · 채팅 · 제품 내장 · 음성 대화 · 실시간 지원 · 이미지/문서 이해 · 멀티모달 입출력. **워크플로를 지탱하는 가장 단순한 모양**을 고르세요 — 단순할수록 검증·거버넌스·운영이 쉬워요.

Realtime·멀티모달 검증 질문: 사용자가 주는/받을 모달리티는? 얼마나 빨라야? 지연을 견디나? 어떤 데이터가 캡처·저장·검토되나? 동의·프라이버시·접근성? **잘못 듣거나 읽거나 확신이 낮을 때**는? 품질은 어떻게 평가?

### 에이전트 도구가 정당할 때

**Agents SDK** = 앱 안에서 에이전트 루프·도구 호출·핸드오프·가드레일·트레이싱을 돌리는 개발 키트(강의 기준).

| | 애플리케이션 소유 (Agents SDK) | 관리형 (Agents API, public beta) |
|---|---|---|
| 조율 소프트웨어 | 팀이 운영 | OpenAI 관리 하네스 |
| 상태·승인·복구 | 팀이 결정 | 세션 진행 유지, 컨텍스트 압축, 끊김 후 복구 |
| 팀이 여전히 할 일 | 도구·권한·검토·eval | 지시·승인된 도구·실행 위치·권한·검토·eval |
| 데이터 (강의 기준) | 팀 설계 | 세션 데이터 보존, 현재 **US-only 거주**, **ZDR 미지원**(셀프 호스팅 샌드박스여도) |

- 관리형은 OpenAI 호스팅 **브라우저**로 웹 페이지를 열고 조작할 수 있어요. 하지만 웹사이트 접근 승인이 **행동마다 확인**을 보장하진 않아요 → 읽기 전용이면 바꿀 수 없는 리소스로 제한, 행동별 승인이 필수면 팀이 통제하는 브라우저 런타임 사용.
- 끊김 후엔 저장된 활동으로 완료 단계를 확인한 뒤 반복 여부를 판단하고, 필요 없어진 세션은 삭제.
- 어느 쪽도 ChatGPT Work의 환경을 상속하지 않아요.

에이전트가 맞는 신호: 다단계 조율, 시스템 간 도구 사용, 워크플로 상태, 사람 승인 지점, 재사용 지시, 핸드오프, 테스트·트레이싱, 운영 모니터링. 단일 답변·조회면 단순한 API 상호작용으로 충분해요.

### Eval과 관측성도 선택 기준이다

| 역량 | 생기는 질문 |
|---|---|
| 검색 | 원천 관련성·근거성 |
| 구조화 출력 | 형식 준수 |
| 도구 | 정확성·권한·실패 처리·승인 동작 |
| 에이전트 | 단계 순서·도구 사용·핸드오프·트레이싱·모니터링 |

- **Eval** = 충분히 잘 되는지 테스트(출력 품질, 형식, 검색 품질, 도구 정확성, 에스컬레이션, 안전, 변경 후 회귀). 브리프가 "잘 읽히는지"만이 아니라 **전체 워크플로**를, 그리고 **잘 안 될 때**(조회 도구 불가, 결과 매칭 실패, 중간 중단)도 테스트.
- **관측성** = 무슨 일이 있었나(실행 단계, 사용 도구, 포함된 원천, 오류, 요청·허가된 승인, 실패 위치).
- 관리형·ChatGPT 호스팅 워크플로에선 볼 수 있는 실행 기록과 중지 수단이 서비스마다 달라요. 모니터링 경보는 틀리거나 놓칠 수 있고, **중지해도 이미 끝난 행동은 되돌려지지 않아요.**

### Codex·개발자 도구가 맞을 때 (강의 기준)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">☁️</span><span class="oa-card__title">Codex Cloud</span>저장소·의존성·도구·승인된 접근을 담은 환경을 준비·게시 → 새 태스크는 각자 분리된 작업공간에서 실행. 로컬 PC가 꺼져도 계속</div>
  <div class="oa-card"><span class="oa-card__icon">🔍</span><span class="oa-card__title">Code Review</span>변경 검토를 돕지만, 피드백 게시·승인·머지는 별개 행동. 현재 Codex Cloud (Legacy) 사용</div>
  <div class="oa-card"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">Codex Security</span>연구 프리뷰. 연결된 GitHub 저장소 스캔·잠재 취약점 조사·수정 제안 → 리뷰어가 판단. 일반 코드 리뷰 ≠ 보안 승인</div>
</div>

맞는 신호: 코드베이스 이해, 코드 변경, 테스트 생성·수리, 디버깅, PR 리뷰, 문서화, 릴리스·유지보수, 엔지니어링 태스크 위임. **패치 생성 ≠ 올바르고 수용된 변경.** 브리프 어시스턴트 자체엔 Codex가 필요 없고, 그 **소프트웨어를 만들고 유지할 때** 관련돼요.

## 5. 가장 작은 책임 있는 역량 세트 고르기

### 4단 역량 결정 구조

| 결정 영역 | 무엇을 밝히나 | 길잡이 질문 |
|---|---|---|
| **① 워크플로 요구·필요 역할** | 해야 할 일, 출력 사용자, 필요한 역할 | 솔루션이 어떤 일을 지원해야 하나? |
| **② 최소 책임 역량 세트** | 첫 버전 필수 vs 선택. surface·출력·런타임 소유자·이벤트 활성화, 미검증 항목 표시 | 지금 필요한 것과 미룰 것은? |
| **③ 의존성·통제·트레이드오프** | 승인 원천·접근 권한, 실행 신원·출력 독자, 허용/제한 행동, 승인·eval·로깅·소유, 지연·비용·품질·프라이버시, 연동 가용성·자금 조건. 로그인·사용량·도구 접근은 분리 | 각 역량이 무엇을 들여오고 무엇에 의존하나? |
| **④ 검증·에스컬레이션 질문** | 제품·보안·데이터·연동·구현·엔지니어링 담당자에게: 현재 제품 정보·가용성, 원천 준비, 권한, 보안 검토, 완료 증거 등 | 무엇을 검증·전문가 검토해야 하나? |

### 같은 브리프, 다른 배치

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">요청 시 만드는 초안</span>
    <ul>
      <li>사용자가 요청 → 승인된 메모로 초안 → 본인이 검토</li>
      <li>API 앱 또는 워크스페이스 surface로 충분</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">매주 월요일 유지되는 브리프</span>
    <ul>
      <li>담당자가 휴가여도 돌아가야 함 → <b>Team Task</b>(팀 서비스 계정) 같은 팀 소유 프로세스 검토</li>
      <li>검토 후 편집 가능한 공유 브리프 → Page. 하지만 <b>Page는 스스로 최신화되지 않고, 트리거는 행동을 인가하지 않아요</b></li>
    </ul>
  </div>
</div>

### 경계를 밝히는 말투

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 이렇게</span>
    <ul>
      <li>"설명해 주신 워크플로 기준으로, 검증할 첫 버전 역량 세트예요. 최종 구현 설계는 아니에요."</li>
      <li>"유력한 블록과 열린 검증 질문을 정리했어요. 고객 대상 사용 전엔 제품·보안·데이터·연동·거버넌스·구현 쪽 전문가 검토가 필요할 수 있어요."</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 이렇게 포장하지 않기</span>
    <ul>
      <li>확정 아키텍처, 운영 구현 계획, 보안·컴플라이언스 승인</li>
      <li>제품 가용성 약속, 가격·상업 추천</li>
      <li>상세 API·SDK·MCP·Codex·eval·관측성 설계, 고객용 구현 명세</li>
    </ul>
  </div>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 사내 어카운트 브리프 어시스턴트 첫 버전에 가장 강한 역량 판단은?</summary>
  <p>① 영업 계획엔 여러 단계가 있으니 에이전트 패턴부터 ② <b>워크플로 역할부터: 사용자 요청, 승인된 컨텍스트, 일정한 출력, 검토, 검증</b> ③ 브리프 생성 즉시 CRM 쓰기부터 ④ 나중에 재작업 없게 모든 주요 역량부터</p>
  <p><b>정답: ②</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. 승인된 메모로 일정한 형식·출처 포함 브리프를 만들고, 사용자마다 문서 접근 권한이 다르다. 가장 강한 추천은?</summary>
  <p>① 검색만, 권한은 출시 후 ② 나중에 소프트웨어 변경이 필요할 수 있으니 Codex ③ 말하는 동안 생성되게 Realtime ④ <b>구조화 출력 + 승인된 검색 + 접근 검증 + 검토/eval</b></p>
  <p><b>정답: ④</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. CRM 기회 상태를 조회하고 예측(forecast) 필드를 업데이트해야 한다. 가장 강한 진술은?</summary>
  <p>① 연결 시스템은 늘 위험하니 도구 사용 회피 ② 같은 CRM이니 둘 다 읽기 전용으로 ③ 외부 시스템이니 자동으로 MCP ④ <b>경계를 나눈다: 상태는 읽고, 쓰기 전엔 승인 필수</b></p>
  <p><b>정답: ④</b></p>
</details>

<details class="oa-quiz">
  <summary>Q4. 음성·이미지·코드·자율 업데이트가 필요 없는 첫 버전 브리프 어시스턴트. 과잉 설계를 가장 잘 피하는 선택은?</summary>
  <p>① 미래 대비로 Realtime·멀티모달·Codex·에이전트 지금 추가 ② 나중에 원할 수 있으니 CRM 쓰기 지금 추가 ③ 언젠가 구현이 필요하니 Codex ④ <b>승인된 컨텍스트·구조화 출력·검토·eval·기본 관측성으로 시작, CRM 조회는 따로 검증</b></p>
  <p><b>정답: ④</b></p>
</details>

<details class="oa-quiz">
  <summary>Q5. 짝 맞추기 — 항목이 속하는 결정 영역</summary>
  <p>워크플로 결과·사용자 그룹·필요한 역량 역할 → <b>워크플로 요구·필요 역할</b><br>첫 책임 버전에 필요한 역량 → <b>최소 책임 역량 세트</b><br>승인 원천·실행 신원·독자·도구 경계·행동 권한·승인 게이트·자금 조건 → <b>의존성·통제·트레이드오프</b><br>제품·보안·데이터·연동·구현 질문 → <b>검증·에스컬레이션 질문</b></p>
</details>

## 정리

- 역량은 **기능 이름이 아니라 워크플로에서 맡는 역할**로 고른다: 상호작용·컨텍스트·행동·조율·평가·개발자 워크플로·거버넌스.
- **API 인터페이스 ≠ UI ≠ 런타임 소유.** 모델의 도구 "요청"은 실행이 아니며, 실행·추적·확인은 앱(또는 관리형 서비스)의 책임.
- 출력은 "워크플로에서 무엇이 되나"로 설계: 브리프 / 구조화 객체 / 범위 결정. 영업용 결과와 운영 기록은 분리.
- 검색은 **콘텐츠 + 접근 준비**가 될 때. 도구는 **읽기 vs 행동**으로 경계를 나누고, 로그인·플랜 사용량·Marketplace는 권한이 아니다.
- Realtime·멀티모달·에이전트·Codex는 **조건부** — 워크플로가 요구할 때만. eval·관측성은 각 역량이 만든 질문에 답한다.
- **4단 결정 구조**로 최소 책임 역량 세트를 정리하고, 경계를 밝히는 말투로 넘긴다.
