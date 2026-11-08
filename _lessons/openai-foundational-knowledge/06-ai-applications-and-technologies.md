---
title: "AI Applications & Technologies"
title_ko: "AI 애플리케이션과 기술 — 여섯 가지 패턴, RAG, 세 가지 결정 나누기"
course: openai-foundational-knowledge
lesson: 6
---

## 학습 목표

- **생성형 AI**를 쉬운 말로 설명하기
- 텍스트 생성 · 요약 · 분류 · 이미지 생성 · 코드 지원 · 검색 기반 답변 같은 **흔한 애플리케이션 패턴** 알아보기
- **RAG**를 개념적으로 설명하고 **근거 기반(grounding)** 이 왜 중요한지 말하기
- 애플리케이션 모양을 설명할 때 **사용자·제품 표면 / 역량·아키텍처 / 배포·운영**을 나눠 보기
- **에이전트형 AI**와 **AGI**를 조심스러운 언어로 다루고 다음에 물을 것 정리하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>생성형 AI</strong>입력 → 새 콘텐츠 생성</li>
  <li><strong>여섯 가지 패턴</strong>AI가 워크플로에서 하는 일</li>
  <li><strong>RAG</strong>검색 + 생성, 그리고 보장하지 않는 것</li>
  <li><strong>세 가지 결정</strong>표면 · 역량 · 배포/운영</li>
  <li><strong>에이전트 · AGI</strong>조심스러운 언어</li>
</ol>

고객은 AI **카테고리**가 아니라 **원하는 결과**로 말해요: "답을 더 빨리", "요약을 더 잘", "쓸 만한 콘텐츠", "코드 도움", "업무 자동화". 이 코스는 그 요청을 **애플리케이션 패턴**으로 번역하는 렌즈를 줘요. 전체 솔루션을 설계하는 게 아니라 **애플리케이션의 모양을 분명히 설명하고 과장하지 않는 것**이 목표예요.

## 1. 생성형 AI와 흔한 애플리케이션 패턴

### 고객은 결과로 말한다

| 고객의 말 | 숨은 AI 활동 |
|---|---|
| "고객 이메일 쓰는 걸 도와줬으면" | 텍스트 생성 |
| "긴 보고서를 요약해야 해요" | 요약 |
| "들어오는 지원 요청을 분류하고 싶어요" | 분류 |
| "캠페인용 제품 이미지가 필요해요" | 이미지 생성 |
| "개발자가 더 빨리 일했으면" | 코드 생성·지원 |
| "사내 문서에서 답을 찾고 싶어요" | 검색 기반 답변 |

### 생성형 AI를 쉬운 말로

**생성형 AI는 새 콘텐츠를 만들어요.** 텍스트·이미지·코드·오디오·구조화된 응답 등. 사용자가 질문·지시·파일·이미지·비즈니스 맥락을 **입력**하면 모델이 그걸 바탕으로 **출력**을 만들어요. 데이터베이스에서 정해진 답을 꺼내 오는 것만이 아니라 **새 출력을 생성**한다는 게 핵심이에요.

<div class="oa-eli">
  <span class="oa-eli__head">📖 쉽게 말하면</span>
  <p>도서관 사서에게 "공룡 책 어디 있어요?"라고 물으면 <b>있는 책을 찾아 줘요</b>(검색). 생성형 AI는 "공룡에 대한 짧은 이야기를 써 줘"라고 하면 <b>새 이야기를 써 줘요</b>(생성). 그런데 이야기가 아무리 그럴듯해도 공룡 사실이 틀릴 수 있어요. <b>매끄러움 ≠ 정확함</b>이에요.</p>
</div>

생성된 콘텐츠는 **그럴듯하지만 틀리거나 근거가 부족할** 수 있어요. 중요한 맥락이 빠졌거나, 오래된 정보를 쓰거나, 근거 없는 추론을 해도 자신 있게 들릴 수 있죠. 그렇다고 기술을 피하자는 게 아니라 **더 좋은 질문**을 하자는 거예요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">어떤 정보?</span>AI가 무엇을 근거로 써야 하나</div>
  <div class="oa-card"><span class="oa-card__icon">📤</span><span class="oa-card__title">어떤 출력?</span>무엇을 만들어야 하나</div>
  <div class="oa-card"><span class="oa-card__icon">🧑‍💼</span><span class="oa-card__title">누가 검토?</span>출력을 누가 확인하나</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🎯</span><span class="oa-card__title">얼마나 정확해야?</span>워크플로가 요구하는 정확성·근거 수준</div>
