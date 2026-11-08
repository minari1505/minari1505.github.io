---
title: "AI Safety & Responsible AI"
title_ko: "AI 안전과 책임 있는 AI — 우려 알아보기, 책임 나누기, 확인하기"
course: openai-foundational-knowledge
lesson: 3
---

## 학습 목표

- AI 안전과 책임 있는 AI가 **신뢰와 도입**에 왜 중요한지 설명하기
- 고객이 꺼낼 수 있는 **흔한 안전 우려 5가지** 알아보기
- OpenAI의 안전 접근법과 **공개 참고 자료**를 큰 틀에서 설명하기
- **OpenAI가 돕는 부분**과 **파트너·고객이 책임질 부분** 구분하기
- 안전 주제를 말할 때 **과장 없는 표현** 쓰기
- 안전 질문을 다루는 **간단한 3단계 프레임워크** 적용하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>왜 중요한가</strong>안전 = 비즈니스 신뢰 문제</li>
  <li><strong>우려 알아보기</strong>편향 · 정확성 · 프라이버시 · 오용 · 감독</li>
  <li><strong>OpenAI의 접근과 자료</strong>어떤 질문엔 어떤 출처</li>
  <li><strong>공동 책임</strong>OpenAI vs 파트너·고객</li>
  <li><strong>말하기</strong>과장 → 더 강한 표현</li>
</ol>

책임 있는 AI는 고객 가치와 **따로 떨어진 게 아니에요.** 유망한 유스케이스라도 출력이 불완전하거나, 민감한 데이터가 섞이거나, 사람의 판단이 꼭 필요한 결정일 수 있어요. 이런 고려 사항은 **걸림돌이 아니라**, 고객이 자신 있게 더 분명한 선택을 하도록 돕는 장치예요.

<div class="oa-eli">
  <span class="oa-eli__head">🚲 쉽게 말하면</span>
  <p>자전거를 산다고 해 볼게요. "이 자전거 빠르고 멋져요!"만 듣고 사면 불안하죠. 브레이크는 잘 듣는지, 헬멧은 누가 챙기는지, 어느 길에서 타면 안 되는지까지 알아야 <b>마음 놓고 자주</b> 탈 수 있어요. 안전 이야기는 자전거를 못 타게 막는 게 아니라, <b>더 많이 타게</b> 해 주는 거예요.</p>
</div>

파트너가 AI 안전·법률·아키텍처 **전문가일 필요는 없어요.** 고객이 우려를 **일찍 꺼내도록** 돕고, 언제 최신 자료·고객 요구사항·전문가로 **확인해야 하는지** 알면 됩니다.

## 1. 안전과 책임 있는 AI가 중요한 이유

### 안전은 책임 있는 AI의 한 부분

| 개념 | 의미 |
|---|---|
| **책임 있는 AI** | AI를 **유용하고 적절하게** 적용하는 것. 기술뿐 아니라 그 주변의 사람·프로세스·정책·거버넌스까지 포함 |
| **안전 (Safety)** | 그 일부. **위험을 줄이고, 한계를 이해하고, 경계를 정하고, 시스템을 계속 개선**하는 것 |

책임은 AI 서비스 자체에만 있지 않고, **사용 맥락 전체**에 흐릅니다: 무엇에 쓰는지, 어떤 정보에 기대는지, 워크플로의 어디에 있는지, 사람이 출력을 어떻게 검토·통제하는지.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 잘못된 생각</span>
    <ul>
      <li>안전은 켜고 끄는 <b>스위치 하나</b>다</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 올바른 생각</span>
    <ul>
      <li>안전은 도입 전 과정에 걸친 <b>계속되는 훈련(discipline)</b>이다</li>
      <li>ChatGPT 업무 활용, API 고객 경험, Codex 소프트웨어 개발, 도구를 넘나드는 에이전트마다 안전 질문의 모양이 다르다</li>
    </ul>
  </div>
</div>

### 고객이 안전을 신경 쓰는 이유 — 결국 비즈니스 문제

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎧</span><span class="oa-card__title">고객 지원 리더</span>상담원이 고객에게 <b>틀린 답</b>을 전할까 봐</div>
  <div class="oa-card"><span class="oa-card__icon">⚖️</span><span class="oa-card__title">법무팀</span>직원이 AI 요약에 <b>지나치게 의존</b>할까 봐</div>
  <div class="oa-card"><span class="oa-card__icon">🖥️</span><span class="oa-card__title">IT 리더</span><b>민감 정보</b>가 엉뚱한 곳에 쓰일까 봐</div>
  <div class="oa-card"><span class="oa-card__icon">👔</span><span class="oa-card__title">경영진</span>직원들이 시스템의 <b>한계</b>를 모를까 봐</div>
