---
title: "From Qualified Opportunity to Deployment Plan"
title_ko: "검증된 영업 기회를 배포 계획의 출발점으로 바꾸기"
course: chatgpt-deployment
lesson: 1
---

## 학습 목표

- 영업팀이 넘긴 ChatGPT 기회(handoff)를 보고 **배포에 쓸 만한 증거가 있는지** 점검하기
- "시간 절약해 준대요" 수준의 영업 문장을 **배포용 문제 정의(problem statement)와 성공 기준**으로 바꾸기
- 스폰서·오너·관리자 같은 **이해관계자와 범위(scope)** 를 분명히 하기
- 증거에 맞는 **시작 모션(starting motion)** — 진행 / 집중 검증 / 범위 재조정 / 보류 — 고르기
- 이 모든 것을 한 장짜리 **ChatGPT Deployment Brief** 로 묶기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>핸드오프 점검</strong>확인된 것 · 가정 · 모르는 것 분리</li>
  <li><strong>문제·워크플로·성공 기준</strong>영업 문장 → 배포용 문장</li>
  <li><strong>이해관계자·범위</strong>누가 결정하고, 어디까지 하나</li>
  <li><strong>시작 모션 결정</strong>진행? 검증? 재조정? 보류?</li>
  <li><strong>Deployment Brief</strong>다음 회의를 위한 한 장 요약</li>
</ol>

이 코스를 관통하는 원칙은 딱 하나예요: **"모션을 약속하기 전에 증거부터 확인하라 (Confirm the evidence before you commit the motion)."**

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면</span>
  <p>부동산 중개인이 "이 집 정말 좋아요, 손님도 엄청 마음에 들어 해요!"라고 넘겨줬다고 바로 이삿짐 트럭을 부르진 않죠. 먼저 <b>계약서에 적힌 것</b>(확인된 사실), <b>그럴 것 같은 것</b>(가정), <b>아직 아무도 모르는 것</b>(예: 수도관 상태)을 나눠 보고, 그다음에 "바로 이사 / 한 번 더 집 보기 / 다른 집 찾기 / 보류"를 정해요.</p>
  <p>ChatGPT 배포도 똑같아요. 고객이 관심 있고 직원들이 이미 AI를 써 보고 있다는 건 <b>분위기(momentum)</b>일 뿐, <b>배포 준비가 됐다는 증거</b>는 아니에요.</p>
</div>

## 1. ChatGPT 배포가 해결하려는 것

강의에서 ChatGPT는 직원이 **지식·추론·워크플로 지원**을 통해 AI를 쓰는 "직원용 창구"로 설명돼요. 그래서 배포는 "계정 나눠 주기"가 아니라 **실제 업무 안에서 AI를 쓰게 만드는 일**이에요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔎</span><span class="oa-card__title">지식 찾기·종합</span>흩어진 정보를 찾아 정리</div>
  <div class="oa-card"><span class="oa-card__icon">✍️</span><span class="oa-card__title">초안 작성·개선</span>업무 문서, 브리핑, 요약</div>
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">추론·분석 지원</span>반복되는 지식 업무의 일관성 향상</div>
</div>

배포팀이 할 일은 고객을 **"관심" → "성과"** 로 옮기는 것. 그러려면 출발점에 여섯 가지가 필요해요: **워크플로 · 사용자 그룹 · 가치 가설 · 성공 증거 · 오너 · 알려진 제약.** 이 코스는 전체 롤아웃 설계가 아니라, 그 계획을 가능하게 하는 **브리프(Brief)** 를 만드는 단계예요.

## 2. 영업 핸드오프 점검하기

### 핸드오프에서 찾아야 할 다섯 가지 증거

