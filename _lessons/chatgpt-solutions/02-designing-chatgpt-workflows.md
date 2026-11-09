---
title: "Designing ChatGPT Workflows"
title_ko: "ChatGPT 워크플로우 설계 — 한 번의 작업을 반복 가능한 업무 흐름으로"
course: chatgpt-solutions
lesson: 2
---

## 학습 목표

- **일회성 작업(task)**과 **반복 가능한 워크플로우(workflow)**를 구분하기
- 고객의 작업을 사용자·트리거·입력·단계·결정·결과물·목적이 있는 **워크플로우 지도**로 그리기
- ChatGPT가 맡는 일을 **prompt-to-output 패턴**(입력 → AI 작업 → 결과물 → 사람 검토)으로 설명하기
- **반복성**과 **도입 가능성**으로 먼저 탐색할 워크플로우 고르기
- 4~6문장짜리 **워크플로우 추천문** 쓰기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>작업 → 워크플로우</strong>"이메일 써 줘" 뒤의 반복 업무 찾기</li>
  <li><strong>워크플로우 지도</strong>8가지 질문 + 마찰 지점 + 맥락</li>
  <li><strong>Prompt-to-output 패턴</strong>입력 → AI 작업 → 결과물 → 검토</li>
  <li><strong>우선순위</strong>반복성 4체크 + 도입 4체크</li>
  <li><strong>추천문</strong>무엇을 먼저, 왜, 다음 검증은?</li>
</ol>

## 1. 작업(task)이 아니라 워크플로우(workflow)로 보기

<div class="oa-eli">
  <span class="oa-eli__head">🍳 쉽게 말하면</span>
  <p>"계란 깨기"는 <b>작업</b>이고, "아침 식사 준비하기"는 <b>워크플로우</b>예요. 계란 깨는 걸 1초 빠르게 해도 별 차이 없지만, 아침 준비 전체(장보기 목록 → 재료 준비 → 요리 → 상차림)를 매일 더 매끄럽게 만들면 큰 차이가 나죠.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🔹 Task (작업)</span>
    <ul>
      <li>한 번의 행동</li>
      <li>"후속 이메일 써 줘"</li>
      <li>몇 분 절약</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🔗 Workflow (워크플로우)</span>
    <ul>
      <li>사용자·입력·단계·결과물·목적이 있는 일의 흐름</li>
      <li>"미팅 후 후속 패키지 준비": 통화 메모 검토 → 고객 약속 정리 → 메일 초안 → 다음 단계 업데이트 → 다음 미팅 준비</li>
      <li>속도·일관성·검토 품질·팀 정렬이 계속 개선</li>
    </ul>
  </div>
</div>

### 탐색할 가치가 있는지 보는 6가지 질문

| # | 질문 | 예시 / 포인트 |
|---|---|---|
| 1 | **누가** 일하나? | 작업이 아니라 사용자 그룹 (주간 보고 준비하는 운영 매니저) |
| 2 | **어떤 단계가 반복**되나? | 메모 수집 → 주제 찾기 → 우선순위 비교 → 초안 → 위험 검토 → 공유 |
| 3 | **어떤 맥락**이 필요한가? | 원자료·고객 이력·정책·템플릿. 기술 설정은 아직 아님 |
| 4 | **어떤 결과물**이 나아져야 하나? | 더 빠르게? 더 완전하게? 더 일관되게? |
| 5 | **거버넌스** 고려사항은? | 승인된 자료, 민감 정보, 검토자, 접근 경계 |
| 6 | **성공을 어떻게** 잴까? | 준비 시간 감소, 재작업 감소 (완전한 ROI 모델은 아직 아님) |

계속되는 일이라면 **누가 이끄는지**(개인 vs 팀), 어디서 검토·에스컬레이션하는지, 결과물을 누가 최신으로 유지하는지도 확인해요.

## 2. 워크플로우 지도 그리기

<div class="oa-eli">
  <span class="oa-eli__head">🗺️ 쉽게 말하면</span>
  <p>택배 추적 화면을 떠올려 보세요. "접수 → 집하 → 이동 → 배송 출발 → 도착". 어디서 오래 멈췄는지 한눈에 보이죠. 워크플로우 지도도 <b>일이 어디서 시작해 어디로 흘러가고, 어디서 막히는지</b>를 보여줘요.</p>
</div>

### 8가지 칸 — "지원 티켓 요약해 줘"의 예

