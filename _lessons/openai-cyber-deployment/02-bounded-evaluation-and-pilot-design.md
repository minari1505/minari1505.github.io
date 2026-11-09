---
title: "Bounded evaluation and pilot design"
title_ko: "제한된 평가와 파일럿 설계 — 관심을 검증 가능한 증거로"
course: openai-cyber-deployment
lesson: 2
---

## 학습 목표

- 첫 Daybreak 평가를 **승인된 워크플로우·대상 하나**로 묶기 (강의 기준)
- 안전하고 측정 가능한 **첫 평가 대상** 고르기
- 범위·리뷰어·증거 기대치·중단 조건·에스컬레이션을 담은 **평가 헌장(charter)** 만들기
- 발견 개수가 아니라 **품질 기반 스코어카드** 짜기
- 과장 없이 **평가 리듬**을 계획하고, 고객용 추천서로 포장하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>왜 제한하나</strong>관심 → 방어 가능한 증거</li>
  <li><strong>첫 대상 선택</strong>8가지 조건</li>
  <li><strong>평가 헌장</strong>질문·범위·사람·증거·중단</li>
  <li><strong>성공 지표</strong>병목에 묶인 스코어카드</li>
  <li><strong>평가 리듬</strong>사전 점검 → 실행·리뷰 → 결정</li>
  <li><strong>고객용 추천</strong>6가지 결정 경로</li>
</ol>

> **"다 테스트해 보자"가 아니라, 승인된 워크플로우 하나에서 시작한다.**

## 1단계 — 왜 제한된 평가인가

<div class="oa-eli">
  <span class="oa-eli__head">🧪 쉽게 말하면</span>
  <p>새 비료가 좋은지 알고 싶을 때 농장 전체에 뿌리면, 수확이 늘어도 비료 덕인지 날씨 덕인지 모르고, 망하면 농사 전체가 망해요. <b>한 고랑</b>에만 뿌리고, <b>누가 무엇을 재서</b> <b>어떤 결과면 성공</b>인지 미리 정해야 진짜 답을 얻어요. 제한된 평가는 "작게 하자"가 아니라 <b>"제대로 배우자"</b>예요.</p>
</div>

"AI로 보안하고 싶어요", "백로그 해결되나요?", "사이버 모델 쓸 수 있나요?" 같은 관심은 좋은 신호지만 **파일럿 준비 완료의 증거는 아니에요.** 준비도를 확인하는 간단한 질문:

> **"무엇을 리뷰하고, 누가 리뷰하고, 어떤 증거를 인정하고, 끝에 고객이 무슨 결정을 내리나?"** — 다 답할 수 있으면 평가 계획으로 넘어가요.

좋은 첫 평가는 **저장소 하나 / 앱·서비스 하나 / 백로그 조각 하나 / 취약점 유형 하나 / PR·diff 패턴 하나 / 승인된 하위 환경 하나** 중 하나에 집중해요.

<ol class="oa-flow">
  <li><strong>승인된 대상 1개</strong></li>
  <li><strong>워크플로우 질문 정의</strong></li>
  <li><strong>리뷰어·증거 경계 확인</strong></li>
  <li><strong>승인된 워크플로우 실행</strong></li>
  <li><strong>증거 리뷰</strong></li>
  <li><strong>결정</strong>계속·조정·확장·중단·종료·에스컬레이션</li>
</ol>

### 제한된 평가가 아닌 것

| 아님 | 왜 |
|---|---|
| 운영 롤아웃 | 학습이 목적, 배포가 아님 |
| 사이버 특화 모델 접근 약속 | 접근은 별도 경로 |
| 전 시스템 대상 광범위 취약점 연구 | 리뷰어가 감당 못 함 |
| 명시 승인 없는 운영 테스트, 에스컬레이션 없는 공격 위주 작업 | 위험·거버넌스 |
| 저장소 전반 자동 수정 | 사람 리뷰 필수 |
| 스캐너·보안 도구·기록 시스템·사람 리뷰 대체 | 보완재일 뿐 |

