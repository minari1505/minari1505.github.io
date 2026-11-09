---
title: "Activate, Measure, and Scale"
title_ko: "활성화·측정·확장 — 증거로 '계속·조정·확장·중단'을 정하기"
course: chatgpt-deployment
lesson: 6
---

## 학습 목표

- **활성화 결과**를 계획과 비교해 평가하기
- **도입·참여 신호**를 판단하기
- 워크플로 가치와 결과 진척의 **초기 증거** 검토하기
- 배포 성공을 막는 **장벽과 근본 원인** 찾기
- **확장 기회**를 증거와 준비도로 평가하기
- **Activation Review and Expansion Plan**과 간결한 임원 보고(Executive readout) 만들기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>활성화 결과 평가</strong>계획 vs 실제 관찰</li>
  <li><strong>도입·가치 신호</strong>도입 · 성공 신호 · 초기 가치 · 한계</li>
  <li><strong>장벽·개선</strong>증상 → 근본 원인 → 조치</li>
  <li><strong>확장 평가</strong>준비도 · 후보 · 고려사항</li>
  <li><strong>Review &amp; Expansion Plan</strong>계속 · 조정 · 확장 · 중단</li>
</ol>

<div class="oa-eli">
  <span class="oa-eli__head">🌱 쉽게 말하면</span>
  <p>학교 텃밭에 새 비료를 한 줄에만 써 봤다고 해요. 한 달 뒤 할 일은 "좋은 것 같다!"고 외치며 밭 전체에 뿌리는 게 아니라, <b>① 계획대로 뿌렸는지 → ② 새싹이 실제로 자랐는지 → ③ 안 자란 곳은 왜인지 → ④ 옆 줄에도 써도 될지</b>를 차례로 보는 거예요. 그리고 "<b>계속 지켜보기 / 방법 고치기 / 옆 줄로 넓히기 / 잠시 멈추기</b>" 중 하나를 고르죠.</p>
</div>

입력으로는 User Enablement and Adoption Plan, Use Case Activation Plan, 승인된 고객 인계 자료, 비식별 고객 예시, 실습 예시 등을 써요. 목표는 관찰을 **분명한 추천**으로 바꾸는 것.

## 1. 활성화 결과 평가

첫 질문은 단순해요: **승인된 워크플로가 계획대로 출시됐나?** 승인됐다고 의도대로 쓰였다고 가정하지 않아요. 로그인·접근 활동 너머를 봐요.

### 활성화 활동 6가지 점검 영역

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧩</span><span class="oa-card__title">활성화된 승인 워크플로</span>"운영에 ChatGPT" ✗ → "지역 운영 매니저의 승인 SOP 기반 절차 요약" ✓</div>
  <div class="oa-card"><span class="oa-card__icon">👥</span><span class="oa-card__title">대상 사용자 참여</span>교육 참석, 오피스아워, 워크플로 사용, 피드백, 첫 사용 과제</div>
  <div class="oa-card"><span class="oa-card__icon">1️⃣</span><span class="oa-card__title">첫 사용 완료</span>첫 갱신 브리프·절차 요약·검토된 고객 응답 등을 만들었나</div>
  <div class="oa-card"><span class="oa-card__icon">👀</span><span class="oa-card__title">매니저·챔피언 관찰</span>숫자로 안 보이는 자신감·혼란·회의·즉흥 사용</div>
  <div class="oa-card"><span class="oa-card__icon">🆘</span><span class="oa-card__title">지원 이슈</span>참여의 증거이자 보강이 필요한 곳의 신호</div>
  <div class="oa-card"><span class="oa-card__icon">🗓️</span><span class="oa-card__title">타임라인 진척</span>교육·첫 사용·검토·피드백·결정 회의가 제때?</div>
</div>

### 계획 vs 관찰 비교

| 계획 | 관찰 | 의미할 수 있는 것 |
|---|---|---|
| 승인 SOP로 절차 요약 | 쓰긴 하지만 많은 사람이 **비공식 메모**를 붙여넣음 | 참여는 OK, **소스 가이드 보강** 필요 |
| 챔피언이 첫 사용 후 피드백 수집 | 공식 채널 대신 **사적 대화**로 들어옴 | 피드백은 있지만 경로 불명확 |
| 매니저가 출력 샘플 검토 | 아주 작은 샘플만 검토 | 사람 검토 **오너십** 명확화 필요 |
| 첫 2주 내 워크플로 완료 | 완료했지만 **오피스아워 도움 후에야** | 가능하지만 라이브 지원 의존도 높음 |