| 증거 종류 | 핵심 질문 |
|---|---|
| **① 기회 요약** | 어떤 문제를, 왜 지금, 어떤 결과를 위해? 고객이 이미 합의한 다음 단계는? |
| **② 워크플로·사용자 증거** | ChatGPT가 도울 일은? 지금 누가 그 일을 하나? 첫 그룹은 누구? |
| **③ 가치 가설** | 시간·품질·역량·일관성·리스크·경험 중 무엇? 테스트할 만큼 구체적인가? |
| **④ 권장 다음 단계** | 기획 회의? 이해관계자 정렬? 집중 검증? 파일럿 설계? 거버넌스 리뷰? |
| **⑤ 제품 경험·맥락·통제 요구** | 어떤 ChatGPT 경험(collaborative Chat, ChatGPT Work, Workspace Agent — 강의 기준)? Company Knowledge·Apps 같은 연결 소스? 접근 권한·승인·사람 검토 게이트는? |

규제 산업이라면 접근 경로, 데이터 경계, 행동(action) 경계, 검토 요구, **명시적으로 제외된 용도**까지 핸드오프에 담겨 있어야 해요.

### 확인된 증거 vs 합리적 가정 vs 미지수

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">✅</span><span class="oa-card__title">확인된 증거 (confirmed)</span>고객이 말했거나, 합의·문서화·배정한 것. 예: 이름 있는 이해관계자, 합의된 다음 단계, 확인된 비즈니스 오너, 확인된 정책 제약</div>
  <div class="oa-card"><span class="oa-card__icon">🤔</span><span class="oa-card__title">합리적 가정 (assumption)</span>그럴듯하지만 미검증. 예: 첫 사용자 그룹, 필요한 지식 소스, 거버넌스·검토 기대, 롤아웃 모양, 측정 방식</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">❓</span><span class="oa-card__title">미지수 (unknown)</span>빠졌거나 미해결이라 계획을 흔들 수 있는 것. 예: 소스 접근, 지역·규제 요건, 데이터 사용 경계, 워크스페이스 오너 부재</div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🔤 ABC 습관</span>
  <p><b>A</b>ssume nothing, <b>B</b>elieve nothing, <b>C</b>onfirm everything. 영업 담당자를 못 믿어서가 아니라, 고객에게 <b>약속</b>을 하려면 그만큼 단단한 증거가 필요하기 때문이에요. 친구가 "내일 소풍 간대"라고 해도, 준비물을 사기 전엔 선생님 공지를 한 번 확인하잖아요.</p>
</div>

### 핸드오프 품질 체크 — 여섯 가지 질문

| 질문 | 약한 답 | 더 강한 답 |
|---|---|---|
| 어떤 **일**이 바뀌나? | "AI로 시간 절약하고 싶어요" | "CSM들이 승인된 계정 노트·지원 이력·사용량으로 갱신(renewal) 브리프를 더 일관되게 준비" |
| 누가 **먼저** 쓰나? | "전사가 쓸 수도" | "엔터프라이즈 갱신 담당 CSM 약 15명 (비즈니스 오너 확인 대기)" |
| **결과의 오너**는? | "리더십이 관심 있어요" | "CS 부사장(VP)이 갱신 준비 성과를 책임지고 첫 결정 회의에 참석" |
| **성공**은 어떻게 보이나? | "사용자들이 좋아해요" | "기존 방식 대비 브리프 초안의 유용성·일관성·준비 품질을 비교 평가" |
| 출시 전 **검증할 것**은? | (언급 없음) | 소스 접근, 워크스페이스 준비, 데이터 경계, 보안·법무 리뷰, 매니저 지원, 출력 검토 기대 |
| **다음에** 할 일은? | "빨리 시작" | 첫 워크플로 확정, 오너와 관리자 정렬, 승인 소스 확인, 킥오프 또는 집중 검증 |

약한 핸드오프는 대개 **너무 넓거나, 기능 위주거나, 측정 가능한 워크플로와 끊겨** 있어요. 모호하면 이 코스 안에서 영업 discovery를 다시 하지 말고 **"범위 재조정 필요"로 표시**합니다.

## 3. 영업 문장을 배포용 문제 정의로 바꾸기

### 배포용 문제 정의의 다섯 요소

<ol class="oa-flow">
  <li><strong>현재 마찰</strong>무엇이 느리고 들쭉날쭉한가</li>
  <li><strong>영향받는 사람</strong>누가 겪나</li>
  <li><strong>운영 영향</strong>왜 중요한가</li>
  <li><strong>ChatGPT 지원 워크플로</strong>무엇을 돕게 할까</li>
  <li><strong>비즈니스 결과</strong>무엇이 좋아져야 하나</li>
