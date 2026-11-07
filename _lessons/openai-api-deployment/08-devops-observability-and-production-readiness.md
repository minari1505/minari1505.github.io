---
title: "DevOps, Observability, and Production Readiness for APIs"
title_ko: "DevOps·관측성·프로덕션 준비 — '로컬에서 됨'에서 '운영 가능'까지"
course: openai-api-deployment
lesson: 8
---

## 학습 목표

- 환경·설정·secret·릴리스·승인 **가정**을 정의하기
- health·가용성·로그·trace·알림·검토 담당자 등 **관측성(observability) 요구사항** 정하기
- 실패 처리·지원 담당·롤백·에스컬레이션·**graceful degradation** 계획하기
- 준비도 증거를 평가해 남은 **blocker·담당자·검증 단계** 찾기
- **API Production Readiness Checklist**와 출시 추천(release recommendation) 만들기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>환경·릴리스 가정</strong>어디서 돌고, 무엇을 근거로 올리나</li>
  <li><strong>관측성</strong>출시 후 무엇이 보여야 하나</li>
  <li><strong>실패·지원</strong>무엇이 깨지고, 누가 대응하나</li>
  <li><strong>준비도 체크리스트</strong>5가지 질문 → 추천</li>
</ol>

<div class="oa-eli">
  <span class="oa-eli__head">🏪 쉽게 말하면</span>
  <p>집 부엌에서 요리가 맛있게 됐다고 바로 <b>식당을 여는 건 아니죠</b>. 가게 위치(환경), 재료 보관법(설정·secret), 개업 허가(승인), 손님이 몰릴 때 대응(용량·알림), 불이 나면 누가 뭘 하는지(실패 대응·담당자)를 정해야 해요.</p>
  <p>프로덕션 준비란 API가 <b>"작동한다"를 넘어 관찰·지원·문서화·복구가 가능하고, 실제 사용자에게 내놓을 만큼 안전하다</b>는 뜻이에요.</p>
</div>

## 핵심 용어

| 용어 | 한 줄 정의 | 비유 |
|---|---|---|
| **DevOps** | 소프트웨어를 설정·출시·모니터링·지원·개선하는 운영 규율 | 식당 운영 매뉴얼 |
| **Observability (관측성)** | 출시 후 API가 무엇을 하는지 보여주는 증거 | 자동차 계기판 |
| **Production readiness** | 작동 + 관찰·지원·문서·복구·안전까지 갖춘 상태 | 개업 허가 + 소방 점검 통과 |
| **Release/version identifier** | 지금 돌고 있는 API·프롬프트·모델 설정·도구·검색 설정의 라벨 | 우유팩 제조 번호 |
| **Smoke test** | 배포 직후 가장 중요한 경로만 빠르게 확인 | 전원 켜고 연기 안 나는지 보기 |
| **Rollback** | 이전의 정상(known-good) 버전·설정으로 되돌리기 | 게임 세이브 파일 불러오기 |
| **Graceful degradation** | 실패를 숨기거나 위험하게 행동하는 대신 제한된 안전한 응답 | 정전 시 비상등 |
| **Incident-response route** | 누가 먼저 대응하고, 누구에게 올리고, 누가 결정하나 | 화재 대피 경로도 |

### 이 코스의 DevOps = 4개의 연결된 질문

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔧</span><span class="oa-card__title">Configure</span>맞는 환경·설정·secret·의존성·접근 경계로 돌아가나?</div>
  <div class="oa-card"><span class="oa-card__icon">🚀</span><span class="oa-card__title">Release</span>무엇을 올리는지, 근거·승인자·롤백 방법이 있나?</div>
  <div class="oa-card"><span class="oa-card__icon">👀</span><span class="oa-card__title">Observe</span>건강·신뢰성·비용·지연·쿼터가 보이나?</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🔁</span><span class="oa-card__title">Respond & improve</span>문제를 감지해 담당자에게 보내고, 계속/보완/중지/롤백을 결정하나?</div>
</div>

## 1. 환경과 릴리스 가정

### 환경마다 증명하는 것이 다르다

