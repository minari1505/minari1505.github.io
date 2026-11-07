---
title: "Deep Research, Images, Distillation, and Specialized API Pattern Fit"
title_ko: "특화 API 패턴 적합성 — Deep Research·이미지·증류·최적화, '고급'보다 '필요'"
course: openai-api-deployment
lesson: 10
---

## 학습 목표

- 특화 API 패턴이 **업무 필요·증거·트레이드오프·검증 부담**으로 정당화되는지 판단하기
- **Deep Research**(Responses API 기반)를 소스 품질·승인된 도구·추적성·장시간 실행·리뷰 담당 관점으로 평가하기
- **이미지 이해(vision)** 와 **이미지 생성·편집**을 따로 떼어 적합성·권리·접근성·리뷰를 평가하기
- 더 단순한 개선을 먼저 해 본 뒤에야 **증류(distillation)·최적화**를 고려하기
- 결과를 **Specialized API Pattern Fit Assessment**로 정리하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>표준 패턴부터</strong>더 단순한 대안이 있나?</li>
  <li><strong>Deep Research</strong>소스·추적성·리뷰</li>
  <li><strong>이미지</strong>이해 vs 생성·편집 분리</li>
  <li><strong>증류·최적화</strong>기준선 증거가 먼저</li>
  <li><strong>Fit Assessment</strong>5가지 추천 중 하나</li>
</ol>

<div class="oa-eli">
  <span class="oa-eli__head">🔧 쉽게 말하면</span>
  <p>못 하나 박는데 <b>전동 드릴</b>부터 꺼내지 않죠. 망치로 되면 망치. 드릴은 강력하지만 전원·안전교육·관리가 따라와요.</p>
  <p>특화 패턴도 같아요. "고객이 <b>고급</b> 솔루션을 원해요"는 이유가 아니에요. <b>표준 패턴으로 부족한 이유</b>를 증거로 댈 수 있을 때만 꺼냅니다. 그리고 꺼내는 순간 <b>검증 부담(validation burden)</b>이 함께 늘어나요.</p>
</div>

## 핵심 용어

| 용어 | 뜻 |
|---|---|
| **표준 API 패턴** | 모델 단독 생성, 구조화 출력, 검색 기반 응답, 도구·액션, 에이전트, 실시간·멀티모달, 그 조합. "표준" ≠ "기초" — 특화 계층 없이 해결된다는 뜻 |
| **특화 API 패턴** | 표준으로 부족할 때 쓰는 좁은 해법: Deep Research, 이미지 이해, 이미지 생성·편집, 증류, 최적화 |
| **검증 부담** | 특화 패턴을 책임 있게 추천하기 전 필요한 추가 증거: 소스 리뷰, eval, 품질 기준, 권리, 안전·접근성, 비용·지연, 유지 담당 |
| **표준 패턴 대안** | 특화 전에 먼저 검토할 더 단순한 선택지: 구조화 출력, 검색 기반 응답, 더 촘촘한 도구 계약, 더 나은 프롬프트, 모델 재선택, 사람 보조 |

## 1. Deep Research — "리서치가 필요해요"만으로는 부족

### 언제 의미가 있나

<div class="oa-eli">
  <span class="oa-eli__head">📚 쉽게 말하면</span>
  <p>"우리 학교 급식 메뉴는?"은 <b>공지 한 장</b>만 보면 돼요 → 검색 기반 응답으로 충분.<br>"우리 동네 학원 5곳의 가격·후기·커리큘럼을 비교해 보고서로" 는 여러 출처를 모으고, 엇갈리는 정보를 비교하고, 근거를 붙여야 해요 → 이럴 때 Deep Research.</p>
</div>