> 사용량이 높다고 **자동 성공이 아니고**, 낮다고 **자동 실패도 아니에요**. 핵심은 관찰이 의도한 워크플로에 대한 **쓸 만한 증거**를 만드느냐. 무엇이 맞았나 · 무엇이 바뀌었나 · 무엇이 놀라웠나 · 결정 전 무엇을 봐야 하나 · 어떤 증거가 행동할 만큼 강하고, 어떤 건 아직 약한가?

### 핵심 관찰 기록 — 6개 범주

긍정 지표 · 예상 밖 행동(인접 워크플로 사용, 미승인 소스, 검토 생략, 챔피언 과의존) · 떠오르는 과제 · 맥락·소스 품질 이슈 · 검토·승인 이슈 · 거버넌스·접근 우려(꼭 중단할 필요는 없지만 **오너와 후속 조치**는 필요).

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 관찰</span>
    <ul><li>"사용자들이 혼란스러워해요."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 관찰</span>
    <ul><li>"사용자들은 절차 요약 워크플로를 완료하지만, 여럿이 <b>어떤 SOP가 승인됐는지</b> 확신하지 못한다. 유망하지만 다른 지역으로 확장하기 전 소스 가이드를 명확히 해야 한다."</li></ul>
  </div>
</div>

### 사례: Churchill Manufacturing (가상)

글로벌 제조·산업 서비스 기업. 지역 운영 매니저가 승인 SOP·운영 정책으로 **사람이 검토하는 절차 요약**을 만드는 워크플로를 활성화했어요.

<div class="oa-bars">
  <div class="oa-bar"><span>교육 참석 (목표 45)</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:87%"></span><span class="oa-bar__val">52 / 60</span></span></div>
  <div class="oa-bar"><span>워크플로 1회 완료 (목표 35)</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:65%"></span><span class="oa-bar__val">39 / 60</span></span></div>
  <div class="oa-bar"><span>2회 이상 반복</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:47%"></span><span class="oa-bar__val">28 / 60</span></span></div>
  <div class="oa-bar"><span>승인 SOP 가이드 사용</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:40%"></span><span class="oa-bar__val">24 / 60</span></span></div>
  <div class="oa-bar"><span>피드백 폼 제출</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:23%"></span><span class="oa-bar__val">14 / 60</span></span></div>
  <p class="oa-bars__caption">강의 사례(첫 코호트 60명)의 수치를 코호트 대비 비율로 나타냈어요. 가상 시나리오의 숫자예요.</p>
</div>

그 밖의 관찰: 매니저가 검토한 요약 **12개 중 9개가 가벼운 수정으로 유용** · 어떤 SOP가 승인됐는지 반복 질문 · 질문 일부가 공유 채널 대신 개인 메시지로 · 한 지역 매니저가 **안전사고 요약**으로 확장 가능한지 문의.

## 2. 도입 신호와 초기 가치

<div class="oa-eli">
  <span class="oa-eli__head">🎸 쉽게 말하면</span>
  <p>기타 학원에 등록(접근)했다고 기타를 배우는 게 아니에요. <b>매주 다시 와서(반복 사용)</b>, <b>실제 노래를 칠 때(업무 순간)</b> 쓰고, 막히면 선생님께 묻는 게 진짜 '배우는 중'의 신호죠. 그리고 "한 곡을 끝까지 칠 수 있게 됐다"가 가치의 신호예요.</p>
</div>

### 네 가지를 구분하기

| 구분 | 무엇을 보나 | Churchill 예시 |
|---|---|---|
| **도입 지표** | 대상 사용자가 승인 워크플로에 **참여**하나 | 28명 반복 사용, 신규 매니저의 오피스아워 참석 |
| **성공 신호** | 합의한 실무 지표 — 산출물·품질·검토 완료·자신감 | 최고 요약은 "가벼운 수정으로 사용 가능", 승인 SOP 쓴 사람의 자신감↑ |
| **초기 가치 증거** | 노력·재작업 감소, 일관성·유용성 향상 (방향성) | "요약을 더 빨리 시작한다"는 사용자 보고 |
| **증거의 한계** | 증거가 증명하지 **못하는** 것 | 사전 시간 기준선 없음, 피드백 14명뿐, 지역별 안전 노트 누락 |

