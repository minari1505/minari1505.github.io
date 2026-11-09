---
title: "Advanced Cyber Deployment Lab: Set Up Codex Security"
title_ko: "랩 — 스캔 전에 준비부터: Codex Security 설정 준비도 기록"
course: openai-advanced-cyber-deployment
lesson: 2
---

## 학습 목표

- 첫 스캔 **전에** 도구·신원·접근·저장소·로컬 환경이 준비됐는지 확인하는 순서 설명하기
- 랩 결과물인 **Setup Readiness Record**(설정 준비도 기록)에 무엇이 들어가고 무엇이 빠져야 하는지 말하기
- 결과를 **Ready / Stopped** 둘 중 하나로 정직하게 기록하는 기준 이해하기
- 훈련용 앱에서 **자산 · 신뢰할 수 없는 입력 지점 · 신뢰 경계**를 하나씩 짚는 이유 설명하기

<div class="oa-eli">
  <span class="oa-eli__head">✈️ 쉽게 말하면</span>
  <p>비행기 조종사는 이륙 전에 <b>체크리스트</b>를 하나씩 확인해요. 연료, 계기판, 관제 허가… 하나라도 빠지면 "출발 보류"라고 적고, 절대 "대충 괜찮겠지" 하고 뜨지 않아요. 이 랩은 Codex Security로 "이륙(스캔)"하기 전의 체크리스트예요. 그래서 <b>이 랩에서는 스캔을 하지 않아요.</b></p>
</div>

## 한눈에 보는 랩 흐름

<ol class="oa-flow">
  <li><strong>가이드 준비</strong>랩 가이드 열어 두기</li>
  <li><strong>도구 버전</strong>개발 도구 확인</li>
  <li><strong>Codex 데스크톱</strong>플러그인·권한·entitlement</li>
  <li><strong>저장소·워크플로우</strong>소유자 승인 확인</li>
  <li><strong>훈련 앱 살펴보기</strong>자산·입력·경계</li>
  <li><strong>Security CLI</strong>설치·로그인·GitHub 확인</li>
  <li><strong>판정 기록</strong>Ready 또는 Stopped</li>
</ol>

## 무엇을 만드나

결과물은 **편집(redact)된 Setup Readiness Record 한 장**이에요. 실습 재료는 랩이 제공하는 **정리된(cleansed) Node.js 훈련용 앱**(강의 기준)이고, 이후 모든 Advanced Cyber Deployment 랩에서 계속 써요. 또는 **본인이 소유하고 평가 권한이 있는 private 저장소**만 사용할 수 있어요.

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⛔</span><span class="oa-card__title">하지 않는 것</span>훈련 앱 실행, 네트워크 노출, 외부 서비스 테스트, Codex Security 스캔</div>
  <div class="oa-card"><span class="oa-card__icon">🏠</span><span class="oa-card__title">로컬에 유지</span>훈련 앱은 마지막 랩(PR 연동)까지 로컬에만. 그때도 이 랩에서 확인한 소유자 통제 private 저장소에만 게시</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🙋</span><span class="oa-card__title">막히면 멈추고 요청</span>접근·승인이 없으면 저장소 소유자나 워크스페이스 관리자에게 연락. 자격 증명을 빌리거나 통제를 우회하지 않음</div>
</div>

## 단계별 요약 — 무엇을 확인하나

