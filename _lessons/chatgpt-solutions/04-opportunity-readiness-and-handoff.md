---
title: "ChatGPT Opportunity Readiness & Handoff"
title_ko: "기회 준비도와 인수인계 — 열정이 아니라 증거로 다음 팀에 넘기기"
course: chatgpt-solutions
lesson: 4
---

## 학습 목표

- **적합성(fit)과 준비도(readiness)**의 차이를 이해하고, 6가지 준비 신호로 증거를 평가하기
- 실무자·성과 책임자·의사결정 영향자·스폰서를 확인하고 **이해관계자 공백** 찾기
- 일반적인 "AI 사용량"이 아닌 **업무 결과물 기반 성공 지표** 정의하기
- 배포 계획으로 넘어가지 않고 **초기 롤아웃 가정**을 Known / Assumed / Needs validation으로 정리하기
- 증거 수준에 맞는 **다음 단계** 추천하기
- 다음 담당자가 바로 쓸 수 있는 **간결한 인수인계 요약(handoff summary)** 쓰기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>준비도 평가</strong>6가지 신호: Strong / Developing / Missing</li>
  <li><strong>이해관계자 정렬</strong>4역할 + 공백 찾기</li>
  <li><strong>성공 지표</strong>결과물이 어떻게 나아지나</li>
  <li><strong>롤아웃 가정</strong>Known · Assumed · Needs validation</li>
  <li><strong>다음 단계</strong>증거에 맞는 5가지 선택지</li>
  <li><strong>인수인계 요약</strong>몇 단락으로 압축</li>
</ol>

## 1. 준비도 평가하기

<div class="oa-eli">
  <span class="oa-eli__head">🏃 쉽게 말하면</span>
  <p>이어달리기에서 바통을 넘길 때, 다음 주자가 <b>준비된 자세로, 정해진 구간에서</b> 받아야 해요. 앞 주자가 아무리 빨라도 바통을 대충 던지면 떨어뜨리죠. 고객이 열광해도 <b>문제·사람·가치·다음 단계</b>가 불분명하면 다음 팀이 기초 조사를 처음부터 다시 해야 해요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🎯 Fit (적합성)</span>
    <ul>
      <li>"이 문제에 ChatGPT가 맞는 길인가?"</li>
      <li>예: 갱신 브리핑 준비 = 지식 종합·초안·검토 → 잘 맞음</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">📦 Readiness (준비도)</span>
    <ul>
      <li>"다음 팀이 추측 없이 이어받을 만큼 증거가 있나?"</li>
      <li>잘 맞아도 아직 넘길 준비가 안 됐을 수 있음</li>
    </ul>
  </div>
</div>

### 6가지 준비 신호

| # | 신호 | 확인할 것 |
|---|---|---|
| 1 | **비즈니스 문제 명확성** | 구체적·업무 기반·중요한가? 일반적인 AI 요청처럼 들리지 않나? |
| 2 | **이해관계자 확인** | 실무자·성과 책임자·의사결정 영향자·스폰서가 보이나? |
| 3 | **ChatGPT 경험·사용 사례** | 업무가 어떻게 바뀌는지 먼저. Chat(대화형 탐색) vs Work(검토 가능한 결과 위임), 계속되는 일이면 dot(개인) vs Team Tasks(팀) — 강의 기준 |
| 4 | **고객 참여 신호** | 관심이 아니라 **구체적 다음 단계 합의** (예: 워크플로우 디스커버리 세션 일정) |
| 5 | **초기 가치 가설** | 문제·워크플로우·기대 결과가 논리적으로 연결되나? (최종 ROI 아님) |
| 6 | **운영 맥락·거버넌스 가정** | 필요한 자료, 사용 가능·활성화 여부, 사용자 접근, 권한·검토·정책 질문이 보이나? |

각 신호는 **Strong(충분) / Developing(방향은 맞음, 증거 더 필요) / Missing(아직 증거 없음)**으로 분류해요. 모두 Strong으로 만드는 게 목표가 아니라, **증거 기반 추천**이 목표예요.

<div class="oa-eli">
  <span class="oa-eli__head">📋 예시: "문서 검토에 ChatGPT를 쓰고 싶어요" (컴플라이언스팀)</span>
  <p>이대로는 너무 넓어요. 더 강한 버전: "컴플라이언스 분석가가 내부 감사 준비용 정책 문서 검토·위험 요약에 몇 시간씩 쓴다. 처리 시간 성과는 컴플라이언스 책임자가 가진다. 민감한 내부 정책 문서라 IT·보안이 접근 제약을 검토해야 한다. 고객은 샘플 문서로 컴플라이언스·IT·보안과 워크플로우 디스커버리 세션을 하기로 합의했다."<br>→ 사용자·워크플로우·결과·책임자·영향자·운영 맥락·참여 신호가 모두 드러나요.</p>