### 도입 신호

대상 사용자 참여 · 승인 워크플로 **반복 사용** · 교육·지원 참여 · 매니저·챔피언 피드백 · 자신감·정서 · **의도한 업무 순간에 사용**. 마지막이 특히 중요해요 — 일반 질문만 하고 있다면 친숙해지는 중이지, 승인 워크플로가 도입된 건 아니에요. 숫자(참여·반복·완료율)와 관찰(자신감·혼란·적합성)을 함께 봐요.

### 성공 신호

기대 산출물 · 품질·유용성 · 마찰·노력 감소 · 자신감 · 고객 승인 가치 지표 · 출력 검토 완료 · 적절한 사람 검증 · 개선에 쓸 피드백.

> **위임·예약·행동형 경험**(예: ChatGPT Work, 강의 기준)이라면 재방문 빈도 이상을 봐요: 합의한 완료 상태 도달, 사용 가능·검토 가능한 산출물, 승인 맥락·도구 사용, 검토·승인 단계 준수, **실패한 행동·예외**가 합의된 지원 경로로 올라왔는지.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 성공 신호</span>
    <ul><li>"사용자들이 ChatGPT를 좋아해요."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 성공 신호</span>
    <ul><li>"운영 매니저가 <b>수작업 종합 노력을 줄여</b> 1차 절차 요약을 만들고, 필수 소스 확인과 매니저 검토를 완료한다."</li></ul>
  </div>
</div>

### 초기 가치 증거 — 방향성으로, 한계와 함께

방향성 효율 신호 · 노력·재작업 감소 · 일관성 향상 · 출력 유용성 향상 · **증거의 한계·가정**. 초기 증거는 작은 그룹, 짧은 기간, 자기 보고, 제한된 샘플에서 오는 경우가 많으니 **과장하지 말고 한계를 적어요.**

**실제 사례 (강의 기준):** 한 항공사 디지털 제품팀이 ChatGPT Work로 고객 여정 전반의 경쟁사 경험을 조사해 검토용 데이터셋으로 정리했고, 팀이 평가·보완한 뒤 5개년 전략 권고에 활용했어요. "몇 주 걸리던 조사가 몇 시간으로"라는 보고였지만, 결과는 **검토를 위한 구조화된 출발점**이지 자동 승인된 전략이 아니었어요.

<div class="oa-eli">
  <span class="oa-eli__head">⏱️ 시간 절약을 셀 때 주의</span>
  <p>라면 끓이는 시간이 3분이라고 "저녁 준비 3분"은 아니죠 — 장보기·설거지도 있어요. 마찬가지로 <b>첫 결과를 만드는 시간</b>과 <b>검토·검증·사용까지의 총 시간</b>을 구분하고, 고객의 실제 워크플로에서 품질과 총 노력을 재야 확장 주장을 할 수 있어요.</p>
</div>

## 3. 장벽과 개선 기회

결과가 섞여 나와도 **실패가 아닐 수 있어요.** 더 명확한 가이드, 소스 지침, 매니저 강화, 눈에 보이는 지원 경로, 검토 프로세스가 필요하다는 신호일 수 있죠.

### 흔한 배포 장벽

| 장벽 | 모습 |
|---|---|
| 낮은 인지 | 워크플로가 있는지, 왜 중요한지, 어디서 시작할지 모름 |
| 워크플로 혼란 | ChatGPT는 알지만 승인 워크플로 적용법(프롬프트 패턴·소스·형식·검토)을 모름 |
| 지원 부족 | 질문이 DM·이메일·회의로 흩어짐 |
| 이해관계자 과제 | 매니저·챔피언·오너가 일관되게 강화하지 않음 |
| 자원 제약 | 시간·지원 인력·검토 역량·소스 오너십 부족 |
| 맥락·검토·가시성 | 승인 맥락 사용? 소스 명확? 영향 전 검토? 이슈가 오너에게 보이나? 피드백 루프? |

