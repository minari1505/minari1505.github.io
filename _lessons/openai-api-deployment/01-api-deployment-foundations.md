---
title: "API Deployment Foundations: Run, Build, and Verify a Minimal API"
title_ko: "API 배포 기초 — 최소 API를 실행·구축·검증하기"
course: openai-api-deployment
lesson: 1
---

## 학습 목표

- 문서에 적힌 사실과 **안전한 인증 placeholder**만으로 기존 API를 호출하고 검증하기
- health·비즈니스 엔드포인트, 입력 검증, 에러 응답을 갖춘 **최소 로컬 API** 만들기
- 성공 테스트와 **일부러 실패시키는 테스트**가 각각 무엇을 증명하는지 설명하기
- SecOps·DevOps·문서화 관점으로 "로컬에서 됨"과 "기업 배포 준비 완료"를 구분하기
- 결과를 **API Foundation Evidence Pack**(증거 묶음)으로 정리하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>기존 API 안전 호출</strong>문서 사실 + placeholder로 첫 호출</li>
  <li><strong>최소 API 만들기</strong>/health + 비즈니스 엔드포인트 1개</li>
  <li><strong>성공·실패 검증</strong>200도, 400도 증거로 남기기</li>
  <li><strong>준비도 점검</strong>SecOps · DevOps · 문서</li>
  <li><strong>Evidence Pack</strong>"증명한 것 / 못한 것" 정리</li>
</ol>

이 코스 전체를 관통하는 한 문장: **"된다고 생각해"가 아니라 "이렇게 테스트했고, 이런 결과가 나왔고, 이건 아직 증명 안 됐어"라고 말하기.**

## API 호출은 식당 주문과 같아요

<div class="oa-eli">
  <span class="oa-eli__head">🍜 쉽게 말하면</span>
  <p>API는 <b>식당 주문 창구</b>예요. 손님(client)이 주문서(request)를 내면, 주방(API service)이 음식(response)을 내줘요. 그리고 <b>영수증(evidence)</b>을 챙겨야 "내가 뭘 시켰고 뭐가 나왔는지" 나중에 증명할 수 있죠.</p>
  <p>재료가 빠진 주문서를 냈을 때 주방이 "재료가 빠졌어요"라고 <b>정중하게 돌려보내는지</b> 확인하는 것도 중요해요. 그게 바로 에러 테스트예요.</p>
</div>

| 구성 요소 | 식당 비유 | 실제 예 |
|---|---|---|
| **Client** (호출하는 쪽) | 손님 | 스크립트, SDK, curl, 앱 |
| **Request** (요청) | 주문서 | method, endpoint, header, 인증 방식, body |
| **API service** | 주방 | 요청을 받아 처리하는 서버 |
| **Response** (응답) | 나온 음식 | status code + body |
| **Verification evidence** | 영수증 | 무엇을 보냈고 무엇이 왔는지 기록 |

핵심 습관은 **응답을 반드시 들여다보는 것**이에요. 성공도 증거, 실패도 증거입니다.

## 1단계 — 기존 API를 안전하게 호출하기

### 문서화된 사실 vs 가정

호출 전에 **문서에 적힌 것**과 **그럴 것 같은 것**을 분리해요. 추측을 증거처럼 쓰면 안 되니까요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 문서화된 사실 (documented fact)</span>
    <ul>
      <li>Endpoint: <code>POST /classify-ticket</code></li>
      <li>필수 header: <code>Authorization</code></li>
      <li>필수 body 필드: <code>ticket_text</code></li>
      <li>성공 응답: <code>category</code>, <code>summary</code></li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❓ 확인이 필요한 가정 (assumption)</span>
    <ul>
      <li>빈 문자열도 받아줄까?</li>
      <li>응답에 <code>confidence</code>가 항상 있을까?</li>
      <li>Bearer 토큰? OAuth? 서명 요청?</li>
      <li>필드 누락 시 정말 400일까?</li>
    </ul>
  </div>
</div>

리뷰할 때는 최소한 **사실 1개 · 필요한 설정 1개 · 확인할 가정 1개**를 적어 두세요.

### 인증(Authentication) vs 인가(Authorization)

<div class="oa-eli">
  <span class="oa-eli__head">🎢 쉽게 말하면</span>
  <p><b>인증</b>은 놀이공원 입구에서 "당신 누구예요?" 하고 입장권을 확인하는 것, <b>인가</b>는 안에서 "이 놀이기구 탈 수 있는 등급이에요?"를 확인하는 거예요. 입장했다고 모든 기구를 탈 수 있는 건 아니죠.</p>