| 적합 신호 | 의미 | 검증할 것 |
|---|---|---|
| 다중 소스 조사 | 공개 정보·내부 문서·분석·데이터를 모두 봐야 함 | 어떤 소스가 승인·최신·권한 있음? |
| 증거 비교 | 주장끼리 비교하고 불일치를 설명해야 함 | 충돌·누락·근거 없는 주장을 어떻게 표시? |
| 최신 정보 | 최근 외부 정보에 의존 | 어떤 공개 소스 허용? 품질 리뷰는? |
| 내부 소스 종합 | 승인된 내부 파일·정책 사용 | vector store가 승인·권한·범위 OK? |
| 장시간 실행 | 초가 아니라 **수십 분** 걸릴 수 있음 | 진행·완료·실패를 사용자가 어떻게 아나? |
| 리뷰용 결과물 | 결정을 돕지만 자동 실행은 안 함 | 누가 리뷰? 언제 에스컬레이션? |

예: 주간 시장 브리핑, 정책 변경 요약, 경쟁사 조사, 기술 문헌 리뷰, 벤더 리스크 조사.

### 소스·도구 선택 (강의 기준)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🌐</span><span class="oa-card__title">Web search</span>최신 공개 정보. 권위·관련성·신선도·인용 가시성 확인</div>
  <div class="oa-card"><span class="oa-card__icon">🗂️</span><span class="oa-card__title">File search</span>승인된 vector store의 내부 문서. 승인·권한·최신성·메타데이터</div>
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">Remote MCP</span>신뢰된 서버로 사내 시스템 접근. 신뢰 경계·공유 데이터·도구 호출 리뷰</div>
  <div class="oa-card"><span class="oa-card__icon">🧮</span><span class="oa-card__title">Code interpreter</span>계산·표·추세 분석. 입력 승인, 재현성, 사람 검증</div>
</div>

강의 기준으로는 Responses API에 **승인된 데이터 소스 최소 1개**(web search / file search / 승인된 원격 MCP)를 붙이고, 분석이 필요하면 code interpreter를 더해요. 지원 도구·설정은 모델마다 다르므로 **출시·추천 직전 최신 문서 확인**이 필수예요.

<div class="oa-eli">
  <span class="oa-eli__head">⏱️ 오래 걸리는 작업의 함정</span>
  <p>수십 분짜리 작업이라 <b>background mode</b>, 완료 감지(polling 또는 webhook), 사용자 알림, timeout, 도구 호출 예산을 정해야 해요. 강의 기준으로 ZDR(Zero Data Retention) 프로젝트의 background 요청은 <code>store=false</code>여도 비동기 처리를 위해 <b>약 10분간 디스크에 임시 저장</b>돼요. 고객 데이터 정책이 이것조차 금지하면 background mode는 끄세요.</p>
</div>

### 진짜 위험은 "그럴듯한 확신"

Deep Research 결과물은 **매끈하고 권위 있어 보여서** 위험해요. 근거가 약해도 최종 답처럼 읽히거든요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 위험한 동작</span>
    <ul>
      <li>두 출처가 마감일을 다르게 말하는데 하나만 골라 사실처럼 제시</li>
      <li>승인된 사내 정책이 있는데 웹 글을 근거로 사용</li>
      <li>권한 없는 사용자에게 모든 소스 노출</li>
      <li>항상 완성된 답을 냄</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 안전한 실패(safe failure)</span>
    <ul>
      <li>"출처가 부족해 이 결론을 지지할 수 없습니다."</li>
      <li>"이 부분은 출처끼리 충돌합니다. 사람 리뷰가 필요합니다."</li>
      <li>"이 주장은 근거가 필요합니다."</li>
      <li>"이건 분석가 리뷰용 초안입니다."</li>
    </ul>
  </div>
</div>

검증은 세 질문으로: **① 소스가 믿을 만하고 허용된 것인가? ② 종합이 정확하고 추적 가능한가? ③ 리뷰 과정이 이 업무에 충분히 안전한가?**

### 통제 흐름 (강의 의사코드를 요약)