</div>

### 파트너가 할 일 vs 하지 말 일

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 할 수 있어야 하는 것</span>
    <ul>
      <li>흔한 안전 우려 알아보기</li>
      <li>책임 있는 AI 원칙을 큰 틀에서 설명</li>
      <li>정확하고 적절히 조건을 단 표현</li>
      <li>확인이 필요한 세부사항 짚기</li>
      <li>더 깊은 전문성이 필요한 순간 알아채기</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 하면 안 되는 것</span>
    <ul>
      <li>모든 맥락에서 안전하다고 보장</li>
      <li>컴플라이언스·공정성·정확성·성과 보장</li>
      <li>법률·규제 자문</li>
      <li>위험이 "제거됐다"고 주장</li>
      <li>제품·요금제·정책·설정에 달린 걸 <b>기억에 의존해</b> 약속</li>
    </ul>
  </div>
</div>

고객의 자신감은 **절대적 주장**이 아니라 **분명한 책임 분담, 적절한 안전장치, 최신 자료, 현실적인 기대**에서 나와야 해요.

### 안전 대화 3단계 프레임워크 ⭐

<ol class="oa-flow">
  <li><strong>① Recognize (알아보기)</strong>고객이 말하는 진짜 우려가 뭔지 이름 붙이기</li>
  <li><strong>② Separate responsibilities (책임 나누기)</strong>OpenAI가 돕는 것 vs 파트너·고객 몫</li>
  <li><strong>③ Verify (확인하기)</strong>약속 전에 확인할 세부사항 짚기</li>
</ol>

<div class="oa-eli">
  <span class="oa-eli__head">🩺 쉽게 말하면</span>
  <p>병원에서 "배가 아파요"라고 하면 의사는 먼저 <b>어떤 종류의 통증인지</b> 알아보고(알아보기), "약은 제가 처방하지만 식단 관리는 환자분이 하셔야 해요"라고 <b>역할을 나누고</b>(책임 나누기), "정확한 건 <b>검사 결과</b>를 보고 말씀드릴게요"라고 해요(확인하기). 안전 질문도 똑같이 다루면 돼요.</p>
</div>

고객은 공식 안전 용어를 안 써요. 질문 속 우려를 **번역**하는 게 첫 단계예요.

| 고객의 말 | 숨은 우려 |
|---|---|
| "답을 믿어도 되나요?" | 정확성 |
| "특정 사용자에게 불리할 수 있나요?" | 편향·공정성 |
| "우리 데이터는 어떻게 되나요?" | 프라이버시 |
| "출시 전에 누가 승인하나요?" | 거버넌스·감독 |

> 예시 답변(의역): "정확성은 중요한 고려 사항이에요. 특히 직원이 AI 답변을 고객 대화에 쓴다면요. OpenAI는 안전 작업과 제품 가이드를 제공하지만, 워크플로의 어디에 사람 검토를 둘지, 어느 수준의 신뢰가 필요한지는 고객이 정해야 해요. 약속 전에 최신 제품 정보를 확인하고 기술·거버넌스 담당자를 참여시키죠."

<div class="oa-eli">
  <span class="oa-eli__head">✍️ 연습 — "AI 비서가 직원에게 절대 틀린 정보를 안 준다고 보장할 수 있나요?"</span>
  <p>모범 방향(의역): "핵심 우려는 <b>정확성</b>이에요(알아보기). OpenAI는 신뢰성과 안전을 개선하려 노력하지만, 어떤 AI도 모든 맥락에서 오류가 없다고 할 순 없어요(책임 나누기). 이 유스케이스라면 비서가 어떤 정보에 기댈지, 사람 검토를 어디에 둘지 확인하고 싶어요(확인하기). 그래야 위험을 줄이고 사용자 기대를 분명히 할 수 있어요."</p>
</div>

## 2. 흔한 AI 안전 우려 다섯 가지

