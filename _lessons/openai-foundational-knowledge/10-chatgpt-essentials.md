---
title: "ChatGPT Essentials"
title_ko: "ChatGPT 핵심 — 협업(Chat)과 위임(Work), 프롬프트 기본, 업무 흐름, 공유와 책임"
course: openai-foundational-knowledge
lesson: 10
---

## 학습 목표

- ChatGPT가 무엇인지, **Chat(협업)** 과 **Work(위임)** 가 어떻게 다른지 설명하기
- 주요 **요금제 · 워크스페이스 개념 · 직장 사용 시 고려사항**을 큰 그림으로 구분하기
- **Task + Context + Expectation** 프롬프트 패턴으로 더 쓸모 있는 결과 얻기
- 리서치 · 글쓰기 · 분석 · 창작 · 반복 지원 같은 **대표 업무 흐름** 알아보기
- 맥락 도구 · 개인 **dot** · 팀 소유 **Team Task** · 공유 **Page**가 각각 무엇을 돕는지 구분하기
- 사용자가 ChatGPT를 **자신 있고 안전하게** 쓰려면 무엇이 필요한지 설명하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>ChatGPT란?</strong>직원용 AI 작업 공간 · Chat vs Work</li>
  <li><strong>요금제·직장 사용</strong>개인 실험 → 관리형 워크스페이스</li>
  <li><strong>프롬프트 기본</strong>Task + Context + Expectation, 그리고 반복</li>
  <li><strong>업무 흐름</strong>배우기 · 리서치 · 창작 · 분석 · 반복 지원</li>
  <li><strong>더 나아가기</strong>dot · Team Task · Page · Apps · 실행 위치</li>
  <li><strong>자신감 있는 사용</strong>예시 · 안전 가이드 · 시작 프롬프트 · 연습</li>
</ol>

많은 고객에게 ChatGPT는 AI 가치를 **가장 빨리** 맛보는 길이에요. 질문하고, 초안 쓰고, 요약하고, 분석하고, 대화하며 결과를 다듬죠. 이 코스는 ChatGPT를 **직원이 매일 쓰는 지식 업무 공간**으로 이해하는 기초를 줘요. 제품·모델·요금제 이름은 모두 **(강의 기준)** 이고 바뀔 수 있어요.

## 1. ChatGPT란 무엇이고 왜 중요한가

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍🍳 쉽게 말하면 — 요리 수업 vs 출장 요리사</span>
  <p><b>Chat</b>은 선생님 옆에서 같이 요리하는 거예요. "소금 조금 더", "불 줄여 볼까?" 하며 한 단계씩 같이 만들죠. <b>Work</b>는 출장 요리사에게 "오늘 저녁 4인분, 매운 건 빼고, 7시까지"라고 <b>결과를 맡기는</b> 거예요. 요리사가 장보기부터 플레이팅까지 여러 단계를 하지만, <b>맛을 보고 상에 올릴지 정하는 건 여전히 나</b>예요.</p>
</div>

ChatGPT는 **협업적 사고 · 창작 · 리서치 · 분석**, 그리고 가능한 곳에선 **위임된 지식 업무**를 위한 OpenAI의 **최종 사용자 제품**이에요. 자연어로 입력하거나 말하고, 파일을 올리거나 참조하고, 작업을 계속 이끌어 가요.

| 구분 | Chat | Work (가능한 경우) |
|---|---|---|
| 관계 | **협업** — 주고받으며 함께 | **위임** — 결과를 정의하고 맡김 |
| 사용자 역할 | 각 초안에 반응하며 모양을 잡음 | 결과 · 맥락 · 경계 제시 → 계획·진행 검토 → 완성본 평가 |
| 예 | 질문 · 아이디어 · 초안·재작성 · 요약 · 문서/데이터 분석 · 설명 | 더 긴 여러 단계 지식 업무 (보고서·브리핑 등) |
| 공통 | 사용자는 **방향 설정과 검토에 계속 책임** | |

Chat은 **Intelligent UI**(지원 시)로 답을 상호작용형 비교표 · 다이어그램 · 차트 · 양식으로 보여 줄 수 있고, **점진적 답변**으로 생각하거나 도구를 쓰는 중에도 답을 먼저 보여 주기 시작해요 (강의 기준).

핵심: ChatGPT는 **작업 공간과 사용자 경험**을 주고, GPT-6 Astra(강의 기준) 같은 **모델이 그 안의 지능**을 제공해요. 그래서 ChatGPT는 API로 만든 맞춤 앱과 달라요. ChatGPT에선 **사용자가 목표 · 맥락 · 제약 · 도구·출처 · 판단**을 가져와요.

### ChatGPT · API · Codex 비교

| | ChatGPT Chat | ChatGPT Work | OpenAI API | Codex |
|---|---|---|---|---|
| 주 사용자 | 직원 | 지식 노동자 | 빌더(개발자) | 엔지니어링팀 |
| 상호작용 | 협업 | 위임 | 만들고 통합 | 엔지니어링 위임 |
| 경험 위치 | ChatGPT | ChatGPT | 제품·시스템 안 | Codex |
| 잘 맞는 일 | 생각·창작 | 여러 단계 지식 업무 | 내장형·맞춤 AI | 소프트웨어 엔지니어링 |

