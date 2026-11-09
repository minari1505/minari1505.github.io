---
title: "Advanced Codex Workflow Integration"
title_ko: "고급 Codex 워크플로우 통합 — 최소한의 유용한 통합 패턴 고르기"
course: codex-deployment
lesson: 4
---

## 학습 목표

- 일회성 Codex 사용과 **반복 가능한 통제된 통합**을 구분하기
- **통합 진단(7문항)**으로 고급 통합이 정말 필요한 워크플로우인지 판단하기
- AGENTS.md, Skill, plugin, MCP, hook, rule, managed configuration, subagent, automation 등 **메커니즘을 목적별로 비교**하기
- 위임 계약·비동기 체크포인트·자동화 준비도로 **위임/비동기 작업을 제한된 상태로** 설계하기
- 위임·검증·승인·소유권·롤백·에스컬레이션을 담은 **한 장짜리 통합 추천서** 만들기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>워크플로우 → 통합 결정</strong>3 렌즈 + 7가지 도구</li>
  <li><strong>통합 진단</strong>일회성 vs 반복, 초기 패턴</li>
  <li><strong>컴포넌트 선택</strong>결정 버킷 · 결정 매트릭스</li>
  <li><strong>위임·비동기 설계</strong>위임 계약 · 체크포인트 · 자동화 준비도</li>
  <li><strong>추천서 · 핸드오프</strong>통제 기준선 · trade-off · 롤아웃</li>
</ol>

이 코스의 **황금 원칙**:

> **최소한의 유용한 통합 패턴을 쓰고, 소유권·통제·증거를 명시하라.**

## 예제 상황 — Charlie Edward와 Rohan Engineering

가상의 파트너 **Charlie Edward**가 고객 **Rohan Engineering**을 도와요. CI 조사, PR 리뷰, 문서 유지보수, 이슈 분류에서 반복 마찰이 있어요. 일이 저장소·도구·담당자·검사·리뷰 단계에 걸쳐 흩어져 있죠.

<div class="oa-eli">
  <span class="oa-eli__head">🧰 쉽게 말하면</span>
  <p>공구함에 전동 드릴, 레이저 수평계, 자동 못총이 다 있다고 해서 <b>액자 하나 걸 때 전부 꺼내지 않아요.</b> 못 하나·망치 하나로 되면 그걸로 끝. 하지만 아파트 100세대에 같은 선반을 달아야 한다면 <b>작업 설명서(Skill), 지그(규칙), 작업 일정표(자동화)</b>가 필요해지죠. 고급 통합은 "도구를 많이 쓰는 것"이 아니라 <b>"이 일에 꼭 필요한 최소한의 도구"</b>를 고르는 거예요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 추천</span>
    <p>"MCP, hooks, Skills, 자동화 같은 고급 기능으로 워크플로우를 개선하세요."</p>
    <p>→ 어떤 워크플로우? 왜 그 컴포넌트? 통제 경계는? 담당자는? 리뷰·모니터링은?</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 추천</span>
    <p>"반복 CI 실패 조사는 <b>프로젝트 가이드 + 재사용 조사 절차</b>로 시작. 외부 context는 필요할 때만 추가. 검증 증거를 요구하고 리뷰어를 지정하고, 수정·명령·네트워크·외부 도구·범위 확장에 대한 샌드박스·승인 정책을 정한다. 스케줄링·외부 오케스트레이션 전에 롤백·에스컬레이션 경로를 세운다."</p>
  </div>
</div>

### 세 가지 통합 렌즈

| 렌즈 | 통합 결정에서 묻는 것 |
|---|---|
| 📚 Context | 어떤 지속 가이드·재사용 절차·저장소·도구·티켓·로그·외부 시스템이 필요한가? 무엇이 워크플로우를 시작하나? 외부 context의 신뢰 경계는? |
| ⚙️ Agentic runtime | 로컬·cloud·비동기·스케줄·프로젝트 범위·위임·외부 트리거 중 어디서 어떻게? 저장소 안인가, 승인된 도구 전반인가? 어떤 승인·샌드박스 자세? |
| 🔭 Observability | 어떤 검증 증거? 누가 결과를 리뷰·통합하나? 시간이 지나도 보여야 할 활동·결과·에스컬레이션·롤백 신호는? 성공/실패 지표는? |

### 이 코스의 도구들

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🩺</span><span class="oa-card__title">통합 진단</span>Trigger · Context · Action · Control · Validation · Handoff · Observability 7문항</div>
  <div class="oa-card"><span class="oa-card__icon">🧭</span><span class="oa-card__title">메커니즘 라우팅 가이드</span>워크플로우 필요 → 최소 패턴. 메커니즘끼리 섞지 않기</div>
  <div class="oa-card"><span class="oa-card__icon">✅</span><span class="oa-card__title">위임·자동화 준비도</span>제한된 위임 / subagent / 장시간 비동기 / 스케줄·외부 트리거 자동화</div>
  <div class="oa-card"><span class="oa-card__icon">📄</span><span class="oa-card__title">한 장 추천서</span>트리거·문제·메커니즘·context·통제·증거·담당자·롤백·성공 신호</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">⚖️</span><span class="oa-card__title">컴포넌트 정당화 테스트</span>더 <b>반복 가능</b>? 더 <b>통제</b>? 더 <b>관찰 가능</b>? 더 <b>확장 가능</b>? 아니면 정당화 안 된 <b>복잡성</b>?</div>
