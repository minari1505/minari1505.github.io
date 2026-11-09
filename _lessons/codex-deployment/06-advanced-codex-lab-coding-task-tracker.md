---
title: "Advanced Codex Lab - Coding Task Tracker"
title_ko: "고급 Codex 실습 — 할 일 관리 API를 Codex와 함께 고치고 확장하기"
course: codex-deployment
lesson: 6
---

## 학습 목표

- 낯선 저장소를 **Codex로 먼저 탐색·설명**하게 하고 구조를 파악하기
- 버그마다 **재현 → 진단 → 패치 → diff 확인 → 검증** 루프를 Codex에게 맡기기
- 요구사항이 분명한 **작은 기능(검색 필터)**을 Codex로 구현하고 기존 동작이 깨지지 않았는지 확인하기
- 의도한 변경만 담긴 **리뷰 가능한 커밋**을 준비하기
- 직접 코딩하는 사람이 아니라 **리뷰어 역할**을 유지하기

## 한눈에 보는 실습 흐름

<ol class="oa-flow">
  <li><strong>저장소 준비</strong>가상환경 · 의존성 · 앱 실행</li>
  <li><strong>Task 1 · 코드베이스 이해</strong>파일 지도 · 요청 흐름 추적</li>
  <li><strong>Task 2 · 버그 2개 수정</strong>재현→진단→패치→diff→검증</li>
  <li><strong>Task 3 · 검색 기능 추가</strong>q 파라미터 · 필터 조합</li>
  <li><strong>Task 4 · 깨끗한 커밋</strong>런타임 부산물 제외 · 승인 후 커밋</li>
  <li><strong>최종 검증</strong>수용 기준 5가지</li>
</ol>

이 실습의 핵심 한 줄:

> **수동 코딩 속도가 아니라, Codex를 잘 지휘하고 diff를 리뷰하고 결과를 검증하는 것.**

## 무엇을 만드나 — 작은 할 일 관리(Task Tracker) API

<div class="oa-eli">
  <span class="oa-eli__head">👷 쉽게 말하면</span>
  <p>당신은 현장 감독이고 Codex는 숙련 작업자예요. 감독은 망치를 들지 않아요. 대신 <b>"먼저 현장을 둘러보고 보고해", "고치기 전에 왜 고장 났는지 설명해", "고친 부분 보여줘", "다시 테스트해서 증명해"</b>라고 지시하고, 결과를 눈으로 확인한 뒤 승인해요. 이 실습은 그 감독 역할을 연습하는 거예요.</p>
</div>

실습 저장소는 **FastAPI로 만든 작은 할 일 관리 API**예요(강의 기준). 일부러 버그 2개가 심어져 있고, 검색 기능이 빠져 있어요. 예상 소요 시간은 30~45분.

| 파일 (강의 기준) | 역할 |
|---|---|
| `app/main.py` | 라우트(엔드포인트) 정의 |
| `app/schemas.py` | 요청·응답 모델 |
| `app/service.py` | 비즈니스 로직 |
| `app/store.py` | JSON 파일 저장 |
| `data/tasks.json` | 샘플 데이터 |