> 예: 새 경비 승인 절차 사내 공지를 써야 해요. Chat에 "비기술 직원 대상, 친근하고 200단어 이내, 시행일·바뀌는 점·문의처 포함"으로 요청 → 초안 → "더 짧게" → "따뜻한 첫 문장 추가" → "관리자용으로 다시". 사용자가 **계속 관여하며 모양을 잡는** 전형적인 Chat 흐름이에요. 더 큰 결과물이면 Work를 고려하되 검토·판단·승인은 여전히 사용자 몫이에요.

## 2. 요금제와 직장 사용 고려사항

모든 요금제 기능·한도를 외울 필요는 없어요(자주 바뀌어요). 중요한 건 **왜 직장 사용은 개인 실험보다 더 많은 구조가 필요한지**예요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">👤</span><span class="oa-card__title">Free · Go · Plus · Pro</span>개인 요금제. 접근·사용량·가용성이 다름. 처음 실험하고 자신감을 쌓는 곳</div>
  <div class="oa-card"><span class="oa-card__icon">👥</span><span class="oa-card__title">Business</span>협업과 관리자 통제가 있는 <b>관리형 팀 워크스페이스</b></div>
  <div class="oa-card"><span class="oa-card__icon">🏢</span><span class="oa-card__title">Enterprise</span>조직 전체 워크스페이스. 더 넓은 보안 · 프라이버시 · 컴플라이언스 · 접근 · 거버넌스</div>
  <div class="oa-card"><span class="oa-card__icon">🎓</span><span class="oa-card__title">Edu</span>자격 있는 교육 기관용 관리형 워크스페이스</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🏛️</span><span class="oa-card__title">공공 부문</span>ChatGPT Gov 등. 약속 전에 제품 · 자격 · 약관 · 지역 가용성 검증</div>
</div>

### 사용량과 비용 — 헷갈리기 쉬운 포인트

<div class="oa-eli">
  <span class="oa-eli__head">📱 쉽게 말하면 — 휴대폰 데이터 요금제</span>
  <p>Work 작업은 "건당 정가"가 아니라 <b>데이터 요금제처럼 사용량</b>으로 계산돼요. 영상을 고화질로 오래 보면 데이터를 많이 쓰듯, 모델 · 추론 강도 · 입력·출력 양 · 작업량에 따라 쓰는 양이 달라요. 그리고 <b>"80% 썼어요" 문자 알림</b>이 와도 인터넷이 저절로 끊기진 않죠 — 관리자 알림도 마찬가지예요.</p>
</div>

- ChatGPT Work 사용량은 워크스페이스의 **허용량·가격**을 따르며, 작업당 고정 가격이 아니에요. **API 청구 정보에는 ChatGPT 사용량이 안 보여요.**
- 일반 **API 청구는 ChatGPT 구독과 별개**. 참여 앱·사이트에서 자격 있는 사용자가 **자기 ChatGPT 허용량을 쓰도록 따로 선택**할 순 있지만, **Sign in with ChatGPT는 신원 확인일 뿐** 허용량 공유나 임의 API 사용을 켜 주지 않아요.
- Enterprise · Edu: **개인 사용 한도 ≠ 워크스페이스 지출 통제.** 임계값 **알림은 사용을 자동으로 멈추지 않아요.**
- 반복 워크플로 비용은 사용량만 보지 말고 **결과 품질 + 사람이 다시 손봐야 하는 양**까지 비교. 더 싼 옵션은 **같은 품질 기준**을 통과해야 확대해요.

### 왜 조직용 요금제가 중요한가

직원이 개인 계정을 따로 쓰면 조직은 **어떤 기능을 쓰는지, 어떤 정보를 공유하는지, 어떤 행동을 하는지, 정책에 맞는지** 잘 보이지 않아요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">😵 흩어진 개인 실험</span>
    <ul>
      <li>각자 개인 계정으로 사용</li>
      <li>어떤 정보를 넣어도 되는지 불분명</li>
      <li>리더는 사용 현황을 거의 모름</li>
      <li>좋은 사용법이 공유되지 않음</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 관리형 워크스페이스</span>
    <ul>
      <li>공유 워크스페이스 · 재사용 팀 워크플로</li>
      <li>신원·접근: 역할 · 그룹 · RBAC · SSO · SCIM</li>
      <li>프라이버시 약속 · 데이터 사용 통제</li>
      <li>App · 플러그인 · 에이전트 · 행동 통제, 공유·게시 통제</li>
      <li>분석 · Task Insights · 보존 · 레지던시 · 컴플라이언스 가시성</li>
    </ul>
  </div>
</div>

요금제 선택은 **조직 규모만으로 정하지 않아요.** 사용자 · 워크플로 · 프라이버시 · 거버넌스 · 데이터 위치 · 필요한 통제 · 롤아웃 목표, 그리고 Apps · Work · Workspace Agents가 **켜져 있는지**에 달려 있어요.