</div>

API 키·토큰·비밀번호 같은 **secret은 코드에 직접 쓰지 않습니다.** 연습용 mock API여도 실제 고객 환경에서 지킬 습관을 똑같이 연습해요.

```text
Authorization: Bearer <SUPPORT_API_TOKEN>     # placeholder
SUPPORT_API_TOKEN=<placeholder-token>         # 환경 변수
```

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🚫</span><span class="oa-card__title">하드코딩된 키</span>복사·로그·커밋으로 새어 나가요. 기업 환경에선 <b>배포 차단 사유</b>예요.</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🔐</span><span class="oa-card__title">placeholder / 환경 변수</span>문법보다 <b>패턴</b>이 중요: 비밀값이 소스 파일 밖에 있어야 해요.</div>
  <div class="oa-card"><span class="oa-card__icon">🤖</span><span class="oa-card__title">AI로 코드 생성 시</span>"문서에 없는 걸 지어내지 마, 진짜 키 쓰지 마"라고 지시하고, <b>실행 전 꼭 검사</b>.</div>
</div>

### 첫 호출 검증 — 성공 테스트 + 일부러 실패 테스트

| 확인 항목 | 성공 테스트 | 의도적 에러 테스트 (필수 필드 빼기) |
|---|---|---|
| 문서와 같은 endpoint·method? | ✅ | ✅ |
| status code | 성공 범위(200대) | 통제된 실패(400 등) |
| 응답 body | 기대 필드 포함 | 이해 가능한 에러 메시지 |
| 민감 정보 노출 | 없어야 함 | 내부 구현 정보도 없어야 함 |
| 개발자가 원인 파악 가능? | — | 충분한 정보 제공 |

### 증거에는 꼭 라벨을 붙여요

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">▶️</span><span class="oa-card__title">executed</span>내가 직접 실행했고, 실제 status·body를 복사해 둠</div>
  <div class="oa-card"><span class="oa-card__icon">👀</span><span class="oa-card__title">inspected only</span>코드·요청을 읽어보기만 함. 실제 결과 칸은 <b>비워 둠</b></div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⏳</span><span class="oa-card__title">not yet tested</span>아직 안 해봄. "검증됨"이라고 쓰면 안 됨</div>
</div>

> 스펙에 적힌 "예상 응답"이나 AI가 만들어 준 응답은 **실행 증거가 아닙니다.** "API verified"라고 쓰려면 실제 실행 기록이 있어야 해요.

## 2단계 — 가장 작은 쓸모 있는 API 만들기

예시 상황: 고객 지원팀이 "들어오는 티켓 문장을 자동 분류할 수 있을까?"를 실제 헬프데스크에 연결하기 **전에** 시험해 보고 싶어 해요. 목표는 분류 성능이 아니라 **통제된 로컬 루프를 증명**하는 것.

| 구성 | 왜 필요한가 |
|---|---|
| `GET /health` | 서버가 살아 있다는 증명 |
| `POST /triage-ticket` (비즈니스 엔드포인트 1개) | 의미 있는 요청을 받아 의미 있는 응답을 준다는 증명 |
| JSON 요청/응답 | 다른 사람이 쉽게 테스트 가능 |
| 입력 검증 | 빠진 값·빈 값·숫자 같은 잘못된 타입 거르기 |
| 통제된 에러 응답 | 조용히 실패하지 않고 400 + `detail`로 알려주기 |

강의의 예제는 FastAPI로 작성돼 있어요. 핵심만 추리면 이런 모양입니다.

```python
from fastapi import FastAPI, HTTPException, Request

app = FastAPI(title="Support Triage Helper")

@app.get("/health")
def health():
    return {"status": "ok"}

@app.post("/triage-ticket")
async def triage_ticket(request: Request):
    body = await request.json()
    text = body.get("ticket_text", "")
    if not isinstance(text, str):          # null·숫자를 "None"/"123"으로 바꾸지 않기
        raise HTTPException(400, "ticket_text must be a string")
    text = text.strip()
    if not text:                            # 누락·빈 문자열·공백만 있는 경우
        raise HTTPException(400, "ticket_text is required")
    category = ("damaged_item" if "damaged" in text.lower()
                else "refund_request" if "refund" in text.lower()
                else "general_support")      # 진짜 분류기가 아닌 placeholder 로직
    return {"category": category, "summary": text[:140]}
```

