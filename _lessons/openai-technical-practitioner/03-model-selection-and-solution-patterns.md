---
title: "Model Selection and Solution Patterns"
title_ko: "모델 선택과 솔루션 패턴 — 모델 이름보다 작업 모양(task shape)부터"
course: openai-technical-practitioner
lesson: 3
---

## 학습 목표

- 모델 선택이 왜 **task shape(작업 모양)과 워크플로 맥락**에서 시작해야 하는지 설명하기
- 흔한 **모델 클래스**의 강점과 트레이드오프 구분하기
- API 모델 선택에 영향을 주는 **결정 요인**(추론 깊이·지연·비용·규모·컨텍스트·구조화 출력·도구·리스크) 짚기
- 사용자/제품 surface와 역량/아키텍처, 배포/운영 결정을 분리하기
- 고객 요구를 반복 가능한 **솔루션 패턴**에 매핑하기
- 추천을 검증할 **가벼운 eval 증거**를 정하고, route·validate·escalate 판단하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>Task shape</strong>AI가 무엇을, 어떤 출력으로, 어떤 위험 아래서?</li>
  <li><strong>모델 클래스·결정 요인</strong>그럴듯한 후보 + 대안 + 트레이드오프</li>
  <li><strong>솔루션 경로·패턴</strong>어디서 일하고 어떤 패턴으로?</li>
  <li><strong>Eval 증거</strong>성공 기준 2~3개 + 작은 seed set</li>
  <li><strong>4단 결정 구조</strong>Route · Validate · Escalate</li>
</ol>

고객이 "어떤 모델 써야 해요?"라고 물으면, 좋은 답은 **모델 이름이 아니라 워크플로**에서 시작해요.

## 1. 모델 이름이 아니라 task shape부터

<div class="oa-eli">
  <span class="oa-eli__head">🧰 쉽게 말하면</span>
  <p>"집수리 좀 해 주세요"라는 말만 듣고 공구를 고를 순 없어요. 못을 박는지, 나사를 조이는지, 벽을 칠하는지에 따라 망치·드라이버·붓이 달라지죠. "계약 검토", "클레임 검토" 같은 말은 <b>집수리</b>처럼 넓은 이름표일 뿐이고, 그 안의 <b>실제 작업</b>을 쪼개 봐야 맞는 모델이 보여요.</p>
</div>

**Task shape**는 워크플로 안에서 AI가 실제로 하는 일이에요. 좋은 task shape 문장에는 **작업 · 입력/컨텍스트 · 기대 출력 · 출력을 쓰는 사람/시스템 · 검토/성공 요건**이 들어가요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 워크플로 이름표</span>
    <ul><li>"지원 티켓 분류가 필요해요."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ Task shape</span>
    <ul><li>"AI가 들어오는 티켓을 읽고 <b>이슈 유형·긴급도로 분류</b>, 필요하면 <b>승인된 가이드를 검색</b>하고, 모호하거나 민감하거나 긴급한 건은 <b>짧은 근거와 함께 사람 큐로</b> 보낸다."</li></ul>
  </div>
</div>

### 흔한 작업 패턴

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📝</span><span class="oa-card__title">요약·종합</span>긴 내용을 핵심으로</div>
  <div class="oa-card"><span class="oa-card__icon">🏷️</span><span class="oa-card__title">분류·라우팅</span>유형 판정 후 큐로</div>
  <div class="oa-card"><span class="oa-card__icon">🧾</span><span class="oa-card__title">추출·구조화</span>문서에서 필드 뽑기</div>
  <div class="oa-card"><span class="oa-card__icon">✏️</span><span class="oa-card__title">초안·재작성</span>글 쓰고 다듬기</div>
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">추론·분석</span>여러 단계 판단</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">검색 기반 답변</span>승인된 출처로 답하기</div>
  <div class="oa-card"><span class="oa-card__icon">🔧</span><span class="oa-card__title">도구 사용 워크플로</span>API 호출·시스템 확인</div>
  <div class="oa-card"><span class="oa-card__icon">🖼️</span><span class="oa-card__title">멀티모달 이해</span>이미지·문서·스크린샷</div>
  <div class="oa-card"><span class="oa-card__icon">🎙️</span><span class="oa-card__title">음성·실시간</span>대화형 음성</div>
  <div class="oa-card"><span class="oa-card__icon">💻</span><span class="oa-card__title">코드·개발</span>개발자 워크플로</div>
  <div class="oa-card"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">모더레이션·안전 필터</span>위험 콘텐츠 걸러내기</div>
