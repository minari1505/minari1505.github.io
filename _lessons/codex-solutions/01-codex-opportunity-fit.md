---
title: "Codex Opportunity Fit"
title_ko: "Codex 기회 적합성 판단 — 'AI 코딩에 관심 있어요'를 증거로 바꾸기"
course: codex-solutions
lesson: 1
---

## 학습 목표

- "로드맵이 자꾸 밀려요", "기술 부채가 많아요" 같은 **넓은 하소연** 뒤에 있는 소프트웨어 개발(SDLC) 병목을 찾아내기
- **Codex Opportunity Fit Lens**의 다섯 가지 점검(반복되는 고통·비즈니스 영향·담당자·준비도·반복 워크플로)과 **과제 실행 가능성(task viability)** 을 써서 신호 강도를 판단하기
- 워크플로 중심 질문으로 **discovery evidence snapshot**(확인된 증거·가설·의미·빈 증거)을 만들기
- 증거에 따라 **Progress / Continue discovery / Pause / Disqualify** 중 하나를 고르고, 필요하면 ChatGPT·API 같은 다른 OpenAI 방향으로 안내하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>개발 압박 알아듣기</strong>5가지 마찰 패턴, SDLC 어디서 막히나</li>
  <li><strong>신호 강도 판단</strong>Fit Lens 5가지 점검 + 과제 실행 가능성</li>
  <li><strong>워크플로 중심 discovery</strong>6단계 질문 경로, 이해관계자 삼각측량</li>
  <li><strong>증거 스냅샷</strong>확인 · 가설 · 의미 · 빈 증거</li>
  <li><strong>진행 결정</strong>Progress · Continue · Pause · Disqualify</li>
</ol>

이 코스를 관통하는 한 문장: **라벨(“개발 생산성”)이 아니라, 그 라벨 뒤의 워크플로와 “검증 가능한 구체적 과제”를 자격 판단한다.**

## 1. 소프트웨어 개발 압박이 왜 기회가 되나

<div class="oa-eli">
  <span class="oa-eli__head">🚚 쉽게 말하면</span>
  <p>택배가 자꾸 늦게 오면 사람들은 "택배사가 느려요"라고만 말해요. 그런데 실제 원인은 분류 센터일 수도, 배송 기사 부족일 수도, 주소 오류일 수도 있죠. 고객이 "개발이 느려요"라고 할 때도 <b>어느 구간이 막혔는지</b>를 찾아야 진짜 문제가 보여요.</p>
</div>

고객은 처음부터 "Codex(강의 기준) 주세요"라고 하지 않아요. 이렇게 말하죠.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📅</span><span class="oa-card__title">"로드맵이 자꾸 밀려요"</span>계획한 일이 제때 안 나감</div>
  <div class="oa-card"><span class="oa-card__icon">👥</span><span class="oa-card__title">"사람을 안 늘리고 더 내야 해요"</span>인력 대비 산출 압박</div>
  <div class="oa-card"><span class="oa-card__icon">🧱</span><span class="oa-card__title">"기술 부채 때문에 느려요"</span>낡은 시스템이 새 작업을 막음</div>
  <div class="oa-card"><span class="oa-card__icon">🧪</span><span class="oa-card__title">"테스트·리뷰가 너무 오래 걸려요"</span>릴리스 날짜 예측이 어려움</div>
</div>

이 말들은 **좋은 출발 신호**지만 그 자체로 자격 증거는 아니에요. 그리고 가장 강한 Codex 대화는 "개발자 한 명이 더 빨리 코딩"이 아니라 **팀의 일이 개발 워크플로 전체를 어떻게 흘러가는지 개선**하는 이야기예요.

### 5가지 개발 마찰 패턴