우려마다 **다음 행동이 달라요.** 먼저 종류를 정확히 알아봐야 대화가 초점을 찾습니다.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">⚖️</span><span class="oa-card__title">편향·공정성</span>사람·집단·언어·역할에 따라 부당하게 다르게 대할까?</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🎯</span><span class="oa-card__title">정확성·환각</span>그럴듯하지만 틀리거나 근거 없는 답을 낼까?</div>
  <div class="oa-card"><span class="oa-card__icon">🔒</span><span class="oa-card__title">프라이버시·민감 정보</span>무엇에 접근하고, 어떻게 다뤄지고, 학습에 쓰일까?</div>
  <div class="oa-card"><span class="oa-card__icon">🎭</span><span class="oa-card__title">오용·조작 콘텐츠</span>해롭거나 기만적이거나 허가 없는 방식으로 쓰일까?</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🧑‍⚖️</span><span class="oa-card__title">사람의 감독·책임</span>어디서 사람의 판단이 필요하고, 누가 책임질까?</div>
</div>

### ① 편향과 공정성

**어떤 AI도 "완전히 편향이 없다"고 말하면 안 돼요.** 공정성은 유스케이스·사용자 집단·데이터·워크플로·지원하는 결정에 따라 달라요. 일반 글쓰기 도우미와, 직원이나 고객에게 영향을 주는 결정에 쓰이는 시스템은 고려 수준이 다르죠. **결정이 민감하고 사람에게 직접 영향을 줄수록** 사람 검토·평가·거버넌스를 더 신중히 봐야 해요.

### ② 정확성과 환각(hallucination)

<div class="oa-eli">
  <span class="oa-eli__head">🗣️ 환각이 뭐예요?</span>
  <p>AI가 <b>지어내거나, 틀리거나, 근거 없는</b> 답을 내는 걸 말해요. 자신 있게 말하든 아니든 상관없어요. 시험 문제를 모르는데 빈칸으로 두기 싫어서 그럴듯한 답을 <b>지어내 쓰는</b> 친구를 떠올리면 돼요. 글씨는 반듯하고 문장도 매끄럽지만 틀린 답이죠.</p>
</div>

위험은 **평가하고 줄일 수 있지만, 완전히 없앨 수는 없어요.** 정확성 위험은 맥락에 따라 크게 달라요.

<div class="oa-bars">
  <div class="oa-bar"><span>승인된 사내 정책 문서 요약 + 사람 승인 후 발송</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:25%"></span><span class="oa-bar__val">상대적으로 낮음</span></span></div>
  <div class="oa-bar"><span>승인된 문서 기반이지만 자동 발송</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:55%"></span><span class="oa-bar__val">중간</span></span></div>
  <div class="oa-bar"><span>정해진 출처 없는 열린 질문 + 자동 발송</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:85%"></span><span class="oa-bar__val">상대적으로 높음</span></span></div>
  <p class="oa-bars__caption">막대 길이는 "출처가 정해져 있는가 / 사람이 승인하는가"에 따른 위험 차이를 보여 주는 개념적 표현이에요 (강의 수치 아님).</p>
</div>

중요: **민감한 영역의 고위험 결정**에 대해 OpenAI의 현재 **이용 정책(Usage Policies)은 자격 있는 사람의 검토를 요구하고, 그것 없는 자동화를 금지**해요 (강의 기준).

### ③ 프라이버시와 민감 정보

고객은 아주 실용적으로 물어요: "우리 데이터로 모델을 학습하나요?", "누가 대화·출력에 접근하나요?", "민감 정보 처리할 수 있나요?", "우리 제품·요금제·지역에선 어떻게 되나요?"

- **최신 승인 자료**로 답하고, 법적·계약적 약속은 하지 않아요.
- 큰 틀에서: 해당되는 **비즈니스 제품의 비즈니스 데이터는 기본적으로 모델 학습에 쓰이지 않아요.** 단, 고객이 명시적으로 동의(opt-in)한 경우는 예외 (강의 기준).
- 그래도 계약·컴플라이언스 발언 전에는 **최신 OpenAI 프라이버시 자료 + 고객의 계약·제품·요금제·설정·지역·요구사항**으로 확인해야 해요.

### ④ 오용과 조작된 콘텐츠

오해를 부르는 콘텐츠 생성, 사칭, 부적절한 출력, 승인 프로세스 밖에서의 사용 등. OpenAI는 정책·안전장치·평가·제품 보호로 대응하지만, 오용 위험은 **사용자·권한·워크플로 설계·소통 방식·거버넌스**에 달려 있어요. 사내 브레인스토밍 도우미와, 공개 마케팅 문구·고객 응대·이미지·"사람이 쓴 것처럼 보이는" 메시지를 만드는 도구는 위험이 달라요. 고객은 **허용 사용 범위·사용자 권한·검토 단계·에스컬레이션 경로**를 정해야 해요.

