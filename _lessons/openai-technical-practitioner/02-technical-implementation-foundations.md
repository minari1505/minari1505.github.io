---
title: "Technical Implementation Foundations"
title_ko: "기술 구현 기초 — '좋은 아이디어'를 '구현 준비 완료'로 바꾸는 법"
course: openai-technical-practitioner
lesson: 2
---

## 학습 목표

- OpenAI 기반 솔루션의 **구현 라이프사이클**을 설명하기
- **Responses API**(인터페이스 선택)와 **누가 agent runtime을 운영하나**(런타임 선택)를 구분하기
- 요청-응답 사고로 입력·컨텍스트·도구·상태·출력 계약·trace를 짚어 내기
- 환경·접근·데이터·연동·**용량(capacity)** 준비도를 점검하기
- Eval과 safeguard가 왜 **구현 준비도의 일부**인지 설명하기
- 1장짜리 **Implementation Readiness Snapshot** 만들기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>구현 파트너처럼 생각</strong>워크플로가 충분히 명확한가?</li>
  <li><strong>API와 런타임</strong>무엇을 보내고, 무엇이 오고, 누가 단계를 조율?</li>
  <li><strong>환경·경계·소유</strong>어디서 돌고, 무엇에 접근하고, 누가 책임?</li>
  <li><strong>Eval·safeguard</strong>출시 자신감의 근거</li>
  <li><strong>Readiness Snapshot</strong>짧은 준비도 메모</li>
</ol>

핵심 한 문장: **구현은 코드를 쓰기 전에 시작된다.** "AI로 이거 돼요?"에서 곧장 "만들죠!"로 뛰지 말고, **"이걸 믿고 구현하려면 무엇이 참이어야 하나?"**로 바꿔 묻기.

## 1. 구현 파트너처럼 생각하기

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면</span>
  <p>"넓은 거실이 있는 집을 짓고 싶어요"는 좋은 <b>꿈</b>이에요. 하지만 건축가는 땅이 어디인지, 몇 명이 사는지, 예산은 얼마인지, 수도·전기는 어디서 끌어오는지, 다 지은 뒤 누가 관리하는지 알아야 <b>착공</b>할 수 있죠. 구현 준비도는 이 "착공 가능 상태"예요.</p>
</div>

"AI로 지원 티켓을 요약하고 싶어요"는 명확해 보이지만, 구현 파트너는 이런 걸 알아야 해요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">범위</span>어떤 티켓 유형? 일회성인가 반복 업무인가?</div>
  <div class="oa-card"><span class="oa-card__icon">👥</span><span class="oa-card__title">사용자</span>요약은 누가 읽나? 내부 리드? 고객?</div>
  <div class="oa-card"><span class="oa-card__icon">🗄️</span><span class="oa-card__title">데이터</span>티켓은 어디 있나? 절대 넣으면 안 되는 정보는?</div>
  <div class="oa-card"><span class="oa-card__icon">✍️</span><span class="oa-card__title">시스템 동작</span>티켓 플랫폼에 다시 써야(write-back) 하나?</div>
  <div class="oa-card"><span class="oa-card__icon">📏</span><span class="oa-card__title">품질</span>"충분히 정확한 요약"의 기준은? 나쁜 요약은 어떻게 잡나?</div>
  <div class="oa-card"><span class="oa-card__icon">🧑‍🔧</span><span class="oa-card__title">소유</span>출시 후 업데이트는 누가?</div>
</div>

### 구현 라이프사이클 — 직선이 아니라 고리