<div class="oa-svg">
<svg viewBox="0 0 720 210" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Local, Development, Staging, Production 환경 계단">
  <g font-family="inherit" font-size="13">
    <rect x="10"  y="140" width="160" height="60" rx="10" fill="#fff8df" stroke="#f1e4b8"/>
    <rect x="190" y="105" width="160" height="95" rx="10" fill="#fff1c2" stroke="#f1e4b8"/>
    <rect x="370" y="70"  width="160" height="130" rx="10" fill="#ffe89e" stroke="#e8cf7a"/>
    <rect x="550" y="35"  width="160" height="165" rx="10" fill="#ffdd7a" stroke="#d9b850"/>
    <text x="90"  y="165" text-anchor="middle" font-weight="700" fill="#4a4a4a">Local</text>
    <text x="90"  y="185" text-anchor="middle" font-size="11" fill="#6b6b6b">내 컴퓨터에서 실행</text>
    <text x="270" y="130" text-anchor="middle" font-weight="700" fill="#4a4a4a">Development</text>
    <text x="270" y="150" text-anchor="middle" font-size="11" fill="#6b6b6b">팀 공용 개발 환경</text>
    <text x="450" y="95"  text-anchor="middle" font-weight="700" fill="#4a4a4a">Staging</text>
    <text x="450" y="115" text-anchor="middle" font-size="11" fill="#6b6b6b">운영과 비슷한 리허설</text>
    <text x="630" y="60"  text-anchor="middle" font-weight="700" fill="#4a4a4a">Production</text>
    <text x="630" y="80"  text-anchor="middle" font-size="11" fill="#6b6b6b">실제 사용자·시스템</text>
    <text x="360" y="20" text-anchor="middle" font-size="11" fill="#8a8a8a">한 칸 올라갈 때마다 "승격 기준(promotion criteria)"을 통과해야 해요</text>
  </g>
</svg>
</div>

| 환경 | 보통 증명하는 것 | 이것만으로는 증명 못 하는 것 |
|---|---|---|
| Local | 개발자가 통제된 환경에서 실행·확인 가능 | 공유 접근, 운영 설정, 모니터링, 규모, 지원, 안전한 배포 |
| Development | 팀 단위 공유 환경에서 실행 가능 | 운영 데이터 경계, 릴리스 통제, 운영 담당 |
| Staging | 운영과 비슷한 환경에서 검증 | 운영 승인, 지원 준비, 롤백 준비, 파일럿 안전성 |
| Production | 실제 사용자·시스템에 노출 | 여전히 지속적 모니터링·지원·사고 대응 필요 |

<div class="oa-eli">
  <span class="oa-eli__head">⚠️ 가장 흔한 착각</span>
  <p>"Staging이랑 Production은 거의 같잖아요." → 실제로는 sanitized 데이터, 작은 검색 인덱스, 적은 트래픽, 다른 자격 증명·네트워크 권한일 수 있어요. 이 <b>차이를 출시 전에 보이게</b> 만드는 게 핵심이에요.</p>
</div>

환경 가정에서 기록할 것:

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__title">환경·노출 범위</span>어떤 환경이 범위인지, 내부 전용인지 외부 접근인지</div>
  <div class="oa-card"><span class="oa-card__title">런타임·호스팅</span>어디서 실행이 승인됐나</div>
  <div class="oa-card"><span class="oa-card__title">데이터·접근 경계</span>환경별 데이터, 접근 주체, 운영/비운영 데이터 분리</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__title">OpenAI 프로젝트 분리</span>staging·production을 별도 OpenAI 프로젝트로 → 사용량 분리, 접근 제한, 프로젝트별 rate·spend 한도</div>
  <div class="oa-card"><span class="oa-card__title">테스트 데이터</span>test·synthetic·redacted·운영 유사 데이터 중 무엇?</div>
  <div class="oa-card"><span class="oa-card__title">승격 기준</span>다음 환경으로 가려면 무엇이 참이어야 하나</div>
</div>

**누가 워크플로우를 돌리는지**도 적어요. 애플리케이션이 호출을 조율하고 미완료 작업을 추적하는 방식인지, Agents API managed session처럼 OpenAI가 세션 맥락을 관리하는 방식인지. (강의 기준: Agents API는 현재 US 데이터 레지던시만 지원하고 ZDR 대상이 아니라서, 데이터 요건과 충돌하면 출시 테스트 전에 제외해야 한다고 설명해요. 최신 문서로 재확인 필요.)