### ⑤ 사람의 감독과 책임

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍✈️ 쉽게 말하면</span>
  <p>비행기에는 자동 조종 장치가 있지만 <b>기장</b>이 항상 있어요. 자동 조종은 일을 크게 덜어 주지만, 이륙·착륙 같은 중요한 순간과 문제가 생겼을 때의 최종 책임은 기장에게 있죠. AI도 <b>사람의 판단을 돕는 것</b>이지, 최종 결정의 책임을 가져가지 않아요.</p>
</div>

조직은 자기 환경에서 AI를 어떻게 쓰는지에 **계속 책임을 져요.** 사람·고객·재무·법적 의무·안전·의료·고용·규제 프로세스에 영향을 주는 일일수록 특히 중요해요.

### 예시 — 고객 지원팀의 초안 작성 도우미

처음엔 **속도**(도움말 검색·반복 답변 시간 줄이기)가 관심사였지만, 대화가 이어지며 "상담원이 모르고 틀린 AI 정보를 고객에게 전하면?"으로 **정확성과 고객 신뢰**로 옮겨 갔어요.

<ol class="oa-flow">
  <li><strong>AI가 초안 작성</strong>승인된 도움말 문서만 사용</li>
  <li><strong>근거 표시</strong>초안마다 뒷받침하는 문서 명시</li>
  <li><strong>상담원 확인</strong>원문과 대조 후 발송 여부 결정</li>
  <li><strong>예외는 리드에게</strong>근거 없거나 충돌하면 지원 리드로</li>
</ol>

이 **인계 규칙에 먼저 합의한 뒤** 고객 대상 사용을 테스트해요. 교훈: 정확성·환각 우려는 기술 문제만이 아니라 **신뢰와 비즈니스 위험** 문제예요.

## 3. OpenAI의 안전 접근법과 공개 참고 자료

안전 질문엔 **머릿속 기억이 아니라 출처에 맞는 최신 답**이 필요해요. 어떤 자료가 맞는지는 고객이 탐색 중인 경로(ChatGPT, API, Codex, 에이전트, 오픈 모델, 하이브리드)에 따라 달라요.

### OpenAI의 안전 접근 — "이미 해결"이 아니라 "계속하는 일"

<div class="oa-svg">
<svg viewBox="0 0 720 200" role="img" aria-label="OpenAI 안전 작업의 네 단계 순환: 가르치기, 테스트하기, 공유하기, 배우기">
  <defs>
    <marker id="ar3" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.a3{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.b3{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.t3{font-size:13px;fill:#4a4a4a;font-weight:700}.s3{font-size:11px;fill:#7a7a7a}</style>
  </defs>
  <rect x="10" y="40" width="160" height="80" rx="12" class="a3"/><text x="90" y="70" text-anchor="middle" class="t3">🎓 가르치기</text><text x="90" y="90" text-anchor="middle" class="s3">모델·시스템이 더</text><text x="90" y="105" text-anchor="middle" class="s3">안전하게 행동하도록</text>
  <rect x="190" y="40" width="160" height="80" rx="12" class="a3"/><text x="270" y="70" text-anchor="middle" class="t3">🧪 테스트하기</text><text x="270" y="90" text-anchor="middle" class="s3">평가·전문가 의견·</text><text x="270" y="105" text-anchor="middle" class="s3">실제 시나리오</text>
  <rect x="370" y="40" width="160" height="80" rx="12" class="a3"/><text x="450" y="70" text-anchor="middle" class="t3">📢 공유하기</text><text x="450" y="90" text-anchor="middle" class="s3">안전 자료·시스템 카드·</text><text x="450" y="105" text-anchor="middle" class="s3">배포 안전 업데이트</text>
  <rect x="550" y="40" width="160" height="80" rx="12" class="b3"/><text x="630" y="70" text-anchor="middle" class="t3">🔁 배우기</text><text x="630" y="90" text-anchor="middle" class="s3">실제 사용과</text><text x="630" y="105" text-anchor="middle" class="s3">피드백에서</text>
  <line x1="170" y1="80" x2="186" y2="80" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar3)"/>
  <line x1="350" y1="80" x2="366" y2="80" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar3)"/>
  <line x1="530" y1="80" x2="546" y2="80" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar3)"/>
  <path d="M630,120 Q630,175 360,175 Q90,175 90,124" fill="none" stroke="#c99400" stroke-width="1.5" stroke-dasharray="5 4" marker-end="url(#ar3)"/>
  <text x="360" y="168" text-anchor="middle" class="s3">계속 개선 — 한 번에 끝나는 성취가 아님</text>
