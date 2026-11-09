---
title: "Cyber application practice"
title_ko: "사이버 적용 실습 — Harborline Bank 사례로 끝까지 따라가기"
course: openai-cyber-deployment
lesson: 5
---

## 학습 목표

- 고객의 **사이버 페르소나·보안 입력·워크플로우 병목** 진단하기
- 범위·접근·위험·에스컬레이션 필요에 따라 Cyber / Daybreak 사용 사례 **라우팅**하기 (강의 기준)
- 제한된 **Codex Security 평가**를 고르고 범위 정하기
- 리뷰어·성공 지표·증거 기대치·거버넌스 안전장치 정의하기
- 기술 증거·처분(disposition)·proof gap 해석하기
- 다음 행동과 소유자가 분명한 **파트너 안전 추천서** 쓰기

## 한눈에 보는 코스 흐름

이 코스는 가상 고객 **Harborline Bank** 하나를 처음 신호부터 최종 추천까지 따라가요. 앞의 네 레슨에서 배운 도구를 실제로 **조립**하는 실습이에요.

<ol class="oa-flow">
  <li><strong>사례 설정</strong>신호 추출</li>
  <li><strong>진단</strong>페르소나·병목·사용 사례</li>
  <li><strong>라우팅</strong>워크플로우 패턴 선택</li>
  <li><strong>범위 설정</strong>제한된 평가 계획</li>
  <li><strong>증거 리뷰</strong>처분과 proof gap</li>
  <li><strong>추천서</strong>6개 섹션</li>
</ol>

> **Daybreak는 방어적 사이버 워크플로우 모션이다. 모델 접근 피치도, 공격 훈련도, 자동 수정 약속도 아니다.**

## 1단계 — 사례 파악: 요청은 결론이 아니라 신호

<div class="oa-eli">
  <span class="oa-eli__head">🩺 쉽게 말하면</span>
  <p>환자가 "MRI 찍어주세요"라고 해도 의사는 바로 찍지 않아요. "어디가 언제부터 어떻게 아프세요?"부터 물어요. 고객의 "Daybreak 전체 저장소에 돌려주세요"도 <b>처방 요청</b>이 아니라 <b>증상 설명</b>이에요. 진단이 먼저예요.</p>
</div>

Harborline Bank는 디지털 고객 포털을 현대화 중인 가상의 지역 금융사예요. 사례에 섞여 있는 신호들:

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">시끄러운 AppSec 백로그</span>스캐너 finding이 쌓이고 엔지니어링이 티켓 품질에 반발</div>
  <div class="oa-card"><span class="oa-card__icon">🔐</span><span class="oa-card__title">민감한 PR</span>customer-account-service의 인가(authorization) 로직을 바꾸는 PR, 릴리스 임박</div>
  <div class="oa-card"><span class="oa-card__icon">⏳</span><span class="oa-card__title">제약</span>리뷰어 여력 부족, 릴리스 속도 압박, AI 보안 작업에 대한 거버넌스 우려</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚩</span><span class="oa-card__title">성급한 요청</span>범위 정하기 전 "전체 저장소에 Daybreak?", 워크플로우 확인 전 "사이버 접근은?"</div>
</div>

사례를 읽을 땐 **알려진 사실 / 합리적 가정 / 열린 질문**을 나눠요. 그리고 두 질문을 비교해요:

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 제한되고 리뷰 가능한 질문</span>
    <p>"Codex Security가 AppSec과 엔지니어링이 이 인가 관련 변경을 <b>더 명확한 증거, 정해진 범위, 사람의 승인</b>으로 리뷰하도록 도울 수 있나?"</p>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 첫 추천엔 너무 넓은 질문</span>
    <p>"Daybreak가 전부 스캔하고, 모든 취약점을 증명하고, 전사 수정을 자동화할 수 있나?"</p>
  </div>
</div>

## 2단계 — 페르소나 · 병목 · 사용 사례 진단

**페르소나는 직함이 아니라 결정 렌즈**예요: 무엇을 책임지고, 어떤 증거를 믿고, 어떤 결정을 내려야 하나.

| 페르소나 | 결정 렌즈 | 추천서가 답해야 할 것 |
|---|---|---|
| CISO | 위험, 거버넌스, 임원 신뢰 | 작업이 제한·승인·리뷰 가능하고 사람 책임과 정렬됨 |
| AppSec 리드 | finding 유효성, 증거 품질 | 후보 finding·proof gap·리뷰어 결정을 구분하도록 도움 |
| DevSecOps 매니저 | 반복성, 도구 적합성 | 첫 단계가 기록 시스템을 우회하거나 섣불리 CI/CD로 가지 않음 |
| 엔지니어링 디렉터 | 릴리스 준비, 개발자 인계 | 자동 승인 주장 없이 PR 리뷰를 지원 |