</div>

"구매 요청 접수" 하나에도 **분류 → 업체 정보 추출 → 정책 질문 답변 → 시스템 확인 → 예외는 사람 검토**처럼 여러 패턴이 섞여 있을 수 있어요. 섞여 있으면 **먼저 분리**하고 나서 추천해요.

### 출력 요건과 리스크가 판단을 바꾼다

| 질문 | 왜 중요한가 |
|---|---|
| 다음 사람/시스템이 이 출력으로 **무엇을 하나?** | 사람이 참고만 하면 자연어로 충분, 워크플로를 움직이면 구조·검증·검토 필요 |
| 어떤 **형식**이어야 하나? | 요약문 / 표 / 라벨 / 인용 답변 / 도구 호출 / 파일 / 코드 변경 / 안전한 거절 |
| 얼마나 **반복**되나? | 반복될수록 구현·테스트·자동화 투자 정당화 |

**범위가 정해진 결정**(조건이 참일 확률, 정해진 선택지 중 하나, 루브릭 점수)은 필드 추출·서면 근거·도구 호출 요청과는 다른 출력이에요.

<div class="oa-eli">
  <span class="oa-eli__head">⚠️ 리스크가 높으면 "더 센 모델"이 답일까?</span>
  <p>아니에요. 고객 대상, 규제·민감 데이터, 대량 처리, 법률·금융·의료·보안·채용 결정, 시스템 쓰기, 적은 사람 검토 — 이런 경우엔 <b>더 강한 검증 · 명확한 사람 검토 · 안전한 에스컬레이션 · 책임 소재</b>가 필요해요. 모델만 키운다고 해결되지 않아요.</p>
</div>

## 2. 모델 클래스와 결정 요인

**모델 클래스**는 오래 유지되는 능력 범주예요. 특정 모델 이름을 외우지 않고도 적합성을 이야기할 수 있게 해 주죠. 단, **출발 가설**이지 최종 답이 아니에요.

| 범주 | 워크플로에서 쓰임 |
|---|---|
| **추론 지향 모델** | 다단계 분석, 계획, 모호함 처리, 정책 해석, 판단이 많은 작업 |
| **범용 모델** | 초안·요약·재작성·분류 등 폭넓은 언어 작업 |
| **효율형 모델** | 속도·비용·규모가 중요하고 작업이 안정적·단순·대량일 때 |
| **멀티모달 모델** | 이미지·문서·스크린샷·오디오 등 혼합 입출력 |
| **오디오/실시간 모델** | 음성·전사·스트리밍·저지연 대화 |
| **임베딩 모델** | 의미 검색, 검색, 유사도 비교, 군집화 |
| *역량*: 모더레이션·안전 필터 | 위험·민감·금지 콘텐츠 식별 |
| *역량*: 도구 사용 | 함수·API 호출, 계산, 시스템 간 행동 조율 |
| *모델/배포 옵션*: open 모델 | 호스팅·커스터마이즈·인프라·정책·비용 구조 제약 평가 |
| *역량*: 검색·랭킹 | 검색은 후보를 가져오고, (re)ranking은 관련도 순으로 정렬 |

> 모델 클래스는 **모델 범주에만** 써요. 검색·도구·에이전트 조율(역량/아키텍처)과 호스팅·권한·로깅(배포/운영)은 **별개 결정**이에요. 서로 영향을 주지만 한 줄로 나열하면 안 돼요.