</svg>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 피할 말</span>
    <ul>
      <li>"OpenAI는 AI 안전을 이미 해결했어요."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 더 강한 말</span>
    <ul>
      <li>"OpenAI는 안전 연구·테스트·안전장치·공개 자료에 투자해요. 그래도 고객 배포에선 유스케이스·워크플로·데이터·사용자·거버넌스를 따로 봐야 해요."</li>
    </ul>
  </div>
</div>

### 질문별로 어떤 자료를 가리킬까 (강의 기준)

| 고객 질문 | 가리킬 자료 | 무엇이 들어 있나 |
|---|---|---|
| "OpenAI는 안전을 어떻게 생각하나요?" | **OpenAI Safety Resources** | 안전 접근법·중점 분야·관련 업데이트 (큰 그림) |
| "이 용도가 허용되나요? 사람 검토가 필요한가요?" | **Usage Policies** | 허용·금지 용도, 고위험 결정의 사람 검토 요건 |
| "이 특정 모델의 능력·한계·평가는?" | **Deployment Safety Hub / System cards** | 배포된 시스템별 평가 방식·고려한 위험·개선 내용 |
| "고급 AI의 심각한 위험은 어떻게 관리하나요?" | **Preparedness Framework** | 프런티어 위험 거버넌스 접근법 (고객별 체크리스트 아님) |
| "우리 데이터 소유권은? 학습에 쓰이나요?" | **Enterprise Privacy / Business Data** | 비즈니스 제품·API Platform의 데이터 처리와 프라이버시 약속 |

<div class="oa-eli">
  <span class="oa-eli__head">💊 쉽게 말하면</span>
  <p>약 상자 안의 <b>설명서</b>를 생각해 보세요. 회사 전체의 안전 철학은 "회사 소개", 이 약을 어떻게 쓰면 안 되는지는 "사용상 주의", <b>이 약 하나</b>의 성분·부작용·임상 결과는 "제품 설명서"(= 시스템 카드)예요. 질문에 맞는 쪽을 펴서 보여 줘야 해요.</p>
</div>

### 근거를 쓸 때 주의 — 예시 두 가지 (강의 기준)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📇</span><span class="oa-card__title">시스템 카드 예시</span>GPT-6 Astra의 안전 개요는 <b>시뮬레이션된 내부 Codex 작업</b>에서의 정렬(alignment) 테스트를 설명해요. 이는 논의의 근거가 되지만, 고객의 실제 워크플로가 <b>오류 없거나 규정을 준수한다는 증거는 아니에요.</b> 쓰기 전에 과제·테스트 조건·남은 한계를 확인하세요.</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚨</span><span class="oa-card__title">Preparedness 예시</span>OpenAI는 GPT-6 Astra를 Preparedness Framework의 <b>Critical 사이버 보안 역량 기준</b>에 해당한다고 평가했어요. 이는 <b>더 강한 안전장치가 필요하다는 위험 분류</b>이지, 모든 고객이 모든 기능을 쓸 수 있다는 뜻이 아니에요.</div>
</div>

### 자료를 잘 쓰는 법

- 기억 말고 **현재 출처**를 가리키기
- 자료는 신뢰를 **뒷받침**하는 용도. 고객 검토를 **건너뛰는** 지름길이 아님
- 큰 틀의 공개 안전 정보 ≠ 제품별·요금제별·계약상 세부사항
- 깊은 질문엔 OpenAI·법무·보안·기술·배포 전문가를 참여

> ⛔ "제가 정책을 기억하니까 확인해 드릴 수 있어요."
> ✅ "큰 틀의 입장은 설명드릴 수 있지만, 정확한 답은 제품·요금제·설정·요구사항에 달려 있어요. 최신 OpenAI 자료로 확인하고 필요하면 전문가를 모시죠."

## 4. 공동 책임 — 누가 무엇을 책임지나

