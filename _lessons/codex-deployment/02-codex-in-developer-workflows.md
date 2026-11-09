---
title: "Codex in Developer Workflows"
title_ko: "개발 워크플로우 속 Codex — 협업 패턴을 일에 맞추기"
course: codex-deployment
lesson: 2
---

## 학습 목표

- 기획·구현·리뷰·테스트·디버깅·문서·장시간 작업·context 연속성까지 **SDLC 전체에 Codex를 매핑**하기
- 각 워크플로우마다 **Codex가 돕는 것 / 엔지니어가 소유하는 것 / 필요한 통제**를 나누기
- 자동완성 · 페어 프로그래밍 · 에이전트 위임 · 리뷰 체크포인트 · 더 강한 수동 워크플로우를 **구분**하기
- 로컬 리뷰 · 앱 내 PR 리뷰 · 저장소 트리거 cloud 리뷰를 구분하고, **검사·게시·승인·merge가 별개 결정**임을 설명하기
- 목표·context·검증·멈춤 조건이 있는 **위임 브리프(delegation brief)**로 작업을 구조화하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>마찰 ↔ 협업 패턴</strong>3 렌즈 + 6가지 실</li>
  <li><strong>워크플로우 분석</strong>5가지 협업 모델, 위임 준비도</li>
  <li><strong>작업 구조화</strong>위임 브리프, context 연속성</li>
  <li><strong>SDLC 전체 적용</strong>지원 · 소유 · 통제 지도</li>
  <li><strong>통제된 AI-native 워크플로우</strong>가시성 · 추천서</li>
</ol>

이 코스의 **황금 원칙**:

> **협업 패턴을 워크플로우에 맞춰라 — 그 반대가 아니라.**

## 예제 상황 — Rocco Labs와 Cavendish Cables

가상의 파트너 **Rocco Labs**가 고객 **Cavendish Cables**를 도와요. 릴리스 지연, 리뷰 병목, 흩어진 context, 들쭉날쭉한 문서, 늘어나는 PR 대기열, 반복되는 디버깅, 팀 간 조율 부담이 문제예요. 리더들은 Codex에 기대하지만 **리뷰 안 된 코드, 불분명한 소유권, 약해진 품질 통제, 관리 안 되는 장시간 작업**을 걱정해요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 추천</span>
    <p>"개발자가 더 빨리 일하도록 Codex를 쓰세요."</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 추천</span>
    <p>"<b>대화형 디버깅 지원</b>과 <b>통제된 PR 리뷰 준비</b>에 Codex를 쓰되, 리뷰 마찰이 반복되고 context가 있고 검증 증거를 돌려받을 수 있고 사람 리뷰어가 책임지는 워크플로우 하나에서 시작한다."</p>
  </div>
</div>

### 세 가지 렌즈 (개발 워크플로우 버전)

| 렌즈 | 개발 워크플로우에서 묻는 것 |
|---|---|
| 📚 Context | 어떤 파일·티켓·로그·테스트 결과·리뷰 기대치·코딩 규칙이 필요한가? 무엇을 범위 밖에 둘까? |
| ⚙️ Agentic runtime | 어디서 일하고 엔지니어가 얼마나 가까이서 조종하나? 리뷰라면 어떤 리뷰 경로인가? |
| 🔭 Observability | Codex가 어떤 증거를 돌려주나? 리뷰어는 무엇을 보나? 무엇이 아직 불확실한가? |

### 6가지 워크플로우 실

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🐢</span><span class="oa-card__title">워크플로우 마찰</span>어디서 시간·연속성·운영 context를 잃나?</div>
  <div class="oa-card"><span class="oa-card__icon">🤝</span><span class="oa-card__title">협업 패턴</span>대화형 지원? 리뷰용 증거 준비? 제한된 위임?</div>
  <div class="oa-card"><span class="oa-card__icon">👤</span><span class="oa-card__title">엔지니어 소유</span>어떤 결정·승인·책임이 사람 몫인가?</div>
  <div class="oa-card"><span class="oa-card__icon">🧱</span><span class="oa-card__title">작업 구조</span>요청이 협업할 만큼 명확한가?</div>
  <div class="oa-card"><span class="oa-card__icon">🗂️</span><span class="oa-card__title">Context 품질</span>필요한 정보는 있고 위험한 범위는 없나?</div>
  <div class="oa-card"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">리뷰·통제</span>증거, 검사자, 승인 대상, 에스컬레이션 조건은?</div>