| 패턴 | 무슨 일이 일어나나 | 들리는 말 | 더 강한 신호 |
|---|---|---|---|
| **처리량(Throughput)** | 일이 시스템을 충분히 빠져나가지 못함 | "백로그가 계속 쌓여요" | 로드맵·고객 약속·경영 우선순위에 영향 |
| **품질(Quality)** | 결함 찾기·고치기에 시간이 과하게 듦 | "문제를 늦게 발견해요" | 안정성·릴리스 일정·고객 신뢰에 영향 |
| **지식(Knowledge)** | 전문 지식이 소수에 몰림, 문서가 낡음 | "이 시스템 아는 사람이 몇 명뿐이에요" | 온보딩 지연·특정인 의존·위험 증가 |
| **조율(Coordination)** | 인계·흩어진 도구·불명확한 담당·반복 리뷰 | "핸드오프가 너무 많아요" | 결과에 영향 + **구체적·검증 가능한 개발 과제**로 이어짐 (조율 자체만은 부족) |
| **현대화(Modernization)** | 레거시·기술 부채가 새 작업을 막음 | "오래된 시스템은 안전하게 못 바꿔요" | 경영진의 명확한 현대화 우선순위 + 느려지는 구체 워크플로 |

패턴은 딱딱한 라벨이 아니에요. "릴리스 승인이 매주 같은 몇 명의 리뷰어에게 달려 있어요"는 **조율 + 지식 + 품질** 압박이 한꺼번에 들어 있을 수 있어요.

> 신호를 들으면 이렇게 묻기: **"이 문제가 어떤 워크플로에 가장 영향을 주고, 그게 느려지면 무슨 일이 생기나요?"**

### SDLC 어디서 막히는지 찾기