### 설정(Configuration) — 숨은 로컬 설정에 기대지 않기

| 설정 영역 | 예시 |
|---|---|
| 환경 변수 | 서비스 URL, feature flag 등 환경마다 바뀌는 **비밀 아닌** 값 |
| Secret 분리 | API 키·토큰·인증서는 승인된 secret 관리 프로세스로 (코드·프롬프트·스크린샷 ✗) |
| 의존성 | 라이브러리, 검색 저장소, 도구, 다운스트림 시스템 |
| 런타임 | timeout, 재시도 가능한 에러 종류, 최대 재시도 횟수, **지수 백오프 + jitter**, 최대 요청 크기, 동시성 |
| Rate·quota·spend | 예상 트래픽, 요청·토큰 한도, 프로젝트 rate·spend 한도, 알림 임계값, 한도 근접 시 동작 |
| 버전 가정 | 서비스·프롬프트·스키마·검색 설정·도구 정의 버전 |

<div class="oa-eli">
  <span class="oa-eli__head">⏳ 비동기 작업 (강의 기준 내용)</span>
  <p><b>Async tool calling</b>은 앱이 함수를 실행하는 동안 모델이 다른 일을 계속하게 하고, <b>Background mode</b>는 응답 생성 자체를 비동기로 돌려요. 어느 쪽이든 <b>앱의 job ID ↔ 원래 call_id</b> 매핑과 작업 상태의 담당자를 정해야 해요. 대화가 다음으로 넘어가도 결과는 원래 call_id로 돌려줘야 하거든요.</p>
  <p>Mid-turn steering(응답 도중 방향 수정)은 강의 시점 기준 지원되는 GPT-6 모델 + Responses API WebSocket 연결이 필요하다고 해요. ZDR·Private Safety Processing(PSP)을 쓰면 보관 위치·키 접근·기록 수명 담당자까지 기록합니다.</p>
</div>

### 릴리스 경로 — "곧 운영 배포"는 계획이 아니다

<ol class="oa-flow">
  <li><strong>무엇을</strong>릴리스 대상 + version ID</li>
  <li><strong>어디서 어디로</strong>staging → pilot 등</li>
  <li><strong>근거</strong>smoke test·실패 테스트 증거</li>
  <li><strong>승인</strong>approval owner</li>
  <li><strong>되돌리기</strong>rollback owner·경로</li>
</ol>

| 역할 | 하는 일 |
|---|---|
| Release owner | 릴리스와 릴리스 증거를 조율 |
| Approval owner | 이동을 승인 (필요한 증거를 받아서) |
| Rollback owner | 필요 시 known-good 버전으로 되돌림 |
| Release notes | 무엇이 바뀌었고, 무엇을 지켜봐야 하나 |

정상 경로뿐 아니라 **막힌 요청·스트리밍 중 에러** 처리도 테스트해요. 강의는 특히 다음을 강조합니다.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 착각하기 쉬운 신호</span>
    <ul>
      <li><code>response.steer.accepted</code> = 수정이 <b>적용됨</b></li>
      <li>웹사이트 접근 승인 = 그 사이트에서 기록 변경 전 확인도 보장</li>
      <li>BAA 활성화 = 앱이 의료 규정 준수</li>
      <li>워터마크 = 내용이 사실임</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 실제 의미</span>
    <ul>
      <li>큐에 <b>들어갔을 뿐</b> → 결과·continuation을 직접 확인</li>
      <li>방문만 허용 → 확인이 필요하면 권한 제한이나 강제 런타임 필요</li>
      <li>계약 확인 + 서비스·설정·승인을 따로 검증</li>
      <li>OpenAI 출처 신호일 뿐 → 일반 릴리스 리뷰는 그대로</li>
    </ul>
  </div>
</div>

### 예시: 고객 통화 준비 요약 API

로컬·staging 테스트 통과 → 고객이 "제한된 파일럿 그룹에 열어 주세요". 이때 바로 "파일럿 준비 완료"로 가지 않아요.