</div>

## 1단계 — Codex를 넣기 전에 워크플로우부터 분석

질문을 "**AI가 어디서 코드를 생성할 수 있나?**"에서 "**개발자가 어디서 반복적으로 시간·연속성·운영 context를 잃나?**"로 바꿔요. 병목은 코드를 쓰는 순간 밖(기획, 리뷰, 테스트, 핸드오프, 릴리스 준비…)에 더 자주 있어요.

<div class="oa-eli">
  <span class="oa-eli__head">🍳 쉽게 말하면 — 자동완성에서 위임까지</span>
  <p><b>자동완성</b>은 요리할 때 옆에서 "소금 여기 있어" 하고 건네주는 사람. <b>페어 프로그래밍</b>은 같이 서서 "이 소스 왜 탔지?" 하며 한 단계씩 이야기하는 요리 친구. <b>에이전트 위임</b>은 "이 레시피로 육수 내서 맛 기록이랑 같이 가져와" 하고 맡기는 거예요. 맡기는 범위가 클수록 <b>레시피(목표)·재료 목록(context)·맛 기준(검증)·언제 멈출지</b>를 더 분명히 정해야 해요.</p>
</div>

### 5가지 협업 모델

| 모델 | 하는 일 | 잘 맞는 곳 | 주의 |
|---|---|---|---|
| 자동완성 | 작은 코드 완성 제안 | 빠른 로컬 수정, 반복 문법 | 넓은 워크플로우 병목은 못 풂 |
| 페어 프로그래밍 | 개발자가 한 턴씩 조종하는 대화형 문제 해결 | 탐색·디버깅·구현 가이드·작은 리팩터 | 범위·context가 흐리면 엉망이 됨 |
| 에이전트 위임 | 정해진 목표까지 제한된 작업을 하고 증거 반환 | 반복·검토·검증 가능한 여러 단계 작업 | 명확한 목표·제약·검증·리뷰 필수 |
| 리뷰 체크포인트 | Codex가 증거를 준비, 결정은 사람 | PR 요약, 검증 증거, 문서 초안, 조사 노트 | 리뷰어가 반드시 직접 검사 |
| 더 강한 수동 워크플로우 | 명확·안전해질 때까지 사람 주도 | 고위험 판단, 프로덕션 승인, 모호한 요구, 소유권 미정 | 개선 가능한데 **영구 차단**이 되면 안 됨 |

<div class="oa-bars">
  <div class="oa-bar"><span>자동완성</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:15%"></span><span class="oa-bar__val">구조 거의 불필요</span></span></div>
  <div class="oa-bar"><span>페어 프로그래밍</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:45%"></span><span class="oa-bar__val">대화할 만큼의 context</span></span></div>
  <div class="oa-bar"><span>에이전트 위임</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:90%"></span><span class="oa-bar__val">목표·경계·검증·리뷰 모두</span></span></div>
  <p class="oa-bars__caption">필요한 작업 구조의 양 — 막대 길이는 개념적 표현이에요 (강의 수치 아님).</p>
</div>

### 마찰 → Codex 지원 연결

| 워크플로우 문제 | 가능한 Codex 지원 |
|---|---|
| 리뷰어가 구현 context가 없어 PR이 열린 채 대기 | **리뷰 준비**: 의도·변경 파일·검증 증거·리뷰어 질문 요약 |
| 비슷한 CI 실패를 반복 조사 | **1차 조사**: 로그 요약, 가능한 원인, 다음 단계 제안 |
| 구현 후 문서가 들쭉날쭉 업데이트 | **문서 초안**: 승인된 변경 context로 초안 → 사람 리뷰 |
| Codex가 프로덕션 배포 승인을 하길 원함 | ⛔ 초기 위임 후보 아님. 승인은 사람 몫 |
| 티켓에 "완료" 기준이 없음 | 🔧 **워크플로우부터 개선** |

