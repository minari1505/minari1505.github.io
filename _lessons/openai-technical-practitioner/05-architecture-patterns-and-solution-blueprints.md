---
title: "Architecture Patterns & Solution Blueprints"
title_ko: "아키텍처 패턴과 솔루션 블루프린트 — 최종 설계 전에 '해법의 모양'부터"
course: openai-technical-practitioner
lesson: 5
---

## 학습 목표

- OpenAI 기반 솔루션의 **흔한 아키텍처 패턴** 알아보기
- 검색 기반 생성(retrieval-grounded generation)·에이전트 조율·사람 검토·멀티모달·음성·엔터프라이즈 연동 패턴이 **언제 맞는지**, 그리고 **랭킹/리랭킹**이 언제 도움 되는지 구분하기
- 블루프린트가 사용자 요구·데이터·컨텍스트·도구·런타임·거버넌스·성공 지표를 **어떻게 잇는지** 설명하기
- 첫 설계(first-pass)의 **가정·리스크·검증 필요 사항** 드러내기
- 4섹션 **OpenAI Solution Blueprint**로 전달 경로(route)를 비교하고 잠정 선택을 정당화하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>패턴부터</strong>패턴 · 블루프린트 · 최종 설계는 서로 다른 층</li>
  <li><strong>지식 vs 조율</strong>검색 기반 생성과 에이전트 조율을 헷갈리지 않기</li>
  <li><strong>수식어 붙이기</strong>사람 검토 · 모달리티 · 엔터프라이즈 연동 · 하이브리드</li>
  <li><strong>데이터 요건</strong>어떤 경로는 데이터 요건 때문에 아예 탈락</li>
  <li><strong>블루프린트 작성·압박 테스트</strong>4섹션 메모 + 경로 비교</li>
</ol>

## 1. 블루프린트보다 패턴 먼저

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면</span>
  <p>집을 지을 때 "단독주택이냐, 아파트냐, 복층이냐"가 <b>패턴</b>이에요. "우리 가족 넷이 살 2층 단독, 거실은 남향, 주차는 아직 미정"이 <b>블루프린트(초안 도면)</b>고, 배관·전기·내진 설계까지 다 들어간 시공 도면이 <b>최종 설계</b>예요. 이 코스는 앞의 두 단계만 다뤄요.</p>
</div>

**아키텍처 패턴** = AI 솔루션이 어떻게 동작할지 설명하는 **재사용 가능한 모양**. 누가 쓰고, 어떤 정보가 필요하고, AI가 무슨 일을 하고, 어떤 도구·시스템이 끼고, 어디서 사람이 검토하고, 성공을 어떻게 판단하고 무엇을 모니터링할지를 공통 언어로 말하게 해 줘요. 매번 "맞춤형 일회성"으로 취급하거나 기술 선택부터 뛰어드는 걸 막아 줘요.

### 세 단계의 기술 대화