<div class="oa-svg">
<svg viewBox="0 0 720 220" role="img" aria-label="구현 라이프사이클 순환도">
  <defs>
    <marker id="ar2t" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.b2{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.t2{font-size:12px;fill:#4a4a4a}.s2{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="40" width="120" height="44" rx="10" class="b2"/><text x="70" y="67" text-anchor="middle" class="t2">워크플로 범위</text>
  <rect x="150" y="40" width="120" height="44" rx="10" class="b2"/><text x="210" y="67" text-anchor="middle" class="t2">필요한 정보</text>
  <rect x="290" y="40" width="120" height="44" rx="10" class="b2"/><text x="350" y="67" text-anchor="middle" class="t2">실행 위치</text>
  <rect x="430" y="40" width="120" height="44" rx="10" class="b2"/><text x="490" y="67" text-anchor="middle" class="t2">테스트·safeguard</text>
  <rect x="570" y="40" width="140" height="44" rx="10" class="b2"/><text x="640" y="67" text-anchor="middle" class="t2">출시 후 소유</text>
  <line x1="130" y1="62" x2="146" y2="62" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar2t)"/>
  <line x1="270" y1="62" x2="286" y2="62" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar2t)"/>
  <line x1="410" y1="62" x2="426" y2="62" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar2t)"/>
  <line x1="550" y1="62" x2="566" y2="62" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar2t)"/>
  <path d="M490,86 C490,170 70,170 70,90" fill="none" stroke="#c99400" stroke-width="1.5" stroke-dasharray="5 4" marker-end="url(#ar2t)"/>
  <text x="280" y="160" text-anchor="middle" class="s2">테스트에서 데이터 누락 발견 → 앞 단계로 돌아가 다시 명확히</text>
  <text x="360" y="200" text-anchor="middle" class="s2">모든 프로젝트가 같은 순서로 가진 않아요. 배우면서 앞으로 갔다 뒤로 돌아오는 게 정상이에요.</text>
</svg>
</div>

### "구현 준비 완료"란?

모든 디테일이 풀린 상태가 **아니에요.** 모르는 것이 **보이고**, 가정에 **이름이 붙고**, 다음 단계가 **적절한 크기로** 정해진 상태예요.

> 판단 질문: **"피할 수 있는 리스크나 재작업 없이 다음 기술 단계를 밟을 만큼 알고 있나?"** — 아니라면 기회가 나쁜 게 아니라, 발견·검증·소유 정리가 더 필요한 것뿐이에요.

다음 단계는 안전한 프로토타입, 데이터 접근 리뷰, 워크플로 명확화 세션, eval 설계 논의, 전문가 핸드오프 중 하나일 수 있어요.

### 흔한 구현 갭

| 갭 | 예 (지원 상담원 Q&amp;A 어시스턴트) |
|---|---|
| 모호한 워크플로 범위 | "더 빨리 답하게"만 있음 |
| source-of-truth 데이터 부재 | 고객 지식이 여러 시스템에 흩어짐 |
| 불명확한 권한 | AI가 어떤 콘텐츠에 접근해도 되는지 모름 |
| 테스트 케이스·품질 기준 없음 | 답변 품질 측정 합의 없음 |
| fallback·사람 검토 경로 없음 | 모를 때 어떻게 할지 미정 |
| 운영 담당자 불명확 | 지식 소스 유지 담당 없음 |
| 데모 결과 과신, 제품 동작 가정 | "데모에서 됐으니까" |

## 2. API — 연동의 기초

<div class="oa-eli">
  <span class="oa-eli__head">✈️ 쉽게 말하면</span>
  <p>여행 사이트는 항공사 시스템에 "이 날짜 좌석 있어요?"라고 <b>묻고</b>, 답을 받아 화면에 보여 줘요. 이 "묻고 답하는 창구"가 API예요. AI도 똑같아요. 앱이 AI 서비스에 요청을 보내고, 받은 응답을 더 큰 업무 흐름에 끼워 넣죠.</p>
</div>

### 인터페이스 선택 vs 런타임 선택 — 서로 다른 질문

OpenAI는 새 프로젝트에 **Chat Completions보다 Responses API를 권장**해요 (강의 기준). 하지만 이건 "앱이 모델 출력을 어떻게 요청하나"라는 **인터페이스** 선택일 뿐이에요. 여러 단계를 거치는 작업이라면 **누가 단계를 조율하나**(agent runtime)는 별도의 결정이에요.

