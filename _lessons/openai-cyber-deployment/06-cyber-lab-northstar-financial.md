---
title: "Cyber Lab - Northstar Financial"
title_ko: "사이버 랩 — Northstar Financial 보안 추천서 설계"
course: openai-cyber-deployment
lesson: 6
---

## 학습 목표

- 넓은 보안 요청을 **제한되고 증거 중심인 다음 단계**로 바꾸는 랩의 구조 이해하기
- 승인된 범위·사람 리뷰·증거 경계를 지키는 **고객 보안 추천서**의 6가지 구성 요소 정리하기
- 앞선 다섯 레슨의 도구가 랩의 어느 칸에 들어가는지 **연결**하기

> 이 랩은 학습자가 직접 쓴 추천서(250–400단어, 한 페이지, PDF/DOCX)를 업로드해 리뷰받는 형식이에요. 이 글은 **랩이 무엇을 요구하고 각 항목이 무엇을 확인하는지**를 정리한 것이고, 제출용 답안이 아니에요.

## 한눈에 보는 랩 흐름

<ol class="oa-flow">
  <li><strong>상황 읽기</strong>요청 vs 승인된 범위</li>
  <li><strong>증거 읽기</strong>훈련용 리뷰 노트</li>
  <li><strong>추천서 6요소</strong>상황·첫 단계·범위·증거·지표·다음 행동</li>
  <li><strong>금지 약속 점검</strong>과장 표현 제거</li>
  <li><strong>제출</strong>본인이 작성·업로드</li>
</ol>

## 1단계 — 고객 상황: 요청과 승인 사이의 간격

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면</span>
  <p>집주인이 "집 전체를 점검하고, 고장 난 건 알아서 다 고치고, 이사 가도 안전하다고 보증해 주세요"라고 말하면서 <b>열쇠는 현관 신발장 하나</b>만 줬다면? 전문가는 "신발장부터 꼼꼼히 보고, 본 것과 못 본 것을 정직하게 말씀드릴게요"라고 해야 해요. 이 랩이 바로 그 간격을 다루는 연습이에요.</p>
</div>

가상의 지역 금융사 **Northstar Financial**이 디지털 고객 포털 업데이트를 준비 중이에요.

| 항목 | 상황 |
|---|---|
| 일정 | 3일 뒤 PR 릴리스 예정 |
| 변경 내용 | 고객 소유 `customer-account-service`의 계정 업데이트 인가 로직 |
| AppSec 상황 | 열린 finding 62개, 대부분 낮은 신뢰도. 이번 주엔 **집중된 보안 산출물 하나**만 리뷰 가능 |
| 엔지니어링 디렉터 | 명확한 릴리스 결정이 필요하고, 우려가 있으면 **유용한 증거와 함께** 개발자에게 돌려받고 싶음 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🙋 고객의 요청</span>
    <p>"모든 저장소에 Codex Security를 써서 백로그를 정리하고, 치명적 이슈는 자동으로 고치고, 이번 릴리스가 안전하다고 확실히 해줄 수 있나요?"</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 실제로 승인된 것</span>
    <p><b>읽기 전용</b> 리뷰: 인가 관련 PR + 직접 관련된 인가 파일 + 직접 관련된 테스트</p>
    <p>❌ 미승인: 다른 저장소, 운영 테스트, 공격 재현, 제3자 시스템, 자동 수정, 자동 merge, 자동 릴리스 결정</p>
  </div>
</div>

## 2단계 — 증거 노트 읽기

랩이 주는 훈련용 리뷰 노트의 요지: 변경된 계정 업데이트 경로에서 **이전 버전에 있던 역할 확인 헬퍼 호출이 사라졌고**, 기존 테스트는 인증된 사용자가 계정을 업데이트할 수 있음은 확인하지만 **한 사용자가 다른 고객의 계정을 업데이트할 수 있는지에 대한 테스트는 없어요.** 런타임 검증이나 재현은 수행되지 않았어요.

