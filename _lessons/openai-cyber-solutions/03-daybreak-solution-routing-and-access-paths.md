---
title: "Daybreak solution routing and access paths"
title_ko: "Daybreak 솔루션 라우팅과 접근 경로 — 워크플로우를 먼저, 접근은 나중에"
course: openai-cyber-solutions
lesson: 3
---

## 학습 목표

- 고객의 **승인된 방어 워크플로우**에서 출발해 사이버 기회를 라우팅하기
- AppSec·secure SDLC 기회에서 **로컬 Codex Security · Codex Security Cloud · 집중 변경 리뷰**를 구분하기 (강의 기준)
- **승인된 표준 모델 경로**로 충분한 때를 설명하기
- **Blue / Red** 승인 경계, 승인된 내부 사용, 별도의 **파트너 서비스 경로** 알아보기
- 사이버 특화 모델 접근 요청과 **에스컬레이션이 필요한 민감 맥락** 식별하기
- 사이버 특화 접근·거절 감소·ZDR·가격·특별 가용성·내부 등급·배포 경로에 대한 **근거 없는 주장 피하기**

## 지난 두 레슨과 이어서 보기

레슨 1에서는 **왜 지금 사이버인가, Daybreak를 어떻게 말할까**를, 레슨 2에서는 **이 기회가 진짜인가, 누가 무엇을 원하나**를 배웠어요. 이번 레슨은 그 결과물(검증 스냅샷)을 받아 **어떤 경로로 보낼지** 결정해요.

