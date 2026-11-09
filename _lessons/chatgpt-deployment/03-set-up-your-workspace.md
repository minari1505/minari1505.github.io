---
title: "Set Up Your Workspace"
title_ko: "워크스페이스 준비 — 승인된 워크플로를 위한 설정 준비도 체크리스트"
course: chatgpt-deployment
lesson: 3
---

## 학습 목표

- 배포 요구사항(워크플로·사용자·성공 증거·거버넌스)을 **워크스페이스 요구사항**으로 번역하기
- 신원(identity)·접근·앱·플러그인·Workspace Agent에 걸친 **소유와 관리 책임** 정하기
- SSO·SCIM 프로비저닝, 역할·그룹, **RBAC(역할 기반 접근 제어)** 요구 파악하기
- 워크플로가 의존하는 Apps·plugins·Company Knowledge·ChatGPT Work·Workspace Agent의 **준비도 평가**하기 (강의 기준 제품명)
- **채택 분석(adoption analytics)과 컴플라이언스 증거를 구분**하고, 행동·보존·레지던시·사용량·지출 통제 의존성을 기록하기
- 결과물: **Workspace Configuration Readiness Checklist**

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>계획 → 워크스페이스 요구</strong>워크플로가 무엇을 필요로 하나</li>
  <li><strong>소유·관리 책임</strong>누가 접근·설정·소스를 맡나</li>
  <li><strong>접근·거버넌스</strong>누가 무엇을 쓰고, 어디까지 허용?</li>
  <li><strong>지식·앱·에이전트</strong>맥락과 도구가 준비됐나</li>
  <li><strong>준비도 체크리스트</strong>Ready · Needs validation · Blocked · Deferred</li>
</ol>

이 코스는 콘솔을 한 번씩 클릭하는 설정 실습이 아니라, **"플랜을 인지한 관리자 리뷰(plan-aware admin review)"** 로 어떤 설정·검증 결정이 필요한지 정하는 과정이에요.

## 1. 워크스페이스 준비도란?

<div class="oa-eli">
  <span class="oa-eli__head">🏫 쉽게 말하면</span>
  <p>새 동아리 활동을 시작한다고 해 봐요. "과학 실험을 한다"(계획)만으로는 부족하고, <b>실험실 열쇠는 누가 갖고</b>(접근), <b>어떤 약품을 써도 되는지</b>(승인된 소스), <b>선생님 확인이 필요한 실험은 뭔지</b>(사람 검토), <b>사고가 나면 누구에게 알리는지</b>(에스컬레이션)가 정해져야 첫날 바로 실험을 할 수 있죠.</p>
  <p>워크스페이스 준비도 = <b>계획과 활성화 사이의 다리</b>예요.</p>
</div>

| 계획 단계가 묻는 것 | 워크스페이스 준비도가 묻는 것 |
|---|---|
| 어떤 워크플로를 출시하나? | 그 워크플로에 필요한 사용자·접근 경로·소스·워크스페이스 조건은? |
| 어떤 성공 증거가 필요하나? | 채택 분석, Task Insights, 피드백 경로, 출력 리뷰, 컴플라이언스 증거 중 무엇이 가능해야? |
| 어떤 거버넌스 가정이 있나? | 정책·소스·앱·에이전트·행동·보존·레지던시·사용량 통제를 **누가 검증**하나? |
| 누가 출시를 소유하나? | 워크스페이스 관리, 소스 준비, 에스컬레이션, 미해결 셋업은 누가? |

연결이 끊기면 셋업이 "기술적 취향 목록"으로 변해요. 그래서 **제품 선호가 아니라 워크플로에서 출발**합니다.

## 2. 배포 계획을 워크스페이스 요구로 번역하기