### 강의에 나오는 후보 모델 (강의 기준)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔭</span><span class="oa-card__title">GPT-6 Astra</span>불완전한 정보 분석, 상충 요건 저울질, 여러 단계 계획 등 <b>까다로운 다단계 추론</b>이 필요할 때 테스트할 후보</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">☀️</span><span class="oa-card__title">GPT-6.1 Sol</span>품질과 비용이 <b>둘 다</b> 중요한 글쓰기·코딩·판단 작업 후보. 복잡한 업무에서 Astra에 가까운 성능을 더 낮은 토큰 가격으로 낸다고 소개됨</div>
  <div class="oa-card"><span class="oa-card__icon">🌙</span><span class="oa-card__title">GPT-6 Luna</span><b>집중적·대량</b> 작업 후보. <b>Decisions API</b>(public beta)는 Luna로 조건 판정·선택·점수 같은 범위 결정을 처리</div>
</div>

Surface(Chat·Work·Codex·API)마다 가용성과 설정이 다르니, 제품·모델 버전·지원 설정·가격·접근 권한을 **현재 기준으로 확인**하고 테스트해요. "패밀리 이름"이 곧 구현 선택은 아니에요.

### 출력에 따라 엔드포인트가 정해진다

| 필요한 출력 | 쓰는 방식 (강의 기준) |
|---|---|
| 생성된 글·설명 | Responses |
| 정해진 필드의 커스텀 객체 (예: topic, urgency, rationale) | Structured Outputs + JSON Schema |
| 도구에 대한 요청 | Function calling (반환 값이 맞는지는 팀이 확인) |
| 조건 판정(predicate)·고정 선택·루브릭 점수 | Decisions API + GPT-6 Luna |

"Luna를 고른다"고 엔드포인트가 정해지는 게 아니라, **필요한 출력**이 엔드포인트를 정해요.

### 추론 깊이 — "더 센 모델이 할 수 있나?"가 아니라

<div class="oa-eli">
  <span class="oa-eli__head">🧮 쉽게 말하면</span>
  <p>"3+4"를 풀자고 수학 교수님을 부를 필요는 없죠. 하지만 "조건이 서로 충돌하는 문제"라면 교수님이 필요해요. 질문은 <b>"이 워크플로가 깊은 추론을 정당화하나?"</b>예요.</p>
</div>

비교 전에 **품질 기준선(quality threshold)**을 먼저 정해요: 출력이 반드시 맞춰야 할 것, 포함해야 할 근거, 사람 검토가 필요한 상황. 그리고 모든 후보를 같은 기준으로 평가해요. 모델이 필요한 입력 유형·도구·설정(예: 쓰려는 reasoning 설정)을 지원하는지도 확인해요. 추론을 더 쓰면 품질·지연·비용이 함께 변해요.

### 지연·비용·규모 — "합격한 결과당 비용"

<div class="oa-eli">
  <span class="oa-eli__head">🛒 쉽게 말하면</span>
  <p>싼 사과 10개 중 3개가 썩었다면, 진짜 가격은 "멀쩡한 사과 7개에 낸 돈"이에요. AI도 <b>쓸 수 있는 결과 하나당</b> 얼마가 들었는지를 봐야 해요.</p>
</div>

**Cost per accepted result** = (모델·추론 사용 + 사람 수정 + 재작업 등 총비용) ÷ (품질 기준을 통과한 결과 수)

강의의 가상 예시: 티켓 100건, 기준은 95건 이상 합격.

<div class="oa-bars">
  <div class="oa-bar"><span>Model A 합격 수</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:90%"></span><span class="oa-bar__val">90/100 · 총 $18 → $0.20/건 · 기준 미달</span></span></div>
  <div class="oa-bar"><span>Model B 합격 수</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:100%"></span><span class="oa-bar__val">100/100 · 총 $24 → $0.24/건 · 기준 충족</span></span></div>
  <p class="oa-bars__caption">강의에 나오는 가상의(invented) 수치예요. A가 더 싸지만 품질 기준을 못 넘고, B도 응답 시간·사람 검토 요건 검증이 더 필요해요.</p>
</div>