| 영역 | 상태 |
|---|---|
| 배포 맥락·설정 | ✅ 파일럿 사용자만 접근, 승인된 시스템으로 데이터 접근, secret 관리, rate 가정 문서화 |
| 검증·릴리스 증거 | ✅ health, smoke test, 권한 없는 계정 에러 테스트, release ID, release notes |
| 열린 blocker | ⛔ 확대 승인자 미정 · 롤백 담당자는 있지만 **롤백 미테스트** · 파일럿 비용 가시성 미확인 |
| **추천** | **파일럿 결정 보류** — 롤백 준비와 비용 가시성 검증 후 진행. 담당자 이름만으론 갭이 닫히지 않음 |

## 2. 관측성 — 출시 후 무엇이 보여야 하나

<div class="oa-eli">
  <span class="oa-eli__head">🚗 쉽게 말하면</span>
  <p>관측성은 <b>자동차 계기판</b>이에요. 속도계(지연), 연료계(비용·쿼터), 엔진 경고등(에러), 주행거리(사용량)가 있어야 "계속 달릴지, 주유소에 들를지, 정비소에 갈지" 정할 수 있어요. 로그만 잔뜩 쌓는 건 블랙박스 영상만 있고 계기판은 없는 차와 같아요.</p>
</div>

| 신호 | 보여주는 것 | 연결되는 결정 |
|---|---|---|
| **Health·가용성** | 필요할 때 응답하는가 | 검증 계속, 장애 조사, 출시 차단 |
| **지연·의존성 타이밍** | 느리다면 API·모델·검색·도구 중 어디? | 원인 조사, graceful degradation, 롤백 |
| **사용량·처리량·쿼터 압력** | 트래픽, rate limit 근접 여부 | 롤아웃 조정, 쿼터 검토, throttling·queue |
| **비용·소비** | 예상 대비 소비, 모델·패턴 적절성 | 사용 상한, 조건부 출시, 모델 재검토 |
| **검색·의존성 신뢰성** | 소스·도구·다운스트림 실패/권한 차단 | 소스 보완, fallback, 확대 중지 |
| **에러·릴리스 추적성** | 릴리스 후 에러 증가? 어느 버전이? | 조사, 롤백, 감사 기록 |
| **미완료 작업·안전 신호** | pending job, steering 결과, 모니터링 차단 | 미완료 조사, dispatch 중지 후 검토 |

### 비용은 "총액"이 아니라 "성공 1건당 비용"으로

<div class="oa-bars">
  <div class="oa-bar"><span>총 지출</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:100%"></span><span class="oa-bar__val">실패·재시도 포함 전부</span></span></div>
  <div class="oa-bar"><span>품질 기준 통과 작업</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:62%"></span><span class="oa-bar__val">이것만 '성공'으로 셈</span></span></div>
  <div class="oa-bar"><span>실패·재시도</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:28%"></span><span class="oa-bar__val">비용엔 포함, 성공엔 제외</span></span></div>
  <div class="oa-bar"><span>진행 중(pending)</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:10%"></span><span class="oa-bar__val">따로 집계</span></span></div>
  <p class="oa-bars__caption">cost per successful task = 총 지출 ÷ 품질 기준을 통과한 작업 수. 막대 비율은 개념적 예시예요. 성공이 0건이면 "건당 비용" 대신 실패와 지출을 그대로 보고합니다.</p>
</div>

비용이 늘면 모델부터 바꾸지 말고 **릴리스 변경·추가 호출·더 길어진 출력**을 먼저 조사한 뒤, 같은 품질 기준으로 재테스트해요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📨</span><span class="oa-card__title">Spend alert</span>담당자에게 <b>알림만</b> — 트래픽은 안 멈춤</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🛑</span><span class="oa-card__title">Spend limit (강제)</span>요청을 <b>끊을 수 있음</b> — 단, 즉시 적용은 아님</div>
  <div class="oa-card"><span class="oa-card__icon">📏</span><span class="oa-card__title">Rate-limit 헤더</span><code>x-ratelimit-*</code>로 남은 요청·토큰과 리셋 시간 확인 (헤더 구성은 최신 문서로 검증)</div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🏎️ 용량 vs 속도 (강의 기준 명칭)</span>
  <p>Build·Launch·Grow는 <b>사용 티어(얼마나 많이 처리할 수 있나)</b>, Standard·Fast·Ultrafast는 <b>처리 방식(얼마나 빨리·얼마에)</b>이에요. 차선 수와 차의 속도가 다른 것처럼, 응답이 빨라져도 트래픽은 여전히 한도에 걸릴 수 있어요. 처리 방식을 비교할 땐 모델·프롬프트·입력·수용 기준·부하를 고정하고, <b>토큰 속도가 아니라 "수용된 작업까지 걸린 시간"</b>을 재요.</p>