<ol class="oa-flow">
  <li><strong>브리프 준비</strong>빠진 조건 있으면 → needs_clarification</li>
  <li><strong>소스·도구 계획 검사</strong>승인 소스 0개·MCP 미확인 → pause_or_escalate</li>
  <li><strong>background 실행</strong>polling/webhook, 도구 호출 예산</li>
  <li><strong>증거·충돌 점검</strong>근거·도구 trace 첨부</li>
  <li><strong>needs_review로 반환</strong>최종 결과로 취급 ✗</li>
</ol>

> 예시 속 역할·권한 값은 설명용일 뿐 **인증된 접근 검사가 아니에요.** 실제 구현은 호출 **전에** 소스 권한과 도구 선택을 강제해야 해요.

**약한 추천**: "리서치가 필요하니 Deep Research." → **강한 추천**: "주간 리스크 브리핑에는 <b>리뷰 지원용</b>으로 그럴듯함. 승인된 공개 소스 + 승인된 내부 vector store + 근거 참조 + (데이터 정책과 맞을 때만) background + 도구 호출 제한 + 분석가 리뷰로 **제한적 프로토타입**. 최종 리스크 결정은 자동화하지 않음."

## 2. 이미지 — "이해"와 "생성·편집"은 다른 결정

<div class="oa-eli">
  <span class="oa-eli__head">📷 쉽게 말하면</span>
  <p>사진을 <b>보고 판단</b>하는 일(이 상품 사진 흐린가?)과 사진을 <b>새로 그리는</b> 일(상품 홍보 이미지 만들기)은 완전히 달라요. 앞은 "눈", 뒤는 "붓"이에요. 쇼핑몰 업무에 둘 다 있어도 <b>따로따로</b> 평가해요.</p>
</div>

| 시각 워크플로우 | 주로 쓰는 표면 (강의 기준) | 물어볼 질문 |
|---|---|---|
| **이미지 이해(vision)** | Responses API + 이미지 입력 | 이미지에만 있는 정보가 필요한가? |
| **이미지 생성** | Images API(직접 생성) / Responses API 이미지 생성 도구(대화형·다단계) | 정말 시각 결과물이 필요한가? 텍스트·사람 제작으로 충분하지 않나? |
| **이미지 편집·변환** | 위와 동일 | 입력 권리·편집 의도·허용 용도·리뷰 기준이 명확한가? |
| 시각 초안 | 생성·편집 | 초안으로 표시되고 재사용 전 사람 리뷰? |
| 시각 QA | 이해 | 통과/실패/리뷰 필요/재촬영 기준은? |
| 다이어그램·콘셉트 | 생성·편집 | 최종 설계가 아닌 정렬용 초안임이 분명? |
| **결합형** | 이해 단계와 생성 단계를 분리 | 두 단계 사이 인계·리뷰·승인 통제는? |

### 이미지 워크플로우 점검 8가지

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__title">🎨 브랜드·제품 정확성</span>스타일·각도·배경 규칙을 리뷰어가 확인 가능?</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__title">©️ 권리·허용 용도</span>입력 이미지·참조 자산·생성물을 이 용도로 써도 되나?</div>
  <div class="oa-card"><span class="oa-card__title">🙋 민감 콘텐츠·동의</span>사람·신분증·위치·개인정보 포함?</div>
  <div class="oa-card"><span class="oa-card__title">🗄️ 미디어 보관</span>어디에, 누가, 얼마나?</div>
  <div class="oa-card"><span class="oa-card__title">♿ 접근성</span>alt text, 읽히는 라벨, 비시각 대체 경로</div>
  <div class="oa-card"><span class="oa-card__title">🌍 고객 노출 여부</span>내부 초안 vs 외부 게시 — 승인 필요?</div>
  <div class="oa-card"><span class="oa-card__title">🚫 출력 한계</span>무엇을 주장·추론·생성·결정하면 안 되나</div>
  <div class="oa-card"><span class="oa-card__title">🧑‍⚖️ 리뷰·에스컬레이션</span>승인·수정·거절 담당자</div>
</div>

"보기 좋네요"는 검증이 아니에요. 예: 상품 콘텐츠는 **브랜드 적합·제품 정확·승인 배경·필수 각도·오해 소지 문구 없음·실제 크기에서 가독성**처럼 구체적인 기준을 써요.