<div class="oa-eli">
  <span class="oa-eli__head">🏊 쉽게 말하면</span>
  <p>수영장을 생각해 보세요. 수영장 운영자는 <b>깨끗한 물, 안전 요원, 수심 표시</b>를 제공해요(OpenAI). 강사는 수업 계획을 짜고 아이 수준에 맞는 레인을 골라 줘요(파트너). 하지만 아이가 어느 레인에서 놀지, 보호자가 지켜볼지는 <b>가족이 정해요</b>(고객). 수영장이 아무리 안전해도 보호자 역할이 사라지진 않죠.</p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 230" role="img" aria-label="OpenAI, 파트너, 고객의 공동 책임 영역">
  <defs>
    <style>.o4{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.p4{fill:#f6f3ea;stroke:#e5dfcf;stroke-width:1.5}.c4{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.t4{font-size:13px;fill:#4a4a4a;font-weight:700}.s4{font-size:11px;fill:#6a6a6a}</style>
  </defs>
  <rect x="10" y="10" width="225" height="210" rx="12" class="o4"/>
  <text x="122" y="36" text-anchor="middle" class="t4">🏢 OpenAI가 돕는 것</text>
  <text x="24" y="66" class="s4">• 모델·플랫폼 안전 작업</text><text x="34" y="82" class="s4">(연구·평가·완화·안전장치)</text>
  <text x="24" y="106" class="s4">• 공개 안전 자료·시스템 카드</text>
  <text x="24" y="130" class="s4">• 제품 수준 기능·통제·가이드</text><text x="34" y="146" class="s4">(제품·요금제에 따라)</text>
  <text x="24" y="170" class="s4">• 비즈니스 데이터·프라이버시</text><text x="34" y="186" class="s4">약속 설명 자료</text>
  <rect x="248" y="10" width="225" height="210" rx="12" class="p4"/>
  <text x="360" y="36" text-anchor="middle" class="t4">🤝 파트너가 돕는 것</text>
  <text x="262" y="66" class="s4">• 유스케이스·워크플로 위험 명확화</text>
  <text x="262" y="90" class="s4">• 올바른 질문 던지기</text>
  <text x="262" y="114" class="s4">• 워크플로 설계 함께하기</text>
  <text x="262" y="138" class="s4">• 위험 짚기</text>
  <text x="262" y="162" class="s4">• 전문가가 필요한 순간 알기</text>
  <rect x="486" y="10" width="224" height="210" rx="12" class="c4"/>
  <text x="598" y="36" text-anchor="middle" class="t4">🏛️ 고객이 책임지는 것</text>
  <text x="500" y="66" class="s4">• 이 작업에 AI가 적절한가</text>
  <text x="500" y="90" class="s4">• 내부 정책·법무·컴플라이언스 정렬</text>
  <text x="500" y="114" class="s4">• 검토·이슈 보고·개선 체계</text>
  <text x="500" y="138" class="s4">• AI 활동 파악·정책 준수 확인</text>
  <text x="500" y="162" class="s4">• 사용자에게 한계·확인 시점 안내</text>
  <text x="500" y="196" class="s4">→ 조직 내 사용의 최종 책임</text>
</svg>
</div>

경계선이 핵심이에요: OpenAI의 안전 작업은 위험을 **줄이고 관리하는 데 도움**을 주지만, 모든 고객 유스케이스를 **자동으로** 적절하고·준수하고·정확하고·완전히 통제된 상태로 **만들어 주지 않아요.**

### 책임 있는 도입의 원칙과 세 가지 운영 질문

원칙: 적절한 사람의 감독 · 분명한 거버넌스와 책임 · 지속적인 검토와 개선 · 한계와 위험에 대한 인식.

| 질문 | 묻는 것 | 예 |
|---|---|---|
| **Context — AI는 무엇을 아나?** | 접근 가능한 정보·정책·시스템·권한·비즈니스 맥락 | 정책 도우미 → 승인된 정책 문서 / 지원 도우미 → 최신 도움말 |
| **Runtime — 어디서 어떻게 일하나?** | 동작 위치와 허용된 행동 | 사람 검토용 초안만? 문서 검색? 도구 호출? 통제된 공간 안에서만? |
| **Observability — 어떻게 모니터링·개선하나?** | 활동 감시, 결과 검토, 이슈 발견, 개선 | 누가 사용 패턴을 보나? 오류는 어떻게 보고? 예상 밖 출력엔? |

### 예시 — 멈춰 버린 지원 기록 요약 작업

<div class="oa-eli">
  <span class="oa-eli__head">🛑 상황</span>
  <p>지원 매니저가 API 앱에 "승인된 지원 기록을 <b>수정하지 말고 요약만</b> 해 줘"라고 시켰어요. 그런데 모니터링이 "기록 변경 가능성" 플래그를 띄우고 대화를 멈췄어요. 요약은 미완성이고, 기록이 실제로 바뀌었는지는 아직 아무도 확인 안 했어요.</p>
</div>

<ol class="oa-flow">
  <li><strong>책임자는 매니저</strong>검토 주체가 분명함</li>
  <li><strong>행동 기록 대조</strong>앱이 한 행동 vs "요약만" 지시</li>
  <li><strong>기록 직접 점검</strong>이미 생긴 변경이 있는지</li>
  <li><strong>섣불리 재시도 ✗</strong>멈췄다고 이전 행동이 되돌려지진 않음</li>
</ol>

포인트: **플래그는 조사의 계기이지 무단 변경의 증거가 아니에요.** 모니터링은 추가 안전장치일 뿐 보증이 아니라서, 정상 작업을 잡아낼 수도, 문제를 놓칠 수도 있어요. 범위와 멈춤 동작은 인터페이스와 설정에 달려 있고, 이런 상황에서 API에는 **일반적인 재개(resume) 기능이 없어요** (강의 기준).

### 예시 — 의료 기관의 정책 검색 도우미

고위 관계자의 질문: **"직원이 틀린 AI 안내를 따르면 누가 책임지나요?"**

> 좋은 답(의역): "중요한 책임 질문이에요. OpenAI는 안전 작업과 공개 자료를 제공하지만, 시스템을 어떻게 쓸지, 어떤 정책 출처에 기댈지, 어디서 사람이 확인할지, 이슈를 어떻게 모니터링·에스컬레이션할지는 기관이 정해야 해요. 운영 프로세스에 넣기 전에 거버넌스·법무·보안·워크플로 담당자를 참여시키죠."

교훈: 고객은 AI의 **능력만** 평가하는 게 아니라, AI가 업무에 들어왔을 때 **책임이 어떻게 관리될지**를 평가하고 있어요.

## 5. 안전에 대해 효과적으로 말하기

### 피해야 할 과장 5가지

| 과장 유형 | ⛔ 이렇게 말하지 말고 | ✅ 더 강한 표현 (요약) |
|---|---|---|
| **안전 보장** | "이 제품은 안전해요." | 안전 작업·평가·안전장치·가이드가 위험을 줄이고 관리하지만, 모든 맥락의 안전을 보장하진 않음. 워크플로·사용자·데이터·사람 검토·거버넌스도 함께 봐야 함 |
| **편향 보장** | "AI는 편향이 없어요." | 편향·공정성은 맥락 속에서 평가해야 함. 완전히 편향 없는 AI는 없음 |
| **정확성 보장** | "환각 안 해요." | 지어내거나 틀린 출력은 생길 수 있음. 좋은 맥락·출처 근거·테스트·워크플로 설계·사람 검토로 줄이고 관리. 민감 영역 고위험 결정은 이용 정책상 사람 검토 필수 |
| **컴플라이언스 보장** | "OpenAI가 규정 준수를 자동으로 충족해요." | OpenAI는 보안·프라이버시 검토를 돕는 자료를 제공. 무엇이 적용되는지는 고객의 법무·보안·구매·컴플라이언스 팀이 확인 |
| **성과 보장** | "책임 있는 AI면 자동으로 위험이 줄고 가치가 생겨요." | 신뢰·도입·의사결정 품질·지속 가치에 도움이 되지만, 결과는 유스케이스·설계·거버넌스·도입에 달려 있음 |

### 더 강한 표현을 만드는 5가지 언어 습관

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📉</span><span class="oa-card__title">감소 언어</span>줄이다·관리하다·평가하다·완화하다·모니터링하다 — <b>"제거"는 금지</b></div>
  <div class="oa-card"><span class="oa-card__icon">🧩</span><span class="oa-card__title">맥락 언어</span>"적절한 감독 수준은 출력이 어떻게 쓰이고, 틀리면 무슨 일이 생기는지에 달려 있어요."</div>
  <div class="oa-card"><span class="oa-card__icon">🤝</span><span class="oa-card__title">공동 책임 언어</span>"OpenAI는 안전 작업과 자료를 제공하고, 워크플로·거버넌스·사용자 기대는 고객이 정해요."</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔍</span><span class="oa-card__title">확인 언어</span>"약속 전에 최신 제품 문서를 확인하고 고객의 보안·법무팀을 참여시키죠."</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">💎</span><span class="oa-card__title">고객 가치 언어</span>"언제 AI에 기대고, 언제 확인하고, 어떻게 이슈를 올릴지 알면 더 자신 있게 도입할 수 있어요."</div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">✍️ 연습 — 과신 문장 고쳐 쓰기</span>
  <p>원문: "네, 이 AI 시스템은 완전히 안전하고 규정 요건도 자동으로 충족해요."</p>
  <p>고친 예(의역): "OpenAI는 책임 있는 사용을 돕는 안전 작업·제품 가이드·공개 자료를 제공해요. 이 유스케이스라면 약속 전에 최신 제품 정보를 확인하고 법무·보안팀 같은 거버넌스 담당자를 참여시키고 싶어요. 그래야 맞는 맥락에서 평가할 수 있어요."</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 과장 없이 고객의 안전 우려에 답한 것은?</summary>
  <p>① "AI 안전은 유스케이스·워크플로·사용자·데이터·감독 모델에 달려 있어요." ② "승인된 제품만 고르면 AI를 믿을 수 있어요." ③ "OpenAI가 안전 작업을 하니 추가 거버넌스는 대개 불필요해요." ④ "파트너가 바로 답하고 추가 검토자는 부르지 말아야 해요."</p>
  <p><b>정답: ①</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 고객 질문과 안전 우려</summary>
  <p>"우리 비즈니스 데이터로 모델을 학습하나요?" → <b>프라이버시·민감 정보</b><br>"고위험 결정에 쓰기 전 누가 출력을 검토하나요?" → <b>사람의 감독·책임</b><br>"AI가 항상 정답을 내나요?" → <b>정확성·환각</b><br>"누가 이걸로 오해를 부르는 콘텐츠를 만들 수 있나요?" → <b>오용·조작 콘텐츠</b><br>"어떤 고객 집단에 더 잘 맞을 수 있나요?" → <b>편향·공정성</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 특정 배포 모델의 능력·한계·평가·안전 고려 사항을 묻는다면 먼저 가리킬 자료는?</summary>
  <p>① 이전 고객 대화의 파트너 메모 ② 고객의 내부 AI 정책 ③ Deployment Safety Hub 또는 해당 시스템 카드 ④ OpenAI의 일반 Safety 자료</p>
  <p><b>정답: ③</b> — 일반 Safety 자료는 큰 그림용이고, 특정 모델 정보는 시스템 카드에 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 짝 맞추기 — 책임과 주체</summary>
  <p>영업 대화 중 유스케이스·워크플로 위험·필요한 전문성 명확화 → <b>파트너</b><br>검토·피드백·모니터링을 포함해 AI가 워크플로에 들어가는 방식 설계·개선 → <b>파트너와 고객</b><br>내부 정책·법무 검토·컴플라이언스·사용자 소통과 정렬 → <b>고객</b><br>공개 안전 자료·시스템 카드 발행, 제품 수준 통제·가이드 제공 → <b>OpenAI</b></p>
</details>

<details class="oa-quiz">
  <summary>Q5. "이 AI 솔루션이 안전하고, 편향 없고, 규정을 준수한다고 보장해 줄 수 있나요?"에 가장 정확한 답은?</summary>
  <p>① "표준 구현 절차만 따르면 파트너가 안전·준수를 확인해 드려요." ② "OpenAI는 안전 작업과 제품 가이드를 제공하지만, 안전·공정성·컴플라이언스가 고객 유스케이스에 어떻게 적용되는지는 확인해야 해요." ③ "아니요, 그런 질문이 있으면 AI를 쓰면 안 돼요." ④ "네, 승인된 제품을 쓰면 안전·무편향·준수 상태예요."</p>
  <p><b>정답: ②</b> — ③처럼 무조건 막는 것도 정답이 아니에요.</p>
</details>

## 정리

- 책임 있는 AI는 **고객 가치의 일부**다. 신뢰·도입·실제 업무 활용 방식을 좌우한다.
- 안전 질문엔 **Recognize → Separate responsibilities → Verify** 3단계로 답한다.
- 다섯 가지 우려(**편향 · 정확성/환각 · 프라이버시 · 오용 · 사람의 감독**)를 먼저 알아보고 이름 붙인다.
- 질문에 맞는 **공개 자료**(Safety · Usage Policies · 시스템 카드 · Preparedness · Enterprise Privacy)를 가리키되, 고객별 검토를 대체하지 않는다.
- 책임은 **공동**이다. OpenAI는 위험을 줄이는 걸 돕고, 파트너는 질문과 설계를 돕고, **고객이 조직 내 사용의 최종 책임**을 진다.
- "제거·보장·자동" 대신 **감소·맥락·공동 책임·확인·고객 가치** 언어를 쓴다.