| 단계 | 하는 일 (개요) | 이 단계가 확인하는 것 |
|---|---|---|
| 1. 도구 버전 | macOS/Windows에서 Git, GitHub CLI, Node.js, npm, Python 버전 확인 | 이후 랩을 돌릴 기본 개발 환경이 갖춰졌나 |
| 2. Codex 데스크톱 | Codex Security 플러그인 설치·활성화, 워크스페이스 권한과 기능 entitlement 확인 | 내 승인된 신원이 실제로 Codex Security를 쓸 수 있나 |
| 3. 저장소·호스팅 준비 | 소유자 승인 private GitHub 저장소, Issues·Actions 워크플로우 접근, 승인된 모델 예산, **마스킹된 저장소 secret** 존재 확인 | 나중의 호스팅 작업(GitHub Actions)이 승인된 경계 안에서 가능할까 |
| 4. 훈련 앱 열어 보기 | 앱을 **실행하지 않고** 코드만 열어 자산 1개·신뢰할 수 없는 입력 지점 1개·신뢰 경계 1개 식별 | 스캔 전에 "무엇을 지키는지" 맥락을 사람이 이해했나 |
| 5. Security CLI | 최신 Security CLI 설치·확인, 승인된 로컬 신원으로 로그인, 해당 저장소에 적용되는 GitHub 계정·워크플로우 접근 확인 | CLI 신원과 GitHub 신원이 각각 올바르고 분리돼 있나 |
| 6. 판정 | 실제 결과를 Ready 또는 Stopped로 기록, 스캔은 하지 않음 | 기록이 실제로 확인한 것만 말하나 |

저장소 secret 이름은 강의에서 `CODEX_SECURITY_API_KEY`로 안내돼요 (강의 기준). 기록에는 **"존재하고 마스킹돼 있음"만** 적고, 값은 절대 적지 않아요 — 예: `CODEX_SECURITY_API_KEY = <마스킹됨, 값 미기재>`.

### 자산 · 신뢰할 수 없는 입력 지점 · 신뢰 경계

