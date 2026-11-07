---
title: "API Solution Architecture"
title_ko: "API 솔루션 아키텍처 — 아이디어를 검토 가능한 설계 브리프로"
course: openai-api-deployment
lesson: 2
---

## 학습 목표

- 고객의 **업무 문제**를 API 아키텍처 요구사항으로 바꾸기 (기능 목록부터 시작하지 않기)
- **해결 범위(boundary)** — 할 것 / 안 할 것 / 아직 결정 안 된 것 — 을 명확히 하기
- API가 닿는 애플리케이션·데이터·도구·채널·승인 경로(**integration needs**) 그리기
- 7가지 아키텍처 패턴 중 **"최소한으로 충분한(minimum useful)" 패턴** 고르고 근거 설명하기
- 거버넌스·평가·관측성 가정을 더해 **API Solution Architecture Brief** 완성하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>문제 정의</strong>누가, 언제, 왜 힘든가 + 개선할 성과</li>
  <li><strong>범위·제약</strong>할 것 / 안 할 것 / 미정, 산업·정책 제약</li>
  <li><strong>통합 지도</strong>호출자 → 컨텍스트 → 모델 → 출력 → 리뷰</li>
  <li><strong>패턴 선택</strong>가장 단순하면서 충분한 패턴</li>
  <li><strong>준비도 가정</strong>거버넌스 · 평가 · 관측성</li>
  <li><strong>브리프 완성</strong>결정 가능한 12개 섹션</li>
</ol>

이 코스의 예시 시나리오는 하나로 이어져요: **금융사 RM(고객 담당 매니저)이 고객 미팅 전에 보는 "사전 브리핑"을 API로 자동 생성**하기.

## 1. 기능이 아니라 "업무 문제"에서 시작하기

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면</span>
  <p>집을 지을 때 "대리석 바닥이랑 엘리베이터 넣어주세요"부터 말하면 이상하죠. 먼저 "<b>4인 가족이 살고, 할머니가 계단을 힘들어하세요</b>"를 말해야 건축가가 엘리베이터가 정말 필요한지 판단할 수 있어요.</p>
  <p>"AI 비서 만들어줘", "에이전트 넣자", "retrieval 쓰자"는 대리석 바닥 얘기예요. 먼저 <b>문제</b>를 말해야 해요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 문제 정의</span>
    <ul>
      <li>"고객이 계정 관리용 AI를 원함"</li>
      <li>"생산성 향상", "영업에 AI 활용"</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 문제 정의</span>
    <ul>
      <li>"RM이 미팅 전 CRM 메모·지원 이력·내부 리서치·이전 회의록을 <b>수작업으로 모으느라</b> 시간이 너무 든다"</li>
    </ul>
  </div>
</div>

### 문제를 정의하는 5가지 질문

| 질문 | 사전 브리핑 예시 |
|---|---|
| ① 고객의 문제는? | 흩어진 계정 정보를 손으로 모으는 데 시간이 너무 듦 |
| ② 해결되면 무엇이 좋아지나? | 준비 시간 ↓, 브리핑 일관성 ↑, 승인된 정보에 근거 |
| ③ 워크플로우의 **어느 순간**에 API가 돕나? | RM이 미팅 전 계정 화면을 열 때 |
| ④ 누가/무엇이 쓰나? | RM(사람) + 계정 워크스페이스(시스템) |
| ⑤ 지금 겪는 고통은? | 느린 정보 수집, 품질 편차, 근거 없는 주장 위험 |

한 문장 템플릿: **"현재 [사용자]는 [이유] 때문에 [업무]가 힘들다. 고객은 [API가 돕는 단계]를 통해 [성과]를 개선하고 싶다."**

<div class="oa-eli">
  <span class="oa-eli__head">📏 성과는 "측정 가능하게"</span>
  <p>현재 기준값(baseline), 측정 지표, 판단할 사람을 정해요. 준비 시간은 <b>검토·수정 시간까지 포함</b>해서 재요. 기준값을 모르면 "아직 측정 안 됨"이라고 적어요. 그리고 <b>"많이 쓴다(adoption)"는 "시간이 줄었다"나 "정확하다"의 증거가 아니에요.</b></p>