### 배포의 핵심은 프롬프트가 아니라 "포장지(wrapper)"

<ol class="oa-flow">
  <li><strong>사용자 요청</strong>초안·업로드·리뷰 요청</li>
  <li><strong>정책·권한 검사</strong>역할·용도·권리·외부 노출</li>
  <li><strong>입력 준비</strong>통제된 프롬프트/승인된 이미지</li>
  <li><strong>이미지 API 호출</strong>검증된 용도만</li>
  <li><strong>출력 처리</strong>저장·반환·폐기 정책</li>
  <li><strong>리뷰·승인</strong>draft / approved / rejected</li>
  <li><strong>로깅</strong>용도·상태·리뷰 결과·차단 요청</li>
</ol>

생성 **전**엔 요청이 허용되는지, 생성 **후**엔 저장·공유·게시 가능한지를 애플리케이션이 강제해요. 강의 예시는 승인되지 않은 워크플로우·허용되지 않은 역할·권리 미확인·외부 게시 시도를 각각 `blocked`로 막고, 통과해도 `draft_needs_review` + "외부 게시 금지·최종 이미지 아님" 한계를 붙여 돌려줘요. 단, `user_role`·`rights_confirmed` 같은 값은 **검증된 앱 상태**여야 하고 사용자나 모델이 준 값을 믿으면 안 돼요.

**약한 추천**: "이커머스는 시각적이니 이미지." → **강한 추천**: "내부 시각 초안과 시각 QA 지원에만 사용, 외부 사용 전엔 사람 리뷰. 생성 상품 이미지 자동 게시 금지. 권리·브랜드 기준·접근성·미디어 처리 규칙 확인 후 확장."

## 3. 증류·최적화 — 먼저 더 쉬운 것부터

<div class="oa-eli">
  <span class="oa-eli__head">🏃 쉽게 말하면</span>
  <p>달리기 기록이 안 나올 때 비싼 신발부터 사는 대신, <b>자세·호흡·운동화 끈</b>부터 고쳐 보죠. 그래도 안 되면 그때 장비를 바꿔요.</p>
  <p>품질·비용·속도 문제도 대부분 <b>설계 문제</b>예요: 프롬프트가 모호하거나, 출력 계약이 느슨하거나, 검색이 엉뚱한 걸 가져오거나, eval이 부족하거나.</p>
</div>

### 사다리: 아래부터 올라가요

<div class="oa-bars">
  <div class="oa-bar"><span>① 프롬프트 개선</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:12%"></span><span class="oa-bar__val">복잡도 낮음</span></span></div>
  <div class="oa-bar"><span>② 출력 계약·구조화 출력</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:22%"></span></span></div>
  <div class="oa-bar"><span>③ 검색·도구 경계 개선</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:35%"></span></span></div>
  <div class="oa-bar"><span>④ eval 확충·데이터 품질</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:45%"></span></span></div>
  <div class="oa-bar"><span>⑤ 모델·역량 재선택</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:58%"></span></span></div>
  <div class="oa-bar"><span>⑥ 최적화 (설계 전반 조정)</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:75%"></span></span></div>
  <div class="oa-bar"><span>⑦ 증류 (teacher → student)</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:92%"></span><span class="oa-bar__val">복잡도·책임 높음</span></span></div>
  <p class="oa-bars__caption">막대 길이는 운영 부담을 비교하기 위한 개념적 표현이에요 (강의 수치 아님).</p>
</div>

> 결정 규칙: **현재 패턴을 정의·테스트하고 더 단순한 통제로 개선하기 전에는 증류·최적화하지 않는다.**

### 고정 라벨 작업이라면 — Decisions vs 생성 (강의 기준)

강의는 지원 티켓 카테고리처럼 **정해진 라벨**이 필요한 작업에 대해 두 설계를 비교하라고 해요.

