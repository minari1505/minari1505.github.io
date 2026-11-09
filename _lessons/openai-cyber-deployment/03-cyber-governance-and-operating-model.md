---
title: "Cyber governance and operating model"
title_ko: "사이버 거버넌스와 운영 모델 — 진행·중단·에스컬레이션·확장의 판단"
course: openai-cyber-deployment
lesson: 3
---

## 학습 목표

- 왜 **거버넌스가 Daybreak 운영 모델 그 자체**인지 설명하기 (강의 기준)
- **6가지 운영 모델 질문**으로 워크플로우 준비도 점검하기
- 최소 준비 조건(승인된 방어 워크플로우·고객 소유 자산·승인 환경·이름 있는 리뷰어) 확인하기
- 접근·권한·데이터·감시·감사·증거 보관·기록 시스템에 대한 **거버넌스 결정** 짚기
- Codex가 지원하는 일과 **고객이 결정하는 일** 나누기
- **Proceed · Pause · Escalate · Expand** 중 다음 단계 고르기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>왜 거버넌스인가</strong>6가지 질문</li>
  <li><strong>승인된 범위</strong>4요소 범위 문장</li>
  <li><strong>접근·권한·데이터</strong>사람·시스템·행동</li>
  <li><strong>증거·감사·기록</strong>결정 등급 증거 기록</li>
  <li><strong>사람의 책임</strong>Codex 실행 vs 고객 결정</li>
  <li><strong>다음 단계</strong>진행·중단·에스컬레이션·확장</li>
</ol>

> **Daybreak는 보안 신호를 검토 가능한 행동으로 바꿀 때 가치를 만든다. 거버넌스는 그 이동을 승인되고, 관찰 가능하고, 책임 있고, 반복 가능하게 만든다.**

## 1단계 — 거버넌스는 브레이크가 아니라 운영 방식

<div class="oa-eli">
  <span class="oa-eli__head">🏊 쉽게 말하면</span>
  <p>수영장에 안전요원, 수심 표시, "다이빙 금지" 표지판이 있다고 수영을 못 하는 건 아니죠. 오히려 그 덕분에 <b>마음 놓고 많은 사람이</b> 수영할 수 있어요. 거버넌스는 "하지 마"가 아니라 <b>"어디까지, 누가 지켜보며, 문제가 생기면 누가 멈추나"</b>를 정하는 규칙이에요.</p>
</div>

Daybreak 작업은 소스 코드, 내부 시스템, 취약점 finding, 보안 도구, 승인 절차, 수정 결정, 리스크·컴플라이언스 기록에 닿아요. 그래서 "전부 스캔", "인터넷에 노출된 모든 시스템 테스트", "모든 수정 자동화" 같은 요청은 **긴급함은 보여주지만 실행할 만큼 구체적이거나 안전하지 않아요.**

이 코스에서 **운영 모델**은 누가·무엇을·어떤 자산·어떤 환경·어떤 도구와 데이터로 하며, 어떤 승인이 필요하고, 어떤 증거가 나오고, 누가 리뷰하고, 얼마나 보관하고, 최종 결정을 어디에 기록하는지 정하는 **실무 패턴**이에요.

### 6가지 운영 모델 질문

| # | 질문 | 확인하는 것 |
|---|---|---|
| 1 | 이 일을 해도 되나? | 승인, 방어 목적, 고객 허락, 범위 |
| 2 | 자산이 고객 소유이고 환경이 승인됐나? | 대상·환경의 적절성 |
| 3 | 워크플로우가 무엇에 접근하고 무엇을 할 수 있나? | 사람·시스템·도구 권한·허용 행동 |
| 4 | 어떤 증거가 생성·리뷰·보관·기록되나? | 감사 가능성, 증거 품질, 기록 시스템 |
| 5 | 어떤 결정이 고객 몫인가? | 위험·심각도·수정·릴리스·보관·종결 책임 |
| 6 | 진행·중단·에스컬레이션·확장 중 무엇? | 준비도에 따른 책임 있는 다음 단계 |

이 코스는 고급 구성이나 랩 실행이 아니라 **거버넌스 판단**에 초점을 둬요.

### Context · Agentic runtime · Observability

