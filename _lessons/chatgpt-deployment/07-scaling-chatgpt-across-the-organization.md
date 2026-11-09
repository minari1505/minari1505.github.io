---
title: "Scaling ChatGPT Across the Organization"
title_ko: "조직 전체로 ChatGPT 확장하기 — 증거·오너십·거버넌스로 넓히기"
course: chatgpt-deployment
lesson: 7
---

## 학습 목표

- 비즈니스 관련성·준비도·실현 가능성·영향 기준으로 **확장 우선순위** 평가하기
- 조직 **확장 기회의 순서** 정하기
- 확장을 위한 **오너십과 거버넌스** 정의하기
- 더 넓은 도입을 위한 **enablement·지원 요구사항** 찾기
- 조직 성장에 따른 **리스크와 의존성** 평가하기
- **ChatGPT Organizational Scaling Strategy**의 핵심 구성요소 만들기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>확장 우선순위</strong>어디로 · 왜 지금 · 무슨 증거로</li>
  <li><strong>오너십·거버넌스</strong>누가 결정·조정·검토·에스컬레이션</li>
  <li><strong>Enablement·지원 확장</strong>반복 가능한 온보딩 · 챔피언 네트워크</li>
  <li><strong>리스크·의존성</strong>완화책 · 오너 · 증거</li>
  <li><strong>Scaling Strategy</strong>단계적 확장 · 다음 행동 · 체크포인트</li>
</ol>

<div class="oa-eli">
  <span class="oa-eli__head">🍜 쉽게 말하면</span>
  <p>동네 분식집 떡볶이가 대박 났다고 다음 달에 전국 100호점을 내면 어떻게 될까요? 레시피가 지점마다 달라지고, 재료 공급이 끊기고, 점장 교육이 안 돼서 "원조 맛"이 사라져요. 똑똑한 사장님은 <b>비슷한 동네에 2호점</b>부터 내고, <b>레시피 매뉴얼·점장 교육·맛 점검 일정</b>을 먼저 정해요.</p>
  <p>ChatGPT 확장도 같아요. 좌석 수 목표나 "전사 배포"가 아니라 <b>증거</b>에서 출발해요.</p>
</div>

## 1. 사례 고객: Middleton Manufacturing

가상의 글로벌 제조·산업 서비스 기업. 지역 운영 매니저용 **승인 SOP 기반 사람 검토 절차 요약** 워크플로를 활성화했어요(레슨 6의 결과를 이어받는 상황).

| 좋은 신호 | 남은 이슈 |
|---|---|
| 첫 코호트 참여 강함, 반복 사용 | 다른 지역 추가 전 **검토 기대 확인** 필요 |
| 최고 요약은 "가벼운 수정으로 유용" | **지원 질문 증가** |
| 소스 가이드 개선 완료 | 지역별 안전 노트 존재 |
| 챔피언 지원 채널 활성화 | 분기 비즈니스 리뷰 전 **가시적 성과** 압박 |

### 출발점: 확장 결정부터 다시 보기

직전의 Activation Review and Expansion Plan이 **계속 · 조정 · 확장 · 중단** 중 무엇을 말했는지 확인해요. 모든 이슈가 해결될 필요는 없지만 **책임 있는 결정을 내릴 만큼의 증거**는 있어야 해요. 확장을 뒷받침하는 증거는? 아직 방향성인 증거는? 해결된 이슈와 관리 가능한 열린 이슈는? 어떤 옵션이 **이미 된 것 위에** 쌓이고, 어떤 옵션이 **미해결 문제를 곱절로** 만드나?

## 2. 확장 우선순위 정하기

### 확장 영역의 종류

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">👥</span><span class="oa-card__title">인접 사용자 그룹</span>비슷한 역할·팀·지역의 같은 워크플로</div>
  <div class="oa-card"><span class="oa-card__icon">🔁</span><span class="oa-card__title">유사 워크플로</span>비슷한 입력·출력·검토 패턴·소스 세트</div>
  <div class="oa-card"><span class="oa-card__icon">🏢</span><span class="oa-card__title">추가 팀·부서</span>배포 재설계 없이 도입할 만큼 준비된 관련 팀</div>
  <div class="oa-card"><span class="oa-card__icon">🧭</span><span class="oa-card__title">관련 비즈니스 기능</span>맥락·오너·거버넌스를 갖추면 패턴 재사용 가능</div>
  <div class="oa-card"><span class="oa-card__icon">🌏</span><span class="oa-card__title">준비 신호가 있는 지역·집단</span>이해관계자 지원·소스 준비·지원 역량 보유</div>