</div>

### 여섯 가지 흔한 애플리케이션 패턴 ⭐

이 패턴들은 **AI가 워크플로 안에서 무엇을 하는지**를 설명해요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">✍️</span><span class="oa-card__title">① 텍스트 생성</span>글을 <b>새로 쓰거나 다시 씀</b>: 초안·메시지·아이디어·설명·이메일·채용 공고·제안서·FAQ<br><small>예: 마케팅팀이 대상별로 제품 발표문 3가지 버전 작성</small></div>
  <div class="oa-card"><span class="oa-card__icon">📝</span><span class="oa-card__title">② 요약</span>긴 정보를 <b>핵심을 살려 짧게</b>: 회의·보고서·통화·리서치·경영진 브리핑<br><small>예: 갱신 미팅 전 긴 계정 이력을 위험과 다음 단계로 요약</small></div>
  <div class="oa-card"><span class="oa-card__icon">🏷️</span><span class="oa-card__title">③ 분류</span>정보에 <b>라벨·카테고리</b>를 붙임: 감정·의도·주제·우선순위·위험도<br><small>예: 티켓을 "결제 / 기술 문제 / 계정 접근 / 제품 피드백"으로 분류</small></div>
  <div class="oa-card"><span class="oa-card__icon">🎨</span><span class="oa-card__title">④ 이미지 생성</span>텍스트·이미지 입력으로 <b>이미지를 만들거나 편집</b>: 콘셉트·캠페인 비주얼·목업<br><small>예: 브랜드팀이 크리에이티브 브리프로 시즌 캠페인 콘셉트 생성</small></div>
  <div class="oa-card"><span class="oa-card__icon">💻</span><span class="oa-card__title">⑤ 코드 생성</span>코드를 <b>작성·설명·수정·리뷰·테스트</b>: 함수 초안, 오류 설명, 리팩터링<br><small>예: 엔지니어링팀이 테스트 케이스 생성 + 실패 원인 설명 요청</small></div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🔎</span><span class="oa-card__title">⑥ 검색 기반 답변</span>지식 출처에서 <b>찾아온 정보로</b> 답함: 사내 정책·제품 정보·릴리스 노트·지식 베이스<br><small>예: "출장비 정산 규정이 뭐예요?" → 승인된 정책 문서로 답변</small></div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🏫 학교로 비유하면</span>
  <p>① 독후감 <b>쓰기</b> ② 교과서 한 단원을 한 쪽으로 <b>요약</b> ③ 분리수거처럼 <b>종류별로 나누기</b> ④ 미술 시간 포스터 <b>그리기</b> ⑤ 코딩 수업 숙제 <b>도와주기</b> ⑥ 오픈북 시험처럼 <b>교과서를 펴 보고 답하기</b>. 마지막 ⑥은 "내 기억"이 아니라 "책에 있는 내용"으로 답해야 한다는 게 핵심이에요.</p>
</div>

### 패턴은 섞인다

고객은 AI 작업 하나가 아니라 **워크플로 전체**를 말하는 경우가 많아요. 한 칸에 억지로 넣으면 AI가 실제로 하는 일을 놓쳐요.