| 설계 | 방식 | 주의 |
|---|---|---|
| **Decisions** (public beta, GPT-6 Luna 기준) | 질문·선택지를 주면 확률 / 선택지 중 하나 / 루브릭 점수를 **타입 있는 답**으로 반환. 별도 confidence 포함, 설명문은 없음 | 라우팅 노트는 **별도 단계**로 작성 → 노트 품질은 따로 평가 |
| **생성 1회 + Structured Outputs** | 카테고리와 노트를 스키마로 한 번에 | 스키마가 맞아도 **틀린 카테고리·쓸모없는 노트**일 수 있음 |

두 경우 모두 카테고리 정의를 명확히 하고, **카테고리 정확도와 노트 유용성을 따로** 평가해야 해요. 신뢰도 임계값은 임의로 정하지 말고 **라벨된 예시와 잘못 라우팅됐을 때의 결과**로 정해요.

<div class="oa-eli">
  <span class="oa-eli__head">⚠️ 파인튜닝 경로 (강의 기준, 수시로 바뀌니 재확인)</span>
  <p>강의 시점 안내: OpenAI가 셀프서비스 파인튜닝을 축소 중 — 이전 사용 이력 없는 조직은 신규 학습 불가, 2026-07-02부터 기존 조직도 최근 60일 내 파인튜닝 모델 추론 이력이 있어야 job 생성, 활성 기존 고객의 신규 job 생성은 2027-01-06 종료 예정. 즉 <b>증류 제안에 학습 경로가 있다고 가정하지 말 것.</b> 고객과 논의 전 자격·모델 수명주기·최신 공식 안내를 확인해요.</p>
</div>

### 증류: "잘 되는 것"을 더 싸고 빠르게

<div class="oa-svg">
<svg viewBox="0 0 720 200" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="증류 흐름: 검증된 teacher 출력을 모아 걸러 student 후보를 같은 held-out 세트로 비교">
  <rect x="10" y="60" width="150" height="70" rx="12" fill="#fff3c4" stroke="#e9c75c"/>
  <text x="85" y="88" text-anchor="middle" font-size="14" font-weight="700" fill="#5a4500">Teacher</text>
  <text x="85" y="108" text-anchor="middle" font-size="11" fill="#6b6b6b">이미 검증된 강한 패턴</text>
  <rect x="195" y="60" width="150" height="70" rx="12" fill="#fffdf5" stroke="#f1e4b8"/>
  <text x="270" y="88" text-anchor="middle" font-size="13" font-weight="700" fill="#4a4a4a">리뷰된 예시</text>
  <text x="270" y="108" text-anchor="middle" font-size="11" fill="#6b6b6b">약한·미승인 예시 제거</text>
  <rect x="380" y="60" width="150" height="70" rx="12" fill="#fffdf5" stroke="#f1e4b8"/>
  <text x="455" y="88" text-anchor="middle" font-size="13" font-weight="700" fill="#4a4a4a">Student 후보</text>
  <text x="455" y="108" text-anchor="middle" font-size="11" fill="#6b6b6b">더 작고·빠르고·싼</text>
  <rect x="565" y="40" width="145" height="110" rx="12" fill="#f4fbf3" stroke="#cfe8c9"/>
  <text x="637" y="70" text-anchor="middle" font-size="13" font-weight="700" fill="#2f6b2a">같은 held-out</text>
  <text x="637" y="88" text-anchor="middle" font-size="13" font-weight="700" fill="#2f6b2a">세트로 비교</text>
  <text x="637" y="112" text-anchor="middle" font-size="11" fill="#4a4a4a">품질 기준 미달 → 거절</text>
  <text x="637" y="130" text-anchor="middle" font-size="11" fill="#4a4a4a">비용·지연 실측 포함</text>
  <g stroke="#c99400" stroke-width="2" fill="none"><path d="M160 95 L190 95"/><path d="M345 95 L375 95"/><path d="M530 95 L560 95"/></g>
  <g fill="#c99400"><path d="M190 90 L198 95 L190 100Z"/><path d="M375 90 L383 95 L375 100Z"/><path d="M560 90 L568 95 L560 100Z"/></g>
  <text x="360" y="180" text-anchor="middle" font-size="12" fill="#8a8a8a">학습용 예시와 평가용 held-out 세트는 반드시 분리</text>