<div class="oa-eli">
  <span class="oa-eli__head">🎼 Agent runtime, 쉽게 말하면</span>
  <p>오케스트라에서 악기(모델·도구)만 있으면 연주가 안 돼요. <b>지휘자</b>가 "이제 바이올린, 다음 첼로" 하고 순서를 맞춰야 하죠. 모델 응답과 도구 호출을 이어 주고 한 단계의 결과를 다음 단계로 넘기는 소프트웨어가 <b>agent runtime</b>, 이 반복이 <b>agent loop</b>예요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🏢 Application-owned runtime</span>
    <ul>
      <li>팀이 자기 앱에서 조율 소프트웨어를 운영 (Agents SDK 선택적 사용)</li>
      <li>Responses가 hosted 도구·턴 간 정보 유지를 제공할 수 있음</li>
      <li>"팀이 loop를 운영"한다는 뜻이지 모든 도구를 직접 호스팅한다는 뜻은 아님</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">☁️ Managed runtime (Agents API)</span>
    <ul>
      <li>OpenAI가 관리형 harness를 운영: agent loop 조율, <b>context compaction</b>(이전 작업 요약), 연결 끊겨도 세션 진행 유지</li>
      <li>앱은 세션에 작업을 보내고 진행을 따라가며 추가 메시지로 방향 조정</li>
      <li>팀은 여전히 지시·승인된 정보·도구·검토 요건을 제공</li>
    </ul>
  </div>
</div>

(강의 기준) Agents API는 세션 데이터를 보존하고 현재 **US 데이터 레지던시만** 지원하며 **Zero Data Retention은 미지원**이라고 안내돼요. 사용할 레코드가 이 조건에서 허용되는지 확인이 필요해요.

### 요청-응답 사고