</div>

## 1단계 — 고급 워크플로우 통합이란

**고급 통합 = 사람·저장소·도구·검사·리뷰 게이트에 걸친 반복 가능하고 통제된 Codex 워크플로우 설계.** 자율 엔지니어링의 약속이 아니라, 반복되는 Codex 작업을 더 명확하고 안전하고 운영하기 쉽게 만드는 방법이에요.

<div class="oa-bars">
  <div class="oa-bar"><span>Repeatable</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:80%"></span><span class="oa-bar__val">사람·팀·저장소마다 일관되게</span></span></div>
  <div class="oa-bar"><span>Governed</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:80%"></span><span class="oa-bar__val">권한·통제·리뷰 게이트·에스컬레이션</span></span></div>
  <div class="oa-bar"><span>Observable</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:80%"></span><span class="oa-bar__val">활동·증거·소유권·결과가 보임</span></span></div>
  <div class="oa-bar"><span>Scalable</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:80%"></span><span class="oa-bar__val">임시 프롬프트·개인 기억 없이 확장</span></span></div>
  <p class="oa-bars__caption">컴포넌트가 자리를 차지할 자격이 있는 4가지 이유 — 개념적 표현이에요 (강의 수치 아님).</p>
</div>

| 통합 범주 | 지원하는 것 |
|---|---|
| 공유 context·반복성 | 저장소 가이드, 팀 규칙, 명령, 지속 기대치 |
| 재사용 작업 구조 | 표준 절차, Skills, 묶음 지시·자산 |
| 외부 시스템·도구 | 워크플로우에 필요한 승인된 시스템·저장소·도구 |
| 거버넌스·강제 | 라이프사이클 검사, 승인 자세, 명령 규칙, requirements, 관리형 통제 |
| 위임·비동기 작업 | 제한된 전문 작업, 장시간 작업, 예약 후속 작업, 승인된 외부 트리거 오케스트레이션 |

추천서는 메커니즘 하나, 작은 조합, 혹은 **아무 고급 메커니즘도 없을 수 있어요.** 예: 문서 신선도 검사는 외부 도구보다 프로젝트 가이드·재사용 절차가 먼저, 반복 CI 조사는 스케줄 자동화보다 가이드·검증·위임 계약이 먼저, 이슈 분류는 MCP·외부 오케스트레이션보다 승인된 외부 context와 접근 경계가 먼저예요.

## 2단계 — 고급 통합이 필요한지 진단하기

### 복잡함 ≠ 파편화

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">단순한 작업 복잡도</span>
    <p>개발자가 어려운 로컬 함수 설명을 요청. 기술적으로 복잡해도 context가 로컬이고 개발자가 통제 → <b>통합 설계 불필요</b></p>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">운영 파편화</span>
    <p>여러 팀에서 같은 CI 실패 반복. 로그는 이 시스템, 저장소 context는 저 시스템, 소유권 불명, 조사 노트 제각각, 어떤 검증 증거를 돌려줘야 할지 모름 → <b>구조가 필요</b></p>
  </div>
</div>

### 통합 진단 7문항