</ol>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 영업 수준 (seller-level)</span>
    <ul>
      <li>"고객은 CS팀 시간 절약을 위해 ChatGPT를 원해요."</li>
      <li>관심 확인용으로는 OK</li>
      <li>워크플로·사용자·산출물·소스·성공 증거·오너·첫 결정이 전부 빠짐</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 배포 수준 (deployment-grade)</span>
    <ul>
      <li>"CSM은 계정 노트·지원 이력·사용 신호가 흩어져 있어 갱신 브리프 준비에 몇 시간씩 쓴다."</li>
      <li>"첫 워크플로는 소수 CSM이 <b>승인된 계정 맥락</b>으로 <b>사람이 검토하는</b> 브리프 초안을 만드는 것."</li>
      <li>"첫 모션 동안 사용자·매니저 피드백과 가치 신호를 모아 진행/개선/재조정을 결정."</li>
    </ul>
  </div>
</div>

### 좋은 첫 워크플로의 조건

| 조건 | 예시 |
|---|---|
| **사용자가 할 행동**으로 적는다 | "승인된 계정 맥락으로 갱신 브리프 초안 작성" (≠ "CS 생산성 향상") |
| 고객 문제와 **연결**된다 | 문제가 갱신 준비 품질이면 워크플로도 갱신 준비 |
| **보이는 오너**가 있다 | CS 운영 리드 = 워크플로 오너, CS VP = 비즈니스 결과 오너 |
| **측정 가능**하다 | 짧은 피드백 폼, 매니저 검토 체크리스트, 샘플 출력 비교 |
| **좁게** 시작할 수 있다 | 엔터프라이즈 담당자의 분기 갱신 브리프만. QBR·협상·임원 보고는 한꺼번에 X |

### 성공 기준 = 관찰 가능한 증거

성공 기준의 목적은 첫 모션 후 **다음 결정(진행·개선·재조정·중단·확장·보류)** 을 돕는 것. 이 단계에선 **최종 ROI 약속이 아니라 계획용 목표치**예요.

| 증거 범주 | 묻는 것 | 계획 기준(예) | 증거 출처 | 오너 |
|---|---|---|---|---|
| **채택·참여** | 의도한 사용자가 실제 업무 순간에 써 봤나? | 첫 코호트 대부분이 검증 기간 중 시도 | 워크스페이스 사용 신호, 체크인, 매니저 확인 | 배포 리드 + 고객 워크플로 오너 |
| **사용자·매니저 피드백** | 계속 다듬을 만큼 유용한가? | 유용성 여부를 설명할 수 있음 | 짧은 설문, 인터뷰, 오피스아워 노트 | 고객 워크플로 오너 |
| **워크플로 가치 신호** | 시간·품질·일관성·리스크가 나아지나? | 브리프가 더 일관되고 검토가 쉬워 보임 | 샘플 출력 리뷰, 품질 체크리스트 | 비즈니스 결과 오너 |

<div class="oa-eli">
  <span class="oa-eli__head">📏 기준선(baseline)부터 재요</span>
  <p>다이어트 효과를 알려면 시작 전 몸무게를 재야 하죠. 마찬가지로 대표 브리프 몇 개를 골라 <b>지금 준비·검토에 드는 시간, 필요한 정보와 출처</b>를 먼저 기록해요. 그리고 "쓸 만한 초안"의 조건(필수 섹션 완성, 사실이 승인 소스와 일치, 모르는 건 지어내지 말고 표시)을 <b>미리</b> 합의한 뒤, 같은 기준으로 AI 지원 방식과 비교합니다.</p>
</div>

### 솔루션 언어는 현실적으로