</div>

## 2. 이해관계자 정렬 공백 찾기

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧑‍💻</span><span class="oa-card__title">Operators 실무자</span>매일 고통을 겪는 사람. 실제 업무·마찰·필요한 결과물을 검증</div>
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">Outcome owner 성과 책임자</span>비즈니스 결과에 책임 (예: 고객성공 VP, 컴플라이언스 책임자)</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚦</span><span class="oa-card__title">Decision influencers 영향자</span>IT·보안·법무·구매·컴플라이언스·데이터 소유자 — 접근·권한·승인에 영향</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🚀</span><span class="oa-card__title">Sponsor 스폰서</span>긴급성을 만들고 막힌 걸 푸는 임원. 모르면 <b>공백으로 기록</b></div>
</div>

계속되는 일이라면 성과 책임자 ≠ 프로세스 운영자일 수 있어요. 결과 검토, 예외 처리, 공유 결과물 유지, **사람이 떠날 때 연결 점검** 담당을 따로 확인해요.

### 흔한 공백

- 호기심 많은 **연락 담당자 한 명**만 관여
- 실무자는 분명한데 **성과 책임자를 모름** (또는 그 반대: 책임자는 관심 있는데 실제 사용자가 고통을 검증 안 함)
- IT·보안 우려가 뻔히 보이는데 **아직 꺼내지 않음**
- 스폰서를 **추정**만 함 / 다음 단계 담당자가 없음
- **참석한 이해관계자가 워크플로우 성격과 안 맞음** (고객 대상 응답 초안이면 법무·품질, CRM 자료면 RevOps·데이터 소유자, 민감 정책이면 보안·컴플라이언스)

> 이 단계에서 공백을 **해결**할 필요는 없어요. **보이게** 만드는 게 중요해요.

## 3. 성공 지표 정의하기

<div class="oa-eli">
  <span class="oa-eli__head">📏 쉽게 말하면</span>
  <p>"공부 앱을 더 많이 쓰기"는 좋은 목표가 아니에요. "수학 단원평가 오답 수 줄이기"처럼 <b>결과물이 어떻게 나아지는지</b>가 목표여야 해요. 앱 사용 시간은 수단일 뿐이죠.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 지표 (일반적 사용량)</span>
    <ul>
      <li>"ChatGPT 도입률 높이기"</li>
      <li>"AI 비서 출시하기"</li>
      <li>"회사 전체 시간 절약"</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 지표 (업무 결과물)</span>
    <ul>
      <li>"갱신 브리핑 초안 준비 시간 단축"</li>
      <li>"지원 응답 초안의 일관성 향상"</li>
      <li>"정책 요약이 승인된 자료를 따른다는 확신 향상"</li>
    </ul>
  </div>
</div>

| 카테고리 | 예시 |
|---|---|
| **Time & Speed** | 준비·검토 시간 감소, 더 빠른 브리핑 |
| **Quality** | 더 완전한 브리핑, 불필요한 수정 감소 |
| **Capacity** | 전문가에만 의존하지 않고 더 많은 작업 |
| **Risk** | 승인된 자료 사용, 고객 대상 전 사람 검토, 검토 안 된 초안 감소 |
| **Experience** | 준비가 쉬워짐, 반복 업무 감소 |

**현실적으로 쓰기**: "$5M 절감", "업무 80% 감소", "검토 불필요" 같은 말은 피하고, "전후 비교로 검증할 수 있다", "기준선과 목표는 아직 확인 필요"처럼 써요. 비용이 중요하면 **합격 결과물 기준, 현재 준비·검토 시간, 테스트 배치, 실제 과금 기준**을 기록하고, 모르는 기준선은 절약액을 가정하지 말고 **검증 공백**으로 남겨요.

## 4. 초기 롤아웃 가정 정리하기

<div class="oa-eli">
  <span class="oa-eli__head">🧳 쉽게 말하면</span>
  <p>여행 계획을 친구에게 넘길 때 "숙소 예약 완료(확인됨)", "아마 기차로 갈 듯(가정)", "박물관 휴관일 확인 필요(검증 필요)"를 구분해 주면 친구가 헷갈리지 않죠. 가정을 사실처럼 쓰면 나중에 큰일 나요.</p>