<div class="oa-svg">
<svg viewBox="0 0 720 200" role="img" aria-label="통합 진단 7문항 흐름">
  <defs>
    <marker id="ar4" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.d1{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.d2{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.dt{font-size:12px;fill:#4a4a4a;font-weight:700}.ds{font-size:10px;fill:#8a8a8a}</style>
  </defs>
  <rect x="5" y="20" width="90" height="70" rx="8" class="d1"/><text x="50" y="48" text-anchor="middle" class="dt">Trigger</text><text x="50" y="68" text-anchor="middle" class="ds">무엇이 시작?</text>
  <line x1="95" y1="55" x2="103" y2="55" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar4)"/>
  <rect x="107" y="20" width="90" height="70" rx="8" class="d1"/><text x="152" y="48" text-anchor="middle" class="dt">Context</text><text x="152" y="68" text-anchor="middle" class="ds">무슨 정보?</text>
  <line x1="197" y1="55" x2="205" y2="55" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar4)"/>
  <rect x="209" y="20" width="90" height="70" rx="8" class="d1"/><text x="254" y="48" text-anchor="middle" class="dt">Action</text><text x="254" y="68" text-anchor="middle" class="ds">무엇을 해도 되나?</text>
  <line x1="299" y1="55" x2="307" y2="55" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar4)"/>
  <rect x="311" y="20" width="90" height="70" rx="8" class="d1"/><text x="356" y="48" text-anchor="middle" class="dt">Control</text><text x="356" y="68" text-anchor="middle" class="ds">한계·승인?</text>
  <line x1="401" y1="55" x2="409" y2="55" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar4)"/>
  <rect x="413" y="20" width="90" height="70" rx="8" class="d1"/><text x="458" y="48" text-anchor="middle" class="dt">Validation</text><text x="458" y="68" text-anchor="middle" class="ds">어떻게 확인?</text>
  <line x1="503" y1="55" x2="511" y2="55" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar4)"/>
  <rect x="515" y="20" width="90" height="70" rx="8" class="d1"/><text x="560" y="48" text-anchor="middle" class="dt">Handoff</text><text x="560" y="68" text-anchor="middle" class="ds">누가 리뷰·롤백?</text>
  <line x1="605" y1="55" x2="613" y2="55" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar4)"/>
  <rect x="617" y="20" width="98" height="70" rx="8" class="d1"/><text x="666" y="48" text-anchor="middle" class="dt">Observability</text><text x="666" y="68" text-anchor="middle" class="ds">무엇이 보이나?</text>
  <rect x="110" y="120" width="500" height="64" rx="10" class="d2"/><text x="360" y="146" text-anchor="middle" class="dt">마지막 질문: 이 워크플로우에 가장 필요한 건?</text><text x="360" y="168" text-anchor="middle" class="ds">반복성 · 거버넌스 · 관찰 가능성 · 확장성 → 이 답이 메커니즘을 정한다</text>
</svg>
</div>

빠진 요소는 곧 위험이에요: 약한 트리거 → 혼란, 과한 context 접근 → 위험, 불분명한 행동 경계 → 리뷰 지원이 통제 없는 실행으로, 검증 증거 없음 → 리뷰어가 재구성, 핸드오프 담당자 없음 → 책임 공백, 약한 관찰성 → 개선·통제 어려움.

### 일회성 vs 반복 통합

| 일회성으로 충분할 때 | 반복 통합이 필요할 때 |
|---|---|
| 요청이 제한적 | 사람·팀에 걸쳐 반복 |
| 로컬 context로 충분 | 공유 프로젝트 가이드·지속 절차 필요 |
| 결과를 바로 검토 가능 | 승인된 외부 context·도구 필요 |
| 반복 필요 없음, 팀 절차 불필요 | 결정적 검사·라이프사이클 게이트 필요 |
| 개발자가 직접 조종·검사 | 제한된 병렬 조사, 예약·스레드·프로젝트·외부 트리거 후속 작업 |
| 위험 낮고 범위 명확 | 관리자·관리형 통제 요구 |

> **반복 가능 ≠ 자동화.** 반복 워크플로우도 프로젝트 가이드나 재사용 절차, 표준 증거 형식만으로 충분할 수 있어요. 자동화·외부 오케스트레이션은 **트리거·통제 경계·모니터링 담당자·롤백 경로가 분명할 때만** 추가해요.

### 초기 패턴 라우팅 (강의 기준 메커니즘)

질문을 "어떤 고급 기능을 쓸까?"에서 **"이 워크플로우가 확실히 해내야 할 일은 무엇이고, 무엇이 그걸 더 반복·통제·관찰·확장 가능하게 하나?"**로 바꿔요. 아래는 **출발 가설**이에요.

| 워크플로우에 필요한 것 | 고려할 메커니즘 | 개선되는 것 |
|---|---|---|
| 공유 저장소 가이드·규칙·명령·리뷰 기대 | AGENTS.md / 프로젝트 가이드 | 반복성, 검토 가능한 저장소 기대치 |
| 단계별 작업·가정·결정 로그·핸드오프 연속성 | 계획 산출물(PLANS.md 등) | 세션 간 연속성·관찰성 |
| 재사용 지시·참고자료·스크립트·표준 출력 | Skill | 반복성, 일관된 실행 |
| 설치·배포 가능한 여러 기능 묶음 | Plugin | 배포·유지보수 (묶인 기능은 각각 리뷰) |
| 승인된 외부 시스템·도구 | MCP | 외부 context·도구 접근 (권한·승인은 별도로 정의) |
| 라이프사이클 검사·알림·이벤트 처리 | Hook | 워크플로우 검사·관찰성 |
| 샌드박스 밖 명령에 승인 요구 | Rule | 명령 수준 거버넌스 |
| 중앙에서 전달하는 런타임 제약·공유 정책 | Requirements / managed configuration | 지원 클라이언트 간 일관성 (외부 접근 부여 아님) |
| 제한된 작업 안의 병렬 전문 작업 | Subagents | 사람이 통합하는 제한된 작업의 확장 |
| 예약·반복·프로젝트·스레드 기반 후속 작업 | Codex automation | 반복성·운영 확장 |
| GitHub PR 리뷰 | 네이티브 Codex 코드 리뷰(@codex review, Automatic reviews) | AGENTS.md 가이드 기반 리뷰 일관성 |
| 비대화형 스크립트 실행 | codex exec | 정의된 런타임·워크스페이스에서 반복 실행 |
| 앱·서비스에서 프로그래밍 방식 제어 | Codex SDK | 앱 소유 인증·런타임·리뷰 하의 맞춤 통합 |
| 커스텀 클라이언트(스트리밍 이벤트·승인·대화 이력) | App Server | 커스텀 클라이언트 내장 (**실험적, 프로덕션 미지원**) |
| GitHub 워크플로우 통합 | GitHub Action | GitHub 워크플로우 권한·secret 안의 자동화 |
| 외부 시스템이 Codex 작업을 트리거·조율 | 외부 오케스트레이션 | 명시적 소유권·롤백과 함께 외부 트리거 확장 |