</div>

가장 안전한 후보는 **이미 증거를 낸 워크플로와 가까운 것**. 예: 같은 SOP 구조·검토 프로세스·챔피언 모델을 쓰는 다른 운영 지역 ✓ / 고객 대면 사고 대응 워크플로 — 새 데이터 경계, 더 높은 검토 기대, 다른 리스크 오너·지원 모델 ⚠️.

### 6가지 비교 기준

| 기준 | 질문 |
|---|---|
| **비즈니스 관련성** | 의미 있는 고객 우선순위와 연결되나? 넓힐 만큼 중요한가? |
| **조직·맥락 준비도** | 비즈니스 맥락·소스·가이드·이해관계자 지원이 준비됐나? |
| **운영 실현 가능성** | 사람·시간·교육 자원·챔피언·검토 역량으로 감당 가능? |
| **기대 영향** | 노력·품질·일관성·속도·리스크·고객 경험 개선을 믿을 근거? |
| **거버넌스·검토 준비도** | 허용 사용·소스 경계·검토·에스컬레이션이 다음 대상에게 명확? |
| **관찰 가능성 (Observability)** | 확장되는 동안 활동·피드백·이슈·리스크·개선을 볼 수 있나? |

<div class="oa-eli">
  <span class="oa-eli__head">🔭 관찰 가능성이 왜 중요할까</span>
  <p>반 친구 5명 숙제는 선생님이 한눈에 보지만, 전교생 500명이면 누가 어디서 막혔는지 <b>대시보드·출석부·질문함</b> 없이는 몰라요. 확장하면 활동·편차·문제 가능성이 모두 커지고 숨기 쉬워져요. 관찰 가능성 = <b>AI가 돕는 일이 커지는 동안 그것을 모니터링·통제·개선하는 방법</b>이에요.</p>
</div>

### Middleton의 5가지 옵션 비교

<div class="oa-bars">
  <div class="oa-bar"><span>2번째 운영 지역</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:90%"></span><span class="oa-bar__val">높음 · 의존성 해결 시</span></span></div>
  <div class="oa-bar"><span>운영 내 안전사고 요약</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:50%"></span><span class="oa-bar__val">나중 · 별도 준비 리뷰</span></span></div>
  <div class="oa-bar"><span>HR 정책 요약</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:40%"></span><span class="oa-bar__val">나중 · 새 기능·소스</span></span></div>
  <div class="oa-bar"><span>재무 운영 리뷰 코멘터리</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:40%"></span><span class="oa-bar__val">나중 · 새 오너·검토</span></span></div>
  <div class="oa-bar"><span>고객 대면 응답 초안</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:20%"></span><span class="oa-bar__val">범위 밖 · 고위험</span></span></div>
  <p class="oa-bars__caption">개념적 표현 — 강의 사례를 바탕으로 "첫 증거와의 거리·준비도"를 상대적으로 나타낸 것이에요. 실제 점수가 아니에요.</p>
</div>

| 항목 | 2번째 운영 지역 (우선순위 요약 예시) |
|---|---|
| 근거 | 비슷한 사용자, 같은 워크플로, 같은 SOP 구조, 비슷한 매니저 검토 기대 |
| 준비 신호 | 첫 지역의 반복 사용, 긍정적 매니저 피드백, 개선된 소스 가이드 |
| 기대 영향 | 지역 간 더 일관된 절차 요약 |
| 핵심 의존성 | 2번째 지역 교육, 챔피언 커버리지, 매니저 검토 역량 확인 |
| 우선순위 | **높음** — 단, 출시 전 의존성 해결 시 |

"첫 코호트가 ChatGPT를 좋아했으니 운영 사용자 더 늘리자"보다 훨씬 강해요. 좋은 우선순위는 **왜 여기 · 왜 지금 · 어떤 증거 · 출시 전 무엇이 준비돼야 · 어떤 리스크**에 답해요.