### ChatGPT Enterprise 큰 그림 (강의 기준)

| 핵심 개념 | 뜻 |
|---|---|
| 관리형 워크스페이스 | 회사 워크스페이스로 접근, 역할·그룹·설정이 접근을 결정 |
| 프라이버시 약속 | **기본적으로 Business · Enterprise 입력·출력은 OpenAI 모델 학습에 쓰이지 않음** (공식 출처로 최신 확인) |
| 신원·접근 | 사용자 · 역할 · 그룹 · SSO · SCIM · 역할 기반 접근 |
| 맥락 · 앱 · 행동 | 관리자가 조직 맥락 · Apps · 플러그인 · 에이전트 · 행동과 **승인 필요 지점**을 관리 |
| Company Knowledge | 승인된 조직 정보를 맥락으로 사용 (설정에 따라) |
| 분석·컴플라이언스 | 워크스페이스 분석 · Task Insights · 컴플라이언스 로그 · Compliance API |
| 보존·레지던시 | 요금제·지역·설정에 따라 다름 → 약속 전 검증 |

### 책임 있는 직장 사용

원칙은 단순해요: **ChatGPT는 일을 빠르게 하지만, 책임은 사용자에게 있다.** 무엇을 공유해도 되는지, 어떤 출처·App에 접근하는지, 어떤 행동에 확인이 필요한지, 언제 법무·보안·전문가 의견이 필요한지 알아야 해요.

특히 꼼꼼히 검토할 것: 중요한 문서 요약 · 분석·계산 · 구조화 데이터 · 정책 민감 정보 · 고객 대면 콘텐츠 · 고위험 결정 · 법률·금융·의료·안전·컴플라이언스 자료. **검토는 ChatGPT를 거부하는 게 아니라 잘 쓰는 방법**이에요.

## 3. 더 좋은 결과를 위한 프롬프트 기본

### Task + Context + Expectation ⭐

<div class="oa-eli">
  <span class="oa-eli__head">✂️ 쉽게 말하면 — 미용실 주문</span>
  <p>"알아서 잘라 주세요"(모호한 프롬프트)도 결과는 나와요. 하지만 "<b>머리 다듬어 주세요</b>(Task), <b>다음 주 졸업 사진 찍어요</b>(Context), <b>앞머리는 눈썹 위, 전체 길이는 어깨까지</b>(Expectation)"라고 하면 원하는 머리가 나올 확률이 훨씬 높죠. 그리고 거울 보며 "옆을 조금만 더"라고 하는 게 <b>반복(Iteration)</b>이에요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">Task (할 일)</span>ChatGPT가 할 일: 요약 · 재작성 · 설명 · 브레인스토밍 · 비교 · 정리 · 분석 · 번역 · 초안</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">Context (맥락)</span>필요한 배경: 대상 · 목적 · 역할 · 원자료 · 제약 · 결정 · 상황</div>
  <div class="oa-card"><span class="oa-card__icon">📐</span><span class="oa-card__title">Expectation (기대)</span>좋은 답의 모양: 형식 · 톤 · 길이 · 상세 수준 · 읽기 난이도 · 출력 유형</div>
</div>

| 프롬프트 | Task | Context | Expectation |
|---|---|---|---|
| "이 문서를 임원용으로 다섯 개 불릿으로 요약해 줘" | 요약 | 임원 대상 | 불릿 5개 |
| "AI가 처음인 고객에게 ChatGPT와 API 차이를 비기술 언어로 설명해 줘" | 차이 설명 | AI 초보 고객 | 비기술 언어 |

매번 "task, context…"라는 단어를 쓸 필요는 없어요. **생각을 돕는 도구**예요.

- **위임(Work) 작업**에선 같은 습관을 넓혀요: 결과 · 맥락 · 제약 · 출처·도구 · **승인 경계** · 완성본을 검토할 **기준**.
- **계속되는 작업**이면 **담당자 · 검토 주기 · 멈출 조건**을 더해요. 계속되는 목표도 일회성 과제만큼 **경계가 분명**해야 해요.

### 진짜 실력은 반복(Iteration)

첫 프롬프트가 완벽할 필요는 없어요. 첫 답은 **반응할 거리**예요. 유용한 후속 프롬프트:

- "더 짧게" · "비기술 대상으로 다시" · "예시 세 개 추가" · "표로 바꿔 줘"
- "어떤 가정을 하고 있어?" · "수정 전에 나한테 질문 세 개 해 줘" · "쓰기 전에 무엇을 검증해야 해?"
- "내 프롬프트를 개선해 줘" ← 프롬프트 자체를 다듬는 **메타 프롬프팅**

> 연습(선택, 5–7분): 자주 하는 일 하나로 첫 프롬프트를 쓰고, Task · Context · Expectation을 더해 개선한 뒤 무엇이 바뀌었는지(대상 · 형식 · 톤 · 길이 · 원자료 · 결정 맥락) 비교해 보세요.