</div>

## 2. 해결 범위(Solution boundary) 긋기

AI 솔루션은 쉽게 부풀어요. 브리핑 도우미가 어느새 CRM 수정 봇이 되고, 지원 도우미가 고객 대상 의사결정 엔진이 되죠. 그래서 경계를 **세 칸**으로 나눠요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">✅</span><span class="oa-card__title">In scope (1차 버전이 할 것)</span>내부용 브리핑 생성 · 승인된 정보만 사용 · RM 검토용 초안 반환 · 빠진/불확실한 정보 표시 · 출처 표기</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⛔</span><span class="oa-card__title">Out of scope (안 할 것)</span>투자 추천 · 고객에게 메시지 발송 · CRM 자동 수정 · 승인 없는 업무 생성 · 미승인 자료 사용 · RM 판단 대체</div>
  <div class="oa-card"><span class="oa-card__icon">❓</span><span class="oa-card__title">Not yet decided (미정)</span>retrieval이 필요한가? · 정형 스키마가 필요한가? · 승인된 출처 목록 · 접근 가능한 역할 · 로그 보관 기간</div>
</div>

경계에는 **사람이 끝까지 책임질 것(human-owned)** — 고객 커뮤니케이션, 최종 추천, 규제 대상 결정, 기록을 바꾸는 결정 — 도 명시해요. 경계는 "막는 벽"이 아니라 1차 버전을 **현실적으로 검토 가능하게** 만드는 도구예요.

## 3. 고객 제약과 산업 맥락

같은 패턴도 산업에 따라 위험도가 달라요.

| 산업 | 특히 신경 쓸 것 |
|---|---|
| 금융 | 출처 통제, 검토, 보존, **추천성 표현 제한** |
| 헬스케어 | 개인·규제 정보 처리 |
| 생명과학 | 근거 검토, 품질 기준 |
| 공공 | 접근성, 투명성, 조달, 데이터 저장 위치 |

아키텍처 단계에서 최종 SLA까지 정할 필요는 없지만, **패턴·인터페이스·대체 경로를 바꿀 수 있는 제약**은 적어 둬요.

| 제약 | 아키텍처 질문 | 사전 브리핑 예시 |
|---|---|---|
| 지연 허용치 | 얼마나 기다리면 업무가 깨지나? | 근거 있고 검토 가능하면 약간의 대기는 OK |
| 가용성·대체 경로 | API가 죽으면? | "브리핑 불가" 표시 → 수동 준비 계속 |
| 정보 신선도 | 얼마나 최신이어야? | 최근 CRM 메모·지원 이력의 기준 기간 검증 |
| 처리량 | 피크 때 요청 수는? | 파일럿 규모부터 확인 |
| 로그·보존 | 무엇을 남기면 안 되나? | 마스킹 규칙 확정 전엔 민감 정보 로그 금지 |
| 권한 확인 | 누구를 먼저 검증? | RM이 그 계정 정보 접근 권한이 있는지 |
| 지원 경로 | 장애는 누가? | 파일럿 전에 담당자 지정 |

<div class="oa-eli">
  <span class="oa-eli__head">⚠️ "설정으로 해결되겠지" 금지</span>
  <p>엔드포인트마다 <b>자격 조건</b>이 달라요. 강의 기준으로 Agents API는 <b>미국 데이터 저장(US residency)만 지원</b>하고 <b>ZDR(Zero Data Retention) 대상이 아니에요</b>(self-hosted sandbox여도). 비미국 저장이나 ZDR이 필수인 고객이면 "호스팅 우회"가 아니라 <b>다른 아키텍처</b>가 필요해요.</p>
  <p>헬스케어는 자격 있는 API 조직이 관리자를 통해 표준 BAA에 동의할 수 있지만, BAA 동의 = 앱 규정 준수가 <b>아니에요</b>. 서비스·데이터 흐름·설정이 각각 자격을 갖춰야 해요.</p>
</div>

## 4. 통합 지도(Integration map) 그리기

