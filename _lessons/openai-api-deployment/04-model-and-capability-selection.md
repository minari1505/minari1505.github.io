---
title: "Model and Capability Selection for API Solutions"
title_ko: "모델·역량 선택 — '가장 좋은 모델'이 아니라 '이 워크플로우에 맞는 모델'"
course: openai-api-deployment
lesson: 4
---

## 학습 목표

- 모델을 고르기 **전에** 작업 유형·기술 요구사항·리스크/리뷰 요구사항부터 정의하기
- 공식 모델 카탈로그로 **날짜가 찍힌 후보 목록(shortlist)** 만들기
- 같은 테스트 케이스로 후보를 비교하고 **성공 작업당 총비용**으로 판단하기
- 필요 이상으로 복잡하거나 비싼 후보를 **근거를 들어 탈락**시키기
- 결과를 **Model and Capability Selection Rationale**(선택 근거서)로 정리하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>요구사항 정의</strong>작업 유형 · 기술 조건 · 리스크</li>
  <li><strong>후보 추리기</strong>공식 카탈로그 + 필터 → shortlist</li>
  <li><strong>같은 조건 비교</strong>같은 케이스 · 같은 채점 기준</li>
  <li><strong>스트레스 테스트</strong>비용 · 지연 · 신뢰성 · 안전</li>
  <li><strong>선택 근거서</strong>추천 · 탈락 사유 · 재검증 항목</li>
</ol>

이 코스의 핵심 원칙 한 줄: **미리 정한 품질·안전·지연·비용 기준을 통과하는 후보 중 "가장 덜 복잡한 것"을 고른다.**

<div class="oa-eli">
  <span class="oa-eli__head">🚲 쉽게 말하면</span>
  <p>학교에 갈 때 "제일 비싸고 빠른 차"가 꼭 정답은 아니에요. 집에서 1km면 자전거가 더 싸고, 주차 걱정도 없고, 충분히 빨라요. 반대로 비 오는 날 무거운 짐이 있으면 차가 필요하겠죠.</p>
  <p>모델 선택도 똑같아요. <b>"어디를, 뭘 들고, 얼마나 빨리 가야 하는지"(워크플로우)</b>를 먼저 정해야 탈것(모델)을 고를 수 있어요.</p>
</div>

## 1. 먼저 작업 유형을 분류해요

고객은 "지원 업무를 개선하고 싶어요"처럼 **비즈니스 언어**로 말해요. 우리는 이걸 **기술적인 작업**으로 번역해야 해요.

| 작업 유형 | 의미 | API 동작 예 |
|---|---|---|
| **Generate** 생성 | 지시·문맥으로 새 콘텐츠 만들기 | 답장 초안, 추천 메모 |
| **Summarize** 요약 | 긴 입력을 핵심만 남겨 줄이기 | 상담 스레드·회의록 요약 |
| **Classify** 분류 | 라벨·카테고리·우선순위 붙이기 | "배송 문제 / 환불 / 계정 문제" |
| **Extract** 추출 | 비정형 텍스트에서 특정 필드 뽑기 | 주문번호, 이슈 유형, 감정 |
| **Reason** 추론 | 근거·규칙으로 여러 단계 판단 | 옵션 비교, 기준 충족 여부 판단 |
| **Transform** 변환 | 형식·톤·구조 바꾸기 | 고객 메모 → 내부 인수인계 양식 |
| **Retrieve** 검색 근거 | 승인된 자료에 근거해 답하기 | 최신 정책 문서 기반 답변 |
| **Act through a tool** 도구 실행 | 정해진 함수·시스템으로 조회·실행 | 주문 상태 조회, 티켓 생성 |
| **Live / Multimodal** | 음성·이미지·실시간 상호작용 | 음성 접수, 이미지 검사 |

여러 유형이 섞이면 **"워크플로우가 이어지려면 API가 무엇을 돌려줘야 하나?"** 를 물어 **주된 출력**을 찾아요.

| 주된 출력이… | 핵심 작업은 아마… |
|---|---|
| 라우팅 라벨 | 분류 |
| 필드 목록 | 추출 |
| 출처 기반 답변 | 검색(retrieval) |
| 실시간 시스템 상태에 의존 | 도구 사용 패턴 |
| 음성 대화 | 실시간·음성 역량 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 작업 문장</span>
    <ul><li>"AI로 고객 지원을 개선한다."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 작업 문장</span>
    <ul><li>"짧은 제품 문의를 <b>승인된 카테고리로 분류</b>하고, <b>우선순위</b>와 지원팀용 <b>짧은 감사(audit) 사유</b>를 반환한다."</li></ul>
  </div>