| 요인 | 핵심 포인트 (강의 기준) |
|---|---|
| **Rate limit·사용 등급** | 시간당 요청·토큰 한도. Build·Launch·Grow 등급이 한도와 월 사용 허용량에 영향. 실제 한도를 예상 트래픽과 비교 |
| **지출 통제** | "얼마까지 쓰게 할지"는 별도 질문 |
| **처리 속도** | Standard · Fast · Ultrafast는 속도·가격이 다른 처리 선택(구독 요금제 아님). Ultrafast는 Responses API에서 Astra·Sol 지원, 더 비쌈 |
| **End-to-end 시간** | 대기 대부분이 도구 조회·사람 검토라면 모델을 빠르게 해도 효과 적음 |
| **Decisions 가격** | 입력에만 과금(캐시 읽기/쓰기·출력 토큰 과금 없음), 지역·긴 컨텍스트 할증은 가능. Luna를 쓰는 모든 요청이 아니라 Decisions 엔드포인트의 가격 |

### 두 가지 캐싱

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">⚡ Prompt caching</span>
    <ul>
      <li>매번 같은 앞부분(지시·정책 텍스트 = <b>prompt prefix</b>)의 처리 작업을 재사용</li>
      <li>재사용 입력은 cached-input 요율, 새 티켓·새 답변은 각자 요율</li>
      <li>캐시 생성 시 cache-write 요율은 일반 입력 요율을 <b>대체</b>(추가 요금 아님)</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🗃️ Application-result caching</span>
    <ul>
      <li>앱이 <b>저장해 둔 답</b>을 재사용해 새 생성을 피함</li>
      <li>새 티켓에 맞는지, 사실이 최신인지, 이 사용자가 받아도 되는지 확인</li>
      <li>저장·확인·갱신 비용도 합격 결과당 비용에 포함</li>
    </ul>
  </div>
</div>

### 컨텍스트·구조화 출력·도구

- 모델이 **프롬프트 밖 정보**(사내 정책, 최신 한도, 업체 상태, 사용자 권한)가 필요하면 **모델 선택만으로는 해결 안 돼요.** 검색·도구·연동 같은 컨텍스트 패턴이 모델 클래스만큼 중요해요. "더 좋은 모델"을 추천하기 전에 진짜 갭이 **컨텍스트 부족**인지 보세요.
- **구조화 출력**은 추출·분류·라우팅·양식 작성·시스템 업데이트처럼 예측 가능한 형식이 필요할 때.
- **도구 사용**은 가치를 키우지만 책임도 키워요. 다른 시스템에 영향을 주면 권한 경계·승인 규칙·에러 처리·관측성·eval·에스컬레이션이 추천에 포함돼야 해요.

### 워크플로 → 모델 클래스 후보 목록