대화가 접근 우선·자동화 우선으로 흐르면 이렇게 돌려요: *"더 넓은 롤아웃이나 접근 경로를 논의하기 전에, 승인된 워크플로우·대상 자산·리뷰어·개선을 보여줄 증거부터 정합시다."*

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 잘못된 질문</span>
    <p>"워크플로우가 이슈를 몇 개나 찾았나?"</p>
    <p>→ 노이즈·중복·낮은 신뢰도의 finding은 오히려 일을 늘려요.</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 올바른 질문</span>
    <p>"워크플로우가 고객이 <b>더 나은 보안 결정</b>을 내리도록 도왔나?"</p>
    <p>→ 확정·억제·보류·수정·에스컬레이션·미해결 중 무엇인지 판단할 증거가 좋아졌나.</p>
  </div>
</div>

파트너 화법 포인트: *"작은 테스트만 할 수 있어요"*가 아니라 **"제한된 평가가 확장할 가치가 있는지 증명하는 가장 빠르고 책임 있는 방법입니다."**

## 2단계 — 첫 평가 대상 고르기

### 강한 첫 대상의 8가지 조건

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🏠</span><span class="oa-card__title">Owned</span>고객이 저장소·앱·백로그·환경을 통제</div>
  <div class="oa-card"><span class="oa-card__icon">✍️</span><span class="oa-card__title">Authorized</span>보안·엔지니어링 이해관계자가 리뷰 승인</div>
  <div class="oa-card"><span class="oa-card__icon">📏</span><span class="oa-card__title">Bounded</span>범위를 명확히 설명·강제 가능</div>
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">Relevant</span>실제 보안 병목·비즈니스 위험과 연결</div>
  <div class="oa-card"><span class="oa-card__icon">👀</span><span class="oa-card__title">Reviewable</span>이름 있는 리뷰어가 평가 가능</div>
  <div class="oa-card"><span class="oa-card__icon">📊</span><span class="oa-card__title">Measurable</span>시작 전에 성공 지표 정의 가능</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">Context-rich</span>코드·테스트·스캐너·티켓·아키텍처 context 충분</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">Safe</span>무승인 운영 테스트·공격 위주·불명확 데이터·소유권 없음</div>
</div>

하나가 빠지면 불가능한 건 아니지만 **추가 범위 설정**이 먼저예요. 그리고 **적합한 대상 ≠ 실행 허가** — 실행 전에 선택한 제품의 활성 접근과 자산·행동 승인을 확인해요.

| 첫 대상 패턴 | 잘 맞을 때 | 주의 |
|---|---|---|
| 저장소 하나 | 알려진 코드베이스의 구조적 평가 | 소유권, 브랜치·커밋 범위, 리뷰어, 데이터 경계 |
| 앱·서비스 하나 | 알려진 AppSec 우려가 있는 고가치 시스템 | 연결된 모든 서비스로 번지지 않게 |
| 백로그 조각 하나 | 스캐너 노이즈가 심함 | finding 선정 방식과 확정/억제 증거 기준 |
| 취약점 유형 하나 | 인가·입력 검증·의존성·비밀 처리 같은 반복 이슈 | 유용할 만큼 넓게, 리뷰 가능할 만큼 좁게 |
| PR·diff 패턴 하나 | merge 전 민감 변경 리뷰 | 결정 지점(merge 준비? 증거? 수정 가이드?) |
| 하위 환경 하나 | 운영 밖 제한적 테스트·재검증 승인 | 운영 증명은 주장 금지 |

병목이 **스캐너 노이즈**면 백로그 조각, **릴리스 위험**이면 민감 PR이 맞아요. 리뷰 surface도 범위에 맞춰요: 로컬 플러그인은 집중 리뷰, GitHub PR 리뷰는 변경 검토, Security Cloud는 연결 저장소 스캔·새 커밋 감시 (강의 기준). **백로그 조각에 집중한다고 해서 저장소 스캔 범위가 저절로 좁아지진 않아요.**

### 나쁜 첫 대상 5가지

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🌊</span><span class="oa-card__title">너무 넓음</span>모든 저장소·앱, 광범위 자율 테스트</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">❓</span><span class="oa-card__title">소유권·승인 불명</span>제3자 인프라, 승인 없는 운영 서비스, 리뷰어·결정권자 없음</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⚠️</span><span class="oa-card__title">고위험 테스트</span>레드팀, 공격 위주 작업</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🏭</span><span class="oa-card__title">민감·특수 환경</span>핵심 인프라, 맞춤 harness, 특이 배포, 민감 데이터</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔑</span><span class="oa-card__title">접근 우선 프레이밍</span>워크플로우보다 사이버 특화 접근부터</div>
</div>