### 증상 → 근본 원인 → 조치

<div class="oa-eli">
  <span class="oa-eli__head">🤒 쉽게 말하면</span>
  <p>열(증상)이 난다고 항상 감기(원인)는 아니에요. 같은 열도 원인에 따라 약이 달라요. "<b>반복 사용이 낮다</b>"는 증상도 매니저 강화 부족, 불명확한 지침, 나쁜 소스 품질, 낮은 자신감, 믿지 못하는 지원 경로 등 원인이 여러 가지예요. <b>원인을 증거로 확인한 뒤</b> 조치를 고르세요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📘</span><span class="oa-card__title">Enablement 공백</span>단계·검토 기대·지원처를 모름, 업무와 가까운 예시를 못 봄</div>
  <div class="oa-card"><span class="oa-card__icon">🔧</span><span class="oa-card__title">워크플로 문제</span>수작업 단계 과다, 출력 형식 불일치, 찾기 어려운 소스, 업무 순간 밖에서 사용 요구</div>
  <div class="oa-card"><span class="oa-card__icon">🆘</span><span class="oa-card__title">지원 한계</span>질문이 흩어짐, 챔피언에 라우팅 권한 없음, 지원 수요 폭증, 거버넌스·소스 에스컬레이션 경로 부재</div>
</div>

| 원인 | 개선 조치 |
|---|---|
| 워크플로를 모름 | 커뮤니케이션 강화 |
| ChatGPT는 알지만 승인 워크플로는 모름 | 더 명확한 워크플로 가이드 |
| 적용 자신감 부족 | 표적 enablement 조정 |
| 질문이 흩어진 채널로 | 지원 경로 명확화 |
| 매니저가 검토 기대를 강화 안 함 | 챔피언·매니저 지원 |
| 소스가 낡았거나 불명확·미준비 | **소스 오너 확인, 승인 소스 세트 갱신, 확장 전 준비 이슈 에스컬레이션** |
| 출력 품질 편차 | 가이드·소스 명확화·검토 기준 보정 |

> ⚠️ 모든 문제를 enablement 문제로 보지 마세요. 소스가 낡아서 품질이 낮다면 교육을 한 번 더 해도 해결 안 돼요. 출력은 나오는데 아무도 검토하지 않는다면 그건 도입 문제가 아니라 **검토 오너십** 문제예요.

좋은 개선 권고 = **장벽 + 추정 원인 + 조치 + 오너 + 다음에 볼 증거**. 예: "소스 사용 혼란이 품질에 영향 → 원인은 불명확한 승인 소스 가이드 → 배포 오너가 승인 SOP 세트 확정, 잡 에이드 갱신, 매니저 승인 요약 샘플 재검토 후 2번째 지역 확장."

## 4. 확장 기회 평가

확장은 **열정이 아니라 증거**로. 판단 테스트 하나:

> **지금 확장하면 워크플로가 더 가치 있고 관리 가능해질까, 아니면 미해결 문제를 곱절로 늘릴까?**

### 확장 준비도 요인

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📈</span><span class="oa-card__title">도입 지표</span>대상자가 승인 워크플로를 반복 사용?</div>
  <div class="oa-card"><span class="oa-card__icon">💡</span><span class="oa-card__title">초기 가치 증거</span>노력·품질·일관성 등에서 신뢰할 만한 개선?</div>
  <div class="oa-card"><span class="oa-card__icon">🤝</span><span class="oa-card__title">이해관계자 지원</span>오너·매니저·챔피언·거버넌스 지지</div>
  <div class="oa-card"><span class="oa-card__icon">🧯</span><span class="oa-card__title">지원 역량</span>더 많은 사용자를 감당 가능?</div>
  <div class="oa-card"><span class="oa-card__icon">⚠️</span><span class="oa-card__title">미해결 리스크</span>이름·오너·에스컬레이션 경로 있음?</div>
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">다음 대상 준비도</span>교육·소스 접근·검토 오너·거버넌스 승인 필요?</div>
  <div class="oa-card"><span class="oa-card__icon">🔭</span><span class="oa-card__title">운영 가시성</span>활동·완료/실패 작업·승인 결과·피드백·예외가 보이나</div>
  <div class="oa-card"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">거버넌스 실효성</span>검토·허용 사용·소스·에스컬레이션이 실제로 지켜지나</div>