## 3단계 — 컴포넌트 선택

### 7가지 결정 버킷

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📘</span><span class="oa-card__title">저장소 가이드·계획</span>AGENTS.md / 프로젝트 가이드, 계획 산출물 — 지속 context·규칙·연속성 (둘은 별개)</div>
  <div class="oa-card"><span class="oa-card__icon">📦</span><span class="oa-card__title">재사용 절차·배포</span>Skill = 재사용 절차, Plugin = 여러 기능을 묶은 설치형 패키지</div>
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">외부 시스템·도구</span>MCP — 연결 수단일 뿐, 권한·소스 승인·승인 정책은 별도 결정</div>
  <div class="oa-card"><span class="oa-card__icon">🛂</span><span class="oa-card__title">검사·정책</span>Hook = 라이프사이클 검사, Rule = 명령 승인, Requirements/managed config = 중앙 제약</div>
  <div class="oa-card"><span class="oa-card__icon">🔀</span><span class="oa-card__title">병렬·반복 작업</span>Subagents = 제한된 병렬, Automations = 예약·반복</div>
  <div class="oa-card"><span class="oa-card__icon">🧑‍💻</span><span class="oa-card__title">프로그래밍·호스팅 통합</span>codex exec, SDK, App Server, GitHub Action, 외부 오케스트레이션 — 트리거·런타임·자격 증명·롤백으로 선택</div>
  <div class="oa-card"><span class="oa-card__icon">🔎</span><span class="oa-card__title">네이티브 GitHub 리뷰</span>@codex review, Automatic reviews + AGENTS.md 리뷰 가이드 (커스텀 리뷰 준비와 다름)</div>
</div>

버킷은 **길 안내용**이고, 같은 버킷 안의 메커니즘도 **서로 바꿔 쓸 수 없어요.** 명확히 개선하지 않으면 초기 추천에서 빼요.

### 결정 매트릭스 — 소유자·경계·"쓰지 말 때"

| 메커니즘 | 이럴 때 | 소유자 | 핵심 한계 |
|---|---|---|---|
| AGENTS.md | 저장소 규칙·명령·리뷰 가이드 | 저장소·엔지니어링 담당 | 중앙 강제 계층·접근 부여가 **아님** |
| 계획 산출물 | 단계 결정·가정·핸드오프 연속성 | 작업·프로젝트 담당 | 정책·권한의 근거가 아님 |
| Skill | 반복 절차에 재사용 지시·자료·스크립트·표준 출력 | Skill·워크플로우 담당 | 짧은 프롬프트로 충분하면 불필요 |
| Plugin | 설치·배포형 패키지 | Plugin·관리자 | 묶인 기능을 각각 리뷰. **Skill의 동의어 아님** |
| MCP | 승인된 외부 context·도구 | 시스템·도구 담당 | **그 자체로 거버넌스 계층이 아님** |
| Hook | 라이프사이클 검사·알림·이벤트 | 프로젝트·플랫폼 담당 | 고위험 작업의 **유일한** 강제 수단으로 쓰지 않기 |
| Rule | 샌드박스 밖 명령에 승인 | 프로젝트·플랫폼 담당 | 역할·저장소 권한·접근 부여가 아님 |
| Requirements / managed config | 지원 클라이언트 동작을 중앙 제약 | 관리자·보안·플랫폼 | 저장소·모델·API·연결 시스템 접근을 **부여하지 못함** |
| Subagents | 제한된 작업을 병렬 전문 작업으로 분할 | 작업 담당·리뷰어 | 쪼갤 수 없는 일, 병렬 수정 충돌에 부적합 |
| Codex automation | 예약·반복 실행 | 자동화·워크플로우 담당 | **준비가 증명되기 전엔 쓰지 않기** |
| 네이티브 GitHub 리뷰 | GitHub PR 리뷰 | 저장소·리뷰 담당 | 커스텀 리뷰 준비·GitHub Action과 다름 |
| codex exec | 비대화형 스크립트 실행 | CI·구현 담당 | 스케줄·권한·거버넌스를 스스로 정의하지 않음 |
| Codex SDK | 앱·서비스의 프로그래밍 통합 | 앱 담당 | 일반 대화형 워크플로우엔 불필요 |
| App Server | 커스텀 클라이언트 | 앱·플랫폼 담당 | 실험적·프로덕션 미지원. 공식 문서·SME 리뷰로 |
| GitHub Action | GitHub Actions 안에서 Codex 실행 | 저장소·플랫폼 담당 | 네이티브 코드 리뷰와 다름 |
| 외부 오케스트레이션 | 승인된 외부 시스템이 트리거·조율 | 통합·워크플로우 담당 | 수동 트리거·내장 자동화로 충분하면 불필요 |