</svg>
</div>

| 증류 전에 말할 수 있어야 하는 것 | 예 |
|---|---|
| 지켜야 할 동작 | 검증된 라우팅 정확도 |
| 개선할 제약 | 예상 물량에서 너무 느리거나 비쌈 |
| 성공 측정법 | 원본 vs 증류본 비교 eval + 품질 기준 |
| 허용 가능한 품질 손실 | 어디까지면 효율 이득이 의미 없는가 |
| 유지 담당 | 프롬프트·데이터·모델 변경 시 재테스트할 팀 |

> 원래 패턴이 불안정한데 증류하면 **"신뢰할 수 없는 시스템의 더 싸고 빠른 버전"**이 될 뿐이에요.

### 최적화: "무엇을, 어떻게 증명할지" 먼저

| 문제 | 먼저 고려 | 증류·심화 최적화는 이럴 때만 |
|---|---|---|
| 출력이 들쭉날쭉 | 프롬프트·출력 계약·스키마·예시·fallback | 목표 동작 명확 + eval 존재 + 단순 통제로 부족 |
| 검색이 약함 | 소스 품질·메타데이터·청킹·필터·grounding 지시 | 검색 패턴은 검증됐는데 비용·지연·규모가 막힘 |
| 도구 사용 불안정 | 도구 경계·호출 조건·스키마·에러 처리·승인 | 도구 동작은 안정, 성능만 필요 |
| 지연이 큼 | 모델 재선택, 불필요한 단계·컨텍스트 축소 | 품질 OK + 측정 가능한 지연 목표 |
| 비용이 큼 | 컨텍스트 크기, 모델, 도구 호출, 중복 작업, 빈도 | 현재 동작이 되고 더 싼 경로 필요 |

"더 빠르게"는 목표가 아니에요. **"라우팅 정확도와 구조화 출력 유효성을 유지하면서 평균 지연을 줄인다"**가 목표예요. 이를 위해 request ID·릴리스 버전·모델·지연·사용량·에러 유형·품질 상태·리뷰/fallback 여부를 **기준선부터 로깅**해요. 로그의 "success"는 호출이 끝났다는 뜻일 뿐, 답이 맞았다는 뜻이 아니에요.

<div class="oa-eli">
  <span class="oa-eli__head">🚀 모델 바꾸기 vs 처리 모드 바꾸기 (강의 기준)</span>
  <p>모델 비교는 "어떤 모델이 품질·비용·지연을 만족하나", 처리 모드 실험은 "<b>같은 모델</b>로 더 빨리 받을 가치가 있나"예요. 예: GPT-6.1 Sol은 모델 후보, Ultrafast는 (해당 모델·GPT-6 Astra의) 더 비싼 고속 처리 옵션. 모드 효과만 보려면 모델·프롬프트·추론 설정·테스트·합격 기준을 고정하고, <b>합격한 완성 결과 1건까지</b>의 총비용·시간(재시도·리뷰 포함)으로 비교해요.</p>
</div>

### 트레이드오프와 담당자

비용을 줄이면 유지보수가 늘 수 있고, 지연을 줄이면 품질이 떨어질 수 있어요. 품질·비용·지연·처리량·신뢰성·유지보수성·거버넌스·버전 관리·eval 부담·운영 담당을 모두 따져요. **담당자 없는 최적화 패턴은 배포 준비가 안 된 것**이에요 — 품질·성능 모니터링, 변경 시 재테스트, 버전·롤백, drift 감시, 업데이트·에스컬레이션 결정을 누군가 맡아야 해요.

## 4. Specialized API Pattern Fit Assessment

