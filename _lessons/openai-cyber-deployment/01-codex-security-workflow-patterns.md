---
title: "Codex Security Workflow Patterns"
title_ko: "Codex Security 워크플로우 패턴 — 경보에서 검토 가능한 결정까지"
course: openai-cyber-deployment
lesson: 1
---

## 학습 목표

- Daybreak 안에서 **Codex Security**가 맡는 역할을 설명하기 (강의 기준)
- 분석 전에 필요한 **범위·context·승인·증거 경계**를 짚어내기
- 위협 모델링부터 재검증까지 **평가 생명주기**를 설명하기
- 고객 작업에 맞는 **가장 좁은 워크플로우 패턴**을 고르고 이유를 말하기
- 자동화·승인·종결을 과장하지 않고 **수정(remediation) 지원**을 설명하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>역할 이해</strong>경보량 → 결정 품질</li>
  <li><strong>context·증거 경계</strong>무엇을 보고, 어디까지 주장하나</li>
  <li><strong>평가 생명주기</strong>위협 모델 → 재검증</li>
  <li><strong>패턴 선택</strong>가장 좁게 맞는 것</li>
  <li><strong>수정 지원</strong>제한적·검토 가능</li>
  <li><strong>안전한 인계</strong>5가지 질문 요약</li>
</ol>

이 코스의 핵심 규칙:

> **승인된 워크플로우에서 출발하라. 그다음에 그에 맞는 Codex Security 패턴을 골라라.**

고객이 "저장소 스캔 되나요?", "백로그 다 고쳐주나요?", "사이버 특화 모델 쓸 수 있나요?"라고 물어도 그건 **신호**일 뿐, 워크플로우 결정이 아니에요.

## 1단계 — Codex Security는 무엇을 하나

<div class="oa-eli">
  <span class="oa-eli__head">🚨 쉽게 말하면</span>
  <p>화재경보기가 100번 울린다고 불이 100번 난 건 아니에요. 토스트를 태웠을 수도, 수증기일 수도 있죠. 진짜 필요한 건 <b>더 많은 경보</b>가 아니라 <b>"이건 진짜 불이다 / 이건 아니다"를 근거와 함께 가려내는 능력</b>이에요. Codex Security는 그 "가려내기"를 돕는 도구예요.</p>
</div>

Codex Security는 Daybreak의 일부로, **로컬 Codex Security 플러그인**과 연결된 GitHub 저장소를 스캔하는 **Codex Security Cloud**로 방어적 애플리케이션 보안을 지원해요. GitHub에는 PR용 **Codex Code Review**와 별도의 **Security Review(research preview)**도 있어요 (모두 강의 기준). 변경 하나를 리뷰하는 것과 저장소 전체를 평가하는 것, 새 커밋을 계속 감시하는 것은 **서로 다른 일**이에요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">코드 context 이해</span>저장소 구조와 시스템 맥락 파악, 위협 모델 작성·검토</div>
  <div class="oa-card"><span class="oa-card__icon">🔬</span><span class="oa-card__title">증거 검증</span>후보 finding이 실제 맥락에서 근거가 있는지 확인</div>
  <div class="oa-card"><span class="oa-card__icon">📦</span><span class="oa-card__title">결정용 포장</span>사람 리뷰어가 판단할 수 있게 보고서로 정리</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🩹</span><span class="oa-card__title">제한적 수정 지원</span>준비된 finding에 대해 검토 가능한 패치·테스트·재검증</div>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 경보량 (alert volume)</span>
    <ul>
      <li>검토할 잠재 이슈가 더 많아짐</li>
      <li>티켓·스캐너 결과·백로그 증가</li>
      <li>보안·엔지니어링 팀 압박 증가</li>
      <li>약한 포지셔닝: "취약점을 더 많이 찾아줍니다"</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 결정 품질 (decision quality)</span>
    <ul>
      <li>finding이 적용되는지에 대한 명확한 증거</li>
      <li>실제·도달 가능·관련 있는지 검증</li>
      <li>영향·맥락 기반 우선순위, 검토 가능한 수정 가이드</li>
      <li>증거가 부족한 곳(proof gap)을 명확히 표시</li>
    </ul>
  </div>