### 위임 준비도 체크리스트 (코스의 정본)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">Clear</span>목표를 쉽게 말할 수 있음</div>
  <div class="oa-card"><span class="oa-card__icon">📦</span><span class="oa-card__title">Bounded</span>범위가 정해져 있음</div>
  <div class="oa-card"><span class="oa-card__icon">🗂️</span><span class="oa-card__title">Context-supported</span>필요한 파일·로그·티켓·문서가 있음</div>
  <div class="oa-card"><span class="oa-card__icon">✅</span><span class="oa-card__title">Verifiable</span>결과를 확인할 수 있음</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">👀</span><span class="oa-card__title">Reviewable</span>사람이 수용·수정·거절할 수 있음</div>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">약한 위임</span>
    <p>"빌드 고쳐줘."</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">강한 위임</span>
    <p>"제공된 실패 출력으로 반복 CI 실패를 조사해. 가능한 원인, 살펴본 파일, 가장 작은 안전한 다음 단계를 알려줘. 리뷰어가 계획을 승인하기 전엔 파일을 수정하지 마."</p>
  </div>
</div>

> **제한된 작업은 위임하고, 판단·승인·책임은 사람이 가진다.** 아키텍처, 우선순위, 프로덕션 승인, 위험, 품질, 최종 수용은 사람의 결정이에요.

## 2단계 — Codex 협업을 구조화하기

"플래키 테스트 고치고 관련된 거 정리해줘" — 그럴듯하지만 어떤 테스트? 어떤 파일? "정리"란? 수정해도 되나 조사만 하나? 성공 기준은? 피해야 할 것은? 언제 멈추나? 이게 비어 있으면 Codex는 엉뚱한 곳에 힘을 쓰거나, 범위를 넓히거나, 검사하기 어려운 결과를 내요.

### 강한 위임 작업의 5요소

| 요소 | 약한 예 | 강한 예 |
|---|---|---|
| 명확한 목표 | "이슈 고쳐" | "할인 코드가 비었을 때 checkout 테스트가 왜 실패하는지 찾고, 가장 작은 안전한 수정을 제안해. 수정 전에 원인부터 보고해." |
| 제한된 범위 | "코드 정리해" | "checkout 검증 흐름과 관련 테스트만. 무관한 파일 리팩터 금지." |
| 기대 산출물 | (없음) | 계획, 요약, diff, 테스트 제안, 문서 초안, 열린 질문, 검증 요약 |
| 검증 기대치 | (없음) | 특정 단위 테스트 실행, diff·변경 파일 반환, 빠진 커버리지, 확인 못 한 것 표시 |
| 의존성 인식 | (없음) | 기존 패턴 따르기, public API 변경 금지, 고객 영향 동작·보안 민감 작업은 멈추고 에스컬레이션 |

### 위임 브리프 (Delegation brief)

<div class="oa-eli">
  <span class="oa-eli__head">📝 쉽게 말하면</span>
  <p>친구에게 심부름을 맡길 때 "<b>뭘 사 와(목표)</b>, <b>이 가게 전단지 봐(context)</b>, <b>과자 코너만(범위)</b>, <b>영수증 챙겨(검증)</b>, <b>만 원 넘으면 전화해(멈춤 조건)</b>"라고 하면 실수가 확 줄죠. 위임 브리프가 바로 그 메모예요.</p>
</div>

| 섹션 | 질문 | 예시 (조사 전용 브리프) |
|---|---|---|
| Goal | 끝났을 때 무엇이 참이어야 하나? | 빈 할인 코드로 checkout 테스트가 실패하는 원인 조사, 최소 안전 수정 제안 |
| Context | 어떤 자료를 쓰나? | 실패한 테스트 출력과 checkout 검증 파일, 근처 테스트의 기존 패턴 |
| Scope | 무엇이 안이고 밖인가? | checkout 검증 흐름·관련 테스트만. **파일 수정·무관한 리팩터·public API 변경 금지** |
| Validation | 무엇을 돌려받나? | 가능한 원인, 살펴본 파일, 제안 변경, 제안 검사, 미해결 질문. **제안한 검사를 "완료된 테스트"로 표시하지 말 것** |
| Stop conditions | 언제 멈추고 묻나? | 가격 로직·고객 데이터·배포 설정에 닿거나 범위가 넓어지면. 구현은 **별도 승인** 필요 |

핵심은 **행동 경계를 명시**하는 것: "조사만"인지, "승인된 구현+검증"인지. "수정 금지" 지시는 담당자가 바꾸기 전까지 유효해요. 수정을 허락한다면 바꿀 수 있는 것, 돌릴 수 있는 검사, 돌려줄 증거, 사람 승인이 필요한 지점을 정해요.

