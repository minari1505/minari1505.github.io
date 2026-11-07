---
title: "Context, Data, and Retrieval for Grounded API Solutions"
title_ko: "컨텍스트·데이터·검색 — 근거 있는 답을 만드는 Retrieval 설계"
course: openai-api-deployment
lesson: 6
---

## 학습 목표

- Retrieval(검색 기반 근거 제공)이 **필요한 경우 / 필요 없는 경우 / 보류할 경우**를 판단하기
- API가 알아야 할 지식과 **알아도 되는 지식**(knowledge access)을 정의하기
- 후보 소스를 권위·최신성·완전성·권한·민감도로 평가해 **포함 / 제외 / 검증 필요**로 분류하기
- RAG, 임베딩, 벡터 검색, 메타데이터, 권한 인식 검색이 각각 **무엇을 해 주고 무엇을 증명하지 못하는지** 설명하기
- 근거 있는 출력(grounded output)의 검증 테스트를 정의하고 **Context, Knowledge Access, and Retrieval Design Plan**으로 묶기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>지식 요구 정의</strong>무엇을 알아야 하고, 틀리면 무슨 일이?</li>
  <li><strong>Retrieval 적합성</strong>필요 / 불필요 / 보류</li>
  <li><strong>소스 준비도</strong>포함 · 제외 · 검증 필요</li>
  <li><strong>패턴 선택</strong>RAG · 임베딩 · 메타데이터 · 권한</li>
  <li><strong>출력 검증</strong>맞는 소스? 근거 안에서 답?</li>
  <li><strong>설계 플랜</strong>진행 / 조건부 / 보류 / 회피</li>
</ol>

<div class="oa-eli">
  <span class="oa-eli__head">📚 쉽게 말하면</span>
  <p>Retrieval은 <b>오픈북 시험</b>이에요. 모델이 머릿속 기억(일반 지식)만으로 답하는 대신, 시험장에 가져간 <b>허락된 책</b>을 펼쳐서 답을 찾죠.</p>
  <p>그런데 오픈북이라고 다 좋은 건 아니에요. 가져간 책이 <b>작년 교과서</b>라면? <b>친구 낙서 노트</b>라면? <b>선생님용 정답지</b>(보면 안 되는 자료)라면? 이 코스는 "어떤 책을, 누가, 어떤 조건으로 펼쳐도 되는지"를 정하는 법을 배워요.</p>
</div>

## 1. 지식 요구부터 — "문서를 연결하자"가 아니라

나쁜 출발은 "회사 문서를 모델에 연결하자"예요. 좋은 출발은 **업무에 필요한 지식 + 틀리면 생기는 일**을 적는 것.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 너무 막연함</span>
    <ul><li>"어시스턴트가 회사 정책에 접근해야 해요."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 구체적</span>
    <ul><li>"직원의 연차·이월·에스컬레이션 질문에 답하려면 <b>현재 연차 정책 + 지역별 휴일 정책</b>이 필요하다. 답이 일반적이거나 낡으면 직원이 잘못 안내받고 HR이 수습해야 한다."</li></ul>
  </div>
</div>

| 플랜 항목 | 적을 내용 |
|---|---|
| Workflow step | API가 돕는 구체적 업무 순간 |
| Required business knowledge | 필요한 정책·기록·문서·규칙 |
| Source expectation | 어떤 소스가 답의 근거여야 하나 |
| Accuracy risk | 일반적·낡은·근거 없는 답이면 생기는 문제 |
| Action affected | 그 답으로 사람/시스템이 하는 행동 |
| Evidence need | 출처 표기·검토 메모가 필요한가 |
| **Output boundary** | API가 **말하거나 추론하면 안 되는 것** |

마지막 줄이 핵심이에요. 컨텍스트 설계는 "무엇을 쓸 수 있나"만큼 **"근거가 없을 때 무엇을 말하지 말아야 하나"**도 정하는 일입니다.

## 2. Retrieval이 정말 필요한가?