</div>

### 사람이 끝까지 쥐고 있는 통제 지점

| 사람 팀의 책임 | 왜 자동화하면 안 되나 |
|---|---|
| 범위·승인 확인 | 무엇을 봐도 되는지는 고객만 정할 수 있음 |
| 심각도 보정, finding이 실제·중대한지 판단 | 비즈니스 맥락이 필요 |
| 수정 제안 수용·변경·거절, 테스트·리뷰 충분성 판단 | 책임 소재 |
| PR 승인, 릴리스 결정, 기록 시스템 업데이트 | 거버넌스 프로세스 |
| 확장·중단·에스컬레이션 결정 | 위험 판단 |

고객용 문장: *"Codex Security는 증거와 검토 가능한 수정 지원을 준비하도록 돕습니다. 무엇을 수용·merge·릴리스·기록할지는 여전히 귀사의 보안·엔지니어링 팀이 결정합니다."*

### Codex Security가 아닌 것

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">대체재가 아님</span>스캐너나 AppSec·엔지니어링 팀을 대체하지 않음</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">보증이 아님</span>모든 취약점 발견도, 보고된 모든 이슈가 실제로 악용 가능하다는 것도 보증 안 함</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">지름길이 아님</span>자동 merge·배포·종결, 승인·접근 통제·리뷰 관문 우회, 무승인 운영/제3자 대상 테스트 불가</div>
</div>

**접근 권한 주의 (강의 기준)**: Security Cloud에 포함된 **Daybreak Blue** 접근은 그 제품 안에서만 적용돼요. API나 다른 제품 접근을 주지 않고, 지원되는 메인라인 모델의 거절을 줄인 접근일 뿐 **사이버 특화 모델 자동 접근이 아니에요.** 저장소 권한만으로, 또는 프로그램 승인만으로 접근이 활성화됐다고 볼 수 없어요. 신원·제품 surface·모델·의도된 작업을 모두 커버하는지 확인해요.

**예시** — 지역 금융사의 AppSec 백로그: 이미 SAST·의존성 스캐너를 쓰는데, 개발자가 "우리 맥락에선 악용 안 될 것 같은데요"라며 티켓을 반려해요. 필요한 건 스캐너 교체 피치가 아니라 **결정 병목** 해소예요: 어떤 게 실제인가? 도달 가능한가? 영향 큰 경로인가? 증거와 함께 억제(suppress)할 수 있나? 수정 준비가 됐나? 누가 결정을 수용하나?

## 2단계 — context와 증거 경계

<div class="oa-eli">
  <span class="oa-eli__head">🔎 쉽게 말하면</span>
  <p>탐정이 "범인은 집사입니다"라고 말하려면 근거가 있어야 해요. 발자국을 <b>봤다</b>(사실), 그래서 집사가 <b>그 방에 있었을 것</b>(추론), CCTV에 <b>찍혔다</b>(실제 확인). 발자국만 보고 "CCTV로 확인했다"고 말하면 거짓말이죠. <b>증거 상한선(evidence ceiling)</b>은 "내가 정직하게 주장할 수 있는 최대치"예요.</p>
</div>

- **증거 경계(evidence boundary)**: 승인된 범위와 허용된 활동에 따라 워크플로우가 **검사·테스트·주장**할 수 있는 범위
- **증거 상한선(evidence ceiling)**: 정직하게 주장할 수 있는 **가장 높은 수준의 증명**. 런타임 테스트가 승인되지 않았다면 소스 기반 증거와 proof gap까지만 가능