**대비 예시(강의 기준) — 구현까지 허락하는 브리프**: Blossom Airlines 샘플 iOS 예약 앱이 좌석 선택 후 "Booking failed"를 띄우는 상황. 같은 5섹션으로 "재현 → 집중 수정 → 검증"을 허락하되, 좌석·부가 옵션·마일리지·총액이 유지된 예약 완료를 목표로, 승인된 저장소·환경, 반복할 예약 여정·검사·diff·결과 보고, 그리고 merge·배포 전 누가 리뷰하는지를 적어요. 저장소 접근·빌드/리셋 단계·테스트 명령은 실행 전에 확인이 필요해요.

### Context 연속성

일이 조사 → 계획 → 구현 → 검증 → 리뷰 → 문서 → 후속 작업으로 넘어가도 **이해 가능한 상태로 남는 것**이에요. 다음 사람이 처음부터 재구성하지 않게요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 나쁜 핸드오프</span>
    <p>"조사 완료."</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 좋은 핸드오프</span>
    <ul>
      <li>관찰된 실패, 살펴본 파일·로그 2개</li>
      <li>가능한 원인과 근거 (확정 아님)</li>
      <li>테스트 환경이 없어 <b>아직 못 돌린</b> 검사</li>
      <li>리뷰할 사람과 다음 승인 단계: 환경 복구 → 검사 실행 → 수정 승인 여부 결정</li>
    </ul>
  </div>
</div>

cloud 핸드오프나 다른 기기에서 이어갈 때도 작업, 저장소·리비전, 저장된 변경, 실제 돌린 검사와 결과, 남은 일, 이어받을 엔지니어, 다음 승인 단계를 남겨요. **이어서 한다고 권한이 늘지 않아요.**

연속성 습관: 가장 작은 유용한 context로 시작 · 넓은 디렉터리 대신 관련 파일 지정 · 디버깅엔 로그·스택트레이스 포함 · "완료" 정의 · 큰 변경 전 계획 먼저 · 바뀐 것/남은 것 요약 요청 · 리뷰 증거를 워크플로우에 붙여두기 · 흐름이 엇나가면 체크포인트와 함께 새 스레드 · 민감 로직·프로덕션·소유권 불명이면 멈추기. **context가 너무 적으면 얕은 결과, 너무 많으면 통제가 어려워요.**

### Runtime = 협업 패턴

| Surface (강의 기준) | 잘 맞는 일 | 요청 예 |
|---|---|---|
| Codex IDE extension | 개발자가 코드 가까이서 하는 빠른 로컬 구현 | "이 함수 설명하고 더 작은 검증 변경과 테스트 수정안을 써줘. 계획 승인 전엔 수정 금지." |
| Codex CLI | 터미널 기반 조사, 로그, 스크립트, 반복 명령 | "실패한 테스트 출력을 보고 관련 파일을 찾고, 수정을 검증할 명령을 나열해." |
| ChatGPT 데스크톱 앱 + Codex | 로컬 context, 여러 스레드, 작업 이력, 검토 가능한 산출물 | "이 변경 세트로 문서 업데이트 계획을 세우고 리뷰용 요약 초안을 준비해." |
| Codex cloud / 비동기 | 증거를 돌려주는 장시간 위임 작업 | "반복 CI 실패를 조사해 원인 요약, 최소 안전 수정 제안, 검증 증거를 돌려줘." |
| GitHub 네이티브 코드 리뷰 | 저장소 워크플로우 안의 PR 리뷰(요청 시 또는 설정 시 자동) | PR에서 Codex를 호출해 AGENTS.md의 리뷰 가이드에 따라 리뷰 요청 |

핵심 질문: **"이 작업은 대화형 협업이 필요한가, 아니면 명확한 context·검증·리뷰와 함께 위임할 수 있나?"** CLI 작업은 클라이언트 버전·실제 모델·권한을 기록해 다른 엔지니어가 실행 조건을 알 수 있게 해요. 새 저장소 접근, cloud·비동기 실행, 민감 시스템, 경계를 넘는 명령, 프로덕션 영향, 감사·컴플라이언스, 고급 통합, 높은 권한, 되돌리기 어려운 작업이면 **기술·보안 SME**를 불러요.

## 3단계 — SDLC 전체에 적용하기

이 코스의 핵심 표예요. **모든 단계에 Codex를 밀어넣으라는 게 아니라, 각 결정을 명시적으로 만들라**는 거예요.