<div class="oa-svg">
<svg viewBox="0 0 720 200" role="img" aria-label="task shape에서 결정 요인을 거쳐 후보 목록과 검증으로 가는 흐름">
  <defs>
    <marker id="ar3t" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.b3{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.g3{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.t3{font-size:12px;fill:#4a4a4a}.s3{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="30" width="160" height="120" rx="10" class="b3"/>
  <text x="90" y="55" text-anchor="middle" class="t3">① Task shape</text>
  <text x="90" y="80" text-anchor="middle" class="s3">무엇을 하나</text>
  <text x="90" y="98" text-anchor="middle" class="s3">어떤 출력</text>
  <text x="90" y="116" text-anchor="middle" class="s3">어떤 컨텍스트</text>
  <text x="90" y="134" text-anchor="middle" class="s3">리스크·검토</text>
  <line x1="172" y1="90" x2="196" y2="90" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar3t)"/>
  <rect x="200" y="30" width="170" height="120" rx="10" class="b3"/>
  <text x="285" y="55" text-anchor="middle" class="t3">② 결정 요인</text>
  <text x="285" y="80" text-anchor="middle" class="s3">추론 깊이 · 지연</text>
  <text x="285" y="98" text-anchor="middle" class="s3">비용 · 규모</text>
  <text x="285" y="116" text-anchor="middle" class="s3">컨텍스트 · 구조화 출력</text>
  <text x="285" y="134" text-anchor="middle" class="s3">도구 · 리스크</text>
  <line x1="372" y1="90" x2="396" y2="90" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar3t)"/>
  <rect x="400" y="30" width="150" height="120" rx="10" class="g3"/>
  <text x="475" y="55" text-anchor="middle" class="t3">③ 후보 목록</text>
  <text x="475" y="80" text-anchor="middle" class="s3">유력 클래스 1</text>
  <text x="475" y="98" text-anchor="middle" class="s3">비교할 대안 1</text>
  <text x="475" y="116" text-anchor="middle" class="s3">핵심 트레이드오프</text>
  <line x1="552" y1="90" x2="576" y2="90" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar3t)"/>
  <rect x="580" y="30" width="130" height="120" rx="10" class="g3"/>
  <text x="645" y="55" text-anchor="middle" class="t3">④ 검증</text>
  <text x="645" y="80" text-anchor="middle" class="s3">같은 조건 비교</text>
  <text x="645" y="98" text-anchor="middle" class="s3">eval 증거</text>
  <text x="645" y="116" text-anchor="middle" class="s3">필요 시 escalate</text>
  <text x="360" y="185" text-anchor="middle" class="s3">목표는 최종 모델 선택이 아니라 "그럴듯한 후보 + 검증할 것 + 에스컬레이션 시점"</text>
</svg>
</div>

예 (강의 기준, 갱신 청구서 대사): 서명된 수정 계약이 요율·갱신일을 바꿨는데 고객 레코드와 청구 일정은 옛 조건. 할 일은 승인된 레코드를 대조해 **재무팀 검토용 초안**을 만드는 것(청구서 발행 아님). 기준: 날짜·금액 정확, 바뀐 조건마다 추적 가능한 근거, 레코드 충돌·근거 누락 시 에스컬레이션. 현재 방식과 Sol(또는 Astra)을 **같은 레코드·지시·출력 요건·허용 도구·품질 기준**으로 비교하고, 더 단순한 방식이 기준을 충족하면 그걸 유지해요. 모델 능력이 레코드 읽기 **권한**을 주진 않는다는 점도 기억!

## 3. 솔루션 경로와 패턴

### 세 가지 결정을 섞지 않기 (지난 레슨 복습)

| 결정 | 질문 | 선택지 |
|---|---|---|
| 사용자/제품 surface | 어디서 일하나? | ChatGPT Chat · ChatGPT Work · Codex · API로 만든 앱/프로세스 |
| 역량/아키텍처 | 무엇을 해야 하나? | 검색 · Structured Outputs · 도구 · 에이전트 조율/자동화 · 멀티모달/Realtime |
| 배포/운영 | 어떻게 돌고 통제되나? | 신원 · 권한 · 승인 · 사람 검토 · 보존 · 로깅 · 모델 호스팅 제약 |

"에이전트", "open 모델", "하이브리드"는 **surface 목록에 같이 놓을 대상이 아니에요.**

### API 모델 선택이 맞는 다음 단계일 때

커스텀 UX, 제품 연동, 백엔드 자동화, 구조화 출력, 도구 사용, 시스템 행동, 운영 규모 앱 동작이 필요할 때예요. 예: 손해사정사가 **클레임 앱 안에서** AI 도움을 받아 케이스를 준비.

- 새 응답 생성 연동은 **Responses API**부터 평가(Chat Completions보다 권장, 강의 기준). 이건 인터페이스 선택.
- **누가 agent 루프를 운영하나**는 별개: 앱 소유 runtime(선택적으로 Agents SDK) vs 관리형 runtime(public beta Agents API의 managed harness).
- **실행 환경**(도구·코드가 도는 곳)은 runtime과 관련 있지만 또 다른 선택이에요.