</div>

좋은 작업 문장 = **API가 할 행동 + 입력 종류 + 반환할 출력 + 그 출력의 쓰임새**.

## 2. 기술 요구사항 체크리스트

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🖼️</span><span class="oa-card__title">입출력 모달리티</span>텍스트로 충분한가? 음성·이미지·실시간은 <b>워크플로우가 정당화할 때만</b> (지연·접근성·동의·폴백 부담 증가)</div>
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">추론 깊이</span>패턴 인식·분류 수준인가, 다단계 판단인가? "중요한 일이라서"는 깊은 추론의 이유가 아님</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">필요한 문맥</span>입력+고정 지시로 충분 → 검색·도구 불필요 / 승인 자료 필요 → 검색 / 실시간 상태·행동 → 도구</div>
  <div class="oa-card"><span class="oa-card__icon">🧱</span><span class="oa-card__title">출력 구조</span>다른 시스템이 읽는다면 <b>예측 가능한 필드</b>(구조화 출력)가 핵심</div>
  <div class="oa-card"><span class="oa-card__icon">⏱️</span><span class="oa-card__title">지연 허용치</span>동기·스트리밍·비동기·타임아웃 시 동작. 검증 안 된 지연 수치 약속 금지</div>
  <div class="oa-card"><span class="oa-card__icon">📈</span><span class="oa-card__title">처리량·규모</span>시간/일 요청 수, 몰림(burst)·계절성, 입출력 길이, 성장 예상</div>
  <div class="oa-card"><span class="oa-card__icon">🔁</span><span class="oa-card__title">신뢰성</span>가동률만이 아님: 출력이 늘 유효한지, 안전하게 실패하는지, 문제를 감지할 수 있는지</div>
  <div class="oa-card"><span class="oa-card__icon">📜</span><span class="oa-card__title">계약·인터페이스</span>필수 필드, 에러, 상태, 비동기, 재시도/멱등성, Responses vs Chat Completions</div>
</div>

예를 들어 지원 라우팅 API라면 이런 **결정 수준의 응답 형태**가 필요하다는 걸 먼저 보여 줘요(SDK 예제가 아님).

```json
{
  "routing_label": "delivery_issue",
  "priority": "high",
  "reason": "분실 택배 신고, 긴급 처리 요청",
  "needs_review": true
}
```

## 3. 리스크와 리뷰 요구사항

기술적으로 그럴듯해도, 리스크가 불분명하면 **틀린 추천**이 될 수 있어요.

| 영역 | 물어볼 질문 |
|---|---|
| **민감 데이터** | 개인정보·규제 정보·기밀을 받거나 저장·로그·반환하나? |
| **사람 검토** | 돈·접근권·자격·안전·법적 의무에 영향? 되돌릴 수 없는 행동? 누가 검토하나? |
| **안전 경계** | API가 **하지 말아야 할 것**: 예) 라우팅 API는 환불 약속·계정 수정·내부 정책 공개 금지 |
| **품질 기준** | 무엇이 정답인가? 어떤 오류는 허용, 어떤 오류는 검토·에스컬레이션? |
| **검증 방법** | 대표 입력, 기대 출력, 엣지·실패 케이스, 더 단순한 기준선과 비교 |
| **데이터 접근 경계** | 누가 어떤 데이터에 접근 가능? 절대 노출·로그되면 안 되는 것은? |
| **거버넌스** | 데이터·도구·워크플로우 승인자, 감사 기록, 이상 동작 시 에스컬레이션 경로 |
| **관측 증거** | 분류: 카테고리 분포·무효 출력률·검토율 / 검색: 출처 누락률 / 도구: 호출 실패율 |

<div class="oa-eli">
  <span class="oa-eli__head">🔒 기억할 점</span>
  <p>경계가 불분명한 문제는 <b>더 똑똑한 모델로 해결되지 않아요.</b> 거버넌스 문제는 거버넌스로 풀어야 해요. 그래서 좋은 추천은 늘 <b>조건부</b>예요: "~가 테스트로 확인되면 이 패턴을 쓴다. 실시간 주문 상태가 필요해지면 재검토한다."</p>
</div>

## 4. 후보 목록(shortlist) 만들기 — 깔때기처럼 걸러요