<div class="oa-svg">
<svg viewBox="0 0 720 200" role="img" aria-label="패턴, 첫 블루프린트, 최종 기술 설계로 올라가는 사다리">
  <defs>
    <style>.b5{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.g5{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.x5{fill:#f6f6f6;stroke:#dddddd;stroke-width:1.5;stroke-dasharray:4 3}.t5{font-size:13px;fill:#4a4a4a}.s5{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="20" y="130" width="220" height="55" rx="10" class="b5"/>
  <text x="130" y="152" text-anchor="middle" class="t5">① 아키텍처 패턴</text>
  <text x="130" y="171" text-anchor="middle" class="s5">어떤 공통 모양이 맞나?</text>
  <rect x="250" y="75" width="220" height="110" rx="10" class="g5"/>
  <text x="360" y="97" text-anchor="middle" class="t5">② 첫 블루프린트</text>
  <text x="360" y="117" text-anchor="middle" class="s5">이 워크플로에 어떻게 적용?</text>
  <text x="360" y="135" text-anchor="middle" class="s5">아직 무엇을 검증해야?</text>
  <text x="360" y="160" text-anchor="middle" class="s5">← 이 코스의 범위 (①+②)</text>
  <rect x="480" y="20" width="220" height="165" rx="10" class="x5"/>
  <text x="590" y="42" text-anchor="middle" class="t5">③ 최종 기술 설계</text>
  <text x="590" y="62" text-anchor="middle" class="s5">검증된 구현 계획</text>
  <text x="590" y="80" text-anchor="middle" class="s5">인증 · 데이터 처리 · 로깅</text>
  <text x="590" y="98" text-anchor="middle" class="s5">모니터링 · 보안 통제</text>
  <text x="590" y="116" text-anchor="middle" class="s5">연동 설계 · DB 구조</text>
  <text x="590" y="140" text-anchor="middle" class="s5">기술 전문가가 소유</text>
</svg>
</div>

블루프린트는 아키텍처·보안·데이터 처리·배포·컴플라이언스가 **이미 검증됐다고 주장하면 안 돼요.** 이런 말투가 좋아요: *"그럴듯한 출발 경로로 보이지만, 원천·접근·런타임·도구·검토·eval·거버넌스 가정을 검증한 뒤에 추천으로 다뤄야 해요."*

### 흔한 패턴 패밀리

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">검색 기반 생성</span>승인된 원천으로 답·요약·초안</div>
  <div class="oa-card"><span class="oa-card__icon">🧾</span><span class="oa-card__title">구조화 추출·변환</span>문서에서 필드 뽑기·형식 바꾸기</div>
  <div class="oa-card"><span class="oa-card__icon">🤖</span><span class="oa-card__title">에이전트 워크플로 조율</span>여러 단계·도구·승인</div>
  <div class="oa-card"><span class="oa-card__icon">🔢</span><span class="oa-card__title">랭킹/리랭킹</span>검색된 후보를 관련도 순으로</div>
  <div class="oa-card"><span class="oa-card__icon">🧑‍⚖️</span><span class="oa-card__title">사람 검토(HITL)</span>판단·책임이 필요한 곳</div>
  <div class="oa-card"><span class="oa-card__icon">🖼️</span><span class="oa-card__title">멀티모달</span>이미지·스캔·스크린샷</div>
  <div class="oa-card"><span class="oa-card__icon">🎙️</span><span class="oa-card__title">음성·실시간</span>라이브 대화·핸즈프리</div>
  <div class="oa-card"><span class="oa-card__icon">🏢</span><span class="oa-card__title">엔터프라이즈 연동</span>기존 시스템·프로세스와 연결</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🧩</span><span class="oa-card__title">하이브리드</span>여러 패턴의 조합 — 실제로 가장 흔함</div>
</div>

### 패턴 ≠ 전달 경로(route)

**패턴은 "어떤 일"**, **경로는 "어떻게 전달하나"**예요. 세 경로는 서로 바꿔 끼울 수 있는 게 아니에요 (강의 기준):

| 경로 | 맞는 상황 |
|---|---|
| **공유 팀 워크플로** (예: Team Task) | 팀 소유자·공유 결과가 있는 반복 사내 프로세스 |
| **API 서비스** | 애플리케이션에 지능을 넣고, 앱 특화 연동·운영 요건이 있을 때 |
| **Plugin 기반 경험** | ChatGPT 안에서 도메인 도구나 인터페이스가 필요할 때. plugin이 API 서비스를 호출할 수도 있음 |

### 첫 블루프린트가 보여 줘야 할 것

워크플로·사용자 그룹 / 선택한 패턴(조합) / 컨텍스트·데이터 원천 / 런타임·도구·연동 가정 / 사람 검토·에스컬레이션 지점 / eval·관측성·성공 신호 / 리스크·가정·검증 필요·다음 단계.

- **런타임** = 단계를 조율하는 소프트웨어(기록 모으기 → 초안 요청 → 검토로 넘기기). **실행 환경**은 도구·코드가 돌아가는 곳. Responses API를 고른다고 누가 조율 소프트웨어를 운영할지가 정해지진 않아요 — 팀 앱(선택적으로 Agents SDK) 또는 관리형 Agents API(public beta, 세션·컨텍스트 압축·끊김 후 복구) (강의 기준).
- **관측성** = 출력·오류·도구 호출·승인·에스컬레이션 기록으로 무슨 일이 있었는지 보는 것. 블루프린트엔 **어떤 이벤트를 기록하고 누가 검토하는지** 적어요.

> 좋은 첫 블루프린트는 **기술 대화를 할 만큼 구체적**이고, **아직 모르는 걸 숨기지 않을 만큼 정직**해요. 두 가지 실수 — 너무 모호하거나, 가정을 테스트하기도 전에 너무 상세하거나 — 를 피해요.

### 실제 예: HR 정책 어시스턴트 → 블루프린트

| 항목 | 내용 |
|---|---|
| 워크플로 | 직원이 정책을 묻고, 후속 조치 초안을 받을 수 있음 |
| 사용자 | 직원, HR 운영 검토자, (에스컬레이션 시) 관리자 |
| 주 패턴 | **검색 기반 생성** |
| 보조 요소 | 검색된 정책 구절 **랭킹/리랭킹**, 민감·근거 없음·모호 건 사람 검토, 후속 조치 초안은 구조화 출력 |
| 컨텍스트·데이터 | 승인된 HR 정책 문서, 원천 소유자, 업데이트 프로세스, 역할 기반 접근 가정 |
| 도구·연동 | 첫 버전은 **티켓 생성 없음** — 나중의 검증 항목 |
| 검토 | 민감한 고용 질문·근거 없는 답·직원 결정에 영향 줄 건은 HR 검토 |
| eval | 근거성·완전성·출처 정확성·근거 없는 답 처리·에스컬레이션 정확도 |
| 리스크·검증 | 원천 소유, 접근 권한, 정책 최신성, 민감 주제 규칙, 향후 티켓 생성 승인 여부 |

> 토픽 빈도 분석·군집화·이상치 탐지는 **별도 분석 작업**이에요. 랭킹은 "이 질문에 가장 관련 있는 구절"을 앞으로 보내는 일이지, 질문들을 키워드로 묶는 일이 아니에요.

## 2. 지식 패턴 vs 조율 패턴

<div class="oa-eli">
  <span class="oa-eli__head">📖 vs 🧑‍🍳 쉽게 말하면</span>
  <p><b>검색 기반 생성</b>은 "교과서를 펴 놓고 답하는 학생"이에요 — 정해진 책에서만 답하고, 책에 없으면 "선생님께 여쭤볼게요"라고 해야 하죠. <b>에이전트 조율</b>은 "요리사"예요 — 재료를 확인하고, 불을 켜고, 맛보고, 손님에게 확인받고, 접시를 내가요. 질문에 답하는 게 목적인데 요리사를 부르면 과해요.</p>
</div>

### 검색 기반 생성(Retrieval-grounded generation)

맞는 신호: 사내 문서·정책·레코드·지식 베이스에 의존 / 원천에 근거해야 함 / 출처·증거 필요 / 원천 통제가 필요 / 근거 없는 답을 피해야 함.

<ol class="oa-flow">
  <li><strong>검색</strong>질문에 대한 후보 구절(chunk) 가져오기</li>
  <li><strong>랭킹/리랭킹</strong>관련도 높은 승인된 컨텍스트를 앞으로</li>
  <li><strong>생성</strong>그 컨텍스트로 답·요약·초안</li>
  <li><strong>안전한 경로</strong>근거 부족 시 확인 질문·한계 표시·검토자·에스컬레이션</li>
</ol>

블루프린트 구성 요소: 사용자 질문/작업, 승인된 원천, 원천·유지 관리 소유자, 접근·권한 가정, 검색/컨텍스트 메커니즘, 출력 형식, 출처·증거 기대, 민감 주제 검토·에스컬레이션, 근거성·완전성·근거 없는 답 eval, 원천 업데이트 프로세스.

<div class="oa-eli">
  <span class="oa-eli__head">💼 실제 사례 (강의 기준)</span>
  <p>한 자산관리 회사가 어드바이저들이 사내 문서를 찾는 시간을 줄이려고 문서 라이브러리 기반 Q&A 어시스턴트를 만들었어요. 어드바이저와 프롬프트 엔지니어가 정확성·일관성을 채점하고, 검색 방식을 다듬고, <b>매일 회귀 테스트</b>를 돌렸어요. 이후 어드바이저 팀의 98% 이상이 활발히 쓴다고 보고했어요. 단, 이건 <b>사용 범위</b> 수치이고 정확도는 별도로 평가됐어요. 다른 조직은 <b>자기 문서와 질문으로</b> 다시 테스트해야 해요.</p>
</div>

가장 중요한 질문: **답을 올바른 원천으로 되짚을 수 있나?** 승인된 내용으로 뒷받침되지 않으면 "그냥 답하기"를 가정하지 말고 더 안전한 동작을 정해요.

### 에이전트 조율(Agentic orchestration)

맞는 신호: 여러 단계가 필요한 목표 / 시스템 간 도구 사용 / 상태·핸드오프·워크플로 컨텍스트 / 사람 승인 / **완료 정의** / 실패 처리 / 로깅·트레이싱 / 결과를 향한 반복.

| 블루프린트에 적을 것 | 예 |
|---|---|
| 트리거(시작점) | 사용자 요청, 양식 제출, 새 레코드, 예약 작업, 시스템 이벤트 |
| 목표 + 완료 정의 | "매니저가 승인한 추천이 기록됨" |
| 주요 단계 | 컨텍스트 수집 → 기록 확인 → 추천 초안 → 승인 요청 → 시스템 업데이트 → 결과 로깅 |
| 도구·시스템 / 승인 지점 | 어디서 시스템을 건드리고, 어디서 사람이 승인하나 |
| 실패·대체 동작 | 도구 불가, 승인 거부, 데이터 누락, 확신 부족 시 |
| 소유·에스컬레이션, eval·트레이스 | 규모가 커져도 일관되게 하는 엄격한 가드레일 |

<div class="oa-eli">
  <span class="oa-eli__head">🎯 단순한 패턴으로 충분할 때 (강의 기준)</span>
  <p>접수팀이 "이 양식을 어느 검토 큐로?"만 알면 된다면 에이전트가 필요 없어요. <b>Decisions API</b>(public beta, 현재 GPT-6 Luna)가 팀이 준 카테고리 중 하나를 돌려주고(choice), 조건이 참일 확률(predicate)이나 단계 기준 점수(score)도 줄 수 있어요. 앱은 그 답을 라우팅 규칙에 쓰지만, <b>타입이 정해진 답이 행동을 인가하지는 않아요.</b></p>
</div>

**호스팅 컴퓨터 사용**(강의 기준): 적합한 데이터 연결이 없고 승인된 웹사이트에만 정보가 있으면, Agents API 에이전트가 OpenAI 호스팅 브라우저로 페이지를 열고 조작할 수 있어요. 기존 데이터 연결이 충분하면 그걸 유지해요. 웹사이트 접근 승인 ≠ 행동마다 멈춤 보장 → 읽기 전용이면 바꿀 수 없게 제한, 행동별 승인이 필요하면 팀이 통제하는 브라우저 환경. 끊겼다고 에이전트가 멈춘 게 아니니, **재시도 전에 기존 세션 진행 상황부터 확인**해요.

### 지식·조율 패턴의 흔한 리스크

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🗂️</span><span class="oa-card__title">원천 품질</span>불완전·오래됨·중복·충돌 → 자신감 있게 틀림</div>
  <div class="oa-card"><span class="oa-card__icon">🔐</span><span class="oa-card__title">접근 로직 누락</span>누가 어떤 원천을 볼 수 있나</div>
  <div class="oa-card"><span class="oa-card__icon">🔧</span><span class="oa-card__title">도구 오용</span>"호출할 수 있다" ≠ "안전하다"</div>
  <div class="oa-card"><span class="oa-card__icon">👤</span><span class="oa-card__title">검토 소유 불명</span>"사람 검토 필요"만으론 책임이 안 생김</div>
  <div class="oa-card"><span class="oa-card__icon">📏</span><span class="oa-card__title">약한 성공 기준</span>eval·관측성 부족</div>
  <div class="oa-card"><span class="oa-card__icon">🔗</span><span class="oa-card__title">미검증 연동 가정</span>시스템 연결이 될 거라는 추측</div>
</div>

### 예시: 월간 예외 보고서 — 세 경로 비교 (강의 기준)

첫 버전: 승인된 운영 기록으로 AI가 보고서·추천 다음 단계 초안 → 매니저 검토. 시스템 업데이트 없음. → **검색 기반 + 읽기 전용**으로 충분. "월간"이라는 이유만으로 자동 예약이 필요한 건 아니에요.

변형: 매월 일정에 맞춰 자동 시작, 담당자 부재 시 동료가 지시를 유지, 기존 운영 앱 안에 넣을 필요 없음.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">👥</span><span class="oa-card__title">공유 팀 워크플로 ✅ 선호</span>Team Task가 일정에 따라 팀 서비스 계정·설정된 연결로 기록을 모아 초안. 공유 Page에 검토된 내용 반영. 단, Page 독자가 적절해야 하고 Page는 스스로 유지되지 않음</div>
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">API 서비스</span>앱이 기록 수집·초안 요청·매니저 라우팅. <b>앱 특화 연동·통제</b>가 필요할 때 추가 구현 비용이 정당화</div>
  <div class="oa-card"><span class="oa-card__icon">🧩</span><span class="oa-card__title">Plugin 경험</span>ChatGPT 검토 패널에서 근거 기록 조회. 인터페이스일 뿐, 예약이나 기록 변경 권한을 주지 않음</div>
</div>

> Team Task 선택도 **가용성·연결·Page 독자·검토 방식**이 검증될 때까진 제안이에요. 실패한 실행은 누가 조사할지도 합의해야 해요. 핵심: **초안 작성만으로 에이전트가 되는 게 아니에요.** 더 많은 조율은 도구·승인·시스템 행동 같은 워크플로상의 이유가 있어야 해요.

## 3. 블루프린트 수식어 — 검토·모달리티·연동

### 사람 검토는 구체적으로

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍🏫 쉽게 말하면</span>
  <p>"누가 한번 봐 줘"라고만 하면 아무도 제대로 안 봐요. "담임 선생님이 <b>맞춤법과 사실 관계</b>를 확인하고, 틀리면 <b>다시 써 오게</b> 한다"처럼 <b>누가 · 무엇을 · 언제 · 실패하면 어떻게</b>가 있어야 검토가 진짜 책임이 돼요.</p>
</div>

검토가 필요한 신호: 고객 대상 출력, 고위험·판단 위주 결정, 민감 데이터, 컴플라이언스 민감 콘텐츠, 행동 도구, 낮은 확신, 예외·모호 건. 사람의 역할도 정해요: 정확성 검토? 행동 승인? 예외 처리? 민감 내용 수정? 에스컬레이션 판단? 개선 피드백?

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약함</span>
    <ul><li>"위험한 건은 human in the loop."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강함</span>
    <ul><li>"환불·계정 해지·법률 문구·미해결 고객 영향 이슈가 포함된 추천은 발송 전 <b>지원 매니저</b>가 검토한다. <b>정책 부합·출처 근거·어조·고객 영향</b>을 확인하고, 근거가 없으면 <b>지원 리드에게 에스컬레이션</b>한다."</li></ul>
  </div>
</div>

검토 실패 시 다음 단계(수정·재작성 요청·전문가 에스컬레이션·근거 없음 표시·행동 차단)를 정하고, 검토자 수정·반려·에스컬레이션·반복 실패를 **프롬프트·원천·eval·안전장치 개선 루프**로 돌려요. 단, 모든 곳에 검토를 넣는 게 아니라 **리스크를 의미 있게 줄이는 곳에** 둬요.

### 모달리티가 블루프린트를 바꾼다

멀티모달·음성이 맞는 신호: 이미지·스크린샷·양식·스캔 문서, 오디오 입출력, 핸즈프리·현장 작업, 라이브 안내, 고객 통화, 접근성, 실시간 통역. 핵심 질문: **모달리티가 워크플로의 가치나 실현 가능성을 바꾸나?** 텍스트 양식·비동기 채팅·문서 업로드로 같은 결과가 나오면 불필요해요.

| 블루프린트에 적을 것 | 현장 음성 예 | 스크린샷 지원 예 |
|---|---|---|
| 입력/출력 모달리티 | 말 → 말 | 이미지 → 텍스트 |
| 사용자 환경 | 소음, 손이 바쁨 | 다양한 화질 |
| 지연 요건 | 즉각 응답 | 수 초 허용 |
| 동의·프라이버시·접근성 | 녹음 동의 | 스크린샷 속 민감 정보 |
| 실패 동작 | 잘못 들었을 때 | 오류 메시지를 잘못 읽었을 때 |
| 모달리티별 eval | 전사 정확도·턴 교대·지연 | 이미지 해석·추출·누락된 시각 정보 |

### 엔터프라이즈 연동과 하이브리드

연동 블루프린트에 적을 것: 관련 시스템, 데이터 이동 가정, 접근·권한 가정, 도구·연동 소유, 승인 지점, 관측성, 검증 질문, 핸드오프·에스컬레이션 경로.

<div class="oa-eli">
  <span class="oa-eli__head">🛋️ 실제 사례 (강의 기준)</span>
  <p>한 홈 리빙 리테일러가 상품 태그(색상·소재 등)의 누락·오류를 고치려고 OpenAI 모델 + 상품 데이터 + 태그 정의로 <b>재사용 가능한 시스템</b>을 만들었어요. 직원이 실물 샘플로 대조하고 공급사가 검증을 도왔어요. <b>근거가 충분한 저위험 수정은 자동 반영 후 공급사에 알림</b>, <b>고위험·불확실 수정은 공급사 확인 먼저</b>. 2026년 3월 기준 100만 개 이상 상품에서 250만 개 태그를 수정했다고 보고했어요. 블루프린트로 보면 핵심은 <b>자동 업데이트 vs 공급사 검토로 갈라지는 분기점</b>이에요.</p>
</div>

- 보고서 예시에서 plugin 패널 + MCP 도구(원천 기록 조회) + MCP Events(레코드 준비 시 초안 준비)를 쓸 수 있지만, **패널과 이벤트는 원천 시스템 수정 권한을 주지 않아요.**
- **Intelligent UI**(Chat 응답 속 차트·계산기)는 탐색용. 검토된 보고서를 어디에 저장·갱신할지는 따로.
- **ChatGPT Sites**: 방문자 연결(방문 중 읽기 전용, 제작자 자격 증명·예약 접근 아님) vs Site 호스팅 plugin(Site 자체 데이터 작업, 외부 시스템 임의 쓰기 아님). Site 공유와 plugin 공유는 별개.

### 데이터 요건이 경로를 탈락시킬 수 있다 (강의 기준)

<div class="oa-eli">
  <span class="oa-eli__head">🚪 쉽게 말하면</span>
  <p>"반려동물 금지" 아파트라면 아무리 좋은 집이어도 강아지 키우는 가족에겐 후보가 될 수 없어요. 데이터 보존·거주 요건도 같아요 — <b>조건 하나로 경로 자체가 탈락</b>할 수 있어요.</p>
</div>

| 항목 | 핵심 |
|---|---|
| **ZDR + Private Safety Processing (PSP)** | 적격 API 구성에서 암호화된 안전 기록을 **고객 클라우드 저장소**에 둠. 고객이 저장소·키·접근을 관리하고 **최소 30일** 보관. 소유자 필요. 앱 로그·제3자 보존·엔드포인트 제약은 그대로 |
| **Agents API** | 세션 데이터 보존, 현재 **미국 전용** 거주, **ZDR 미지원**(셀프 호스팅 샌드박스여도). ZDR이나 타 지역 거주가 필수면 이 경로는 탈락. PSP를 더해도 해결 안 됨 |
| **Private Inference** | 모델이 요청을 처리하는 **동안** 데이터 보호(기밀 컴퓨팅). 2026년 가을 별도 프리뷰 발표 — 가용성·범위 확인 필요. 안전 기록의 보관 위치·접근·기간 문제는 별개 |
| **PHI (해당 시에만)** | API는 체결된 **BAA + Modified Retention**, 선택 엔드포인트 커버 필요. ChatGPT는 기능별 커버리지 — 이벤트 트리거 예약 작업은 PHI 미커버. 기능이 켜져 있다고 커버되는 게 아님 |

### 예시: 하이브리드 블루프린트 — 접수(intake) 워크플로

<div class="oa-svg">
<svg viewBox="0 0 720 230" role="img" aria-label="정책 검색과 양식 추출이 자격 규칙과 Decisions 단계로, 초안 생성과 매니저 승인을 거쳐 레코드 업데이트로 가는 하이브리드 흐름">
  <defs>
    <marker id="ar5h" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.b5h{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.g5h{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.r5h{fill:#fdf3f3;stroke:#efc9c9;stroke-width:1.5}.t5h{font-size:12px;fill:#4a4a4a}.s5h{font-size:10px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="20" width="130" height="55" rx="10" class="b5h"/>
  <text x="75" y="43" text-anchor="middle" class="t5h">정책 검색</text>
  <text x="75" y="61" text-anchor="middle" class="s5h">승인된 정책 원천</text>
  <rect x="10" y="105" width="130" height="55" rx="10" class="b5h"/>
  <text x="75" y="128" text-anchor="middle" class="t5h">양식 필드 추출</text>
  <text x="75" y="146" text-anchor="middle" class="s5h">구조화 추출</text>
  <line x1="142" y1="48" x2="176" y2="80" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar5h)"/>
  <line x1="142" y1="132" x2="176" y2="100" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar5h)"/>
  <rect x="180" y="55" width="140" height="70" rx="10" class="b5h"/>
  <text x="250" y="78" text-anchor="middle" class="t5h">자격 확인</text>
  <text x="250" y="96" text-anchor="middle" class="s5h">고정 규칙 → 앱이 직접</text>
  <text x="250" y="112" text-anchor="middle" class="s5h">서술형 → Decisions 단계</text>
  <line x1="322" y1="90" x2="346" y2="90" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar5h)"/>
  <rect x="350" y="55" width="110" height="70" rx="10" class="g5h"/>
  <text x="405" y="84" text-anchor="middle" class="t5h">추천 초안</text>
  <text x="405" y="102" text-anchor="middle" class="s5h">별도 생성 단계</text>
  <line x1="462" y1="90" x2="486" y2="90" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar5h)"/>
  <rect x="490" y="55" width="105" height="70" rx="10" class="r5h"/>
  <text x="542" y="84" text-anchor="middle" class="t5h">매니저 승인</text>
  <text x="542" y="102" text-anchor="middle" class="s5h">사람 검토</text>
  <line x1="597" y1="90" x2="611" y2="90" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar5h)"/>
  <rect x="615" y="55" width="100" height="70" rx="10" class="g5h"/>
  <text x="665" y="84" text-anchor="middle" class="t5h">레코드 업데이트</text>
  <text x="665" y="102" text-anchor="middle" class="s5h">인가된 연동</text>
  <text x="360" y="175" text-anchor="middle" class="s5h">확률(predicate)엔 컷오프를 정하고 라벨된 케이스로 테스트:</text>
  <text x="360" y="193" text-anchor="middle" class="s5h">부적격 건 통과 vs 적격 건 지연 — 결과의 무게가 달라요</text>
  <text x="360" y="215" text-anchor="middle" class="s5h">증거가 없거나 충돌하면 앱이 승인을 가정하지 말고 검토자가 해결</text>
</svg>
</div>

주 패턴은 **에이전트 조율**일 수 있어요(접수 → 승인 → 기록 업데이트 전체 조율이 핵심이면). 나머지 — 검색(근거), 추출(구조화), 검토(책임), 연동(기존 시스템) — 은 보조 패턴이에요.

### 합격한 결과당 비용으로 경로 비교

<div class="oa-eli">
  <span class="oa-eli__head">🧮 쉽게 말하면</span>
  <p>싼 초안이라도 고치는 데 시간이 오래 걸리면 결국 더 비싸요. 그래서 <b>(모델·도구 사용 + 실패 실행 + 재시도 + 사람 검토·수정 시간) ÷ 매니저가 합의 기준으로 승인한 추천 수</b>로 비교해요. 오류 유형과 제출자가 기다리는 시간도 함께 봐요.</p>
</div>

- GPT-6.1 Sol이 까다로운 업무에서 GPT-6 Astra에 근접하면서 토큰 가격이 낮다고 보고됨(강의 기준) → 테스트해 볼 가능성. 단, 같은 합격 기준을 재시도·사람 수정 증가 없이 맞추는지 보여야 해요.
- 과금 방식도 달라요: Decisions는 해당 엔드포인트 입력에 과금, 추천 생성 요청은 별도. **ChatGPT 구독으로 돌아가는 작업에 API 토큰 단가를 적용하면 비용이 왜곡**돼요.
- 추정치엔 사용한 모델·제품·엔드포인트 가격과 **확인 날짜**를 적고, 측정 전엔 절감을 주장하지 말고 **필요한 증거**를 적어요.

## 4. 블루프린트 작성과 압박 테스트

### 4섹션 OpenAI Solution Blueprint (= 아키텍처 선택 메모)

| 섹션 | 담을 내용 |
|---|---|
| **1. 워크플로·사용자·선택 패턴** | 결과와 사용자, 패턴(어떻게 일하나), 경로 비교(팀 워크플로 / API / plugin 중 왜 이것, 혹은 조합 시 각 구성 요소의 역할) |
| **2. 컨텍스트·데이터·런타임·도구·연동 가정** | 승인된 원천·도구, **누구 계정으로 접근**, 누가 조율 소프트웨어 운영, 상태·출력 저장 위치·기간·통제자·독자. 실행 시점에 따라 접근이 달라짐(방문 중 접근 ≠ 예약·이벤트 작업) |
| **3. 사람 검토·에스컬레이션·eval·성공 신호** | 검토자, 승인 필요 행동, 에스컬레이션 시점, 품질·오류 기준, end-to-end 지연, 합격 결과당 비용, 실패·복구 테스트. 범위 결정 단계는 필요할 때만 |
| **4. 리스크·검증·다음 기술 단계** | 각 불확실성 → 필요한 증거 → 담당자 → 실용적 다음 단계. PHI가 관련될 때만 계약·기능 커버리지 확인 |

### 가볍게 유지하기

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 블루프린트가 할 말</span>
    <ul><li>"첫 버전은 승인된 운영 기록을 쓰고 시스템 쓰기는 없어요. 원천 소유와 접근 경계는 검증이 필요해요."</li></ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 블루프린트가 되면 안 되는 것</span>
    <ul><li>전체 아키텍처 다이어그램, 클라우드 배포 계획, 상세 연동 맵</li><li>보안·거버넌스 설계, 제품 설정 가이드, 최종 eval·관측성 명세</li><li>데이터 모델·인증·보존 정책·모니터링이 "승인됐다"는 주장</li></ul>
  </div>
</div>

### 압박 테스트 질문

- 워크플로가 분명한가? 패턴과 경로가 대안보다 **왜** 더 맞나?
- 원천 접근·출력 독자·상태·데이터 보관 가정이 보이나?
- 런타임 소유자·연결 신원·허용 행동·복구 경로가 분명한가?
- 사람 검토가 중요한 곳에 있나? 품질·오류·지연·합격 결과당 비용 테스트가 선택을 뒷받침하나?
- 리스크·가정·에스컬레이션 지점이 보이나? 토론할 만큼 단순한가?

> 열린 질문이 있어도 토론 준비가 된 블루프린트일 수 있어요. 오히려 **열린 질문을 드러내는 게 목적**이에요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 짝 맞추기 — 각 항목의 수준</summary>
  <p>정책 어시스턴트가 승인된 HR 문서를 쓰고, 출처를 보여 주고, 민감 질문을 에스컬레이션하며, 티켓 생성은 검증 항목으로 둔다 → <b>첫 블루프린트</b><br>구조화 추출 + 예외 사람 검토 → <b>아키텍처 패턴 조합</b><br>검색 기반 생성 + 사람 검토 → <b>아키텍처 패턴</b><br>상세한 인증·보존·로깅·배포·연동·지원 설계 → <b>최종 기술 설계</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. 승인된 정책 원천, 출처 인용 답변, 정책 내용이 없으면 에스컬레이션. 가장 유력한 패턴은?</summary>
  <p>① 음성·실시간 ② 쓰기 포함 엔터프라이즈 연동 ③ 기본값으로 에이전트 조율 ④ <b>검색 기반 생성</b></p>
  <p><b>정답: ④</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 짝 맞추기 — 워크플로 신호와 블루프린트 수식어</summary>
  <p>입력에 스캔 양식 포함 → <b>멀티모달 워크플로</b><br>시스템 레코드를 업데이트 → <b>엔터프라이즈 연동</b><br>출력이 고객에게 감 → <b>사람 검토</b><br>라이브 음성 지원 필요 → <b>음성·실시간 상호작용</b><br>검색·도구 사용·승인을 결합 → <b>하이브리드 패턴</b></p>
</details>

<details class="oa-quiz">
  <summary>Q4. 매월 보고서 초안 예약 생성, 담당자 부재 시 동료가 지시 유지, 배포 전 매니저 검토, 앱 내장·시스템 쓰기 불필요. 경로를 가장 잘 정당화한 메모는?</summary>
  <p>① 토큰 가격이 가장 싸니 API 서비스, 연동·검토 노력은 첫 보고서 후 평가 ② 매니저가 인터페이스를 좋아하니 plugin, 이를 예약 접근·권한이 해결된 증거로 ③ <b>공유 예약에는 커스텀 빌드보다 Team Task를 선호하고, 원천 접근·검토·공유·복구를 검증</b> ④ 공유 Page를 완전한 아키텍처로, 원본 권한이 복사된 내용까지 제한한다고 가정</p>
  <p><b>정답: ③</b></p>
</details>

## 정리

- **패턴(공통 모양) → 블루프린트(이 워크플로 적용 + 검증할 것) → 최종 설계(전문가 소유)**. 이 코스는 앞의 둘.
- **패턴은 "무슨 일", 경로는 "어떻게 전달"**: 팀 워크플로 / API 서비스 / plugin은 서로 대체재가 아니며 조합 가능.
- 근거 있는 답이 핵심이면 **검색 기반 생성(+랭킹)**, 단계·도구·승인 조율이 핵심이면 **에이전트 조율**. 단순한 패턴부터.
- 사람 검토는 **누가·무엇을·언제·실패 시**까지, 모달리티는 **워크플로가 요구할 때만**, 연동은 **연결 지점과 소유**를 드러낸다.
- 데이터 보존·거주·PHI 요건은 **경로 자체를 탈락**시킬 수 있다. 비용은 **합격 결과당 비용**으로, 과금 방식 차이까지.
- 4섹션 블루프린트 = **아키텍처 선택 메모**: 잠정 선택 + 이유 + 선택을 뒤집을 증거 + 담당자 있는 다음 단계.