### 컴포넌트별 주의 메모

- **가이드·계획 산출물**은 검증 증거의 대체물이 아니에요. 새 모델(강의 기준 GPT-6 Astra)을 기존 워크플로우에 넣을 땐 가이드와 충돌하는 지시를 점검하되, 작업을 계속시키려고 **권한·승인 경계·필수 검사를 느슨하게 하지 않아요.**
- **Plugin 확장 UI**(패널·폼)는 사용자가 정보를 보고 넣는 화면일 뿐, 호출 가능한 도구나 행동 권한이 아니에요. 데스크톱 전용 확장이 CLI 기능이 되지도 않아요.
- **MCP Events**: 호출형 MCP 도구는 불렸을 때 실행되고, Events 구독은 일치하는 업데이트를 설정된 응답 워크플로우로 전달해요. 이슈 알림이 분류 요약을 시작할 순 있어도 **수정 권한을 주진 않아요.** 구독·응답 담당자, 이벤트 필터, 허용 행동, 증거, 중단·철회 경로를 정하고, 서명된 전달·중복 처리·접근 철회 시 중단을 검증해요. 지원 호스트도 실제로 확인해요.
- **Hook·Rule·Requirements**는 샌드박스·승인 정책·연결 시스템 권한·사람 리뷰의 **대체물이 아니에요.**
- **리뷰 경로**: 로컬 / 앱 내 PR / 저장소 트리거 cloud 리뷰를 구분하고, 발견 사항 검사는 게시·승인·merge 권한이 아니에요. 자동 게시도 그 자체로 승인돼야 해요.
- **관리형 브라우저 실행**(웹 여정 검증): 서비스가 브라우저를 호스팅해도 세션 연속성·사이트 접근·로그인 요청·결과 확인·복구는 앱이 관리해요. **네트워크 접근, 웹사이트 승인, 중대한 행동 전 확인은 서로 다른 경계**예요. 연결이 끊겼다고 중대한 행동을 반복하지 말고, 기록된 세션과 대기 중 행동부터 확인해요. 브라우저 활동 자체는 성공 증거가 아니에요.

## 4단계 — 위임·비동기 워크플로우 설계

### 위임 계약 (Delegation contract)

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍🍳 쉽게 말하면</span>
  <p>큰 식당 주방에서 셰프가 보조 요리사 셋에게 일을 나눠 줄 때 "너는 채소만, 너는 소스만, 너는 고기만. 끝나면 이 양식에 적어서 가져와. 소금 떨어지면 나 불러"라고 하죠. 그리고 <b>마지막에 접시에 담는 건 셰프</b>예요. 위임 계약은 그 분담표, subagent는 보조 요리사, <b>최종 통합 담당자</b>는 셰프예요.</p>
</div>

| 계약 항목 | 정의할 것 |
|---|---|
| 하위 작업 경계 | 정확히 어떤 조각을 맡기나 |
| 허용 소스 | 볼 수 있는 파일·시스템·로그·도구 |
| 실행·리뷰 자세 | 읽기 전용 / 승인된 워크스페이스 안 수정·명령 / 고객 정의 제안 전용 게이트 |
| 소스 우선순위 | 입력이 충돌하면 무엇을 먼저 믿나 |
| 금지 행동 | 하면 안 되는 것 |
| 요약 형식 | 작업·저장소·리비전, 발견·완료한 일, **실제 돌린 검사와 결과**, 미해결, 다음 승인 단계 |
| 대기 조건 | 어떤 결과가 와서 확인돼야 다음 의존 작업이 진행되나 |
| 비용·시간 민감도 | 실무적 한도 |
| 검증 증거 | 필수 검사, 실패·미실행 검사 보고 방식 |
| 에스컬레이션 기준 | 입력 누락·지시 충돌·검사 실패·도구 불가·경계 변경 시 멈추고 지정 리뷰어에게 |
| 최종 리뷰 지점 | 누가 결과를 합치고, 일관성을 확인하고, 충돌을 풀고, 다음 단계를 승인하나 |
| 위임 이유 | 규모·속도·조사 폭·리뷰 품질·반복성 중 무엇을 개선하나 |

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">👍 좋은 subagent 후보</span>
    <ul>
      <li>읽기 위주 탐색, 테스트 조사, 로그 분석</li>
      <li>코드 리뷰 패스, 보안·유지보수성 스캔</li>
      <li>대량 소스 요약, 독립 가설 병렬 진단</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">👎 나쁜 subagent 후보</span>
    <ul>
      <li>깔끔하게 쪼갤 수 없는 작업</li>
      <li>병렬 수정 충돌 가능성</li>
      <li>요약 계약·소스 경계·통합 담당자 없음</li>
    </ul>
  </div>