| 섹션 | 답할 질문 |
|---|---|
| 업무 필요 + 표준 대안 | 무엇이 필요하고, 왜 표준 패턴으로 충분/부족한가 |
| 후보 특화 패턴 | Deep Research? 이미지 이해? 생성? 증류? 최적화? |
| 패턴별 적합 증거 | 소스 필요, 시각 필요, 반복 품질 갭, 비용·지연·규모 압박 |
| 인접 의존성 | 검색·도구·에이전트·구조화 출력·background·리뷰 |
| 데이터·품질·권리·안전·거버넌스 | 누가 쓸 권리가 있고, 어떤 기준과 승인이 필요한가 |
| 트레이드오프·운영 복잡도 | 장점만 나열 금지 |
| 검증·에스컬레이션 계획 | 무엇을, 누가, 어떤 증거로 |
| 위험·제외·추천 | 쓰면 안 되는 곳 + 명시적 추천 |

### 다섯 가지 추천 범주

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__title">① 표준 패턴으로 충분</span>더 낮은 복잡도·위험으로 충분한 품질</div>
  <div class="oa-card"><span class="oa-card__title">② 검증 조건부 특화</span>예: 내부 시각 초안은 OK, 외부 사용은 권리·브랜드·접근성·승인 후</div>
  <div class="oa-card"><span class="oa-card__title">③ 증거 확보까지 보류</span>예: 승인된 예시·eval 결과 부족</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__title">④ 중지·에스컬레이션</span>규제 결정에 미승인 소스, 보관 규칙 없는 민감 미디어</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__title">⑤ 특화 패턴 거절</span>실제 필요를 풀지 못하거나 불필요한 위험만 추가</div>
</div>

"아니오/아직"으로 끝내지 말고 **어떤 증거가 나오면 결정이 바뀌는지**도 적어요.

### 사례: "고급 API 솔루션" 하나로 다 해 달라는 제조사

<div class="oa-svg">
<svg viewBox="0 0 720 230" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="하나의 요청을 네 부분으로 나눠 각각 다른 추천을 내리는 그림">
  <rect x="250" y="8" width="220" height="44" rx="10" fill="#fff3c4" stroke="#e9c75c"/>
  <text x="360" y="35" text-anchor="middle" font-size="14" font-weight="700" fill="#5a4500">"고급 API 솔루션" 요청</text>
  <g stroke="#c99400" stroke-width="1.6" fill="none"><path d="M360 52 L90 92"/><path d="M360 52 L270 92"/><path d="M360 52 L450 92"/><path d="M360 52 L630 92"/></g>
  <g font-size="12">
    <rect x="10" y="92" width="160" height="128" rx="10" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="90" y="114" text-anchor="middle" font-weight="700" fill="#4a4a4a">시장 리서치 종합</text>
    <text x="90" y="138" text-anchor="middle" fill="#6b6b6b">Deep Research 후보</text>
    <text x="90" y="170" text-anchor="middle" font-weight="700" fill="#b0563a">③ 보류·제한</text>
    <text x="90" y="190" text-anchor="middle" fill="#6b6b6b">승인 소스·리뷰 담당</text>
    <text x="90" y="206" text-anchor="middle" fill="#6b6b6b">정해질 때까지</text>
    <rect x="190" y="92" width="160" height="128" rx="10" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="270" y="114" text-anchor="middle" font-weight="700" fill="#4a4a4a">시각 콘셉트 초안</text>
    <text x="270" y="138" text-anchor="middle" fill="#6b6b6b">이미지 생성</text>
    <text x="270" y="170" text-anchor="middle" font-weight="700" fill="#2f6b2a">② 조건부</text>
    <text x="270" y="190" text-anchor="middle" fill="#6b6b6b">내부 초안만,</text>
    <text x="270" y="206" text-anchor="middle" fill="#6b6b6b">브랜드·권리 리뷰</text>
    <rect x="370" y="92" width="160" height="128" rx="10" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="450" y="114" text-anchor="middle" font-weight="700" fill="#4a4a4a">지역별 요약</text>
    <text x="450" y="138" text-anchor="middle" fill="#6b6b6b">구조화 출력 + 검색</text>
    <text x="450" y="170" text-anchor="middle" font-weight="700" fill="#2f6b2a">① 표준으로 충분</text>
    <text x="450" y="190" text-anchor="middle" fill="#6b6b6b">승인된 내부</text>
    <text x="450" y="206" text-anchor="middle" fill="#6b6b6b">출시 자료 활용</text>
    <rect x="550" y="92" width="160" height="128" rx="10" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="630" y="114" text-anchor="middle" font-weight="700" fill="#4a4a4a">최적화</text>
    <text x="630" y="138" text-anchor="middle" fill="#6b6b6b">기준선 없음</text>
    <text x="630" y="170" text-anchor="middle" font-weight="700" fill="#b0563a">③ 아직 안 함</text>
    <text x="630" y="190" text-anchor="middle" fill="#6b6b6b">품질·비용·지연</text>
    <text x="630" y="206" text-anchor="middle" fill="#6b6b6b">기준선부터</text>
  </g>