계획 입력(Deployment Success Plan, 승인된 핸드오프 등)에서 셋업에 영향을 주는 정보를 찾아요: 대상 워크플로, **필요한 ChatGPT 경험**(Chat·Work·Workspace Agent)과 그 역할에서의 활성화 여부, 코호트, 범위, 성공 기준, 제약, Company Knowledge·앱·플러그인·동기화 소스 의존성, 거버넌스·행동 승인·보존·레지던시·사용량 통제·지역 고려, 필요한 비즈니스 맥락.

예시 워크플로 하나 — "법무 운영 담당자가 승인 자료로 **사람이 검토하는 공급업체 계약 접수 요약**을 준비" — 만으로도 요구가 줄줄이 나와요. 또 이 일이 **사용자가 시작하는 일회성 작업인지, 계속 돌아가는 작업인지**, 누구의 인증된 연결을 쓰는지, 결과를 누가 검토하고 누가 받는지도 확인해요. **반복 작업이나 공유 페이지는 별도의 범위 결정**이지 첫 출시에 자동 포함되지 않아요.

| 배포 요구 | 워크스페이스 함의 | 검증 오너 |
|---|---|---|
| 대상 사용자 그룹 확정 | 접근·프로비저닝 경로 필요 | 워크스페이스 관리자 / IT |
| 사내 정책 문서에 의존 | 지식 소스·앱·플러그인·Agent 준비 필요 | 소스 오너 (내용·접근·승인) |
| 규제·민감 워크플로 | 데이터 사용 경계, 사람 검토 기대 | 법무·보안·컴플라이언스 |
| 성공 기준에 사용·참여 포함 | 분석·피드백·리뷰 메커니즘 | 배포 오너 |
| 검토 가능한 출력 필요 | 모니터링·피드백·이슈 경로 | 배포 오너 |

<div class="oa-eli">
  <span class="oa-eli__head">🔍 한 단어 안에 숨은 요구들</span>
  <p>"법무 운영 코호트"는 그냥 사용자 그룹이 아니라 <b>접근 경로 + 프로비저닝 + 역할·그룹 결정 + 접근 문제 지원 경로</b>를 뜻해요. "승인된 계약 접수 자료"도 <b>소스 오너 + 내용 준비 + 접근 검증 + 첫 활성화에 필수인지 판단</b>을 뜻하고요.</p>
</div>

### 설정 의존성(configuration dependency)

워크스페이스가 대상 워크플로를 지원할 수 있는지에 영향을 주는 모든 것:

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">👤</span><span class="oa-card__title">사용자·역할·그룹</span>제품 표면 접근 포함</div>
  <div class="oa-card"><span class="oa-card__icon">🪪</span><span class="oa-card__title">신원·수명주기</span>도메인, SSO, SCIM, 프로비저닝·디프로비저닝</div>
  <div class="oa-card"><span class="oa-card__icon">🧑‍🔧</span><span class="oa-card__title">오너들</span>워크스페이스·접근 제어·앱/에이전트·분석/컴플라이언스</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">맥락·도구</span>Company Knowledge, Apps, plugins, 동기화 소스, Workspace Agent</div>
  <div class="oa-card"><span class="oa-card__icon">🗄️</span><span class="oa-card__title">보존·레지던시</span>보존·삭제, 데이터·추론 레지던시, 지역 요건</div>
  <div class="oa-card"><span class="oa-card__icon">🚧</span><span class="oa-card__title">경계·통제</span>민감 데이터, 앱 행동, 승인, 공유, 로깅, 사용량·지출 통제</div>
</div>

이 코스에서 다 해결할 필요는 없어요. **보이게 하고, 오너를 붙이고, "활성화 전 필수 / 미룰 수 있음"을 정하는 것**이 목표. 그리고 가용성·통제 정보는 **시점 의존적**이라, 고객의 요금제·워크스페이스·역할·표면·지역 기준으로 확인한 뒤에만 Ready로 표시해요.

## 3. 워크스페이스 소유와 관리 책임