</div>

### 로그와 Trace

<div class="oa-eli">
  <span class="oa-eli__head">📦 쉽게 말하면</span>
  <p><b>로그</b>는 택배의 "OO 터미널 도착" 같은 개별 기록, <b>trace</b>는 그 기록들을 운송장 번호 하나로 이어 <b>택배가 어디서 늦어졌는지</b> 보여주는 배송 추적이에요.</p>
</div>

<ol class="oa-flow">
  <li><strong>API 서비스</strong>request_id</li>
  <li><strong>모델 호출</strong>x-request-id</li>
  <li><strong>검색 호출</strong>retrieval_status</li>
  <li><strong>도구 호출</strong>call_id</li>
  <li><strong>다운스트림</strong>dependency_status</li>
</ol>

준비도 수준의 로그는 **안전한 운영 메타데이터**만 남겨요 (강의 예시를 축약):

```json
{
  "request_id": "req_8f31",
  "environment": "pilot",
  "release_version": "account-support-api-pilot-r3",
  "status_code": 200,
  "latency_ms": 842,
  "model_call_status": "success",
  "retrieval_status": "success",
  "rate_limit_remaining_requests": 42,
  "sensitive_content_logged": false
}
```

- `200` + `model_call_status: success`가 **요약 품질·검토 통과를 뜻하진 않아요.** 그건 작업 기록에 따로.
- OpenAI가 만든 **`x-request-id`**와 앱이 보내는 **`X-Client-Request-Id`**를 구분해 둘 다 기록하면, 응답 헤더가 안 온 timeout도 추적할 수 있어요.
- 앱 job ID·상태 ↔ `call_id` ↔ response ID ↔ request/release ID를 연결해 **작업 전체**를 추적해요.
- Agents API 사용량은 늦게 오거나 알 수 없을 수 있어요(강의 기준). **사용량이 비어 있다 ≠ 비용 0**.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 로그에 남길 것</span>
    <ul>
      <li>request/release ID, status, 에러 범주</li>
      <li>지연, 의존성별 성공 여부</li>
      <li>rate-limit 잔여량</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 로그에 남기지 말 것</span>
    <ul>
      <li>고객 기밀·개인·규제 데이터</li>
      <li>토큰·자격 증명</li>
      <li>내부 시스템 프롬프트, 민감한 도구 원본 출력, 전체 원문 문서</li>
    </ul>
  </div>
</div>

**로그는 많을수록 좋은 게 아니에요.** 소음·비용·개인정보 위험·검토 부담이 늘어요.

### 알림과 사고 대응 경로

<ol class="oa-flow">
  <li><strong>신호 발생</strong>트리거·우선순위</li>
  <li><strong>1차 대응</strong>누가 먼저 받나</li>
  <li><strong>에스컬레이션</strong>사용자·비용·안전 영향 시</li>
  <li><strong>증거 검토</strong>무엇을 보고 판단하나</li>
  <li><strong>결정</strong>계속·중지·보완·롤백·조건부</li>
</ol>

| 알림 종류 | 트리거 예시 |
|---|---|
| 가용성 | health check 실패, 접근 불가 |
| 지연 | 합의한 응답 시간 초과 |
| 에러율 | 기대 범위 이상 급증 |
| 쿼터 | 한도에 근접 |
| 비용·소비 | 예상 범위 초과 |
| 의존성 | 검색·도구·모델·다운스트림 실패 |
| 안전·품질 | 형식 오류, 낮은 confidence, 반복 에스컬레이션 |

**담당자가 없는 알림은 만들지 마세요.** 주기적 리뷰로 충분한 신호까지 알림으로 만들면 정말 중요한 걸 놓쳐요. (강의는 MCP Events·ChatGPT Site 예약 작업도 다루는데, 요점은 같아요: **"전달 완료" ≠ "작업 완료"**, 백그라운드 작업은 방문자가 승인한 연결을 재사용할 수 없으니 실행 시점에 쓸 수 있는 데이터 소스와 권한을 따로 정해야 해요.)

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 관측성 계획</span>
    <ul><li>"API를 모니터링하겠습니다."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 관측성 계획 (리테일 지원 API 파일럿)</span>
    <ul>
      <li>health·업무시간 uptime·릴리스별 에러율</li>
      <li>그룹별 요청 수, OpenAI 프로젝트별 사용량·비용 vs spend 한도</li>
      <li>전체/모델/검색 지연, 고 percentile 알림</li>
      <li>request ID·release ID 기록, 원문 고객 메시지 미기록</li>
      <li>지원팀 1차 → 엔지니어링 → 에스컬레이션 담당자가 결정</li>
    </ul>
  </div>