5가지 질문으로 API가 어디에 닿는지 찾아요: **누가 호출? / 어떤 시스템·데이터? / 출력은 어디로? / 그 응답에 기대는 다운스트림은? / 행동 전 어떤 승인?**

<div class="oa-svg">
<svg viewBox="0 0 720 200" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="사전 브리핑 통합 지도" font-family="inherit">
  <defs><marker id="arr2" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#c99400"/></marker></defs>
  <g font-size="12" text-anchor="middle">
    <rect x="10" y="70" width="110" height="56" rx="10" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="65" y="94" font-weight="700" fill="#4a4a4a">계정 워크스페이스</text>
    <text x="65" y="112" fill="#8a8a8a" font-size="11">호출자 · account ID</text>
    <rect x="150" y="70" width="110" height="56" rx="10" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="205" y="94" font-weight="700" fill="#4a4a4a">승인된 컨텍스트</text>
    <text x="205" y="112" fill="#8a8a8a" font-size="11">CRM·지원이력·리서치</text>
    <rect x="290" y="70" width="110" height="56" rx="10" fill="#ffefbf" stroke="#ffdd7a"/>
    <text x="345" y="94" font-weight="700" fill="#4a4a4a">모델 상호작용</text>
    <text x="345" y="112" fill="#8a8a8a" font-size="11">요약·구조화</text>
    <rect x="430" y="70" width="120" height="56" rx="10" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="490" y="94" font-weight="700" fill="#4a4a4a">정형 내부 브리핑</text>
    <text x="490" y="112" fill="#8a8a8a" font-size="11">필드 + review flags</text>
    <rect x="580" y="70" width="130" height="56" rx="10" fill="#f4fbf3" stroke="#cfe8c9"/>
    <text x="645" y="94" font-weight="700" fill="#4a4a4a">RM 검토 👤</text>
    <text x="645" y="112" fill="#8a8a8a" font-size="11">사용 전 사람 확인</text>
    <line x1="120" y1="98" x2="148" y2="98" stroke="#c99400" stroke-width="2" marker-end="url(#arr2)"/>
    <line x1="260" y1="98" x2="288" y2="98" stroke="#c99400" stroke-width="2" marker-end="url(#arr2)"/>
    <line x1="400" y1="98" x2="428" y2="98" stroke="#c99400" stroke-width="2" marker-end="url(#arr2)"/>
    <line x1="550" y1="98" x2="578" y2="98" stroke="#c99400" stroke-width="2" marker-end="url(#arr2)"/>
    <rect x="560" y="150" width="150" height="38" rx="10" fill="#fff5f3" stroke="#f3cfc6" stroke-dasharray="4 3"/>
    <text x="635" y="173" fill="#b0604a" font-size="11">CRM 업무 생성 (1차 범위 밖)</text>
    <line x1="645" y1="126" x2="645" y2="148" stroke="#d9a090" stroke-width="1.5" stroke-dasharray="3 3"/>
    <text x="360" y="40" fill="#8a8a8a" font-size="11">무엇이 호출을 시작하고 → 어떤 컨텍스트가 필요하고 → 모델이 무엇을 만들고 → 어디로 가고 → 어디서 검토되나</text>
  </g>
</svg>
</div>

**사람이 보는 출력**과 **다른 시스템이 자동으로 쓰는 출력**은 위험이 달라요. 자동으로 업무를 만들거나 기록을 바꾸면 구조·검증·에러 처리·리뷰가 더 강해야 해요.

<div class="oa-eli">
  <span class="oa-eli__head">🔔 알림 ≠ 허락</span>
  <p>MCP 도구는 <b>작업을 노출</b>하고, MCP Events는 <b>업데이트를 구독</b>해 워크플로우를 트리거할 수 있어요. 하지만 "알림이 왔다"는 것이 "기록을 바꿔도 된다"는 <b>권한은 아니에요</b>. 단계마다 데이터 주인과 권한을 명시해요. 사이트를 공유하거나 플러그인을 설치했다고 모든 소스·행동 권한이 생기는 것도 아니에요.</p>