</svg>
</div>

이 사례엔 고정 라벨 분류 필요가 없으니 **Decisions도 쓰지 않는다**고 이유와 함께 기록해요. "쓸 수 있다"는 것이 "써야 한다"는 뜻은 아니에요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 규제 업데이트 + 내부 정책으로 주간 임원 브리핑을 만드는 리서치형 워크플로우. 승인 소스·인용 기준·충돌 처리·리뷰 담당이 미정이라면?</summary>
  <p><b>정답:</b> 소스·추적성·충돌 처리·리뷰 담당이 정해질 때까지 <b>보류</b>. (그냥 진행 ✗ / 외부 소스만 쓰기 ✗ / 시각 요약으로 대체 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짧은 설명에서 고객용 상품 이미지를 자동 생성하고 싶다. 브랜드 기준·권리·접근성·게시 전 승인이 미정이라면?</summary>
  <p><b>정답:</b> 승인된 입력으로 <b>내부 초안만</b> 만들고, 권리·브랜드·접근성·승인 요건이 해결될 때까지 <b>게시는 차단</b>. (alt text 제외 ✗ / 사람 리뷰 제거 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 카테고리 몇 개가 자주 헷갈리고 물량이 늘었다. 정의 중복·eval 없음·confidence/fallback 없음·기준선 미측정. 가장 강한 다음 단계는?</summary>
  <p><b>정답:</b> 카테고리 정리 → 출력 계약 강화 → eval 추가 → 비용·지연 기준선 측정 → 성능 목표 정의, 그다음에 최적화 재검토. (바로 증류 ✗ — 작은 모델이 품질을 자동으로 고치지 않아요)</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 리서치 + 시각 콘셉트 + 지역 요약을 한 "고급 솔루션"으로 원하는 제조사. 가장 방어 가능한 추천은?</summary>
  <p><b>정답:</b> 요청을 <b>업무별로 쪼개고</b> 각각에 가장 단순한 안전한 패턴 적용. (전부 Deep Research ✗ / 하나로 만들고 통제는 나중에 ✗ / 전체 거절 ✗)</p>
</details>

## 정리

- 특화 패턴은 **"고급"이 아니라 "표준으로 부족하다는 증거"**로 정당화한다. 꺼내는 순간 검증 부담이 늘어난다.
- Deep Research는 **불확실성을 숨기지 않고 드러내는** 리뷰 지원 도구. 승인 소스·권한·근거·background·리뷰 담당이 먼저.
- 이미지는 **이해(눈)와 생성·편집(붓)을 분리**하고, 권리·브랜드·접근성·외부 게시 승인은 애플리케이션 wrapper가 강제한다.
- 증류·최적화는 **사다리 맨 위**. 기준선·eval·held-out 비교·담당자 없이 하지 않는다. 파인튜닝 경로가 있다고 가정하지 않는다(강의 기준).
- Fit Assessment는 5가지 추천 중 하나를 명시하고, **결정을 바꿀 증거**까지 적는다.