```bash
pip install fastapi uvicorn
uvicorn main:app --reload --port 8000     # 이 터미널은 켜 둔 채로
curl -i http://localhost:8000/health
curl -i -X POST http://localhost:8000/triage-ticket \
  -H "Content-Type: application/json" -d '{}'
```

<div class="oa-eli">
  <span class="oa-eli__head">🧪 주의할 점</span>
  <p>타입 검사를 <b>strip() 전에</b> 하는 게 포인트예요. 순서가 반대면 <code>null</code>이 문자열 "None"으로 바뀌어 통과해 버릴 수 있어요. 또 이 예제는 깨진 JSON이나 객체가 아닌 body는 따로 처리하지 않으니, 더 넓게 쓰기 전에 보완이 필요해요.</p>
</div>

### 테스트 매트릭스 — 각 테스트가 증명하는 것

| 테스트 | 요청 | 기대 결과 | 증명하는 것 |
|---|---|---|---|
| Health check | `GET /health` | 200 `{"status":"ok"}` | 로컬 서버가 실행 중이다 |
| 비즈니스 성공 | `POST` + 정상 `ticket_text` | 200 `category`, `summary` | 기대 입력을 받고 기대 형태로 응답한다 |
| 필드 누락 | `POST {}` | 400 `ticket_text is required` | 검증 경로가 있다(조용히 실패 안 함) |
| 잘못된 타입 | `{"ticket_text": null}`, 숫자 | 400 `must be a string` | 타입 규칙을 지킨다 |
| 요청/응답 기록 | — | 캡처된 contract | 다른 사람이 기대값을 검토할 수 있다 |

## 3단계 — "로컬에서 됨" ≠ "배포 준비 완료"

로컬 API가 잘 돌아도 증명한 범위는 생각보다 좁아요.

<div class="oa-bars">
  <div class="oa-bar"><span>요청/응답 형태</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:95%"></span><span class="oa-bar__val">로컬 테스트로 증명됨</span></span></div>
  <div class="oa-bar"><span>입력 검증 경로</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:80%"></span><span class="oa-bar__val">일부 증명 (malformed JSON 제외)</span></span></div>
  <div class="oa-bar"><span>보안·접근 제어</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:10%"></span><span class="oa-bar__val">미증명</span></span></div>
  <div class="oa-bar"><span>모니터링·롤백</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:8%"></span><span class="oa-bar__val">미증명</span></span></div>
  <div class="oa-bar"><span>분류 품질·확장성</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:5%"></span><span class="oa-bar__val">미증명</span></span></div>
  <p class="oa-bars__caption">막대 길이는 이해를 돕기 위한 개념적 표현이에요 (강의 수치 아님).</p>
</div>

그래서 세 가지 렌즈로 남은 숙제를 **이름 붙여** 둡니다.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">SecOps (보안 운영)</span>"이 API가 민감한 걸 받거나, 저장하거나, 돌려주거나, 로그에 남길 수 있나?" — secret, 민감 데이터, 접근 경계, 승인 필요 여부</div>
  <div class="oa-card"><span class="oa-card__icon">⚙️</span><span class="oa-card__title">DevOps (배포·운영)</span>"다른 사람이 로컬에서 실행하고, 건강한지 보고, 실패 원인을 알 수 있나?" — 실행 명령, 설정, health, 로그, 담당자</div>
  <div class="oa-card"><span class="oa-card__icon">📄</span><span class="oa-card__title">Documentation (문서)</span>목적·method·route·입력·출력·에러·데이터 경계·담당자·한계·지원 경로</div>
</div>

### Secret이 새는 9군데

소스 코드 · 프롬프트 · 설정 파일 · 요청 예시 · 응답 예시 · **로그** · 공유 스크린샷 · 문서 · 에러 메시지. 이 중 어디에도 실제 키나 고객 데이터가 있으면 안 돼요.

<div class="oa-eli">
  <span class="oa-eli__head">📝 SecOps 메모 예시 (요약)</span>
  <p>"가상의 티켓 문장만 사용, 실제 자격 증명 없음, 로컬 starter에는 인증·인가 검사가 <b>없음</b>. 더 넓게 쓰려면 자격 증명 관리, 최소 권한, 데이터 분류, 로깅, 고객 시스템 연결에 대한 보안·플랫폼 리뷰 필요."</p>
  <p>→ 이 메모는 보안을 <b>해결</b>하지 않아요. 현재 보안 경계를 <b>보이게</b> 할 뿐이에요.</p>
</div>