</div>

이어서 최종 계약이 아닌 **요청/응답 스케치**를 만들어요. 핵심은 `status`, 브리핑 필드, `source_notes`, `review_flags`, 그리고 **`not_yet_decided` 목록**이에요.

```json
{
  "candidate_request": { "account_id": "required", "user_id": "required", "user_role": "required" },
  "candidate_response": {
    "status": "draft_ready | needs_review | cannot_generate",
    "briefing": { "account_summary": "...", "recent_support_themes": [], "preparation_questions": [] },
    "source_notes": [{ "source_type": "crm_notes", "source_reference": "...", "used_in_briefing": true }],
    "review_flags": ["missing_context", "conflicting_context", "recommendation_like_language"]
  },
  "not_yet_decided": ["final_response_schema", "whether_retrieval_is_required", "logging_and_retention_rules"]
}
```

## 5. API 아키텍처 패턴 7가지

<div class="oa-eli">
  <span class="oa-eli__head">🧰 쉽게 말하면</span>
  <p>패턴은 <b>공구함</b>이에요. 못 하나 박는데 전동 드릴·레이저 수평계·크레인을 다 꺼낼 필요는 없죠. 공구가 하나 늘 때마다 <b>배울 것, 고장 날 곳, 점검할 것</b>도 늘어나요.</p>
</div>

| 패턴 | 이럴 때 가치가 있음 | 새로 생기는 기술 결정 |
|---|---|---|
| **단순 모델 호출** | 요청 안에 필요한 정보가 다 있음 | 프롬프트, 요청 형태, 응답 처리, 기본 에러 |
| **Structured output** | 검토·라우팅·저장을 위해 고정 필드가 필요 | 스키마, 깨진 출력 처리, 검증 규칙 |
| **Retrieval-grounded** | 실행 시점에 승인된 내부 자료가 필요 | 출처 주인, 권한, 신선도, 검색 실패 처리 |
| **Tool / action** | 실시간 상태 조회, 계산, 기록 생성·수정 | 도구 계약, 실행 경계, 승인 게이트, 감사 이벤트 |
| **Agentic workflow** | 여러 시스템을 넘나드는 다단계 작업 | 상태, 도구 순서, 에스컬레이션, 모니터링, 롤백 |
| **Realtime / multimodal** | 음성·이미지·실시간 대화가 꼭 필요 | 지연, 동의, 미디어 품질, 대체 채널, 보존 |
| **Hybrid** | 패턴 하나로는 부족 | 조정, 겹치는 실패 모드, 지원 책임 |

복잡도는 아래로 갈수록 커져요.

<div class="oa-bars">
  <div class="oa-bar"><span>단순 모델 호출</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:15%"></span><span class="oa-bar__val">낮음</span></span></div>
  <div class="oa-bar"><span>Structured output</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:28%"></span></span></div>
  <div class="oa-bar"><span>Retrieval</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:48%"></span></span></div>
  <div class="oa-bar"><span>Tool / action</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:60%"></span></span></div>
  <div class="oa-bar"><span>Realtime / multimodal</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:68%"></span></span></div>
  <div class="oa-bar"><span>Agentic</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:85%"></span></span></div>
  <div class="oa-bar"><span>Hybrid</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:95%"></span><span class="oa-bar__val">높음</span></span></div>
  <p class="oa-bars__caption">검증·운영 부담을 이해하기 위한 개념적 순서예요 (강의 수치 아님).</p>
</div>