## 4. ChatGPT의 대표 업무 흐름

사용자는 "기능"이 아니라 **끝내야 할 일**에서 출발해요. 그래서 기능 목록보다 **업무 흐름 언어**가 중요해요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">💡</span><span class="oa-card__title">배우고 묻고 탐색</span>낯선 주제 이해, 후속 질문, 브레인스토밍, 수준별 설명. 음성으로 생각을 말하며 탐색도 가능 (지원 시)</div>
  <div class="oa-card"><span class="oa-card__icon">🔎</span><span class="oa-card__title">검색·리서치 지원</span>배경 수집 · 요약 · 종합. 더 깊은 조사는 <b>Deep Research</b>. 출처 확인·검증은 필수</div>
  <div class="oa-card"><span class="oa-card__icon">✍️</span><span class="oa-card__title">창작·협업</span>이메일 · 개요 · 톤 조정 · 번역 · 메모를 구조화. <b>writing blocks</b>로 나란히 편집 (지원 시)</div>
  <div class="oa-card"><span class="oa-card__icon">📊</span><span class="oa-card__title">분석·해석</span>파일 · 데이터 · 이미지 · 차트 · 긴 문서. 추세 요약, 이상치 표시, 조사할 질문 제안</div>
  <div class="oa-card"><span class="oa-card__icon">⏰</span><span class="oa-card__title">간단한 반복 지원</span>Tasks · 예약 프롬프트로 리마인더 · 주간 요약 · 계획 메모 (지원 시)</div>
</div>

이 흐름들은 **이어져요.** 한 사람이 하루에 주제를 조사하고 → 요약하고 → 메시지 초안을 쓰고 → 톤을 고치고 → 후속 질문을 준비할 수 있죠. 실전 기술은 **어떤 도움이 필요한지 알아보고, 충분한 맥락을 주고, 쓰기 전에 검토하는 것**이에요.

### 데이터 분석의 함정

<div class="oa-eli">
  <span class="oa-eli__head">📅 쉽게 말하면 — 아직 안 온 날의 출석부</span>
  <p>출석부에서 다음 주 칸이 비어 있다고 "다음 주에 다 결석"은 아니잖아요. <b>아직 그날이 안 온 것</b>뿐이에요. 고객 유지율 표에서도 미래 기간의 빈칸은 "유지율 0%"가 아니라 <b>아직 관측 안 된 데이터</b>일 수 있어요. 그러니 <b>빈칸이 무슨 뜻인지 ChatGPT에 분명히 알려 주고</b>, 중요한 계산은 직접 확인해요.</p>
</div>

### 상호작용형 답변도 검증이 필요하다

> 예: 팀이 같은 주제의 워크숍 안건 두 개와 학습 브리프(참가자 경험이 섞여 있고 배운 걸 바로 써야 함)를 주고 **이 자료만으로** 비교해 달라고 해요. Intelligent UI가 나란히 비교를 보여 주고(지원 시), 팀은 "실습 우선, 초보자 지원도 고려해서 각 안이 브리프에 어떻게 맞는지, 무엇이 빠졌는지"로 기준을 다듬어요. 점진적 답변 덕분에 비교가 **세부를 채우는 중에** 먼저 보일 수 있어요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 하지 말 것</span>
    <ul>
      <li>먼저 보인 안을 고르고 나중에 확인</li>
      <li>상호작용형이니 정확하다고 믿기</li>
      <li>Page에 옮기면 검증된 결과라고 여기기</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 할 것</span>
    <ul>
      <li>기준을 다듬은 뒤 <b>완성된</b> 비교를 원래 자료와 대조</li>
      <li>상호작용 형식은 <b>정확성을 보장하지 않음</b></li>
      <li>고위험·민감한 일은 전문가와 정책을 따름</li>
    </ul>
  </div>
</div>

반복 작업(Tasks)이 다른 사람 · 시스템 · 민감 정보 · 핵심 업무에 영향을 주면 직장 가이드를 따르고 **검토·승인**을 갖춰요.

## 5. ChatGPT 더 깊이 쓰기

### 시간이 지날수록 더 유용해지는 기능 (강의 기준)

| 기능 | 무엇을 돕나 |
|---|---|
| **Memory · Custom Instructions** | 선호(간결한 답, 글 스타일, 요약 형식)와 역할을 기억 — 요금제·설정·정책에 따라 |
| **Projects** | 관련 채팅 · 파일 · 지시를 한곳에 모아 긴 작업(캠페인 · 리서치 · 제안서) 관리 |
| **Custom GPTs** | 특정 작업용 재사용 어시스턴트 (일관된 톤, 승인 자료 기반 답변) |
| **ChatGPT Work** | 위임된 여러 단계 지식 업무. 결과 정의 → 진행 조정 → 중요 행동 승인 → 완성본 검토 |
| **Workspace Agents** | 자격 있는 Business · Enterprise에서 에이전트를 만들고 · 테스트 · 게시 · 공유 · 예약 · 트리거 |