</div>

## 3. 실패 처리와 지원

프로덕션 준비된 API는 **절대 실패하지 않는 API가 아니라**, 중요한 실패를 예상하고 · 보이게 하고 · 복구할 수 있고 · 담당자가 있는 API예요.

### 실패 모드 8가지

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__title">🔑 인증 실패</span>신원 증명 불가</div>
  <div class="oa-card"><span class="oa-card__title">🚪 인가 실패</span>인증은 됐지만 접근 권한 없음</div>
  <div class="oa-card"><span class="oa-card__title">⏱️ Timeout</span>API나 의존성이 너무 느림</div>
  <div class="oa-card"><span class="oa-card__title">🔗 의존성 실패</span>모델·도구·검색·DB·다운스트림</div>
  <div class="oa-card"><span class="oa-card__title">📊 Rate limit·쿼터</span>한도 근접/초과</div>
  <div class="oa-card"><span class="oa-card__title">📝 잘못된 입력</span>필수 필드 누락, 깨진 데이터</div>
  <div class="oa-card"><span class="oa-card__title">🤖 예상 밖 모델 출력</span>형식 오류, 낮은 confidence, 구조 불일치</div>
  <div class="oa-card"><span class="oa-card__title">📚 검색·맥락 실패</span>오래되거나 없거나 접근 불가</div>
</div>

### 대응 행동 — 무시와 과잉 반응 사이

<div class="oa-bars">
  <div class="oa-bar"><span>Retry</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:15%"></span><span class="oa-bar__val">가장 가벼움</span></span></div>
  <div class="oa-bar"><span>Fallback</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:30%"></span></span></div>
  <div class="oa-bar"><span>Graceful degradation</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:42%"></span></span></div>
  <div class="oa-bar"><span>Escalation</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:55%"></span></span></div>
  <div class="oa-bar"><span>Pause rollout</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:72%"></span></span></div>
  <div class="oa-bar"><span>Rollback</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:90%"></span><span class="oa-bar__val">가장 무거움</span></span></div>
  <p class="oa-bars__caption">대응의 "무게"를 개념적으로 나타낸 막대예요. 모든 문제에 롤백(가장 무거운 대응)만 쓰는 것도, 아무것도 안 하는 것도 위험해요.</p>
</div>

| 대응 | 언제 / 어떻게 |
|---|---|
| **Retry** | rate limit·일시적 네트워크 오류 같은 **일시적** 실패만. **지수 백오프 + 랜덤 jitter**, 최대 횟수 제한. 실패한 요청도 분당 한도에 포함되니 무한·즉시 재시도는 상황을 악화 |
| **Fallback** | 더 단순한 응답, 캐시된 안전한 결과, 표준 지원 인계 |
| **Graceful degradation** | 조용히 실패하거나 위험하게 행동하는 대신 제한된 안전 응답 |
| **Escalation** | 맞는 담당자에게 검토·결정 요청 |
| **Rollback** | known-good 버전·설정으로 복귀 |
| **Pause rollout** | 조사하는 동안 확대 중지 |
| **사용자 커뮤니케이션** | 영향받은 사용자·이해관계자에게 알림 |

<div class="oa-eli">
  <span class="oa-eli__head">⏰ 지수 백오프 + jitter, 쉽게 말하면</span>
  <p>사람 많은 매장 문 앞에서 "1초 뒤, 2초 뒤, 4초 뒤…"처럼 <b>기다리는 시간을 점점 늘리고</b>, 모두가 동시에 다시 들이닥치지 않게 <b>각자 약간씩 랜덤하게</b> 늦추는 거예요. 그리고 몇 번 해도 안 되면 포기해요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 검색 소스가 죽었을 때 위험한 응답</span>
    <ul><li>근거 없이 그럴듯한 답을 지어냄 → 신뢰·정확성·규정 위험</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ Graceful degradation</span>
    <ul><li>"지금은 승인된 자료로 답할 수 없어요. 잠시 후 다시 시도하거나 지원팀으로 연결해 주세요."</li></ul>
  </div>