<div class="oa-svg">
<svg viewBox="0 0 720 200" role="img" aria-label="소프트웨어 개발 워크플로와 마찰 위치">
  <defs>
    <marker id="x1a" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.x1b{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.x1r{fill:#fff4f2;stroke:#f2c9c2;stroke-width:1.5}.x1t{font-size:13px;fill:#4a4a4a;font-weight:bold}.x1s{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="30" width="100" height="56" rx="10" class="x1b"/><text x="60" y="63" text-anchor="middle" class="x1t">계획</text>
  <rect x="130" y="30" width="100" height="56" rx="10" class="x1b"/><text x="180" y="63" text-anchor="middle" class="x1t">개발</text>
  <rect x="250" y="30" width="100" height="56" rx="10" class="x1r"/><text x="300" y="63" text-anchor="middle" class="x1t">리뷰</text>
  <rect x="370" y="30" width="100" height="56" rx="10" class="x1r"/><text x="420" y="63" text-anchor="middle" class="x1t">테스트</text>
  <rect x="490" y="30" width="100" height="56" rx="10" class="x1b"/><text x="540" y="63" text-anchor="middle" class="x1t">문서화</text>
  <rect x="610" y="30" width="100" height="56" rx="10" class="x1b"/><text x="660" y="63" text-anchor="middle" class="x1t">릴리스·유지</text>
  <line x1="110" y1="58" x2="127" y2="58" stroke="#c99400" stroke-width="1.5" marker-end="url(#x1a)"/>
  <line x1="230" y1="58" x2="247" y2="58" stroke="#c99400" stroke-width="1.5" marker-end="url(#x1a)"/>
  <line x1="350" y1="58" x2="367" y2="58" stroke="#c99400" stroke-width="1.5" marker-end="url(#x1a)"/>
  <line x1="470" y1="58" x2="487" y2="58" stroke="#c99400" stroke-width="1.5" marker-end="url(#x1a)"/>
  <line x1="590" y1="58" x2="607" y2="58" stroke="#c99400" stroke-width="1.5" marker-end="url(#x1a)"/>
  <text x="360" y="125" text-anchor="middle" class="x1s">"로드맵이 밀려요" → 계획·리뷰·테스트·릴리스 조율 중 어디?</text>
  <text x="360" y="148" text-anchor="middle" class="x1s">"테스트가 오래 걸려요" → 수동 검증? 늦은 결함 발견? 불명확한 합격 기준?</text>
  <text x="360" y="171" text-anchor="middle" class="x1s">"기술 부채" → 시스템 복잡도? 깨지기 쉬운 변경 절차? 문서 부족?</text>
</svg>
</div>

이 단계에서 고객의 툴체인을 진단하거나 기술 설계를 할 필요는 없어요. **어디서 일이 느려지고, 그게 결과에 어떻게 영향 주는지**만 알면 돼요. 마지막으로 한 번 더 판단합니다: 이 마찰을 **Codex가 수행할 수 있는 구체적·범위가 정해진 개발 과제**로 바꿀 수 있나?

### 증상 → 결과 → 비즈니스 영향 → 다음 질문

<ol class="oa-flow">
  <li><strong>고객 증상</strong>"테스트가 오래 걸려요"</li>
  <li><strong>개발상 결과</strong>릴리스 확신이 떨어지고 다음 단계가 늦어짐</li>
  <li><strong>비즈니스 영향</strong>릴리스 날짜·고객 약속·품질 목표·운영 위험에 영향?</li>
  <li><strong>다음 질문</strong>"테스트가 늦어지면 릴리스 일정이나 로드맵 약속에 무슨 일이 생기나요?"</li>
</ol>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 지역적 불편</span>
    <ul>
      <li>"테스트가 오래 걸려요."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 비즈니스와 연결된 신호</span>
    <ul>
      <li>"테스트 지연 때문에 세 개 제품 팀의 릴리스 날짜가 계속 밀려요."</li>
    </ul>
  </div>
</div>

진행 전 고객이 답할 수 있어야 하는 3가지: **① 어떤 워크플로가 마찰을 만드나? ② 어떤 개발 결과가 영향받나? ③ 왜 지금 비즈니스에 중요한가?**

### 셀러 체크리스트: 라벨이 아니라 워크플로를 자격 판단

| 설명할 수 있어야 할 것 | 예시 |
|---|---|
| 어떤 워크플로가 느려지나 / SDLC 어디인가 | 릴리스 전 회귀 테스트 업데이트 |
| 반복되나 | 매 릴리스마다, 3개 팀 |
| 어떤 결과가 영향받나 | 고객 대상 업데이트 일정 |
| 평가할 구체적 개발 과제 | "변경된 API에 맞춰 테스트 갱신" |
| 필요한 맥락·도구 | 저장소, 티켓, 기존 테스트, CI |
| 대상 산출물·환경 | 특정 서비스의 테스트 스위트 |
| 검증 방법 | 통과해야 할 체크 + 사람 리뷰어 |
| 아직 빠진 증거 | 담당자 확정 여부 등 |

## 2. Codex Opportunity Fit Lens — 강한 신호 알아보기

<div class="oa-eli">
  <span class="oa-eli__head">🏥 쉽게 말하면</span>
  <p>건강검진에서 키·체중·혈압·혈액·시력을 따로 재듯, 기회도 <b>다섯 가지 항목</b>을 따로 점검해요. 하나가 좋다고 건강한 게 아니고, 모든 항목이 첫 검진부터 완벽할 필요도 없어요. 대신 <b>어디가 약한지</b>를 알면 다음에 뭘 검사할지 정해지죠.</p>
</div>

Lens는 제품 피치도, 기술 설계 도구도, 배포 체크리스트도 아닌 **셀러의 자격 판단 체크포인트**예요. 피해야 할 세 가지 실수:

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">👀</span><span class="oa-card__title">관심을 증거로 착각</span>"AI 코딩 도구에 관심 있어요"는 대화의 문을 열 뿐</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">📢</span><span class="oa-card__title">워크플로 전에 피칭</span>바로 기능 설명 → 흔한 도구 비교 대화로 전락</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🕳️</span><span class="oa-card__title">빈 증거로 진행</span>고통·영향·담당·준비도가 없으면 약한 파이프라인</div>
</div>

### 다섯 가지 점검 (+ 과제 실행 가능성)

| 점검 | 핵심 질문 | 찾아야 할 증거 |
|---|---|---|
| **① 반복되는 고통** | 이 문제가 반복되나? | 여러 팀·저장소·제품·단계에 걸친 반복 패턴 |
| **② 비즈니스 영향** | 느려지면 어떤 결과가 영향받나? | 로드맵·릴리스 예측성·품질·현대화·비용·운영 위험 |
| **③ 이해관계자 소유** | 누가 개선을 책임지나? | 엔지니어링·플랫폼·개발 생산성·기술 리더의 적극 참여 |
| **④ 준비도** | 더 나은 방식을 현실적으로 평가할 수 있나? | 현재 워크플로 설명, 맥락·도구 제공, 평가 과제·대상·검증 방법 정의 |
| **⑤ 반복 워크플로** | 요청 뒤의 구체적 워크플로는? | 리뷰·테스트·디버깅·온보딩·문서화·현대화·유지보수 |
| **+ 과제 실행 가능성** | 범위가 정해진 과제로 바꿀 수 있나? | 접근 가능한 맥락·도구 + 정해진 대상 + 명확한 검증 기준 |

### Codex 적합 범위 — 무엇이 들어가고 무엇이 안 들어가나

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 너무 넓음</span>
    <ul>
      <li>"엔지니어링이 더 빨라져야 해요."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 쓸 만한 기회 문장</span>
    <ul>
      <li>"늦은 검증 이슈가 세 제품 팀에서 재작업을 만들어 릴리스가 밀리고, 엔지니어링 리더십은 릴리스 예측성을 높이고 싶어 해요."</li>
      <li>→ 좋은 discovery 증거지만 <b>아직 Codex 과제는 아님</b>. 과제·맥락/도구·대상·검증 기준을 정해야 해요.</li>
    </ul>
  </div>
</div>

셀러 수준의 경계: 반복되는 개발 문제와 **검증 가능한 평가 과제를 정의할 준비도**를 확인하는 데까지. 구현 설계, 거버넌스 설정, 보안 아키텍처, 배포 계획은 하지 않아요. 보안·접근·검토 우려는 **준비도 신호**로 기록하고, 자세한 신뢰·배포 대응은 이후 코스(레슨 4)에서 다뤄요.

### 신호 압박 테스트 질문

| 테스트 대상 | 질문 |
|---|---|
| 반복되는 고통 | "얼마나 자주, 어떤 팀·시스템에서 일어나나요?" |
| 비즈니스 영향 | "이게 느려지면 개발·릴리스·고객 약속에 무슨 일이 생기나요?" |
| 담당자 | "이 워크플로 개선은 누가 책임지나요?" |
| 준비도 | "더 나은 방식을 평가하려면 무엇이 갖춰져야 하나요?" |
| 반복 워크플로 | "리뷰·테스트·온보딩·현대화·문서화·릴리스 조율 중 무엇부터 볼까요?" |
| 과제 실행 가능성 | "무슨 과제부터, 어떤 맥락·도구로, 무엇을 대상으로, 어떻게 검증할까요?" |
| 솔루션 방향 | "개발 워크플로 개선인가요, 직원 생산성 전반인가요, 아니면 AI 제품·시스템인가요?" |

한 번에 다 묻지 말고 **가장 큰 증거 공백**을 채우는 질문 몇 개만 고르세요.

### 신호 강도 3단계

<div class="oa-bars">
  <div class="oa-bar"><span>강한 신호</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:90%"></span><span class="oa-bar__val">여러 팀 영향 + 담당자 + 정의된 과제·검증</span></span></div>
  <div class="oa-bar"><span>중간 신호</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:55%"></span><span class="oa-bar__val">영향·담당 일부 있음, 평가할 워크플로 불명확</span></span></div>
  <div class="oa-bar"><span>약한 신호</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:15%"></span><span class="oa-bar__val">AI 호기심뿐, 워크플로·담당·영향 없음</span></span></div>
  <p class="oa-bars__caption">개념적 표현 (실제 수치 아님)</p>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">💬 좋은 셀러 응답 예시</span>
  <p>고객: "AI를 엔지니어링에 써 보려 해요. 검증 단계에서 문제를 늦게 발견해 릴리스 날짜가 흔들려요. 플랫폼 팀이 이번 분기 개발 생산성을 보고 있지만 평가할 워크플로는 아직 안 정했어요."<br>셀러: "늦은 검증이 릴리스 예측성에 영향을 준다면 의미 있는 문제네요. 적합성을 보려면 <b>구체적 과제 하나</b>로 좁히고 싶어요. 어떤 검증·리뷰 과제를 먼저 평가하면 좋을지, 어떤 맥락과 도구를 쓸 수 있는지, 결과는 팀이 어떻게 검증할지 알려 주시겠어요?"<br>→ 비즈니스 신호 인정 + 바로 피칭하지 않음 + 넓은 문제를 <b>검증 가능한 과제</b>로 이동.</p>
</div>

## 3. 워크플로 중심 discovery

<div class="oa-eli">
  <span class="oa-eli__head">🔍 쉽게 말하면</span>
  <p>자전거가 "삐걱거려요"라고 하면 정비사는 "어제 마지막으로 탔을 때 어떻게 됐어요?"라고 물어요. 지난번 상황을 따라가다 보면 체인인지, 브레이크인지, 페달인지 드러나죠. "<b>지난번에 이 일이 있었을 때 과정을 따라가 볼 수 있을까요?</b>"가 바로 그 질문이에요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 기능 중심 질문</span>
    <ul>
      <li>"AI 코딩 도구에 관심 있으세요?"</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 증거를 쌓는 질문</span>
    <ul>
      <li>"어떤 워크플로가 느려지고, 왜 중요하며, 누가 개선을 책임지나요?"</li>
    </ul>
  </div>
</div>

### 6단계 discovery 경로

<ol class="oa-flow">
  <li><strong>① 압박에서 시작</strong>"지금 가장 압박받는 개발 결과는?"</li>
  <li><strong>② 워크플로 위치</strong>"워크플로 어디서 가장 자주 느려지나요?"</li>
  <li><strong>③ 반복 패턴</strong>"얼마나 자주, 어떤 팀·시스템?"</li>
  <li><strong>④ 영향 테스트</strong>"느려지면 릴리스·고객 약속은?"</li>
  <li><strong>⑤ 담당·준비도</strong>"누가 책임지고, 평가하려면 무엇이 필요?"</li>
  <li><strong>⑥ 과제 실행 가능성</strong>"무슨 과제부터, 결과는 어떻게 검증?"</li>
</ol>

"테스트가 오래 걸려요"의 실제 원인은 불명확한 요구사항, 불안정한 테스트 환경, 수작업 테스트 작성, 늦은 결함 발견, 리뷰 지연, 릴리스 인계 중 무엇이든 될 수 있어요. 해결하려 하지 말고 **워크플로를 지도로 그리세요** — 무슨 일이었는지, 과제로 묶을 수 있는지, 어디서 지연됐는지, 누가 관련됐는지, 어떤 맥락·도구가 필요했는지, 결과는 어떻게 확인·리뷰됐는지.

### 이해관계자별로 드러나는 것

| 이해관계자 | 주로 드러내는 것 | 자격 판단에 왜 중요? |
|---|---|---|
| **엔지니어링 리더십** | 개발 압박·역량 제약·품질·현대화 우선순위 | 문제가 행동할 만큼 중요한지 |
| **플랫폼·개발 생산성** | 워크플로 표준화·도구 전략·평가 소유권 | 개선 담당자가 있는지 |
| **제품·딜리버리 리더십** | 로드맵 영향·릴리스 일정·고객 약속 | 개발 마찰을 비즈니스 결과와 연결 |
| **개발자·기여자** | 실제 막히는 곳, 빠진 맥락, 반복 과제 | 현실에 근거한 discovery |
| **보안·거버넌스** | 리뷰 기대·승인·접근·감독 요구 | **준비도 신호** + 전문가 후속 필요 |

보안 질문이 깊어지면 첫 대화에서 다 해결하려 하지 말고, 우려를 기록해 **적절한 기술·신뢰 담당 후속으로 연결**하세요.

### Discovery evidence snapshot

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">✅</span><span class="oa-card__title">확인된 증거</span>고객이 실제로 말한 것. 예: 릴리스 예측성 압박, 늦은 검증 이슈, 제품·개발·QA 간 수동 조율</div>
  <div class="oa-card"><span class="oa-card__icon">💭</span><span class="oa-card__title">작업 가설</span>원인 추정. 예: 테스트·검증·릴리스 조율·인계 중 어딘가</div>
  <div class="oa-card"><span class="oa-card__icon">📌</span><span class="oa-card__title">자격 판단 의미</span>의미 있는 discovery 줄기지만 <b>아직 Codex 과제는 아님</b> — 조율 문제를 실행 가능한 개발 작업으로 바꿔야 함</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🕳️</span><span class="oa-card__title">증거 공백</span>어떤 구체 과제? 맥락·도구 접근? 대상? 담당? 검증 방법?</div>
</div>

→ 다음 질문: **"릴리스 과정에서 어떤 구체적 과제를 먼저 평가하면 가장 유용하고, 결과는 무엇으로 검증하시겠어요?"**

## 4. 준비도 평가와 진행 결정

<div class="oa-eli">
  <span class="oa-eli__head">🚦 쉽게 말하면</span>
  <p>신호등처럼 생각하세요. <b>초록(Progress)</b>은 건너도 됨, <b>노랑(Continue discovery)</b>은 조금 더 살펴봄, <b>빨강(Pause)</b>은 지금은 멈춤, 그리고 <b>길이 아예 다른 곳(Disqualify)</b>이면 다른 길로 안내해요. 빨강이나 다른 길이 "실패"가 아니라 <b>안전과 집중</b>을 위한 판단이에요.</p>
</div>

### 셀러 수준 준비도 — 3가지 과제 준비 질문

| 질문 | 확인할 것 |
|---|---|
| **과제** — Codex가 무엇을 해야 하나? | 워크플로·코드베이스 맥락·저장소·티켓·문서·엔지니어링 지식을 설명할 수 있나 |
| **맥락과 도구** — 과제에 무엇이 필요한가? | 저장소·티켓·문서·테스트·시스템·도구를 제공할 수 있나 |
| **대상과 검증** — 무엇이 만들어지고, 어떻게 확인하나? | 대상 산출물·환경, 통과해야 할 체크, 사람 리뷰어, 전문가 후속이 필요한 우려 |

Progress가 "구현 계획 준비 완료"를 뜻하지는 않아요. **기회가 실제적이고 관련 있으며 더 깊은 Codex 대화를 할 가치가 있다**는 셀러의 확신을 뜻해요.

### 4가지 진행 결정

<div class="oa-svg">
<svg viewBox="0 0 720 250" role="img" aria-label="진행 결정 4가지">
  <defs>
    <marker id="x1b" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.x1g{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.x1y{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.x1o{fill:#fff8ee;stroke:#f2dcb8;stroke-width:1.5}.x1d{fill:#fff4f2;stroke:#f2c9c2;stroke-width:1.5}.x1h{font-size:13px;fill:#4a4a4a;font-weight:bold}.x1m{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="235" y="10" width="250" height="44" rx="10" class="x1y"/><text x="360" y="37" text-anchor="middle" class="x1h">Discovery 증거</text>
  <line x1="300" y1="54" x2="95" y2="96" stroke="#c99400" stroke-width="1.5" marker-end="url(#x1b)"/>
  <line x1="340" y1="54" x2="275" y2="96" stroke="#c99400" stroke-width="1.5" marker-end="url(#x1b)"/>
  <line x1="380" y1="54" x2="445" y2="96" stroke="#c99400" stroke-width="1.5" marker-end="url(#x1b)"/>
  <line x1="420" y1="54" x2="625" y2="96" stroke="#c99400" stroke-width="1.5" marker-end="url(#x1b)"/>
  <rect x="10" y="100" width="170" height="130" rx="10" class="x1g"/><text x="95" y="126" text-anchor="middle" class="x1h">Progress</text><text x="95" y="150" text-anchor="middle" class="x1m">반복 고통 + 영향</text><text x="95" y="168" text-anchor="middle" class="x1m">+ 담당자 참여</text><text x="95" y="186" text-anchor="middle" class="x1m">+ 과제·맥락·대상·검증</text><text x="95" y="210" text-anchor="middle" class="x1m">→ 깊은 Codex 대화</text>
  <rect x="190" y="100" width="170" height="130" rx="10" class="x1y"/><text x="275" y="126" text-anchor="middle" class="x1h">Continue discovery</text><text x="275" y="150" text-anchor="middle" class="x1m">고통은 진짜 같음</text><text x="275" y="168" text-anchor="middle" class="x1m">영향·담당·과제·준비도</text><text x="275" y="186" text-anchor="middle" class="x1m">중 일부가 불명확</text><text x="275" y="210" text-anchor="middle" class="x1m">→ 빈 증거 채우기</text>
  <rect x="370" y="100" width="170" height="130" rx="10" class="x1o"/><text x="455" y="126" text-anchor="middle" class="x1h">Pause</text><text x="455" y="150" text-anchor="middle" class="x1m">관심은 있음</text><text x="455" y="168" text-anchor="middle" class="x1m">긴급성·운영 필요·</text><text x="455" y="186" text-anchor="middle" class="x1m">변화 준비도 약함</text><text x="455" y="210" text-anchor="middle" class="x1m">→ 관계 유지</text>
  <rect x="550" y="100" width="160" height="130" rx="10" class="x1d"/><text x="630" y="126" text-anchor="middle" class="x1h">Disqualify</text><text x="630" y="150" text-anchor="middle" class="x1m">개발 개선과 무관</text><text x="630" y="168" text-anchor="middle" class="x1m">또는 검증 가능한</text><text x="630" y="186" text-anchor="middle" class="x1m">과제가 없음</text><text x="630" y="210" text-anchor="middle" class="x1m">→ 다른 경로 안내</text>
</svg>
</div>

**다른 OpenAI 방향으로 안내하기 (강의 기준)**

| 주된 필요 | 고려할 방향 |
|---|---|
| 개발 워크플로 개선 + 검증 가능한 개발 과제 | **Codex** |
| 직원 생산성·지식 업무 전반 | **ChatGPT** |
| 제품·앱·시스템·고객 경험에 지능을 내장 | **API** |
| 아직 문제 자체가 불명확 | 어떤 경로도 강하게 밀지 말고 discovery 계속 |

> 고객이 궁금해해서, 전략 계정이라서, 데모를 요청해서 **진행하지 마세요.** 진행은 증거로 합니다.

### 강한 진행 추천문의 5요소

워크플로 고통은? · 왜 중요한가? · 누가 소유하나? · 왜 지금인가? · 다음은 무엇인가?

```text
고객은 고객 대상 제품의 릴리스 예측성에 영향을 주는 반복적인 늦은 검증 이슈를 겪고 있다.
제품·엔지니어링 리더십 모두 영향을 인정한다. 플랫폼 팀이 범위가 정해진 검증 과제를 정의했고,
관련 저장소·테스트 맥락을 제공할 수 있으며, 합의된 체크와 사람 리뷰로 결과를 검증한다.
다음 단계: 적절한 기술 이해관계자와 더 깊은 Codex 논의.
```

<div class="oa-eli">
  <span class="oa-eli__head">💳 실제 사례 (디지털 결제 회사)</span>
  <p>고객 대상 결제 플랫폼에서 늦은 결함·수동 검증·릴리스 조율 문제가 반복되고, 몇몇 개선 사항이 검증 시간 때문에 밀렸어요. 고통은 진짜지만 <b>어느 워크플로가 가장 큰 지연인지, 누가 책임지는지, 리더십이 일하는 방식을 바꿀 의지가 있는지, AI 작업을 어떻게 리뷰할지, 어떤 맥락·도구·대상·체크가 있는지</b> 아직 몰라요.<br>→ 최선의 결정: <b>Continue discovery</b>. 넓은 압박만으로 진행하지 않아요.</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 고객: "테스트와 리뷰 지연이 여러 팀에서 나타나 릴리스 날짜가 계속 밀려요." 의미 있는 Codex 적합 discovery를 가장 잘 반영한 해석은?</summary>
  <p>① 추가 discovery 전에 일반 AI 코딩 데모가 필요하다 ② 개발 문제를 말했으니 가격 논의 준비가 됐다 ③ 테스트·리뷰는 소프트웨어 개발이 아니므로 탈락 ④ 비즈니스 영향이 있는 반복적 워크플로 마찰이 있을 수 있다</p>
  <p><b>정답: ④</b> — 반복성과 영향을 인정하되, 아직 확정이 아니라 "있을 수 있다"로 discovery를 이어 가요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 고객 상황을 신호 강도(약함·중간·강함)에 짝지으세요.</summary>
  <p>(a) 릴리스 예측성 압박, 플랫폼 팀 참여, 평가할 워크플로는 불명확 (b) 리더십이 AI에 호기심, 워크플로·담당·영향 없음 (c) 테스트 병목이 여러 팀 릴리스에 영향, 엔지니어링 담당자 참여, 저장소 맥락과 합의된 검증 체크가 있는 범위 정해진 테스트 갱신 과제 정의</p>
  <p><b>정답:</b> (a) <b>중간</b> · (b) <b>약함</b> · (c) <b>강함</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 고객: "릴리스 날짜 예측이 어려워요. 테스트·리뷰 지연, 불명확한 담당 얘기가 나오는데 무엇이 가장 중요한지는 모르겠어요." 최선의 다음 질문은?</summary>
  <p>① "보안팀이 프로덕션용 AI 워크플로를 승인할 수 있나요?" ② "어떤 워크플로가 릴리스 날짜를 가장 자주 밀리게 하나요?" ③ "개발자들은 어떤 Codex 경험을 평가하고 싶어 하나요?" ④ "AI 코딩 도구 비교 데모가 도움이 될까요?"</p>
  <p><b>정답: ②</b> — 제품·보안·데모로 건너뛰지 않고 워크플로 위치부터 좁혀요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 고객 상황을 진행 결정(Progress · Continue discovery · Pause · Disqualify)에 짝지으세요.</summary>
  <p>(a) 현대화 지연은 진짜 같지만 담당·과제·맥락·대상·검증·영향이 불명확 (b) 문제가 소프트웨어 개발과 무관하고 Codex 적합 워크플로 없음 (c) 리뷰 지연이 3개 팀에서 반복, 릴리스 영향, 담당자 참여, 저장소 맥락과 검증 체크가 있는 리뷰 과제 정의 (d) 리더십이 AI 도구를 배우고 싶어 하지만 개발 우선순위·긴급성 없음</p>
  <p><b>정답:</b> (a) <b>Continue discovery</b> · (b) <b>Disqualify</b> · (c) <b>Progress</b> · (d) <b>Pause</b></p>
</details>

## 정리

- 개발 압박은 **처리량·품질·지식·조율·현대화** 5가지 패턴으로 알아듣고, **SDLC 어디서** 막히는지 찾는다.
- **Fit Lens 5가지 점검**(반복 고통·비즈니스 영향·담당자·준비도·반복 워크플로)에 **과제 실행 가능성**(맥락·도구 / 대상 / 검증)을 더해 신호 강도를 본다.
- discovery는 **압박 → 워크플로 → 반복 → 영향 → 담당·준비도 → 과제** 순으로, 결과는 **증거 스냅샷**으로 남긴다.
- 결정은 **Progress · Continue discovery · Pause · Disqualify** 중 증거에 따라. 개발 일이 아니면 ChatGPT나 API로 정직하게 안내한다.
- 보안·거버넌스 우려는 이 단계에서 해결하지 않고 **준비도 신호로 기록하고 전문가 후속으로 연결**한다.