| 역할 | 소유하는 것 |
|---|---|
| **Business owner** | 워크플로 결과, 비즈니스 우선순위, 출시 목표 지원 여부 결정 |
| **Deployment owner** | 파트너·고객 간 준비 조율, 의존성 추적, 미해결 항목 가시화 |
| **Workspace administrator** | 워크스페이스 설정, 제품 표면 활성화, 앱·플러그인 가용성, Agent 통제 |
| **Access-control owner** | 역할·그룹·RBAC 할당 (표면·앱·플러그인·Agent별) |
| **App / agent owner** | 앱·플러그인·Agent 설정, 연결 모델, 게시, 허용 행동, 승인 설정 |
| **IT / identity owner** | 도메인, SSO, SCIM, 프로비저닝·디프로비저닝, 기기·접근 경로 |
| **Security·legal·compliance owner** | 민감 데이터 경계, 앱·에이전트 행동 리스크, 보존·레지던시, Compliance API·로깅 |
| **Source owner** | 문서·Company Knowledge·동기화 콘텐츠가 최신·접근 가능·적합한지 |

작은 배포면 한 사람이 여러 역할을 해도 되지만 **책임은 따로 보이게** 적어요. 예: 법무 운영 리드가 워크플로·결과 오너를 겸해도, 워크스페이스 관리·소스 승인·정책 검증은 다른 고객 오너일 수 있어요.

관리 책임도 **암묵적으로 두지 말고** 명시: 사용자 관리, 접근 관리, 워크스페이스 설정, 거버넌스 검증, 소스 준비 조율, 셋업 막힘 에스컬레이션, 지속 관리. "출시는 소유하지만 설정은 아님", "워크플로는 승인하지만 소스 접근은 아님"처럼 분명히 써요.

에스컬레이션 대상 막힘: 접근·프로비저닝, 설정, 지역·레지던시 질문, 앱·플러그인·Agent·지식 소스 접근, 민감 데이터·정책, 승인 지연, 불명확한 관리 책임. 각 경우 **누가 결정하고 누가 검증하는지**를 적어요.

## 4. 접근과 거버넌스 요구

### 접근은 "층"으로 쌓여 있어요