<div class="oa-svg">
<svg viewBox="0 0 720 150" role="img" aria-label="GET tasks 요청이 main, service, store, tasks.json으로 흐르는 구조">
  <defs>
    <marker id="ar6" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.b6{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.g6{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.t6{font-size:13px;fill:#4a4a4a;font-weight:700}.s6{font-size:10.5px;fill:#8a8a8a}</style>
  </defs>
  <rect x="5" y="40" width="120" height="70" rx="8" class="g6"/><text x="65" y="70" text-anchor="middle" class="t6">요청</text><text x="65" y="90" text-anchor="middle" class="s6">GET /tasks</text>
  <line x1="125" y1="75" x2="148" y2="75" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/>
  <rect x="152" y="40" width="130" height="70" rx="8" class="b6"/><text x="217" y="70" text-anchor="middle" class="t6">main.py</text><text x="217" y="90" text-anchor="middle" class="s6">라우트</text>
  <line x1="282" y1="75" x2="305" y2="75" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/>
  <rect x="309" y="40" width="130" height="70" rx="8" class="b6"/><text x="374" y="70" text-anchor="middle" class="t6">service.py</text><text x="374" y="90" text-anchor="middle" class="s6">필터·완료 로직</text>
  <line x1="439" y1="75" x2="462" y2="75" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/>
  <rect x="466" y="40" width="120" height="70" rx="8" class="b6"/><text x="526" y="70" text-anchor="middle" class="t6">store.py</text><text x="526" y="90" text-anchor="middle" class="s6">JSON 읽기·쓰기</text>
  <line x1="586" y1="75" x2="609" y2="75" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar6)"/>
  <rect x="613" y="40" width="102" height="70" rx="8" class="g6"/><text x="664" y="70" text-anchor="middle" class="t6">tasks.json</text><text x="664" y="90" text-anchor="middle" class="s6">데이터</text>
  <text x="374" y="135" text-anchor="middle" class="s6">Bug A(상태 필터)와 Bug B(완료 저장)는 이 흐름의 어딘가에 숨어 있어요</text>
</svg>
</div>

### 실습 규칙 — 모든 단계에 Codex를 쓴다

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔍</span><span class="oa-card__title">바꾸기 전에 살펴보기</span>코드 수정 전에 Codex에게 먼저 탐색시키기</div>
  <div class="oa-card"><span class="oa-card__icon">▶️</span><span class="oa-card__title">명령도 Codex가</span>설치·검증 명령을 Codex가 실행하고 무엇을 돌렸는지 보고</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">패치 전 설명</span>원인을 먼저 설명받고 나서 고치기</div>
  <div class="oa-card"><span class="oa-card__icon">🧾</span><span class="oa-card__title">diff 리뷰</span>변경을 수락하기 전에 diff를 직접 확인</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🔁</span><span class="oa-card__title">매번 재검증</span>변경할 때마다 검증을 다시 돌리기</div>
</div>

준비물(강의 기준): Git, Codex 앱 또는 Codex cloud 접근, Python 3.9 이상.

## 0단계 — 저장소 준비

**하는 일:** 실습 저장소를 받아 가상환경을 만들고, 의존성을 설치하고, 개발 서버(uvicorn)로 앱을 띄워요. 로컬 주소의 API와 자동 생성 문서 페이지(`/docs`)가 열리면 성공이에요.

**Codex에게 맡기는 방식:** "개발용으로 세팅하고, 가상환경 만들고, 의존성 설치하고, 앱을 띄운 뒤 **정확히 무엇을 실행했는지** 알려줘."

**검증하는 것:** 앱이 뜨고 엔드포인트가 응답하는가, 그리고 Codex가 실행한 명령이 투명하게 보고되는가.

## 1단계 — Task 1: 코드베이스 이해하기

<div class="oa-eli">
  <span class="oa-eli__head">🗺️ 쉽게 말하면</span>
  <p>처음 간 대형 마트에서 물건을 찾을 때 무작정 돌아다니지 않고 <b>안내도부터</b> 보죠. Codex에게 "이 저장소의 안내도를 그려줘"라고 먼저 요청하는 단계예요.</p>
</div>

**하는 일:** 바꾸기 전에 Codex에게 저장소가 무엇을 하는지, 핵심 파일이 어디 있는지 설명시키고, `GET /tasks` 요청이 라우트에서 저장소까지 어떻게 흐르는지, **필터 로직이 어디 사는지** 추적시켜요. "오늘 합류한 엔지니어에게 이 코드베이스를 요약해줘" 같은 요청도 좋아요.

**검증하는 것:** 위 표 같은 **머릿속 지도**(라우트 · 스키마 · 비즈니스 로직 · 저장 · 데이터)가 생겼는가. 이 지도가 있어야 다음 단계에서 Codex의 진단이 맞는지 판단할 수 있어요.

## 2단계 — Task 2: 버그 두 개 고치기

버그마다 같은 루프를 Codex에게 돌려요.

<ol class="oa-flow">
  <li><strong>Reproduce</strong>문제가 실제로 있는지 재현</li>
  <li><strong>Diagnose</strong>근본 원인 설명</li>
  <li><strong>Patch</strong>가장 작고 깔끔한 수정</li>
  <li><strong>Show diff</strong>무엇이 바뀌었는지 보여주기</li>
  <li><strong>Verify</strong>같은 호출로 고쳐졌음을 증명</li>
</ol>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🐞 Bug A — 상태 필터가 고장</span>
    <p><b>증상:</b> 전체 목록과 <code>status=open</code> 필터 목록을 호출해 비교하면, 필터가 제대로 걸러내지 못해요.</p>
    <p><b>고친 뒤 기대:</b> <code>status=open</code>이면 <b>열린 작업만</b> 돌아와야 해요.</p>
    <p><b>Codex 지시 요지:</b> 재현하고, 근본 원인을 설명하고, 가장 작은 수정을 하고, 결과로 검증해.</p>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🐞 Bug B — 완료 상태가 저장 안 됨</span>
    <p><b>증상:</b> 작업 1을 "완료" 처리한 뒤 다시 조회하면 완료가 유지되지 않아요.</p>
    <p><b>고친 뒤 기대:</b> 다시 조회해도 <b>done 상태 유지</b>, <code>completed_at</code>이 비어 있지 않음.</p>
    <p><b>Codex 지시 요지:</b> <b>저장소/비즈니스 로직 경계</b>에서 고치고, API 호출을 다시 돌려 영속성을 증명해.</p>
  </div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">💾 Bug B가 왜 중요할까</span>
  <p>게임에서 레벨업을 했는데 저장 버튼이 고장 나 있으면, 껐다 켜면 원래대로죠. 화면에서는 바뀐 것처럼 보여도 <b>파일에 기록되지 않으면</b> 의미가 없어요. 그래서 검증도 "완료 API가 성공 응답을 줬다"가 아니라 <b>"다시 조회해도 그대로다"</b>로 해야 해요.</p>
</div>

**검증하는 것:** 재현에 쓴 **같은 호출**을 수정 후 다시 돌려서 기대 결과가 나오는가. 그리고 diff가 문제 범위에만 머무는가.

## 3단계 — Task 3: 검색 기능 추가

**하는 일:** `GET /tasks`에 선택적 `q` 쿼리 파라미터를 추가해요.

| 요구사항 | 확인 방법 (개념) |
|---|---|
| 대소문자 구분 없이 매칭 | 대문자·소문자로 검색해도 같은 결과 |
| 제목과 설명 **둘 다** 검색 | 설명에만 있는 단어로도 찾아짐 |
| 상태 필터와 **함께** 동작 | `status=open` + `q=…` 조합이 둘 다 만족하는 것만 반환 |
| `q`가 없으면 기존 동작 그대로 | 이전 호출 결과가 바뀌지 않음 |

**Codex 지시 요지:** "제목·설명에 걸쳐 대소문자 무시 매칭하는 q 필터를 구현하고, q가 없을 때 기존 동작을 유지하고, 여러 검색어로 호출해서 검증해."

**검증하는 것:** 새 기능이 동작하는가 + **기존 기능이 깨지지 않았는가(회귀 없음)** + 필터끼리 조합이 맞는가.

## 4단계 — Task 4: 깨끗한 커밋 준비

<div class="oa-eli">
  <span class="oa-eli__head">🧹 쉽게 말하면</span>
  <p>숙제를 제출할 때 연습장 낙서까지 같이 넣지 않죠. 테스트하느라 API를 여러 번 호출하면 <b>샘플 데이터 파일(tasks.json)이 덩달아 바뀌어요.</b> 그건 "의도한 코드 변경"이 아니라 실험 부산물이니 되돌리고, 진짜 변경만 커밋해요.</p>
</div>

**하는 일:**

1. Codex에게 `data/tasks.json`이 **런타임 테스트 때문에만** 바뀌었는지 확인시키고, 그렇다면 되돌리기
2. 코드 변경을 요약하게 하기
3. **최종 diff를 사람이 승인한 뒤에** 새 브랜치에서 커밋 (강의 예시는 `codex/` 접두사 브랜치)
4. PR 리뷰용으로 원격에 올리기

**검증하는 것:** 커밋에 **의도한 코드 변경만** 들어 있는가, 그리고 커밋이 **사람 승인 뒤에** 만들어졌는가.

## 5단계 — 최종 검증 체크리스트

상태 필터(open/done), 다른 작업 완료 후 재조회, 검색, 검색+상태 조합 호출을 Codex에게 다시 돌리게 하고 "이제 저장소가 올바르게 동작하는지" 보고받아요.

<div class="oa-bars">
  <div class="oa-bar"><span>상태 필터</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:100%"></span><span class="oa-bar__val">open/done이 정확히 걸러짐</span></span></div>
  <div class="oa-bar"><span>완료 영속성</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:100%"></span><span class="oa-bar__val">재조회해도 done + completed_at 설정</span></span></div>
  <div class="oa-bar"><span>검색</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:100%"></span><span class="oa-bar__val">대소문자 무시</span></span></div>
  <div class="oa-bar"><span>필터 조합</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:100%"></span><span class="oa-bar__val">검색 + 상태가 함께 동작</span></span></div>
  <div class="oa-bar"><span>diff 품질</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:100%"></span><span class="oa-bar__val">리뷰 가능하고 범위가 좁음</span></span></div>
  <p class="oa-bars__caption">5가지 수용 기준 — 모두 통과해야 하는 체크리스트를 막대로 표현한 개념적 표현이에요.</p>
</div>

실습 마지막에는 Codex와의 작업 대화를 (개인 식별 정보를 지운 뒤) 정리해 **리뷰어에게 제출**하는 단계가 있어요. 무엇을 시켰고, 무엇이 돌아왔고, 어떻게 검증했는지가 곧 평가 대상이라는 뜻이에요.

## 이 실습이 앞 레슨과 연결되는 지점

| 앞 레슨 개념 | 이 실습에서의 모습 |
|---|---|
| 리뷰 가능한 출력 (레슨 5) | 매 수정마다 diff를 보여주고 사람이 승인 |
| 검증 증거 (레슨 2·4) | 재현에 쓴 호출을 다시 돌려 결과로 증명 |
| 제한된 위임 = Level 2 (레슨 5) | 버그 하나·기능 하나씩, 정해진 테스트 경로, 지정된 리뷰어(나) |
| 사람이 소유하는 결정 (레슨 3) | 커밋·푸시는 최종 diff 승인 후에만 |
| 작업 범위 통제 | 런타임 부산물(tasks.json)을 커밋에서 제외 |

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 이 실습에서 내 역할은?</summary>
  <p><b>정답: 리뷰어.</b> Codex를 구현 팀원으로 두고, 나는 지시하고 diff를 리뷰하고 결과 검증을 확인한다. 수동 코딩 속도 경쟁이 아니다.</p>
</details>
<details class="oa-quiz">
  <summary>Q2. 버그 하나를 고칠 때 Codex에게 맡기는 5단계 루프는?</summary>
  <p><b>정답:</b> 재현(Reproduce) → 진단(Diagnose) → 패치(Patch) → diff 보여주기(Show diff) → 검증(Verify).</p>
</details>
<details class="oa-quiz">
  <summary>Q3. "완료 처리" 버그가 고쳐졌다는 가장 좋은 증거는?</summary>
  <p><b>정답:</b> 완료 처리 후 <b>다시 조회했을 때</b>도 done이 유지되고 <code>completed_at</code>이 채워져 있다는 것. 완료 API의 성공 응답만으로는 영속성을 증명하지 못한다.</p>
</details>
<details class="oa-quiz">
  <summary>Q4. 검색 기능(q)을 추가한 뒤 꼭 확인할 "회귀" 항목은?</summary>
  <p><b>정답:</b> q를 빼고 호출했을 때 기존 결과가 그대로인지, 그리고 상태 필터와 함께 쓸 때 두 조건을 모두 만족하는지.</p>
</details>
<details class="oa-quiz">
  <summary>Q5. 커밋 전에 data/tasks.json이 바뀌어 있다. 어떻게 하나?</summary>
  <p><b>정답:</b> 테스트 호출 때문에만 바뀐 것이면 되돌려서, 커밋에 의도한 코드 변경만 담는다. 커밋은 최종 diff를 승인한 뒤에.</p>
</details>

## 정리

- 실습 대상은 FastAPI 기반 **할 일 관리 API**(강의 기준): 버그 2개 수정 + 검색 기능 추가 + 깨끗한 커밋.
- 순서는 **탐색(지도 그리기) → 버그 루프(재현·진단·패치·diff·검증) → 기능 추가(회귀 확인) → 커밋 정리 → 최종 검증.**
- 검증은 언제나 **같은 호출을 다시 돌려 결과로** 한다. "고쳤어요"라는 말이 아니라 증거를 받는다.
- 커밋에는 **의도한 변경만**, 그리고 **사람의 승인 후에.** 이것이 레슨 1~5에서 말한 리뷰 가능성·검증 증거·사람 소유 결정을 손으로 직접 해보는 연습이다.