| 레이어 | 핵심 질문 |
|---|---|
| Context | 이 워크플로우에 승인된 정보는 무엇이고, 무엇이 범위 밖인가? |
| Agentic runtime | 로컬 플러그인(승인된 프로젝트), Security Cloud(연결된 GitHub 저장소), GitHub Code Review·Security Review(PR) 중 어디서? 서로 바꿔 쓸 수 있는 경로로 취급하지 않기 (강의 기준) |
| Observability | 무엇이 실행됐고 무엇에 접근했고 결과가 어떻게 리뷰됐는지 **추적**할 수 있나? |

> **취약점 감시(monitoring)**는 잠재 이슈를 찾고, **워크플로우 관측성(observability)**은 무엇이 돌았고 결과가 리뷰됐는지를 기록해요.

## 2단계 — 승인된 범위와 고객 소유 자산

- **승인됨(authorized)**: 이름 있는 자산에, 승인된 환경에서, 정의된 방어 목적으로 일할 명시적 허락
- **방어적(defensive)**: 고객 **자신의** 시스템을 보호하려는 목적 (finding 검증, 위험 우선순위, 리뷰용 수정 제안, 보안 결정용 증거)

고객 안전 질문: *"무엇을, 어떤 방어 목적으로, 누구의 권한으로 검사해도 되나요?"* — "인터넷 노출 시스템을 리뷰하고 싶어요"만으로는 부족해요. 답하지 못한다면 기회가 약한 게 아니라 **승인·범위부터 명확히 할 차례**라는 뜻이에요.

### 4요소 범위 문장

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🏠</span><span class="oa-card__title">고객 소유 자산</span>어떤 저장소·앱·백로그 조각·워크플로우·환경?</div>
  <div class="oa-card"><span class="oa-card__icon">🧪</span><span class="oa-card__title">승인 환경·접근 경로</span>어디서 일하고, 접근은 어떻게 승인되나?</div>
  <div class="oa-card"><span class="oa-card__icon">👤</span><span class="oa-card__title">소유자·리뷰어</span>워크플로우는 누가 소유하고, 결과는 누가 리뷰하나?</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🛑</span><span class="oa-card__title">증거 기대치·중단 조건</span>어떤 증거가 나와야 하고, 언제 멈추거나 에스컬레이션하나?</div>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 아직 준비 안 된 첫 워크플로우</span>
    <ul>
      <li>"전부 스캔", "인터넷 노출 시스템 전부 테스트"</li>
      <li>"수정 전부 자동화", "운영부터 돌려보자"</li>
      <li>"AI로 외부 대상 테스트"</li>
      <li>"리뷰어는 결과 보고 정하자"</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 출발점</span>
    <ul>
      <li>고객 소유 저장소 하나 / 승인된 앱 하나</li>
      <li>백로그 조각 하나 / 취약점 유형 하나</li>
      <li>PR·diff 패턴 하나 / 승인된 하위 환경 하나</li>
      <li>+ 이름 있는 리뷰어, 합의된 증거, 중단 조건</li>
    </ul>
  </div>
</div>

운영·실시간 테스트는 명시적 승인이 필요하며 **기본 출발점이 아니에요.** 파트너는 고객의 야망을 거절하는 게 아니라 **방어 가능한 첫 워크플로우로 바꿔주는** 역할이에요.

## 3단계 — 접근 통제, 도구 권한, 데이터 처리

<div class="oa-eli">
  <span class="oa-eli__head">🔑 쉽게 말하면</span>
  <p>집 청소 도우미에게 <b>현관 열쇠</b>를 주는 것과 <b>금고 비밀번호·자동차 키·은행 카드</b>까지 주는 건 완전히 다르죠. 같은 "청소"라도 무엇을 열 수 있느냐에 따라 위험이 달라져요. 워크플로우도 <b>할 일에 딱 맞는 만큼만</b> 열어줘야 해요.</p>
</div>

접근은 행정 디테일이 아니라 **솔루션 설계의 일부**예요. 저장소 하나를 읽기 전용으로 리뷰하는 건 비교적 좁지만, 브랜치 생성·내부 시스템 연결·테스트 실행·로그 접근·티켓 업데이트가 가능해지면 위험이 달라져요.

**접근 확인 포인트 (강의 기준)**: Codex Security는 research preview예요. Security Cloud 사용 전 의도한 신원·저장소에 대한 **활성 접근**을 확인하고, Enterprise/Edu는 Codex Cloud와 Codex Security 권한이 모두 필요하며 스캔 관리 권한은 별도예요. Daybreak Blue는 다른 제품·경로로 확장되지 않고, 파트너가 수행하는 작업이면 파트너의 승인된 접근이 해당 고객 참여를 커버하는지도 확인해요. **저장소 권한만으로는 부족해요.**