레슨 1의 증거 라벨로 나눠 보면:

| 라벨 | 이 랩에서 어디에 해당하나 |
|---|---|
| Source fact | 헬퍼 호출이 제거됨 / 기존 테스트가 다루는 범위 / 다른 고객 계정 업데이트 테스트의 부재 |
| Supported inference | 이 경로의 인가 동작이 바뀌었을 수 있어 리뷰가 필요함 |
| Runtime proof | 없음 — 승인되지도 수행되지도 않음 |
| Proof gap | 다른 곳(상위 계층)에서 인가가 여전히 강제되는지, 실제 런타임 동작은 어떤지 |
| 처분 | **후보 finding / 추가 리뷰 필요** (검증된 취약점도, 오탐도 아님) |

## 3단계 — 추천서의 6가지 구성 요소

랩은 아래 여섯 질문에 모두 답하라고 요구해요. 각 칸이 **무엇을 확인하는지**와 앞 레슨의 어떤 도구를 쓰는지 정리했어요.

| 요소 | 랩이 묻는 것 | 확인하는 역량 | 연결 레슨 |
|---|---|---|---|
| 1. 고객 상황 | 어떤 즉각적 결정을, 누가 내리고, 워크플로우는 어디서 막혔나? | 페르소나=결정 렌즈, 병목 진단 | 5 |
| 2. 추천 첫 단계 | 어떤 제한된 방어 리뷰를 먼저 하고, 왜 전 저장소보다 적절한가? | 가장 좁게 맞는 패턴 선택 | 1, 2 |
| 3. 범위와 리뷰어 | 범위 안/밖, 리뷰어는? 저장소 **전체**를 보려면 어떤 별도 범위·승인·접근·소유자가 필요한가? | 4요소 범위 문장, 사람·시스템·행동 | 2, 3 |
| 4. 증거 위치 | 직접 지지되는 것, 합리적 추론, 분류, 미증명 | 증거 라벨, 처분, 증거 상한선 | 1, 5 |
| 5. 성공 지표·안전장치 | 리뷰가 유용하려면? 유지할 경계는? | 품질 기반 지표, 행동 경계 | 2, 3 |
| 6. 다음 행동·소유자 | 다음에 무엇을, 누가, 어떤 결정을 위해? | 결정 지점으로 끝내기 | 4 |

### 각 칸을 채울 때의 사고 포인트

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">결정 질문을 좁힌다</span>"모든 저장소가 안전한가?"가 아니라 "이 PR이 릴리스 전에 추가 보안 작업이 필요한가?"</div>
  <div class="oa-card"><span class="oa-card__icon">📏</span><span class="oa-card__title">승인 범위를 그대로 쓴다</span>추천 범위가 고객이 승인한 읽기 전용 범위를 <b>넘지 않게</b>. 더 넓은 리뷰는 "별도 승인이 필요한 다음 결정"으로 분리</div>
  <div class="oa-card"><span class="oa-card__icon">👥</span><span class="oa-card__title">사람을 이름으로</span>AppSec 리드(증거·보안 관련성), 엔지니어링 소유자(코드·테스트), 릴리스 결정 소유자</div>
  <div class="oa-card"><span class="oa-card__icon">🔬</span><span class="oa-card__title">증거를 과장하지 않는다</span>"우회가 증명됐다" ✗ → 소스 사실·추론·gap을 분리해 서술</div>
  <div class="oa-card"><span class="oa-card__icon">📊</span><span class="oa-card__title">성공 = 결정 품질</span>리뷰어가 수용·거절·증거 추가 요청을 할 수 있는가, 개발자 인계가 유용한가 (finding 수 아님)</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">➡️</span><span class="oa-card__title">결정 지점으로 끝낸다</span>누가, 언제까지, 무엇을 확인해서 → 릴리스 진행 / 추가 증거 / 수정 중 무엇을 고를 수 있게 되나</div>
</div>

### 저장소 전체를 보려면? (요소 3의 후반부)