| 피해야 할 표현 | 더 나은 표현 |
|---|---|
| 너무 넓음: "ChatGPT가 CS 생산성을 혁신할 것" | "첫 CSM 코호트가 승인된 맥락으로 사람이 검토하는 갱신 브리프 초안을 준비하도록 지원" |
| 기능 나열: "Work, Apps, Company Knowledge, Workspace Agents, 분석으로 갱신 운영 강화" | "어떤 계정 맥락이 승인됐는지, 사용자가 어떻게 준비하고 매니저가 무엇을 검토할지 확인" |
| 과한 확신: "준비 시간 50% 감소" | "준비 마찰 감소와 일관성 향상 여부에 대한 증거를 수집" |

검증되지 않은 소스 접근 주장, 고객이 소유한 결정을 **완전 자동화**한다는 약속, 검토 없이 고객·시스템에 영향을 준다는 암시도 금지예요.

## 4. 이해관계자·범위·거버넌스 확인

### 스폰서가 "지지한다"만으로는 부족해요

브리프에는 **누가 결정하는지 · 어떤 결정인지 · 언제 하는지 · 어떤 증거로 하는지** 가 있어야 해요. 지지하지만 중요한 결정 회의(킥오프, 파일럿 리뷰, 첫 모션 결과 보고, 거버넌스 승인 등)에 나타나지 않는 스폰서는 계획을 전진시키기 어려워요.

| 역할 | 책임지는 것 |
|---|---|
| **Executive sponsor** | 긴급성, 우선순위, 조직 정렬 |
| **Business outcome owner** | 워크플로 결과·운영 지표 |
| **Workflow owner** | 일상 업무와 사용자 필요를 앎 |
| **Workspace owner / admin** | 워크스페이스 설정, 접근 결정, 셋업 의존성 |
| **IT·보안·법무·컴플라이언스·데이터 오너** | 소스 접근, 정책, 리스크, 지역 요건 검증 |
| **Partner deployment lead** | 배포 계획과 고객 대면 다음 단계 조율 |

모든 역할이 같은 깊이로 필요하진 않아요. 위험이 낮고 좁은 워크플로면 거버넌스 쪽은 가볍게, 규제·소스 연결·정책 민감 워크플로면 깊게. 중요한 건 **가시성**: 오너가 비어 있으면 숨기지 말고 브리프에 **준비도 갭**으로 적어요. 개인 세션을 넘어 팀이 계속 돌리는 작업이라면, 누가 프로세스를 바꾸고 예외를 검토하고 실행을 멈추고 연결을 유지하는지도 정해 둡니다.

### 범위 경계 (scope boundaries)

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 첫 모션에 포함 (in scope)</span>
    <ul>
      <li>워크플로: 엔터프라이즈 CSM의 갱신 브리프 준비</li>
      <li>사용자: 첫 코호트 10–15명</li>
      <li>출시 전 필수 의존성: 워크스페이스 오너 확인, 승인 소스 목록, 코호트 확정, 매니저 검토 체크리스트</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 제외 / 나중으로 미룸</span>
    <ul>
      <li>제외: 지원 운영, 영업 예측, 글로벌 계정 계획</li>
      <li>제외 데이터·행동: 고객 PHI, 미승인 소스, 자동 고객 커뮤니케이션</li>
      <li>보류(parked): 자동 갱신 리스크 점수, CRM write-back, 글로벌 롤아웃</li>
    </ul>
  </div>
</div>

**소스 접근과 출력 접근은 별개**예요. "누가 이 자료를 읽어도 되나"와 "만들어진 브리프를 누가 봐도 되나"를 따로 확인해요.

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🧑‍💼</span><span class="oa-card__title">오너십 갭</span>워크스페이스 관리자 없음, 실제 사용자 미상, 첫 사용 후 검토·개선 담당 없음</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⚖️</span><span class="oa-card__title">결정 갭</span>임원 정렬 미확인, 첫 결정 시점 불명확, 다음 단계 승인자 모름</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">📐</span><span class="oa-card__title">범위 갭</span>사용자 그룹·기능·유스케이스 과다, 포함/제외 미구분, 나중 과제 미보류</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔐</span><span class="oa-card__title">거버넌스·소스 갭</span>데이터 오너 미참여, 지식·앱 접근 미검증, 데이터/추론 레지던시 미해결, 출력 검토 요건 불명확</div>
</div>