이런 신호가 보이면 속도를 늦추고, 범위를 명확히 하거나 보안·엔지니어링 소유자를 끌어들이거나 **OpenAI SME로 에스컬레이션**해요.

**예시**: 후보가 ① 모든 저장소 ② 결제 서비스 저장소 ③ 지난주 SAST 상위 40개 finding(결제 서비스)일 때 — 병목이 커버리지면 ②, 노이즈면 ③이 더 좋아요. ①은 가장 약해요.

## 3단계 — 평가 헌장(Evaluation Charter)

<div class="oa-eli">
  <span class="oa-eli__head">⚽ 쉽게 말하면</span>
  <p>친선 경기도 시작 전에 <b>경기장 크기, 심판, 경기 시간, 어떤 상황에 경기를 멈추는지</b>를 정해요. 시작하고 나서 "골대 좀 넓히자"고 하면 결과를 아무도 안 믿죠. 헌장은 평가의 <b>경기 규칙</b>이에요.</p>
</div>

### 질문과 가설

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 질문</span>
    <p>"Daybreak가 우리 보안을 개선할 수 있나?"</p>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 질문</span>
    <p>"Codex Security가 customer-account-service 저장소의 상위 50개 스캐너 finding을 <b>더 명확한 증거, 더 적은 오탐, 더 적은 리뷰 부담</b>으로 검증하도록 리뷰어를 도울 수 있나?"</p>
  </div>
</div>

강한 질문은 **워크플로우·대상·리뷰어 작업·필요한 증명 유형**을 다 담아요. 가설 예: *"이 저장소와 백로그 조각에서 Codex Security는 리뷰어가 유효한 finding과 노이즈를 가르고 더 명확한 개발자 인계를 준비하도록 돕는다."*

### 범위와 경계

| 범위(Scope)에 들어갈 것 | 경계(Boundaries)에 들어갈 것 |
|---|---|
| 대상 자산·저장소·백로그·유형·PR 패턴·환경 | 명시적 범위 밖 항목 |
| 브랜치·커밋·기간·finding 세트 | 런타임 테스트 허용 여부 |
| 선택한 surface·워크플로우·트리거 | 수정 지원 허용 여부 |
| 기대 산출물 | 허용 행동(finding 게시, 수정 제안) — **승인·merge는 별도 권한** |
| 리뷰 프로세스, 최종 결정 지점 | 운영 제외, 사용 가능한 데이터·로그·파일, 중단 조건 |

범위 밖 항목은 행정 디테일이 아니라 **평가를 보호하는 장치**예요. 경계 안에선 빠르게, 경계를 넘을 땐 천천히 갈 수 있게 해줘요. Cloud 파일럿이라면 승인된 저장소·리비전·감시 기간을 기록하고, 스캔이 승인 범위를 넘으면 더 좁은 워크플로우를 고르거나 추가 승인을 받아요.

### 이름 있는 사람들

| 역할 | 책임 |
|---|---|
| 비즈니스·보안 스폰서 | 평가가 왜 중요한지, 보안 목표와 어떻게 연결되는지 확인 |
| AppSec·보안 리뷰어 | finding·증거 품질·유효성·심각도 근거·proof gap 리뷰 |
| 엔지니어링 소유자 | 기술 타당성·코드 영향·테스트 기대치·수정 준비도 |
| 결정 소유자 | 계속·조정·확장·중단·종료·에스컬레이션 결정 |
| 파트너·OpenAI 지원 | 범위 설정·진행·에스컬레이션 지원 (책임자를 대체하지 않음) |

흔한 실패: **도구와 대상은 정했는데 사람의 리뷰 경로를 안 정함.** 지속 작업이면 스캔 구성·예산 소유자, 리뷰 여력, 중단 권한자도 이름을 붙여요.

### 증거 기대치와 상한선 — 공개된 사례에도 똑같이