랩은 일부러 "더 넓게 보려면 무엇이 필요한가"를 따로 물어요. 레슨 1–3에서 배운 대로, 저장소 전체 평가는 **별도의 결정**이에요: 새 범위(저장소·브랜치·리비전), 그 범위에 대한 고객 승인, 선택한 surface와 의도한 신원의 활성 접근, 허용 행동(finding 게시 여부 포함), 리뷰어와 리뷰 여력, (Cloud라면) 구성·예산 소유자와 중단 조건. **PR 리뷰 승인이 저장소 전체 승인으로 번지지 않아요.**

### 금지 약속 체크리스트

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 추천서에 넣으면 안 되는 약속</span>
    <ul>
      <li>광범위한 제품 접근</li>
      <li>운영 테스트</li>
      <li>자동 수정 / 자동 merge</li>
      <li>취약점 발견 보장</li>
      <li>보안 결과 보장 ("이번 릴리스는 안전합니다")</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 대신 담을 것</span>
    <ul>
      <li>승인된 범위 안의 제한된 리뷰</li>
      <li>사람이 내리는 릴리스·수정 결정</li>
      <li>정직한 처분과 proof gap</li>
      <li>더 넓은 작업은 별도 승인 경로로</li>
      <li>소유자와 시점이 있는 다음 행동</li>
    </ul>
  </div>
</div>

## 4단계 — 전체 과정 한눈에: 다섯 레슨이 랩에서 만나는 곳