<div class="oa-svg">
<svg viewBox="0 0 720 300" role="img" aria-label="모델 후보를 6개 필터로 거르는 깔때기">
  <defs><style>.f{font-size:13px;fill:#4a4a4a}.ft{font-size:12px;fill:#8a6a00;font-weight:700}</style></defs>
  <polygon points="40,20 680,20 620,62 100,62" fill="#fff3c4" stroke="#f1e4b8"/>
  <text x="360" y="46" text-anchor="middle" class="f">공식 Models · Compare models 카탈로그의 전체 모델</text>
  <polygon points="100,66 620,66 570,104 150,104" fill="#ffecaa" stroke="#f1e4b8"/>
  <text x="360" y="90" text-anchor="middle" class="f">① 작업·모달리티 적합  ② 인터페이스·역량(구조화 출력 등)</text>
  <polygon points="150,108 570,108 525,146 195,146" fill="#ffe48c" stroke="#f1e4b8"/>
  <text x="360" y="132" text-anchor="middle" class="f">③ 문맥·출력 조건  ④ 운영(처리량·rate limit·리전)</text>
  <polygon points="195,150 525,150 485,188 235,188" fill="#ffdd7a" stroke="#f1e4b8"/>
  <text x="360" y="174" text-anchor="middle" class="f">⑤ 비용 입력값  ⑥ 평가 가능성(대표 케이스)</text>
  <rect x="255" y="200" width="210" height="44" rx="10" fill="#fff" stroke="#c99400"/>
  <text x="360" y="220" text-anchor="middle" class="ft">Shortlist 2~3개</text>
  <text x="360" y="236" text-anchor="middle" class="f" style="font-size:11px">모델 ID · 출처 · 확인 날짜 기록</text>
  <text x="360" y="275" text-anchor="middle" class="f" style="font-size:11px;fill:#8a8a8a">필수 조건을 못 맞추면 제외(또는 보류)</text>
</svg>
</div>

각 후보마다 **정확한 모델 ID · 공식 출처 · 확인 날짜 · 포함한 근거**를 적어요. 모델 이름·지원 기능·한도·가격은 계속 바뀌니까요.

<div class="oa-eli">
  <span class="oa-eli__head">🗓️ 강의 시점 예시 (반드시 최신 문서로 재확인)</span>
  <p>강의는 <b>GPT-6.1 Sol</b>(품질·속도·비용 균형이 필요한 복잡한 작업), <b>GPT-6 Astra</b>(가장 까다로운 추론; reasoning <code>none</code> 미지원, 파인튜닝 미지원)를 "테스트해 볼 이유가 있는 후보"로 소개해요. 출시 벤치마크는 <b>테스트할 이유</b>일 뿐 고객 워크로드의 결과가 아니에요.</p>
  <p>또 <b>모델 가용성과 처리 방식(processing) 가용성은 별개 필터</b>예요. 예: Sol의 Ultrafast는 US·EU 데이터 레지던시 지원, Astra Ultrafast는 EU 레지던시 미지원 → EU 처리 요구가 있으면 비교 대상 자체가 바뀌어요.</p>
  <p>범주·점수처럼 <b>정해진 형태의 답</b>만 필요하면 타입 있는 답을 돌려주는 <b>Decisions API</b>도 고려 대상. 단, 설명문(이유)이 필요한 워크플로우의 대체재는 아니에요.</p>
</div>

## 5. 모델 선택의 지름길 함정 6가지

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🆕</span><span class="oa-card__title">제일 새롭고 센 모델</span>추가 능력이 결과를 개선하지 않으면 비용·지연·검증 부담만 늘어요</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🏷️</span><span class="oa-card__title">표시 가격이 제일 싼 모델</span>재시도·긴 출력·실패·검토 인력까지 합치면 더 비쌀 수 있어요</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🏆</span><span class="oa-card__title">일반 벤치마크 = 증명</span>벤치마크는 가설, 증명은 고객 데이터로 한 대표 평가(eval)</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">📦</span><span class="oa-card__title">옛 코드의 모델 재사용</span>지금도 적합한지·지원되는지 다시 확인</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⚖️</span><span class="oa-card__title">다른 조건에서 비교</span>케이스·프롬프트·채점 기준이 다르면 비교 불가</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔖</span><span class="oa-card__title">테스트한 ID 미기록</span>별칭(alias)은 바뀔 수 있어요 → 정확한 모델 ID로 재현 가능하게</div>
</div>

## 6. 같은 조건으로 비교하기

### 필수 조건 vs 선호 조건

| 필수(must-have) — 못 맞추면 탈락 | 선호(preference) — 통과자끼리 비교 |
|---|---|
| 필요한 입출력 모달리티 지원 | 더 낮은 지연 |
| 의도한 인터페이스와 호환 | 더 낮은 작업 비용 |
| 구조화 출력 등 필수 역량 지원 | 어려운 케이스에 대한 추가 능력 |
| 의도한 API 환경에서 사용 가능 | 더 단순한 구현·운영 |
| 문맥·출력 조건 충족 | 더 적은 검토·수정 부담 |

선호 조건이 아무리 좋아도 **필수 조건 하나를 못 맞추면 끝**이에요.

### 비교 매트릭스 예시

| 요구사항 | 후보 A | 후보 B | 후보 C | 의미 |
|---|---|---|---|---|
| 텍스트 입력 + 구조화 출력 | ✅ | ✅ | ✅ | 셋 다 유지 |
| 의도한 인터페이스 | ✅ | ✅ | ❓ 미확인 | C는 확인 후 진행 |
| 문맥·출력 조건 | ✅ | ⚠️ 조건부 | ✅ | B는 미해결 가정 있음 |
| 지연·처리량 | ❓ | ❓ | ❓ | 검증으로 넘김 |
| 추가 능력 | 필요 이상 | 딱 맞음 | 딱 맞음 | A는 더 강한 근거 필요 |
| 운영 복잡도 | 높음 | 낮음 | 중간 | 복잡도가 이득을 주는지? |

### 후보마다 결정 위치를 붙여요

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">✅</span><span class="oa-card__title">Retain 유지</span>문서화된 요구를 충족, 계속 유력</div>
  <div class="oa-card"><span class="oa-card__icon">⚠️</span><span class="oa-card__title">Retain with conditions</span>맞을 수 있지만 특정 가정 확인 필요</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">❌</span><span class="oa-card__title">Remove 제거</span>필수 요구와 문서상 충돌</div>
  <div class="oa-card"><span class="oa-card__icon">⏸️</span><span class="oa-card__title">Defer 보류</span>책임 있게 비교할 근거 부족</div>
</div>

**잠정 선두(provisional lead)** 는 아직 가설이에요. 기존(incumbent) 프롬프트의 비즈니스 규칙·입력·예시·스키마·도구 권한은 유지하고, **호환에 꼭 필요한 것만** 바꾼 뒤 같은 케이스로 돌려요. 실패 케이스가 나오면 **한 번에 하나의 지시만** 바꿔 재실행하고, 전체 세트로 회귀(regression)를 확인해요.

<div class="oa-eli">
  <span class="oa-eli__head">🔬 과학 실험과 같아요</span>
  <p>식물 성장 실험에서 물·햇빛·비료를 한꺼번에 바꾸면 무엇 때문에 자랐는지 모르죠. <b>모델을 비교할 땐 처리 모드를 고정</b>하고, <b>속도를 비교할 땐 모델을 고정</b>해요. 조건을 못 맞췄다면 "모델 차이 때문"이라고 결론 내리면 안 돼요.</p>
</div>

매 실행마다 기록: 모델 ID, 인터페이스, reasoning 설정, 프롬프트·스키마 버전, 케이스 세트 버전, 채점 규칙, 날짜, 케이스별 결과·통과 여부·지연·비용. **실행하지 않은 테스트는 결과 칸을 비우고 planned로 표시** — 그럴듯한 생성 답변은 측정 결과가 아니에요.

## 7. 비용은 "성공한 작업 1건당"으로 계산해요

강의의 가상 예시: 같은 메시지 100개를 처리했을 때 (실제 가격·성능 아님)

<div class="oa-bars">
  <div class="oa-bar"><span>후보 A — $18 / 90건</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:53%"></span><span class="oa-bar__val">$0.20 · 하지만 검토 트리거 기준 탈락 ❌</span></span></div>
  <div class="oa-bar"><span>후보 B — $24 / 96건</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:67%"></span><span class="oa-bar__val">$0.25 · 전 기준 통과 → 추천 ✅</span></span></div>
  <div class="oa-bar"><span>후보 C — $36 / 96건</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:100%"></span><span class="oa-bar__val">$0.375 · 통과했지만 이득 없이 비쌈</span></span></div>
  <p class="oa-bars__caption">막대 = 성공 작업 1건당 비용 (강의의 가상 수치)</p>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🍕 쉽게 말하면</span>
  <p>피자 한 판이 싸도 절반을 태워 버리면, "먹을 수 있는 조각 하나당" 가격은 오히려 비싸요. 그래서 <b>총비용 ÷ 성공 건수</b>로 봐요. 그리고 가장 싼 A라도 <b>필수 기준(검토 트리거)을 못 넘으면 자격 자체가 없어요.</b> 성공 0건이면 이 지표는 계산 불가 → 실패와 지출을 그대로 보고해요.</p>
</div>

총비용에 넣을 것: 입력·출력 사용량, 캐시 읽기/쓰기, reasoning 사용량, 도구 요금, 실패 시도, 허용된 재시도, 검토자 시간, 수정 작업. **차단(blocked)된 응답은 성공이 아니며, 성공률을 올리려고 자동 재시도하면 안 돼요.**

### 비용·용량에서 헷갈리기 쉬운 것들

| 헷갈리는 것 | 실제 |
|---|---|
| 캐시 쓰기 토큰 | 미캐시 입력으로 **또** 과금하지 않음. 캐시는 프롬프트 계산 재사용이지 "검증된 답" 재사용이 아님 |
| 긴 문맥 가격 | 임계값을 넘으면 **요청 전체** 요금이 바뀔 수 있음 → 규칙과 가격 날짜 기록 |
| Decisions API(강의 시점 GPT-6 Luna) | 입력만 과금, 출력·캐시 별도 요금 없음. 단 긴 입력·리전 규칙은 적용 |
| Spend alert(알림) | **트래픽을 멈추지 않음.** 하드 한도(hard limit)를 명시적으로 설정해야 차단되고, 그마저 즉시는 아님 |
| Build · Launch · Grow | API **사용 등급**(모델별 용량 한도) |
| Standard · Fast · Ultrafast | **처리 방식** 선택. 빠른 모드·높은 예산이 처리량 충분을 증명하진 않음 |
| ChatGPT 플랜 | 명시적으로 활성화된 플랜 연결로만 사용 가능. 로그인만으로 API 호출이 구독 비용이 되지 않음 |

## 8. 지연·신뢰성·안전 스트레스 테스트

강의 예시: 계절 피크가 있는 대량 라우팅 API에 **모든 요청에 고가 모델 + 긴 설명 + 폴백 없음** 을 제안했어요. "능력을 더 쓰니 안전하다"처럼 들리지만, 근거가 없어요.

| 영역 | 스트레스 질문 | 왜 중요한가 |
|---|---|---|
| 비용 | 모든 요청이 같은 능력 수준이 필요한가? | 대량에선 불필요한 능력이 큰 비용 |
| 출력 길이 | 하류 시스템이 긴 설명을 쓰나? | 비용·지연·통합 복잡도 증가 |
| 지연 | 접수 화면 기대 속도를 맞추나? | 느린 라우팅은 지원 업무를 막음 |
| 신뢰성 | 필수 4개 필드가 늘 유효한가? | 무효 출력은 하류를 깨뜨림 |
| 안전 | 고우선순위·애매한 건이 검토로 가나? | 조용한 오분류 = 고객·운영 리스크 |
| 검증 | 단순한 기준선보다 낫다는 증거는? | 복잡함 ≠ 더 좋음 |
| 폴백 | 피크·장애 때 어떻게 되나? | 폴백 없음 = 부하 시 리스크 증가 |

**더 나은 추천**: 기준을 통과하는 후보 중 작업 경제성이 가장 좋은 것으로 시작 → 피크·애매·고우선순위 케이스로 검증 → 더 강한 후보는 **증거가 있을 때만** 애매/고위험 케이스에 한정 → 지연 급증·무효 출력·의존성 장애에 대한 폴백 정의.

속도 비교 시엔 모델·reasoning·프롬프트·입력·수락 기준을 고정하고, 도구·대기열·재시도·검토를 포함한 **수락된 결과까지의 총 시간**을 재요. 토큰 생성이 빨라도 워크플로우 전체가 빨라지는 건 아니에요.

## 9. Model and Capability Selection Rationale

| 섹션 | 담을 내용 |
|---|---|
| 워크플로우 작업 | 분류·추출·생성·요약·추론·변환·검색·도구·실시간 중 무엇 |
| 비즈니스 성과 | 속도·품질·일관성·리스크 감소·고객 경험 등 |
| 입출력 모달리티 | 텍스트·구조화 데이터·이미지·오디오·실시간 |
| 필요 역량 | 구조화 출력, 중간 추론, 근거 자료, 도구, 검토 지원 |
| 계약·인터페이스 요구 | 필수 필드, 에러, 상태, 스트리밍, 비동기, 재시도 |
| 후보 패턴 | 구조화 분류 / 검색 기반 / 도구 / 에이전트 / 특수 최적화 |
| **추천 패턴** | 현재 증거에 맞는 **가장 단순한** 패턴 |
| **탈락 대안과 이유** | 왜 첫 선택이 아닌지 |
| 트레이드오프 | 성공 작업 정의, 총비용·성공 수, 건당 비용, 지출 통제 가정 |
| 리스크 · 검증 질문 · 다음 리뷰 영역 | 추천을 바꿀 수 있는 것, 빌드 전 확인할 증거 |

예시의 탈락 사유: 검색 ✗(승인 자료 불필요) · 도구 ✗(실시간 데이터·행동 불필요) · 에이전트 ✗(다단계 조율 없음) · 특수 최적화 ✗(기준선 검증 전이라 시기상조).

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 추천</span>
    <ul><li>"중요한 워크플로우니까 플래그십 모델을 쓰자."</li><li>"최신 모델이 제일 좋으니까."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 추천</span>
    <ul><li>"[날짜]에 카탈로그 확인. 같은 대표 케이스에서 후보 B가 모든 필수 기준을 통과했고 성공 작업당 비용이 가장 좋았다. 정확한 모델 ID·평가 증거를 보관하고, 빌드 전 제품 사실을 재검증한다."</li></ul>
  </div>
</div>

**결정 논리**(예: 저지연 구조화 분류가 필요)와 **변하기 쉬운 제품 사실**(모델 가용성, 구조화 출력 지원, rate limit, 가격)을 **분리**해 두면, 제품이 바뀌어도 근거서 전체를 다시 쓸 필요가 없어요.

### 추천을 바꿀 조건 (조건부인 건 약점이 아니라 좋은 판단)

<ol class="oa-flow">
  <li><strong>승인된 정책에 의존</strong>→ 검색(retrieval) 평가</li>
  <li><strong>실시간 주문 데이터 필요</strong>→ 도구 사용 평가</li>
  <li><strong>기준선 품질 미달</strong>→ 더 강한 모델·최적화 평가</li>
  <li><strong>피크 때 지연 실패</strong>→ 출력 단순화·처리 모드 변경·폴백</li>
  <li><strong>민감 데이터 포함</strong>→ 데이터 처리·접근 통제 리뷰까지 보류</li>
</ol>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 구조화 분류 워크플로우를 정의했고, 카탈로그에 호환 후보가 여러 개 있다. 다음 단계로 가장 좋은 것은?</summary>
  <p>① 지금 플래그십 모델 선택 ② 같은 대표 케이스로 후보들을 돌려 작업 품질·스키마·검토 트리거·지연·성공 작업당 비용 비교 ③ 가장 오래된 동작 코드의 모델 사용 ④ 표시 토큰 가격이 가장 싼 후보 선택</p>
  <p><b>정답: ②</b> — 나머지는 모두 "지름길 함정"이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짧은 구조화 필드만 필요한 대량 라우팅 API에 "고가 모델 + 긴 설명 + 모든 요청에 검색"을 추천했다. 가장 좋은 비평은?</summary>
  <p><b>정답:</b> 추가된 비용·지연·검색·출력 길이가 <b>정당화되지 않았기 때문에</b> 불완전하다. (능력 추가 = 준비도 향상 ✗ / 모든 기업 API에 검색 필요 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 근거서에 추천 후보는 있지만 탈락 이유와 재검증할 제품 사실이 없다. 가장 중요한 개선은?</summary>
  <p><b>정답:</b> 탈락 후보와 그 근거를 추가하고, 모델 정보·가격·한도·가용성 중 <b>재검증할 항목</b>을 기록한다. (날짜 있는 shortlist를 지우는 건 오히려 역효과)</p>
</details>

## 정리

- 모델 선택은 기능 비교가 아니라 **배포 결정**. 워크플로우(작업·입출력·제약)가 먼저다.
- 공식 카탈로그로 **날짜가 찍힌 shortlist** → 필수 조건으로 거르고, **같은 케이스·같은 기준**으로 비교한다.
- 비용은 **성공 작업 1건당 총비용**. 필수 기준 탈락 후보는 아무리 싸도 제외.
- 비교 대상(모델 vs 처리 모드 vs 패턴)을 분리하고, 실행 안 한 테스트는 planned로 남긴다.
- 근거서 = 추천 + **탈락 사유** + 트레이드오프 + **재검증할 변하는 사실** + 추천을 바꿀 조건.