연속성과 위임이 늘수록 **맥락 · 권한 · 승인 경계 · 소유권**이 더 중요해져요.

### 누가, 얼마나 오래, 누구와 — 일하는 관계 고르기 ⭐

<div class="oa-eli">
  <span class="oa-eli__head">🏫 쉽게 말하면 — 학급 운영</span>
  <p><b>Chat</b>은 짝꿍과 문제 풀기. <b>Work</b>는 "이번 과학 보고서 하나 완성"처럼 끝이 있는 과제. <b>dot</b>은 "한 달 동안 내 화분에 물 주는 걸 챙겨 줘"처럼 <b>한 사람</b>의 계속되는 책임. <b>Team Task</b>는 "매주 금요일 학급 소식지 만들기"처럼 <b>반 전체</b>의 당번 일 — 반 공용 계정을 쓰고, 만든 친구의 개인 일기장은 못 봐요. <b>Page</b>는 게시판에 붙인 소식지 — 결과물일 뿐, <b>누가 다음 호를 만들지</b>는 따로 정해야 해요.</p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 270" role="img" aria-label="Chat, Work, dot, Team Task가 지속 기간과 소유자 축에서 어디에 놓이는지, 그리고 결과물인 Page">
  <defs>
    <marker id="ar10" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.a10{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.b10{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.c10{fill:#fff7dc;stroke:#e8d48f;stroke-width:1.5}.t10{font-size:13px;fill:#4a4a4a;font-weight:700}.s10{font-size:11px;fill:#7a7a7a}</style>
  </defs>
  <line x1="60" y1="230" x2="520" y2="230" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar10)"/>
  <text x="290" y="252" text-anchor="middle" class="s10">일회성 → 계속되는 일</text>
  <line x1="60" y1="230" x2="60" y2="20" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar10)"/>
  <text x="40" y="125" text-anchor="middle" class="s10" transform="rotate(-90 40 125)">개인 → 팀 소유</text>
  <rect x="80" y="140" width="130" height="70" rx="12" class="a10"/>
  <text x="145" y="166" text-anchor="middle" class="t10">💬 Chat</text><text x="145" y="186" text-anchor="middle" class="s10">함께 생각·창작</text>
  <rect x="225" y="140" width="130" height="70" rx="12" class="a10"/>
  <text x="290" y="166" text-anchor="middle" class="t10">📦 Work</text><text x="290" y="186" text-anchor="middle" class="s10">경계 있는 위임 결과</text>
  <rect x="380" y="140" width="130" height="70" rx="12" class="b10"/>
  <text x="445" y="166" text-anchor="middle" class="t10">🔵 dot</text><text x="445" y="186" text-anchor="middle" class="s10">한 사람의 지속 책임</text>
  <rect x="380" y="40" width="130" height="80" rx="12" class="b10"/>
  <text x="445" y="66" text-anchor="middle" class="t10">👥 Team Task</text><text x="445" y="86" text-anchor="middle" class="s10">일정·이벤트 실행</text><text x="445" y="104" text-anchor="middle" class="s10">팀 서비스 계정</text>
  <rect x="560" y="70" width="150" height="110" rx="12" class="c10"/>
  <text x="635" y="98" text-anchor="middle" class="t10">📄 Page (Space)</text><text x="635" y="120" text-anchor="middle" class="s10">공유·편집 결과물</text><text x="635" y="140" text-anchor="middle" class="s10">≠ 유지 프로세스</text><text x="635" y="160" text-anchor="middle" class="s10">대상(audience) 따로 확인</text>
  <line x1="512" y1="80" x2="556" y2="110" stroke="#c99400" stroke-width="1.2" stroke-dasharray="4 3" marker-end="url(#ar10)"/>
  <line x1="512" y1="175" x2="556" y2="150" stroke="#c99400" stroke-width="1.2" stroke-dasharray="4 3" marker-end="url(#ar10)"/>
</svg>
</div>

**dot — 한 사람의 지속 책임 (강의 기준)**: 유지된 맥락 · 연결된 앱 · 자체 클라우드 컴퓨터로 대화와 대화 사이에도 이어지는 일을 해요.

> 예시 요청: "앞으로 4주간 승인된 계정 노트에서 고객 우선순위 변화를 지켜봐 줘. 매주 출처 링크가 달린 요약을 주고, 충돌하는 정보는 내 검토용으로 표시해. **고객에게 연락하지 마.**" → **책임 · 기간 · 출처 · 출력 · 경계**가 다 들어 있어요. 개인 맥락은 **자동으로 팀 맥락이 되지 않고**, 계속되는 목표가 **무제한 접근·권한**을 주지도 않아요.

**Space와 Page — 일을 공유하기**: 대화는 답을 **만들고**, Page는 동료가 함께 고치는 **공유 편집 문서**예요. Space는 Page와 파일의 집이고, 계정 수준 워크스페이스가 아니에요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 흔한 착각</span>
    <ul>
      <li>원본 파일이 비공개니 Page에 옮긴 발췌도 안전하다</li>
      <li>보기 전용으로 공유하면 발췌 내용이 숨겨진다</li>
      <li>Page를 만들면 누군가 알아서 최신으로 유지한다</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 실제 (강의 기준)</span>
    <ul>
      <li>개인 채팅·메모리는 자동 공유되지 않지만, <b>Page에 복사·요약한 정보는 대상에게 보임</b></li>
      <li>외부 파일 <b>링크</b>는 그 파일 권한을 따르지만, <b>발췌</b>는 이제 공유 콘텐츠</li>
      <li>Page는 결과물이지 유지 프로세스가 아님 → <b>누가 확인·갱신할지 합의</b></li>
    </ul>
  </div>
</div>

**Team Task와 공유 대화**:

- **Team Task**는 팀 **서비스 계정**과 설정된 연결로 일정·이벤트 기반 작업을 클라우드에서 실행해요. 만든 사람의 개인 채팅·메모리는 **상속하지 않아요.** 팀원은 지난 실행과 파일을 볼 수 있으니 그 **대상**을 고려해 작업을 정의해요. 누군가는 지시 수정 · 예외 처리 · 계속 여부 결정을 맡아야 해요.
- **Slack · Microsoft Teams 안의 ChatGPT**(관리자 활성화)는 채널·스레드에서 함께 일하는 경험이에요. **ChatGPT 안에서 Slack을 App으로 연결하는 것과는 달라요.** 모두의 개인 기록·접근이 자동 포함되진 않아요.

### 더 많은 맥락으로 일하게 하는 기능

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">Apps (구 Connectors)</span>외부 도구 · 정보 · 행동과 연결. 플러그인으로 앱 기반 흐름 발견. 요금제 · 설정 · 역할 · 지역 · 앱 권한에 따라</div>
  <div class="oa-card"><span class="oa-card__icon">🏢</span><span class="oa-card__title">Company Knowledge</span>관리형 워크스페이스에서 승인된 조직 정보를 맥락으로</div>
  <div class="oa-card"><span class="oa-card__icon">📎</span><span class="oa-card__title">파일 업로드</span>문서 · 스프레드시트 · 이미지를 대화로 가져와 요약 · 분석 · 변환</div>
  <div class="oa-card"><span class="oa-card__icon">🔬</span><span class="oa-card__title">Deep Research</span>여러 출처에 걸친 더 깊은 조사 · 구조화된 종합</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚪</span><span class="oa-card__title">인터페이스 · 이벤트 · Site</span>플러그인 확장(앱 같은 화면)과 <b>MCP Events</b>(외부 업데이트로 작업 시작)는 <b>모든 행동 권한이 아님</b>. 연결된 <b>Site</b>는 방문자 <b>각자의 연결</b>로 읽기 전용 접근</div>
</div>

### Work는 어디서 돌고 무엇에 접근하나

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면 — 집에서 하는 숙제 vs 학원에서 하는 숙제</span>
  <p>집(내 기기)에서 하면 내 책상 위 책과 공책(허락한 파일 · 앱 · 브라우저)을 쓸 수 있어요. 학원(클라우드)에서 하면 <b>내 책상 위 물건은 없어요</b> — 필요한 자료를 챙겨 가야 하죠. 그리고 집에서 해도 모르는 건 선생님께 물어보듯, <b>로컬 실행이라도 일부 내용은 OpenAI로 보내 처리될 수 있어요.</b> 학원 가는 버스를 집 앞에서 탔다고 숙제를 집에서 한 게 아니듯, <b>데스크톱 앱에서 시작한 클라우드 작업도 로컬이 아니에요.</b></p>
</div>

| | 내 기기에서 실행 | 클라우드에서 실행 |
|---|---|---|
| 접근 | 허락한 파일 · 앱 · 브라우저 맥락 | OpenAI 관리 환경. 내 컴퓨터 파일 · 탭 · 로그인 세션 **자동 접근 없음** |
| 주의 | 완전 오프라인이 아님, 일부 내용은 처리 위해 전송될 수 있음 | 필요한 파일 제공 또는 승인된 출처 연결. 클라우드 브라우저 세션은 별개 |

- **접근 ≠ 권한(authorization)**: 어떤 계정·출처가 연결됐는지, 무엇이 허용됐는지, 사람 승인이 필요한지 확인. **브라우저 · 연결 앱 · 코드용 네트워크 접근은 따로** 통제돼요.
- **데이터마다 처리 방식이 달라요**: Space가 켜지면 Library를 대체(Projects는 별개). 채팅 · Space/Library 파일 · 프로젝트 파일 · 브라우저 세션 데이터는 **저장·삭제 통제가 각각 달라요.** 대화를 지워도 관련 파일·공유물이 다 지워진다고 가정하지 마세요.

### 일회성 채팅에서 재사용 워크플로로

<ol class="oa-flow">
  <li><strong>Chat</strong>디스커버리 질문 브레인스토밍</li>
  <li><strong>Deep Research</strong>출처 있는 시장 브리핑</li>
  <li><strong>Project</strong>채팅·노트·파일 모아 두기</li>
  <li><strong>Apps · Company Knowledge</strong>조직 맥락 추가 (허용 시)</li>
  <li><strong>Work</strong>한 장짜리 계정 브리핑 위임</li>
  <li><strong>Workspace Agent</strong>정기적으로 반복되면 (활성화 시)</li>
</ol>

Work에 줄 과제 예: "이 Project의 승인된 회의 노트와 리서치로 영업팀용 한 장 브리핑을 준비해 줘. 우선순위 · 미해결 질문 · 논의 포인트 3개를 넣고, 중요한 발견은 출처에 연결하고, **추측 대신 빈틈을 표시**하고, 내 검토용으로 저장해. **고객에게 보내지 마.**" → 무엇을 · 어떤 출처로 · 누구를 위해 · 무엇을 담아 · 어디서 사람이 검토하는지가 분명해요.

## 6. 자신감 있는 ChatGPT 사용 지원하기

### 사용자의 출발점은 제각각

| 출발점 | 모습 | 도움 |
|---|---|---|
| 😃 열정적 | 강력한 건 알지만 첫 워크플로가 없음 | 구체적 예시 · 시작 프롬프트 |
| 🤨 회의적 | 믿을 만한지, 시간이 절약되는지, 내 역할에 맞는지 의심 | 작고 확인하기 쉬운 성공 경험 |
| 😟 걱정 | 정확성 · 프라이버시 · 데이터 사용 · 고객 대면 콘텐츠 걱정 | 안전 사용 가이드 · 검토 기준 |
| 🙋 개인 경험자 | 개인적으론 써 봤지만 직장 사용 가이드 필요 | 직장 정책 · 공유 관행 |

좋은 첫 워크플로는 **실용적이고 익숙하고 검토하기 쉬운 것**: 공개 정보 요약, 사내 메모 초안, 브레인스토밍, 텍스트 서식 바꾸기. 민감하거나 고위험 작업으로 시작하지 않아요.

### 자신감에 필요한 네 가지

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧭</span><span class="oa-card__title">명확한 예시</span>"생산성에 써 보세요"는 너무 넓음. "회의 메모를 후속 이메일로 바꾸기"는 해 보기 쉬움</div>
  <div class="oa-card"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">안전 사용 가이드</span>공유해도 되는 정보, 피할 것, 추가 검토가 필요한 때</div>
  <div class="oa-card"><span class="oa-card__icon">🚀</span><span class="oa-card__title">시작 프롬프트</span>진입 장벽을 낮추고 빠르게 유용한 결과를 경험</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🏋️</span><span class="oa-card__title">연습할 허락</span>저위험 작업으로 손에 익히기</div>
</div>

도입 메시지: **작게 시작하고 → 맥락을 주고 → 결과를 검토하고 → 프롬프트를 개선하라.**

> 예: 직원들이 각자 ChatGPT로 메모 초안 · 자료 요약 · 브레인스토밍을 하지만 리더는 현황을 모르고 직원은 어떤 정보를 넣어도 되는지 헷갈려요. 관리형 워크스페이스로 옮기면 시작 프롬프트를 공유하고, 검토 기대치를 합의하고, 신입이 연습할 저위험 워크플로를 정할 수 있어요. 가치는 **접근 그 자체가 아니라 지원받는 사용 방식**이에요.

<div class="oa-eli">
  <span class="oa-eli__head">✍️ 연습 — 검토 가능한 브리프 만들기 (선택, 9–11분)</span>
  <p>가상의 프로젝트 노트만 쓰기: 온보딩 가이드 초안 완료 · 팀원 두 명이 아직 검토 필요 · 다음 팀 회의에서 프로젝트 리더가 배포 여부 결정 · 대상은 고객이 아닌 프로젝트팀.</p>
  <p>현재 상태 · 미해결 질문 · 다음 행동을 담은 브리프를 요청하는 Task + Context + Expectation 프롬프트를 쓰세요(노트로 돌아가는 출처 표시, 빠진 정보 표시, <b>보내기 금지</b>). 그리고 브리프 아래에 <b>담당자·검토자 / 대상 / 허용 정보 / 계속되는 일인지(일회성 · dot · Team Task)와 이유 / 계속이면 트리거·주기와 멈출 때 / 응답 형식과 검토 방법</b>을 적어요. 실제 출처는 연결하지 말고 글로 된 목업이면 충분해요.</p>
  <p>돌아보기: 검토 중 무엇을 빼거나 단서를 달았나? 대상에 고객이 들어가면 무엇이 바뀌나? 공유 결과물과 그것을 유지하는 프로세스는 왜 따로 정해야 하나?</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. ChatGPT를 가장 잘 설명한 것은?</summary>
  <p>① 소프트웨어 개발을 지원하는 코딩 에이전트 ② 맞춤 앱에 AI 모델을 내장하는 개발자 플랫폼 ③ 협업적 사고·창작, 그리고 가능한 곳에선 위임된 지식 업무를 위한 최종 사용자 AI 작업 공간 ④ 사람의 검토·판단·최종 결정을 대체하는 것</p>
  <p><b>정답: ③</b> — ①은 Codex, ②는 API, ④는 절대 아님.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 팀이 업무에 개인 ChatGPT 계정을 쓰고 있는데, 리더는 더 명확한 감독 · 공유 관행 · 강한 직장 가이드를 원해요. 가장 좋은 해석은?</summary>
  <p>① 프라이버시 · 접근 · 협업 · 거버넌스 통제가 있는 관리형 워크스페이스가 필요할 수 있다 ② 생산적으로 쓰고 있다면 개인 실험으로 늘 충분하다 ③ 모든 사용 사례가 자동화될 때까지 AI 사용을 멈춘다 ④ 직장 기술 도입 문제이니 Codex를 쓴다</p>
  <p><b>정답: ①</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 프롬프트 요소와 그것이 제공하는 것을 짝지어 보세요.</summary>
  <p>Task · Context · Expectation · Iteration ↔ (가) 형식·톤·길이·상세 수준·출력 유형 (나) 결과를 개선하거나 방향을 바꾸는 후속 안내 (다) 배경·목적·대상·원자료·제약 (라) ChatGPT가 할 일</p>
  <p><b>정답:</b> Task → (라) · Context → (다) · Expectation → (가) · Iteration → (나)</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Chat에서 워크숍 두 안을 비교하는데, ChatGPT가 아직 세부를 채우는 중에 상호작용형 비교가 나타났어요. 추천 전에 무엇을 해야 할까요?</summary>
  <p>① 먼저 보인 안을 고르고, 추천을 돌린 뒤 세부 확인 ② 기준을 다듬고, <b>완성된</b> 비교를 제공된 사실과 대조 ③ 상호작용 비교는 위임 실행이 필요하니 Work로 전환 ④ Page로 옮겨 공유본을 검증된 결과로 신뢰</p>
  <p><b>정답: ②</b></p>
</details>

<details class="oa-quiz">
  <summary>Q5. 필요와 그것을 가장 직접 가리키는 ChatGPT 기능을 짝지어 보세요. (강의 기준)</summary>
  <p>(가) 동료와 함께 고치는 공유 편집 문서 (나) 관련 채팅·파일·지시를 모아 긴 작업 관리 (다) 팀의 설정된 연결을 쓰는 팀 소유 예약 프로세스 (라) 허용된 맥락·도구로 한 사람의 지속 책임 수행</p>
  <p><b>정답:</b> (가) Space의 Page · (나) Projects · (다) Team Task · (라) dot</p>
</details>

<details class="oa-quiz">
  <summary>Q6. 새 사용자가 비공개 프로젝트 파일로 Page 초안을 만들어 동료에게 공유하려 해요. 공유 결정에 맞는 안내는?</summary>
  <p>① Team Task를 써서 연결 신원이 Page 독자를 정하게 한다 ② 공유 전에 Page의 대상과, 복사된 정보가 그 대상에게 적절한지 확인한다 ③ 원본 파일을 비공개로 두면 그 권한이 Page에 복사된 정보도 제한한다 ④ 보기 전용으로 주면 비공개 파일 발췌가 숨겨진다</p>
  <p><b>정답: ②</b></p>
</details>

## 정리

- ChatGPT는 직원이 매일 쓰는 **최종 사용자 AI 작업 공간**. **Chat = 협업**, **Work = 위임**(가능한 곳). API는 **빌더·통합 경로**, Codex는 **소프트웨어 엔지니어링** 전용 (강의 기준).
- 직장 사용엔 **구조**가 필요: Business · Enterprise · Edu 같은 관리형 워크스페이스가 신원·접근 · 프라이버시 · 행동 통제 · 분석 · 보존·레지던시를 더한다. 요금제는 **규모가 아니라 워크플로·통제 요건**으로 고르고, 세부는 항상 최신 공식 출처로 검증.
- 사용량·비용: Work는 **허용량 기반**, API 청구와 별개, **알림 ≠ 자동 중단**, 비용은 **품질 + 재작업**까지 비교.
- 프롬프트 = **Task + Context + Expectation**, 진짜 실력은 **반복**. 위임엔 승인 경계·검토 기준, 계속되는 일엔 **담당자 · 주기 · 멈출 조건**.
- 대표 흐름: **배우기 · 리서치 · 창작 · 분석 · 반복 지원**, 서로 이어진다. 상호작용형·점진적 답변도 **완성본을 원자료와 대조**.
- **dot(개인 지속) · Team Task(팀 소유, 서비스 계정) · Page(공유 결과물)** 는 서로 다른 선택. 공유 시 **대상과 복사된 정보**를 확인하고, 결과물과 **유지 프로세스는 따로** 정한다.
- **실행 위치 · 접근 · 권한 · 데이터 통제**는 각각 따로 확인. 인터페이스·이벤트·일정은 행동 권한이 아니다.
- 도입 성공 = 접근 + **명확한 예시 · 안전 가이드 · 시작 프롬프트 · 연습할 허락**. 작게 시작하고, 맥락을 주고, 검토하고, 개선한다.