### 헷갈리기 쉬운 구분

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">Decisions API vs 구조화 생성</span>Decisions API(퍼블릭 베타)는 확률·선택지·등급 같은 <b>고정 예측</b>을 돌려줘요. 브리핑 같은 <b>서술 초안</b>은 못 써요. 모델이 쓴 "자신감 메모"나 확률 필드 모두 <b>정답의 증거는 아님</b>.</div>
  <div class="oa-card"><span class="oa-card__icon">🔁</span><span class="oa-card__title">누가 실행을 책임지나</span><b>앱 소유 루프</b>: Responses로 출력·도구 요청을 받고, 앱이 검증·실행·계속 여부 결정. <b>Conversations</b>는 상태를 저장할 뿐 규칙을 집행하지 않음. <b>Agents API</b>: 관리형 런타임과 세션 제공.</div>
  <div class="oa-card"><span class="oa-card__icon">🏗️</span><span class="oa-card__title">호스팅은 별개 결정</span>OpenAI 호스팅/self-hosted sandbox는 도구가 <b>어디서 실행되는지</b>만 바꿔요. Bedrock Managed Agents, Agents API, Responses 도구 루프, Codex SDK, App Server는 서로 <b>다른 통합 선택지</b>예요.</div>
</div>

## 6. 최소한으로 충분한 패턴 고르기

**Minimum useful pattern**은 "가장 약한 것"이 아니라, **불필요한 능력 없이 일을 해내는 가장 명확한 해법**이에요.

<ol class="oa-flow">
  <li><strong>앱이 컨텍스트를 다 줌?</strong>→ 단순 호출 / structured output</li>
  <li><strong>고정 필드 필요?</strong>→ structured output 추가</li>
  <li><strong>요청에 없는 승인 자료 필요?</strong>→ retrieval 고려</li>
  <li><strong>상태 조회·기록 변경?</strong>→ tool/action 고려</li>
  <li><strong>여러 단계·도구·승인 조율?</strong>→ agentic 고려</li>
  <li><strong>음성·이미지·실시간이 본질?</strong>→ realtime/multimodal</li>
</ol>

둘 이상 필요하면 **각각이 왜 필요한지** 설명해요. 그리고 "나중에 범위가 커지면 추가할 것"(예: CRM 업무 생성)도 따로 적어 둬요.

### 패턴 근거(rationale) 4단 문장

> **우리는 [패턴]을 추천한다. 왜냐하면 [업무 필요] 때문이다. 이것은 [가능해지는 것]을 가능하게 한다. 하지만 [해결 못 하는 것]은 해결하지 않는다. 구축 전에 [열린 질문]을 검증해야 한다.**

사전 브리핑 예: "승인된 정보에 근거한 일관된 내부 브리핑이 필요하므로 **structured output**(출처 검증에 따라 retrieval 조건부)을 추천. 워크스페이스에 검토 가능한 필드를 보여줄 수 있음. 출처 승인·권한·컴플라이언스는 해결 못 함. 구축 전, 앱이 승인된 컨텍스트를 다 줄 수 있는지 vs retrieval이 필요한지 검증."

### 아키텍처 결정 지점(decision points)

| 결정 지점 | 사전 브리핑에서는 |
|---|---|
| 가장 단순한 패턴은? | Structured output |
| 모델 상호작용은 어디서? | 제공된 컨텍스트로 브리핑 생성 |
| 비즈니스 컨텍스트는 어디로 들어오나? | 앱이 요청에 담거나, API가 승인 자료를 검색 |
| 도구·시스템 행동은? | 1차 버전엔 없음 (CRM 수정은 범위 밖) |
| 사람은 어디서 검토? | 사용 전 RM 검토 |
| 출시 후 무엇이 보여야? | 요청 상태, 출처 실패, review flag, 피드백, 지연 |
| 구축 전 검증할 것은? | 승인 출처, 권한, 스키마, 로그·보존, 보안 리뷰 |

## 7. 준비도 가정 — 거버넌스 · 평가 · 관측성

"해피 패스"만 적힌 설계는 믿을 수 없어요. 네 가지 질문을 붙여요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🏛️</span><span class="oa-card__title">거버넌스</span>무엇에 접근하고, 누가 승인하나? 데이터가 있다고 쓸 수 있는 게 아님. <b>쓰기(write)</b>가 있으면 승인·감사 기준이 더 높아짐.</div>
  <div class="oa-card"><span class="oa-card__icon">🧪</span><span class="oa-card__title">평가</span>어떤 행동을 테스트해야 하나? 좋은 출력 / 나쁜 출력 / 에스컬레이션 조건 / 나중에 만들 세부 eval.</div>
  <div class="oa-card"><span class="oa-card__icon">🔭</span><span class="oa-card__title">관측성</span>사용 후 무엇이 보여야 하나? 요청·출처·도구 상태, review flag, 에러, 피드백, 지연.</div>
  <div class="oa-card"><span class="oa-card__icon">🚦</span><span class="oa-card__title">프로덕션 준비</span>더 많은 사용자에게 보여주기 전 무엇이 참이어야? 검토 담당자, 대체 동작, 로그 규칙, 지원 담당.</div>