| SDLC 단계 | Codex가 돕는 것 | 엔지니어가 소유 | 필요한 통제 |
|---|---|---|---|
| 기획 | 티켓 요약, 빠진 수용 기준 찾기, 의존성 지도, 계획 초안 | 우선순위, 제품 trade-off, 아키텍처 방향, 최종 수용 기준 | 승인된 티켓 context, PO 리뷰, 결정 담당자, 모호하면 멈춤 |
| 구현 | 코드 설명, 구현 옵션, 작은 변경 초안, 테스트 골격. 승인 시 범위 내 구현 + diff·검증 증거 | 설계 판단, 코드 품질, merge·프로덕션 준비 결정 | 범위 지정 파일, 코딩 규칙, 사람 리뷰, 대규모 리팩터·public API 변경 제한 |
| 리뷰 | 검사용 요약 준비, 로컬 변경·앱 내 PR 리뷰, 설정된 저장소 트리거 리뷰 | 승인, merge, 아키텍처 판단, 위험 수용, 피드백 품질 | 리뷰 경로·저장소 접근 확인, AGENTS.md 같은 가이드, 이름 있는 리뷰어, **게시·승인·merge 권한 분리**, 자율 승인·merge 금지 |
| 테스트 | 테스트 케이스 제안, 단위·통합 테스트 초안, 실패 요약, 커버리지 공백. 허용 시 승인된 검사 실행 | 테스트 전략, 커버리지 수용, 품질 위험 해석, 릴리스 준비 결정 | 테스트 범위, 승인된 명령, 기대 출력, 검증 기록. **실제 결과와 제안을 구분** |
| 디버깅 | 로그 요약, 원인 후보, 제한된 파일 검사, 다음 단계, 조사 노트. 승인 시 재현→수정→재테스트 | 근본 원인 결정, 수정 승인, 고객 영향 평가, 장애 소유 | 승인된 로그, 저장소 경계, 명령 제한, 멈춤 조건, 민감·프로덕션 발견 시 에스컬레이션 |
| 문서 | 릴리스 노트·기술 문서 초안, 변경 요약, 핸드오프 노트 | 정확성, 게시, 고객 대상 문구, 최종 승인 | 승인된 소스, 문서 담당자, 게시 전 리뷰, 기밀 제외 |
| 장시간 작업 | 제한된 조사, 의존성 매핑, 리팩터 준비, 비동기 분석 → 계획·diff·증거 | 작업 승인, 환경 위험, 최종 수용, 계속/중단/에스컬레이션 | 정의된 목표, 승인된 저장소·환경, 체크포인트, 활동 가시성, 고위험이면 SME. cloud면 환경 소유자 |
| Context 연속성 | 무엇을 봤고·바꿨고·테스트했고·불확실한지 기록 | 팀 표준, 무엇이 source of truth가 될지, 핸드오프 노트 승인 | 요약 템플릿, 저장 위치, 담당자 리뷰, 오류 수정 절차 |

### 리뷰 경로 3가지와 4가지 별개 행동