</div>

### 모니터링 차단은 별도 경로 (강의 기준 내용)

강의는 misalignment monitoring을 **추가 신호**로 설명해요 — 모더레이션도, 승인도, 위반의 증거도 아니고, 정상 작업을 잡거나 문제를 놓칠 수 있어요.

| 상황 | 해야 할 일 |
|---|---|
| `error.code = misalignment_policy_violation` (스트리밍 전 HTTP 403 또는 스트리밍 중 에러) | 해당 대화의 **추가 행동 dispatch 중지**, **자동 재시도 금지**, 지정된 대응 담당자에게 전달 |
| `request_paused: true` 알림 | 차단이 등록됐다는 뜻일 뿐 — 외부 job이 멈췄거나 이전 행동이 취소됐다는 뜻 아님 |
| 이미 실행된 도구 효과 | 앱 job 기록·완료된 도구 효과를 **직접 확인**한 뒤 복구 결정 (릴리스 롤백 ≠ 도구 행동 되돌리기) |
| Managed session 실패 | 요청·턴·세션·실행 환경을 구분, 현재 상태와 저장된 작업 확인. 재연결이 작업이나 승인을 자동 재전송하면 안 됨 |

### 지원 담당(Ownership) — 신호가 결정으로 이어지게

| 역할 | 책임 |
|---|---|
| Service owner | API 서비스 전체 책임 |
| First-response route | 알림을 처음 받고 대응 |
| Escalation owner | 위험이 커질 때 결정·조율 |
| Release / Rollback owner | 릴리스 조율 / 롤백 개시·승인 |
| Documentation owner | 런북·릴리스 노트·지원 가이드 유지 |
| Issue-review cadence | 이슈·추세를 얼마나 자주 검토하나 |

담당은 **증거와 연결**돼야 해요: "지연이 늘면 누가 지연 증거를 보나?", "비용이 파일럿 범위를 넘으면 누가 사용을 제한하나?", "최신 릴리스가 실패를 만들면 누가 롤백을 결정하나?"

### 예시: 현장 정비 API의 실시간 장애

기술자들이 "응답이 느려요" → 같은 시기에 검색 호출이 간헐 실패 + 사용량이 쿼터에 근접.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 대응</span>
    <ul><li>"가끔은 응답하니까 계속 가자."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 대응</span>
    <ul>
      <li>즉시: 검색 실패 시 graceful degradation + 확대 중지</li>
      <li>증거: release ID, 지연 추세, 검색 실패율, 쿼터 신호, 영향 그룹, 에러 범주·trace</li>
      <li>담당: 현장 지원(사용자 영향) · 엔지니어링(검색·지연) · 에스컬레이션 담당(계속/롤백/보완 결정)</li>
      <li>릴리스가 원인이거나 영향이 크면 → 그때 롤백</li>
    </ul>
  </div>
</div>

## 4. API Production Readiness Checklist

체크리스트는 기술 감사가 아니라 **결정 기록**이에요. 핵심 질문은 하나: **"이 API가 안전하게 다음 단계로 갈 수 있나, 어떤 조건에서?"**

<ol class="oa-flow">
  <li><strong>무엇을 어디에?</strong>워크플로우·환경·설정·데이터·한도</li>
  <li><strong>출시 근거는?</strong>health·smoke·실패 테스트·release ID·담당자</li>
  <li><strong>출시 후 어떻게 보나?</strong>신호·로그·ID·알림·로그 금지 데이터</li>
  <li><strong>실패하면?</strong>실패 모드·재시도·degradation·대응 경로·롤백</li>
  <li><strong>추천은?</strong>결정·이유·blocker·다음 담당자</li>
</ol>