**context ≠ 무제한 접근**이에요. PR 보안 리뷰라면 변경 파일, 관련 인가(authorization) 헬퍼, 관련 테스트, 기대 동작이 필요하지만 모든 저장소·운영 데이터·무관한 내부 시스템은 필요 없어요.

### 세 가지 렌즈로 묻기

| 렌즈 | 질문 | Codex Security 예시 |
|---|---|---|
| Context | 무엇을 알거나 써도 되나? | 승인된 저장소·브랜치, 스캐너 finding, 아키텍처 노트, 테스트 결과, 보안 정책 |
| Agentic runtime | 어디서 어떻게 일하나? | 로컬 플러그인은 승인된 프로젝트에서 Codex 작업으로, Security Cloud는 구성된 환경에서 연결 저장소 스캔, GitHub Security Review는 PR에 finding 게시 |
| Observability | 고객이 어떻게 보고·기록·개선하나? | 보고서, 검증 노트, 리뷰어 결정, PR 토론, CI 결과, 티켓·기록 시스템 |

> **코드를 볼 권한 ≠ 코멘트 게시·변경 준비·merge 권한.** 그리고 **감시(monitoring)**는 지속적 보안 작업이고, **관측성(observability)**은 그 작업을 보고 결정을 기록하는 방법이에요. 하나가 다른 하나를 대신하지 않아요.

### 분석 전에 확인할 범위

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 부족한 요청</span>
    <p>"고객 포털 좀 스캔해 주실 수 있어요?"</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 범위가 잡힌 요청</span>
    <p>"승인 범위는 고객 소유 account-service 저장소의 staging 브랜치, 인증·인가 모듈입니다. AppSec 리드가 리뷰를 맡고, 엔지니어링 리드가 수정안을 검토하며, 운영 환경 테스트는 범위 밖입니다."</p>
  </div>
</div>

최소 확인 항목: 승인된 대상(저장소·코드 조각·백로그·브랜치·커밋·PR·환경), 보안/엔지니어링 소유자, 이름 있는 리뷰어, 데이터 처리 경계, 범위 안/밖 대상, 허용 런타임 활동(테스트·로컬 실행·의존성 설치·네트워크), 불명확할 때 에스컬레이션 경로. **더 강한 모델 능력이 승인 범위를 넓혀주지 않아요.**

### 증거 수준 5단계