<div class="oa-svg">
<svg viewBox="0 0 720 250" role="img" aria-label="리뷰 경로와 검사·게시·승인·merge의 분리">
  <defs>
    <marker id="ar2" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.r1{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.r2{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.r3{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}.rt{font-size:12px;fill:#4a4a4a}.rs{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="15" width="220" height="44" rx="8" class="r1"/><text x="120" y="42" text-anchor="middle" class="rt">로컬 리뷰 (공유 전 작업 변경)</text>
  <rect x="10" y="70" width="220" height="44" rx="8" class="r1"/><text x="120" y="97" text-anchor="middle" class="rt">앱 내 PR 리뷰</text>
  <rect x="10" y="125" width="220" height="44" rx="8" class="r1"/><text x="120" y="147" text-anchor="middle" class="rt">저장소 트리거 cloud 리뷰</text><text x="120" y="162" text-anchor="middle" class="rs">연결·접근·리뷰 설정 확인</text>
  <rect x="10" y="190" width="220" height="44" rx="8" class="r3"/><text x="120" y="210" text-anchor="middle" class="rt">요약 초안 준비</text><text x="120" y="225" text-anchor="middle" class="rs">리뷰 경로 아님 · 게시 권한 없음</text>
  <line x1="232" y1="120" x2="300" y2="120" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar2)"/>
  <rect x="305" y="40" width="190" height="40" rx="8" class="r2"/><text x="400" y="65" text-anchor="middle" class="rt">① 결과 검사 (inspect)</text>
  <rect x="305" y="90" width="190" height="40" rx="8" class="r2"/><text x="400" y="115" text-anchor="middle" class="rt">② 피드백 게시 (publish)</text>
  <rect x="305" y="140" width="190" height="40" rx="8" class="r2"/><text x="400" y="165" text-anchor="middle" class="rt">③ 변경 승인 (approve)</text>
  <rect x="305" y="190" width="190" height="40" rx="8" class="r2"/><text x="400" y="215" text-anchor="middle" class="rt">④ 병합 (merge)</text>
  <rect x="520" y="70" width="190" height="130" rx="10" class="r1"/><text x="615" y="105" text-anchor="middle" class="rt">각 행동마다</text><text x="615" y="125" text-anchor="middle" class="rt">누가 할 수 있는지</text><text x="615" y="145" text-anchor="middle" class="rt">따로 합의</text><text x="615" y="175" text-anchor="middle" class="rs">리뷰 접근 ≠ 게시·승인·merge 권한</text>
</svg>
</div>

### 사례로 보는 "보고된 검증" (강의 기준)

원래 Blossom 사례에선 견적 캐시 키가 **전체 예약 선택이 정해지기 전에** 만들어진 게 원인이었고, 키에 전체 선택을 넣는 수정이 보고됐어요. 검증은 예약 로직 자동 검사 1,763개 실패 0, 재빌드, 실패했던 예약 여정 반복이었고, 좌석·부가 옵션·마일리지·총액이 유지된 채 탑승권까지 도달했어요. 단, **자동 검사는 예약 로직만, 앱 재테스트는 한 운임만** 다뤘어요. 리뷰어가 여전히 변경·커버리지·남은 위험을 판단해야 하고, **통과 보고가 릴리스를 허락하지 않아요.**

### 장시간·비동기 작업

엔지니어가 매 턴 조종하지 않으니 **시작 전에 더 명확**해야 해요. 강의 기준 GPT-6 Astra를 쓸 때도 승인된 작업 범위 안에서 끝까지 해내되(요청 시 구현·검증, 아니면 조사에서 멈춤), 최종 보고에 변경·실제 돌린 검사·결과·남은 한계를 요구해요. **진행 상황 업데이트나 소요 시간은 완료의 증거가 아니에요.**

### SDLC 전반의 통제 질문 7개

<ol class="oa-flow">
  <li><strong>범위 안</strong>어떤 파일·로그·저장소?</li>
  <li><strong>범위 밖</strong>대규모 리팩터·민감 데이터·프로덕션</li>
  <li><strong>돌려받을 증거</strong>리뷰·핸드오프용</li>
  <li><strong>리뷰어</strong>사람의 소유·책임</li>
  <li><strong>승인 필요</strong>계속·확장 전 경계</li>
  <li><strong>에스컬레이션</strong>모호·실패·민감·위험</li>
  <li><strong>확장 근거</strong>열정 말고 증거</li>
</ol>

## 4단계 — 통제된 AI-native 엔지니어링 워크플로우

**AI-native ≠ 모든 걸 위임.** 같은 회사 안에서도 여러 패턴이 동시에 필요해요.

| 워크플로우 조건 | 추천 패턴 |
|---|---|
| 엔지니어가 탐색·학습하며 계속 조종해야 함 | 페어 프로그래밍 |
| 위임 준비도 체크리스트 통과 | 통제된 위임 |
| 산출물이 사람의 결정을 돕지만 결정하진 않음 | 리뷰 체크포인트 |
| 프로덕션 승인·고위험 판단 포함 | 더 강한 수동 워크플로우 |
| 요청이 넓고 모호하고 context 부족 | **작업 구조부터 개선** |

나쁜 위임 후보: 모호한 요구사항, 정의 안 된 비즈니스 규칙, 민감한 프로덕션 작업, 고위험 아키텍처 결정, 무제한 저장소 접근, 리뷰어·검증 경로·에스컬레이션 경로 없음.

### 통제된 워크플로우에서 보여야 할 것

| 가시성 질문 | 정의할 것 |
|---|---|
| Codex가 무엇에 접근하나? | 승인된 저장소·파일·로그·티켓·도구·서비스·환경, runtime의 실제 권한, cloud면 환경 소유자 |
| 무엇을 하라고 했나? | 대화형 지원 / 가이드 작업 / 제한된 위임 / 리뷰 준비 / 장시간 비동기 |
| 무엇을 보고 만들었나? | 작업·저장소·리비전, 살펴본 파일, 제안·저장된 변경·diff, **실제 돌린 검사와 결과**, 제안 검사, 열린 질문·한계 |
| 리뷰어는 무엇을 검사하나? | diff, 테스트 결과, 요약, 명령 출력, 가정, 빠진 증거 |
| 무엇에 승인이 필요한가? | 멈춤 지점, 피드백 게시·승인·merge·작업 확장 권한자. **도구 권한 ≠ 고객 승인** |
| 무엇이 에스컬레이션을 부르나? | 민감 로직, 소유권 불명, 검증 실패, 예상보다 넓은 범위, 프로덕션 위험 |

> Codex 협업은 **자동화가 가능해 보이는 곳**이 아니라, 일이 **명확하고·검토 가능하고·검증되고·통제되고·연속성과 연결된 곳**에서 확장해야 한다.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 반복 CI 실패. 로그가 있고 저장소도 알고, 코드 변경 전에 원인 후보와 증거를 돌려받아 리뷰어가 다음을 결정한다. 맞는 협업 패턴은?</summary>
  <p>① 페어 프로그래밍 ② 자동완성 ③ 통제된 위임 ④ 더 강한 수동 워크플로우</p>
  <p><b>정답: ③</b> — 작업이 제한적이고, 검토 가능하고, 증거 기반이에요.</p>
</details>
<details class="oa-quiz">
  <summary>Q2. 통제된 Codex 위임의 가장 좋은 후보는?</summary>
  <p><b>정답:</b> 로그가 있고 리뷰어가 원인 후보를 검사할 수 있는 <b>반복 CI 실패 조사</b>. ("서비스 전체 현대화" ✗ / 핵심 시스템 교체 같은 아키텍처 결정 ✗ / 승인 경로 없는 프로덕션 배포 승인 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q3. "이 모듈 고치고 테스트도 개선해줘"라는 요청의 주된 문제는?</summary>
  <p><b>정답: 명확한 범위와 리뷰용 산출물이 없다.</b> (IDE냐 CLI냐, cloud 여부, context 양의 문제가 아님)</p>
</details>
<details class="oa-quiz">
  <summary>Q4. 짝 맞추기 — 워크플로우 순간 ↔ 협업 패턴</summary>
  <p>사람 리뷰어가 결정하기 전에 Codex가 PR 증거를 준비 → <b>리뷰 체크포인트</b><br>승인된 로그로 반복 CI 실패 조사, 원인 후보 반환 → <b>통제된 위임</b><br>승인 경로 없이 프로덕션 배포 승인을 요구 → <b>더 강한 수동 워크플로우</b><br>로컬 함수를 이해하려고 대화형 도움을 원함 → <b>페어 프로그래밍</b></p>
</details>
<details class="oa-quiz">
  <summary>Q5. 엔지니어링 소유권을 지키며 Codex를 SDLC에 매핑한 가장 좋은 추천은?</summary>
  <p><b>정답:</b> 티켓 명확화, 구현 보조, 리뷰 증거 준비 같은 <b>제한된 워크플로우 순간</b>을 Codex가 지원하고, 결정·승인·책임은 엔지니어가 유지한다. (전 과정 인수 ✗ / 자동완성만 ✗ / 대기열 길면 배포 승인 ✗)</p>
</details>

## 정리

- 출발점은 "어디서 코드를 생성할까"가 아니라 **"어디서 시간·연속성·context를 잃나"**.
- 5가지 협업 모델(자동완성·페어·위임·리뷰 체크포인트·수동) 중 **워크플로우에 맞는 것**을 고른다. 한 조직에 여러 패턴이 공존한다.
- 위임은 **Clear·Bounded·Context-supported·Verifiable·Reviewable**을 통과할 때만. 아니면 위임 브리프(Goal·Context·Scope·Validation·Stop)로 구조부터 고친다.
- 행동 경계(조사만 vs 승인된 구현)를 명시하고, **제안한 검사와 실제 결과를 구분**한다. 이어받기는 권한을 늘리지 않는다.
- 리뷰 경로는 3가지, **검사·게시·승인·merge는 4개의 별개 결정**. 요약 초안은 게시 권한이 아니다.
- SDLC 각 단계마다 **Codex 지원 · 엔지니어 소유 · 통제** 세 칸을 모두 채워야 추천이 완성된다.