### 다섯 가지 출시 추천

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🟢</span><span class="oa-card__title">Ready</span>필요한 증거가 있고 blocking 갭 없음</div>
  <div class="oa-card"><span class="oa-card__icon">🟡</span><span class="oa-card__title">Ready with conditions</span>제한된·통제된 방식으로, 특정 조건 충족 시</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🟠</span><span class="oa-card__title">Remediate before release</span>노출 전에 고쳐야 할 blocking 갭</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔴</span><span class="oa-card__title">Pause or escalate</span>리뷰 수준을 넘는 위험 → 플랫폼·보안·거버넌스·고객·OpenAI 전문가 결정</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⏪</span><span class="oa-card__title">Rollback or pause expansion</span>출시 후 증거가 중지·복귀를 가리킴</div>
</div>

각 추천에는 **이유 · 가장 큰 blocker/조건 · 다음 행동 담당자 · 결정을 바꿀 증거**를 함께 적어요. 좋은 체크리스트는 짧고, 구체적이고, 결정 가능한(decision-ready) 문서예요. 아무 일도 안 일어난다고 증명하는 게 아니라, 팀이 배포·모니터링·지원·개선·중지·롤백을 **책임 있게 할 수 있을 만큼 준비 상태를 보이게** 하는 게 목표예요.

<div class="oa-eli">
  <span class="oa-eli__head">💡 증거로 못 메우는 빈칸</span>
  <p>더 빠른 처리 모드, 더 높은 용량 티어, BAA 활성화, 워터마크는 <b>담당자·롤백 경로·spend 통제 같은 빠진 증거를 대신하지 못해요.</b> 가상 예시로 받은 증거와 직접 관찰한 테스트도 구분해서 적어요.</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. staging에선 잘 되지만 release ID 미기록, 운영 데이터 경계 미확인, 승격 승인자 미정. 제한된 파일럿으로 옮기려면?</summary>
  <p><b>정답:</b> 파일럿 노출 전에 <b>빠진 릴리스·환경·승인 증거를 보완</b>한다. (staging 성공 = 운영 준비 ✗ / UI 리뷰만 하면 OK ✗ / 모델만 쓰는 단순 워크플로우로 교체 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 관측성 항목과 답하는 질문</summary>
  <p>사용량 신호 → 얼마나 쓰이고, 파일럿·운영 범위를 넘어 늘고 있나?<br>사고 대응 경로 → 누가 대응·에스컬레이션하고 계속/중지/보완/롤백을 어떻게 정하나?<br>비용·소비 신호 → 예상 지출·자원 범위 안에서 돌고 있나?<br>Release/version ID → 어떤 버전·설정이 이 동작을 만들었나?</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 정책 문서 기반 API가 접근은 되지만 검색이 간헐 실패하고, 그때 근거 없이 답한다. 최선의 대응은?</summary>
  <p><b>정답:</b> <b>graceful degradation + 확대 중지 + 검색 실패 보완</b>. (접근되니까 계속 ✗ / 불만 나올 때까지 무시 ✗ / 로그 전부 삭제 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 200명에게 출시하려는데 health check와 smoke test 1개뿐. release ID·사용량/비용 가시성·사고 대응 경로·롤백 경로가 없다. 추천은?</summary>
  <p><b>정답:</b> <b>Remediate before release</b> — 여러 blocking 준비 신호가 빠져 있다. (smoke test 통과로 Ready ✗ / health check만으로 조건부 ✗ / 아직 운영 전이라 rollback 추천은 맞지 않음)</p>
</details>

## 정리

- **로컬 성공 ≠ 프로덕션 준비.** 환경마다 증명하는 범위가 다르고, staging과 production의 차이를 드러내야 한다.
- 설정·secret·rate/spend 한도·버전 가정을 명시하고, 릴리스는 **무엇을·근거·승인자·롤백 담당자**로 정의한다.
- 관측성 = 계기판: health·지연·사용량·**성공 1건당 비용**·쿼터·로그·trace·release ID·알림, 그리고 **로그에 남기지 않을 것**.
- 실패 대응은 무시와 과잉 반응 사이에서: 재시도(백오프+jitter) → fallback → graceful degradation → 에스컬레이션 → 중지 → 롤백.
- 체크리스트는 5개 질문에 답하는 **결정 기록**이고, 끝은 Ready / 조건부 / 보완 / 중지·에스컬레이션 / 롤백 중 하나의 추천이다.
- 모델·기능 이름(GPT-6, Agents API, Ultrafast, PSP 등)은 **강의 시점 기준**이므로 실제 적용 전 최신 문서로 확인한다.