<div class="oa-svg">
<svg viewBox="0 0 720 200" role="img" aria-label="증거 수준 사다리">
  <defs>
    <style>.e1{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.e2{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.e3{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.t1{font-size:13px;fill:#4a4a4a;font-weight:700}.t2{font-size:11px;fill:#6a6a6a}</style>
  </defs>
  <rect x="10" y="20" width="130" height="110" rx="10" class="e1"/><text x="75" y="50" text-anchor="middle" class="t1">Source fact</text><text x="75" y="74" text-anchor="middle" class="t2">코드·설정·로그에</text><text x="75" y="92" text-anchor="middle" class="t2">직접 보이는 것</text>
  <rect x="152" y="20" width="130" height="110" rx="10" class="e1"/><text x="217" y="50" text-anchor="middle" class="t1">Supported</text><text x="217" y="66" text-anchor="middle" class="t1">inference</text><text x="217" y="90" text-anchor="middle" class="t2">사실에서 나온</text><text x="217" y="108" text-anchor="middle" class="t2">합리적 결론</text>
  <rect x="294" y="20" width="130" height="110" rx="10" class="e1"/><text x="359" y="50" text-anchor="middle" class="t1">Runtime proof</text><text x="359" y="74" text-anchor="middle" class="t2">승인된 런타임</text><text x="359" y="92" text-anchor="middle" class="t2">테스트에서 관찰</text>
  <rect x="436" y="20" width="130" height="110" rx="10" class="e2"/><text x="501" y="50" text-anchor="middle" class="t1">Proof gap</text><text x="501" y="74" text-anchor="middle" class="t2">아직 증명 안 됨</text><text x="501" y="92" text-anchor="middle" class="t2">추가 증거·리뷰 필요</text>
  <rect x="578" y="20" width="132" height="110" rx="10" class="e3"/><text x="644" y="50" text-anchor="middle" class="t1">Unsupported</text><text x="644" y="66" text-anchor="middle" class="t1">claim</text><text x="644" y="90" text-anchor="middle" class="t2">증거를 넘어선 주장</text><text x="644" y="108" text-anchor="middle" class="t2">→ 쓰지 말 것</text>
  <text x="360" y="170" text-anchor="middle" class="t2">초록 = 말해도 되는 것 · 노랑 = 솔직히 표시할 것 · 빨강 = 신뢰를 깎는 것</text>
</svg>
</div>

| 수준 | 예시 (계정 내보내기 라우트) |
|---|---|
| Source fact | "PR이 내보내기 라우트에서 역할 확인 헬퍼 호출을 제거했다." |
| Supported inference | "이전엔 헬퍼를 호출했는데 이제 안 하므로, 이 동작의 인가 방식이 바뀌었을 수 있다." |
| Runtime proof | "승인된 staging에서 내보내기 역할이 없는 사용자가 실제로 내보내기를 실행했다." (런타임 활동이 명시 승인되고 관찰된 경우만) |
| Unsupported claim | "공격자가 모든 고객 계정을 내보낼 수 있다." (근거 없음) |
| Proof gap | "상위 미들웨어가 런타임에 이 라우트를 여전히 막는지는 확인되지 않았다." |

### finding 상태를 섞지 않기

| 상태 | 의미 |
|---|---|
| Candidate | 분석이 찾은 잠재 이슈. 그럴듯하지만 리뷰 필요 |
| Validated | 승인 범위 안에서 실제·관련·조치 가능하다고 볼 증거가 충분 |
| False positive | 리뷰어가 해당 없다고 판단 |
| Suppressed | 탐지는 되지만 기존 통제·도달 불가 경로·승인된 예외 때문에 이 맥락에선 무관 |
| Unresolved proof gap | 확정도 오탐도 아님. 결정 전에 증거가 더 필요 |

"취약점을 찾았다"가 아니라 → *"후보 finding을 찾았습니다. 인가 회귀 가능성을 시사하지만, 확정하려면 승인된 런타임 검증이 필요합니다."*

## 3단계 — 평가 생명주기

<div class="oa-eli">
  <span class="oa-eli__head">🏥 쉽게 말하면</span>
  <p>병원도 "기침해요" → 바로 수술하지 않아요. <b>문진(위협 모델)</b> → <b>검사(발견)</b> → <b>진단 확인(검증)</b> → <b>소견서(보고)</b> → <b>치료(수정)</b> → <b>재검사(재검증)</b>. 단계를 건너뛰면 엉뚱한 치료를 하게 되죠.</p>
</div>

<ol class="oa-flow">
  <li><strong>위협 모델</strong>무엇이 중요한가</li>
  <li><strong>finding 발견</strong>가능한 이슈 표면화</li>
  <li><strong>검증·공격 경로 분석</strong>근거가 있나</li>
  <li><strong>증거 패키지 보고</strong>리뷰어 결정용</li>
  <li><strong>검토 가능한 수정 지원</strong>제한적으로</li>
  <li><strong>재검증·처분</strong>무엇이 바뀌었나</li>
</ol>

더 넓은 방어 흐름으로는 **찾기 → 검증 → 테스트 → 수정 → 증명**이에요. 모든 워크플로우가 한 번에 전 단계를 거치진 않아요. 변경 리뷰는 proof gap으로 끝날 수 있고, 보고서가 패치 대신 억제·보류·에스컬레이션으로 이어질 수도 있어요. **생명주기는 "다 찾고 다 고친다"는 약속이 아니라 규율이에요.**

| 단계 | 하는 일 |
|---|---|
| 위협 모델 | 서비스가 하는 일, 상호작용하는 사용자·시스템, 신뢰할 수 없는 입력의 진입점, 인증·인가 위치, 민감 데이터, 서로 신뢰하는 컴포넌트, 확인이 필요한 가정. (파트너가 직접 할 필요는 없고 **왜 필요한지** 이해하면 됨) |
| finding 발견 | 인증·인가·입력 검증·데이터·비밀 처리·의존성·비즈니스 로직·통합 경계·설정·오류 처리 영역의 누락 통제나 위험 패턴 탐색 |
| 검증·공격 경로 | 이 저장소에서 실제인가? 도달 가능한가? 전제 조건이 그럴듯한가? 기존 통제가 완화하나? 리뷰어가 조치할 만큼 증거가 강한가? 약점이 **왜 중요할 수 있는지**를 개념 수준으로 설명 |
| 증거 패키지 | 범위 / finding 상태 / 증거 / gap / 다음 조치 — **자동 티켓 큐가 아님** |
| 재검증·처분 | 타깃 테스트, CI, 코드 리뷰, 승인된 런타임 검증, 리뷰어 서명, 회귀 확인 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 보고</span>
    <p>"인가 취약점 발견. 즉시 수정하세요."</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 보고</span>
    <p>"계정 내보내기 라우트의 후보 인가 회귀. 소스상 라우트가 더 이상 내보내기 역할 헬퍼를 호출하지 않음. 미들웨어는 세션 유효성만 확인하며, 내보내기 전용 역할 검사를 확인하는 소스 증거는 없음. 다음 조치: AppSec이 기대 동작을 리뷰하고, 수정 수용 전에 하위 환경 테스트 승인."</p>
  </div>
</div>

**처분(disposition)** 7가지: 확인·수정됨 / 확인·미수정 / 오탐 / 기존 통제로 완화 / 증거 부족으로 막힘 / 보류 / 에스컬레이션. **"패치가 제안됐다" ≠ "이슈가 고쳐졌다."**

## 4단계 — 가장 좁게 맞는 패턴 고르기

> **고객의 대상·시점·필요 증거·수정 준비도에 맞는 가장 좁은 워크플로우 패턴을 고른다.**

<ol class="oa-flow">
  <li><strong>대상은?</strong>저장소 전체 / 백로그 조각 / PR / 검증된 finding</li>
  <li><strong>시점은?</strong>첫 평가 / 심층 / merge 전 / 정기 / 지속 감시</li>
  <li><strong>필요 증거는?</strong>후보 / 검증 / 경로 / 수정 / 재검증</li>
  <li><strong>수정 준비됐나?</strong>사람이 수정용으로 수용했나</li>
</ol>

**먼저 제품 surface와 승인 범위**(로컬 플러그인 / GitHub PR 리뷰 / Cloud 저장소 스캔 / 지속 감시)를 정하고, 로컬 플러그인 안에서 4가지 패턴을 골라요. 플러그인에는 기존 finding을 위한 "Triage a backlog", 완료된 스캔 후 인계를 위한 "Export or track findings"도 있어요 (강의 기준).

| 패턴 (강의 기준) | 언제 | 고객이 얻는 것 | 핵심 안전장치 |
|---|---|---|---|
| Run a security scan | 저장소·서비스·모듈·백로그 조각의 범위 있는 첫/정기 리뷰 | 후보 finding, 증거, 우선순위, 수정 방향 | 범위 제한·리뷰어 주도 |
| Run a deep security scan | 표준 스캔 후, 같은 범위를 더 철저히 (핵심·고위험·복잡 시스템) | 더 포괄적 탐색, 더 많은 리뷰 부담 | 리뷰어 여력·중대 위험 확인 |
| Review code changes for security | PR·브랜치 범위·커밋·작업 트리 변경 | 변경별 finding, 소스 기반 증거, merge 가이드 | 승인된 테스트 없이 런타임 증명 주장 금지 |
| Fix and verify findings | 사람이 finding을 수정용으로 **수용한 뒤** | 최소 패치, 테스트, PR 요약, 재검증 질문 | 제안된 수정 = 수용된 수정 아님 |

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🐙</span><span class="oa-card__title">GitHub PR 리뷰</span>Code Review는 일반 리뷰, Security Review(research preview)는 diff·저장소 context·보안 가이드로 PR에 finding 게시. <b>게시 권한</b>을 따로 확인</div>
  <div class="oa-card"><span class="oa-card__icon">☁️</span><span class="oa-card__title">Cloud 저장소 스캔</span>연결 저장소를 요청 시/정기적으로 스캔. 저장소·브랜치·이력 기간·환경·리뷰어 합의. <b>정기 스캔 ≠ 로컬 deep scan</b></div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📡</span><span class="oa-card__title">Cloud 감시</span>기본 브랜치의 새 커밋을 확인. 기간·구성/예산 소유자·리뷰 여력·중단 조건 합의. <b>1회 리뷰 완료가 지속 작업을 승인하지 않음</b></div>
</div>

**트리거는 범위를 넓히지 않아요.** 스케줄은 "언제 시작하나"를 바꿀 뿐, "무엇을 해도 되나"를 바꾸지 않아요. 또 **finding이 0건인 스캔이 저장소가 안전하다는 증명은 아니에요.**

<div class="oa-eli">
  <span class="oa-eli__head">🧹 쉽게 말하면 — 회귀 증거 vs 보안 증거</span>
  <p>고장 난 자물쇠를 고친 뒤 "문이 여전히 잘 열리고 닫히나"만 확인하면 반쪽이에요. <b>"원래 열리면 안 되는 열쇠로는 이제 안 열리나"</b>도 확인해야죠. 기능이 정상인지(회귀 증거)와 원래 보안 문제가 해결됐는지(finding별 재검증)는 <b>다른 증거</b>예요.</p>
</div>

패턴과 이유를 함께 말하는 습관: *"특정 인가 변경의 merge 전 리뷰이지 저장소 전체 평가가 아니므로, 'Review code changes for security'를 추천합니다."*

## 5단계 — 제한적이고 검토 가능한 수정 지원

**제한적 수정(bounded fix)**은 특정 finding만 겨냥하고, 정상 동작을 보존하며, 무관한 파일·광범위 리팩터링을 피하고, 관련 테스트를 추가하고, 어떤 보안 통제를 적용하는지 설명하고, 남은 위험을 밝히고, 사람 리뷰용으로 준비된 변경이에요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 과장 표현</span>
    <ul>
      <li>"Codex Security가 취약점을 고쳤습니다."</li>
      <li>"패치가 생성됐으니 이슈 종결입니다."</li>
      <li>"이 PR은 merge해도 안전합니다."</li>
      <li>"운영 환경이 보호됨을 증명합니다."</li>
      <li>"더 이상 리뷰가 필요 없습니다."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 증거 기반 표현</span>
    <ul>
      <li>"검토 가능한 패치 제안을 준비했습니다."</li>
      <li>"승인 범위 안에서 특정 finding을 다룹니다."</li>
      <li>"기능 회귀 확인만으론 보안 수정 확인이 안 됩니다."</li>
      <li>"엔지니어링·AppSec 리뷰가 여전히 필요합니다."</li>
      <li>"최종 처분은 고객 기록 시스템에 남겨야 합니다."</li>
    </ul>
  </div>
</div>

PR 인계에는 다룬 finding, 수정을 정당화한 증거, 변경 파일, 추가·복원한 보안 통제, 테스트, (보안 증거와 기능 회귀 확인을 구분한) 검증 내역, 남은 위험, 리뷰 포인트를 넣어요.

강한 처분 문장 예: *"엔지니어링이 내보내기 엔드포인트의 역할별 인가를 복원하는 최소 패치를 리뷰·수용. 승인된 하위 환경에서 타깃 인가 테스트 통과. 운영 동작은 테스트하지 않음. AppSec이 승인 범위 내 '확인·수정됨'으로 기록."* → **무엇이 바뀌었고, 누가 결정했고, 무엇을 테스트했고, 무엇은 증명되지 않았는지**가 다 들어 있어요.

## 6단계 — 고객에게 안전한 워크플로우 요약

### 아직 준비가 안 된 신호

대상 자산, 승인 범위, 고객 소유권, 승인 환경, 소유자, 리뷰어, 데이터 경계, 허용 런타임 활동, 증거 상한선, 패턴 적합성, 중단 조건, 에스컬레이션 경로 중 하나라도 불명확하면 **멈추고 명확히** 해요. 실행 전엔 surface·필요 권한·활성 접근·고객 참여에 대한 승인 범위를 확인하고, Enterprise/Edu Security Cloud라면 Codex Cloud와 Codex Security 권한을 **둘 다** 확인해요. Cloud 스캔은 토큰 과금이라 유료 사용 opt-in과 예산도 확인해요 (강의 기준).

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🧑‍💼</span><span class="oa-card__title">전문가에게 넘길 요청</span>광범위 자동화, 운영·실서비스·제3자 대상 테스트, 공격 위주 작업, 특수 모델 접근, 특수 데이터 처리 조건, 가격·프로비저닝 약속, 맞춤 harness·통합, 파트너가 승인할 수 없는 약속</div>
</div>

### 5가지 질문 요약

| 질문 | 예시 답 |
|---|---|
| 1. 무엇을 리뷰하나? | 고객 소유 account-service 저장소, 인가 모듈과 현재 스캐너 백로그 조각 |
| 2. 어떤 워크플로우가 맞나? | 이 제한된 조각엔 로컬 플러그인 security scan. 저장소 전체 Cloud 평가·지속 감시는 별도 합의 필요 |
| 3. 누가 책임지나? | AppSec 리드가 finding·심각도, 엔지니어링 리드가 수정안 리뷰 (정기 작업이면 구성·예산 소유자, 리뷰 여력, 중단 권한자) |
| 4. 무엇을 증명할 수 있나? | 소스 기반 증거와 proof gap. 런타임 테스트 미승인이므로 런타임 증명 주장 안 함 |
| 5. 다음은? | surface·활성 접근·승인·허용 행동·리뷰어 확인 → 승인된 워크플로우만 실행 → 증거 리뷰 후 검증/억제/수정/에스컬레이션 결정 |

**예시 — "크리티컬 다 고치고, PR merge하고, 티켓 자동 종결해 주세요"**에 대한 안전한 답: 먼저 수용·검증된 finding과 후보 finding을 나누고, AppSec이 수정용으로 수용한 것만 "Fix and verify findings"로 최소 패치와 회귀 증거를 준비하며, 변경 승인·merge·릴리스·티켓 종결은 고객 리뷰어가 하고, 미검증 finding은 검증부터 시작한다. → **범위 축소 · 후보/검증 분리 · 증거 준비도로 패턴 선택 · 사람의 승인 유지.**

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. "스캐너는 이미 있어요. AppSec이 진짜 finding을 가리느라 며칠씩 쓰고, 엔지니어링은 티켓을 안 믿어요." Codex Security의 역할을 가장 잘 설명한 것은?</summary>
  <p>① 모든 크리티컬 finding 자동 패치 ② 더 정확한 목록으로 스캐너 대체 ③ 저장소 context와 스캐너 결과를 함께 추론해 검증·증거·우선순위·검토 가능한 수정을 지원하되 사람 리뷰어가 책임 ④ 범위·리뷰어보다 사이버 특화 모델 접근부터</p>
  <p><b>정답: ③</b> — 경보량이 아니라 결정 품질을 높이고, 책임은 사람에게.</p>
</details>
<details class="oa-quiz">
  <summary>Q2. 로그인 동작을 바꾸는 PR 리뷰 요청. diff와 관련 파일은 있지만 런타임 테스트는 미승인. 가장 안전한 증거 경계는?</summary>
  <p><b>정답: 소스 기반 finding과 지지되는 추론은 제공하되, 승인된 런타임 테스트 없이는 런타임 증명을 주장하지 않는다.</b> (코드 추론만으로 악용 가능성 확정 ✗ / 인증 관련은 모두 검증된 것으로 ✗ / 자동으로 로컬 실행 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q3. 문서 처리 서비스 저장소 리뷰가 승인됨. 신뢰할 수 없는 업로드가 검증 없이 내부 파서에 닿는지 알고 싶다. 책임 있는 순서는?</summary>
  <p><b>정답: 위협 모델 작성·검토 → 후보 finding 발견 → 증거·공격 경로 검증 → 리뷰용 패키지 → (적절하면) 제한적 수정 지원 → 재검증.</b> (개수 세서 바로 배정 ✗ / 패치·운영 테스트·종결 ✗ / Fix부터 시작 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q4. 관리자 권한을 바꾸는 PR 하나만 리뷰 승인(운영 테스트 없음). 동료가 저장소 전체 정기 Security Cloud 스캔을 제안한다. 최선은?</summary>
  <p><b>정답: 승인된 surface에서 PR과 관련 context에 묶어 리뷰하고, finding 게시 권한을 확인하며, 저장소 전체 스캔·지속 감시는 별도 범위·승인을 받는다.</b> (정기 스캔을 deep scan으로 취급 ✗ / merge 후 포함되니 감시 시작 ✗ / 같은 저장소니 권한 공유 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q5. 검증된 인가 finding에 대한 패치가 제안됐고 기능 회귀 확인은 통과. 엔지니어링 리뷰와 보안 재검증은 미완료. 맞는 말은?</summary>
  <p><b>정답: 검토 가능한 수정 지원이 준비된 상태다. 기능 확인 통과가 보안 수정을 확인하진 않으므로, 엔지니어링 리뷰·finding별 재검증·AppSec 수용 후에야 "수정됨"이라 할 수 있다.</b></p>
</details>
<details class="oa-quiz">
  <summary>Q6. "스캐너 백로그의 크리티컬을 다 고치고, PR merge하고, 티켓을 자동 종결해 주세요." 가장 안전한 인계는?</summary>
  <p><b>정답: 범위를 정하고, surface와 활성 접근을 확인하고, 리뷰어를 지정하고, 후보와 수정 수용된 finding을 분리한다. "Fix and verify findings"는 제한적·사람 검토 수정 지원에만 쓴다.</b> (사이버 특화 모델 먼저 ✗ / 접근만 주면 전체 흐름 자동 ✗ / 모든 저장소 deep scan부터 ✗)</p>
</details>

## 정리

- Codex Security는 **경보를 늘리는 도구가 아니라 결정 품질을 높이는 워크플로우 계층**이다. 사람이 범위·심각도·수용·merge·기록을 결정한다.
- 분석 전에 **범위·소유자·리뷰어·허용 활동·데이터 경계·증거 상한선**을 정한다. 사실 / 추론 / 런타임 증명 / proof gap / 근거 없는 주장을 구분한다.
- 생명주기: **위협 모델 → 발견 → 검증 → 증거 패키지 → 제한적 수정 → 재검증·처분**. "패치 제안 ≠ 수정 완료".
- **가장 좁게 맞는 패턴**을 고른다. 트리거·스케줄은 범위를 넓히지 않고, 1회 리뷰는 지속 감시를 승인하지 않는다.
- 인계는 **5가지 질문**(무엇을 / 어떤 워크플로우 / 누가 / 무엇을 증명 / 다음은)으로. 빈칸은 진행 허가가 아니라 확인할 이유다.