## 3. 확장을 위한 오너십과 거버넌스

확장하면 팀·워크플로·지역·지원 요구가 늘어 **비공식 오너십으로는 부족**해요. 거버넌스가 무거울 필요는 없지만 **보여야** 해요. "고객이 맡는다"가 아니라 **역할·그룹 이름**으로 적어요.

### 오너십 영역

| 영역 | 역할 |
|---|---|
| **임원 스폰서** | 정렬 유지, 우선순위·경쟁 수요 조정, 결정 주기 지원 (일상 운영은 X) |
| **확장 영역별 비즈니스 오너** | 업무를 이해하고 의도한 결과를 책임 |
| **운영 롤아웃 조정자** | 계획·시점·커뮤니케이션·교육·의존성·체크포인트 |
| **워크스페이스·제품 통제 오너** | 역할·SCIM 그룹 접근, Analytics Viewer 부여, 플러그인·Apps·Company Knowledge·Workspace Agent 빌드·게시, 공유 연결·행동 승인, 컴플라이언스 기록 접근 (강의 기준, 플랜·설정에 따라 다름) |
| **챔피언 네트워크 리드** | 커버리지를 **가정하지 말고 계획** |
| **에스컬레이션 오너** | 도입 장애·소스·검토·거버넌스·역량 이슈의 행선지 |
| **오너 변경 시 연속성** | 개인 Dots, 팀 소유 Team Tasks, Workspace Agents는 소유·접근 방식이 다름 (강의 기준) |

<div class="oa-eli">
  <span class="oa-eli__head">🔑 담당자가 바뀔 때</span>
  <p>반장이 전학 가면 반 단톡방 관리자 권한, 공용 사물함 열쇠를 <b>누가 넘겨받는지</b> 정해야 하죠. 팀 소유 작업도 누가 수정·예외 검토·다음 실행 일시중지·연결 재검증을 할지 정해요. <b>팀 서비스 계정이 있어도, 개인 외부 계정에 묶인 연결은 사라질 수 있어요.</b></p>
</div>

### 거버넌스 고려사항

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📏</span><span class="oa-card__title">정책 일관성</span>팀마다 같은 허용 사용·데이터·검토·에스컬레이션 기대</div>
  <div class="oa-card"><span class="oa-card__icon">👁️</span><span class="oa-card__title">사람 검토</span>고객·시스템·결정·규제·운영에 영향 주기 전 검토</div>
  <div class="oa-card"><span class="oa-card__icon">🚧</span><span class="oa-card__title">소스·워크플로 경계</span>무엇을 쓰고 무엇이 범위 밖인지</div>
  <div class="oa-card"><span class="oa-card__icon">📅</span><span class="oa-card__title">감독·결정 주기</span>도입·품질·피드백·거버넌스·준비도를 얼마나 자주 보나</div>
  <div class="oa-card"><span class="oa-card__icon">🧾</span><span class="oa-card__title">감사·증거 요구</span>검토 샘플, 지원 로그, 피드백 기록, 결정 노트</div>
  <div class="oa-card"><span class="oa-card__icon">🔐</span><span class="oa-card__title">출력 접근</span>소스 접근과 <b>별도로</b> 결과물을 누가 볼 수 있나</div>
</div>

**출력 접근 주의 (강의 기준):** Team Tasks에 멤버를 추가하기 전 이전 실행·생성 파일 접근을 검토하고, 공유 Pages는 대상 독자와 관리자를 확인해요. 소스 권한이 복사된 정보까지 제한한다거나, 공유 콘텐츠가 자동 갱신된다고 가정하지 않아요.

### 분석 vs 컴플라이언스 기록 — 증거의 경계

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">📊 Analytics Viewer · Task Insights (강의 기준)</span>
    <ul>
      <li>도입·작업 패턴, 그룹별 세분화</li>
      <li>교육·확장 결정에 참고</li>
      <li>⚠️ 사용량만으로 <b>품질·컴플라이언스·가치를 증명하지 않음</b></li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🗄️ 컴플라이언스 기록·로그</span>
    <ul>
      <li>감사·조사·보존·정책 검토용</li>
      <li>권한 있는 사람만 접근</li>
      <li>분석 대시보드와 <b>분리해서</b> 관리</li>
    </ul>
  </div>