</div>

완벽할 필요는 없어요. 무엇이 되고, 무엇이 남았고, 다음 단계에 어떤 **조건**이 필요한지 이해하면 돼요.

### 확장 후보 — 성공한 것과 가까울수록 안전

<div class="oa-svg">
<svg viewBox="0 0 720 200" role="img" aria-label="성공 워크플로와의 거리에 따른 확장 후보 위험도">
  <defs>
    <marker id="ar6a" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.g6a{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.q6a{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.w6a{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.t6a{font-size:12px;fill:#4a4a4a;font-weight:bold}.s6a{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="30" width="165" height="110" rx="10" class="g6a"/><text x="92" y="56" text-anchor="middle" class="t6a">추가 사용자</text><text x="92" y="78" text-anchor="middle" class="s6a">같은 팀·역할·지역</text><text x="92" y="100" text-anchor="middle" class="s6a">예: 다른 운영 지역,</text><text x="92" y="116" text-anchor="middle" class="s6a">같은 SOP·검토</text>
  <rect x="190" y="30" width="165" height="110" rx="10" class="g6a"/><text x="272" y="56" text-anchor="middle" class="t6a">추가 팀</text><text x="272" y="78" text-anchor="middle" class="s6a">비슷한 업무·맥락</text>
  <rect x="370" y="30" width="165" height="110" rx="10" class="q6a"/><text x="452" y="56" text-anchor="middle" class="t6a">추가 워크플로</text><text x="452" y="78" text-anchor="middle" class="s6a">같은 소스·검토 경로·</text><text x="452" y="94" text-anchor="middle" class="s6a">출력 패턴 재사용</text>
  <rect x="550" y="30" width="160" height="110" rx="10" class="w6a"/><text x="630" y="56" text-anchor="middle" class="t6a">추가 비즈니스 기능</text><text x="630" y="78" text-anchor="middle" class="s6a">준비 점검 필수</text><text x="630" y="100" text-anchor="middle" class="s6a">예: 고객 대면</text><text x="630" y="116" text-anchor="middle" class="s6a">사고 대응 = 고위험</text>
  <line x1="20" y1="170" x2="700" y2="170" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6a)"/>
  <text x="20" y="190" class="s6a">성공 워크플로와 가까움 · 낮은 위험</text><text x="700" y="190" text-anchor="end" class="s6a">새 데이터 경계·검토·리스크 오너 · 높은 위험</text>
</svg>
</div>

후보 체크 질문: 이미 성공한 것과 비슷한가 · 대상에게 분명한 도입 이유가 있나 · 소스가 승인·준비됐나 · 검토 경로가 명확한가 · 지원 역량이 있나 · 거버넌스 기대를 이해하나 · 사용·피드백·이슈·개선을 모니터링할 수 있나.

확장 고려사항: 지원 요구 · 자원 요구 · 미해결 의존성(소스·접근·거버넌스·검토·보고·승인) · 거버넌스·승인 필요.

### 네 가지 결정

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">▶️</span><span class="oa-card__title">Continue (계속)</span>유망 — 운영을 유지하며 증거를 더 모음</div>
  <div class="oa-card"><span class="oa-card__icon">🔧</span><span class="oa-card__title">Adjust (조정)</span>잠재력 있지만 확장 전 특정 문제를 고쳐야 함</div>
  <div class="oa-card"><span class="oa-card__icon">📈</span><span class="oa-card__title">Expand (확장)</span>도입·가치·준비도·지원·거버넌스·가시성이 충분하고 남은 리스크는 오너가 있고 관리 가능</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⏸️</span><span class="oa-card__title">Pause (중단)</span>블로커·리스크·약한 증거·오너 공백 때문에 계속·확장이 무책임</div>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 권고</span>
    <ul><li>"도입이 좋으니 확장하자."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 권고</span>
    <ul><li>"<b>확장 전 조정.</b> 반복 사용과 긍정적 매니저 피드백으로 도입은 유망하지만, 승인 소스가 아직 불명확하다. 소스 가이드를 명확히 하고 잡 에이드를 갱신한 뒤, 매니저 승인 출력 샘플을 한 번 더 검토하고 2번째 지역을 추가한다."</li></ul>
  </div>
</div>

**Churchill의 확장 압박 사례:** 스폰서는 분기 비즈니스 리뷰 전에 다음 달 2번째 지역 확장을 원해요. 반복 사용 증가, 승인 SOP 세트 정리 완료라는 좋은 신호가 있지만 — 시간 기준선 없음, 지원 질문 증가, **2번째 지역 교육 미실시**, 지역 거버넌스 오너가 검토 기대 확인을 원함. → 이런 조건이면 바로 확장보다 **조정 후 조건부 확장**이 증거에 맞는 결정이에요.

## 5. Activation Review and Expansion Plan

<div class="oa-svg">
<svg viewBox="0 0 720 300" role="img" aria-label="활성화 증거에서 결정으로 가는 논리">
  <defs>
    <marker id="ar6b" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.q6b{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.g6b{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.w6b{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.n6b{fill:#f6f3ea;stroke:#e5dfcf;stroke-width:1.5}.t6b{font-size:12px;fill:#4a4a4a;font-weight:bold}.s6b{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="230" y="10" width="260" height="44" rx="10" class="n6b"/><text x="360" y="37" text-anchor="middle" class="t6b">활성화 증거 검토</text>
  <line x1="360" y1="54" x2="360" y2="74" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6b)"/>
  <rect x="150" y="78" width="420" height="44" rx="10" class="q6b"/><text x="360" y="98" text-anchor="middle" class="t6b">블로커·오너 공백·거버넌스 우려가 있나?</text><text x="360" y="114" text-anchor="middle" class="s6b">예 → Pause</text>
  <line x1="360" y1="122" x2="360" y2="142" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6b)"/>
  <rect x="150" y="146" width="420" height="44" rx="10" class="q6b"/><text x="360" y="166" text-anchor="middle" class="t6b">확장 전 고쳐야 할 특정 문제가 있나?</text><text x="360" y="182" text-anchor="middle" class="s6b">예 → Adjust</text>
  <line x1="360" y1="190" x2="360" y2="210" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6b)"/>
  <rect x="150" y="214" width="420" height="44" rx="10" class="q6b"/><text x="360" y="234" text-anchor="middle" class="t6b">도입·가치·준비도·지원·가시성이 다음 코호트를 뒷받침?</text><text x="360" y="250" text-anchor="middle" class="s6b">예 → Expand · 아직 → Continue (증거 더 모으기)</text>
  <rect x="590" y="78" width="120" height="44" rx="10" class="w6b"/><text x="650" y="105" text-anchor="middle" class="t6b">⏸️ Pause</text>
  <line x1="570" y1="100" x2="586" y2="100" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6b)"/>
  <rect x="590" y="146" width="120" height="44" rx="10" class="q6b"/><text x="650" y="173" text-anchor="middle" class="t6b">🔧 Adjust</text>
  <line x1="570" y1="168" x2="586" y2="168" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6b)"/>
  <rect x="590" y="214" width="120" height="44" rx="10" class="g6b"/><text x="650" y="241" text-anchor="middle" class="t6b">📈 Expand</text>
  <line x1="570" y1="236" x2="586" y2="236" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6b)"/>
  <rect x="10" y="214" width="120" height="44" rx="10" class="n6b"/><text x="70" y="241" text-anchor="middle" class="t6b">▶️ Continue</text>
  <line x1="150" y1="236" x2="134" y2="236" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6b)"/>
  <text x="360" y="285" text-anchor="middle" class="s6b">개념적 표현 — 강의의 결정 논리를 단순화한 흐름이에요</text>