### API 중심 솔루션 패턴

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧾</span><span class="oa-card__title">구조화 추출</span>클레임 정보를 앱이 검사할 수 있는 필드로</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">검색 기반 답변</span>어떤 승인된 정책이 설명을 뒷받침하는지 보여 주기. 인용이 실제로 뒷받침하는지 테스트</div>
  <div class="oa-card"><span class="oa-card__icon">🏷️</span><span class="oa-card__title">분류·라우팅</span>Decisions 단계로 검토 큐 제안 가능. 서면 사유가 필요하면 별도 생성, 실제 라우팅 행동은 앱이 수행</div>
  <div class="oa-card"><span class="oa-card__icon">🧑‍⚖️</span><span class="oa-card__title">Human-in-the-loop</span>검토자가 승인·거절·전문가 이관의 <b>기준</b>을 알아야 함</div>
  <div class="oa-card"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">모더레이션</span>민감 콘텐츠 걸러내기</div>
  <div class="oa-card"><span class="oa-card__icon">➕</span><span class="oa-card__title">그 외</span>텍스트 생성, 요약·종합, 멀티모달, 음성/실시간, 도구 사용 워크플로</div>
</div>

### 패턴 조합은 "목록"이 아니라 "순서"

<ol class="oa-flow">
  <li><strong>분류</strong>요청 유형 식별</li>
  <li><strong>구조화 추출</strong>업체·구매 정보</li>
  <li><strong>검색 기반 답변</strong>정책 확인</li>
  <li><strong>도구 사용</strong>업체 상태 조회</li>
  <li><strong>사람 검토</strong>예외 처리</li>
</ol>

(구매 요청 접수 예) 각 단계마다 **AI가 무엇을 만들고, 어떤 컨텍스트·도구가 필요하고, 무엇이 잘못될 수 있고, 어떤 증거로 검증하고, 어디서 사람이 개입하나**를 물어요.

## 4. Eval 증거로 추천 검증하기

데모는 가능성을 보여 주고, **가벼운 eval 증거**는 실제 워크플로에서 될 것 같은지를 보여 줘요. "이 모델이면 될 거예요" 대신 **"이걸 테스트했고, 여기선 잘 됐고, 여기는 아직 불확실하고, 다음엔 이걸 검증해야 해요"**라고 말할 수 있게 해 줘요.

### 성공 기준은 2~3개만

| 워크플로 | 중요한 기준 |
|---|---|
| 구매 요청 접수 | 분류 정확도 · 형식 준수 · 에스컬레이션 동작 |
| 검색 기반 정책 답변 | 근거성(groundedness) · 완전성 · 인용 근거 |
| 음성 상호작용 | 지연 · 대화 주고받기 품질 |

기준이 너무 많으면 정밀해 보이지만 **진짜 결정을 가려요.**

### 작은 eval seed set

정상 케이스 + 어려운 케이스(모호함, 정보 누락, 민감 입력, 고위험 예외, 필수 형식, 검색·도구 요건). 갱신 청구서 예라면 일치하는 레코드, 옛 청구 일정과 충돌하는 수정 계약, 금액을 확정하면 안 되는 근거 누락, **읽기 도구가 안 될 때 솔직하게 멈추고 검토로 넘기는지**까지.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🔁 모델 비교 (like-for-like)</span>
    <ul>
      <li>같은 케이스 · 승인된 근거 · 프롬프트 · 출력 스키마 · 허용 도구</li>
      <li>같은 품질 기준으로 판정</li>
      <li>모델 버전·reasoning 설정·피할 수 없는 설정 차이 기록 → 재현 가능</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🔀 엔드포인트 비교</span>
    <ul>
      <li>예: Decisions 단계 vs 생성형 응답으로 같은 라우팅 질문</li>
      <li>범주·정답 의미는 같게, 요청·출력 형식은 엔드포인트별로</li>
      <li>합격 결과 + 요청→사용 가능 결과까지 시간 + 사람 수정 + 총 사용량 기록</li>
    </ul>
  </div>
</div>

작은 비교로 **약점은 드러낼 수 있지만, 보편적 모델 순위를 증명하진 않아요.**

### 결과를 보기 전에 "증거의 의미"부터 정하기