</div>

### 비동기 워크플로우 체크포인트

체크포인트는 증거를 돌려주고, 리뷰를 위해 멈추고, 승인을 요청하고, 계속해도 되는지 확인하는 **의도된 지점**이에요. 이게 비동기 작업을 **관찰 가능**하게 만들어요.

<ol class="oa-flow">
  <li><strong>출시 전</strong>트리거·context·담당자·행동 경계·승인 자세. cloud면 환경 소유자, CLI면 버전·모델·권한</li>
  <li><strong>소스 수집 후</strong>본 소스·로그·파일, 빠진 context</li>
  <li><strong>경계를 넘는 행동 전</strong>워크스페이스 밖 수정, 비신뢰 명령, 네트워크·외부 도구, 범위 확장</li>
  <li><strong>검증 후</strong>테스트 출력, diff 요약, 위험, 실패·누락 증거</li>
  <li><strong>merge·핸드오프 전</strong>이어받을 리뷰어, 작업·리비전·발견·실제 검사·미실행·남은 일</li>
</ol>

**요구사항이 바뀔 때**: 명시적으로 대체·취소하지 않는 한 정정은 **현재 작업의 업데이트**예요. 여전히 유효한 완료 작업은 유지하고, 영향받는 행동·결과를 찾고, 위임 지시를 갱신하고, 무효화된 것을 다시 확인해요. 통합 담당자가 정정이 적용됐는지 확인한 뒤에 의존 작업을 진행해요. 강의 기준으로 API의 턴 중간 조종(mid-turn steering)은 GPT-6 계열 + Responses API WebSocket에서 지원되며 모든 Codex 클라이언트에서 보장되진 않아요. **입력이 수락·대기열에 들어간 것 ≠ 적용된 것**, 이미 끝난 행동을 되돌리거나 실행 중인 도구를 취소하지도 않아요. 연결이 끊기면 다시 보내기 전에 응답 기록과 대조해요.

### 자동화 준비도 ≠ 위임 준비도

위임 작업은 한 번, 가까운 감독 아래 돌 수 있지만, 자동화는 **반복적으로·예약으로·외부 이벤트로** 돌아요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">목적·패턴</span>자동화가 무엇을 개선? 왜 수동 트리거로는 부족? 무엇이 시작? 어떤 패턴?</div>
  <div class="oa-card"><span class="oa-card__icon">🧱</span><span class="oa-card__title">런타임·경계</span>어디서 돌고, 어떤 context·행동이 허용되고, 어떤 샌드박스·승인 한도?</div>
  <div class="oa-card"><span class="oa-card__icon">🧾</span><span class="oa-card__title">증거·리뷰</span>어떤 검증 증거, 어디서 사람 리뷰, 무엇을 기록·추적?</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🛑</span><span class="oa-card__title">소유·확장</span>누가 출력·건강을 모니터링, 어떻게 멈추고 롤백, 확장 전 필요한 증거</div>
</div>

> **반복된다고 자동화 준비가 된 건 아니에요.** 트리거·경계·검증·리뷰·모니터링·롤백(비활성화) 경로가 분명할 때만 자동화를 추천해요.

## 5단계 — 통제된 통합 추천서와 핸드오프

### 추천서의 4개 질문

| 질문 | 담을 것 |
|---|---|
| 어떤 워크플로우 필요를 푸나? | 워크플로우 필요, 트리거, (해당 시) 자동화 유형, 메커니즘, "이럴 때 쓴다" 근거, 주된 개선 |
| Codex는 어디서 일하고 어떤 한도가 있나? | 런타임 위치, context 소스, 행동 경계, 샌드박스·승인 정책, MCP 정책, hook·rule·requirements·managed config |
| 작업을 어떻게 신뢰·리뷰하나? | 검증 증거, 담당자, 리뷰어, 승인 지점, 핸드오프 기대, 추적할 활동 |
| 무엇을 미루고, 언제 확장하나? | 롤백·에스컬레이션, 성공 신호, **아직 추천하지 않는 컴포넌트와 이유**, 확장 전 필요한 증거 |