강의는 고객이 보고한 관찰 하나를 예로 들어요: 한 회사 CTO가 GPT-6 Astra(강의 기준)에 대해 "다섯 개 버그를 찾으면 다섯 개 다 진짜"라고 말한 사례예요. 이건 **고객이 보고한 엔지니어링 관찰**이지 독립적인 보안 평가가 아니고, 표본 크기·분모·방법론이 없어요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 지지하는 것</span>
    <p>설명된 버그 finding들에 대해 그 팀이 <b>신뢰를 느꼈다</b>는 것</p>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 지지하지 않는 것</span>
    <p>100% 정밀도, 취약점 수정 효과, 운영 안전성, 고객 보안 자산 전반의 개선 — 각각 별도의 승인된 평가와 리뷰어 증거가 필요</p>
  </div>
</div>

공개된 능력 증거는 평가에 참고가 될 순 있지만, **고객 환경에서의 결과를 증명하거나 테스트를 승인하지 않아요.** 헌장에는 보존할 산출물도 정해요: 리뷰 기록, 증거 요약, 스코어카드, 수정 인계, proof gap 목록, 임원용 요약, (고객 승인 시) 기록 시스템 항목.

### 예시 — 저장소 하나 Cloud 파일럿 헌장 (접근·승인 미확인 상태)

| 항목 | 내용 |
|---|---|
| 평가 질문 | customer-auth-service의 인증 관련 finding을 더 명확한 증거·더 적은 리뷰 부담으로 검증·우선순위화하도록 도울 수 있나? |
| 대상 | GitHub 저장소 하나. 승인 범위와 리뷰한 리비전 기록 |
| 범위 안 | 승인된 저장소 스캔 1회, 스캐너·아키텍처 context 활용, AppSec·엔지니어링 리뷰 |
| 범위 밖 | 운영 테스트, 제3자 시스템, 공격 위주 검증, 자동 수정·merge, 헌장에 없는 저장소 |
| 리뷰어 | AppSec 리드(유효성·증거), 엔지니어링 소유자(수정 타당성), 보안 스폰서(최종 결정) |
| 증거 상한선 | 소스 기반 증거 + 승인된 하위 환경 테스트까지. 운영 런타임 증명 주장 안 함 |
| 시작 조건 | 활성 접근, 저장소 연결, 고객 승인, 구성·예산 소유자, 리뷰 여력, 성공 기준·증거 보관 위치 합의 |
| 지속 작업 | 감시는 범위·기간·트리거·리뷰·예산을 별도 승인 |
| 중단 조건 | 범위·접근·예산·리뷰 여력이 부족해지면 중단. 긴급 릴리스 위험·운영 테스트 필요·헌장 밖 작업은 에스컬레이션 |

## 4단계 — 성공 지표: 활동이 아니라 결정을 재라

<div class="oa-eli">
  <span class="oa-eli__head">🎣 쉽게 말하면</span>
  <p>낚시 대회에서 "몇 번 낚싯대를 던졌나"로 순위를 매기진 않죠. 장화·깡통을 100개 건지는 것보다 <b>먹을 수 있는 물고기 5마리</b>가 낫듯, finding 개수가 아니라 <b>쓸모 있는 결정</b>을 세야 해요.</p>
</div>

| 병목 | 지표 영역 |
|---|---|
| 분류·검증·우선순위·증거 수집이 막힘 | Finding 품질 |
| finding이 엔지니어가 행동하기에 불명확 | 개발자 인계 |
| 확정 finding → 리뷰된 수정·테스트·재검증이 느림 | 수정 진행 |
| 사람 판단은 지키면서 리뷰 부담을 줄이고 싶음 | 리뷰어 노력 |
| 산출물을 기존 시스템에서 리뷰·보관·기록할 수 있어야 함 | 운영 통제 |

"엔지니어링이 티켓을 안 믿어요"라면 finding 개수는 아무것도 증명하지 못해요. **증거 유용성, 인계 품질, 수용된 수정 비율, 리뷰어 신뢰, proof gap 표시 품질**이 맞는 지표예요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔍</span><span class="oa-card__title">Finding 품질</span>유효성, 증거 유용성, 오탐 감소, 중복 감소, 우선순위 품질, 전제 조건 명확성, proof gap 표시, 리뷰어 신뢰</div>
  <div class="oa-card"><span class="oa-card__icon">🩹</span><span class="oa-card__title">수정·리뷰</span>패치 품질, 테스트·CI 통과율, 수용된 수정 비율, 리뷰된 수정까지 시간, 인계 품질, 리뷰어 부담, 회귀 신뢰, 잔여 위험 요약 품질 <i>(수정이 범위 안일 때만)</i></div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">⚙️</span><span class="oa-card__title">운영·거버넌스</span>finding당 리뷰 시간, 에스컬레이션 비율, 재작업, 보관 증거 품질, 기록 시스템 적합성, 사람 통제 지점 명확성, 리뷰된 고유 finding당 비용·시간</div>
