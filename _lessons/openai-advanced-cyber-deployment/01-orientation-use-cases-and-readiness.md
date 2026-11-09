---
title: "Advanced Cyber Deployment: Orientation, Use Cases and Deployment Readiness"
title_ko: "오리엔테이션 — Daybreak 스택, Blue/Red 경로, 거절 진단, 준비도 점검"
course: openai-advanced-cyber-deployment
lesson: 1
---

## 학습 목표

- Daybreak 보안 스택의 세 층 — **모델 · Codex · Codex Security** — 이 각각 무슨 일을 하는지 설명하기 (강의 기준)
- **Daybreak Blue**와 **Daybreak Red**를 비교하고, 작업에 맞는 경로 고르기
- **시스템 거절 · 모델 거절 · 추가 안전 점검 · 모니터링 중단**을 구분하고, 조사에 필요한 증거 말하기
- 주요 사용 사례 3분류를 알맞은 경로에 연결하기
- 고객 생명주기 4단계 **run → patch → audit → embed** 설명하기
- 작업이 **승인·범위·소유·준비**를 갖췄는지 판단하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>스택 이해</strong>추론 · 연결 · 전문 워크플로우</li>
  <li><strong>경로 선택</strong>Blue가 기본, Red는 전문</li>
  <li><strong>멈춤 진단</strong>어디서 멈췄나?</li>
  <li><strong>사용 사례</strong>SDLC · 방어 운영 · 승인된 공격 테스트</li>
  <li><strong>생명주기</strong>run → patch → audit → embed</li>
  <li><strong>준비도</strong>승인·범위·소유·준비</li>
</ol>

이 코스를 관통하는 한 문장:

> **실행하기 전에 작업을 이해하고 승인받아라.** 작업이 요구하는 경로를 고르고, 범위를 명시하고, 사람의 판단을 유지하라.

## 1단계 — Daybreak 보안 스택의 세 층

<div class="oa-eli">
  <span class="oa-eli__head">🏥 쉽게 말하면</span>
  <p>병원을 떠올려 보세요. <b>의사의 두뇌</b>(판단력)가 있고, 그 의사가 환자 차트·검사 장비·간호사와 연결되는 <b>병원 시스템</b>이 있고, 마지막으로 "응급실 진료 절차"처럼 정해진 <b>전문 프로토콜</b>이 있어요. 셋 다 필요하지만 서로 같은 것이 아니죠. Daybreak도 똑같이 <b>모델(두뇌) · Codex(병원 시스템) · Codex Security(전문 프로토콜)</b>로 나뉘어요.</p>
</div>

흔한 실수는 세 이름을 **같은 것처럼 섞어 쓰는 것**이에요. 함께 일하지만 층이 달라요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">모델 — 추론 층</span>과제를 해석하고 주어진 context 위에서 추론. 예: GPT-6 Astra (강의 기준) — Codex Security나 Daybreak 접근 경로의 다른 이름이 아님</div>
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">Codex — 하네스(harness)</span>추론을 코드·도구·테스트·사람 리뷰에 연결. 작업의 "틀"을 주지만 보안 워크플로우 자체를 정하진 않음</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">Codex Security — 전문 에이전트</span>전문가가 만든 스킬로 보안 작업을 반복 가능한 워크플로우로 구조화</div>
</div>

**모델 선택 주의 (강의 기준)**: 승인된 신원(identity)과 제품 surface가 지원하는 모델인지 먼저 확인해요. "일반 공개됐다"는 사실이 전문 접근권이나 호환성을 보장하지 않아요.

### Codex Security가 보안 작업을 나누는 4단계

| 단계 | 하는 일 |
|---|---|
| 조사 (Investigation) | 승인된 보안 질문과 관련 소프트웨어 context를 살핌 |
| 검증 (Validation) | 가진 증거가 실제로 무엇을 뒷받침하는지 판단 |
| 수정 (Remediation) | 증거가 뒷받침할 때 보안 변경이나 다음 단계를 준비 |
| 보고 (Reporting) | finding·증거·결과를 리뷰 가능하게 정리 |