</svg>
</div>

| 섹션 | 담을 것 |
|---|---|
| **활성화 결과** | 핵심 관찰, 성공 지표, 주의할 영역 — 짧지만 구체적으로 |
| **배포 발견** | 도입 관찰, 초기 가치 증거, 개선 기회, 검토·모니터링 관찰, 맥락·접근·거버넌스 이슈 — **아는 것과 방향성인 것 구분** |
| **확장 권고** | 확장 기회, 리스크·의존성, 결정과 이유, **오너와 가까운 결정 시점** |
| **임원 보고** | 승인 워크플로·상태, 가장 강한 증거, 주요 한계·리스크, 결정, 다음 행동·오너·리뷰 시점 |

### 가치를 과장하지 않는 발견 문장

"사용자들은 초안이 빨라졌다고 하고, 매니저는 검토한 12개 중 9개를 가벼운 수정으로 유용하다고 봤다. **시간 기준선이 없으므로 측정된 시간 절약은 아니다.** 다음 리뷰에서 워크플로 오너가 비슷한 과제를 현재 방식과 승인 워크플로로 비교하되, 같은 품질 기준으로 준비·소스 확인·검토·재작업까지 포함해 재고, 표본과 측정 방법을 기록한다. 출력 유용성은 매니저 피드백으로 따로 평가한다."