<div class="oa-eli">
  <span class="oa-eli__head">🏦 쉽게 말하면</span>
  <p>은행으로 치면 <b>자산</b>은 금고 속 돈, <b>신뢰할 수 없는 입력 지점</b>은 누구나 들어올 수 있는 정문, <b>신뢰 경계</b>는 손님 구역과 직원 전용 구역 사이의 문이에요. 보안 점검은 "정문으로 들어온 것이 경계를 넘어 금고까지 갈 수 있나?"를 묻는 일이에요. 이 세 가지를 먼저 알아야 스캔 결과도 해석할 수 있어요.</p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 170" role="img" aria-label="입력 지점에서 신뢰 경계를 넘어 자산으로 이어지는 개념도">
  <defs>
    <style>.u{fill:#fff4f0;stroke:#f1c9b8;stroke-width:1.5}.t{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.h{font-size:14px;fill:#4a4a4a;font-weight:700}.s{font-size:11px;fill:#6a6a6a}.a{stroke:#b9b9b9;stroke-width:2;fill:none}.bd{stroke:#d9a441;stroke-width:3;stroke-dasharray:8 6}</style>
  </defs>
  <rect x="10" y="30" width="300" height="110" rx="12" class="u"/>
  <text x="160" y="62" text-anchor="middle" class="h">신뢰할 수 없는 영역</text>
  <text x="160" y="90" text-anchor="middle" class="s">입력 지점: 사용자가 보내는 요청·폼·파일</text>
  <text x="160" y="110" text-anchor="middle" class="s">"누구나 보낼 수 있다"</text>
  <line x1="360" y1="20" x2="360" y2="150" class="bd"/>
  <text x="360" y="166" text-anchor="middle" class="s">신뢰 경계</text>
  <path d="M312 85 L398 85" class="a"/><path d="M392 79 L400 85 L392 91" class="a"/>
  <rect x="410" y="30" width="300" height="110" rx="12" class="t"/>
  <text x="560" y="62" text-anchor="middle" class="h">신뢰 영역</text>
  <text x="560" y="90" text-anchor="middle" class="s">자산: 지켜야 할 데이터·기능</text>
  <text x="560" y="110" text-anchor="middle" class="s">"경계를 넘으면 검증이 필요"</text>
</svg>
</div>

이 셋은 **훈련 앱에 구체적이어야** 해요. "웹 앱엔 보통 입력이 있다" 같은 일반론은 통과 기준에 맞지 않아요.

## Ready vs Stopped — 정직한 판정

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ Ready</span>
    <ul>
      <li>필요한 설정 조건이 <b>모두</b> 있고 승인됨</li>
      <li>기록이 실제로 확인한 도구·신원·권한·저장소 상태를 반영</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⏸️ Stopped</span>
    <ul>
      <li>전제 조건이나 승인이 하나라도 빠짐</li>
      <li>무엇이 빠졌는지, 누가(저장소 소유자 / 워크스페이스 관리자) 해결해야 하는지 기록</li>
      <li>임시 우회책을 만들지 않음</li>
    </ul>
  </div>
</div>

> **Stopped는 실패가 아니라 정확한 기록이에요.** 근거 없는 Ready보다 정직한 Stopped가 훨씬 가치 있어요.

## 남길 증거 vs 절대 남기면 안 되는 것

| 남길 증거 | 남기면 안 되는 것 |
|---|---|
| Git·GitHub CLI·Node.js·npm·Python·Security CLI 버전 | 자격 증명, 토큰, API 키, secret 값 |
| 편집된 Codex Security CLI·GitHub 세션 상태 | 편집되지 않은 세션 상세 |
| private 저장소·Issues·Actions·워크플로우 접근이 소유자 승인됐다는 확인 | 빌린 계정·승인 안 된 계정으로 얻은 결과 |
| 모델 예산과 마스킹된 저장소 secret이 준비됐다는 확인 (값 없이) | secret 값을 보여 주는 화면 캡처 |
| 자산·입력 지점·신뢰 경계 각 1개 | 스캔 결과 (이 랩에선 스캔 안 함) |
| 최종 Ready/Stopped와 (Stopped면) blocker·해결 담당 | 더 넓은 접근 요청, 통제 우회 흔적 |

### 경계 규칙 (Boundaries)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🪪</span><span class="oa-card__title">내 신원만</span>본인의 승인된 ChatGPT·GitHub 신원만 사용</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔑</span><span class="oa-card__title">비밀 미포함</span>제출물에 자격 증명·토큰·키·secret·원본 세션 정보 금지</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚧</span><span class="oa-card__title">우회 금지</span>자격 증명 빌리기, 미승인 계정 전환, 더 넓은 접근 요청, 저장소·조직·관리자 통제 우회 금지</div>
  <div class="oa-card"><span class="oa-card__icon">🧪</span><span class="oa-card__title">취약 앱은 잠자게</span>의도적으로 취약한 앱을 실행·노출하지 않음</div>
</div>

## 제출과 리뷰

랩은 **편집된 Setup Readiness Record 한 개를 업로드**하는 것으로 끝나고, 배정된 리뷰어가 다음을 평가해요.

- 증거가 기록된 상태(Ready/Stopped)를 뒷받침하나
- 승인 · 신원 · 자격 증명 · 저장소 · **스캔 안 함** 경계가 지켜졌나

리뷰어가 요청할 때만 수정·재제출해요.

<div class="oa-eli">
  <span class="oa-eli__head">📌 이 블로그 글에 대해</span>
  <p>이 랩은 학습자 <b>본인의 실제 환경</b>에서 확인한 기록을 제출해야 완료돼요. 그래서 이 글은 랩 안내문을 요약한 것이고, 실제 기록 작성·제출은 직접 해야 해요.</p>
</div>

## 지금까지 배운 것 (코스 1–2 요약)

<div class="oa-svg">
<svg viewBox="0 0 720 230" role="img" aria-label="레슨 1과 레슨 2의 핵심을 잇는 요약도">
  <defs>
    <style>.b1{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.b2{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.b3{fill:#eef4ff;stroke:#c6d6f3;stroke-width:1.5}.t1{font-size:13px;fill:#4a4a4a;font-weight:700}.t2{font-size:11px;fill:#6a6a6a}.ar{stroke:#b9b9b9;stroke-width:2;fill:none}</style>
  </defs>
  <rect x="10" y="10" width="340" height="150" rx="12" class="b1"/>
  <text x="180" y="36" text-anchor="middle" class="t1">L1 오리엔테이션</text>
  <text x="180" y="62" text-anchor="middle" class="t2">스택: 모델 · Codex · Codex Security</text>
  <text x="180" y="82" text-anchor="middle" class="t2">경로: Blue 기본, Red는 승인된 전문 작업</text>
  <text x="180" y="102" text-anchor="middle" class="t2">멈춤 진단: 시스템·모델·추가 점검·모니터링</text>
  <text x="180" y="122" text-anchor="middle" class="t2">생명주기: run → patch → audit → embed</text>
  <text x="180" y="142" text-anchor="middle" class="t2">준비도: 승인 · 범위 · 소유 · 준비</text>
  <path d="M355 85 L385 85" class="ar"/><path d="M379 79 L387 85 L379 91" class="ar"/>
  <rect x="390" y="10" width="320" height="150" rx="12" class="b2"/>
  <text x="550" y="36" text-anchor="middle" class="t1">L2 랩: 설정 준비</text>
  <text x="550" y="62" text-anchor="middle" class="t2">도구 버전 · 플러그인 · entitlement</text>
  <text x="550" y="82" text-anchor="middle" class="t2">소유자 승인 저장소 · 마스킹된 secret</text>
  <text x="550" y="102" text-anchor="middle" class="t2">자산 · 입력 지점 · 신뢰 경계</text>
  <text x="550" y="122" text-anchor="middle" class="t2">CLI·GitHub 신원 분리 확인</text>
  <text x="550" y="142" text-anchor="middle" class="t2">판정: Ready 또는 Stopped (스캔 없음)</text>
  <rect x="120" y="178" width="480" height="44" rx="12" class="b3"/>
  <text x="360" y="205" text-anchor="middle" class="t1">"준비도"라는 원칙이 개념(L1)에서 실제 체크리스트(L2)로</text>
</svg>
</div>

| 레슨 | 핵심 개념 | 다음으로 이어지는 것 |
|---|---|---|
| 1. 오리엔테이션 | 세 층 스택, Blue/Red, 멈춤 진단, 사용 사례, 생명주기, 준비도 4질문 | 준비도 원칙을 실제 설정에 적용 |
| 2. 랩: 설정 | 스캔 전 체크리스트와 정직한 Ready/Stopped 기록 | 다음 코스: 실제 스캔과 결과 해석 |

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 저장소 워크플로우 접근 승인이 아직 없다. 기록의 판정은?</summary>
  <p><b>정답: Stopped.</b> 빠진 승인과 그것을 해결할 사람(저장소 소유자 또는 워크스페이스 관리자)을 기록한다. 다른 계정을 빌리거나 우회하지 않는다.</p>
</details>
<details class="oa-quiz">
  <summary>Q2. 마스킹된 저장소 secret이 준비됐음을 기록에 어떻게 남기나?</summary>
  <p><b>정답: 존재하고 마스킹돼 있다는 사실만 적고 값은 절대 적지 않는다.</b> 예: <code>CODEX_SECURITY_API_KEY = &lt;마스킹됨&gt;</code></p>
</details>
<details class="oa-quiz">
  <summary>Q3. 준비가 다 됐으니 이 랩에서 바로 스캔을 돌려도 될까?</summary>
  <p><b>정답: 아니다.</b> 이 랩은 준비 확인만 한다. 훈련 앱을 실행·노출하지 않고, Codex Security 스캔도 하지 않는다.</p>
</details>
<details class="oa-quiz">
  <summary>Q4. 자산·입력 지점·신뢰 경계를 왜 스캔 전에 사람이 직접 짚을까?</summary>
  <p><b>정답:</b> 무엇을 지키는지, 어디로 신뢰할 수 없는 데이터가 들어오는지 알아야 나중에 스캔 결과가 의미 있는지 판단할 수 있기 때문. 훈련 앱에 <b>구체적</b>이어야 한다.</p>
</details>

## 정리

- 이 랩의 결과물은 **편집된 Setup Readiness Record 한 장**, 판정은 **Ready 또는 Stopped**.
- 확인 순서: **도구 버전 → Codex 데스크톱(플러그인·권한·entitlement) → 소유자 승인 저장소·워크플로우·예산·마스킹된 secret → 훈련 앱의 자산·입력·경계 → Security CLI와 GitHub 신원**.
- 빠진 게 있으면 **Stopped + blocker + 해결 담당자**. 우회·빌린 자격 증명은 절대 금지.
- 이 랩에선 **스캔하지 않고**, 훈련 앱은 **로컬에만** 둔다.
- L1의 "실행 전에 이해하고 승인받아라"가 L2에서 실제 체크리스트가 됐다.