</div>

### 거버넌스 요약에 담을 것

**책임**(품질·소스·행동·검토·에스컬레이션·확장 결정) · **제품 통제 오너십** · **에스컬레이션 경로**(비즈니스 오너행 vs 거버넌스·보안·법무·IT·컴플라이언스행) · **강화 필요**(온보딩·매니저 브리핑·챔피언 세션·지원 자료에 반복) · **증거 경계**.

> 예: "2번째 지역 확장 전, 지역 매니저가 승인 SOP 사용·필수 사람 검토·에스컬레이션을 이해했는지 확인한다. 운영 워크플로 오너=소스 가이드, 지역 매니저=출력 검토, 챔피언 리드=반복 이슈 수집, 거버넌스 오너=첫 확장 체크포인트 참석."

## 4. Enablement와 지원의 확장

한 그룹에 통한 방식이 큰 롤아웃엔 부족할 수 있어요. 라이브 세션, 비공식 코칭, 과로하는 소수 챔피언에만 기대지 않도록 **반복 가능하게** 만들어요.

### Enablement 요구사항

| 요구 | 내용 |
|---|---|
| 반복 가능한 온보딩 | 새 팀마다 같은 워크플로·행동·소스·검토·지원 경로 소개 |
| 재사용 가능한 워크플로 가이드 | 단계, 예시 프롬프트, 승인 소스 지침, 출력 예시, 검토·에스컬레이션 |
| 매니저·챔피언 enablement | 질문 응대, 시연, 이슈 라우팅을 위한 별도 준비 |
| 셀프서비스 자료 | 잡 에이드, 짧은 데모, FAQ, 예시, 채널 안내 |
| 지속 학습·리프레시 | 첫 주 후, 첫 체크포인트 후, 워크플로 변경 시 |

**실제 사례 (강의 기준):** 한 글로벌 금융기관은 ChatGPT Enterprise 접근에 구조화된 학습을 결합하고 보안·법무·컴플라이언스·기술팀과 초기부터 조율했어요. 고위 리더를 교육하고, 챔피언·고급 사용자 네트워크가 실습 워크숍을 열어 동료의 일상 업무 적용과 유용한 워크플로 발굴을 도왔어요. **중앙팀은 공통 기대를 정하고, 리더는 학습을 강화하고, 챔피언은 실천을 돕는** 분업이에요.

### 챔피언 네트워크 요구사항

팀·기능별 **커버리지** · 명확한 **책임** · 커뮤니티 참여(같은 문제를 각자 풀지 않게) · **피드백 수집** · 반복 장애의 **에스컬레이션 경로**.

> ⚠️ **챔피언 피로**: 같은 사람에게 교육·응답·피드백·에스컬레이션·확장 지원을 시간과 권한 없이 맡기면 확장 자체가 깨지기 쉬워져요.

### 질문 유형별 행선지