<div class="oa-svg">
<svg viewBox="0 0 720 360" role="img" aria-label="코스 전체 요약: 다섯 레슨이 하나의 추천서로 모이는 흐름">
  <defs>
    <style>.b1{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.b2{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.b3{fill:#eef4ff;stroke:#c6d6f3;stroke-width:1.5}.t1{font-size:13px;fill:#4a4a4a;font-weight:700}.t2{font-size:11px;fill:#6a6a6a}.ar{stroke:#b9b9b9;stroke-width:2;fill:none}</style>
  </defs>
  <rect x="10" y="10" width="220" height="70" rx="10" class="b1"/><text x="120" y="36" text-anchor="middle" class="t1">L1 워크플로우 패턴</text><text x="120" y="56" text-anchor="middle" class="t2">가장 좁은 패턴 · 증거 라벨</text>
  <rect x="250" y="10" width="220" height="70" rx="10" class="b1"/><text x="360" y="36" text-anchor="middle" class="t1">L2 제한된 평가</text><text x="360" y="56" text-anchor="middle" class="t2">첫 대상 · 헌장 · 품질 지표</text>
  <rect x="490" y="10" width="220" height="70" rx="10" class="b1"/><text x="600" y="36" text-anchor="middle" class="t1">L3 거버넌스</text><text x="600" y="56" text-anchor="middle" class="t2">6가지 질문 · 고객 결정</text>
  <rect x="130" y="110" width="220" height="70" rx="10" class="b1"/><text x="240" y="136" text-anchor="middle" class="t1">L4 가치 스토리</text><text x="240" y="156" text-anchor="middle" class="t2">임원 보고 8요소</text>
  <rect x="370" y="110" width="220" height="70" rx="10" class="b1"/><text x="480" y="136" text-anchor="middle" class="t1">L5 적용 실습</text><text x="480" y="156" text-anchor="middle" class="t2">Harborline 끝까지</text>
  <path d="M120 82 L300 228" class="ar"/><path d="M360 82 L360 228" class="ar"/><path d="M600 82 L420 228" class="ar"/><path d="M240 182 L330 228" class="ar"/><path d="M480 182 L390 228" class="ar"/>
  <rect x="160" y="230" width="400" height="70" rx="12" class="b2"/><text x="360" y="258" text-anchor="middle" class="t1">L6 Northstar 추천서</text><text x="360" y="280" text-anchor="middle" class="t2">상황 · 첫 단계 · 범위 · 증거 · 지표 · 다음 행동</text>
  <rect x="60" y="316" width="600" height="34" rx="8" class="b3"/><text x="360" y="338" text-anchor="middle" class="t2">공통 원칙: 승인된 범위 → 가장 좁은 워크플로우 → 증거로 말하기 → 사람이 결정 → 증거가 있을 때만 확장</text>
</svg>
</div>

| 레슨 | 핵심 한 줄 | 랩에서 쓰이는 곳 |
|---|---|---|
| 1. Codex Security Workflow Patterns | 경보량이 아니라 결정 품질, 가장 좁게 맞는 패턴 | 첫 단계 선택, 증거 라벨 |
| 2. Bounded evaluation and pilot design | 대상 하나·헌장·품질 지표로 시작 | 범위·리뷰어·성공 지표 |
| 3. Cyber governance and operating model | 6가지 질문, Codex는 돕고 고객이 결정 | 범위 밖 항목, 저장소 전체 확대 조건 |
| 4. Cyber Value Story & Executive Readout | 범위·증거·결정·gap·다음 행동으로 말하기 | 다음 행동·소유자, 과장 없는 표현 |
| 5. Cyber application practice | 신호 → 진단 → 라우팅 → 범위 → 증거 → 추천 | 추천서 전체의 뼈대 |

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Northstar가 승인한 범위로 맞는 것은?</summary>
  <p><b>정답: 인가 관련 PR, 직접 관련된 인가 파일, 직접 관련된 테스트에 대한 읽기 전용 리뷰.</b> 다른 저장소·운영 테스트·재현·제3자·자동 수정·자동 merge·자동 릴리스 결정은 미승인.</p>
</details>
<details class="oa-quiz">
  <summary>Q2. 증거 노트(헬퍼 호출 제거, 교차 계정 테스트 부재, 런타임 검증 없음)의 올바른 분류는?</summary>
  <p><b>정답: 후보 finding / 추가 리뷰 필요.</b> 소스 사실이 리뷰할 이유를 주지만, 런타임 증명과 리뷰어 결정이 없으므로 검증된 취약점이 아니고, 그렇다고 오탐도 아님.</p>
</details>
<details class="oa-quiz">
  <summary>Q3. "이번 릴리스가 안전하다고 확실히 해 달라"는 요청에 추천서가 할 수 있는 것은?</summary>
  <p><b>정답: 보안 결과를 보장하지 않고, 승인된 범위의 제한된 리뷰로 릴리스 결정권자가 판단할 증거와 proof gap을 제공한다.</b> 릴리스 결정은 고객(엔지니어링·AppSec)이 내림.</p>
</details>
<details class="oa-quiz">
  <summary>Q4. 같은 저장소 전체를 평가하고 싶다면 무엇이 추가로 필요한가?</summary>
  <p><b>정답: 별도의 범위 정의, 그 범위에 대한 고객 승인, surface·신원의 활성 접근 확인, 허용 행동, 리뷰어와 여력, (반복 작업이면) 구성·예산 소유자와 중단 조건.</b> PR 리뷰 승인은 전체 저장소 승인이 아님.</p>
</details>

## 정리

- 랩의 핵심은 **요청("전부·자동·보장")과 승인("PR 하나·읽기 전용") 사이의 간격**을 정직하게 메우는 것이다.
- 증거는 **사실 / 추론 / 런타임 증명 / gap**으로 나누고, 처분은 **후보 · 추가 리뷰 필요**로.
- 추천서 6요소: **상황 · 첫 단계 · 범위와 리뷰어 · 증거 위치 · 성공 지표와 안전장치 · 다음 행동과 소유자**.
- 더 넓은 작업은 **별도 범위·승인·접근·소유자**가 필요한 다음 결정으로 분리한다.
- 코스 전체의 한 문장: **승인된 범위에서, 가장 좁은 워크플로우로, 증거로 말하고, 사람이 결정하며, 증거가 있을 때만 넓힌다.**