| 칸 | 질문 | 예시 (고객성공 매니저) |
|---|---|---|
| **User** | 누가 하고, 계속 책임지나? | 고객성공 매니저 |
| **Work object** | 무엇이 흘러가나? | 갱신 위험 브리핑 |
| **Trigger** | 무엇이 시작시키나? | 다가오는 임원 계정 리뷰 |
| **Inputs** | 어떤 정보가 필요한가? | 지원 티켓, 계정 메모, 사용 추세, 갱신 이력, 고객 목표 |
| **Steps** | 지금은 어떻게 하나? | 정보 찾기 → 읽기 → 주제 파악 → 초안 → 정확성 확인 |
| **Decisions/actions** | 무엇을 결정·승인·전달? | 리더십에 올릴 위험·기회 선정, 다음 단계 확정 |
| **Output** | 무엇이 나오고, 누가 보나? | 위험·기회·추천 다음 단계가 담긴 리뷰용 요약 |
| **Business purpose** | 왜 중요한가? | 더 빠른 준비, 더 일관된 갱신 대화 |

반복 업무라면 "평소 담당자가 휴가면 **누가 대신하고 연결은 유지되나**?"를 물어 개인 책임인지 팀 프로세스인지 구분해요.

### 마찰 지점(friction) 찾기

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔀</span><span class="oa-card__title">흩어진 자료</span>연결 안 된 여러 출처를 오가야 함</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🧶</span><span class="oa-card__title">지저분한 정보 정리</span>엉킨 정보를 쓸 수 있는 형식으로 바꾸는 반복 작업</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🎲</span><span class="oa-card__title">품질 편차</span>같은 일인데 누구는 완벽, 누구는 위험·다음 행동 누락</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🧑‍🔬</span><span class="oa-card__title">전문가 의존</span>소수 전문가가 복잡한 지식을 번역해 줘야 함</div>
</div>

> "브리핑 준비가 너무 오래 걸려요" = "여러 곳에서 정보를 모아, 시간 압박 속에 종합해서, 리더십용으로 일관되게 만들어야 해요" → **이게 워크플로우 기회**예요.

### 맥락 더하기 — 같은 "문서 요약"도 다르다

| 맥락 | "문서 요약해 줘"의 진짜 의미 | 거버넌스 포인트 |
|---|---|---|
| 법무 | 변호사 검토용 1차 계약 요약 | 어떤 계약 데이터를 써도 되나, 공유 전 검토 단계 |
| 고객성공 | 계정 메모·지원 이력으로 갱신 브리핑 | 계정 데이터 접근, 리더십 미팅 전 사람 검토 |
| 공공 | 정책 문서 요약 | 정확성·접근성·승인. 속도만이 성공 지표가 아님 |

맥락 체크: 사업 부서 · 업종 · 사용자 그룹 · 원자료 · 거버넌스 + 성공 신호(측정 가능한 결과, 모니터링 신호).

<div class="oa-eli">
  <span class="oa-eli__head">⚠️ 착각하기 쉬운 것 (강의 기준)</span>
  <p>• <b>로컬 실행 ≠ 데이터가 기기에만 머묾</b>, <b>파일·도구 접근 ≠ 모든 행동 허가</b>.<br>• <b>Space</b>의 공유 페이지는 독자에게 페이지 안 내용·업로드 파일을 보여주지만, 링크된 원본은 원래 권한을 유지해요. 페이지를 공유한다고 <b>누가 최신으로 유지할지</b>가 정해지진 않아요.<br>• <b>ChatGPT Sites</b>는 방문자의 연결 앱 정보를 방문 중에 읽을 수 있지만, 그 앱을 바꾸거나 예약된 백그라운드 작업을 하진 못해요.<br>• <b>MCP Events</b>는 연결 앱의 업데이트(새 메시지 등)가 작업을 시작시키게 할 수 있어요 → 어떤 업데이트에 무엇을 할지 정해야 해요.</p>
</div>