</div>

"리뷰 필요"보다 **"RM이 고객 사용 전 내부 브리핑을 검토, 추천성 표현은 컴플라이언스 검토 가능"**처럼 다음 대화를 이끌 만큼 구체적으로 써요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 좋은 출력</span>
    <ul>
      <li>계정 요약·지원 테마·이전 미팅·준비 질문</li>
      <li>출처 메모</li>
      <li>정보 부족 시 review flag</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 나쁜 출력</span>
    <ul>
      <li>근거 없는 주장, 투자 조언</li>
      <li>미승인 출처 사용, flag 누락</li>
      <li>정보가 부족한데 과신하는 답</li>
    </ul>
  </div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🔍 "끝났다"는 신호 ≠ "성공했다"는 증거</span>
  <p>턴이 완료됐다, 신뢰도 필드가 있다, 승인 메시지가 왔다, 재연결됐다 — 이것만으로는 <b>원하던 비즈니스 결과가 나왔다는 증거가 아니에요</b>. 관리형·이벤트 기반 작업이면 요청/이벤트 → 세션 → 승인 → 도구 결과 → 검증된 결과를 연결하고, 결과를 모를 때 <b>재시도 전에 누가 조사할지</b> 정해요.</p>
</div>

거버넌스에서 추가로 기억할 것:

- **ZDR + PSP(Private Safety Processing)**는 고객 통제 스토리지를 쓰지만 "아무 데도 안 남는다"는 뜻이 아니에요. 스토리지 접근·보존·삭제 담당자를 지정해요.
- **신원 · 데이터 접근 · 행동 권한 · 결제**는 서로 분리해서 생각해요. "Sign in with ChatGPT"가 API 키를 공유하거나 API 사용을 구독료로 처리해 주는 건 아니에요.
- 호스팅 브라우저에서 출처 승인이나 확인 함수가 있어도 **모든 중요한 행동 전 멈춤이 보장되진 않아요.** 확실한 통제는 행동 가능한 자원을 제한하거나 통제된 실행 환경을 써야 해요.

## 8. API Solution Architecture Brief — 12개 섹션

| # | 섹션 | 핵심 |
|---|---|---|
| 1 | 고객 문제·비즈니스 성과 | 기준값 미측정이면 그렇게 표시 |
| 2 | 워크플로우 순간·행위자 | 언제, 누가/무엇이 호출 |
| 3 | 산업·고객 제약 | 금융: 승인 출처, 추천 제한 |
| 4 | 해결 범위 | in / out / not-yet-decided |
| 5 | 통합 필요 | 호출자, 입력, 후보 출처, 출력 채널, 검토 지점 |
| 6 | 애플리케이션 패턴 | 1순위 + 조건부 패턴 |
| 7 | 아키텍처 결정 지점 | 인터페이스·상태·실행·호스팅·트리거·데이터 주인·승인 지점 → 문서화/고객 검증/미결 표시 |
| 8 | 컨텍스트·데이터 가정 | 권한, 민감도, 신선도 |
| 9 | 모델·역량 요구사항(인계용) | 품질·지연·비용 기준만, **모델 확정은 나중에** |
| 10 | 도구·행동 가정 | 지금 필요 / 불필요 / 나중에 |
| 11 | 평가·거버넌스·관측성·준비도 | 탐색해야 할 가정 목록 |
| 12 | 열린 리스크·검증 질문 | 첫 번째 기술 검증 단계 |