</div>

### 첫 워크플로우와 사용자 그룹

```text
우선 워크플로우: 갱신 브리핑 준비
첫 사용자 그룹: 미드마켓 갱신 담당 고객성공 매니저
현실적 출발점인 이유: 반복되고, 준비에 시간이 들고, 이미 매니저가 검토함
기대 결과물: 검토용 갱신 브리핑 초안
비즈니스 결과: 갱신 준비도 향상, 더 일관된 계정 준비
```

### Known / Assumed / Needs validation

<div class="oa-svg">
<svg viewBox="0 0 720 190" role="img" aria-label="Known, Assumed, Needs validation 세 칸">
  <defs>
    <style>.c4g{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.c4q{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.c4w{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.c4t{font-size:14px;fill:#4a4a4a;font-weight:bold}.c4s{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="15" width="225" height="160" rx="10" class="c4g"/><text x="122" y="42" text-anchor="middle" class="c4t">✅ Known</text><text x="122" y="64" text-anchor="middle" class="c4s">고객이 분명히 말했거나 확인한 것</text><text x="122" y="96" text-anchor="middle" class="c4s">법무 분석가가 1차 공급 계약</text><text x="122" y="112" text-anchor="middle" class="c4s">요약에 시간을 씀. 매니저는</text><text x="122" y="128" text-anchor="middle" class="c4s">더 빠르고 일관된 구조를 원함</text>
  <rect x="248" y="15" width="225" height="160" rx="10" class="c4q"/><text x="360" y="42" text-anchor="middle" class="c4t">🤔 Assumed</text><text x="360" y="64" text-anchor="middle" class="c4s">그럴 것 같지만 미검증</text><text x="360" y="96" text-anchor="middle" class="c4s">공급 계약 요약이 첫 워크플로우,</text><text x="360" y="112" text-anchor="middle" class="c4s">법무 분석가가 첫 사용자,</text><text x="360" y="128" text-anchor="middle" class="c4s">사용 전 사람 검토 예상</text>
  <rect x="486" y="15" width="225" height="160" rx="10" class="c4w"/><text x="598" y="42" text-anchor="middle" class="c4t">🔍 Needs validation</text><text x="598" y="64" text-anchor="middle" class="c4s">진행 전 반드시 확인할 것</text><text x="598" y="96" text-anchor="middle" class="c4s">계약 저장소 접근, 검토 요건,</text><text x="598" y="112" text-anchor="middle" class="c4s">조항별 수동 검토 범위,</text><text x="598" y="128" text-anchor="middle" class="c4s">법무·보안 승인 절차</text>
</svg>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 가정을 사실처럼</span>
    <ul>
      <li>"ChatGPT가 고객 CRM에 접근해 고객용 최종 갱신 브리핑을 만든다."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 중립적 표현</span>
    <ul>
      <li>"CRM·지원 이력의 계정 맥락이 필요할 수 있다. 자료 접근·권한 경계·검토 기대치는 검증 필요. 고객은 사용 전 사람이 검토하길 기대한다."</li>
    </ul>
  </div>
</div>

### 가정으로 기록해야 하는 것들 (강의 기준)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">경험·모델</span>정확한 모델·추론 설정·속도 옵션과 요금제·지역·클라이언트·활성화 여부. Intelligent UI는 Chat 전용, 첫 응답이 보여도 완료된 결과를 검토해야 함</div>
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">연결된 경험</span><b>인터페이스 · 시작 트리거 · 맥락을 주는 계정 · 허용된 행동</b>을 따로 기록. 플러그인 화면이나 MCP Event가 행동 승인을 뜻하지 않음</div>
  <div class="oa-card"><span class="oa-card__icon">💻</span><span class="oa-card__title">실행 환경</span>Work의 로컬 실행 ≠ 데이터가 기기에만 머묾. 파일 접근 ≠ 업무 행동 권한</div>
  <div class="oa-card"><span class="oa-card__icon">📄</span><span class="oa-card__title">공유 결과물</span>Space 페이지에 복사된 정보는 원본 권한이 없어도 독자에게 보임 → 독자와 내용 범위 합의, 유지 담당 따로</div>
  <div class="oa-card"><span class="oa-card__icon">💳</span><span class="oa-card__title">상업 조건</span>Sign in with ChatGPT ≠ 요금제 사용량 허가, 기업 구독 ≠ 개인 요금제 혜택, Marketplace 목록 ≠ 모든 제품 해당, API는 별도 과금</div>
  <div class="oa-card"><span class="oa-card__icon">🌐</span><span class="oa-card__title">ChatGPT Sites</span>방문 중 방문자의 승인된 자료를 읽기 전용으로. 만든 사람의 연결을 공유하지 않고, 예약 백그라운드 작업 불가</div>
</div>

## 5. 다음 단계 추천하기

<div class="oa-eli">
  <span class="oa-eli__head">🚥 쉽게 말하면</span>
  <p>신호등처럼 생각하면 쉬워요. 초록불(증거 충분)이면 출발, 노란불(특정 공백)이면 그 공백부터 해결, 빨간불(너무 막연)이면 아직 출발하지 않아요. "무조건 넘기기"가 항상 정답은 아니에요.</p>
</div>

| 다음 단계 | 언제 | 신호 예시 |
|---|---|---|
| 🟢 **Ready for structured handoff** | 문제·워크플로우·이해관계자·가치·가정·다음 단계가 충분히 명확 | 고통 확인, 실무자·책임자 확인, 영향자 참여, 인수인계 논의 합의 |
| 🟡 **Needs focused discovery** | 일부 신호가 developing이지만 해결 가능 | 워크플로우는 유망한데 정확한 프로세스·입력·결과물이 불명확 |
| 🟡 **Needs stakeholder alignment** | 책임자·스폰서·영향자가 빠짐 | 실무자는 열정적인데 책임자 미참여, IT·보안 미관여 |
| 🟡 **Needs value clarification** | 측정할 결과가 불분명 | "팀이 더 빨리 일하게"만 있고 어떤 결과물인지 없음 |
| 🔴 **Not ready yet** | 아직 일반적·기능 중심·탐색 단계 | "AI 비서가 필요해요"인데 워크플로우·사용자·가치를 말 못 함 |

유망한 워크플로우는 몇 가지 질문이 남아 있어도 **검증·인수인계 논의**로 갈 수 있어요. 단, 각 질문에 담당자를 붙이고, 그 논의를 **반복 작업 시작 승인으로 취급하지 않아요.**

### 다음 단계 추천문

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 모호</span>
    <ul>
      <li>"다음 단계: 대화를 계속한다."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 구체적</span>
    <ul>
      <li><b>행동</b>: 60분 워크플로우 검증 세션</li>
      <li><b>참석자</b>: 고객성공, RevOps, IT·보안</li>
      <li><b>확인할 증거</b>: 자료, 데이터 경계, 첫 사용자, 검토 기대치, 성공 지표</li>
      <li><b>파트너 행동</b>: 세션 전 기회 노트·인수인계 요약 공유</li>
    </ul>
  </div>
</div>

## 6. 인수인계 요약(handoff summary) 만들기

| 항목 | 담을 내용 |
|---|---|
| 고객 문제 | 지금 무슨 일이, 누구에게, 왜 중요한가 |
| 우선 워크플로우 | 협업 / 위임 / 개인 지속 / 팀 운영 중 무엇, 기대 결과물 |
| 사용자·이해관계자 | 사용자, 성과 책임자, 영향자, 스폰서 상태, (지속 업무) 운영·검토·연결 담당 |
| 가치 가설 | 기대 개선과 그 근거 |
| 준비 신호 | 6가지 신호의 Strong / Developing / Missing |
| 맥락·제품·접근 가정 | 자료와 승인 상태, 경험·모델·클라이언트·연결, 확인 담당자 |
| 거버넌스·검토 가정 | 접근·권한·정책·승인·모니터링 질문 |
| 공유 결과물·유지 | 결과물, 독자, 검토자, 유지 담당, 트리거·연결 의존성 |
| 상업 가정 | 사용량·크레딧·API 과금·Marketplace 주장 확인 필요 여부 |
| 성공 지표 · 롤아웃 가정 · 미해결 질문 · 다음 단계 | 위 1~5절 내용 |

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">넣지 않는 것</span>상세 워크스페이스 설정, 최종 배포 설계, 근거 없는 ROI, 미검증 기술 가정, 긴 기능 목록, 일반적 AI 생산성 주장</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">✅</span><span class="oa-card__title">보내기 전 셀프 체크</span>대화 밖 사람이 문제를 이해하나? 워크플로우가 구체적인가? 이해관계자나 공백이 보이나? 가치가 현실적인가? 가정과 사실이 분리됐나? 거버넌스 질문이 보이나? 다음 단계가 분명한가?</div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">📝 요약 예시 (갱신 브리핑, 압축)</span>
  <p><b>문제</b>: CSM이 흩어진 계정 메모·지원 이력·사용 요약·갱신 위험으로 브리핑을 만드느라 몇 시간씩 씀.<br><b>경험</b>: 여러 자료를 정해진 초안으로 모으는 일이라 Work를 첫 테스트 후보로, 범위를 정할 땐 Chat. 대상 사용자의 Work 접근·활성화는 미확인. 지속 프로세스는 별도 결정.<br><b>사람</b>: 사용자 CSM, 책임자 고객성공 VP, 영향자 RevOps·IT/보안, VP 이상 스폰서는 미확인.<br><b>가치·지표</b>: 준비 시간 감소·일관성 향상(가설), 고객 대상 사용 전 사람 검토. 기준선 미정.<br><b>다음 단계</b>: 고객성공·RevOps·IT/보안과 워크플로우 검증 세션 → 자료·접근 경계·검토 요건·지표 확정 후 배포 계획으로.</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. "지원 상담원이 ChatGPT에 관심이 있고, 답 찾는 데 시간이 들며, 응답 품질을 높이고 싶다. 다음 주 60분 검증 세션에 합의했지만 성과 책임자·영향자·스폰서는 미확인." 가장 분명히 빠진 신호는?</summary>
  <p><b>정답: Stakeholders identified (이해관계자 확인)</b> — 참여 신호(세션 합의)는 이미 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. IT는 검증 콜에 합류하기로 했지만 응답 품질·지원 효율·고객 경험 성과의 비즈니스 책임자가 없다. 인수인계 전 해결할 공백은?</summary>
  <p><b>정답: 성과의 비즈니스 책임자(business owner)가 지정되지 않았다.</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 갱신 브리핑 워크플로우에 가장 강한 성공 지표는?</summary>
  <p>① CSM의 ChatGPT 사용량 증가 ② 생산성이 높아졌다고 느끼는지 확인 ③ <b>갱신 브리핑 초안 준비 시간 단축과 브리핑 품질 일관성 향상</b> ④ 팀 전체에 ChatGPT 출시</p>
  <p><b>정답: ③</b> — 워크플로우 결과물과 개선 내용을 이름 붙였어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 가장 좋은 판매자 수준의 롤아웃 가정은?</summary>
  <p><b>정답:</b> "법무 분석가가 공급 계약 요약의 첫 사용자 그룹<b>일 수 있다</b>. 저장소 접근, 검토 요건, 법무·보안 승인 기대치는 <b>아직 검증이 필요하다</b>." — "확정된", "모든 계약 유형부터", "배포팀이 설정한 뒤"는 가정을 사실처럼 쓰거나 배포 계획으로 넘어간 오답이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q5. 재무팀: 워크플로우 명확, 재무 이사가 성과 책임, 가치도 분명. 하지만 재무 원본 파일이 필요할 수 있고 IT·보안은 미관여. 최선의 다음 단계는?</summary>
  <p><b>정답: Needs stakeholder alignment before handoff</b> — 즉시 인수인계도, "아직 막연함"도 아니에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q6. 판매자 수준 인수인계 요약에 넣지 말아야 할 것은?</summary>
  <p><b>정답: 최종 워크스페이스 설정(final workspace configuration)</b> — 그건 배포 단계의 일이에요.</p>
</details>

## 정리

- **Fit ≠ Readiness.** 잘 맞는 기회도 증거가 부족하면 아직 넘길 때가 아니다.
- 6가지 신호(문제·이해관계자·경험/사용 사례·참여·가치 가설·운영 맥락)를 Strong / Developing / Missing으로 분류한다.
- 이해관계자 4역할을 확인하고, 공백은 해결보다 **보이게** 만든다.
- 성공 지표는 **사용량이 아니라 결과물 개선**. 근거 없는 수치 대신 검증할 질문으로 남긴다.
- 롤아웃 가정은 **Known / Assumed / Needs validation**으로 나누고, 인터페이스·트리거·계정·허용 행동, 공유 결과물 독자·유지, 상업 조건을 따로 기록한다.
- 다음 단계는 증거에 맞춰 5가지 중 고르고, 행동·참석자·확인할 증거를 구체적으로 쓴다.
- 인수인계 요약은 **배포 계획이 아니라** 다음 팀이 바로 움직일 수 있는 몇 단락이다.