갭을 당장 다 해결할 필요는 없지만, **보이게** 만들어야 해요. 좋은 첫 모션은 "배울 수 있을 만큼 좁고, 의미 있을 만큼 믿을 만한" 것.

## 5. 시작 모션 고르기

### 첫 모션 유스케이스 품질 기준

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🛠️</span><span class="oa-card__title">실제 워크플로</span>"브리프 초안 준비" (≠ "생산성 향상")</div>
  <div class="oa-card"><span class="oa-card__icon">🔁</span><span class="oa-card__title">자주 하거나 고통이 큼</span>반복 브리핑, 반복 분석, 문서 검토, 사내 지식 검색</div>
  <div class="oa-card"><span class="oa-card__icon">👀</span><span class="oa-card__title">검증 기간 안에 관찰 가능</span>피드백, 출력 품질, 준비 마찰, 일관성 신호</div>
  <div class="oa-card"><span class="oa-card__icon">👥</span><span class="oa-card__title">첫 코호트에 적합</span>연락 가능하고, 지원받고, 업무와 가까운 사람들</div>
  <div class="oa-card"><span class="oa-card__icon">🧩</span><span class="oa-card__title">의존성이 감당 가능</span>미해결 소스·정책·접근·변화관리 과제가 너무 많지 않음</div>
  <div class="oa-card"><span class="oa-card__icon">📊</span><span class="oa-card__title">확장 결정용 증거 생산</span>진행·개선·재조정·중단을 판단할 근거</div>
</div>

### 롤아웃 가정에 상태 라벨 붙이기

가정마다 **Confirmed(확정) / Likely(유력) / Unresolved(미해결)** 를 표시해요.

| 가정 | 상태 | 이유 |
|---|---|---|
| CSM이 첫 사용자 그룹 | Likely | 영업이 언급했지만 비즈니스 오너 미확인 |
| 갱신 브리프 초안이 첫 워크플로 | Likely | 문제와 맞지만 브리프 형식 미확정 |
| 계정 노트·지원 이력이 승인 소스 | **Unresolved** | 소스 접근·정책 경계 검증 전엔 계획 불가 |
| CS VP가 결과 오너 | **Unresolved** | 지지는 하지만 결과 책임은 미확인 |

### 제약 점검 — 해결하려 들지 말고 "분류"하기

워크스페이스 관리자, 사용자 프로비저닝, Company Knowledge·파일·앱 접근, SSO·기기, Workspace Agent 가용성(강의 기준), PHI 같은 민감 데이터, 법무·보안 리뷰, 출력 검토, 매니저 지원, 모니터링·감사, 연결된 행동의 승인 단계… 이런 제약을 지금 다 풀 필요는 없어요. 각각을 네 칸 중 어디에 넣을지만 정해요.

<div class="oa-bars">
  <div class="oa-bar"><span>이미 해결됨</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:90%"></span><span class="oa-bar__val">그대로 진행</span></span></div>
  <div class="oa-bar"><span>계획 중 관리 가능</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:65%"></span><span class="oa-bar__val">브리프에 기록</span></span></div>
  <div class="oa-bar"><span>집중 검증 필요</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:40%"></span><span class="oa-bar__val">먼저 확인</span></span></div>
  <div class="oa-bar"><span>재조정·보류 사유</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:15%"></span><span class="oa-bar__val">멈추고 다시</span></span></div>
  <p class="oa-bars__caption">막대 길이는 "진행 가능성"을 보여 주는 개념적 표현이에요 (강의 수치 아님).</p>
</div>

### 시작 모션 결정 게이트 — 두 단계로 나눠 결정