강한 추천은 **선택과 절제** 둘 다 설명해요. 예: "반복 CI 실패엔 프로젝트 가이드 + 재사용 조사 Skill. 외부 CI context가 필요할 때만 MCP를 추가하고 소스 승인·도구 범위는 따로 정의. 수정·명령·네트워크·확장 행동에 샌드박스·승인 정책과 고객 리뷰 게이트 설정. 테스트 증거와 리뷰어 지정. 트리거·모니터링 담당·리뷰 주기·비활성화 경로가 검증되기 전엔 자동화하지 않음."

### 통제 기준선: 샌드박스 vs 승인 정책

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🧱 샌드박스 모드 — 기술적으로 무엇이 가능한가</span>
    <ul>
      <li>읽기만? 워크스페이스 안 쓰기?</li>
      <li>네트워크 접근?</li>
      <li>보호할 위치·환경은?</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✋ 승인 정책 — 언제 먼저 물어야 하나</span>
    <ul>
      <li>샌드박스를 벗어날 때, 네트워크, 신뢰 목록 밖 명령</li>
      <li>누가 승인 프롬프트를 보고, 무엇을 검사하나</li>
      <li>무엇을 자동 거절하나, 경계를 넘으면 어떻게 되나</li>
    </ul>
  </div>
</div>

강의 기준으로 로컬 Codex의 Auto 프리셋은 작업 디렉터리 안에서 읽기·수정·명령을 별도 승인 없이 할 수 있고, 워크스페이스 밖 수정이나 네트워크처럼 정책·경계가 요구할 때 승인을 받아요. **제안 전용 리뷰는 네이티브 모드가 아니라 고객 정의 게이트**예요. 각 통제는 워크플로우 위험에 연결해요(MCP → 서버·도구 범위·소스 승인·데이터 경계, 반복 → 트리거 통제·모니터링 담당·비활성화 경로, hook → 이벤트·담당자·실패 동작).

### Trade-off를 숨기지 않기

| Trade-off | 질문 |
|---|---|
| 속도·병렬·자동화 vs 책임 있는 리뷰 | 사람 리뷰, subagent 결과 통합, 모니터링, 롤백은 어디서? |
| 풍부한 외부 context vs 데이터 노출·과부하 | 꼭 필요한 context는? 범위 밖은? 어떤 허용 목록·승인 경계? |
| 넓은 도구 접근 vs 명령·행동 안전 | 허용 / 금지 / 승인 필요 명령은? |
| 재사용 Skill·공유 기본값 vs 소유·예외 | 누가 Skill을 유지·갱신·폐기하나? 중앙 vs 워크플로우별 요건은? |
| 기능 폭 vs 메커니즘 단순성 | 필요해서인가, 있어서인가? |

해결 안 된 trade-off가 있으면 **더 단순한 메커니즘**을 추천해요.

### 롤아웃·검증 노트 (강의 예시 수치)

같은 CI 조사 10건, 같은 context·수용 기준으로 두 통합 패턴을 비교한 **교육용 가상 수치**예요(제품 가격·기대 성능 아님).

<div class="oa-bars">
  <div class="oa-bar"><span>단순 패턴</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:75%"></span><span class="oa-bar__val">40 크레딧 → 수용 8건 = 5.0/건</span></span></div>
  <div class="oa-bar"><span>병렬 패턴</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:100%"></span><span class="oa-bar__val">60 크레딧 → 수용 9건 ≈ 6.7/건</span></span></div>
  <p class="oa-bars__caption">막대 = 수용된 보고서당 크레딧 (강의의 가상 예시 수치).</p>
</div>

- 공정 비교엔 실패 시도·재시도·위임 단계·최종 종합까지 **모든 사용량**을 포함하고, 도구 사용·캐시/비캐시 입력·대기·실행·리뷰 시간·리뷰어 재작업도 기록해요. 더 많이 써도 결과가 의미 있게 좋거나 빨라지면 가치 있을 수 있지만 **이 워크플로우에서 증명**해야 해요.
- 모델(강의 기준 GPT-6.1 Sol, GPT-6 Astra)과 현재 모델을 같은 조건으로 비교하고, **처리 등급(Standard·Fast·Ultrafast)은 모델이 아니라 별도 선택**이에요. 구독 사용량과 API 요금은 서로 바꿔 쓸 수 없어요.
- 크레딧은 사용 단위예요. 과금을 이 워크플로우에 귀속할 수 있고 수용 건이 1건 이상일 때만 건당 비용을 계산해요. **대시보드·알림은 지출 한도를 강제하지 않아요.**
- 작은 파일럿으로 시작해, 합의된 증거가 충족되고 지정 리뷰어가 승인할 때만 확장. 아니면 **유지하거나 좁혀요.**

### 핸드오프 전 결정 순서 점검