<div class="oa-svg">
<svg viewBox="0 0 720 190" role="img" aria-label="고객 지원 어시스턴트가 검색 기반 답변, 텍스트 생성, 분류, 요약을 조합하고 복잡한 건은 사람에게 넘기는 흐름">
  <defs>
    <marker id="ar6" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.a6{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.b6{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.t6{font-size:12px;fill:#4a4a4a;font-weight:700}.s6{font-size:11px;fill:#7a7a7a}</style>
  </defs>
  <text x="360" y="20" text-anchor="middle" class="t6">고객 지원 어시스턴트 = 패턴 여러 개의 조합</text>
  <rect x="10" y="40" width="125" height="70" rx="10" class="a6"/><text x="72" y="68" text-anchor="middle" class="t6">🔎 문서 검색</text><text x="72" y="88" text-anchor="middle" class="s6">검색 기반 답변</text>
  <rect x="155" y="40" width="125" height="70" rx="10" class="a6"/><text x="217" y="68" text-anchor="middle" class="t6">✍️ 답변 생성</text><text x="217" y="88" text-anchor="middle" class="s6">텍스트 생성</text>
  <rect x="300" y="40" width="125" height="70" rx="10" class="a6"/><text x="362" y="68" text-anchor="middle" class="t6">🏷️ 유형 분류</text><text x="362" y="88" text-anchor="middle" class="s6">분류</text>
  <rect x="445" y="40" width="125" height="70" rx="10" class="a6"/><text x="507" y="68" text-anchor="middle" class="t6">📝 대화 요약</text><text x="507" y="88" text-anchor="middle" class="s6">요약</text>
  <rect x="590" y="40" width="120" height="70" rx="10" class="b6"/><text x="650" y="68" text-anchor="middle" class="t6">🧑 사람에게</text><text x="650" y="88" text-anchor="middle" class="s6">라우팅(다음 단계)</text>
  <line x1="135" y1="75" x2="151" y2="75" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/>
  <line x1="280" y1="75" x2="296" y2="75" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/>
  <line x1="425" y1="75" x2="441" y2="75" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/>
  <line x1="570" y1="75" x2="586" y2="75" stroke="#c99400" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#ar6)"/>
  <text x="360" y="145" text-anchor="middle" class="s6">라우팅은 AI 활동의 "종류"가 아니라 워크플로에서 "다음에 일어나는 일"</text>
  <text x="360" y="168" text-anchor="middle" class="s6">질문: AI가 만들기 · 줄이기 · 나누기 · 그리기 · 코딩 · 출처로 답하기 중 무엇을 하나?</text>
</svg>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 너무 넓은 해석</span>
    <ul>
      <li>"지원팀이 고객 질문을 더 빨리 처리하게 AI로 도와주세요."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 패턴으로 읽은 해석 (의역)</span>
    <ul>
      <li>"관련 제품 정보를 <b>검색</b>하고, 답변 초안을 <b>생성</b>하고, 요청 유형을 <b>분류</b>하고, 상호작용을 지원팀용으로 <b>요약</b>하길 원하시는 거네요."</li>
    </ul>
  </div>
</div>

이렇게 읽으면 다음 질문이 보여요: 어떤 제품 정보를 쓸까? 어떤 요청 카테고리가 중요할까? 어떤 답은 바로 보내도 되고 어떤 답은 사람 검토가 필요할까? **찾은 정보가 없거나 애매하면** 어떻게 할까?

## 2. RAG — 개념 수준으로

고객이 이런 말을 하면 **검색 기반 답변**이 필요하다는 신호예요: "사내 지식 베이스에 RAG가 필요해요" / "정책 문서로 답할 수 있나요?" / "최신 제품 정보 기반으로 답해야 해요" / "일반 지식으로 지어내지 않았으면 해요".

### RAG가 뭔가요

**RAG = Retrieval-Augmented Generation(검색 증강 생성).** 두 가지 동작의 조합이에요.

<ol class="oa-flow">
  <li><strong>❓ 사용자 질문</strong>"출장비 정산 규정이 뭐예요?"</li>
  <li><strong>🔎 검색 (Retrieval)</strong>승인된 출처에서 관련 정보 찾기</li>
  <li><strong>✍️ 생성 (Generation)</strong>찾은 정보를 써서 답변 만들기</li>
</ol>

<div class="oa-eli">
  <span class="oa-eli__head">📒 쉽게 말하면 — 오픈북 시험</span>
  <p>그냥 시험은 <b>머릿속 기억</b>만으로 답해요. 기억이 흐리면 그럴듯하게 지어낼 수도 있죠. 오픈북 시험은 <b>교과서에서 관련 쪽을 먼저 찾아 읽고</b> 답을 써요. RAG가 바로 오픈북 시험이에요. 단, <b>교과서가 틀렸거나, 엉뚱한 쪽을 펼쳤거나, 읽고도 잘못 옮겨 쓰면</b> 답은 여전히 틀릴 수 있어요.</p>
</div>

지식 출처: 고객 문서, 회사 정책, 제품 가이드, 릴리스 노트, 지식 베이스 문서, 승인된 비즈니스 가이드, 그 밖에 관리·승인된 출처.

고객이 "RAG가 필요해요"라고 할 때 실제 뜻은 대개 **"우리 정보에 근거한 답이 필요해요"** 예요. 그 뜻을 알아듣는 게 중요해요. 임베딩·벡터 DB·청킹·인덱싱·검색 설정은 **구현 주제**라 이 코스에선 몰라도 돼요.

### 고객이 RAG를 쓰는 이유

기본 모델은 일반 작업엔 유용하지만, 고객 워크플로는 **특정하고 최신이고 승인된** 정보에 기대는 경우가 많아요. 이 필요는 ChatGPT, API로 만든 앱, 에이전트 워크플로 어디서든 나타나요. 출발 질문은 같아요: **AI가 어떤 출처 정보에 기대야 하나?**

| 이유 | 설명 | 예 |
|---|---|---|
| **고객 고유 지식** | 그 조직만의 정보로 답해야 함 | HR팀: 일반 HR 상식이 아니라 **승인된 사내 복리후생 정책**으로 답변 |
| **최신 정보** | 자주 바뀌는 정보 | 제품팀: 자주 바뀌는 **최신 릴리스 노트**로 답변 → 관리되는 출처 필요 |
| **근거 기반 (Grounding)** | 모델이 근거 없는 정보로 빈칸을 채울 가능성을 줄임 | 모든 답이 완벽해지는 건 아님. 관련 출처를 응답에 끌어오는 길이 생기는 것 |
| **투명성 지원** | 설계에 따라 응답을 출처와 연결하기 쉬워짐 | 사내 어시스턴트가 정책 답변과 함께 **해당 조항을 가리킴** → 언제 원문을 직접 봐야 할지 앎 |

### RAG가 보장하지 않는 것

RAG는 유용하지만 **정확성·안전·거버넌스의 완전한 해결책이 아니에요.**

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">❌</span><span class="oa-card__title">정답 보장 아님</span>엉뚱한 섹션 검색, 중요한 예외 누락, 출처보다 과장된 답 → 여전히 틀릴 수 있음</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">📄</span><span class="oa-card__title">출처 품질이 중요</span>오래되거나·불완전하거나·서로 충돌하거나·틀린 자료 → 답도 나쁨</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🎣</span><span class="oa-card__title">검색 품질이 중요</span>맞는 정책·제품 노트를 못 찾아오면 모델에 필요한 맥락이 없음</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🧑‍⚖️</span><span class="oa-card__title">검토는 여전히 필요</span>고위험·민감·규제·고객 대면 워크플로는 평가와 사람 검토 필요</div>
</div>

<div class="oa-bars">
  <div class="oa-bar"><span>사내 출장비 규정 답변</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:25%"></span><span class="oa-bar__val">상대적으로 낮음</span></span></div>
  <div class="oa-bar"><span>고객에게 하는 약속</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:65%"></span><span class="oa-bar__val">높음</span></span></div>
  <div class="oa-bar"><span>법률 해석 · 의료 결정 · 금융 추천</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:90%"></span><span class="oa-bar__val">매우 높음</span></span></div>
  <p class="oa-bars__caption">RAG를 써도 사람 검토가 얼마나 필요한지 보여 주는 개념적 표현이에요 (강의 수치 아님).</p>
</div>

> 안전하게 설명하기(의역): "RAG는 관련 출처 정보에 답을 근거하게 도와주지만, 그것만으로 정답을 보장하진 않아요. **출처 자료, 검색 단계, 검토 기대치**가 여전히 중요해요."

## 3. 애플리케이션 모양에서 솔루션 결정으로

패턴을 알아봤다면, 다음엔 **한 덩어리로 뭉치기 쉬운 세 가지 결정**을 나눠요. 이건 최종 추천이 아니에요. 애플리케이션 패턴은 **AI가 무엇을 하는지**만 알려 줄 뿐, 제품 표면·역량 설계·배포 모델을 저절로 정해 주지 않아요.

<div class="oa-eli">
  <span class="oa-eli__head">🍕 쉽게 말하면 — 피자 주문</span>
  <p>"피자 먹고 싶어"(패턴)라고 해도 세 가지는 따로 정해야 해요. ① <b>어디서 먹지?</b> 매장 · 배달 앱 · 집에서 직접(표면) ② <b>어떤 피자?</b> 토핑 · 크기 · 도우(역량) ③ <b>누가 굽고 어떻게 오지?</b> 매장 주방 · 배달 기사 · 우리 집 오븐, 그리고 위생 규칙(배포·운영). 배달 앱으로 주문해도 굽는 건 가게 주방이듯, <b>내 앱이 내 클라우드에 있어도 모델 추론은 OpenAI가 할 수 있어요.</b></p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 260" role="img" aria-label="세 가지 결정 레이어: 사용자·제품 표면, 역량·아키텍처, 배포·운영">
  <defs>
    <style>.a6b{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.b6b{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.c6b{fill:#fff7dc;stroke:#e8d48f;stroke-width:1.5}.t6b{font-size:13px;fill:#4a4a4a;font-weight:700}.s6b{font-size:11px;fill:#7a7a7a}</style>
  </defs>
  <rect x="10" y="10" width="225" height="240" rx="14" class="a6b"/>
  <text x="122" y="38" text-anchor="middle" class="t6b">🖥️ 사용자·제품 표면</text><text x="122" y="56" text-anchor="middle" class="s6b">"어디서 일하나?"</text>
  <text x="30" y="90" class="s6b">• ChatGPT Chat — 함께 생각·창작</text>
  <text x="30" y="114" class="s6b">• ChatGPT Work — 지식 업무 위임</text>
  <text x="30" y="138" class="s6b">• Codex — 소프트웨어 엔지니어링</text>
  <text x="30" y="162" class="s6b">• OpenAI API로 만든 앱·프로세스</text>
  <text x="122" y="230" text-anchor="middle" class="s6b">(강의 기준)</text>
  <rect x="248" y="10" width="225" height="240" rx="14" class="b6b"/>
  <text x="360" y="38" text-anchor="middle" class="t6b">⚙️ 역량·아키텍처</text><text x="360" y="56" text-anchor="middle" class="s6b">"시스템이 무엇을 해야 하나?"</text>
  <text x="268" y="90" class="s6b">• 생성 · 요약 · 분류 · 이미지 · 코드</text>
  <text x="268" y="114" class="s6b">• 검색(출처 정보) · 구조화 출력</text>
  <text x="268" y="138" class="s6b">• 앱·도구 사용 · 에이전트</text>
  <text x="268" y="162" class="s6b">• 여러 단계 조율 · 코드 실행</text>
  <text x="268" y="186" class="s6b">• 워크플로 일부 자동화</text>
  <rect x="486" y="10" width="224" height="240" rx="14" class="c6b"/>
  <text x="598" y="38" text-anchor="middle" class="t6b">☁️ 배포·운영</text><text x="598" y="56" text-anchor="middle" class="s6b">"어디서 돌고 누가 운영하나?"</text>
  <text x="506" y="90" class="s6b">• 앱은 어디서 실행?</text>
  <text x="506" y="114" class="s6b">• 모델 추론은 어디서?</text>
  <text x="506" y="138" class="s6b">• 각 구성요소 운영 주체는?</text>
  <text x="506" y="162" class="s6b">• 데이터는 어떻게 이동?</text>
  <text x="506" y="186" class="s6b">• 신원·권한·승인·보존·로깅·</text>
  <text x="506" y="204" class="s6b">  사람 검토 통제는?</text>
</svg>
</div>

**추론(inference)** = 학습된 모델이 입력을 받아 결과를 만드는 과정. 고객은 세 레이어에서 **선택을 섞을 수 있어요.** 서로 배타적인 길이 아니라 **확인할 질문**으로 다뤄요.

### 배포의 네 가지 흔한 형태

| 형태 | 설명 |
|---|---|
| **OpenAI 호스팅 추론** | OpenAI가 ChatGPT나 OpenAI API로 모델을 서비스 |
| **고객 클라우드 애플리케이션** | 고객 앱은 고객 클라우드에서 실행, **모델 추론은 여전히 OpenAI 호스팅일 수 있음** |
| **제3자 호스팅 서비스** | 다른 제공자가 지원 모델·서비스를 운영. 제공자별 약관·통제·데이터 경로·지원을 **확인 필요** |
| **고객 운영 오픈 웨이트 모델** | 오픈 웨이트 모델을 내려받아 고객(또는 고객이 고른 제공자)이 통제하는 인프라에서 실행. 모델 라이선스 조건 적용 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 흔한 오해</span>
    <ul>
      <li>"앱을 우리 클라우드에서 돌리니까 OpenAI 모델도 우리가 자체 호스팅하는 거죠?"</li>
      <li>"고객 클라우드 앱이면 반드시 오픈 웨이트 모델을 써야 해요."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 정확한 이해</span>
    <ul>
      <li><b>앱 호스팅과 모델 추론은 별개 결정</b>이에요. 고객 클라우드 앱도 OpenAI 호스팅 추론을 호출할 수 있어요.</li>
      <li>고객 운영 배포는 <b>지원되는 오픈 웨이트 모델</b>(또는 확인된 다른 방식)에 해당해요. ChatGPT·OpenAI API로 쓰는 <b>독점(proprietary) OpenAI 모델이 자체 호스팅 가능하다고 가정하면 안 돼요</b> (강의 기준).</li>
    </ul>
  </div>
</div>

### 결정을 형성하는 신호들

| 레이어 | 물어볼 것 | 신호 → 가능한 방향 (인식 단서일 뿐 최종 추천 아님) |
|---|---|---|
| **사용자·워크플로** | 누가 일하고, 경험은 어디에 있어야 하나? | 협업형 지식 업무 → ChatGPT Chat / 위임형 지식 업무 → ChatGPT Work / 소프트웨어 엔지니어링 → Codex / 임베디드·맞춤 경험 → API 앱 |
| **필요한 역량** | 시스템이 무엇을 해야 하나? | 생성·요약·분류·이미지·코드·검색 기반 답변·도구 사용·여러 단계 조율. **같은 역량이 여러 표면에** 나타날 수 있음 |
| **앱·추론·운영** | 앱/추론은 어디서? 운영 주체? 데이터 이동? 통제? | 고객이 앱을 통제한다고 독점 모델을 호스팅하는 건 아님 |
| **근거·출처 소유** | AI에 필요한 정보는? 승인된 출처는? 누가 최신으로 유지? | 출처 소유자가 분명해야 RAG가 의미 있음 |

> 좋은 인식 문장 예(의역): "이 워크플로는 검색 기반 답변과 요약을 조합해요. 첫 경험은 ChatGPT에서, 나중 포털 경험은 API로 만들 수 있어요. 포털은 고객 클라우드에서 돌고 추론은 OpenAI 호스팅으로 남을 수 있죠. 추천 전에 출처 소유·데이터 흐름·권한·검토 지점·현재 제품 가용성을 확인해야 해요."

### 예시 세 가지

| | 위임형 지식 업무 | 고객 클라우드 앱 | 고객 운영 오픈 웨이트 |
|---|---|---|---|
| **상황** | 재무팀이 승인된 보고서 + 표준 템플릿으로 월말 리뷰 초안을 받고 싶음 | 제품팀이 고객 포털 안에 AI 지원 답변을 넣고 싶음 | 모델 추론이 반드시 고객 통제 인프라에서 돌아야 함 |
| **패턴** | 검색 기반 답변 + 요약 + 텍스트 생성 (+ 여러 단계 조율, 결과물 생성) | 검색 기반 답변 + 분류 + 텍스트 생성 | 요약 · 분류 · 검색 기반 답변 등 |
| **표면** | ChatGPT Work — 지식 업무 덩어리를 맡기고 완성된 결과물을 검토 | OpenAI API로 만든 앱 | — |
| **배포** | 워크스페이스 | 포털은 고객 클라우드, 추론은 OpenAI 호스팅 | 지원되는 오픈 웨이트 모델을 고객 인프라에서, 또는 확인된 제3자 호스팅 |
| **확인할 것** | 데이터 권한, 워크스페이스 통제, 승인, 사람 검토 | 데이터 흐름, 권한, 출처 품질, 사람 검토, 제공자별 통제 | 독점 OpenAI 모델 자체 호스팅은 **아님** |

**깊이의 경계:** 상세한 제품 적합성·아키텍처·제공자 선택·가격·구현 계획은 이 코스 범위 밖이에요. 파트너의 역할은 **세 레이어를 알아보고, 무엇을 검증해야 하는지 정리하고, 약속 전에 적절한 제품·기술 전문가를 참여시키는 것**이에요.

## 4. 에이전트형 AI와 AGI — 조심스러운 언어

### 에이전트형 AI (Agentic AI)

목표를 향해 **단계를 계획하고, 도구를 쓰고, 행동을 조율하며** 더 큰 워크플로에 기여하는 AI 시스템을 흔히 이렇게 불러요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">💬 단순한 AI 상호작용</span>
    <ul>
      <li>질문 하나 → 답 하나</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🤖 에이전트형 워크플로</span>
    <ul>
      <li>정보 모으기 → 도구 쓰기 → 결과물 초안 → 진행 확인 → 필요할 때 승인 요청</li>
    </ul>
  </div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🧳 쉽게 말하면</span>
  <p>"부산 날씨 어때?"에 답하는 건 그냥 대답이에요. "주말 부산 여행 준비해 줘"라고 하면 날씨 확인 → 기차표 검색 → 숙소 비교 → 일정표 작성 → "이 숙소로 예약할까요?"라고 <b>물어보는</b> 것까지 여러 단계를 진행해요. 이게 에이전트형이에요. 그리고 마지막에 <b>"예약할까요?"라고 사람에게 묻는 지점</b>이 중요해요.</p>
</div>

- "에이전트형"을 **"고급 AI"의 동의어로 쓰지 마세요.** 핵심은 AI가 **응답 하나 이상**을 하며, 단계나 도구를 넘나들며 일을 진전시킨다는 것.
- 되묻기(의역): **"에이전트형이라고 하셨는데, AI가 어떤 행동을 하길 기대하시나요? 어떤 도구를 쓰고, 사람의 감독은 어디서 해야 하나요?"**

### AGI (범용 인공지능, Artificial General Intelligence)

OpenAI의 미션과 장기 연구 방향과 관련해 들을 수 있는 말이에요. 고객 대화에선 **장기 연구 목표**로 다루고, 현재의 제품 라벨·배포 경로·고객 솔루션으로 다루지 않아요.

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">지금 제품을 AGI라 부르지 않기</span>오늘의 제품·고객 앱은 AGI가 아님</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">AGI 역량 약속 금지</span>고객 워크플로에 AGI 수준을 약속하지 않음</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">"아주 유능한 AI"의 줄임말 아님</span>AGI를 지름길 표현으로 쓰지 않음</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🎯</span><span class="oa-card__title">워크플로로 돌아오기</span>실제 워크플로와 애플리케이션 패턴에 초점</div>
</div>

### 연습 — 애플리케이션 패턴 분석

<div class="oa-eli">
  <span class="oa-eli__head">✍️ 상황: HR 운영팀에 매니저들의 휴가 정책·온보딩·직원 서류 질문이 반복돼요</span>
  <p>지금은 매니저들이 정책 문서를 뒤지거나 HR 담당자에게 물어요. HR은 <b>승인된 사내 정책 문서</b>로 쉬운 답을 주길 원하고, <b>민감한 질문은 HR 검토로</b> 넘기고 싶어 해요. 나중엔 <b>매니저 셀프서비스 포털</b> 안에 넣고 싶다고 해요.</p>
  <p>틀: "워크플로는 [ ]. 패턴은 [ ]. 검색 기반 답변이 [필요/불필요]한 이유는 [ ]. 표면은 [ ], 필요한 역량은 [ ]. 확인할 배포·운영 질문은 [ ]. 검증할 담당자·출처는 [ ]."</p>
  <p>모범 예(의역): 워크플로는 <b>매니저의 HR 정책 정보 접근</b>. 패턴은 <b>검색 기반 답변 + 텍스트 생성 + (민감 질문을 HR로 넘기기 위한) 분류</b>. 답이 일반 지식이 아니라 승인된 정책 문서에서 나와야 하니 검색 기반 답변이 필요. 처음엔 직원이 직접 쓰는 <b>ChatGPT</b>, 나중 포털은 <b>API</b>로. 역량은 검색·생성·권한을 고려한 접근·사람에게 에스컬레이션. 확인할 것은 포털과 추론이 어디서 도는지, 누가 원문 문서를 관리하는지, 데이터 이동, 사람 검토 지점. 최종 표면과 운영 모델은 제품·기술 담당자가 검증.</p>
</div>

### 마지막에 계속 물을 질문들

<ol class="oa-flow">
  <li><strong>워크플로</strong>고객이 개선하려는 일은?</li>
  <li><strong>패턴</strong>AI가 무엇을 하나?</li>
  <li><strong>근거</strong>고객 고유 정보에 근거해야 하나?</li>
  <li><strong>표면</strong>사람·워크플로는 어디서?</li>
  <li><strong>역량</strong>무엇이 필요한가?</li>
  <li><strong>배포·운영</strong>앱·추론은 어디서, 누가, 무엇을 검증?</li>
</ol>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 고객이 들어오는 지원 티켓을 읽고 요청 유형별로 라벨을 붙이길 원해요. 가장 직접적인 패턴은?</summary>
  <p>① 분류 — 티켓마다 카테고리를 붙이므로 ② 코드 생성 — 소프트웨어 코드를 바꾸므로 ③ 텍스트 생성 — 새 고객 응답 초안을 쓰므로 ④ 이미지 생성 — 시각 출력을 만들므로</p>
  <p><b>정답: ①</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. 직원들이 수시로 바뀌는 승인된 사내 정책 문서를 근거로 답을 받아야 한다고 해요. 가장 정확한 말은?</summary>
  <p>① RAG를 쓰면 추가 검토 없이 모든 정책 답변이 정확해요 ② RAG는 주로 정책 예시 이미지를 만드는 데 써요 ③ RAG는 승인된 출처에 답을 근거하게 하지만, 검토는 여전히 중요해요 ④ RAG를 쓰면 정확한 원문 문서를 관리할 필요가 없어져요</p>
  <p><b>정답: ③</b> — ①④는 "RAG = 정답 보장"이라는 과장이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 고객이 자사 클라우드에서 지원 앱을 돌리며 모델 응답은 OpenAI API를 써요. 가장 정확한 말은?</summary>
  <p>① API를 쓰면 데이터 흐름·권한·사람 검토를 확인할 필요가 없어요 ② 앱 호스팅과 모델 추론은 별개 결정이에요. 앱은 고객 클라우드에서, 추론은 OpenAI 호스팅으로 남을 수 있어요 ③ 앱이 고객 클라우드에 있으니 독점 OpenAI 모델을 자체 호스팅하는 거예요 ④ 고객 클라우드 앱은 반드시 고객 운영 오픈 웨이트 모델을 써야 해요</p>
  <p><b>정답: ②</b></p>
</details>

## 정리

- **생성형 AI**는 입력으로 새 콘텐츠를 만든다. 그럴듯함 ≠ 정확함이니 **어떤 정보 · 어떤 출력 · 누가 검토 · 얼마나 정확해야**를 묻는다.
- 여섯 패턴(**텍스트 생성 · 요약 · 분류 · 이미지 생성 · 코드 생성 · 검색 기반 답변**)으로 AI가 하는 일을 이름 붙이고, 섞여 있으면 **조합으로** 말한다.
- **RAG = 검색 + 생성.** "RAG 필요해요"는 대개 "우리 정보에 근거한 답이 필요해요"라는 뜻. 고객 고유·최신 정보, 근거 기반, 투명성에 도움이 되지만 **정답을 보장하지 않는다** — 출처 품질, 검색 품질, 검토가 여전히 중요하다.
- 솔루션 이야기 전에 **사용자·제품 표면 / 역량·아키텍처 / 배포·운영** 세 결정을 나눈다. 고객은 레이어별로 섞어 고를 수 있다.
- **앱 호스팅 ≠ 모델 추론.** 고객 클라우드 앱도 OpenAI 호스팅 추론을 쓸 수 있고, 고객 운영 배포는 지원되는 **오픈 웨이트 모델** 이야기이지 독점 OpenAI 모델 자체 호스팅이 아니다.
- **에이전트형**은 "고급 AI"가 아니라 단계·도구를 넘나드는 일 — 어떤 행동·도구·감독 지점인지 되묻는다. **AGI**는 장기 연구 목표이지 현재 제품 라벨이 아니다.