</div>

### 반복 가능하게 만들기

- 결과가 나오기 **전에** 고정된 승인 표본을 고르고, finding마다 확정·거절·미해결의 기준을 기록
- finding·근거 산출물·리뷰어 판단·proof gap을 **함께** 보관 → 다른 리뷰어도 점수를 이해 가능
- 새 모델·방식과 비교할 땐 **같은 작업, 비슷한 조건**으로, 차이를 기록
- 중복 발생은 연결만 하고 별도 finding으로 세지 않기, **리뷰 안 된 finding을 확정으로 세지 않기**
- Cloud라면 사용 비용과 청구 비용을 구분하고 미완료 실행도 포함. 리뷰된 finding이 0이면 finding당 비율 대신 **총비용**을 보고

<div class="oa-bars">
  <div class="oa-bar"><span>Pilot A — finding 수</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:90%"></span><span class="oa-bar__val">많음</span></span></div>
  <div class="oa-bar"><span>Pilot A — 결정 품질</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:25%"></span><span class="oa-bar__val">노이즈·중복·리뷰 부담↑</span></span></div>
  <div class="oa-bar"><span>Pilot B — finding 수</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:35%"></span><span class="oa-bar__val">적음</span></span></div>
  <div class="oa-bar"><span>Pilot B — 결정 품질</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:88%"></span><span class="oa-bar__val">증거·근거·인계 명확</span></span></div>
  <p class="oa-bars__caption">개념적 표현 — 강의의 두 파일럿 비교를 그림으로 옮긴 것(실제 수치 아님). 더 유용한 쪽은 Pilot B.</p>
</div>

## 5단계 — 평가 리듬