<div class="oa-eli">
  <span class="oa-eli__head">🚨 결정 임계값(decision threshold), 쉽게 말하면</span>
  <p>화재 경보기의 민감도를 생각해 봐요. 너무 둔하면 진짜 불을 놓치고(<b>false negative</b>), 너무 예민하면 토스트만 구워도 울려요(<b>false positive</b>). 긴급 티켓 판정도 같아요. 모델이 "긴급일 확률 0.7"이라고 해도, <b>컷오프</b>가 0.8이면 일반 큐로, 0.6이면 긴급 검토로 가요.</p>
</div>

<div class="oa-bars">
  <div class="oa-bar"><span>컷오프를 낮추면 · 놓친 긴급 건</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:25%"></span><span class="oa-bar__val">줄어듦 ↓</span></span></div>
  <div class="oa-bar"><span>컷오프를 낮추면 · 불필요한 긴급 검토</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:75%"></span><span class="oa-bar__val">늘어남 ↑</span></span></div>
  <p class="oa-bars__caption">개념적 표현이에요. 사람이 미리 라벨을 단 대표 티켓으로 두 효과를 함께 보고, 평균 정확도가 아니라 "도움 지연 vs 불필요한 검토"를 저울질해 컷오프를 정해요.</p>
</div>

필드 누락과 "긴급 티켓을 일반 큐로 보냄"은 **종류가 다른 실패**라 후속 조치도 달라요. 정보 누락·모델 거절·단계 실패 케이스도 꼭 넣어요. 그럴듯한 라우팅 결과라도 **티켓이 맞는 사람에게 도달하지 못하면** 소용없어요.

증거가 가리키는 다음 단계: 출발 방식 추천 / 표적 예시 추가 수집 / 다른 모델 클래스 테스트 / 패턴 수정 / 사람 검토 추가 / 전문가 에스컬레이션.

## 5. 4단 모델 선택 결정 구조

| 결정 영역 | 정리하는 것 | 핵심 질문 |
|---|---|---|
| **Task shape·출력 요건** | 워크플로 이름표, 작업 패턴, 필요한 출력, 컨텍스트, 리스크·검토, 빠진 질문, 성공 기준 | AI에게 무엇을 시키나? |
| **모델 클래스·결정 요인** | 유력 클래스와 대안; surface·엔드포인트·버전·reasoning 설정·날짜가 적힌 처리/용량 가정; 지연·합격 결과당 비용·컨텍스트·출력·도구·리스크·eval 필요 | 그럴듯한 출발 가설은? 가장 중요한 트레이드오프는? |
| **솔루션 라우팅·패턴** | 주 surface, 역량·아키텍처, 인터페이스와 runtime 소유자, 환경·접근·운영 제약, 패턴과 이유, 검증할 것 | 어디서 일하고, 무엇을 하고, 어떻게 돌고 통제되나? |
| **Eval 증거·에스컬레이션** | 성공 기준, 라벨된 대표 케이스, 임계값과 오류 결과, 지지/약화 신호, 에스컬레이션 필요 | 고객 대상·구현 준비 단계로 가기 전 어떤 증거가 필요한가? |

### Route · Validate · Escalate

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🧭</span><span class="oa-card__title">Route</span>워크플로가 충분히 명확 → 출발 모델 클래스·경로·패턴 추천</div>
  <div class="oa-card"><span class="oa-card__icon">🔬</span><span class="oa-card__title">Validate</span>증거나 요건이 부족 → 추천 전에 더 검증</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">📣</span><span class="oa-card__title">Escalate</span>제품·보안·데이터·컴플라이언스·아키텍처·구현 디테일 → 전문가 검토</div>
</div>