### 오너가 있는 권고 예시

"권고: **확장 전 조정.** 고객 배포 오너는 검토 기대 확정, 운영 워크플로 오너는 SOP 가이드 갱신, 챔피언 리드는 지원 경로 강화, 파트너 배포 리드는 2번째 지역 추가 전 **2주 뒤 증거 리뷰** 일정 확정."

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 활성화가 예상대로 진행 중임을 가장 잘 보여주는 지표는?</summary>
  <p><b>정답:</b> <b>대상 사용자가 승인된 워크플로에 ChatGPT를 적용하고 있다.</b> 많은 사람이 한 번 로그인, 임원의 "느낌이 좋다", 계획보다 많은 교육 자료 제작은 활성화 증거가 아니에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 도입 지표와 초기 가치 증거를 가장 잘 구분한 설명은?</summary>
  <p><b>정답:</b> 도입 지표는 사용자가 <b>승인 워크플로에 참여하는지</b>를 보여준다(가치 증거는 그 워크플로가 유용한 결과를 내기 시작하는지). "도입=정량, 가치=정성", "도입이 ROI 증명", "전사 롤아웃 후에만 유용"은 틀려요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. HR 정책 요약 워크플로에 관심은 높지만 어떤 정책을 참조해도 되는지 계속 묻는다면?</summary>
  <p><b>정답:</b> <b>승인 소스 세트를 명확히 하고, 워크플로 가이드를 갱신하고, 소스 오너를 확인</b>한다. 관심 높으니 확장 ✗, 질문 있다고 전체 중단 ✗, 로그인만 늘리기 ✗.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 확장 권고에 가장 큰 영향을 줘야 하는 요인은?</summary>
  <p><b>정답:</b> <b>도입·초기 가치 증거·준비도가 다음 코호트를 뒷받침하는지.</b> 고객이 빨리 하고 싶은지, 새 팀이 접근을 요청했는지, 장기 가치가 최대인지는 기준이 아니에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q5. Activation Review and Expansion Plan의 핵심 구성요소를 가장 잘 나타낸 묶음은?</summary>
  <p><b>정답:</b> <b>임원 보고, 활성화 결과, 도입·초기 가치 증거, 장벽, 리스크, 다음 행동.</b> 소스 목록·접근 티켓(셋업), 교육 일정·공지·챔피언 명단(도입 계획), 사용량 스냅샷·기능 아이디어는 해당 안 돼요.</p>
</details>

## 정리

- 활성화 리뷰는 **계획 vs 관찰**을 나눠 사실 기반을 만든다. 사용량이 높다고 성공, 낮다고 실패가 아니다.
- 관찰은 증거와 의미를 연결해 **구체적으로**: 긍정·예상 밖·과제·소스·검토·거버넌스.
- **도입 지표 · 성공 신호 · 초기 가치 · 증거 한계**를 구분하고, 방향성 증거는 방향성으로만 말한다. 시간 절약은 검토·검증 포함 총 노력으로.
- 장벽은 **증상**, 원인을 증거로 찾은 뒤 조치. 모든 문제가 교육 문제는 아니다(소스·검토 오너십).
- 확장은 증거로: "지금 넓히면 가치가 커질까, 문제가 곱절이 될까?" 성공한 것과 가까운 후보부터.
- 결정은 **Continue · Adjust · Expand · Pause** 중 하나로, 오너·시점·다음에 볼 증거와 함께. 임원 보고는 한눈에 결정이 보이게.