<div class="oa-eli">
  <span class="oa-eli__head">🚦 쉽게 말하면</span>
  <p>먼저 "<b>출발해도 되나?</b>"(신호등)를 정하고, 초록불일 때만 "<b>어떤 길로 갈까?</b>"(경로)를 고르는 거예요. 신호도 안 봤는데 고속도로냐 국도냐를 논하면 순서가 뒤집힌 거죠.</p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 300" role="img" aria-label="시작 모션 결정 게이트 2단계">
  <defs>
    <marker id="ar1d" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.q1d{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.g1d{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.n1d{fill:#f6f3ea;stroke:#e5dfcf;stroke-width:1.5}.w1d{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.t1d{font-size:12px;fill:#4a4a4a}.s1d{font-size:11px;fill:#8a8a8a}.h1d{font-size:12px;fill:#c99400;font-weight:bold}</style>
  </defs>
  <text x="10" y="18" class="h1d">STEP 1 · 준비도 결정 (readiness)</text>
  <rect x="10" y="30" width="160" height="56" rx="10" class="g1d"/><text x="90" y="54" text-anchor="middle" class="t1d">✅ Proceed (진행)</text><text x="90" y="72" text-anchor="middle" class="s1d">증거·오너·거버넌스 충분</text>
  <rect x="190" y="30" width="160" height="56" rx="10" class="q1d"/><text x="270" y="54" text-anchor="middle" class="t1d">🔬 Focused validation</text><text x="270" y="72" text-anchor="middle" class="s1d">가정 1–2개만 먼저 검증</text>
  <rect x="370" y="30" width="160" height="56" rx="10" class="n1d"/><text x="450" y="54" text-anchor="middle" class="t1d">✂️ Targeted re-scope</text><text x="450" y="72" text-anchor="middle" class="s1d">너무 넓거나 오너 부족</text>
  <rect x="550" y="30" width="160" height="56" rx="10" class="w1d"/><text x="630" y="54" text-anchor="middle" class="t1d">⛔ No-go / defer</text><text x="630" y="72" text-anchor="middle" class="s1d">필수 게이트 누락</text>
  <line x1="90" y1="86" x2="90" y2="140" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar1d)"/><text x="98" y="118" class="s1d">진행일 때만</text>
  <text x="10" y="160" class="h1d">STEP 2 · 롤아웃 구조 (rollout structure)</text>
  <rect x="10" y="172" width="220" height="70" rx="10" class="g1d"/><text x="120" y="196" text-anchor="middle" class="t1d">🧪 Pilot</text><text x="120" y="214" text-anchor="middle" class="s1d">워크플로는 명확, 확장 전</text><text x="120" y="230" text-anchor="middle" class="s1d">고객이 증명을 원함</text>
  <rect x="250" y="172" width="220" height="70" rx="10" class="g1d"/><text x="360" y="196" text-anchor="middle" class="t1d">🪜 Phased rollout</text><text x="360" y="214" text-anchor="middle" class="s1d">코호트·지역·기능을</text><text x="360" y="230" text-anchor="middle" class="s1d">단계별로 넓힘</text>
  <rect x="490" y="172" width="220" height="70" rx="10" class="g1d"/><text x="600" y="196" text-anchor="middle" class="t1d">🌐 Broader deployment</text><text x="600" y="214" text-anchor="middle" class="s1d">사용자·오너·정책·범위가</text><text x="600" y="230" text-anchor="middle" class="s1d">모두 구체적</text>
  <text x="360" y="280" text-anchor="middle" class="s1d">※ Focused validation은 "준비도 결정"이지 롤아웃 구조가 아니에요</text>
</svg>
</div>

| 모션 | 언제 고르나 |
|---|---|
| **Proceed** | 확인된 증거, 오너십, 거버넌스 준비, 측정 가시성이 충분할 때 |
| **Focused validation** | 유망하지만 첫 사용자 그룹·소스 접근·가치 신호·오너 같은 **특정 가정 1–2개**를 먼저 확인해야 할 때 |
| **Targeted re-scope** | 기회가 너무 넓거나, 오너가 불분명하거나, 측정 가능한 워크플로와 끊겨 있을 때 → 워크플로·코호트·오너·성공기준·소스 범위를 좁힘 |
| **No-go / defer** | 협상 불가한 게이트가 빠졌을 때: 임원 스폰서, 결정 시점, 관리자 경로, ID·접근 준비, 필수 검토, 승인된 소스·앱, 운영 주기, 측정 방식 |

### 사례로 감 잡기

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 추천</span>
    <ul>
      <li>"유스케이스가 명확하니 바로 배포하자." → 소스 접근이 미해결인데 건너뜀</li>
      <li>"모든 계정 계획에 ChatGPT 쓰자." → 첫 모션 유스케이스가 아님</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 추천</span>
    <ul>
      <li>"파일럿 설계 전에 <b>집중 검증</b>: 어떤 계정 노트·지원 이력·사용 신호가 승인됐는지 확인 → 확정되면 첫 코호트로 소규모 파일럿."</li>
      <li>"<b>범위 재조정</b>: 워크플로 1개, 사용자 그룹 1개, 오너 1명, 증거 계획 1개로 좁힌 뒤 모션 선택."</li>
    </ul>
  </div>
</div>

추천문은 **추천 모션 → 근거 증거 → 가정·미지수 → 먼저 풀 제약 → 즉시 다음 단계** 순서로 쓰면 깔끔해요. 예: "집중 검증. 워크플로는 구체적이고 코호트(약 12명)도 좁지만 소스 접근이 미승인 → CS 운영 리드·워크스페이스 관리자·소스 오너와 검증 세션을 잡는다."

## 6. ChatGPT Deployment Brief 완성하기

브리프는 **긴 구현 계획이 아니라, 다음 계획 행동으로 넘어가게 해 주는 간결한 출발 문서**예요.

| 브리프 섹션 | 답해야 할 질문 |
|---|---|
| 배포 문제 정의 | 고객이 풀려는 워크플로 문제는? |
| 첫 사용자 그룹 / 결과 오너 | 누가 먼저 쓰나? 결과는 누가 책임지나? |
| ChatGPT 지원 워크플로 | 사용자는 ChatGPT로 무엇을 하나? |
| 제품 경험·통제 의존성 | 어떤 경험을 쓰나? Company Knowledge·Apps·ID·접근·행동·승인·사람 검토 의존성 중 미해결은? |
| 성공 기준 | 어떤 채택·피드백·가치 증거를 모으나? |
| 이해관계자 정렬 / 범위 경계 | 확정·누락된 사람은? 포함·제외·보류는? **출력은 누가 볼 수 있나?** |
| 유스케이스 품질 체크 / 롤아웃 가정 | 6가지 품질 기준 충족? 가정은 확정·유력·미해결 중 무엇? |
| 시작 모션 추천 (+ 진행 시 롤아웃 구조) | 진행·검증·재조정·보류 중 무엇, 왜? |
| 관찰 가능성·검토 오너 | 활동·출력·피드백·리스크·다음 증거는 누가 검토? |
| 미해결 리스크 / 즉시 다음 단계 | 출시 계획 전 풀 것은? 다음에 필요한 회의·결정·산출물은? |

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 브리프에 넣을 것</span>
    <ul>
      <li>출시 범위, 성공 기준, 오너, 필요한 증거</li>
      <li>제약과 미해결 준비 항목</li>
      <li>의도한 ChatGPT 경험과 접근·통제 의존성</li>
      <li>즉시 다음 행동</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 브리프에 넣지 않을 것</span>
    <ul>
      <li>상세 워크스페이스 설정, 제품 구성 지침</li>
      <li>전체 활성화 계획, enablement 메시지</li>
      <li>임원 보고 내용, 상세 측정 실행 방법</li>
      <li>기능 탐방, 근거 없는 가치 주장</li>
    </ul>
  </div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🧪 좋은 브리프 테스트</span>
  <p>"배포 리드가 <b>영업 히스토리를 처음부터 다시 읽지 않고도</b> 이 브리프만 보고 다음 고객 회의를 계획할 수 있나?" — 아니면 명확성이 부족한 것. 길기만 하고 여전히 흐릿하면 더 날카롭게 다듬어야 해요.</p>
</div>

실습 팁: ChatGPT에게 브리프 초안을 비평시켜도 좋지만, **가상·익명화·승인된 학습용 입력만** 쓰고 고객 식별자·PHI·비밀 정보는 넣지 않아요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 리더들이 관심 있고 직원들이 이미 AI를 써 보고 있어 고객이 빨리 출시하고 싶어 한다. 배포팀이 가장 먼저 할 일은?</summary>
  <p>① 전사 사용자 교육 시작 ② 리더십 관심을 준비 신호로 보고 바로 출시 계획 ③ 핸드오프를 검토해 확인된 것·가정·검증할 것을 나눈 뒤 시작 모션 추천 ④ 완전한 롤아웃·거버넌스·측정 계획이 나올 때까지 보류</p>
  <p><b>정답: ③</b> — 관심과 실험은 분위기일 뿐 준비 증거가 아니에요. 그렇다고 완벽한 계획을 요구하며 멈출 필요도 없어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. "직원들이 AI에 열광해서 ChatGPT를 원하고, 영업은 전 지식근로자 대상 출시를 제안. 첫 워크플로·오너·성공 기준은 미확인." 다음 할 일은?</summary>
  <p><b>정답:</b> 핸드오프를 <b>정보 부족(under-specified)</b>으로 표시하고, 확정해야 할 워크플로·오너·성공 기준을 명시한다. (바로 전사 계획 ✗ / 기회 거절 ✗ / 영업 제안을 확정으로 보고 셋업 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 가장 배포 준비된 문제 정의는?</summary>
  <p><b>정답:</b> "CSM은 정보가 여러 소스에 흩어져 있어 갱신 브리프 준비에 몇 시간을 쓴다. 첫 워크플로는 승인된 계정 맥락으로 사람이 검토하는 갱신 브리프 초안 작성을 돕는다." — 마찰·사용자·워크플로·검토·소스 경계가 다 있어요. "모두 더 빨리", "직원 생산성", "갱신 계획 자동화"는 너무 넓거나 과장이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. "스폰서는 지지하지만 첫 사용자 그룹 미확정, 워크스페이스 관리자 미지정, 첫 결정 시점 불명확." 핵심 준비도 문제는?</summary>
  <p><b>정답:</b> 이해관계자·범위 갭이 있어, 전진 전에 해결하거나 최소한 브리프에 보이게 해야 한다. (스폰서 지지가 범위 문제를 해결하진 않고, 그렇다고 거절할 일도 아님)</p>
</details>

<details class="oa-quiz">
  <summary>Q5. 유망한 워크플로지만 첫 사용자 그룹 미확정, 소스 접근 미해결, 측정 가능한 가치 신호 미정의. 알맞은 시작 모션은?</summary>
  <p><b>정답: Focused validation</b> — 사용자 그룹·소스 의존성·가치 신호를 확인한 뒤 파일럿이나 더 넓은 배포를 고른다. 모든 게 완벽해야 한다며 보류하는 것도, 바로 전사 배포하는 것도 틀려요.</p>
</details>

<details class="oa-quiz">
  <summary>Q6. 문제 정의와 첫 워크플로는 훌륭하지만 결과 오너·범위 경계·소스 접근 리스크·다음 결정 시점이 없는 브리프. 빠진 것은?</summary>
  <p><b>정답:</b> 기회를 계획으로 옮기는 데 필요한 <b>배포 준비도 정보</b>. (제품 기능 상세 ✗ / 전체 enablement 계획 ✗ / 모든 제약을 푼 셋업 가이드 ✗)</p>
</details>

## 정리

- 원칙: **모션을 약속하기 전에 증거를 확인하라.** 관심·긴급함·실험은 준비의 증거가 아니다.
- 핸드오프는 **확인 / 가정 / 미지수**로 나누고, ABC(Assume nothing, Believe nothing, Confirm everything)로 점검한다.
- 영업 문장을 **마찰·사용자·영향·워크플로·결과**가 담긴 배포용 문제 정의로 바꾸고, 성공 기준은 **채택·피드백·가치 신호**의 관찰 가능한 증거로 — ROI 약속이 아니라 계획 목표치로.
- 스폰서 "지지"만으론 부족: **누가·무엇을·언제·어떤 증거로** 결정하는지, 포함·제외·보류 범위와 출력 접근까지 명시.
- 먼저 **준비도 결정**(진행·집중 검증·재조정·보류), 진행일 때만 **롤아웃 구조**(파일럿·단계적·광범위).
- 결과물인 **Deployment Brief**는 짧고 날카롭게: 다음 회의를 바로 계획할 수 있게.