<div class="oa-eli">
  <span class="oa-eli__head">🏢 쉽게 말하면</span>
  <p>회사 건물 출입증이 있다고 <b>모든 층·모든 방</b>에 들어갈 수 있는 건 아니죠. 1층 로비(워크스페이스) → 특정 층(제품 표면: Chat·Work) → 특정 방(앱·플러그인·Agent) → 방 안의 특정 서랍(행동 권한)까지, 각각 따로 열려야 해요. <b>"사용자를 안다" ≠ "접근 준비 완료"</b>.</p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 230" role="img" aria-label="층층이 쌓인 접근 권한">
  <defs>
    <style>.a3{fill:#f6f3ea;stroke:#e5dfcf;stroke-width:1.5}.b3{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.c3{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.d3{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.t3{font-size:12px;fill:#4a4a4a;font-weight:bold}.s3{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="40" y="170" width="640" height="44" rx="8" class="a3"/><text x="60" y="190" class="t3">① 워크스페이스 프로비저닝</text><text x="60" y="206" class="s3">도메인 · SSO · SCIM · 디프로비저닝</text>
  <rect x="100" y="120" width="520" height="44" rx="8" class="b3"/><text x="120" y="140" class="t3">② 제품 표면 · 모델</text><text x="120" y="156" class="s3">Chat / Work / Agent · 웹·모바일·데스크톱 · 모델 접근은 따로</text>
  <rect x="160" y="70" width="400" height="44" rx="8" class="c3"/><text x="180" y="90" class="t3">③ 역할 · 그룹 · RBAC</text><text x="180" y="106" class="s3">앱 · 플러그인 · Agent 실행/빌드/게시</text>
  <rect x="220" y="20" width="280" height="44" rx="8" class="d3"/><text x="240" y="40" class="t3">④ 행동 권한</text><text x="240" y="56" class="s3">읽기/쓰기 · 승인 · 파라미터 제약</text>
</svg>
</div>

확인할 것: 사용자 그룹별 표면, 역할·그룹·RBAC 범위, SSO·SCIM·프로비저닝, 앱·플러그인·연결 소스 접근 경로, Agent의 실행·빌드·편집·게시·공유 권한, 브라우저·기기·네트워크·지역 가정.

<div class="oa-eli">
  <span class="oa-eli__head">🧪 모델 접근은 따로 검증 (강의 기준 예시)</span>
  <p>강의는 ChatGPT Work에서 <b>GPT-6 Astra</b>를 쓰는 예를 들어요. 모델 접근과 Work 접근은 <b>서로 다른 권한</b>이에요. 예전의 Early Model Access 설정이나 기본 모델 선택이 "쓸 수 있다"는 증거가 되지 않아요. 권한은 여러 역할에 걸쳐 <b>합쳐지기</b> 때문에, 한 역할이 막아도 다른 역할이 허용하면 열려 있을 수 있어요.</p>
  <p>관리자가 Admin Console의 Models &gt; Test(강의 기준)로 사용자의 <b>실효 권한</b>을 확인할 수 있지만, 이 도구는 저장된 설정을 보여 줄 뿐 접근을 부여하거나 요금제·좌석·사용 한도를 바꾸지 않아요. 마지막엔 <b>실제 사용자</b>가 올바른 워크스페이스·경험에서 접근을 확인하고, 증거·날짜·오너·남은 제약을 기록해요.</p>
</div>

### 거버넌스 요구 — "보편 규칙"이 아니라 "검증할 가정"

| 영역 | 예 |
|---|---|
| 조직 정책 | 허용 사용, 공유 기대, 사람 검토, 고영향 업무 에스컬레이션 |
| 데이터 통제 | 승인 입력·소스, 민감 데이터 제한, 보존·삭제, 데이터·추론 레지던시 |
| 컴플라이언스·감독 | 법무·보안·지역, Compliance API, 로깅, 감사, 증거 접근 |
| 앱·플러그인·Agent 통제 | 역할 접근, 개인/공유 연결, 읽기·쓰기, 행동 승인, 파라미터 제약, 게시·스케줄·트리거·공유 한도 |
| 채택 증거 | Workspace Analytics, Task Insights + 출력 품질 리뷰 + 고객 승인 성공 증거 |
| 컴플라이언스 증거 | 대화·행동 감독 — 채택 분석과 **별개**, 품질·ROI 증명도 아님 |
| 이슈·통제 | 수정, 사용량·지출 통제, 워크플로를 **멈추거나 좁히거나 끌 수 있는 오너** |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🔔 사용량 알림 (usage alert)</span>
    <ul>
      <li>임계치 도달 시 지정된 사람에게 알림</li>
      <li>사용·지출을 <b>막지는 않을 수 있음</b></li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🛑 강제 한도 (enforced limit)</span>
    <ul>
      <li>고객이 실제 제한을 요구하면, 그 요금제·워크스페이스에서 가능한 통제와 <b>실제로 무엇을 제한하는지</b> 확인</li>
      <li>모니터링·대응 오너 지정, 미해결이면 체크리스트에 기록</li>
    </ul>
  </div>
</div>

지역·조직 요건(데이터 처리, 데이터·추론 레지던시, 조직 통제, 지역 가용성, 규제 워크플로 제약)은 **기억이나 일반 상식으로 답하지 말고**, 고객 오너와 최신 공식 문서로 검증해요.

### 거버넌스를 워크플로 행동에 연결

| 거버넌스 가정 | 영향받는 행동 | 검증 오너 | 출시 의존 |
|---|---|---|---|
| 사용 전 사람 검토 필수 | 요약 초안을 검토·수정 후 사용 | 법무 운영 + 컴플라이언스 | 활성화 전 가이드·검토 절차 |
| 승인된 정책 자료만 사용 | 확인된 정책·접수 자료 사용 | 소스 오너 | 활성화 전 소스 준비 확인 |
| 민감 데이터 경계 준수 | 입력·연결·공유 금지 항목 인지 | 법무·보안·컴플라이언스 | 허용 사용 가이드 검증 |
| 앱·에이전트 쓰기 행동은 제한·승인 | 승인 범위 내에서만 기록 생성·수정, 중요한 행동은 확인 경로 | 관리자 + 앱/에이전트 오너 + 보안 | 연결 소유·허용 행동·승인·로깅·에스컬레이션 확인 |
| 채택 증거와 컴플라이언스 증거는 다른 결정용 | 참여도와 대화·행동 기록·출력 품질을 따로 리뷰 | 배포·분석·컴플라이언스 오너 | 증거 출처·접근·주기 지정 |

<div class="oa-eli">
  <span class="oa-eli__head">🖱️ 사례: 컴퓨터 사용이 켜져 있어도 모든 앱 행동이 허락된 건 아니에요</span>
  <p>ChatGPT Work에서 computer use(강의 기준)가 켜져 있지만 필요한 앱 인증·허용 행동이 미확인이라면, 그 의존성은 <b>Needs validation</b>이고 확인 전까지 해당 앱 행동은 하지 않아요. <b>초안 준비가 제출 권한을 의미하지 않아요.</b> 기록 제출 같은 중요한 행동은 승인 경로를 먼저 확인하고, 계약 시스템 쓰기 행동은 따로 승인되지 않는 한 첫 활성화에서 <b>Deferred</b>.</p>
</div>

## 5. 지식·앱·에이전트 준비도

### 용어 정리 (강의 기준)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">Apps</span>ChatGPT를 외부 도구·정보·행동에 연결 (예전 자료의 "connector")</div>
  <div class="oa-card"><span class="oa-card__icon">📦</span><span class="oa-card__title">Plugins</span>특정 워크플로용으로 앱 하나 이상 + 스킬을 묶은 패키지</div>
  <div class="oa-card"><span class="oa-card__icon">🤖</span><span class="oa-card__title">Workspace Agents</span>지시·파일·스킬·도구·앱·스케줄·트리거를 결합한 반복 작업</div>
</div>

### 두 가지 준비도 체크

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">📚 지식 소스 준비도 — "내용이 적합한가?"</span>
    <ul>
      <li>사용 가능한가 (접근 가능)</li>
      <li>최신인가</li>
      <li>이 워크플로에 승인됐나</li>
      <li>이름 있는 오너가 있나</li>
      <li>대상 워크플로에 맞나 (너무 넓거나 민감하지 않나)</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🔌 앱·플러그인·Agent 준비도 — "도구가 안전하게 묶였나?"</span>
    <ul>
      <li>정말 필요한가 (정적·동기화 맥락으로 시작 가능?)</li>
      <li>요금제·역할·표면·지역 기준으로 가용·활성화?</li>
      <li>연결 모델 승인: 사용자 본인 인증 vs 공유/에이전트 소유 연결(최소 권한 + 오너)</li>
      <li>읽기·쓰기, 승인, 제약, 게시, 스케줄, 공유, 로깅, 에스컬레이션이 적절?</li>
    </ul>
  </div>
</div>

피해야 할 두 실수: ① **존재한다고 준비된 것으로 보기**, ② **나중에 쓸모 있을 것 같다고 지금 켜기**. 앱·Agent는 **첫 워크플로가 의존할 때만** 블로커가 돼요.

> 강한 노트 예: "계약 시스템을 업데이트하는 Workspace Agent가 논의 중이지만 첫 활성화엔 필요 없음. 법무 운영 오너가 필요성을 확인하고 빌더·게시자 접근, 연결 소유, 쓰기 승인, 제약, 로깅, 에스컬레이션이 검증될 때까지 **Deferred**."

### 실행 환경과 데이터 객체도 따로 확인

ChatGPT Work가 **클라우드에서 도는지 로컬에서 도는지** 확인해요. 클라우드 작업은 데스크톱 파일, 브라우저 세션, 사내 네트워크 접근을 **자동으로 물려받지 않아요.**

| 데이터 객체 (강의 기준) | 확인 포인트 |
|---|---|
| **Files / Library** | 파일이 채팅과 별도로 Library에 저장될 수 있음. 채팅을 지워도 Library 사본은 남음 → 사본마다 보존·삭제 방법 확인 |
| **Projects** | 메시지는 워크스페이스 보존·레지던시 설정을 따르고, 파일은 삭제·보존 정책 적용 전까지 남을 수 있음 |
| **Cloud browser** | 기기 브라우저와 별개의 쿠키·로그인 세션. 사이트 데이터를 지우면 로그아웃되지만 관련 채팅·파일은 안 지워짐 |

로그인 성공이 곧 "승인된 데이터 처리 요건 충족"은 아니에요.

### 준비도 갭

빠졌거나 오래된 자료, 불명확한 소스 오너, 미승인 소스·앱 접근, 앱·Agent 가용성 미상, 검증 필요한 권한, 데이터 사용 질문, 첫 활성화에서 미룰 의존성… 모든 갭이 같은 긴급도는 아니에요. 예: 승인된 정책 가이드나 동기화된 Company Knowledge는 **활성화 전 필요**할 수 있지만, 계약 수명주기 시스템에 쓰는 Agent는 **미뤄도** 돼요.

## 6. 준비도 체크리스트 완성하기

검토 순서: **소유 → 접근·거버넌스 → 지식 준비도 → 미해결 셋업 행동 → 상태 지정.** 오너가 없으면 Ready로 표시하지 않아요.

모호한 항목은 행동 가능한 문장으로 바꿔요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 모호함</span>
    <ul>
      <li>"거버넌스 확인"</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 실행 가능</span>
    <ul>
      <li>"법무 운영 오너와 컴플라이언스 오너가 활성화 전에 공급업체 계약 요약의 사람 검토 기대를 확인한다."</li>
    </ul>
  </div>
</div>

### 네 가지 준비도 상태

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🟢</span><span class="oa-card__title">Ready</span>오너·요구·검증·다음 단계가 명확해 활성화를 지원할 만큼 확인됨</div>
  <div class="oa-card"><span class="oa-card__icon">🟡</span><span class="oa-card__title">Needs validation</span>쓸 수 있을지 모르지만 고객 오너 확인이 남음 (가정·승인·소스·접근·정책)</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔴</span><span class="oa-card__title">Blocked</span>필수 오너·접근·승인·소스가 없어 해결 전엔 활성화 불가 — <b>영원히 불가라는 뜻은 아님</b></div>
  <div class="oa-card"><span class="oa-card__icon">⏸️</span><span class="oa-card__title">Deferred</span>첫 활성화엔 불필요 — <b>중요하지 않다는 뜻은 아님</b>, 나중에 다룸</div>
</div>

### 완성된 체크리스트 예시 (요약)

| 항목 | 상태 | 활성화 전? | 다음 행동 |
|---|---|---|---|
| 워크스페이스 관리자 지정 | 🟢 Ready | 예 | 셋업 조율, 의존성 추적 |
| 접근·셋업 에스컬레이션 | 🟡 Needs validation | 예 | 막힘 담당자 지정 |
| 코호트의 표면·모델·역할/그룹 접근 | 🟡 Needs validation | 예 | 실제 사용자 기준 실효 권한, SSO/SCIM·RBAC·디프로비저닝 검증 |
| 사람 검토 기대 | 🟡 Needs validation | 예 | 검토 오너·절차·가이드 확정 |
| 민감 데이터 경계 | 🟡 Needs validation | 예 | 허용 입력·연결·공유 확인 |
| 계약 접수 템플릿 | 🟡 Needs validation | 예 | 최신성·승인·접근·적합성 확인 |
| Company Knowledge·동기화 앱 맥락 | 🟡 Needs validation | 필요 시 예 | 소스 승인, 역할 접근, 데이터 경계, 실행 환경별 읽기 전용 확인 |
| 정책 가이드 | 🔴 Blocked | 예 | 오너 지정 + 최신성·승인 검증, 안 되면 중단·재조정 |
| 이후 Agent/앱 쓰기 워크플로 | ⏸️ Deferred | 아니오 | 나중에 빌더·게시·연결 소유·쓰기 승인·로깅 정의 |
| 분석·컴플라이언스·보존·레지던시·사용량 통제 | 🟡 Needs validation | 고객 요건에 따라 | 채택 분석과 컴플라이언스 증거 구분, 한도·알림 수신자·대응 오너 확인 |
| 피드백·이슈 경로 | 🟡 Needs validation | 예 | 접근·소스·검토·출력 이슈 신고처 정의 |

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 법무 운영 코호트가 승인 자료로 사람 검토 요약을 만드는 계획. 가장 강한 워크스페이스 함의는?</summary>
  <p><b>정답:</b> 활성화 전에 <b>코호트 접근, 소스 오너십, 승인된 사용 경계, 검토 기대</b>를 확인한다. (넓은 법무팀 접근·인접 워크플로 ✗ / enablement 자료부터 ✗ / 이미 정의됐으니 바로 활성화 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 비즈니스 오너만 있고 워크스페이스 관리자·소스 오너·접근 막힘 연락처가 없다. 핵심 문제는?</summary>
  <p><b>정답:</b> 접근·소스 검증·셋업 결정을 지연시킬 수 있는 <b>소유 갭</b>이 있다. 비즈니스 오너가 모든 셋업을 승인할 수는 없어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 공유 앱 연결로 기록을 업데이트할 Workspace Agent. 대상 그룹은 알지만 게시 권한·연결 소유·쓰기 행동·승인 설정은 미확인. 체크리스트 상태는?</summary>
  <p><b>정답: Needs validation</b> — 역할·그룹 접근, 연결 소유, 행동 범위, 승인 설정, 에스컬레이션을 활성화 전에 확인해야 해요. (Ready ✗ / 자동 Deferred ✗ / 영구 Blocked ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 워크플로가 의존하는 정책 문서가 오래됐고 오너도 없다. 상태는?</summary>
  <p><b>정답: Needs validation</b> — 소스 오너십·최신성·승인을 사용 전에 확인. (존재하니 Ready ✗ / 고칠 수 없으니 영구 Blocked ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q5. ChatGPT Work의 GPT-6 Astra(강의 기준)를 쓰는 워크플로. 소스는 승인됐지만 사용자의 모델 접근은 미확인, 예전 Early Model Access 설정만 근거. 상태는?</summary>
  <p><b>정답: Needs validation</b> — 활성화 전에 실제 사용자 접근을 확인해야 해요. 소스가 승인됐다고 접근까지 Ready는 아니에요.</p>
</details>

## 정리

- 워크스페이스 준비도는 **계획과 활성화 사이의 다리**. 제품 취향이 아니라 **승인된 워크플로**에서 출발한다.
- 배포 요구 하나하나를 **워크스페이스 함의 + 검증 오너**로 번역하고, 의존성은 보이게·오너 붙여·필수/연기 구분.
- 역할별 책임(관리자·접근 제어·앱/에이전트·IT·보안·소스 오너)을 **암묵 없이** 적고 에스컬레이션 경로까지.
- 접근은 **층층이**: 프로비저닝 → 표면·모델 → 역할·RBAC → 행동 권한. "사용자를 안다" ≠ "접근 준비".
- 거버넌스는 검증할 **가정**. 채택 분석 ≠ 컴플라이언스 증거, 사용량 알림 ≠ 강제 한도.
- 소스는 "존재"가 아니라 **최신·승인·오너·적합**이어야 Ready. 앱·Agent는 첫 워크플로가 의존할 때만 블로커.
- 체크리스트 상태: **Ready · Needs validation · Blocked · Deferred** + 항목마다 오너와 다음 행동.