### 경계를 밝히는 말투

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 이렇게 말하기</span>
    <ul>
      <li>"설명해 주신 워크플로를 기준으로, 이건 <b>검증할 출발 모델 클래스와 솔루션 패턴</b>이지 최종 구현 추천이 아니에요."</li>
      <li>"그럴듯한 경로와 아직 필요한 증거를 정리한 거예요. 고객 대상 사용 전에 제품·보안·데이터·컴플라이언스 등은 전문가 검토가 필요할 수 있어요."</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 초기 추천을 이렇게 포장하지 않기</span>
    <ul>
      <li>확정 아키텍처, 운영 구현 계획, 보안·컴플라이언스 승인</li>
      <li>가격·가용성 약속, 정확한 모델 선정, 벤치마크 결과</li>
      <li>완전한 eval 계획, 상세 API·도구·연동 설계, 고객용 구현 명세</li>
    </ul>
  </div>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. "클레임 검토에 어떤 모델을 써야 하죠?" 워크플로 우선을 지키는 다음 질문은?</summary>
  <p>① 구현 회의에 몇 명 오나요? ② 클레임 검토가 무엇을 해야 하나요? ③ 최신 모델 써도 되나요? ④ 이미 승인된 정확한 모델 이름은?</p>
  <p><b>정답: ②</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 워크플로 요구와 가장 관련 있는 모델 클래스·역량·엔드포인트</summary>
  <p>승인된 사내 문서 검색 → <b>임베딩/검색 지원 역량</b><br>모호한 정책 해석 → <b>추론 지향 모델</b><br>범위가 정해진 조건 판정·고정 선택·루브릭 점수 → <b>Decisions API + GPT-6 Luna</b><br>검토 전 안전 필터링 → <b>모더레이션·안전 역량</b><br>문서 이미지 이해 → <b>멀티모달 모델</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 클레임 처리 앱에 AI를 넣어 유형 분류·필드 추출·정책 검색·예외는 사람 검토로. 가장 강한 초기 패턴 추천은?</summary>
  <p>① Codex를 주 surface로 ② 앱 연동 없이 ChatGPT Chat만 ③ <b>API로 만든 앱/프로세스 + 분류·구조화 추출·검색·사람 검토</b> ④ 워크플로·통제 요건이 정해지기 전에 open 모델 배포 결정</p>
  <p><b>정답: ③</b></p>
</details>

<details class="oa-quiz">
  <summary>Q4. 라우팅 방식이 쉬운 예시에선 잘 되지만, 정한 임계값에서 긴급 건을 놓친다. 다음은?</summary>
  <p>① 텍스트 작업이어도 멀티모달로 ② 평균이 좋으니 추천 ③ 고위험 예시를 eval에서 빼기 ④ <b>실패를 라우팅 증거로 다룬다</b></p>
  <p><b>정답: ④</b> — 임계값 조정, 다른 모델 클래스, 라우팅 로직, 더 강한 검토 게이트를 검토할 근거가 돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q5. 짝 맞추기 — 항목이 속하는 결정 영역</summary>
  <p>워크플로 이름표·작업 패턴·필요한 출력·컨텍스트·리스크 신호 → <b>Task shape·출력 요건</b><br>성공 기준·seed 케이스·지지/약화/에스컬레이션 신호 → <b>Eval 증거·에스컬레이션</b><br>유력 모델 클래스·대안·결정 요인·트레이드오프 → <b>모델 클래스·결정 요인</b><br>주 surface·역량/아키텍처·배포/운영·패턴 조합 → <b>솔루션 라우팅·패턴</b></p>
</details>

## 정리

- "어떤 모델?"의 답은 **task shape**에서 시작: 무엇을 하고, 어떤 출력을, 누가 쓰고, 어떤 리스크·검토가 있나.
- 모델 클래스는 **출발 가설**. 검색·도구(역량)와 호스팅·권한(배포)은 별개 결정. 모델 이름·가격·설정은 (강의 기준) 현재 정보로 확인.
- 비용은 **합격한 결과당 비용**으로, 속도는 **end-to-end 시간**으로 비교. 필요한 출력이 엔드포인트를 정한다.
- 패턴 조합은 **순서 있는 흐름**으로 설명하고, 각 단계의 실패·검증·검토 지점을 밝힌다.
- 성공 기준 2~3개 + 작은 seed set + 같은 조건 비교 + 임계값의 FN/FP 저울질 → **Route · Validate · Escalate**, 그리고 경계를 밝히는 말투.