## 3. Prompt-to-output 패턴

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍🍳 쉽게 말하면</span>
  <p>"요리 잘 해요"보다 "<b>냉장고 재료(입력)</b>를 주면 <b>레시피대로 볶아서(AI 작업)</b> <b>도시락(결과물)</b>을 만들고, <b>엄마가 간을 봐요(검토)</b>"가 훨씬 구체적이죠. 고객에게 필요한 건 기능 목록이 아니라 이런 설명이에요.</p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 150" role="img" aria-label="Prompt-to-output 패턴">
  <defs>
    <marker id="c2a" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.c2q{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.c2g{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.c2w{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.c2t{font-size:13px;fill:#4a4a4a;font-weight:bold}.c2s{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="30" width="155" height="80" rx="10" class="c2q"/><text x="87" y="60" text-anchor="middle" class="c2t">입력 + 지시</text><text x="87" y="80" text-anchor="middle" class="c2s">예산·실적·임계값</text><text x="87" y="96" text-anchor="middle" class="c2s">담당자 메모</text>
  <rect x="190" y="30" width="155" height="80" rx="10" class="c2q"/><text x="267" y="60" text-anchor="middle" class="c2t">AI 작업 유형</text><text x="267" y="80" text-anchor="middle" class="c2s">비교 · 주요 차이 찾기</text><text x="267" y="96" text-anchor="middle" class="c2s">초안 작성</text>
  <rect x="370" y="30" width="155" height="80" rx="10" class="c2g"/><text x="447" y="60" text-anchor="middle" class="c2t">결과물</text><text x="447" y="80" text-anchor="middle" class="c2s">1차 차이 분석</text><text x="447" y="96" text-anchor="middle" class="c2s">코멘트</text>
  <rect x="550" y="30" width="160" height="80" rx="10" class="c2w"/><text x="630" y="60" text-anchor="middle" class="c2t">사람 검토</text><text x="630" y="80" text-anchor="middle" class="c2s">재무팀 확인 후</text><text x="630" y="96" text-anchor="middle" class="c2s">리더십에 공유</text>
  <line x1="165" y1="70" x2="186" y2="70" stroke="#c99400" stroke-width="1.5" marker-end="url(#c2a)"/>
  <line x1="345" y1="70" x2="366" y2="70" stroke="#c99400" stroke-width="1.5" marker-end="url(#c2a)"/>
  <line x1="525" y1="70" x2="546" y2="70" stroke="#c99400" stroke-width="1.5" marker-end="url(#c2a)"/>
  <text x="360" y="138" text-anchor="middle" class="c2s">예: 재무 매니저의 월간 차이(variance) 코멘트 준비</text>
</svg>
</div>

### 7가지 대표 패턴

| 패턴 | ChatGPT가 하는 일 | 쓸모 있는 상황 |
|---|---|---|
| **Summarize & synthesize** | 여러 자료 → 간결한 요약 | 문서·티켓·회의록을 읽느라 시간이 듦 |
| **Draft & refine** | 초안 → 어조·구조·완성도 다듬기 | 비슷한 문서를 반복 작성 |
| **Compare & recommend** | 선택지 비교 → 추천 | 대안·위험·트레이드오프 평가 |
| **Extract & structure** | 핵심 정보를 표·체크리스트·템플릿으로 | 지저분한 정보를 일정한 형식으로 |
| **Prepare & brief** | 배경 정보 → 미팅·계정·임원 브리핑 | 반복되는 회의 준비 |
| **Review & improve** | 기준·정책·품질에 맞춰 점검 | 기준은 있는데 검토가 느리고 들쭉날쭉 |
| **Guide & coach** | 단계별로 과정 안내 | 전문가 지도가 부족할 때 |

패턴은 섞일 수 있어요. 예: 가이드 개정 작업 = 비교·추천 + 초안·다듬기 + 검토·개선. 단, **파일 접근 권한이 게시·공유 권한을 뜻하진 않아요.**

### 패턴 문장 템플릿 3종

```text
[협업형 · Chat]
사용자가 [입력]을 주면 ChatGPT가 [작업 유형]을 돕고,
결과물은 [사용자/검토자]가 검토·사용할 [비즈니스 산출물]이다.

[위임형 · Work]
사용자가 [결과]를 정의하고 [맥락·경계]를 주면 ChatGPT가 [승인된 앱·도구]로
[여러 단계 작업]을 완료한다. [승인/에스컬레이션]에서 멈추고,
결과물은 [검토자]가 검증할 [산출물/행동]이다.

[지속형 확장]
[사람/팀]이 [지속 책임]을 관리한다. [일정/지원 이벤트]가 작업을 시작하고,
[허용된 자료]와 [승인된 행동]만 쓴다. [검토자]가 [승인/에스컬레이션]을 맡고,
[대상]은 [공유 결과물]을 보며, [담당자]가 [합의된 절차]로 최신 상태를 유지한다.
```

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">Chat + Intelligent UI (강의 기준)</span>응답 안 차트·계산기로 숫자를 바꿔 보며 분석. Instant~Extra High에서만, Work·Pro·Voice에선 안 됨 → 시연 전 요금제·설정·버전 확인</div>
  <div class="oa-card"><span class="oa-card__icon">📋</span><span class="oa-card__title">Work</span>입력·행동 한계·검토 기준을 합의하고 초안을 위임</div>
  <div class="oa-card"><span class="oa-card__icon">🔁</span><span class="oa-card__title">dot / Team Tasks (강의 기준)</span>보고 주기 사이에도 계속되는 일: 개인은 dot, 팀 반복 프로세스는 Team Tasks(일정·이벤트, 팀 서비스 계정)</div>
</div>

## 4. 반복 가능하고 도입될 워크플로우 고르기

<div class="oa-eli">
  <span class="oa-eli__head">🏃 쉽게 말하면</span>
  <p>운동 습관을 만들 때 "가끔 마라톤"보다 "매일 등굣길 걷기"가 오래가요. 자주 하고(반복성), 이미 하던 일에 붙일 수 있고(행동 변화 적음), 효과가 눈에 보이면(결과물 명확) 자연스럽게 계속하게 되죠.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🔁 반복성 4체크</span>
    <ul>
      <li><b>빈도</b>: 충분히 자주 일어나나?</li>
      <li><b>일관성</b>: 입력·결과물이 표준화할 만큼 비슷한가?</li>
      <li><b>소유권</b>: 개인/팀? 검토·유지·중단은 누가? 담당자 부재 시?</li>
      <li><b>실행 패턴</b>: 자료·행동·검토 단계가 매번 작동하나? 트리거·연결은?</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🙋 도입 가능성 4체크</span>
    <ul>
      <li><b>사용자 끌림</b>: 이미 고통을 느끼고 더 나은 방법을 원하나?</li>
      <li><b>결과물 명확성</b>: 결과물을 알아보고 검토하기 쉬운가?</li>
      <li><b>적은 행동 변화</b>: 일하는 방식을 크게 안 바꿔도 되나?</li>
      <li><b>팀 적합성</b>: 기존 공유·검토·인계 방식에 맞나?</li>
    </ul>
  </div>
</div>

### 마케팅 예시 — 셋 중 무엇부터?

<div class="oa-bars">
  <div class="oa-bar"><span>B. 반복되는 캠페인 브리프 작성</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:90%"></span><span class="oa-bar__val">반복·명확한 결과물·기존 업무</span></span></div>
  <div class="oa-bar"><span>A. 막힐 때마다 아이디어 브레인스토밍</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:35%"></span><span class="oa-bar__val">트리거 모호·결과 편차</span></span></div>
  <div class="oa-bar"><span>C. 인턴의 소셜 포스트 1개 재작성</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:12%"></span><span class="oa-bar__val">일회성 작업</span></span></div>
  <p class="oa-bars__caption">우선순위 판단을 돕는 개념적 표현이에요 (강의 수치 아님).</p>
</div>

가장 화려한 아이디어보다 **익숙하고, 반복되고, 아프고, 검증하기 쉬운** 워크플로우가 좋은 첫 후보예요. 위임형 워크플로우는 **한 번의 완전한 실행**으로 시험하고, 준비·검토·수정 시간과 실제 사용량, 품질 기준 충족 여부를 함께 기록해요. 사용량이 적어도 재작업이 늘면 이득이 아니에요.

## 5. 워크플로우 추천문 쓰기

추천문은 **배포 계획이 아니라** 다음 대화를 이끌 짧은 판매자 관점의 단락이에요.

| 묶음 | 담을 내용 (필요한 것만 골라서) |
|---|---|
| 워크플로우·사용자 | 부서·사용자 그룹, 개선할 워크플로우와 work object, 의존하는 자료 |
| ChatGPT의 기여 | prompt-to-output 패턴, Chat 협업 vs Work 위임 |
| 실행·지속 책임 | 허용 앱·도구·행동, 일정·트리거, 검토·승인·에스컬레이션, 개인/팀 소유 |
| 거버넌스·결과물 | 승인된 자료, 검토자와 대상 독자, 공유 결과물 유지 담당 |
| 반복성·도입 | 왜 반복되는지, 왜 쓸 것 같은지 |
| 검증·다음 단계 | 검증할 측정 결과, 모니터링 방법, 다음에 확인할 것 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 너무 넓은 추천</span>
    <ul>
      <li>"부서 전체에 ChatGPT를 배포해 생산성을 높이세요."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 실용적인 추천 (요약)</span>
    <ul>
      <li>"임원 계정 리뷰용 갱신 브리핑부터 시작하자. Work로 승인된 계정 메모·지원 이력·사용 요약을 초안으로 만들고 매니저가 검토(직접 분석을 선호하면 Chat). 초기 팀과 완전한 테스트 1회로 준비·검토·수정 시간과 품질을 비교. 다음 단계: 원자료, 검토 기대치, 모니터링 신호, 초기 팀 확정."</li>
    </ul>
  </div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🤖 ChatGPT로 추천문 초안 만들 때</span>
  <p>"모르는 건 모른다고 쓰고, 효과는 조건부로 쓰고, 승인·가용성·결과를 지어내지 말고, 기술 설정은 빼 줘"라고 지시하세요. 그다음 직접 점검: 작업이 아닌 워크플로우인가? 반복·도입 이유가 있나? 자료·행동·소유·검토·독자·유지가 있나? 측정 결과와 다음 검증이 있나?</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 반복 가능한 ChatGPT 워크플로우 기회로 가장 강한 것은?</summary>
  <p>① 발표 자료 하나 다듬기 ② 어카운트 팀의 여러 항목짜리 미팅 후 후속 패키지 준비 ③ 글쓰기 도움이 필요할 때마다 ④ 영업 담당자 이메일 한 통 작성</p>
  <p><b>정답: ②</b> — 사용자·반복 단계·결과물이 있는 워크플로우예요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. "영업팀이 통화 메모와 CRM으로 리더십용 계정 리뷰 요약을 만든다." 빠진 워크플로우 요소는?</summary>
  <p><b>정답: 요약이 뒷받침하는 결정이나 행동</b> (배포 계획·모델·라이선스 유형은 아직 필요 없음)</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 법무팀이 반복적인 공급 계약서에서 주요 조건·갱신일·누락 조항·위험 메모를 표준 요약 템플릿에 옮기느라 바쁘다. 가장 맞는 패턴은?</summary>
  <p><b>정답: Extract & structure</b> — 흩어진 정보를 정해진 형식으로 뽑아 정리하는 일이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 먼저 탐색할 가장 강한 후보는?</summary>
  <p><b>정답:</b> 오디언스 인사이트·제품 메시지·채널 요구사항으로 <b>반복 캠페인 브리프</b>를 준비하고, 팀이 이미 검토하며 더 일관된 인계를 원하는 경우. (채널별 카피는 브리프 이후·제품팀마다 다름 / 브레인스토밍은 입력이 제각각 / 성과 요약은 지표·자료 합의 전)</p>
</details>

<details class="oa-quiz">
  <summary>Q5. 가장 강한 워크플로우 추천문은?</summary>
  <p><b>정답:</b> "고객성공 매니저의 갱신 브리핑 준비부터. 임원 리뷰 전마다 반복되고, 계정 메모·지원 이력을 쓰며, 매니저가 검토할 브리핑을 만든다. 승인된 자료와 검토 기대치를 확인한 뒤 준비 시간과 브리핑 일관성을 검증." — 워크플로우·반복성·입력·결과물·검토·측정이 다 있어요.</p>
</details>

## 정리

- **작업은 한 번의 행동, 워크플로우는 사용자·트리거·입력·단계·결과물·목적의 흐름.** 가치는 보통 워크플로우에서 나온다.
- 8칸 워크플로우 지도 → 마찰 지점 → 맥락(부서·업종·자료·거버넌스·성공 신호) 순으로 구체화한다.
- ChatGPT의 역할은 **입력 → AI 작업 유형 → 결과물 → 사람 검토**의 패턴 문장으로 설명한다 (협업형·위임형·지속형).
- 반복성(빈도·일관성·소유권·실행 패턴)과 도입 가능성(끌림·명확성·적은 변화·팀 적합)으로 첫 워크플로우를 고른다.
- 추천문은 짧게: 무엇을 먼저, 왜, 무엇을 검증할지. 배포 계획은 나중 단계의 일이다.