<div class="oa-svg">
<svg viewBox="0 0 720 170" role="img" aria-label="평가 리듬: 사전 점검, 실행·리뷰, 수정 리뷰, 최종 리뷰">
  <defs>
    <style>.r1{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.r2{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.t1{font-size:13px;fill:#4a4a4a;font-weight:700}.t2{font-size:11px;fill:#6a6a6a}.ar{stroke:#b9b9b9;stroke-width:2;fill:none}</style>
  </defs>
  <rect x="10" y="20" width="155" height="100" rx="10" class="r1"/><text x="87" y="48" text-anchor="middle" class="t1">① 사전 점검</text><text x="87" y="72" text-anchor="middle" class="t2">범위·승인·리뷰어</text><text x="87" y="90" text-anchor="middle" class="t2">"무엇이 우릴 멈추나?"</text>
  <path d="M168 70 L190 70" class="ar"/>
  <rect x="193" y="20" width="155" height="100" rx="10" class="r1"/><text x="270" y="48" text-anchor="middle" class="t1">② 실행·리뷰</text><text x="270" y="72" text-anchor="middle" class="t2">보고서 = 증거 패키지</text><text x="270" y="90" text-anchor="middle" class="t2">상태별 분류</text>
  <path d="M351 70 L373 70" class="ar"/>
  <rect x="376" y="20" width="155" height="100" rx="10" class="r1"/><text x="453" y="48" text-anchor="middle" class="t1">③ 수정 리뷰</text><text x="453" y="72" text-anchor="middle" class="t2">헌장에 있을 때만</text><text x="453" y="90" text-anchor="middle" class="t2">확정 finding 한정</text>
  <path d="M534 70 L556 70" class="ar"/>
  <rect x="559" y="20" width="151" height="100" rx="10" class="r2"/><text x="634" y="48" text-anchor="middle" class="t1">④ 최종 리뷰</text><text x="634" y="72" text-anchor="middle" class="t2">요약이 아니라</text><text x="634" y="90" text-anchor="middle" class="t2">결정을 낸다</text>
  <text x="360" y="150" text-anchor="middle" class="t2">각 단계마다 체크포인트 — 막히면 조정·중단·에스컬레이션</text>
</svg>
</div>

**사전 점검**: 목적·대상, 경계, 책임, 성공 기준, 안전 통제. Security Cloud면 활성 접근·저장소 연결·허용 활동·예산, Enterprise/Edu면 Codex Cloud와 Codex Security 권한도 확인해요. Daybreak Blue 접근은 다른 제품으로 확장되지 않아요 (강의 기준).

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🛑</span><span class="oa-card__title">강한 중단 조건 예</span>승인 대상 밖 접근 필요 / 미승인 런타임 테스트 필요 / 운영 시스템 연루 / 긴급 릴리스·운영 위험 시사 / 고객 데이터 처리 불명확 / 미승인 수정 요청 / 범위 정하기 전 사이버 특화 접근 요구 / 예산·리뷰 여력 부족</div>
</div>

**실행·리뷰**: 승인된 워크플로우 실행 → 보고서를 증거 패키지로 리뷰 → 후보·검증·오탐·억제·보류·proof gap 분류 → 수정 리뷰로 넘길지 결정. **보고서는 결정이 아니라 결정을 위한 증거**예요. 고유 이슈마다 증거·불확실성·리뷰어 결정·완료/필요 검증·다음 소유자를 기록해요.

**수정 리뷰(범위 안일 때만)**: 확정됐나? 근본 원인에 맞나? 의도된 동작을 보존하나? **보안 확인과 회귀 테스트를 따로** 정했나? 엔지니어링이 수용하나? 남은 gap은? 최종 승인·릴리스는 누가?

### 확장 기준 — 흥분이 아니라 증거로

결과를 모으기 **전에** 확장·재테스트 기준(기준선, 선정 케이스, 채점 규칙, 리뷰 소유자, 필요 증거)을 합의해요. 아래 표는 다음 단계를 **추천**할 뿐 **승인**하지 않아요.

| 증거가 보여주는 것 | 책임 있는 다음 단계 |
|---|---|
| 유효·유용, 증거 품질 강함, 리뷰 부담 감당 가능 | 인접 저장소·백로그·워크플로우·팀 **하나** 추가 고려 |
| 리뷰어가 신뢰하고 기록 시스템에 맞음 | 업데이트한 지표·같은 리뷰 규율로 반복 |
| 수정 산출물이 리뷰 가능, 거버넌스·데이터 기대치 명확 | 사람 리뷰·증거 보관·에스컬레이션을 유지하며 신중히 확장 |
| 다음 범위를 명확히 제한할 수 없음 | 중단하고 범위를 좁힘 |
| 운영 테스트·핵심 인프라·공격 위주·맞춤 harness·headless CI/CD·광범위 자율 테스트·사이버 특화 접근·특수 데이터 약속·불명확한 외부 대상 | **중단 후 에스컬레이션** |

> 강한 확장: *"증거상 같은 리뷰 프로세스와 업데이트한 지표로 인접 저장소 하나를 추가할 수 있습니다."*
> 위험한 확장: *"첫 테스트가 괜찮았으니 전부 켭시다."*

## 6단계 — 고객용 평가 추천서

추천서는 **할 일을 제안**하는 것이지 결과를 보고하거나 가치가 증명됐다고 주장하는 게 아니에요. 공유 전 체크: 명확한 질문, 승인된 대상 하나, 범위 안/밖, 리뷰어·결정 소유자, 병목에 묶인 지표, 증거 기대치·한계, 중단·에스컬레이션 트리거, 리뷰 프로세스, 최종 결정 지점. 접근·승인·예산·리뷰 여력이 미확인이어도 **리뷰 준비**는 될 수 있지만, 각 항목과 소유자를 기록하고 **"실행 준비 완료"라고 부르지 않아요.**

| 결정 | 의미 |
|---|---|
| Continue | 유용함, 같은 범위로 계속 |
| Adjust | 유망하지만 대상·지표·증거 기대치·리뷰 프로세스 보완 필요 |
| Expand | 인접 저장소·백로그·워크플로우·팀 하나 추가를 증거가 지지 |
| Pause | 더 강한 증거·리뷰어 신뢰, 또는 범위·접근·데이터·거버넌스 문제 해결 필요 |
| Stop | 현재 필요에 유용하지 않음 |
| Escalate | 승인·운영 테스트·공격 위주·핵심 인프라·특수 접근·불명확 데이터 등 경계에 닿음 |

**"파일럿 건너뛰고 바로 전면 롤아웃하고 싶어요"**에 대한 답: *"관심은 좋은 신호지만, 넓은 롤아웃은 평가 증거로 얻어야 합니다. 승인된 저장소·백로그 조각 하나로 시작해 리뷰어·지표·증거 기대치·결정 경로를 합의하면, 무엇을 확장하고 무엇을 바꾸고 무엇을 범위 밖에 둘지 판단할 근거가 생깁니다."*

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. "전체 그림을 보게 모든 저장소에 Daybreak를 돌리고, 리뷰어와 지표는 결과 보고 정할게요." 가장 좋은 답은?</summary>
  <p><b>정답: 넓은 스캔은 나중에 쓸모 있을 수 있지만, 첫 평가는 승인된 대상 하나·이름 있는 리뷰어·증거 기대치·품질 기반 지표로 시작해야 신뢰할 결정을 낼 수 있다.</b> (사이버 특화 접근부터 ✗ / 자동 패치부터 ✗ / 더 많은 finding이 가치 증명 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q2. 소유한 내부 저장소의 시끄러운 스캐너 백로그, AppSec 리드·엔지니어링 매니저가 리뷰 가능. 가장 강한 첫 대상은?</summary>
  <p><b>정답: 그 저장소의 정의된 백로그 조각을, 이름 있는 소유자들이 증거 품질·결정 지표로 리뷰.</b> (운영 앱 실시간 테스트 ✗ / 사이버 특화 접근 신청 ✗ / 전 저장소 비교 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q3. 평가 계획에 저장소 이름만 있고 범위 밖 활동·증거 한계·리뷰어·중단 조건·지표가 없다. 다음 단계는?</summary>
  <p><b>정답: 실행 전에 승인 범위·허용 행동·리뷰어·증거 기대치·성공 지표·예산·중단 조건으로 헌장을 완성한다.</b> (일단 진행 ✗ / 더 넓은 접근 요청 ✗ / 데모로 대체 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q4. 고객의 진짜 병목은 "어떤 finding이 유효한지 모르고, 엔지니어링이 인계를 안 믿는 것". 가장 강한 스코어카드는?</summary>
  <p><b>정답: finding 유효성, 증거 유용성, 오탐 감소, 개발자 인계 품질, 리뷰어 부담, 리뷰된 수정까지 시간.</b> (총 finding·저장소 수 ✗ / 거절 회피 수·접근 신청 수 ✗ / 자동 패치·운영 테스트 수 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q5. 첫 평가가 성공적이었고 고객이 다음 주에 전 저장소 CI/CD에 넣고 싶어 한다. 최선은?</summary>
  <p><b>정답: 같은 리뷰 프로세스·업데이트한 지표로 인접 저장소나 백로그 조각 하나를 추가하고, CI/CD 통합은 더 강한 증거·거버넌스·기술 리뷰가 필요한 이후 단계로 둔다.</b> (바로 롤아웃 ✗ / 접근 얘기로 전환 ✗ / 모든 확장 중단 ✗)</p>
</details>
<details class="oa-quiz">
  <summary>Q6. 저장소 하나·시끄러운 백로그·리뷰어·엔지니어링 소유자가 있는 고객이 파일럿을 건너뛰고 전면 롤아웃을 원한다. 가장 강한 추천은?</summary>
  <p><b>정답: 이름 있는 리뷰어·성공 지표·증거 기대치·중단 조건·최종 결정 경로를 갖춘 제한된 첫 평가를 먼저 하고, 그 뒤에 확장한다.</b></p>
</details>

## 정리

- 관심은 계획이 아니다. **승인된 워크플로우·대상·리뷰 경로·지표 세트 하나**로 첫 평가를 묶는다.
- 첫 대상은 **Owned · Authorized · Bounded · Relevant · Reviewable · Measurable · Context-rich · Safe**. 적합한 대상도 실행 허가는 아니다.
- **헌장**이 질문·범위·사람·증거 상한선·지표·중단 조건을 고정한다. 공개된 성과 사례도 같은 증거 규율로 읽는다.
- 지표는 **병목에 묶고**, 결과 전에 기준을 정하고, 리뷰 안 된 finding을 확정으로 세지 않는다. 더 많은 finding ≠ 더 나은 결정.
- 리듬은 **사전 점검 → 실행·리뷰 → (범위 안이면) 수정 리뷰 → 최종 결정**. 확장은 증거로, 한 칸씩.