"CISO는 위험을 신경 써요"보다 → *"CISO는 첫 평가가 소유 저장소로 제한되고, 운영 테스트를 제외하고, AppSec·엔지니어링이 결정권자로 남고, 기록 가능한 증거를 낸다는 확신이 필요해요."*

### 병목 찾기: Find → Validate → Test → Remediate → Prove

<div class="oa-svg">
<svg viewBox="0 0 720 170" role="img" aria-label="방어 워크플로우 다섯 단계와 Harborline의 병목 위치">
  <defs>
    <style>.s1{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.s2{fill:#fff5f3;stroke:#e9a99a;stroke-width:2.5}.t1{font-size:13px;fill:#4a4a4a;font-weight:700}.t2{font-size:11px;fill:#6a6a6a}.ar{stroke:#b9b9b9;stroke-width:2;fill:none}</style>
  </defs>
  <rect x="10" y="30" width="120" height="70" rx="10" class="s1"/><text x="70" y="60" text-anchor="middle" class="t1">Find</text><text x="70" y="80" text-anchor="middle" class="t2">스캐너·리뷰</text>
  <path d="M132 65 L152 65" class="ar"/>
  <rect x="155" y="30" width="120" height="70" rx="10" class="s2"/><text x="215" y="60" text-anchor="middle" class="t1">Validate</text><text x="215" y="80" text-anchor="middle" class="t2">진짜인가?</text>
  <path d="M277 65 L297 65" class="ar"/>
  <rect x="300" y="30" width="120" height="70" rx="10" class="s1"/><text x="360" y="60" text-anchor="middle" class="t1">Test</text><text x="360" y="80" text-anchor="middle" class="t2">승인된 확인</text>
  <path d="M422 65 L442 65" class="ar"/>
  <rect x="445" y="30" width="120" height="70" rx="10" class="s1"/><text x="505" y="60" text-anchor="middle" class="t1">Remediate</text><text x="505" y="80" text-anchor="middle" class="t2">리뷰된 수정</text>
  <path d="M567 65 L587 65" class="ar"/>
  <rect x="590" y="30" width="120" height="70" rx="10" class="s1"/><text x="650" y="60" text-anchor="middle" class="t1">Prove</text><text x="650" y="80" text-anchor="middle" class="t2">기록·처분</text>
  <text x="215" y="130" text-anchor="middle" class="t2">▲ Harborline의 막힌 곳</text>
  <text x="215" y="148" text-anchor="middle" class="t2">신호는 있는데 검증된 행동이 못 됨</text>
</svg>
</div>

고객은 이미 스캐너·티켓·저장소·리뷰 회의가 있어요. 문제는 신호가 없는 게 아니라 **검증되고 우선순위가 매겨진 리뷰 가능한 행동이 되기 전에 막히는 것**이에요. "AppSec 전체가 느리다"는 너무 넓고, 더 정확한 병목은:

> **AppSec과 엔지니어링이, 인가에 민감한 PR이 릴리스 전에 추가 보안 작업이 필요한지 더 빠르고 증거 중심으로 판단할 방법이 필요하다.**

### 사용 사례 분류

| 후보 라벨 | 판단 |
|---|---|
| 민감 PR의 코드 변경 리뷰 | ✅ **1차 사용 사례** — 지금 내려야 할 결정(이 변경이 리뷰 후 진행 가능한가)과 일치 |
| AppSec 백로그 검증 | 🔜 **확장 경로** — 첫 리뷰가 명확한 결정을 내고, 증거 품질이 괜찮고, 리뷰어가 확인된 뒤 같은 저장소의 백로그 조각으로 |
| 리뷰어 결정용 증거 생성 | 두 사례 모두에 녹아 있음 |
| 제한적 수정 지원 | ⏸️ finding이 검증되고 수정이 명시 승인된 경우에만 |

## 3단계 — 라우팅과 워크플로우 패턴 선택

<div class="oa-eli">
  <span class="oa-eli__head">🚌 쉽게 말하면</span>
  <p>"서울 여행 가고 싶어요"라는 사람에게 일단 <b>오늘 가장 급한 목적지</b>(병원 예약)부터 데려다주고, 관광은 그다음에 계획하는 것과 같아요. 라우팅은 <b>지금 가장 중요한 결정</b>에 맞는 첫 경로를 고르는 거예요.</p>
</div>

파트너의 재구성: *"더 넓은 커버리지를 추천하기 전에, customer-account-service PR부터 시작합시다. 고객 소유이고, 인가 관련이고, 시간에 민감하고, 이름 있는 AppSec·엔지니어링 담당자가 리뷰할 수 있으니까요."*

파트너가 **약속하면 안 되는 것**: 사이버 특화 접근, 거절 감소, Zero Data Retention, 특수 가용성, 가격, 운영 테스트, 광범위 자율 테스트, 자동 수정, 취약점 발견 보장. 그리고 **고객이 PR 리뷰를 승인했다고 제품 접근이 생기는 건 아니에요.** 실행 전엔 선택한 surface, 의도한 신원의 활성 접근, 허용 행동, 고객 참여 승인을 확인해요.

| 패턴 (강의 기준) | Harborline에서의 판단 |
|---|---|
| Review code changes (로컬 플러그인) | ✅ 첫 선택. 저장소 전체를 스캔하지 않고 Git 기반 변경과 관련 코드를 리뷰 |
| GitHub Security Review | 대안. 단 finding을 PR에 **게시**하므로 접근과 게시 권한 필요 — 아직 미확인 |
| Triage a backlog | 🔜 나중에, 문제가 백로그 검증으로 옮겨가면 |
| Deep security scan | ❌ 첫 리뷰 루프를 증명하기 전에 리뷰 부담만 늘림 |
| Fix and verify findings | ❌ 수용된 finding에서 시작하는 패턴이라 시기상조 |
| 광범위 자율 테스트 | ❌ 범위가 너무 넓고 운영·실대상·전 저장소 미승인 |
| 접근 우선 경로 | ❌ 접근은 승인된 워크플로우를 따라가야지 대화를 이끌면 안 됨 |

> **워크플로우 패턴은 가장 큰 야망이 아니라 첫 고객 결정에 맞춘다.** 더 넓은 Security Cloud 스캔·감시는 별도 범위·승인·접근·운영 합의가 필요한 이후의 결정이에요.

## 4단계 — 제한된 평가 범위 정하기

| 항목 | Harborline 첫 평가 |
|---|---|
| 대상 | 고객 소유 customer-account-service 저장소의 인가 관련 PR(diff) + 변경 리뷰에 필요한 관련 인가 파일·테스트 |
| 범위 밖 | 운영 테스트, 실시간 공격 재현, 제3자 인프라, 전 저장소 리뷰, 고객 데이터, 자동 merge·릴리스 결정, 자동 취약점 종결, (finding 검증 + 명시 승인 전) 수정 |
| AppSec 리뷰어 | finding 품질·증거·보안 관련성 |
| 엔지니어링 소유자 | 코드 동작·테스트·구현 영향 |
| 결정 소유자 | PR 진행 / 증거 추가 / 수정 작업 필요 여부 |

### 이 평가가 증명할 수 있는 것과 없는 것

- **답할 수 있음**: diff가 후보 인가 우려를 만드나? 어떤 소스 사실이 뒷받침하나? 어떤 가정·gap이 남나? 다음 결정은 누가? 무엇을 기록하나?
- **증명할 수 없음**: 런타임 악용 가능성, 운영 동작, 전사적 위험 — 범위 밖이니까요.

| 계획 점검 | 의미 | Harborline 예 |
|---|---|---|
| Context | 워크플로우가 쓸 수 있는 것 | 승인된 저장소, PR diff, 관련 인가 파일, 테스트, 이해관계자 노트 |
| Agentic runtime | 어디서 어떻게 일하나 | 운영 테스트·자동 변경 없는 제한된 diff 리뷰 |
| Observability | 어떻게 보고 기록하나 | AppSec·엔지니어링 리뷰, 증거 노트, 고객 승인 기록 시스템에 결정 기록 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 성공 지표</span>
    <p>"생성된 finding 수"</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 성공 지표</span>
    <p>"AppSec과 엔지니어링이 간결한 증거 기록으로 후보 finding을 <b>수용·거절·증거 추가 요청</b>할 수 있다." + 리뷰어 신뢰, 개발자 인계, 다음 결정(진행·증거 추가·수정·억제·에스컬레이션)의 명확성</p>
  </div>
</div>

## 5단계 — 기술 증거 리뷰 연습

가상의 Codex Security 스타일 출력이 이렇게 말해요: **"리팩터링 후 계정 업데이트 경로 하나에서 인가 확인이 우회될 수 있음."** 이걸 액면 그대로 받지 말고 증거에 비춰 따져요.

<div class="oa-eli">
  <span class="oa-eli__head">⚖️ 쉽게 말하면</span>
  <p>심판이 "반칙일 수도"라고 느꼈다면 바로 레드카드를 주지 않고 <b>VAR(추가 확인)</b>을 봐요. 그렇다고 "확인 안 했으니 반칙 아님"으로 넘기지도 않죠. 지금 상태는 <b>"검토 필요"</b>예요.</p>
</div>

| 증거 라벨 | 이 사례에서 |
|---|---|
| Source fact | diff가 계정 업데이트 경로를 변경함. 그 경로가 역할 확인 로직에 닿는 방식이 바뀜 |
| Supported inference | 변경된 경로는 AppSec 리뷰가 필요할 수 있음 |
| Runtime proof | **없음** (승인·수행된 런타임 검증 없음) |
| Unsupported claim | "무단 접근이 증명됐다" |
| Proof gap | 라우트 수준 통제가 이미 인가를 강제하는지 모름. 제공된 테스트가 위임 관리자 동작을 명확히 다루지 않음 |

### 처분: 후보 finding / 추가 리뷰 필요

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">✗</span><span class="oa-card__title">Validated가 아님</span>런타임 증명·승인된 테스트 실행·리뷰어 결정이 없음</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">✗</span><span class="oa-card__title">False positive도 아님</span>소스 사실이 리뷰할 그럴듯한 이유를 보여줌</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">✗</span><span class="oa-card__title">"치명적"도 아님</span>인가 이슈라고 자동으로 심각한 건 아님. 심각도는 고객 리뷰어가 판단</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">✓</span><span class="oa-card__title">Candidate · needs review</span>AppSec·엔지니어링이 변경 경로를 리뷰, 다른 곳에서 인가가 강제되는지 확인, 추가 테스트·수정 필요 여부 결정, 결과 기록</div>
</div>

파트너 안전 증거 노트 요지: *"제한된 코드 변경 리뷰에서 계정 업데이트 경로의 후보 인가 우려가 나왔습니다. 소스 증거상 PR이 역할 확인 로직에 닿는 방식을 바꿨고, 제공된 테스트는 위임 관리자 동작을 명확히 다루지 않습니다. 런타임 검증은 승인·수행되지 않았으므로 검증된 취약점으로 제시하면 안 됩니다. AppSec과 엔지니어링이 변경 경로를 리뷰하고 추가 테스트·수정 필요 여부를 결정해야 합니다."* → **증거 · 한계 · 처분 · 다음 행동**이 다 들어 있어요.

## 6단계 — 파트너 안전 추천서

| 섹션 | Harborline 예시 요지 |
|---|---|
| 고객 상황 | AppSec 백로그 증가 + 인가 관련 PR의 즉각적 릴리스 결정. 이해관계자: CISO·AppSec 리드·DevSecOps·엔지니어링 디렉터. 병목은 전사 커버리지가 아니라 **민감 PR의 검증과 리뷰어 신뢰** |
| 추천 경로 | 인가 관련 PR로 제한한 "Review code changes" 워크플로우. 고객 소유·리뷰 가능·시간 민감·이름 있는 리뷰 경로가 있어서 |
| 제한된 평가 | 승인된 PR + 관련 인가 파일·테스트. 운영 테스트·실시간 재현·제3자·고객 데이터·자동 수정·자동 merge·전 저장소 제외. AppSec·엔지니어링이 결정 |
| 성공·안전장치 | 증거 품질·리뷰어 신뢰·결정 명확성·인계로 측정(finding 수 아님). 증거 기록·gap·처분·다음 행동 보관. 실행 전 접근·허용 행동 확인. 특수 접근·운영 테스트·광범위 자동화는 별도 승인·에스컬레이션 |
| 증거 위치 | 후보 finding / 추가 리뷰 필요. 런타임 증명 없음, 무단 접근 미증명 |
| 다음 행동 | Harborline: PR 리뷰 범위·surface·활성 접근·허용 행동·리뷰어 확인, 증거 기록 위치 합의. 파트너: 평가 계획 지원, 특수 접근·운영 테스트·고급 요청은 약속 전 에스컬레이션 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 추천</span>
    <p>"Daybreak가 Harborline 전 저장소를 스캔하고, 오탐을 줄이고, 인가 이슈를 고치고, 사이버 특화 접근도 받게 도와드립니다."</p>
    <p>→ 범위·결과·수정·접근을 모두 과장</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 추천</span>
    <p>"Harborline은 인가 관련 PR에 대한 제한된 Codex Security 리뷰로 시작해야 합니다. 실행 전 surface·활성 접근·허용 행동을 확인하고, AppSec과 엔지니어링이 증거를 리뷰해 추가 조사나 수정이 필요한지 결정합니다. 더 넓은 커버리지와 통합은 별도 승인이 필요합니다."</p>
  </div>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Harborline 사례의 초기 Cyber / Daybreak 신호를 가장 잘 표현한 것은?</summary>
  <p><b>정답: 넓은 보안 요청이 있지만, 첫 단계는 승인된 워크플로우·리뷰어·범위·증명 지점을 명확히 하는 것이다.</b> (인가 취약점 확정 ✗ / 사이버 특화 접근이 핵심 ✗ / 전 저장소 이미 승인 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q2. 엔지니어링 디렉터: "또 다른 경보 큐는 싫어요. 이 인가 관련 PR이 피할 수 있는 릴리스 위험 없이 진행될 수 있는지 알고 싶어요." 가장 강한 진단은?</summary>
  <p><b>정답: 첫 사용 사례는 민감 PR의 코드 변경 리뷰이며, 릴리스 결정 전에 AppSec·엔지니어링이 증거를 리뷰한다.</b> (백로그 대량 수정 ✗ / 전 저장소 스캔 ✗ / 사이버 특화 접근 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q3. 전 저장소 Daybreak와 사이버 특화 접근을 묻지만, 소유 저장소의 릴리스 민감 PR·리뷰어·운영 테스트/자동 수정 금지 조건이 있다. 가장 강한 첫 경로는?</summary>
  <p><b>정답: 인가 관련 PR로 범위를 정하고, 리뷰어를 지명하고, 실행 전 surface·활성 접근·허용 행동을 확인한다.</b> (Trusted Access 자격부터 ✗ / 스캔 후 무엇이 중요한지 정하기 ✗ / 백로그부터, PR은 나중에 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q4. Harborline 첫 단계로 가장 강한 평가 계획은?</summary>
  <p><b>정답: 소유 저장소의 인가 관련 PR을 리뷰하고, 운영 테스트·자동 수정을 제외하고, AppSec·엔지니어링이 증거 기록을 보고 다음 행동을 결정한다.</b> (후보 finding을 위험 증명으로 보고 릴리스 차단 ✗ / CI/CD 통합부터 ✗ / 전 저장소 심각도 순위 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q5. 출력은 인가 확인이 "우회될 수 있다"고 하지만 런타임 검증·테스트 실행·AppSec 결정이 없다. 최선의 처분은?</summary>
  <p><b>정답: 수정이나 릴리스 결정 전에 AppSec·엔지니어링 리뷰가 필요한 후보 finding.</b> (인가니까 치명적 ✗ / 런타임 증명 없으니 오탐 ✗ / 구체적이니 검증됨 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q6. 시끄러운 백로그, 즉각적인 인가 PR, 이름 있는 리뷰어, 미해결 사이버 접근 질문이 있는 상황. 가장 강한 추천은?</summary>
  <p><b>정답: PR 중심 리뷰를 추천하고 실행 전 접근·승인을 확인한다. AppSec·엔지니어링의 소유권, 증거, proof gap을 유지하고 더 넓은 커버리지는 따로 평가한다.</b> (저장소 security scan으로 확장 근거 찾기 ✗ / 빠른 릴리스 승인으로 포지셔닝 ✗ / 백로그 먼저 ✗)</p>
</details>

## 정리

- 고객의 넓은 요청은 **신호**다. 사실·가정·열린 질문을 나누고, 제한되고 리뷰 가능한 결정 질문으로 바꾼다.
- **페르소나 = 결정 렌즈.** Find → Validate → Test → Remediate → Prove에서 **실제로 막힌 곳**을 찾는다.
- 워크플로우 패턴은 **첫 고객 결정**에 맞춘다(여기선 PR 코드 변경 리뷰). 접근은 워크플로우를 따라간다.
- 평가는 대상·범위 밖·리뷰어·증거 한계·성공 지표를 먼저 정한다. **Context · Runtime · Observability**로 점검한다.
- 증거는 라벨로 나누고, 처분은 정직하게(**후보 / 추가 리뷰 필요**). 추천서는 6개 섹션으로 **다음 행동·소유자**까지.