<div class="oa-svg">
<svg viewBox="0 0 720 270" role="img" aria-label="레슨 1, 2, 3을 잇는 흐름">
  <defs>
    <style>.r1{fill:#f3f0ff;stroke:#d6ccf5;stroke-width:1.5}.r2{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.r3{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.rh{font-size:14px;fill:#4a4a4a;font-weight:700}.rt{font-size:11px;fill:#6a6a6a}.ro{font-size:11px;fill:#4a4a4a;font-weight:700}.rar{stroke:#9a9a9a;stroke-width:1.5;fill:none}</style>
  </defs>
  <rect x="10" y="10" width="220" height="190" rx="12" class="r1"/>
  <text x="120" y="36" text-anchor="middle" class="rh">레슨 1 · 포지셔닝</text>
  <text x="120" y="62" text-anchor="middle" class="rt">사이버 = 속도 문제</text>
  <text x="120" y="82" text-anchor="middle" class="rt">find → validate → test</text>
  <text x="120" y="98" text-anchor="middle" class="rt">→ remediate → prove</text>
  <text x="120" y="118" text-anchor="middle" class="rt">Daybreak 3개 층 + 가드레일</text>
  <text x="120" y="138" text-anchor="middle" class="rt">워크플로우 → 접근 → 자동화</text>
  <rect x="30" y="156" width="180" height="30" rx="8" class="r2"/><text x="120" y="176" text-anchor="middle" class="ro">산출물: 워크플로우 메시지</text>
  <path d="M232 105 L248 105" class="rar"/><path d="M242 99 L248 105 L242 111" class="rar"/>
  <rect x="250" y="10" width="220" height="190" rx="12" class="r1"/>
  <text x="360" y="36" text-anchor="middle" class="rh">레슨 2 · 검증</text>
  <text x="360" y="62" text-anchor="middle" class="rt">3가지 진단 질문</text>
  <text x="360" y="82" text-anchor="middle" class="rt">5개 페르소나 렌즈</text>
  <text x="360" y="102" text-anchor="middle" class="rt">신호원·기록 시스템·실행 surface</text>
  <text x="360" y="122" text-anchor="middle" class="rt">병목 → 가설 → 증명 지점</text>
  <text x="360" y="142" text-anchor="middle" class="rt">적합 / 확인 필요 / 에스컬레이션</text>
  <rect x="270" y="156" width="180" height="30" rx="8" class="r2"/><text x="360" y="176" text-anchor="middle" class="ro">산출물: 검증 스냅샷</text>
  <path d="M472 105 L488 105" class="rar"/><path d="M482 99 L488 105 L482 111" class="rar"/>
  <rect x="490" y="10" width="220" height="190" rx="12" class="r3"/>
  <text x="600" y="36" text-anchor="middle" class="rh">레슨 3 · 라우팅</text>
  <text x="600" y="62" text-anchor="middle" class="rt">승인된 워크플로우 6가지 사실</text>
  <text x="600" y="82" text-anchor="middle" class="rt">Codex Security 먼저</text>
  <text x="600" y="102" text-anchor="middle" class="rt">표준 경로로 충분한가</text>
  <text x="600" y="122" text-anchor="middle" class="rt">Trusted Access는 필요할 때만</text>
  <text x="600" y="142" text-anchor="middle" class="rt">민감 요청은 SME로</text>
  <rect x="510" y="156" width="180" height="30" rx="8" class="r2"/><text x="600" y="176" text-anchor="middle" class="ro">산출물: 라우팅 추천 노트</text>
  <text x="360" y="232" text-anchor="middle" class="rt">세 레슨을 관통하는 한 줄: 승인된 방어 워크플로우 → 범위 → 리뷰어 → 증거 → (그다음) 접근</text>
  <text x="360" y="252" text-anchor="middle" class="rt">사람이 수용·merge·릴리스·기록을 결정한다</text>
</svg>
</div>

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>라우팅 원칙</strong>접근 경로보다 워크플로우 먼저</li>
  <li><strong>Codex Security 우선</strong>AppSec·secure SDLC</li>
  <li><strong>표준 경로</strong>충분할 때가 많다</li>
  <li><strong>Trusted Access</strong>적절할 때만</li>
  <li><strong>에스컬레이션</strong>특화 접근·민감 맥락</li>
  <li><strong>라우팅 추천</strong>5부 패턴 + 이유</li>
</ol>

## 1단계 — 접근 경로가 아니라 워크플로우부터

<div class="oa-eli">
  <span class="oa-eli__head">🚕 쉽게 말하면</span>
  <p>택시를 탄 손님이 "제일 빠른 고속도로로 가 주세요!"라고 해도, 기사는 먼저 <b>"어디 가세요?"</b>라고 물어요. 목적지가 바로 옆 동네면 고속도로는 오히려 돌아가는 길이죠. "가장 강한 사이버 모델 주세요"도 목적지(워크플로우)를 묻기 전엔 답할 수 없어요.</p>
</div>

**솔루션 라우팅** = 검증된 고객 워크플로우에 가장 안전하고 관련 있는 다음 단계를 고르는 과정.

> **접근 경로를 논하기 전에 워크플로우를 라우팅하라.**

"사이버 모델 접근", "거절 감소", "Trusted Access", "가장 고급 사이버 모델" 요청은 **추천이 아니라 워크플로우를 확인하라는 신호**예요. 경로는 레슨 1에서 본 세 가지 운영 모델 렌즈로 판단해요:

| 렌즈 | 질문 | 예 |
|---|---|---|
| Context | AI가 알거나 써도 되는 것? | 저장소, 스캐너 결과, 티켓, 정책, 로그, 문서, 리뷰어 노트 |
| Agentic runtime | 어디서 어떻게 일하나? | Codex, Codex Security, 승인 환경, 저장소 워크플로우, 샌드박스, 리뷰 surface |
| Observability | 어떻게 감시·리뷰·기록·개선하나? | 증거, 리뷰어 결정, 감사 추적, 기록 시스템, 에스컬레이션 경로 |

### 승인된 방어 워크플로우 — 최소 6가지 사실

| # | 사실 | 확인 질문·예 |
|---|---|---|
| 1 | 방어 목적 | 무엇을 보호·개선하나? (finding 검증, 백로그 우선순위, 보안 코드 리뷰, 수정 증거, 개발자 인계) |
| 2 | 범위 있는 자산·워크플로우 | 저장소·서비스·앱·백로그 조각·취약점 유형·PR 패턴·하위 환경 중 하나 |
| 3 | 소유·승인 | 누가 소유하고, 누가 승인 권한이 있으며, 대상 검사·분석이 명시적으로 허가됐나? |
| 4 | 사람 리뷰어 | finding·수정·증거·보고서를 누가 리뷰하나? |
| 5 | 증거 기대 | 무엇이 개선의 증거인가? |
| 6 | 민감도·에스컬레이션 | 특화 모델 접근, 거절 감소, 운영 테스트, 실서비스 대상, 악성코드, 리버스 엔지니어링, 레드팀, 핵심 인프라, 특별 데이터 처리·ZDR, 가격, 특별 가용성? |

하나라도 불명확하면 기회는 유망해도 **경로는 아직 준비 안 됨** → 다음 단계는 접근 약속이 아니라 **확인**. 그리고 이 6가지는 **작업 경계**일 뿐, 제품 접근 활성화가 아니에요. 실행 추천 전에 의도한 사용자의 surface·모델, 저장소 연결, 운영 준비도 따로 확인하고, 빠진 확인은 담당자와 함께 기록해요.

### 라우팅 스냅샷

레슨 2의 검증 스냅샷을 **결정 보조 도구**로 확장한 것이에요. 전체 솔루션 설계가 아니에요.

| 항목 | 기록할 내용 |
|---|---|
| 고객 워크플로우 / 보안 신호 / 병목 | 어떤 방어 프로세스? 일은 어디서 시작? 어디서 느려지나? |
| 범위 / 리뷰어 / 증명 지점 | 범위·승인된 자산 · 평가자 · 개선 증거 |
| 민감도 | 접근·운영·공격 중심·데이터 처리·외부 대상·에스컬레이션 우려 |
| 추천 경로 | 제품 surface와 맞는 이유. 접근 승인·확인·SME 지원은 **따로** 기록 |
| 접근 증거 | 승인된 사용자·워크스페이스/프로젝트, 모델·surface, 아직 확인할 승인·활성화 |
| 운영 준비 | 저장소 연결, 사용·관리 권한, 리뷰 여력, 예산, 중단 담당자 |
| 다음 결정 | 빠진 증거, 책임자, 작업 전 다음 행동 |

**예시 — "사이버 모델 쓸 수 있나요?"로 시작한 고객**: ❌ 모델명·등급·거절·가용성으로 바로 점프. ✅ "접근 경로를 과장하고 싶지 않아요. 개선하려는 방어 작업부터 볼까요?" → 알고 보니 소유 앱 저장소, 스캐너 백로그, 증거 약한 티켓을 엔지니어링이 불신, AppSec 리드가 리뷰 가능, 공격 테스트·특별 접근 필요는 아직 없음. → "사이버 모델 접근"이 **"AppSec 백로그 검증·증거 생성을 위한 범위 있는 방어 워크플로우"**로 재정의되고, **Codex Security + 승인된 표준 경로**로 시작할 수 있어요.

## 2단계 — AppSec·secure SDLC는 Codex Security부터

<div class="oa-eli">
  <span class="oa-eli__head">🔬 쉽게 말하면</span>
  <p>실력 좋은 과학자(모델)만 있다고 실험이 되진 않아요. 시약·장비·실험 노트·검토하는 교수님이 있는 <b>실험실</b>이 있어야 결과를 믿을 수 있죠. Codex Security는 "모델을 고르는 일"이 아니라, 코드·도구·테스트·finding·사람 리뷰가 있는 <b>실험실(워크플로우 surface)</b>을 고르는 일이에요.</p>
</div>

Codex Security가 실질적 첫 경로가 되는 경우: 문제가 **저장소 기반 · 방어적 · 범위 있음 · 리뷰어 소유**일 때. 가치는 "고객이 사이버 모델을 얻는다"가 아니라, **코드·도구·테스트·finding·보고서·수정·사람 리뷰가 있는 곳에 모델 추론을 가져온다**는 것이에요. 스캐너·AppSec·엔지니어링 리뷰·거버넌스·기록 시스템을 대체하지 않아요.

### 적합 신호

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📁</span><span class="oa-card__title">소유 코드·범위 있는 자산</span>저장소, 앱, 서비스, PR, 패키지, 코드 조각</div>
  <div class="oa-card"><span class="oa-card__icon">📡</span><span class="oa-card__title">소프트웨어 작업과 연결된 신호</span>SAST, SCA, 의존성 스캔, 코드·PR 리뷰, 백로그</div>
  <div class="oa-card"><span class="oa-card__icon">🧐</span><span class="oa-card__title">검증 마찰</span>진짜·도달·악용 가능·중복·가치 판단이 어려움</div>
  <div class="oa-card"><span class="oa-card__icon">🤝</span><span class="oa-card__title">개발자 인계 문제</span>수정 가이드·재현 노트·위치·테스트 기대 부족</div>
  <div class="oa-card"><span class="oa-card__icon">🧾</span><span class="oa-card__title">증거 공백</span>감사·보고·기록 시스템·리뷰어 신뢰용 증거</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">👤</span><span class="oa-card__title">사람 리뷰 가능</span>이름 있는 리뷰어가 finding·수정·다음 결정을 평가</div>
</div>

### 고객의 '결정'에 맞춰 surface 고르기 (강의 기준)

신호가 어디서 나타났는지가 아니라 **고객이 내려야 할 결정**으로 골라요.

| surface | 맞는 상황 |
|---|---|
| **로컬 Codex Security 플러그인** | 고객 로컬 환경에서 코드·finding으로 조사 |
| **Codex Security Cloud** | 연결된 GitHub 저장소를 클라우드에서 스캔 + 새 커밋 감시. 기존 코드베이스의 넓은 평가와 지속 관리. 리뷰 후 제안 패치·draft PR 가능, 수용·merge·릴리스는 고객 |
| **GitHub Code Review / Security Review** | 제안된 변경 하나를 검토. Security Review(research preview)는 변경과 관련 저장소 맥락을 더 깊게 보안 관점으로 리뷰. 저장소 평가를 보완 |

Cloud도 새 커밋을 리뷰할 수 있으니 **"변경이 생겼다"는 트리거가 경로를 정하거나 더 넓은 스캔을 승인하지 않아요.** 1회 리뷰와 지속 커밋 감시는 같은 제품이라도 **운영 약속이 달라요.**

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 과장된 말</span>
    <ul>
      <li>"스캐너를 대체합니다." / "AppSec 팀을 대체합니다."</li>
      <li>"수정을 자동으로 배포합니다."</li>
      <li>"사람 리뷰가 필요 없어요."</li>
      <li>"모든 취약점을 찾는다고 보장해요."</li>
      <li>"사이버 특화 모델 접근이 필요해요."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 라우팅 깊이의 안전한 말</span>
    <ul>
      <li>"AppSec·secure SDLC엔 Codex Security가 보통 현실적 출발점이에요."</li>
      <li>"저장소 맥락 분석, finding 검증, 증거 생성, 리뷰 가능한 수정을 지원할 수 있어요."</li>
      <li>"심각도 판단·수정 수용·최종 리뷰는 사람 팀의 책임이에요."</li>
      <li>"기존 스캐너·워크플로우를 보완해요."</li>
    </ul>
  </div>
</div>

스캔 유형·CI/CD 통합·수정 워크플로우·설정 같은 깊은 내용은 이후 과정 몫이에요. 여기선 **경로를 알아보는 것**까지.

### Codex Security만으로 부족할 때

- 코드·저장소·AppSec·SDLC·개발자 인계와 무관한 워크플로우
- 범위·리뷰 관문·거버넌스 전에 운영 자동화, CI/CD 강제, 맞춤 harness·API 게이트웨이·통합, 담당자·통제 없는 반복 작업을 원함
- 레드팀, 모의해킹, 공격 중심, 악성코드, 리버스 엔지니어링, 실서비스 대상, 핵심 인프라
- 사이버 특화 모델 접근, 거절 감소, ZDR, 가격, 특별 가용성, 내부 등급 질문
- 이름 있는 리뷰어·수정 수용 담당자 없음, 자산 소유·승인 불명확
- 명시적 승인·가드레일 없이 외부 대상·운영 시스템 테스트

> 기본 Cloud 감시는 그 자체로 **예외적 모델 접근 요청이 아니에요.** 일반 활성화·권한은 고객 관리자·저장소 소유자에게 묻고, 자격·민감 작업·맞춤 통합·승인이 불확실할 때 OpenAI SME를 불러요. 그 공백을 접근 약속으로 바꾸지 않아요.

**예시 — 소유 저장소의 AppSec 백로그**: SAST·의존성 finding, 엔지니어링은 맥락 부족, AppSec은 검증·증거가 느림, CISO는 위험 감소 증거 원함. → 방어적 · 저장소 기반 · 범위 가능 · 신호 존재 · 병목은 검증·증거 · 리뷰어 있음 · 증명 정의 가능 → **강한 Codex Security 신호.** "소유 저장소 1개나 백로그 조각 1개에서 범위 있는 Codex Security 워크플로우로 시작하고, 특별 접근 논의 전에 AppSec 리뷰어·증거 기대·개선 기준을 확인하죠."

## 3단계 — 승인된 표준 모델 경로로 충분할 때

<div class="oa-eli">
  <span class="oa-eli__head">🚲 쉽게 말하면</span>
  <p>학교에 가는 데 스포츠카가 필요하진 않아요. 자전거가 <b>더 안전하고 딱 맞는</b> 선택일 때가 많죠. 자전거가 "덜 좋은 차"가 아니라 그 길에 맞는 탈것이에요. 표준 경로도 "타협안"이 아니라 많은 방어 작업에 맞는 출발점이에요.</p>
</div>

**승인된 표준 모델 경로** = 일반 기업 방어 워크플로우에 쓸 수 있는 현재 OpenAI 승인 모델·제품 경로. GPT-6 Astra도 가용하고 적절하면 표준 경로에 포함될 수 있어요. Daybreak Blue에서는 표준 안전장치를 유지하고, 거절 감소는 Red 승인 + 활성화가 필요해요 (강의 기준). **모델을 선택할 수 있다는 것이 승인이나 고객 자산 작업 허가를 뜻하지 않아요.**

좋은 라우팅 질문: **"추가 접근 고려 없이, 승인된 표준 경로로 이 워크플로우를 안전하고 효과적으로 지원할 수 있는가?"**

### 표준 경로가 충분한 조건

| 조건 | 의미 |
|---|---|
| 명확히 방어적 | 고객 소유 시스템 보호, AppSec 품질 개선, 보안 리뷰, 고객 결정용 증거 |
| 범위 있음 | 특정 저장소·백로그 조각·코드 리뷰·의존성 목록·문서 |
| 공격 중심 아님 | 익스플로잇 개발, 외부 대상, 악성코드, 리버스 엔지니어링, 레드팀 자동화, 운영 테스트 요청 없음 |
| 사람 리뷰 있음 | 유효성·수정 수용·증거 보존을 고객 리뷰어가 결정 |
| 접근 마찰 아직 없음 | 정당한 승인 작업이 사이버 관련 마찰로 막힌 적이 없음 |
| 라우팅 깊이 | 첫 경로 추천이지, 전체 배포·맞춤 통합·CI/CD 자동화 설계가 아님 |

표준 경로로 시작하기 좋은 작업 예: 소유 저장소의 의존성 스캐너 결과 요약 · 백로그 조각의 리뷰어용 증거 요약 · 관련성·증명 공백별 finding 정리 · 사람 리뷰용 개발자 수정 노트 초안 · 보안 코딩 가이드·위협 모델 문서 검토 · 고급 접근 요청 전 범위 있는 평가 계획 수립.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 피할 말</span>
    <ul>
      <li>"이건 사이버 특화 모델이 필요해요."</li>
      <li>"표준 모델은 사이버에 안 맞아요."</li>
      <li>"자동으로 Trusted Access 대상이에요."</li>
      <li>"거절 감소·특별 접근을 보장해요."</li>
      <li>"표준 경로는 기초 작업용이에요."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 쓸 말</span>
    <ul>
      <li>"범위 있는 방어 워크플로우 + 승인된 표준 경로가 맞는 출발점일 수 있어요."</li>
      <li>"추가 접근 논의 전에 기존 승인 역량으로 되는지 먼저 보죠."</li>
      <li>"현재 승인 모델·제품 경로는 공식 OpenAI 가이드로 확인해요."</li>
      <li>"접근 경로는 워크플로우 필요를 따라가요."</li>
      <li>"나중에 정당한 마찰이 생기면 기록하고 Trusted Access 관련성을 평가해요."</li>
    </ul>
  </div>
</div>

강의 연습은 **워크플로우 surface와 접근 자세를 따로** 추천하라고 해요. 예를 들어 기존 승인 모델 경로가 Codex Security 워크플로우를 지원할 수 있으므로, "제품 선택 / 승인 / 에스컬레이션"을 서로 배타적인 경로로 보지 않아요.

## 4단계 — Trusted Access는 적절할 때만

<div class="oa-eli">
  <span class="oa-eli__head">🎫 쉽게 말하면</span>
  <p>놀이공원 <b>자유이용권(Blue)</b>과 특정 놀이기구의 <b>특별 체험권(Red)</b>은 따로 신청해요. 자유이용권이 있다고 특별 체험권이 생기지 않고, 내 표를 친구에게 넘겨줄 수도 없어요. 그리고 표가 있어도 <b>남의 집 마당에서 놀아도 된다는 허락</b>은 아니죠.</p>
</div>

**Trusted Access for Cyber (= Daybreak Access)** — 승인 기반 프로그램 (강의 기준)

| 구분 | 내용 |
|---|---|
| Blue | 대부분의 승인된 방어 작업 출발점 |
| Red | 고급 승인 연구·테스트용 **별도 승인**. Blue 승인 ≠ Red |
| Security Cloud 포함 Blue | 별도 신청 없이 **그 제품 안에서만**. API·다른 surface 접근 아님 |
| 매칭 원칙 | 승인은 의도한 사용자·워크스페이스/API 프로젝트·모델·전달 surface와 일치해야 함. **승인과 활성화는 별개 확인** |
| 내부 사용 | 일반 Trusted Access는 승인된 내부 작업 한정. 외부 고객을 섬기는 조직도 승인된 내부 전용 API 프로젝트·사용자로 제한 가능 (조직 전체가 내부 전용일 필요는 없음). 외부 사용자·다운스트림 고객 트래픽으로 확장 금지 |
| 고객 대상 서비스 | **Daybreak Cyber Partner Program** 별도 승인. 파트너가 모델 승인을 갖고 승인된 범위에서 운영. 모델 접근은 **고객에게 이전되지 않음** |

### Trusted Access가 아닌 것

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">보장이 아님</span>사이버 특화 모델 접근, 모든 경우의 거절 감소, 무제한 사이버 역량</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">우회로가 아님</span>OpenAI 정책·안전장치 우회, 워크플로우 범위·승인·사람 리뷰 생략 이유</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">상업 조건이 아님</span>ZDR·특별 데이터 처리, 특별 가격, 특별 가용성, 고객용 내부 등급 체계</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">전달 권리가 아님</span>제3자·외부 사용자·다운스트림 트래픽 전달, 재판매·프록시·제품 내장</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">대체재가 아님</span>고객의 보안·법무·개인정보·컴플라이언스·아키텍처 리뷰</div>
</div>

### 언제 관련 있어지나

먼저 이해할 것: 방어 워크플로우 · 고객 소유/승인 자산 · 승인 환경 · 리뷰어 · 증거 필요 · 요청 접근이 워크플로우에 맞는 이유와 **선택한 제품이 이미 포함하는 것** · 민감 접근·데이터·거버넌스 우려.

- 일반 방어 작업엔 Blue, 고급 승인 연구·테스트는 별도의 Red 결정.
- Cloud 워크플로우라면 **포함된 Blue와 운영 준비**를 확인하지, 불필요한 별도 신청을 만들지 않아요.
- **거절이나 막힌 작업 하나가 곧 다른 등급이 맞다는 증거는 아니에요.**
- OpenAI 공개 심사는 신원·신뢰 검증, 위험, 의도한 사용 사례, 더 넓은 보안 생태계 강화 능력 등을 고려할 수 있고 **승인은 자동이 아니에요.**
- 핵심 표현: **"관련 있을 수 있다(may be relevant)"**, "제공될 것이다(will be provided)"가 아님.

### "Astra를 고를 수 있으니 고급 사이버 접근이 있는 거죠?" — 세 질문으로 분리

<ol class="oa-flow">
  <li><strong>모델 가용성·접근 승인</strong>그 사용자·surface에 어떤 승인? Blue는 표준 안전장치</li>
  <li><strong>작업 승인</strong>그 대상·워크플로우의 사이버 활동이 허가됐나? 모델 선택 ≠ 작업 허가</li>
  <li><strong>데이터 처리 조건</strong>이 제품·계정·워크플로우의 조건은? 모델 접근에 ZDR이 따라온다고 가정 금지</li>
</ol>

**예시 — 승인된 방어자가 사이버 관련 마찰을 만남**: 소유 저장소, 승인된 하위 환경, 정의된 검증 워크플로우, 리뷰어, 기록 시스템이 모두 있는데 정당한 방어 분석이 마찰로 막힘. ❌ "Trusted Access로 해결돼요. 받아드릴게요." ✅ "이 승인된 방어 워크플로우에 사이버 관련 마찰 감소가 필요하다면 Trusted Access가 관련 있을 수 있어요. 워크플로우·승인·자산·리뷰어·환경·구체적 마찰을 기록하고 승인된 OpenAI 절차로 적절한 경로를 확인하죠."

## 5단계 — 사이버 특화 접근과 민감 맥락은 에스컬레이션

<div class="oa-eli">
  <span class="oa-eli__head">🏥 쉽게 말하면</span>
  <p>동네 병원 의사가 복잡한 수술이 필요해 보이면 <b>진료 의뢰서</b>를 써서 큰 병원으로 보내요. 의뢰서에 증상·검사 결과·모르는 것을 잘 적어야 다음 의사가 처음부터 다시 묻지 않죠. 에스컬레이션 노트가 바로 그 의뢰서예요. "민감하다 = 불가능하다"가 아니라 <b>"신중하게 보낸다"</b>예요.</p>
</div>

사이버 특화 모델 접근은 **예외 기반**이에요. 많은 방어 워크플로우는 Codex Security(첫 surface) → 승인된 표준 경로 → 맞으면 Blue(Cloud 범위 Blue 포함), 고급 연구·테스트는 별도 Red 심사 → 민감·고급·불명확·고위험이면 SME 리뷰 순으로 시작해요. 파트너의 역할은 **접근을 승인하는 게 아니라**, 에스컬레이션이 필요한 요청을 알아보고 SME가 조언할 수 있게 맥락을 기록하는 거예요.

GPT-5.6-Cyber는 모델별 추가 승인과 프로비저닝이 필요해요. Red가 승인됐거나 다른 사이버 모델이 보인다고 활성화됐다고 추론하지 않아요. 요청 모델·surface를 기록해 OpenAI 담당자가 확인하게 하고, 등급 라벨로 그 증거를 대신하지 않아요 (강의 기준).

### 에스컬레이션 트리거

| 범주 | 트리거 |
|---|---|
| 접근 | 사이버 특화 모델 접근 명시 요청, 거절 감소·제거 요청 |
| 작업 유형 | 레드팀, 모의해킹, 공격 중심, 악성코드, 리버스 엔지니어링, 실서비스 대상 |
| 환경 | 가드레일 없는 운영 테스트, 외부 대상·소유 불명확, 핵심 인프라·고위험 |
| 통합 | 맞춤 harness, API 게이트웨이, headless, 깊은 통합, 첫 워크플로우 범위 전 CI/CD 강제·반복 자동화 |
| 조건 | 특별 데이터 처리, ZDR, 가격, 가용성, 배포 경로 주장 |
| 불명확 | 승인·자산 소유·리뷰어 책임·증거 기대 |
| 대외 | 고급 사이버 역량의 고객 대상 사용, 다운스트림·제3자 접근 |

일상적인 Cloud 연결·사용자 권한·스캔 관리 질문은 먼저 **고객 관리자·저장소 소유자**에게. 기본 커밋 감시와 Cloud 포함 Blue는 예외적 접근 경로가 필요하지 않아요.

### 에스컬레이션 노트에 담을 것

- 고객명·이해관계자 역할, 방어 워크플로우·비즈니스 맥락
- 자산·저장소·환경·대상 시스템, 승인 상태와 소유자, 대상이 고객 소유/제3자/외부/운영/민감한지
- 요청된 보안 작업 유형, 표준 경로·Codex Security로 부족할 수 있는 이유
- 실제 사용자의 제품·워크스페이스와 활성 모델 + 추가로 요청된 모델·접근·데이터 처리·배포 경로
- 리뷰어·승인자·기록 시스템, 증거 기대, 데이터 처리 질문
- 열린 질문, 위험 플래그, 고객 약속·일정 압박

그리고 분명히: **아는 것 · 모르는 것 · 왜 민감한지 · 무엇을 약속하지 않았는지 · 어떤 가이드가 필요한지.**

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 피할 말</span>
    <ul>
      <li>"GPT-5.5-Cyber 받아드릴게요." / "사이버 모델 대상이세요."</li>
      <li>"프로그램이 거절을 없애줘요."</li>
      <li>"추가 리뷰 없이 이 운영 테스트 지원돼요."</li>
      <li>"내부 접근 레벨이 이렇게 매핑돼요."</li>
      <li>"승인됐다고 말만 하시면 괜찮아요."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 안전한 에스컬레이션 말</span>
    <ul>
      <li>"고급 접근 세부를 논의하기 전에 OpenAI SME 리뷰가 필요할 수 있어요."</li>
      <li>"워크플로우 검토 전에 접근 경로를 과장하고 싶지 않아요."</li>
      <li>"승인된 워크플로우·대상 자산·리뷰어·증거 필요를 기록해 맞는 팀이 조언하게 하죠."</li>
      <li>"내부·옛 접근 용어 대신 현재 승인 경로를 확인해요."</li>
    </ul>
  </div>
</div>

**예시 — "Astra 결과를 봤으니 외부 자산 대상 레드팀 계획을 시작해도 되죠?"**: 파트너는 전술·접근 등급·거절 감소·자격 여부를 논하지 않아요. "Astra를 선택할 수 있고 평가 결과를 읽은 것이 이 작업의 허가를 만들진 않아요. 의도한 워크플로우, 대상 소유·승인, 환경, 리뷰어, 증거 필요를 기록하고 접근 경로 추천이나 거절 관련 주장 전에 OpenAI SME를 참여시키죠." → 확인 안 된 고급 사용 · 외부 자산 · 공격 중심 가능성 · 거절 감소 기대 · 승인 복잡성 = **에스컬레이션.**

## 6단계 — 안전한 Daybreak 라우팅 추천 만들기

### 5부 패턴

<ol class="oa-flow">
  <li><strong>워크플로우</strong>어떤 승인된 방어 워크플로우를 개선하나</li>
  <li><strong>시작 경로</strong>로컬 조사 / Cloud 저장소 평가 / 집중 변경 리뷰 + 이유</li>
  <li><strong>접근 자세</strong>사용자·프로젝트·모델·surface, Blue/Red/모델 승인과 활성화 구분, 내부 vs 파트너 서비스</li>
  <li><strong>가드레일</strong>약속하지 않을 것 명시</li>
  <li><strong>다음 단계</strong>빠진 증거·담당자·다음 결정, 중단 권한자</li>
</ol>

Cloud 준비 확인 (강의 기준): 고객 플랜이 Pro·Business·Enterprise·Edu인지(research preview), Enterprise/Edu는 Codex Cloud + Codex Security 둘 다 활성화 · Security **사용 권한 ≠ 스캔 설정 관리 권한** · 토큰 기반 과금, 기존 고객은 유료 사용 전 opt-in, 예산 없으면 스캔 일시중지 → 가격 계산이 아니라 **준비 질문**. Amazon Bedrock으로 Daybreak를 고려하면 의도한 모델·전달 경로의 승인·가용성을 따로 확인 — **Codex Security Cloud 접근이 Bedrock으로 자동 확장되지 않아요.**

### 라우팅 노트 — "왜 이 경로인가"를 남기기

라우팅 노트가 답해야 할 질문: 왜 이 경로? 왜 지금 더 고급 접근 경로가 아닌가? 무엇을 아직 확인해야 하나? 누가 리뷰하나? 무엇이 경로가 효과 있다는 증거인가? 어떤 주장을 하면 안 되나? 다음 단계는 누구 담당인가?

| 항목 | 예시 라우팅 노트 (강의 예시 요약) |
|---|---|
| 추천 경로 | 첫 워크플로우 surface로 Codex Security |
| 이유 | 소유 저장소, 스캐너 finding, AppSec 검증 지연, 엔지니어링 신뢰 문제, 이름 있는 AppSec 리뷰어 |
| surface 판단 | 현재 사실은 **범위 있는 조사**를 지지할 뿐 Cloud 시작 확정이 아님. 저장소 제공자와, 넓은 평가·지속 리뷰·집중 변경 중 무엇이 필요한지 확인 |
| 접근 자세 | surface·모델, 승인된 신원·워크스페이스/프로젝트, 활성 접근 기록. Cloud의 Blue는 Cloud 안에서만, 별도 Red·모델 승인은 가정 금지. 내부 사용과 파트너 서비스 구분 |
| 가드레일 | GPT-5.5-Cyber, 거절 감소, ZDR, 가격, 특별 가용성, 자동 수정 약속 금지 |
| 다음 단계 | 저장소 소유자 → 제공자·리뷰 범위 / 관리자 → 연결·사용/관리 권한·예산 / AppSec 리드 → 리뷰 여력·증거 목적지·지속 작업 중단 권한자. 각 공백과 담당자 기록 |

### 두 요청 비교 (강의 연습 요약)

| | 가상 의료기관 사례: 여러 소유 환자 포털 저장소의 노이즈 백로그 | 긴급 PR 1건 |
|---|---|---|
| 워크플로우 | SAST·의존성 백로그 검증, 증거·개발자 인계 개선 | merge 전 보안 결정 |
| 시작 경로 | 소유 저장소 1개에서 범위 있는 Codex Security 조사. 저장소 제공자·준비 상태는 **공백으로 남김** (Cloud 자격을 지어내지 않음) | 그 변경과 필요한 맥락에 한정된 **집중 Security Review** |
| 접근 자세 | "특별 사이버 접근" 요청이 있으나 고급 접근 필요는 미확인. ZDR·가격은 OpenAI 절차로 | 제품 활성화 미확인 → 관리자 확인 |
| 경계 | 증명 = 증거 품질·개발자 인계 개선. 운영 테스트 없음 | 저장소 전체 평가·지속 감시는 **별도 범위 결정**. 긴급함이 승인을 대신하지 않음 |

둘은 서로 보완할 수 있지만 **어느 하나가 다른 것을 허가하지 않아요.**

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. "사이버 모델 쓸 수 있나요? 취약점 백로그는 있는데 저장소나 리뷰어는 아직 안 정했어요." 워크플로우 우선 라우팅에 가장 맞는 답은?</summary>
  <p>① 백로그엔 보통 사이버 특화 모델과 낮춘 안전장치가 필요하니 표준 경로는 부적절 ② 가장 고급 역량이 필요할 테니 특화 접근부터 요청 ③ 접근 논의 전에 방어 워크플로우·소유 자산·리뷰어·신호원·병목·증명 지점을 명확히 하고, 그 뒤 가장 안전한 경로를 추천 ④ Trusted Access가 거절을 없애니 해결됨</p>
  <p><b>정답: ③</b></p>
</details>
<details class="oa-quiz">
  <summary>Q2. "소유 저장소에 의존성·SAST 백로그가 있어요. 엔지니어링은 티켓을 받기 전 더 명확한 증거가 필요하고, AppSec 리드가 결과를 리뷰할 수 있어요." 현실적 첫 단계는?</summary>
  <p><b>정답: 범위 있는 AppSec 워크플로우 surface로 Codex Security부터 시작하고, 리뷰어·증거 기대를 확인하며, 특별 접근이나 자동 수정을 약속하지 않는다.</b> (운영 CI/CD 자동화 직행 ✗ / Trusted Access 먼저 ✗ / GPT-5.5-Cyber 즉시 추천 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q3. "소유 저장소 1개의 의존성 finding을 요약하고 엔지니어링용 수정 노트를 준비하고 싶어요. 티켓 변경 전 AppSec 리드가 전부 리뷰해요." 가장 적절한 경로는?</summary>
  <p><b>정답: 범위 있고·방어적이며·소유·리뷰 가능하고 아직 고급 접근 필요가 보이지 않으므로 승인된 표준 경로로 시작한다.</b> (자동 수정·배포 약속 ✗ / 모든 사이버 작업에 Trusted Access 필요 ✗ / 즉시 에스컬레이션 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q4. "자사 제품 저장소 리뷰를 승인받았고, 내부 보안팀이 finding 검증 중 사이버 관련 마찰을 겪어요. Trusted Access가 적용되나요?" 가장 안전한 답은?</summary>
  <p><b>정답: 승인된 방어 워크플로우에 사이버 관련 마찰 감소가 필요하다면 관련 있을 수 있다. 워크플로우·승인·리뷰어·자산·증거 필요를 기록하고 승인된 OpenAI 절차로 경로를 확인한다.</b> (GPT-5.5-Cyber·ZDR·특별 가격 자동 포함 ✗ / 공격 테스트 전용 ✗ / 거절을 없애 거버넌스 불필요 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q5. Astra를 선택할 수 있는 고객이 공개 사이버 결과를 외부 대상 레드팀 자동화 허가로 여긴다. 승인 문서는 미완이고 거절 감소를 기대한다. 어떻게 하나?</summary>
  <p><b>정답: 접근 주장을 멈추고, 승인된 워크플로우·대상·리뷰어·증거 필요·위험 플래그를 기록해 접근 경로 추천 전에 OpenAI SME로 에스컬레이션한다.</b> (대상 목록·테스트 전술을 받아 등급 판단 ✗ / Trusted Access로 SME 리뷰 생략 ✗ / Astra 선택 = 올바른 접근 경로 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q6. A 고객은 기존 GitHub 저장소의 승인된 평가 + 새 커밋 지속 리뷰가 필요하다. B 고객은 긴급 PR 1건의 merge 전 보안 결정만 승인됐다. 둘 다 AppSec 리뷰어가 있고 활성 접근은 미확인. 가장 맞는 조합은?</summary>
  <p><b>정답: A는 Cloud, B는 집중 Security Review를 고려하고, 어느 쪽이든 시작 전에 활성 접근과 승인 범위를 확인한다.</b> (둘 다 Red 승인 먼저 ✗ / PR 하나로 저장소 전체 평가 ✗ / 둘 다 PR 리뷰로 기존 코드 평가 대체 ✗)</p>
</details>

## 정리

- **접근 경로가 아니라 워크플로우를 라우팅한다.** 목적 · 범위 · 소유·승인 · 리뷰어 · 증거 · 민감도 6가지 사실이 작업 경계이고, 제품 접근 활성화는 따로 확인한다.
- AppSec·secure SDLC는 **Codex Security**가 보통 첫 surface. 고객의 결정에 맞춰 로컬 조사 / Cloud 저장소 평가·감시 / 집중 변경 리뷰를 고른다. 1회 리뷰 ≠ 지속 감시 승인.
- 많은 방어 작업은 **승인된 표준 경로**로 충분하다. 가장 고급 경로가 아니라 워크플로우에 맞는 가장 안전한 경로.
- **Trusted Access**는 정당화될 때만: Blue ≠ Red, Cloud의 Blue는 Cloud 안에서만, 내부 사용과 Daybreak Cyber Partner Program은 별개, 모델 접근은 이전되지 않고 자산 허가를 대신하지 않는다.
- 사이버 특화 접근·민감 맥락은 **사실 · 가정 · 위험 · 약속하지 않은 것**을 담은 노트와 함께 SME로. 추천은 **워크플로우 · 시작 경로 · 접근 자세 · 가드레일 · 다음 단계**의 5부 패턴과 그 이유로 남긴다.