<div class="oa-svg">
<svg viewBox="0 0 720 200" role="img" aria-label="세 층이 하나의 리뷰 가능한 보안 워크플로우로 이어지는 흐름">
  <defs>
    <style>.c1{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.c2{fill:#eef4ff;stroke:#c6d6f3;stroke-width:1.5}.c3{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.h{font-size:14px;fill:#4a4a4a;font-weight:700}.s{font-size:11px;fill:#6a6a6a}.a{stroke:#b9b9b9;stroke-width:2;fill:none}</style>
  </defs>
  <rect x="10" y="40" width="200" height="110" rx="12" class="c1"/>
  <text x="110" y="75" text-anchor="middle" class="h">모델</text>
  <text x="110" y="100" text-anchor="middle" class="s">보안 질문을 추론</text>
  <text x="110" y="120" text-anchor="middle" class="s">"무엇이 문제일까?"</text>
  <path d="M215 95 L250 95" class="a"/><path d="M244 89 L252 95 L244 101" class="a"/>
  <rect x="260" y="40" width="200" height="110" rx="12" class="c2"/>
  <text x="360" y="75" text-anchor="middle" class="h">Codex</text>
  <text x="360" y="100" text-anchor="middle" class="s">코드·도구·테스트·리뷰어</text>
  <text x="360" y="120" text-anchor="middle" class="s">"실제 코드에서 확인"</text>
  <path d="M465 95 L500 95" class="a"/><path d="M494 89 L502 95 L494 101" class="a"/>
  <rect x="510" y="40" width="200" height="110" rx="12" class="c3"/>
  <text x="610" y="75" text-anchor="middle" class="h">Codex Security</text>
  <text x="610" y="100" text-anchor="middle" class="s">조사·검증·수정·보고</text>
  <text x="610" y="120" text-anchor="middle" class="s">"리뷰 가능한 결과"</text>
  <text x="360" y="185" text-anchor="middle" class="s">어느 한 층도 세 가지 일을 혼자 하지 않는다</text>
</svg>
</div>

**예시** — 승인된 저장소 보안 리뷰: 모델이 보안 질문을 해석하고, Codex가 그 추론을 실제 코드·도구·테스트·리뷰어에 연결하고, Codex Security가 조사 → 증거 확인 → (적절하면) 수정 경로 준비 → 리뷰용 정리를 구조화해요.

## 2단계 — Daybreak Blue vs Daybreak Red

<div class="oa-eli">
  <span class="oa-eli__head">🚒 쉽게 말하면</span>
  <p>소방서에는 <b>일반 소방차</b>와 <b>특수 구조대</b>가 있어요. 대부분의 화재는 일반 소방차가 맡고, 특수 구조대는 "고층 건물 구조"처럼 그 장비가 꼭 필요할 때만 출동해요. 특수 구조대가 "더 세 보인다"고 모든 화재에 보내지 않죠. <b>Blue = 일반 소방차, Red = 특수 구조대</b>예요.</p>
</div>

경로는 **승인된 작업이 무엇을 요구하느냐**로 고르지, "어느 쪽이 더 고급 같으냐"로 고르지 않아요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🔵 Daybreak Blue — 여기서 시작</span>
    <ul>
      <li>취약점 발견과 분류(triage)</li>
      <li>보안 코드 리뷰</li>
      <li>악성코드 분석</li>
      <li>탐지 엔지니어링·사고 대응</li>
      <li>패치 검증</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🔴 Daybreak Red — 전문 작업만</span>
    <ul>
      <li>개념 증명(PoC) 개발</li>
      <li>공격 체인 검증</li>
      <li>침투 테스트·레드팀</li>
      <li>명시적으로 승인되고, 그 전문 작업이 목적에 꼭 필요할 때만</li>
    </ul>
  </div>
</div>

| 결정 지점 | Daybreak Blue | Daybreak Red |
|---|---|---|
| 출발점 | 넓은 방어적 보안 작업의 기본 | 승인된 워크플로우가 전문 역량을 요구할 때 |
| 선택 논리 | 주로 방어적이고 전문 경로가 필요 없음 | 명시적으로 승인됐고 전문 작업이 결과에 필수 |

> **경로를 고르기 전에**: 대상(target), 운영자(operator), 환경, 승인, 의도한 효과 중 하나라도 불분명하면 **명확화 단계에서 멈춰요.** 아직 경로 선택 단계가 아니에요.

**예시 비교**
- 출시 전 자사 저장소의 취약점을 검토·분류하고 패치를 검증 → **Blue** (넓은 방어 작업)
- 별도로 승인된 보안 연구 계약에서 허가된 환경의 레드팀 연습 → **Red** (전문 역량이 명시적으로 필요)

## 3단계 — 거절과 안전 중단 진단하기

<div class="oa-eli">
  <span class="oa-eli__head">📦 쉽게 말하면</span>
  <p>택배가 안 왔을 때 원인은 여러 가지예요. ① <b>물류센터에서 반송</b>(가게에서 출발도 못 함), ② <b>가게가 "이건 못 보내요"라는 쪽지</b>를 넣어 보냄, ③ <b>통관 검사 중</b>이라 잠깐 지연, ④ 배송 중간에 <b>운송이 중지</b>됨(일부는 이미 배달됨). 원인마다 해야 할 일이 다르니, 먼저 <b>"어디서 멈췄나"</b>를 정확히 봐야 해요.</p>
</div>

경로를 고르는 것과 멈춘 작업을 조사하는 건 **다른 결정**이에요. 정당하고 승인된 요청도 거절되거나, 추가 점검으로 대기하거나, 시작 후 중단될 수 있어요.

| 관찰된 신호 | 무슨 일이 일어났나 | 보존할 증거 | 대응 |
|---|---|---|---|
| **시스템 거절** (상류) | 모델 응답이 나오기 전에 차단·우회됨. 보이는 건 상류 오류 | 실제 상태·오류 코드·오류 상세 | 실제 응답으로 진단. 특정 상태 코드 하나를 영구적 정의로 여기지 않기 |
| **모델 거절** (응답 안) | 모델에 도달했지만 응답 자체가 거절하거나 방향을 돌림 | 정확한 요청 + 전체 모델 응답 | 의도한 방어 목적의 맥락에서 무엇이 거절됐는지 검토 |
| **추가 안전 점검** | 반환 전 콘텐츠를 점검 중이라는 안내 | 실제 안내, 시간, 이후 결과 | 점검 중엔 할 일 없음. **지연 ≠ 거절 ≠ 정책 위반** |
| **모니터링 중단** | 지원되는 API 워크플로우에서 모니터링이 우려를 감지해 대화를 중단. 도구가 이미 실행됐을 수 있음 | 중단 안내, 요청 ID·시간, 도구 기록, 변경 사항, 미완료 작업 | 그 대화의 추가 작업 중지, 기록 보존, 책임 운영자가 완료된 작업 검토. **자동 재시도 금지** |

### 해석 전에 보존할 것

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📝</span><span class="oa-card__title">요청 맥락</span>정확한 요청, 선택한 모델, 제품 surface, 조직, 사용자, 의도한 방어 목적</div>
  <div class="oa-card"><span class="oa-card__icon">📸</span><span class="oa-card__title">실제 신호</span>실제 안내·오류·응답, 요청 ID와 시간 — 기억 속 "대충 이런 오류"로 대체 금지</div>
  <div class="oa-card"><span class="oa-card__icon">🔧</span><span class="oa-card__title">실행 흔적</span>어떤 도구가 돌았고, 무엇이 바뀌었고, 무엇이 미완료인지</div>
  <div class="oa-card"><span class="oa-card__icon">🔒</span><span class="oa-card__title">공유 방식</span>자격 증명·불필요한 민감 정보는 가린 기록을 승인된 리뷰 경로로</div>
</div>

**중단은 이미 한 일을 되돌리지 않아요.** 예를 들어 도구가 파일을 쓴 뒤 모니터링 중단이 일어나면, 리뷰는 완료된 것도 아니고 파일이 원래대로 복구된 것도 아니에요. 실제로 무엇이 실행·변경됐는지 운영자와 검토해요. 또 모니터링 적용 범위와 자동 중단은 API와 대화 context 보존 방식에 따라 달라서, 모든 Security surface가 똑같이 동작한다고 가정하지 않아요 (강의 기준).

### 거절 행동은 승인을 정의하지 않는다

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">질문 1 — 기술적으로 무슨 일이?</span>
    <p>실제 거절·점검 안내·모니터링 중단이 무엇이었고, 이미 수행된 작업은 무엇인가</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">질문 2 — 이 작업은 승인됐나?</span>
    <p>대상·운영자·환경·승인·의도한 효과를 확인</p>
  </div>
</div>

두 질문은 **분리**해요. 시스템 거절 마찰이 줄었다고 미승인 대상이 승인되는 것도 아니고, 요청이 통과됐다고 맥락 확인을 건너뛸 수도 없어요. 모니터링은 정당한 작업을 표시할 수도, 문제를 놓칠 수도 있어요.

## 4단계 — Daybreak 사용 사례 3분류

고객의 **원하는 보안 결과**에서 출발해, 실제 필요한 작업으로 경로를 고르는 순서예요.

<ol class="oa-flow">
  <li><strong>목표 결과</strong>무엇을 이루려 하나</li>
  <li><strong>작업 분해</strong>실제 하는 일들</li>
  <li><strong>승인 확인</strong>각 작업이 승인됐나</li>
  <li><strong>작업별 라우팅</strong>각자 필요한 경로로</li>
</ol>

| 분류 | 원하는 결과 | 대표 워크플로우 | 대표 경로 (강의 기준) |
|---|---|---|---|
| Secure SDLC·AppSec | 출시 전 취약점 예방, 소프트웨어 위험 감소 | 지속 코드 스캔, 테스트 환경 스캔, finding 검증, 보안 패치 자동화 | Blue + Codex Security |
| 방어 운영 | 운영 환경의 위협 탐지·조사·대응 | 블루팀, 위협 모델링, 위협 인텔리전스, 위협 헌팅 | 대부분 Blue, **위협 헌팅은 Red** |
| 승인된 공격 테스트 | 허가 하에 공격자 관점으로 약점을 찾아 고침 | 침투 테스트, 레드팀, 공격 검증·개발 | 침투 테스트는 Blue·Red, 레드팀·공격 검증은 Red |

워크플로우가 여러 분류에 걸치면, 요청 전체에 라벨 하나를 억지로 붙이지 말고 **밑에 있는 작업들을 각각** 라우팅해요. 예: 악성코드 분석·탐지 개선은 Blue로 시작하고, 별도 승인된 위협 헌팅 부분만 Red로.

### 진행해도 될까? — 4가지 준비도 질문

<div class="oa-eli">
  <span class="oa-eli__head">🏗️ 쉽게 말하면</span>
  <p>집 리모델링을 시작하기 전에 확인할 것: <b>허가</b> 받았나? <b>어디까지</b> 고치나? 이 집이 <b>내 집</b>이거나 주인에게 권한을 받았나? 공사 장비와 <b>감리자</b>가 준비됐나? 하나라도 빠지면 망치를 들기 전에 멈춰야죠.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">✅</span><span class="oa-card__title">Authorized (승인)</span>작업이 명시적으로 승인됐나?</div>
  <div class="oa-card"><span class="oa-card__icon">📏</span><span class="oa-card__title">Bounded (범위)</span>무슨 작업을, 어디까지 하는지 분명한가?</div>
  <div class="oa-card"><span class="oa-card__icon">🏠</span><span class="oa-card__title">Owned (소유)</span>고객이 대상을 소유하거나 명확한 권한이 있나?</div>
  <div class="oa-card"><span class="oa-card__icon">🧭</span><span class="oa-card__title">Ready (준비)</span>환경과 사람 리뷰 경로가 진행할 만큼 분명한가?</div>
</div>

하나라도 빠지면 **실행이 아니라 명확화**에서 멈춰요. 예: 외부 시스템 레드팀 요청에서 소유·승인이 확인 안 됐다면, Red가 기술적으로 맞더라도 경로 선택·실행 전에 멈춰요.

## 5단계 — 고객 생명주기: run → patch → audit → embed

<div class="oa-eli">
  <span class="oa-eli__head">🦷 쉽게 말하면</span>
  <p>치과 치료와 비슷해요. 먼저 <b>검진</b>으로 충치를 찾고(run), 급한 충치를 <b>치료</b>하고(patch), 치약·칫솔 같은 <b>생활 도구를 점검</b>하고(audit), 마지막엔 <b>매일 양치 습관</b>으로 충치를 예방해요(embed). 치료에서 예방으로 무게가 옮겨가는 거예요.</p>
</div>

<ol class="oa-flow">
  <li><strong>Run</strong>핵심 저장소에서 보안 이슈 발견</li>
  <li><strong>Patch</strong>근거 있는 핵심 취약점을 검토된 패치로</li>
  <li><strong>Audit</strong>서드파티 라이브러리 교체·통합·제거 검토</li>
  <li><strong>Embed</strong>PR 워크플로우에 Codex Security 통합</li>
</ol>

| 단계 | 핵심 질문 | 주의 |
|---|---|---|
| Run | 핵심 저장소에서 어떤 보안 이슈를 찾을 수 있나? | 발견된 후보가 모두 검증되거나 패치 준비된 건 아님 |
| Patch | 하류(downstream) 영향을 무시하지 않고 테스트된 수정으로 갈 수 있나? | 진행 여부는 사람이 결정 |
| Audit | 어떤 서드파티 라이브러리를 교체·통합·제거할까? | 범위가 의존성으로 넓어짐 |
| Embed | 어떻게 PR 워크플로우의 일부가 되어 더 일찍 다룰까? | 일회성 수정 → 지속적 예방 |

<div class="oa-bars">
  <div class="oa-bar"><span>Run</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:20%"></span><span class="oa-bar__val">발견</span></span></div>
  <div class="oa-bar"><span>Patch</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:40%"></span><span class="oa-bar__val">수정</span></span></div>
  <div class="oa-bar"><span>Audit</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:65%"></span><span class="oa-bar__val">점검</span></span></div>
  <div class="oa-bar"><span>Embed</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:95%"></span><span class="oa-bar__val">예방</span></span></div>
  <p class="oa-bars__caption">예방 쪽으로 옮겨가는 정도 — 개념적 표현</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 각 층의 주된 역할을 짝지으세요: Codex Security / Codex / 모델</summary>
  <p><b>정답:</b> Codex Security → 전문 조사·검증·수정·보고 워크플로우 구성 / Codex → 추론을 코드·도구·테스트·사람 리뷰에 연결 / 모델 → 복잡한 사이버·소프트웨어 보안 작업을 위한 고급 추론.</p>
</details>
<details class="oa-quiz">
  <summary>Q2. AppSec 팀이 자사 저장소의 취약점을 찾고 분류하고, 코드를 검토하고, 방어 패치를 검증하려 한다. 출발 경로는?</summary>
  <p><b>정답: Daybreak Blue.</b> 발견·분류·리뷰·패치 검증을 아우르는 넓은 방어 작업이기 때문. "전문 경로가 항상 더 강하다"거나 "지금 쓸 수 있는 쪽"은 선택 기준이 아님.</p>
</details>
<details class="oa-quiz">
  <summary>Q3. 지원되는 API 워크플로우가 부분 출력을 낸 뒤 모니터링 중단을 받았다. 동료가 "승인된 작업이니 자동 재시도하자"고 한다. 어떻게 할까?</summary>
  <p><b>정답: 추가 작업을 멈추고, 기록을 보존하고, 책임 운영자에게 완료된 작업과 의도한 작업의 검토를 요청한다.</b> 부분 출력으로 완료 처리하거나, 세부를 줄여 재시도하거나, 플래그만으로 정책 위반을 확정하는 건 모두 오답.</p>
</details>
<details class="oa-quiz">
  <summary>Q4. 외부 시스템 레드팀 요청. Red가 기술적으로 맞지만 대상 소유·테스트 승인이 확인 안 됐다면?</summary>
  <p><b>정답: 경로 선택·실행 전에 멈추고 소유·승인·범위·리뷰 경로를 명확히 한다.</b> "정찰만 하겠다"거나 "일단 Blue로 시작"도 승인 문제를 해결하지 못함.</p>
</details>
<details class="oa-quiz">
  <summary>Q5. 기대되는 고객 생명주기 순서는?</summary>
  <p><b>정답: Run(핵심 저장소) → Patch(핵심 취약점) → Audit(의존성) → Embed(SDLC 통합).</b></p>
</details>

## 정리

- Daybreak는 **모델(추론) · Codex(연결) · Codex Security(전문 워크플로우)** 세 층의 스택이다. 이름을 섞어 쓰지 말자.
- **Blue가 기본**, Red는 명시적으로 승인되고 전문 작업이 꼭 필요할 때만.
- 멈춘 작업은 먼저 **어디서 멈췄는지**(시스템·모델·추가 점검·모니터링 중단)를 실제 기록으로 진단한다. 모니터링 중단 뒤에는 **자동 재시도 금지**, 완료된 작업은 사람이 검토.
- 거절 여부는 **승인 여부가 아니다.** 기술 진단과 승인 판단은 따로.
- 사용 사례는 결과 → 작업 → 승인 → 작업별 경로 순으로, 생명주기는 **run → patch → audit → embed**.
- 실행 전 4가지: **승인 · 범위 · 소유 · 준비.** 하나라도 없으면 명확화에서 멈춘다.