<div class="oa-svg">
<svg viewBox="0 0 720 250" role="img" aria-label="질문 유형별 지원 라우팅">
  <defs>
    <marker id="ar7a" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.n7a{fill:#f6f3ea;stroke:#e5dfcf;stroke-width:1.5}.q7a{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.t7a{font-size:12px;fill:#4a4a4a;font-weight:bold}.s7a{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="100" width="150" height="50" rx="10" class="n7a"/><text x="85" y="122" text-anchor="middle" class="t7a">사용자 질문</text><text x="85" y="139" text-anchor="middle" class="s7a">종류를 먼저 구분</text>
  <line x1="160" y1="125" x2="296" y2="22" stroke="#c99400" stroke-width="1.2" marker-end="url(#ar7a)"/>
  <line x1="160" y1="125" x2="296" y2="62" stroke="#c99400" stroke-width="1.2" marker-end="url(#ar7a)"/>
  <line x1="160" y1="125" x2="296" y2="102" stroke="#c99400" stroke-width="1.2" marker-end="url(#ar7a)"/>
  <line x1="160" y1="125" x2="296" y2="142" stroke="#c99400" stroke-width="1.2" marker-end="url(#ar7a)"/>
  <line x1="160" y1="125" x2="296" y2="182" stroke="#c99400" stroke-width="1.2" marker-end="url(#ar7a)"/>
  <line x1="160" y1="125" x2="296" y2="222" stroke="#c99400" stroke-width="1.2" marker-end="url(#ar7a)"/>
  <rect x="300" y="6" width="410" height="32" rx="8" class="q7a"/><text x="315" y="27" class="t7a">워크플로 질문</text><text x="700" y="27" text-anchor="end" class="s7a">→ 챔피언 채널</text>
  <rect x="300" y="46" width="410" height="32" rx="8" class="q7a"/><text x="315" y="67" class="t7a">소스 질문</text><text x="700" y="67" text-anchor="end" class="s7a">→ 워크플로·소스 오너</text>
  <rect x="300" y="86" width="410" height="32" rx="8" class="q7a"/><text x="315" y="107" class="t7a">검토 질문</text><text x="700" y="107" text-anchor="end" class="s7a">→ 매니저·비즈니스 오너</text>
  <rect x="300" y="126" width="410" height="32" rx="8" class="q7a"/><text x="315" y="147" class="t7a">접근 질문</text><text x="700" y="147" text-anchor="end" class="s7a">→ 워크스페이스 관리자·IT</text>
  <rect x="300" y="166" width="410" height="32" rx="8" class="q7a"/><text x="315" y="187" class="t7a">거버넌스 우려</text><text x="700" y="187" text-anchor="end" class="s7a">→ 거버넌스 오너</text>
  <rect x="300" y="206" width="410" height="32" rx="8" class="q7a"/><text x="315" y="227" class="t7a">반복되는 도입 장애</text><text x="700" y="227" text-anchor="end" class="s7a">→ 배포 오너 리뷰</text>
</svg>
</div>

지원은 질문에 답하는 것만이 아니라, 확장하는 동안 **도입을 계속 보이게 하는 장치**예요.

## 5. 확장 리스크와 의존성

작을 때 관리되던 문제가 크면 커져요. 소수에게만 영향 주던 소스 품질 문제가 큰 품질 문제가 되고, 한 지역에 통하던 지원 채널이 과부하되고, 열성 매니저 한 명에 기대던 검토가 팀마다 들쭉날쭉해져요.

### 흔한 확장 리스크

| 리스크 | 왜 중요한가 |
|---|---|
| 팀별 도입 편차 | 어떤 팀은 빠르게, 어떤 팀은 업무 순간 밖에서만 |
| 소스·맥락 품질 편차 | 낡은·불완전·미승인 로컬 자료 사용 |
| 매니저 지원·검토 규율 편차 | 시간·명확성·자신감 부족 |
| 거버넌스 공백 | 경계·검토·소스 규칙·에스컬레이션이 일관되지 않음 |
| 챔피언 피로 | 지원 요청 폭증 |
| 워크플로 품질 편차 | 가이드·소스·프롬프트·검토 기대 불일치 |
| 자원 제약 | 교육·지원·검토 역량·운영 관심 부족 |
| 가시성 부족 | 신호가 흩어져 신뢰가 깨진 뒤에야 문제 발견 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 리스크 문장</span>
    <ul><li>"거버넌스가 문제가 될 수 있다."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 리스크 문장</span>
    <ul><li>"2번째 지역 확장 시 지역 매니저가 명확한 검토 기대를 받지 않고 거버넌스 오너가 첫 체크포인트에 참여하지 않으면, <b>검토 관행이 일관되지 않을 수 있다</b>." → 완화책이 바로 보임</li></ul>
  </div>
</div>

### 의존성 — 준비·확인·해결돼야 하는 것

이해관계자 지원 · 거버넌스 준비도 · 지원 역량 · 챔피언 커버리지 · 재사용 가능한 교육 자원 · 조직 정렬(임원 우선순위·사업부 목표·타이밍) · **워크스페이스·제품 통제**(역할·SCIM 그룹, 승인 플러그인·Apps, Company Knowledge 권한, Workspace Agent 게시·연결 방식, 행동 승인) · **분석 접근법** · **컴플라이언스 증거 접근법** · 에스컬레이션·개선 루프 · 승인 소스·워크플로 가이드.

의존성이 항상 차단 요인은 아니에요. 출시 전 해결할 것도, 단계적 롤아웃 중 모니터링할 것도 있어요. 중요한 건 **이름 붙이고 오너를 정하는 것**. 그리고 의존성이 해결 안 된 **선택적 워크플로는 미뤄도**, 따로 준비된 롤아웃을 막지 않아요.

<div class="oa-eli">
  <span class="oa-eli__head">📬 사례: 주간 SOP 변경 브리프</span>
  <p>Middleton은 지역 매니저용 <b>주간 SOP 변경 브리프</b>를 Team Task(강의 기준)로 만들까 고민 중이에요. 그런데 제안한 오너가 부서를 옮기고, 후임·필요한 연결·이전 출력 접근이 확인되지 않았어요. → 이 제안은 <b>2번째 지역 롤아웃과 분리</b>해서, 후임 오너십과 연결 접근이 확인될 때까지 <b>보류</b>하고, 절차 요약 확장은 자체 준비 점검대로 진행해요.</p>
</div>

### 완화 접근

리스크 감소(가이드 갱신, 소스 지침 명확화, 검토 기대 확인, **범위 축소**) · 준비 리뷰 체크포인트 · 거버넌스 강화(온보딩·브리핑·챔피언 세션·자료에) · 챔피언·지원 역량 계획. 완화 조치 = **리스크/의존성 + 조치 + 오너 + 시점 + 볼 증거**.

<div class="oa-eli">
  <span class="oa-eli__head">📈 질문이 늘었다 = 나빠졌다?</span>
  <p>학생 수가 2배가 되면 질문도 2배가 되는 게 자연스럽죠. 지원 질문 증가는 <b>단지 활성 사용자가 늘어서</b>일 수 있어요. 2주 체크포인트에서 <b>질문 수 ÷ 활성 사용자</b>, 미해결 질문과 대기 시간, 반복 소스 이슈, <b>매니저 검토 대기 중인 요약</b>을 함께 봐요. 적체가 커지거나 필수 검토가 제때 안 되면 범위를 줄이거나 지원을 늘리고, <b>질문 수만 보고 확장을 늦추지는 않아요.</b></p>
</div>

## 6. ChatGPT Organizational Scaling Strategy

| 섹션 | 담을 것 | Middleton 예시 |
|---|---|---|
| **확장 우선순위** | 대상, 순서(먼저·다음·나중·아직 아님), 목표 | 새 기능·고객 대면보다 **2번째 운영 지역 먼저**. COO의 운영 일관성 목표 지원. 2번째 지역 교육·검토 오너·챔피언 확인까지 단계적으로 |
| **거버넌스·오너십** | 오너십 모델, 거버넌스, 감독 주기, 관찰 가능성, 운영 통제, 분석·컴플라이언스 경계 | **2주 확장 체크포인트**: 승인 그룹 확인, 관련 시 앱·Agent 통제 확인, Analytics/Task Insights + 매니저 검토 샘플 + 지원 질문 + 소스 이슈 + 미해결 에스컬레이션 검토. 컴플라이언스 기록은 필요할 때 별도 |
| **지원 요구** | 교육, 챔피언 네트워크, 지원 구조 | 잡 에이드에 승인 SOP 가이드·지역 안전 노트 예시·검토 단계·지원 경로 추가. 사전 세션 1회 + 첫 2주 오피스아워 2회 |
| **권고·다음 행동** | 접근법, 리스크·의존성, 오너, 다음 행동, 체크포인트, 볼 증거 | 아래 참고 |

### 네 가지 확장 접근

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🪜</span><span class="oa-card__title">Phased expansion</span>인접 팀·지역·워크플로 하나씩, 매번 체크포인트</div>
  <div class="oa-card"><span class="oa-card__icon">🔂</span><span class="oa-card__title">Repeat and extend</span>비슷한 맥락에서 반복 → 증거·준비도가 좋아지면 관련 워크플로로</div>
  <div class="oa-card"><span class="oa-card__icon">🔧</span><span class="oa-card__title">Adjust before scaling</span>소스·검토 오너·지원 역량·거버넌스 같은 특정 장벽 먼저 해결</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">✋</span><span class="oa-card__title">Hold expansion</span>증거·준비·오너십·거버넌스·지원이 너무 약하면 보류</div>
</div>

### 최종 권고 예시

"Middleton은 교육·매니저 검토 기대·챔피언 커버리지·승인 코호트가 확인된 **뒤에만** 절차 요약 워크플로를 **운영 지역 하나 더**로 확장한다. 워크스페이스·거버넌스 오너는 출시 전 필요한 제품·접근·행동 통제를 확인한다. **안전사고 요약과 고객 대면 응답 초안은 자체 준비 리뷰를 마칠 때까지 범위 밖**. 2주 체크포인트에서 배포 오너는 합의된 도입·출력 품질 증거와 지원 적체·반복 소스 이슈·지연된 검토를 함께 본다. 지원·검토가 따라오지 못하면 다음 확장을 늦추되, 질문 수만으로 판단하지 않는다."

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 보통 가장 강한 첫 확장 우선순위 후보는?</summary>
  <p><b>정답:</b> <b>비즈니스 관련성이 분명한 유사 워크플로나 인접 팀.</b> 관심을 보였으니 전 직원 롤아웃 ✗, 장기 야망이 큰 완전히 새로운 고객 대면 워크플로 ✗, 소스·검토·지원 요건을 확인 안 한 채 접근만 요청한 팀 ✗.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 여러 팀에 걸친 확장을 가장 잘 지원하는 오너십 모델은?</summary>
  <p><b>정답:</b> <b>확장 영역마다 명확한 비즈니스 오너십, 롤아웃 조정, 챔피언 리더십</b>이 있다. 스폰서가 발표만 하고 팀이 알아서 ✗, 교육을 마쳤으니 오너 불필요 ✗, 파트너가 모든 결정을 계속 소유 ✗.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 팀이 늘어날 때 도입을 지속시키는 지원 방식은?</summary>
  <p><b>정답:</b> <b>반복 가능한 워크플로 가이드, 셀프서비스 자료, 명확한 에스컬레이션 경로.</b> 출시 웨비나 1회, 성공 사례만 공유, 챔피언에게 모든 이슈 떠넘기기는 ✗.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 빨리 확장하고 싶지만 지원 질문 증가, 팀별 검토 기대 차이, 피드백 수집이 들쭉날쭉하다면?</summary>
  <p><b>정답:</b> 더 넓히기 전에 <b>지원·검토·피드백 리스크를 파악</b>한다. 영구 중단 ✗, 참여 신호라며 질문 무시 ✗, 열기 있을 때 빨리 확장 ✗.</p>
</details>

<details class="oa-quiz">
  <summary>Q5. ChatGPT Organizational Scaling Strategy의 핵심 구성요소를 가장 잘 나타낸 묶음은?</summary>
  <p><b>정답:</b> <b>확장 목표, 확장 우선순위, 롤아웃 접근, 다음 행동.</b> 직급순 접근 요청 목록, 라이선스 수·기능 목록·조달 일정·일반 교육 달력, 기술 아키텍처·모델 벤치마크·API 통합 계획은 해당 안 돼요.</p>
</details>

## 정리

- 확장은 "사용자 더 추가"가 아니라 **증거로 다음 확장 지점을 정하는 일**. 좌석 목표나 "전부 배포"에서 시작하지 않는다.
- 후보는 **관련성·준비도·실현 가능성·영향·거버넌스·관찰 가능성**으로 비교하고, 이미 성공한 것과 **가까운 것부터**.
- 확장할수록 오너십·거버넌스는 **이름으로, 보이게**. 제품 통제(그룹·앱·Agent·공유 연결), 오너 교체 시 연속성, 출력 접근까지.
- **분석(도입·작업 패턴)과 컴플라이언스 기록(감사·조사)은 분리**. 사용량은 품질·가치의 증거가 아니다.
- 지원은 **반복 가능하게**: 온보딩·재사용 가이드·셀프서비스·질문 유형별 행선지. 챔피언 피로를 경계한다.
- 리스크·의존성은 오너·시점·볼 증거와 함께. 준비 안 된 선택적 워크플로는 분리해 보류하고, 질문 수는 활성 사용자 대비로 읽는다.
- 전략 = **우선순위 + 오너십·거버넌스 + 지원 + 리스크 + 다음 행동·체크포인트**. 단계적으로, 관찰하며 넓힌다.