### 인계(handoff)용으로 쓸모 있게 만드는 규칙

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">✂️</span><span class="oa-card__title">짧게</span>회의 전에 읽을 수 있는 분량</div>
  <div class="oa-card"><span class="oa-card__icon">🚫</span><span class="oa-card__title">미검증 구현 디테일 금지</span>특정 모델·엔드포인트·DB를 아는 척 X. 단, <b>문서화된 자격 제한</b>은 "제외됨"으로 바로 기록</div>
  <div class="oa-card"><span class="oa-card__icon">🏷️</span><span class="oa-card__title">Known / Assumed / Unvalidated 분리</span>사실과 가정을 섞지 않기</div>
  <div class="oa-card"><span class="oa-card__icon">➡️</span><span class="oa-card__title">다음 검증 단계</span>예: 데이터 주인과 출처 검증, 정제 예시로 structured output 프로토타입</div>
  <div class="oa-card"><span class="oa-card__icon">📐</span><span class="oa-card__title">다음에 정할 계약</span>요청·응답 필드, 에러 상태, 상태 가정, 인증·인가, 스트리밍/비동기</div>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 결정 불가능한 브리프</span>
    <ul>
      <li>"계정 데이터로 AI가 요약해서 RM 준비를 도움"</li>
      <li>범위 밖, 출처, 리뷰, 나쁜 출력, 다음 단계가 없음</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 결정 가능한 브리프</span>
    <ul>
      <li>문제 + 1차 범위 + 하지 않을 것</li>
      <li>structured output, retrieval은 조건부</li>
      <li>CRM 수정은 범위 밖, 구축 전 검증 목록 명시</li>
      <li>"성공한 실행이 있었다"고 암시하지 않음</li>
    </ul>
  </div>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 업무 요구사항과 섣부른 기술 선택을 가장 잘 구분한 것은?</summary>
  <p>① 승인된 계정 정보로 RM의 미팅 준비를 돕는 내부 브리핑 API, 사용 전 사람 검토 ② 시간 절약을 위해 CRM 자동 수정 + 고객 이메일 발송 ③ 고급 기능이니 retrieval·에이전트·실시간 음성 사용 ④ 금융은 중요하니 가장 강력한 모델</p>
  <p><b>정답: ①</b> — 나머지는 범위 확장, 기능 나열, 모델부터 고르기예요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 사용자가 붙여넣은 짧은 지원 메모를 요약해 팀장이 검토한다. 출처 검색·시스템 수정·다단계·음성이 필요 없다. 첫 패턴은?</summary>
  <p><b>정답: 단순 모델 호출 또는 structured output.</b> (Decisions API는 서술 요약을 대체 못 하고, agentic·retrieval은 불필요한 복잡도예요.)</p>
</details>

<details class="oa-quiz">
  <summary>Q3. "API가 계정 데이터로 사전 브리핑을 만들어 RM에게 보여준다." 가장 중요한 빠진 가정은?</summary>
  <p><b>정답: 누가 데이터에 접근하고, 출력을 검토하고, 실패를 살펴보는가.</b> (모델 인기, 말투, 프로그래밍 언어가 아니라요.)</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 가장 강한 아키텍처 브리프는?</summary>
  <p><b>정답:</b> "승인된 계정 정보로 계정 워크스페이스에서 내부 사전 브리핑을 생성하고, 사용 전 RM이 검토한다." — 출처·권한을 나중으로 미루거나, "최고 모델 + 나중에 다 추가"는 결정 불가능한 브리프예요.</p>
</details>

## 정리

- 아키텍처는 **기능 목록이 아니라 업무 문제**에서 시작한다. 성과는 측정 가능하게.
- 범위는 **in / out / not-yet-decided** + 사람이 책임질 것.
- 산업·정책·엔드포인트 **자격 조건**은 설정 문제가 아니라 아키텍처 문제일 수 있다.
- 통합 지도로 **호출 → 컨텍스트 → 모델 → 출력 → 검토** 경로를 보이게.
- **Minimum useful pattern**: 기능이 늘 때마다 검증·운영 부담도 는다. 근거는 4단 문장으로.
- 거버넌스·평가·관측성·준비도 가정을 붙여 **결정 가능한 브리프**를 만든다.