Retrieval은 소스 선정·접근 통제·메타데이터·최신성·품질 테스트·유지보수를 **한꺼번에 떠안는 설계 약속**이에요. "더 고급스러워 보여서" 쓰면 안 됩니다.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">✅</span><span class="oa-card__title">Retrieval needed</span>승인된·최신·권한 있는 사내 지식이 답의 근거여야 할 때<br><small>예: 지역별 HR 정책 Q&A</small></div>
  <div class="oa-card"><span class="oa-card__icon">➖</span><span class="oa-card__title">Retrieval not needed</span>요청에 필요한 정보가 다 들어 있거나, 분류·요약·변환이면 충분할 때<br><small>예: 티켓 문장 분류, 붙여넣은 문단 요약</small></div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⏸️</span><span class="oa-card__title">Defer pending validation</span>쓸모는 있어 보이지만 소스 승인·소유자·최신성·권한이 미해결일 때<br><small>예: 초안 섞인 지역별 가격표</small></div>
</div>

### 판단 흐름

<div class="oa-svg">
<svg viewBox="0 0 720 260" role="img" aria-label="Retrieval 적합성 판단 흐름도">
  <defs>
    <marker id="ar6" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.q6{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.g6{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.n6{fill:#f6f3ea;stroke:#e5dfcf;stroke-width:1.5}.w6{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.t6{font-size:12px;fill:#4a4a4a}.s6{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="20" width="200" height="50" rx="10" class="q6"/><text x="110" y="42" text-anchor="middle" class="t6">요청 밖의 사내 지식이</text><text x="110" y="58" text-anchor="middle" class="t6">필요한가?</text>
  <rect x="10" y="190" width="200" height="50" rx="10" class="n6"/><text x="110" y="212" text-anchor="middle" class="t6">➖ Retrieval 불필요</text><text x="110" y="228" text-anchor="middle" class="s6">프롬프트·구조화 출력으로</text>
  <line x1="110" y1="70" x2="110" y2="186" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/><text x="118" y="130" class="s6">아니오</text>
  <rect x="260" y="20" width="200" height="50" rx="10" class="q6"/><text x="360" y="42" text-anchor="middle" class="t6">소스가 승인·소유자·최신·</text><text x="360" y="58" text-anchor="middle" class="t6">권한 모두 확인됐나?</text>
  <line x1="210" y1="45" x2="256" y2="45" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/><text x="216" y="38" class="s6">예</text>
  <rect x="260" y="190" width="200" height="50" rx="10" class="w6"/><text x="360" y="212" text-anchor="middle" class="t6">⏸️ 소스 검증까지 보류</text><text x="360" y="228" text-anchor="middle" class="s6">검증 담당자·필요 증거 기록</text>
  <line x1="360" y1="70" x2="360" y2="186" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/><text x="368" y="130" class="s6">아니오</text>
  <rect x="510" y="20" width="200" height="50" rx="10" class="q6"/><text x="610" y="42" text-anchor="middle" class="t6">가치 &gt; 복잡도이고,</text><text x="610" y="58" text-anchor="middle" class="t6">출력을 검증할 수 있나?</text>
  <line x1="460" y1="45" x2="506" y2="45" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/><text x="466" y="38" class="s6">예</text>
  <rect x="510" y="190" width="200" height="50" rx="10" class="g6"/><text x="610" y="212" text-anchor="middle" class="t6">✅ Retrieval 필요</text><text x="610" y="228" text-anchor="middle" class="s6">소스·권한·테스트와 함께 핸드오프</text>
  <line x1="610" y1="70" x2="610" y2="186" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/><text x="618" y="130" class="s6">예 (아니면 단순 패턴/보류)</text>
</svg>
</div>

Retrieval을 고르면 다른 설계에도 파급돼요: **계약**(출처 필드·검토 플래그), **보안**(역할별 접근), **상태**(멀티턴에서 무엇을 유지할지), **도구·행동**(검색 결과가 행동을 일으키면 승인 경로), **평가·가드레일**, **관측성**(검색 실패·최신성·지연·비용).

## 3. 지식 접근 — 알아야 할 것 vs 알아도 되는 것

<div class="oa-eli">
  <span class="oa-eli__head">🔑 쉽게 말하면</span>
  <p>같은 학교 도서관이라도 <b>학생은 일반 서가</b>, <b>선생님은 교무실 자료</b>까지 볼 수 있죠. "더 많은 자료 = 더 좋은 답"이 아니라, <b>이 사용자가 볼 수 있는 자료만</b> 펼쳐야 해요. 이게 <b>권한 인식 검색(permission-aware retrieval)</b>이에요.</p>
</div>

| 소스 결정 | 의미 | 예시 |
|---|---|---|
| **Include** | 관련·승인·소유자 있음·충분히 최신·권한 OK | HR이 승인한 현재 직원 정책 페이지 |
| **Exclude** | 이 워크플로우에 쓰면 안 됨 | 초안 메모, 낡은 교육 자료, 경영진 기밀 메모 |
| **Validate** | 유용할 수 있으나 승인·소유자·최신성·권한 미해결 | 소유자도 시행일도 불명확한 지역 부록 |

권한은 역할·부서·지역·고객/테넌트·제품군·데이터 등급·계약 제한·환경(dev/staging/prod) 등에 따라 달라질 수 있어요. **권한 동작은 가정하지 말고 검증**합니다. 예: 같은 API를 쓰더라도 지원 매니저와 일선 상담원은 볼 수 있는 고객 기록·내부 메모가 다를 수 있어요.

## 4. 소스 준비도 — "관련 있음" ≠ "써도 됨"

후보 소스는 **후보일 뿐**이에요. 관련성 말고도 10가지를 봅니다.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__title">👤 Owner</span>승인·최신 여부를 확인해 줄 사람</div>
  <div class="oa-card"><span class="oa-card__title">✔️ Approval</span>공식? 초안? 보관본? 내부용?</div>
  <div class="oa-card"><span class="oa-card__title">🕒 Freshness</span>언제 갱신? 얼마나 자주 바뀌나?</div>
  <div class="oa-card"><span class="oa-card__title">🧩 Completeness</span>예외·지역 차이까지 다루나?</div>
  <div class="oa-card"><span class="oa-card__title">🔐 Permissions</span>이 역할·지역·테넌트가 봐도 되나?</div>
  <div class="oa-card"><span class="oa-card__title">📄 Format</span>스캔본? 조각난 문서? 파싱 가능?</div>
  <div class="oa-card"><span class="oa-card__title">⚠️ Sensitivity</span>개인·규제·기밀 정보 마스킹 필요?</div>
  <div class="oa-card"><span class="oa-card__title">🏛️ Source of truth</span>원본인가, 사본·요약·교육 자료인가?</div>
  <div class="oa-card"><span class="oa-card__title">🔄 Update cadence</span>누가 언제 갱신하고 검색에 어떻게 반영?</div>
  <div class="oa-card"><span class="oa-card__title">⚖️ Conflict rule</span>소스끼리 다르면 누가 이기나?</div>
</div>

### 소스 유형별 주의점

| 소스 유형 | 쓸모 | 조심할 점 |
|---|---|---|
| 승인된 정책 문서 | 공식 규칙·절차 | 지역·역할·시행일별로 다름 |
| 사내 지식베이스 | 운영 가이드 | 낡거나 중복되거나 비공식 |
| 구조화 기록 (계정·티켓) | 실제 데이터 | 역할별 접근, 최소화·민감도 |
| 지원 아티클 | 문제 해결 | 고객용과 내부용이 섞임 |
| 과거 사례 | 처리 예시 | 현재 정책과 다를 수 있음 |
| 운영 시스템 | 실시간 기록 | 검색이 아니라 도구·시스템 접근 결정과 겹침 |

<div class="oa-eli">
  <span class="oa-eli__head">🚨 준비 안 된 소스가 만드는 사고</span>
  <p>낡은 안내 인용 · 초안을 승인본처럼 사용 · 충돌하는 소스를 설명 없이 섞음 · 볼 권한 없는 정보 노출 · 불완전한데 자신 있게 답함 · 관련은 있지만 권위 없는 소스 인용.</p>
  <p>→ Retrieval은 <b>약한 소스를 믿을 만한 소스로 바꿔 주지 않아요.</b></p>
</div>

## 5. RAG · 임베딩 · 벡터 검색 · 메타데이터

### RAG (Retrieval-Augmented Generation)

<ol class="oa-flow">
  <li><strong>요청</strong>사용자/시스템이 질문</li>
  <li><strong>관련 소스 판단</strong>어떤 내용이 필요할까</li>
  <li><strong>검색</strong>승인된 소스에서 찾기</li>
  <li><strong>컨텍스트 전달</strong>찾은 내용을 모델에</li>
  <li><strong>답 생성</strong>근거 기반으로 작성</li>
  <li><strong>반환</strong>답 + 출처·검토 플래그</li>
</ol>

RAG의 목적은 "모든 답을 자동으로 맞게" 만드는 게 아니라, **검토·테스트 가능한 근거**를 주는 거예요.

| RAG 패턴 | 쓸 때 | 피하거나 보류할 때 |
|---|---|---|
| 단일 소스 근거 | 하나의 공식 원본이면 될 때 | 여러 소스에 꼭 필요한 예외·지역 차이가 있을 때 |
| 다중 소스 근거 | 여러 승인 소스를 비교·종합해야 할 때 | 충돌 해결 규칙이 없을 때 |
| Retrieval + 구조화 출력 | 근거가 정해진 필드로 하위 시스템에 가야 할 때 | 스키마가 불명확할 때 |
| Retrieval + 사람 검토 | 고객·민감 업무·정책 해석에 영향 | 검토자·에스컬레이션 경로가 없을 때 |
| 보류(Deferred) | 쓸모는 있지만 소스·권한·메타데이터 미준비 | 리스크 해결 없이 진행하자는 압박이 있을 때 |

### 임베딩과 벡터 검색

<div class="oa-eli">
  <span class="oa-eli__head">🗺️ 쉽게 말하면</span>
  <p><b>임베딩</b>은 문장의 "뜻"을 숫자 좌표로 바꾼 거예요. 뜻이 비슷한 문장은 지도에서 <b>가까운 곳</b>에 찍혀요. <b>벡터 검색</b>은 질문의 좌표와 가까운 문서를 찾는 거죠.</p>
  <p>그래서 사용자가 "현장 가이드"라고 물어도, 공식 문서에 "영업 담당자 토킹 포인트"라고 적혀 있으면 찾아낼 수 있어요. 단어가 달라도 <b>뜻이 가까우니까</b>.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">👍 임베딩·벡터 검색이 해 주는 것</span>
    <ul>
      <li>단어가 달라도 뜻이 비슷한 내용 찾기</li>
      <li>큰 문서 더미에서 자연어 질문으로 검색</li>
      <li>사용자가 공식 용어를 몰라도 OK</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🚫 증명하지 못하는 것</span>
    <ul>
      <li>정확성 · 권위(원본인가)</li>
      <li>최신성 · 완전성</li>
      <li>권한 준수 · 규제 업무 적합성</li>
    </ul>
  </div>
</div>

벡터 검색의 함정: **비슷하지만 틀린 소스**, **비슷하지만 옛날 버전**, **비슷하지만 볼 권한 없는 문서**, **질문의 일부만 답하는 구절**, 그리고 비슷한 텍스트로 그럴듯하게 지어낸 **거짓 확신**.

### 메타데이터와 권한 인식 검색

메타데이터는 소스에 붙은 **꼬리표**예요 (제품·지역·버전·시행일·승인 상태·대상·민감도…). 검색 범위를 좁혀서 "뜻은 비슷한데 엉뚱한" 문서를 걸러 줍니다.

```json
{
  "source_id": "regional-guidance-eu-2026",
  "product": "Product A",
  "region": "EU",
  "approval_status": "approved",
  "effective_date": "2026-05-01",
  "audience": "field_team",
  "sensitivity": "internal"
}
```

<div class="oa-eli">
  <span class="oa-eli__head">⚠️ 메타데이터 ≠ 접근 통제</span>
  <p><code>sensitivity: restricted</code>라는 꼬리표는 <b>그걸 실제로 막아 주는 시스템</b>이 있을 때만 의미가 있어요. 이름표만 붙이고 문을 안 잠그면 소용없죠.</p>
</div>

<div class="oa-bars">
  <div class="oa-bar"><span>필터 너무 넓음</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:85%"></span><span class="oa-bar__val">관련 없음·낡음·권한 밖 문서 혼입</span></span></div>
  <div class="oa-bar"><span>적당함</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:55%"></span><span class="oa-bar__val">승인된 관련 소스만</span></span></div>
  <div class="oa-bar"><span>필터 너무 좁음</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:20%"></span><span class="oa-bar__val">유용한 소스 누락 → 불완전한 답</span></span></div>
  <p class="oa-bars__caption">막대는 검색 범위의 넓이를 개념적으로 표현한 것 (강의 수치 아님).</p>
</div>

여러 종류의 소스(예: 코드 + 데이터 웨어하우스 + 사내 문서)를 함께 쓰면 **소스 종류마다 접근을 따로** 정의하고, 권한 있는 사용자와 없는 사용자 둘 다로 테스트해요. 한 소스에 접근 가능하다고 다른 소스까지 되는 건 아니에요.

## 6. 근거 있는 출력 검증 — 두 단계로 본다

<div class="oa-eli">
  <span class="oa-eli__head">📝 쉽게 말하면</span>
  <p>오픈북 시험 채점에서 <b>① 맞는 페이지를 펼쳤나</b>와 <b>② 그 페이지 내용대로 썼나</b>는 따로 봐야 해요. 맞는 페이지를 펼쳐 놓고 책에 없는 내용을 덧붙여 쓰면 여전히 감점이죠. 출처 표시가 있다고 모든 문장이 근거 있는 건 아니에요.</p>
</div>

| 테스트 범주 | 확인하는 것 |
|---|---|
| Golden questions | 핵심 질문에 기대한 소스·답 동작이 나오나 |
| Expected source references | 맞는 소스·섹션을 가리키나 |
| Missing-source | 근거가 없을 때 지어내지 않나 |
| Conflicting-source | 소스끼리 다를 때 우선순위 규칙을 따르거나 에스컬레이션하나 |
| Outdated-source | 낡은 소스를 제외·표시하나 |
| Unsupported-claim | 검색 결과에 없는 내용을 덧붙이지 않나 |
| Permission-boundary | 권한 있는 내용만 검색하나 |
| Citation quality | 출처가 검토에 쓸 만큼 구체적인가 (ID·섹션·버전·시행일) |
| Metadata / filter | 지역·제품·승인 상태·시행일 필터가 의도대로 동작하나 |
| No-answer / escalation | 증거가 부족하면 거절·확인 질문·에스컬레이션하나 |

### 흔한 실패 모드

| 실패 | 모습 | 대응 |
|---|---|---|
| 근거 없는 덧붙임 | 맞는 소스 + 없는 디테일 추가 | 근거 밖 주장 테스트, source-bounded 출력 |
| 엉뚱한 소스 | 비슷하지만 권위 없는 소스 | 원본 규칙·메타데이터 필터·우선순위 |
| 낡은 소스 | 옛 정책 인용 | 시행일 필터·낡은 소스 테스트 |
| 소스 없는데 자신만만 | 검색 결과 없는데 확신 있는 답 | 확인 질문·거절·사람 검토 |
| 약한 출처 | "정책에 따르면"뿐 | 출처 ID·섹션·버전 요구 |
| 권한 누출 | 볼 수 없는 내용 노출 | 권한 경계 테스트 + 소유자 검증 |
| 충돌 감추기 | 두 소스를 섞은 한 가지 답 | 우선순위 규칙 또는 에스컬레이션 |

<div class="oa-eli">
  <span class="oa-eli__head">🧪 모델이 스스로 매긴 플래그를 믿지 말 것</span>
  <p>응답 JSON에 <code>"needs_review": false</code>, <code>"unsupported_claims_detected": false</code>가 있어도 그건 <b>모델의 판단</b>일 뿐, 검사를 통과했다는 증거가 아니에요. 중요한 주장은 승인된 원문과 직접 대조하고, 접근 제한·필수 검토는 별도로 확인해요.</p>
</div>

### 실제 사례 — 출처가 있어도 위험한 두 답

생명과학 고객의 현장팀 Q&A API 검증 중:

1. 답 A: 맞는 승인 문서를 인용했지만 **문서에 없는 디테일**을 추가함
2. 답 B: 메타데이터 필터가 **지역만 보고 시행일은 안 봐서** 옛 지역 부록을 인용함

둘 다 언뜻 근거가 있어 보이지만 **추천하면 안 되는** 답이에요. → 근거 밖 주장·낡은 소스·출처 품질·메타데이터(지역 + 시행일 + 승인 상태)·권한·소스 없음·충돌 테스트를 추가해야 해요.

### Retrieval은 "설치하고 끝"이 아니다

새 버전 발행 · 옛 버전이 여전히 검색됨 · 메타데이터 누락/불일치 · 권한 변경 · 새 제품·지역 추가 · 테스트 밖 질문 증가 → **검색 품질이 시간이 지나며 흔들려요.** 그래서 **소스 소유자**와 **검색 동작 담당자**, 검토할 증거, 재테스트 트리거를 정해 둡니다.

## 7. Context, Knowledge Access, and Retrieval Design Plan

플랜이 답해야 할 다섯 가지 질문:

<ol class="oa-flow">
  <li><strong>무엇을 알아야?</strong>지식 요구</li>
  <li><strong>어디서 가져와?</strong>소스·소유자</li>
  <li><strong>Retrieval이 정당·안전?</strong>적합성 결정</li>
  <li><strong>무엇을 먼저 검증?</strong>블로커·조건</li>
  <li><strong>어떤 증거로?</strong>검증 테스트·유지보수</li>
</ol>

| 섹션 | 내용 |
|---|---|
| 지식 요구 / 적합성 결정 | 필요·불필요·보류와 이유 |
| 접근 요구 / 승인 소스 / 제외·검증 소스 | 포함·제외·검증 목록과 사유 |
| 소유자·승인 상태 / 최신성·완전성 평가 | 누가 책임지고 얼마나 믿을 만한가 |
| 권한 가정 | 역할·지역·테넌트·서비스 계정 경계 |
| RAG 패턴 / 임베딩 가정 / 메타데이터·필터 | 어떤 패턴, 무엇을 증명 못 하나, 어떤 꼬리표 |
| 품질 리스크 / 검증 테스트 | 낡음·충돌·근거 밖 주장·권한 테스트 |
| 거버넌스·품질 검토 / 열린 질문 / 하위 제약 | 담당자, 트리거, 도구·가드레일·관측성 제약 |

<div class="oa-eli">
  <span class="oa-eli__head">🗂️ 호스팅 파일 검색을 제안한다면 (강의 기준 내용)</span>
  <p>OpenAI 호스팅 파일 검색에서는 업로드 파일이 <b>vector store</b>(검색 가능한 파일 모음)에 색인돼요. 이 방식을 제안하면 승인 소스·메타데이터·검색/인용 점검 계획·정리 담당자, 그리고 <b>vector store와 업로드 파일 각각의 보관 기간과 만료·삭제 방식</b>을 따로 적어요. vector store ID가 있다고 검색이 잘 된다는 증거는 아니에요. 세부 기능·제한은 구현 전에 최신 공식 문서로 확인합니다.</p>
</div>

### 약한 플랜 → 강한 플랜

| 약한 문장 | 왜 약한가 | 더 강한 문장 |
|---|---|---|
| "RAG 쓰자" | 소스·권한·검증·실패 동작 없음 | "승인된 현재 보증 정책 + 검증된 지역 부록으로 검색, 제품·지역·승인 상태·시행일 필터" |
| "지식베이스 연결" | 소유자·품질·제외 없음 | "Support Ops 소유 승인 아티클만, 보관본·초안 제외" |
| "출처를 달게 하자" | 출처 품질·근거 밖 주장 점검 없음 | "출처 ID·섹션·버전·시행일 반환, 근거 밖 주장 테스트" |
| "기존 권한 따름" | 구현 동작을 검증 없이 가정 | "역할·지역·테넌트·서비스 계정 접근을 시스템 소유자와 검증" |

### 액션 아이템은 "막는가?"까지

| 액션 | 담당 | 필요한 증거 | Retrieval을 막나? |
|---|---|---|---|
| 현재 승인 정책 확정 | 정책 소유자 | 승인 버전·시행일 | 예/아니오 |
| 역할 기반 접근 검증 | 보안/시스템 소유자 | 역할·지역·테넌트별 규칙 | 예/아니오 |
| 메타데이터 분류 체계 확정 | 소스/데이터 소유자 | 제품·지역·승인·시행일 필드 | 예/아니오 |
| 소스 없음 동작 재테스트 | 구현/QA | 소스 없음·충돌 케이스 결과 | 예/아니오 |

"초안은 승인 전까지 포함 금지", "고객용 파일럿은 소스 없음·근거 밖 주장 테스트 통과 전까지 금지" 같은 **진행 금지 조건**을 적어 두면 가짜 준비 완료를 막을 수 있어요.

<div class="oa-eli">
  <span class="oa-eli__head">🧬 사례 결론</span>
  <p>생명과학 현장팀 사례의 가장 강한 추천은 "<b>Retrieval은 필요하다. 단, 소스 검증까지 보류</b>"였어요. 핵심 소스 세트는 정당하지만, 지역 부록 검증·권한 확인·메타데이터 필터 정의·검증 테스트 준비 전에는 구현을 시작하지 않는다는 거죠. "제품 문서로 RAG"보다 훨씬 검토 가능한 결론이에요.</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 구매 정책 Q&A API. 소스는 ① 전 직원 승인 현재 정책 ② 미승인 초안 공급사 리스크 부록 ③ 낡은 교육 자료. 지역별 규칙도 있다. 가장 강한 추천은?</summary>
  <p><b>정답:</b> 승인된 현재 정책만 검색하고 <b>지역 필터</b>를 걸며, 낡은 자료는 제외하고 초안은 사용 전 검증한다. (전부 넣고 랭킹에 맡기기 ✗ / 일반 지식만 사용 ✗ / 전부 파인튜닝 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 갱신 브리프 API — CRM 메모(민감), 초안 계정 계획, 소유자 제각각, 계정별 접근 제한. 가장 강한 추천은?</summary>
  <p><b>정답:</b> 권한·민감 데이터 처리·초안 제외·소유권·계정 필터가 <b>검증된 후에만</b> Retrieval을 사용한다. (지금 전부 검색하고 파일럿에서 필터 추가 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 보증 질문 API가 현재 정책을 검색했지만, 그 정책에 없는 예외를 덧붙였다. 출처 표시는 있다. 결론은?</summary>
  <p><b>정답:</b> 예외 조항에 근거가 없으므로 <b>미검증 출력</b>이다. 맞는 소스가 나왔다고 검증된 게 아니고, 한 번 실패했다고 Retrieval을 버릴 일도 아니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. "보증 문서·지원 아티클·보관 자료·과거 상담 메모 전부로 RAG, 출처 달고, 평범한 질문 몇 개 테스트" — 가장 강한 비판은?</summary>
  <p><b>정답:</b> 통제(제외·권한·필터·실패 테스트)와 <b>담당자</b>가 빠져 있어 <b>불완전</b>하다.</p>
</details>

## 정리

- Retrieval은 "문서가 있으니까"가 아니라 **업무의 지식 요구**에서 시작한다. 필요 / 불필요 / 보류 중 하나를 고른다.
- **더 많은 컨텍스트 ≠ 더 좋은 답.** 승인·관련·최신·권한·구체성을 갖춘 컨텍스트가 좋은 컨텍스트.
- 소스는 **포함 / 제외 / 검증 필요**로 나누고, 제외도 눈에 보이게 기록한다.
- 임베딩·벡터 검색은 "뜻이 비슷한" 걸 찾을 뿐, **권위·최신성·권한을 증명하지 않는다.** 메타데이터도 접근 통제를 대신하지 않는다.
- 검증은 **맞는 소스를 찾았나 + 그 안에서만 답했나** 두 단계. 출처 표시가 모든 문장의 근거를 보장하지 않는다.
- 플랜은 진행 / 조건부 진행 / 보류 / 회피 중 하나를 **담당자·증거·블로커**와 함께 제시한다.