<div class="oa-svg">
<svg viewBox="0 0 720 240" role="img" aria-label="주간 업데이트 워크플로의 요청과 응답 흐름">
  <defs>
    <marker id="ar2r" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.p2{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.g2{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.w2{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.t2r{font-size:12px;fill:#4a4a4a}.s2r{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="20" width="200" height="200" rx="10" class="p2"/>
  <text x="110" y="42" text-anchor="middle" class="t2r">📤 요청 (Request)</text>
  <text x="24" y="68" class="s2r">· 사용자/앱 입력</text>
  <text x="24" y="88" class="s2r">· system-level 지시</text>
  <text x="24" y="108" class="s2r">· 승인된 컨텍스트·파일</text>
  <text x="24" y="128" class="s2r">· 도구 (검색 포함)</text>
  <text x="24" y="148" class="s2r">· 출력 계약 (JSON Schema)</text>
  <text x="24" y="168" class="s2r">· 상태 (이전 응답 정보)</text>
  <text x="24" y="188" class="s2r">· 신원·권한</text>
  <line x1="212" y1="120" x2="268" y2="120" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar2r)"/>
  <rect x="272" y="80" width="170" height="80" rx="10" class="g2"/>
  <text x="357" y="112" text-anchor="middle" class="t2r">Responses API</text>
  <text x="357" y="132" text-anchor="middle" class="s2r">모델 출력 + 도구 호출</text>
  <line x1="444" y1="120" x2="500" y2="120" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar2r)"/>
  <rect x="504" y="20" width="206" height="140" rx="10" class="p2"/>
  <text x="607" y="42" text-anchor="middle" class="t2r">📥 응답 (Response)</text>
  <text x="518" y="68" class="s2r">· 답변/초안, 구조화 출력</text>
  <text x="518" y="88" class="s2r">· 분류·추천</text>
  <text x="518" y="108" class="s2r">· 도구 호출/결과</text>
  <text x="518" y="128" class="s2r">· 상태·승인 요청</text>
  <text x="518" y="148" class="s2r">· 거절·에스컬레이션</text>
  <rect x="504" y="172" width="206" height="48" rx="10" class="w2"/>
  <text x="607" y="200" text-anchor="middle" class="t2r">👀 관리자 검토 → 공유</text>
</svg>
</div>

| 용어 | 뜻 |
|---|---|
| **State (상태)** | 다음 턴을 이어가기 위해 필요한 이전 응답 정보 |
| **Output contract (출력 계약)** | 받는 쪽 시스템이 기대하는 필드와 형식. 하류 시스템이 믿을 만한 구조가 필요하면 **Structured Outputs + JSON Schema** |
| **JSON Schema** | 어떤 필드가 필수이고 각 필드가 어떤 타입(문자·숫자)인지 정한 규칙 |
| **Trace (실행 기록)** | 모델 출력·도구 호출과 결과·승인·행동을 나중에 들여다볼 수 있게 남긴 기록 |

<div class="oa-eli">
  <span class="oa-eli__head">📋 형식이 맞다 ≠ 내용이 맞다</span>
  <p>시험지 칸을 전부 채웠다고 정답인 건 아니죠. 주간 업데이트 초안이 필수 필드를 다 갖춰도 <b>담당자 이름이 틀릴</b> 수 있어요. 스키마 검사는 <b>구조</b>를, 원본 레코드와의 대조는 <b>정확성</b>을 확인해요. 담당자를 확인할 수 없다면 "스키마 통과 = 사용 가능"으로 보지 말고 검토로 보내요.</p>
</div>

판단 질문: **"AI에 무엇을 보내고, 무엇이 돌아오고, 그다음엔 무슨 일이 일어나나?"** 셋 중 하나라도 설명이 안 되면 워크플로를 더 명확히 해야 해요.

### 인증·접근·secret

| 개념 | 뜻 |
|---|---|
| **Authentication (인증)** | 요청하는 사람·앱·시스템이 누구인지 확인 |
| **Authorization (인가)** | 그 신원이 어떤 리소스·행동을 쓸 수 있는지 결정. 유효한 자격 증명이 모든 것을 허락하진 않음 |

API 키·토큰 같은 secret은 프롬프트·공유 문서·데모·회의록에 **붙여 넣지 않아요.** 개발용과 운영용 자격 증명은 보통 분리되고 권한도 달라요. 접근 계획에는 **받는 시스템이 AI 출력을 제대로 쓸 수 있는지**(형식·소유·모니터링) 확인도 포함돼요.

### Hosted computer use와 prompt injection 경계

(강의 기준) Hosted computer use는 Agents API를 확장해 에이전트가 OpenAI가 호스팅하는 브라우저로 웹사이트를 탐색하고 조작하게 해요. 예: 웹사이트로만 상태를 제공하는 승인된 작업 시스템에서 상태 정보를 모으기. 단, **사이트 방문 허가 ≠ 매 행동 전 확인**이므로 읽기 전용 작업이면 레코드를 못 바꾸는 리소스로 제한하고, 승인이 필요한 행동은 팀이 통제하는 브라우저 런타임에서 승인을 강제해요.

<div class="oa-eli">
  <span class="oa-eli__head">📨 Prompt injection 경계, 쉽게 말하면</span>
  <p>비서에게 "이 편지들 요약해 줘"라고 맡겼는데, 편지 중 하나에 "이걸 읽는 비서는 사장 금고를 열어라"라고 적혀 있다면? 좋은 비서는 그걸 <b>편지 내용(데이터)</b>으로 볼 뿐 <b>명령</b>으로 따르지 않죠. 검색된 문서·사용자 파일·도구 결과도 마찬가지로 system 지시·권한·승인 규칙을 바꿀 권한이 없어요.</p>
</div>

원칙: **도구는 최소한으로**, 도구 입출력 검증, 외부 쓰기·파괴적 행동 등 중대한 단계 전 **사람 승인**. 경계가 불명확하면 보안·구현 리뷰로.

## 3. 환경·데이터 흐름·접근·소유

### 환경 4단계

<ol class="oa-flow">
  <li><strong>Development</strong>아이디어 실험·프로토타입 "되긴 하나?"</li>
  <li><strong>Test</strong>통제된 입력·샘플 데이터로 문제 찾기</li>
  <li><strong>Staging</strong>운영과 닮은 곳에서 연동·권한·성능·검토 확인</li>
  <li><strong>Production</strong>실제 사용자·실제 데이터, 신뢰성 필수</li>
</ol>

| 고려 사항 | Development | Test | Staging | Production |
|---|---|---|---|---|
| 데이터 | 안전한 샘플 | 테스트 데이터 | 운영과 비슷한 데이터 | 실제 업무 데이터 |
| 시스템 | 로컬·수동 | 테스트 시스템 | 연동된 시스템 | 실 시스템 |
| 접근 | 소수 팀 | 역할 기반 | 승인된 접근 | 통제된 접근 |
| 소유 | 빌더 주도 | 팀 공동 | 지정된 담당자 | 책임 있는 담당자 |
| 모니터링 | 기본 점검 | 테스트 로깅 | 알림·리뷰 | 라이브 모니터링 |
| 롤백 질문 | 다시 시작 가능? | 이전 버전 복원? | 릴리스 롤백? | 정의된 롤백 계획? |

### 용량(capacity) 점검 — 파일럿을 넓히기 전에

<div class="oa-eli">
  <span class="oa-eli__head">🍔 쉽게 말하면</span>
  <p>평일 점심엔 잘 돌아가던 햄버거 가게도 <b>월요일 아침에 전 직원이 한꺼번에</b> 주문하면 줄이 터져요. 한 프로젝트에서 잘 된 주간 업데이트 파일럿도 여러 팀이 동시에 요청하면 버거울 수 있어요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🚦</span><span class="oa-card__title">Traffic</span><b>Rate limit</b>은 시간당 요청·토큰 수를 제한. 사용 등급(Build·Launch·Grow, 강의 기준)이 한도에 영향. 실제 조직·프로젝트·모델 한도를 예상 피크와 비교</div>
  <div class="oa-card"><span class="oa-card__icon">⏱️</span><span class="oa-card__title">Waiting time</span><b>Latency</b>는 결과가 돌아오기까지의 시간. 빠른 처리 모드는 대기 시간을 줄일 수 있지만 비용이 다르고, 버스트 처리량을 보장하진 않음</div>
  <div class="oa-card"><span class="oa-card__icon">💸</span><span class="oa-card__title">Spending</span>OpenAI가 승인한 월 사용 한도 ≠ 팀이 설정한 지출 통제. 알림은 요청을 계속 보내고, hard limit은 요청을 막지만 즉시 적용되진 않아 약간 초과 가능</div>
</div>

### 배포 경계(deployment boundary)

배포 경계는 **솔루션이 어디서 돌고 무엇에 접근하는지**를 정해요. 소규모 팀이 정보를 붙여 넣고 결과를 검토하는 프로젝트 업데이트 어시스턴트는 경계가 좁아요. 그런데 같은 어시스턴트가 프로젝트 관리 시스템에서 레코드를 끌어오고 다른 도구에 업데이트를 **써 넣으면** 경계가 넓어지고, 접근·권한·로깅·검토·소유가 훨씬 중요해져요.

### 운영 소유와 릴리스·롤백

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧭</span><span class="oa-card__title">워크플로 소유자</span>업무 프로세스 책임</div>
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">앱·연동 소유자</span>애플리케이션·통합 책임</div>
  <div class="oa-card"><span class="oa-card__icon">🔑</span><span class="oa-card__title">데이터 접근 승인자</span>어떤 데이터를 써도 되는지</div>
  <div class="oa-card"><span class="oa-card__icon">📈</span><span class="oa-card__title">모니터링 담당</span>사용량·품질·응답 시간·지출·rate-limit 실패, 한도 도달 시 대응</div>
  <div class="oa-card"><span class="oa-card__icon">🛠️</span><span class="oa-card__title">개선 담당</span>프롬프트·도구·eval·지시 업데이트</div>
</div>

책임 있는 릴리스는 **작게 시작**해요 — 제한된 파일럿, 통제된 데이터 소스. 롤백/비활성화 경로도 미리 정해요: 기능 끄기, 연동 일시 중지, 수동 프로세스로 복귀, 접근 제한, 모든 출력을 사람 검토로 보내기.

예: 재무 정책 어시스턴트 프로토타입은 **작은 테스트 그룹 · 승인된 정책 문서 · 읽기 전용 · 피드백 추적 · 정책 업데이트 담당자 · 불확실/민감 질문 에스컬레이션**으로 시작. 출처에 없거나, 문서끼리 충돌하거나, 민감한 예외면 **지어내지 말고 에스컬레이션**.

## 4. Eval과 safeguard로 출시 자신감 쌓기

데모는 **한 번** 된다는 걸 보여 주고, eval은 **여러 경우에 꾸준히** 된다는 걸 보여 줘요. 프롬프트·모델·도구·검색 방식을 비교하고, 사용자보다 먼저 문제를 잡고, 개선을 추적하는 데 쓰여요. 단, eval도 사람의 판단이 필요해요.

### Starter eval set에 넣을 것

<div class="oa-bars">
  <div class="oa-bar"><span>일반적인 성공 케이스</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:90%"></span><span class="oa-bar__val">기본 동작 확인</span></span></div>
  <div class="oa-bar"><span>엣지 케이스</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:70%"></span><span class="oa-bar__val">경계 상황</span></span></div>
  <div class="oa-bar"><span>실패 케이스</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:70%"></span><span class="oa-bar__val">틀려야 할 때 잘 틀리나</span></span></div>
  <div class="oa-bar"><span>모호한 입력</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:60%"></span><span class="oa-bar__val">되묻는가</span></span></div>
  <div class="oa-bar"><span>안전·정책 민감</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:60%"></span><span class="oa-bar__val">에스컬레이션하나</span></span></div>
  <div class="oa-bar"><span>도구·검색·상태·prompt injection</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:50%"></span><span class="oa-bar__val">해당될 때</span></span></div>
  <p class="oa-bars__caption">막대는 개념적 표현이에요. 완벽한 벤치마크가 아니라 초기 안전망이 목표예요.</p>
</div>

### Safeguard는 위험에 맞춰

| 위험 | 맞는 safeguard |
|---|---|
| 민감 데이터 사용 | 접근 통제, 검토 규칙, 민감 데이터 출력 검사 |
| 다른 시스템 업데이트 | 승인 게이트, 로깅, 도구 범위 제한 |
| 사용자가 출력으로 의사결정 | 품질 검사, 출처 표기, 에스컬레이션 경로 |
| 도구·검색 기반 워크플로 | prompt injection 경계 + 테스트 |

safeguard 범주: 입력 필터링, 출력 검사, 구조화 출력, 도구 범위 제한, prompt injection 경계, 사람 승인, 접근 제한, 모니터링, 로깅, 에스컬레이션 규칙.

### 로그·모니터링·개선 루프

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧵</span><span class="oa-card__title">Trace</span>다단계 작업의 출력·도구 호출·승인·행동 순서를 재구성</div>
  <div class="oa-card"><span class="oa-card__icon">📒</span><span class="oa-card__title">Log</span>특정 사건 조사: 어떤 입력·컨텍스트·출력·다음 행동이었나</div>
  <div class="oa-card"><span class="oa-card__icon">📊</span><span class="oa-card__title">Monitoring</span>시간에 따른 패턴: 사용량·실패·품질·지연·비용·에스컬레이션률</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🔁</span><span class="oa-card__title">Improvement loop</span>증거를 행동으로: 프롬프트·도구·검색 소스·eval·safeguard 수정, 릴리스 축소, 검토 단계 추가</div>
</div>

관측성은 나중에 붙이는 게 아니라 **설계 고려 사항**이에요.

## 5. Implementation Readiness Snapshot

| 질문 | 짧게 적을 것 |
|---|---|
| 1. 무엇을 구현하려 하나? | 워크플로, 사용자 그룹, 의도한 출력, 다음 단계 |
| 2. AI 상호작용은 어떻게? | (커스텀 API 빌드라면) 인터페이스 · 런타임 소유자 · 그 조합의 이유 / 입력·지시·승인된 컨텍스트 / 도구·상태·출력 계약 / 출력 후 사람 검토·행동 |
| 3. 주변에 무엇이 준비돼야? | 첫 실행 환경, 데이터·시스템, 접근자, 검토·유지·모니터링 담당, rate limit·응답 시간·지출 통제 확인 담당 |
| 4. 무엇을 테스트·검토·에스컬레이션? | starter eval 1~2개, safeguard·prompt injection 경계·검토 지점, trace·모니터링 신호, 릴리스/롤백 기준과 전문가 검증 질문 |

<div class="oa-eli">
  <span class="oa-eli__head">💡 예시 (주간 프로젝트 업데이트, 요약)</span>
  <p>파일럿은 Responses API + 팀 앱이 조율하는 고정 순서(승인된 회의록·작업 레코드 수집 → 구조화 초안 요청 → 프로젝트 담당자 검토). 나중에 에이전트가 빠진 정보를 조사하며 여러 번 수정해야 하면 Agents API 세션을 비교 검토. 정상 업데이트·상충 입력·누락 정보·원본 속 신뢰할 수 없는 지시를 테스트하고, 반복 실패 시 검토·범위 축소·롤백.</p>
  <p>Snapshot의 어느 칸이 <b>채우기 어렵다면</b>, 그 자체가 "발견·검증·전문가가 더 필요하다"는 유용한 신호예요.</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. "강한 유스케이스와 경영진 지원이 있어요. 다음 달 출시하고 싶어요." 그런데 사용자 그룹·데이터 소스·검토·출시 후 담당자가 불명확하다면?</summary>
  <p>① 경영진 지원이 강한 신호이니 출시 ② 데모 한 번 더 ③ 완전한 운영 계획 전까지 모든 작업 중단 ④ 사용자·데이터·검토·소유를 먼저 명확히 한 뒤 구현 준비 완료로 본다</p>
  <p><b>정답: ④</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 요청-응답 사고에서의 역할</summary>
  <p>다음 턴에 필요한 이전 응답 정보 → <b>State</b><br>회의록 발췌와 작업 레코드 → <b>Input and approved context</b><br>모델 출력·도구 호출·승인·행동 기록 → <b>Trace</b><br>새 프로젝트에서 Responses vs Chat Completions → <b>권장 인터페이스(런타임 소유는 별개)</b><br>JSON Schema로 정의한 리스크·담당자·마감일 필드 → <b>Output contract</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 샘플 데이터로 잘 되는 프로토타입을 전 직원 대상 운영으로 옮기려 한다. 가장 좋은 다음 질문은?</summary>
  <p><b>정답:</b> "출시 전 필요한 <b>운영 데이터, 접근 통제, 모니터링 신호, 운영 담당자, 롤백 경로</b>는 무엇인가?" (출시 공지·교육 담당, 프로토타입 설정 재사용, 샘플 데이터 테스트 몇 개 추가는 핵심을 놓침)</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 짝 맞추기 — 구현 리스크와 가장 좋은 첫 대응</summary>
  <p>도구가 레코드를 잘못 업데이트할 수 있음 → <b>승인 게이트 + 도구 사용 테스트</b><br>오래된 정책으로 답할 수 있음 → <b>검색/소스 품질 eval</b><br>실패를 조사해야 함 → <b>로깅·모니터링</b><br>민감한 직원 정보 포함 가능 → <b>사람 검토 또는 민감 데이터 safeguard</b><br>출력이 고정 형식을 따라야 함 → <b>구조화 출력 검사</b></p>
</details>

<details class="oa-quiz">
  <summary>Q5. 워크플로·프로토타입은 명확하다. 더 빠른 처리 모드를 골랐지만 피크 용량 점검과 출시 후 실패 모니터링 담당자 지정은 안 했다. 다음 단계는?</summary>
  <p><b>정답:</b> Snapshot에 <b>용량·소유·모니터링 질문</b>을 추가하고 출시 전에 해결. (더 강한 모델 선택, UI 문구 먼저, "프로토타입이 되니 진행"은 오답)</p>
</details>

## 정리

- 좋은 아이디어 ≠ 구현 준비 완료. 준비도란 **모르는 것이 보이고, 가정에 이름이 붙고, 다음 단계가 적절한 크기**인 상태.
- **Responses API 권장(인터페이스)**과 **누가 agent runtime을 운영하나(앱 소유 vs Agents API 관리형)**는 별개의 결정.
- 요청-응답 사고: 무엇을 보내고(입력·지시·컨텍스트·도구·상태·권한), 무엇이 오고(출력 계약), 다음에 누가 검토·행동하나. **스키마 통과 ≠ 내용 정확**.
- 환경(Dev→Test→Staging→Prod)이 올라갈수록 데이터·접근·소유·모니터링·롤백 기대가 엄격해지고, 넓히기 전 **traffic·latency·spending**을 확인.
- Eval·safeguard·trace·모니터링은 처음부터 설계하고, Snapshot으로 남은 질문을 드러낸다.