<ol class="oa-flow">
  <li><strong>진단</strong>일회성을 과잉 설계하지 않았나</li>
  <li><strong>메커니즘 선택</strong>각각 무엇을 개선하는지 이름 붙임</li>
  <li><strong>통제·소유·검증·에스컬레이션</strong>메커니즘만으로는 미완성</li>
  <li><strong>위임·자동화 리뷰 가능성</strong>체크포인트·요약 계약·모니터링</li>
  <li><strong>파일럿 준비</strong>범위·담당·트리거·리뷰 주기·성공 신호·확장 논리</li>
</ol>

핸드오프가 위험해지는 순간은 **메커니즘 이름은 남았는데 워크플로우 필요·담당자·통제·리뷰 경로가 사라질 때**예요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 짝 맞추기 — 렌즈·도구가 결정을 돕는 것</summary>
  <p>Context → Codex에 필요한 가이드·승인된 소스·저장소·도구·외부 시스템<br>Agentic runtime → 어디서 어떻게 실행하나(승인·샌드박스 자세 포함)<br>Observability → 보여야 할 증거·활동·소유권·에스컬레이션·롤백 신호<br>통합 진단 → 트리거·context·행동 경계·통제·검증·핸드오프·관찰 경로가 분명한가<br>컴포넌트 정당화 테스트 → 메커니즘이 복잡성을 정당화할 만큼 반복성·거버넌스·관찰성·확장성을 높이나</p>
</details>
<details class="oa-quiz">
  <summary>Q2. 같은 유형의 CI 실패를 반복 조사하는데 과정이 들쭉날쭉하고 리뷰어는 검증 증거가 부족하다. 가장 좋은 출발점은?</summary>
  <p><b>정답: 재사용 가이드나 반복 가능한 조사 절차부터.</b> (모든 실패에 외부 오케스트레이션 ✗ / 모든 조사에 subagent ✗ / 프로덕션 아니니 일회성 프롬프트 유지 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q3. 개발자가 열어둔 로컬 파일 하나의 설명을 원하고, 바로 검토할 수 있고, 팀 절차도 필요 없다. 추천은?</summary>
  <p><b>정답: 일회성 Codex 상호작용으로 유지.</b> (스케줄 ✗ / 외부 도구 ✗ / Skill 만들기 ✗ — 과잉 설계)</p>
</details>
<details class="oa-quiz">
  <summary>Q4. 짝 맞추기 — 워크플로우 필요 ↔ 초기 메커니즘·개선점</summary>
  <p>지속 저장소 규칙·반복 기대치 → <b>AGENTS.md / 프로젝트 가이드; 반복성</b><br>라이프사이클 검증 검사·승인 게이트 → <b>Hooks·rules·requirements·managed config; 거버넌스·관찰성</b><br>재사용 지시·묶음 참고자료·표준 출력 → <b>Skill; 반복성·확장성</b><br>사람이 결과를 통합하는 제한된 병렬 조사 → <b>Subagents; 제한된 작업의 확장성</b></p>
</details>
<details class="oa-quiz">
  <summary>Q5. 깔끔히 쪼갤 수 없는 대규모 리팩터에 subagent를 쓰려 하고, 결과 통합 담당자도 없다. 추천은?</summary>
  <p><b>정답: 위임 전에 작업 경계를 명확히 하고 통합 담당자를 지정한다.</b> (반복 자동화 ✗ / 병렬로 밀어붙이기 ✗ / 외부 도구 추가 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q6. 트리거는 승인됐지만 리뷰 담당자도 비활성화 경로도 없는 반복 문서 검사를 스케줄하려 한다. 추천은?</summary>
  <p><b>정답: 리뷰 소유권과 비활성화 경로가 정해질 때까지 스케줄링을 미룬다.</b> (트리거 승인만으로 스케줄 ✗ / subagent로 대체 ✗ / 영원히 수동 ✗)</p>
</details>

## 정리

- 고급 통합은 기능 묶음이 아니라 **반복 가능하고 통제된 워크플로우 설계**다. 컴포넌트는 반복성·거버넌스·관찰성·확장성을 높일 때만 자리를 얻는다.
- 먼저 **통합 진단 7문항**(Trigger·Context·Action·Control·Validation·Handoff·Observability)으로 일회성인지 반복인지 판단한다. 복잡함 ≠ 파편화, 반복 ≠ 자동화.
- 메커니즘은 서로 대체되지 않는다: AGENTS.md는 강제 계층이 아니고, MCP는 거버넌스가 아니며, managed config는 접근을 부여하지 않고, hook·rule은 샌드박스·승인·사람 리뷰를 대신하지 않는다.
- 위임은 **위임 계약**(경계·소스·금지 행동·요약 형식·검증·에스컬레이션·최종 통합 담당자)으로, 비동기는 **체크포인트**로, 자동화는 트리거·모니터링·롤백이 증명된 뒤에.
- 추천서는 **선택과 절제**를 함께 설명한다: 샌드박스와 승인 정책을 구분하고, trade-off를 드러내고, 모든 시도를 포함한 사용량 대비 수용 결과로 확장 여부를 정한다.