### 사람 · 시스템 · 행동

| 범주 | 영업·GTM이 들을 것 | 기술·배포가 확인할 것 |
|---|---|---|
| 👤 People | 누가 소유·승인·실행·리뷰·확장 요청하나 | 승인된 신원과 활성 접근, 사용·구성 관리·finding 리뷰·변경 승인 권한자 |
| 🗄️ Systems | 어떤 저장소·브랜치·finding·로그·티켓·정책·환경 | surface, 저장소 연결, 브랜치·리비전, 환경, 허용 도구, 증거 보관 위치 |
| ⚡ Actions | 워크플로우가 무엇을 해도 되나 | 검사·테스트·finding 게시·변경 제안·증거 내보내기·반복 실행·후속 행동을 **각각** 확인 |

가장 중요한 기술 질문은 "도구가 이걸 할 수 있나?"가 아니라 **"고객이 승인한 운영 모델이 이 워크플로우가 이 환경에서 이 데이터로 이 리뷰 경로 아래 이 행동을 하도록 허락하나?"**예요.

### 행동 경계 체크리스트 (코스가 만든 운영 도구, 제품 모드 아님)

<div class="oa-svg">
<svg viewBox="0 0 720 210" role="img" aria-label="최소 권한에서 시작하는 행동 경계 사다리">
  <defs>
    <style>.a1{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.a2{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.a3{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.t1{font-size:12px;fill:#4a4a4a;font-weight:700}.t2{font-size:11px;fill:#6a6a6a}</style>
  </defs>
  <rect x="10" y="150" width="96" height="44" rx="8" class="a1"/><text x="58" y="170" text-anchor="middle" class="t1">Review-only</text><text x="58" y="186" text-anchor="middle" class="t2">보고서만</text>
  <rect x="112" y="128" width="96" height="66" rx="8" class="a1"/><text x="160" y="152" text-anchor="middle" class="t1">승인된 테스트</text><text x="160" y="170" text-anchor="middle" class="t2">승인 환경에서</text>
  <rect x="214" y="106" width="96" height="88" rx="8" class="a2"/><text x="262" y="130" text-anchor="middle" class="t1">변경 준비</text><text x="262" y="148" text-anchor="middle" class="t2">별도 브랜치</text>
  <rect x="316" y="84" width="96" height="110" rx="8" class="a2"/><text x="364" y="108" text-anchor="middle" class="t1">PR 준비</text><text x="364" y="126" text-anchor="middle" class="t2">사람 리뷰용</text>
  <rect x="418" y="62" width="96" height="132" rx="8" class="a2"/><text x="466" y="86" text-anchor="middle" class="t1">증거 게시·내보내기</text><text x="466" y="104" text-anchor="middle" class="t2">승인된 곳에만</text>
  <rect x="520" y="40" width="96" height="154" rx="8" class="a3"/><text x="568" y="64" text-anchor="middle" class="t1">반복 실행</text><text x="568" y="82" text-anchor="middle" class="t2">일정·이벤트</text>
  <rect x="622" y="18" width="88" height="176" rx="8" class="a3"/><text x="666" y="42" text-anchor="middle" class="t1">후속 행동</text><text x="666" y="60" text-anchor="middle" class="t2">하류 시스템</text><text x="666" y="76" text-anchor="middle" class="t2">영향</text>
  <text x="10" y="24" class="t2">← 첫 워크플로우는 질문에 답할 수 있는 가장 낮은 칸에서 시작</text>
</svg>
</div>

finding 검증 개선만 알고 싶다면 **review-only**로 충분해요. 수정 지원은 리뷰 관문·테스트 기대치·롤백 가정이 명확해진 뒤에 고려해요. Codex Security가 자동으로 commit·merge·배포·티켓 종결·위험 수용을 한다고 암시하지 않아요. 경계를 넓히기 전엔 누가 권한 변경을 승인하는지, 환경·신원·접근 도구·보관 로그·리뷰어를 확인해요.

### 데이터 처리, 비밀, 보관

데이터 처리 결정은 첫 프롬프트만이 아니라 워크플로우가 쓰거나 만드는 **모든 것**(소스, finding, 로그, 보고서, 임시 파일, 저장소 clone, 내보낸 증거)에 적용돼요. 확인할 것: 허용·금지 데이터 유형, 비밀·자격 증명·운영 데이터가 범위에 나타날 수 있는지와 그 처리(제외·가림), 임시 산출물 보관 여부, 최종 증거 저장 위치·접근자·보관 기간, 작업 후 삭제할 것. **최종 보고서가 안전해 보여도 중간 산출물에 민감 정보가 남을 수 있어요.** surface나 보관 위치가 바뀌면 데이터 처리 확인을 다시 해요. 확인되지 않은 특수 보관 조건·ZDR·접근 예외는 약속하지 않아요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 이렇게 말하기</span>
    <ul>
      <li>"먼저 워크플로우 범위를 정하고, 보안 소유자·OpenAI 계정팀과 필요한 접근 경로를 확인합시다."</li>
      <li>"워크플로우는 승인 범위에 필요한 시스템·산출물에만 접근해야 합니다."</li>
      <li>"수정 제안은 여전히 고객의 리뷰·승인이 필요합니다."</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 피하기</span>
    <ul>
      <li>"사이버 모델 받아드릴게요." / "거절이 없어집니다."</li>
      <li>"Trusted Access는 자동입니다."</li>
      <li>"특수 데이터 처리·배포 조건이 포함돼요."</li>
      <li>"자동으로 고쳐서 배포합니다."</li>
    </ul>
  </div>
</div>

## 4단계 — 감시, 감사, 증거 보관, 기록 시스템

Codex 지원 보안 워크플로우의 산출물은 **사람이 리뷰할 증거**이지, 자동 티켓 큐·종결 결정·운영 수정·위험 수용이 아니에요.

### 결정 등급 증거 기록 (decision-grade evidence record)

<div class="oa-eli">
  <span class="oa-eli__head">📦 쉽게 말하면</span>
  <p>택배 송장에는 <b>보낸 곳, 받는 곳, 무엇이 들었는지, 언제 누가 받았는지</b>가 다 적혀 있어서 문제가 생기면 추적할 수 있어요. 증거 기록도 "어느 저장소를, 어느 버전으로, 무엇을 확인했고, 누가 어떻게 판단했고, 어디에 남겼는지"를 한 장에 모은 <b>송장</b>이에요.</p>
</div>

| 요소 | 담을 것 |
|---|---|
| 실행 식별자 | 실행·리뷰 ID, surface, 트리거, 실제 완료/중단 상태 |
| 범위 경계 | 리뷰한 것과 범위 밖 |
| 승인된 산출물 | 저장소, 정확한 리비전, 경로, 연결된 티켓·finding ID |
| 환경 | 로컬 checkout, sandbox, 하위 환경, CI형 워커 등 |
| Source fact / 검증 방법 | 직접 보이는 것 / 실제로 확인한 것과 그 승인 (관찰된 테스트 vs 제안·대기 중인 검증 구분) |
| Supported inference / Proof gap | 합리적 결론 / 테스트·리뷰·재현·승인 안 된 것 |
| 제안 조치 | 조사·억제·보류·수정·재테스트·에스컬레이션·기록 |
| 리뷰어 처분 | 이름 있는 리뷰어, 처분과 이유, 남은 불확실성, 다음 검증 담당 |
| 기록 시스템 항목 | 승인된 기록, 연결 증거, 상태, 소유자, 다음 조치 (중복은 원본에 연결) |
| 보관·정리 상태 | 보관할 증거와 지울 임시 산출물 |

빠진 정보는 **빠졌다고 명시**해요. 불완전한 기록을 완료된 리뷰의 증거로 취급하지 않아요. **스캔 완료 ≠ finding 수용·해결.**

### 기록 시스템은 고객 소유

공식 finding·티켓·수용된 수정·예외·위험 수용·백로그 항목·감사 기록·종결 메모는 **고객이** 정해요. Daybreak는 기존 스캐너·티켓·정책·위험 대장·CI·승인·감사 프로세스를 기본적으로 대체하는 게 아니라 **보완**해요. 많은 보안팀의 문제는 신호 부족이 아니라, 신호를 **검증되고 우선순위가 매겨진 검토 가능한 행동**으로 바꾸는 것이에요.

## 5단계 — 누가 결정하나

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🤖 Codex 지원 실행이 도울 수 있는 것</span>
    <ul>
      <li>범위 내 산출물 리뷰</li>
      <li>finding·증거 구조화, 리뷰용 형식으로 정리</li>
      <li>제한적 수정 제안, 보고서·리뷰 자료 준비</li>
      <li>승인된 경우 승인된 검사 실행</li>
      <li>잔여 위험·후속 항목 요약</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">👤 고객이 결정하는 것</span>
    <ul>
      <li>범위, 자산·환경 승인, 데이터 처리 허용</li>
      <li>위험이 실제·중대한지, 심각도가 타당한지</li>
      <li>수정 수용, merge·릴리스·보류·종결</li>
      <li>운영 승인, 증거 보관, 공식 기록</li>
    </ul>
  </div>
</div>

메시지는 "Codex가 기대보다 덜 한다"가 아니라 **"Daybreak는 고객의 책임을 지키면서 거버넌스 있는 워크플로우를 더 빠르게 지나가게 돕는다"**예요.

| 리뷰어 | 검증하는 것 |
|---|---|
| 보안 | 위험, 심각도, 증거 품질, 정책 정합성, 에스컬레이션 필요 |
| 엔지니어링 | 코드 영향, 테스트 커버리지, 유지보수성, 의존성 영향, 운영 위험 |
| 비즈니스·리스크·법무·컴플라이언스·아키텍처 | 잔여 위험, 예외, 규제 데이터, 핵심 시스템, 운영 활동, 민감 접근 |

리뷰 관문은 **산출물이 나오기 전에** 정해요: *"이 산출물이 티켓·위험 기록·코드 변경·비즈니스 결정에 영향을 주기 전에 누가 리뷰해야 하나?"*

## 6단계 — 진행 · 중단 · 에스컬레이션 · 확장

| 다음 단계 | 언제 | 의미 |
|---|---|---|
| 🟢 Proceed | 승인·제한·리뷰·증거·고객 소유가 다 갖춰짐 | 명확한 제한적 다음 행동으로 (≠ "전부 자동화") |
| 🟡 Pause | 필수 조건 하나가 불명확 | 빠진 조건과 해결할 고객 담당자를 지명 |
| 🟠 Escalate | 민감·고급 요청, OpenAI 확인 필요 | 주장·추천 전에 OpenAI·파트너 SME 투입 |
| 🔵 Expand | 제한된 워크플로우가 증거 품질·리뷰 가능성·통제를 입증 | 리뷰 관문·고객 결정은 유지하며 반복성 확대 |

**Proceed 조건**: 소유 자산, 승인 환경, surface·신원·참여에 대한 활성 접근 확인, 허용 데이터, 도구 권한, 리뷰어, 증거 기대치, 중단 조건, 기록 시스템 경로.

**Pause**는 기회를 무기한 묶어두는 게 아니에요. 예: *"고객이 승인 환경을 확인하고 AppSec 리뷰어를 지명할 때까지 멈춥시다. 그다음 제한된 워크플로우를 다시 봅시다."* 사람 리뷰를 건너뛰려 할 때, 미승인 운영 테스트·자동 수정 전, 반복 작업의 승인 만료나 접근·예산·리뷰 여력·구성 소유권이 무너졌을 때도 멈춰요.

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">❓</span><span class="oa-card__title">승인·소유 불명</span>제3자 대상, 불확실한 자산 소유</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🏭</span><span class="oa-card__title">운영·실대상</span>운영 테스트, 실시간·파괴적 테스트, 승인 환경 밖 활동</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🧨</span><span class="oa-card__title">고급·공격 성격</span>공격 위주 워크플로우, 레드팀, 악성코드 인접 분석, 리버스 엔지니어링</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔑</span><span class="oa-card__title">접근·가용성</span>사이버 특화 접근, 미확인 Daybreak 활성화, 거절 마찰 주장</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔌</span><span class="oa-card__title">통합</span>맞춤 harness, API·headless, MCP·외부 도구, CI/CD 자동화</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">💼</span><span class="oa-card__title">민감 맥락·상업</span>핵심 인프라, 규제 환경, 특권 접근 / 가격, 특수 데이터 처리, ZDR, 재판매·프록시</div>
</div>

에스컬레이션은 **구체적으로** 해요. "에스컬레이션해야 해요"가 아니라 *"이 요청엔 운영 테스트와 맞춤 도구 통합이 포함돼 있으니, 경로를 추천하기 전에 승인 범위·런타임 경계·데이터 처리·리뷰 통제를 적합한 OpenAI·기술 지원과 확인합시다."*

**Expand의 성숙 경로**: ① 수동 제한 워크플로우 → ② 반복 저장소 커버리지 → ③ 기록 시스템 통합 → ④ 통제된 CI/CD·트리거 리뷰. 첫 워크플로우가 산출물을 냈다는 이유만으로 확장하지 않아요. 반복 Security Cloud 감시 전엔 저장소·감시 범위, 기간, 접근·허용 행동, 리뷰어·여력, 구성·예산 소유자, 중단 조건을 확인하고, 토큰 과금이므로 유료 사용 opt-in도 확인해요 (강의 기준). **성공한 1회 리뷰는 지속 감시를 승인하지 않아요.**

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. "백로그 전체에서 finding을 자동 검증해 주세요. 빨라져야 해요." 운영 모델을 먼저 꺼내야 하는 가장 강한 이유는?</summary>
  <p><b>정답: 작업 전에 승인된 워크플로우·승인 자산·런타임 경계·증거 경로·고객 소유 결정을 확인하게 해주기 때문.</b> (기존 도구 얘기 회피 ✗ / 위험 결정 대체 증명 ✗ / 임원 발표용 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q2. 제한된 Daybreak 다음 단계로 논의할 만큼 준비된 첫 워크플로우는?</summary>
  <p><b>정답: 승인된 하위 환경의 고객 소유 저장소 + 이름 있는 AppSec·엔지니어링 리뷰어 + 합의된 증거 기대치 + 중단 조건.</b> (인터넷 노출 전체 + 자동 종결 ✗ / 운영 먼저 ✗ / 아마 허락 있을 제3자 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q3. 접근 결정을 범주에 연결하라 (매칭).</summary>
  <p>코드 검사·finding 게시·테스트·변경 준비 여부 → <b>Actions</b> / 사용·스캔 설정 관리·finding 리뷰 권한자 → <b>People</b> / 로그·정책·리뷰 기록을 context로 써도 되는지 → <b>Systems</b> / 승인된 surface·저장소 연결·환경·증거 보관 위치 → <b>Systems</b> / 예산·리뷰 여력·반복 작업 중단 결정 소유자 → <b>People</b></p>
</details>
<details class="oa-quiz">
  <summary>Q4. 증거 요소와 의미를 연결하라 (매칭).</summary>
  <p>직접 보이는 것 → <b>Source fact</b> / 증명·테스트·리뷰 안 된 것 → <b>Proof gap</b> / 증거에서 나온 합리적 결론 → <b>Supported inference</b> / 리뷰어의 처분·이유·다음 리뷰 → <b>Reviewer decision</b> / 리뷰한 것과 범위 밖 → <b>Scope</b> / 연결 증거·결정·다음 소유자·조치가 보관되는 곳 → <b>System-of-record entry</b></p>
</details>
<details class="oa-quiz">
  <summary>Q5. 작업·결정을 소유자 유형에 연결하라 (매칭).</summary>
  <p>merge·릴리스 승인, 위험이 수정이 필요할 만큼 중대한지 판단, 공식 종결 기록 → <b>고객 결정</b> / 승인된 산출물로 finding 요약 작성, 승인된 타깃 검사 실행, 증거를 리뷰 형식으로 정리 → <b>Codex 지원 실행</b></p>
</details>
<details class="oa-quiz">
  <summary>Q6. 수동 리뷰 1회를 마치고 증거·리뷰어 결정을 보관했다. 같은 저장소에 Security Cloud 감시를 제안하지만 감시 범위·기간·예산 소유자·리뷰 여력이 미합의. 최선은?</summary>
  <p><b>정답: 감시 범위·접근·예산·리뷰 여력·운영 책임이 확인될 때까지 활성화를 멈춘다(Pause).</b> (한 사이클 먼저 돌리기 ✗ / 사이버 특화 접근 에스컬레이션 ✗)</p>
</details>

## 정리

- 거버넌스는 Daybreak의 **운영 모델 그 자체**다. 6가지 질문으로 넓은 관심을 구체적 준비도로 바꾼다.
- 첫 워크플로우는 **4요소 범위 문장**(자산·환경/접근·소유자/리뷰어·증거/중단)으로 요약한다.
- 접근은 **사람·시스템·행동**으로 나누고, **최소 권한 행동 경계**에서 시작한다. 데이터 처리는 임시 산출물까지 포함한다.
- 산출물은 **결정 등급 증거 기록**으로 만들고, 공식 기록은 **고객 시스템**에 남긴다.
- Codex는 실행을 돕고, **고객이 결정**한다. 다음 단계는 **Proceed · Pause · Escalate · Expand** 중 근거와 함께 고른다.