DevOps 메모에서도 **실제로 실행한 명령과 결과만** 적고, 안 한 건 untested로 표시해요. "검증 에러가 반환됨"과 "검증 실패가 로그에 기록됨"은 **서로 다른 증거**예요. 모델을 쓰는 테스트라면 모델 이름·설정·실측 응답 시간·사용량 근거를 따로 기록하고, ChatGPT 요금제나 "빠름" 라벨로 API 비용·속도를 추정하지 않습니다.

## 4단계 — API Foundation Evidence Pack

마지막 결과물은 다음 기술 담당자에게 넘길 **짧은 인수인계 기록**이에요.

| 증거 항목 | 담을 내용 |
|---|---|
| 기존 API 호출 결과 | 요청 형태, status, body, 한 줄 해석 |
| 로컬 health check | `/health` 응답 |
| 비즈니스 엔드포인트 결과 | 기대 입력 → 기대 응답 형태 |
| 의도적 에러 결과 | 실행한 에러 테스트와 각각이 증명하는 것 |
| 요청/응답 contract | endpoint, method, 필수 입력, 출력, status, 에러 상태 |
| SecOps·DevOps·문서 체크리스트 | placeholder, 권한 가정, 데이터 경계, 로그, 실행 명령, 담당자, 한계 |
| 준비도 갭 | 로컬 증명 vs 기업 배포의 차이 |
| 실행 중인 API 설명 | 작동 방식, 검증된 것, 로컬 한정인 것, 리뷰 필요한 것 |

각 항목은 같은 틀로 적어요.

```text
Evidence item / Evidence basis(executed·inspected only·not yet tested)
Command or request / Expected status·body / Actual status·body
What this proves / What this does NOT prove / Open question
```

**"이 증거가 증명하지 못하는 것"** 줄이 가장 중요해요. 팀이 결과를 과장하지 않게 막아 주거든요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 이 증거로 할 수 있는 결정</span>
    <ul>
      <li>기술 리뷰 단계로 넘어가기</li>
      <li>discovery(요구사항 탐색) 계속하기</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 이 증거로 할 수 없는 결정</span>
    <ul>
      <li>프로덕션 출시</li>
      <li>"보안·확장성·분류 품질 OK" 선언</li>
    </ul>
  </div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">💬 Confidence vs Evidence</span>
  <p><b>Confidence(자신감)</b>: "아마 될 거예요."<br><b>Evidence(증거)</b>: "이렇게 테스트했고, 이런 결과가 나왔고, 이건 아직 증명 안 됐어요."</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. mock API로 첫 호출 연습을 할 때 가장 안전한 방법은?</summary>
  <p>① SDK 출력을 믿고 요청·응답은 안 본다 ② 성공 응답에 필드가 있으면 에러 테스트는 생략 ③ 실제 키로 생성된 요청을 보내고 성공 응답 저장 ④ 문서화된 사실 + placeholder로 성공·실패를 모두 테스트하고 증거를 남긴다</p>
  <p><b>정답: ④</b> — 나머지는 검사 생략, 실패 경로 누락, 실제 secret 노출이라는 문제가 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. health·비즈니스 엔드포인트와 누락 입력 에러까지 로컬에서 잘 동작한다. 이게 증명하는 것은?</summary>
  <p><b>정답:</b> 쓸모 있는 <b>로컬 증거</b>가 생겼지만, 여전히 <b>준비도 리뷰가 필요</b>하다. (기업 출시 준비 완료 ✗ / 샘플 데이터라서 안전 ✗ / 공개 배포 전엔 리뷰 불가 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 짝 맞추기 — 각 요소가 증명하는 것은?</summary>
  <p><code>GET /health</code> → 로컬 서비스가 실행 중<br>성공한 <code>POST /triage-ticket</code> → 기대 입력을 받고 기대 형태로 응답<br>의도적 누락 테스트 → 통제된 검증·에러 동작이 있음<br>캡처한 요청/응답 contract → 다른 담당자가 API의 기대값을 검토 가능</p>
</details>

## 정리

- **문서 사실과 가정을 분리**하고, secret은 항상 placeholder·환경 변수로.
- 최소 API = `/health` + 비즈니스 엔드포인트 1개 + JSON + 입력 검증 + 통제된 에러.
- **성공도 실패도 증거**다. 증거엔 executed / inspected only / not yet tested 라벨을 붙인다.
- SecOps·DevOps·문서 렌즈로 "로컬 증명"과 "배포 준비"의 간극을 이름 붙여 드러낸다.
- Evidence Pack의 핵심은 **"무엇을 증명하지 못하는가"**를 함께 쓰는 것.
