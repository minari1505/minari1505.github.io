---
title: "AI Infrastructure & Implementation"
title_ko: "AI 인프라와 구현 — 네 가지 배포 질문, 학습 vs 추론, 실행 가능성 제약"
course: openai-foundational-knowledge
lesson: 7
---

## 학습 목표

- **네 가지 배포 질문**(앱 위치 · 모델 추론 위치 · 운영 주체 · 데이터 이동)을 큰 그림으로 설명하기
- 성능 · 비용 · 거버넌스 · 데이터 통제 · 운영 책임 사이의 **현실적인 트레이드오프** 알아보기
- **학습(training)** 과 **추론(inference)** 을 쉬운 말로 구분하기
- **CPU · GPU · NPU · 가속기** 같은 기본 하드웨어 용어 알아보기
- 흔한 **실행 가능성 제약 세 가지**: 지연 시간(latency) · 처리량(volume) · 데이터와 프라이버시
- 짧은 워크로드 설명에 **인프라 인식 렌즈**를 적용하기 — 아키텍처를 추천하지 않고

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>어디서 도나</strong>하드웨어 용어 + 네 가지 배포 질문</li>
  <li><strong>학습 vs 추론</strong>모든 AI 프로젝트가 학습을 하진 않는다</li>
  <li><strong>세 가지 제약</strong>지연 시간 · 처리량 · 데이터/프라이버시</li>
  <li><strong>워크로드에서 시작</strong>맥락 · 런타임 · 관측 가능성 렌즈</li>
</ol>

고객은 보통 **원하는 결과**로 시작해요: 더 빠른 지원, 더 나은 지식 접근, 자동화된 워크플로, 제품 안의 AI. 그런데 그 결과 뒤엔 현실적인 질문이 숨어 있어요. **앱은 어디서 돌지? 모델 추론은 어디서 일어나지? 어떤 데이터·도구가 필요하지? 어떤 제약이 실현 가능성을 바꿀까?** 이 코스의 목표는 시스템을 설계하는 게 아니라, 이런 질문을 **일찍 알아채고** 언제 기술·보안·거버넌스·구현 전문가를 불러야 할지 아는 거예요.

## 1. AI 시스템은 어디서 도나

### 기본 하드웨어 용어

<div class="oa-eli">
  <span class="oa-eli__head">🍳 쉽게 말하면 — 식당 주방</span>
  <p><b>CPU</b>는 무엇이든 하는 <b>만능 요리사 한 명</b>이에요. 손이 빠르지만 한 번에 한두 가지씩 해요. <b>GPU</b>는 <b>감자 깎기만 수백 명이 동시에</b> 하는 팀이에요. 같은 단순 작업을 엄청 많이 동시에 해야 할 때(AI 계산) 압도적이죠. <b>NPU</b>는 <b>집 주방에 있는 작은 전용 기계</b>(예: 휴대폰 속 AI 칩)예요. 식당까지 안 가도 집에서 간단한 AI 요리를 할 수 있게 해 줘요. <b>가속기</b>는 이런 "특정 작업 전용 도구"를 통틀어 부르는 말이에요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧠</span><span class="oa-card__title">CPU</span>중앙 처리 장치. <b>범용</b> 프로세서. 거의 모든 컴퓨터·서버의 일상 작업</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🎮</span><span class="oa-card__title">GPU</span>그래픽 처리 장치. 많은 연산을 <b>병렬로</b> 처리 → AI 학습 · 대규모 추론에 흔히 쓰임</div>
  <div class="oa-card"><span class="oa-card__icon">📱</span><span class="oa-card__title">NPU</span>신경망 처리 장치. 휴대폰·노트북·엣지 기기에서 <b>로컬 AI 처리</b></div>
  <div class="oa-card"><span class="oa-card__icon">⚡</span><span class="oa-card__title">가속기 (Accelerator)</span>특정 작업을 더 효율적으로 돌리는 <b>특수 하드웨어의 총칭</b>. GPU·NPU도 가속기의 예</div>
</div>

하드웨어 전문가가 될 필요는 없어요. 핵심은 하나예요: AI 워크로드는 **무엇을 하는지, 얼마나 빨리 응답해야 하는지, 어디서 도는지, 얼마나 많이 쓰이는지**에 따라 다른 종류의 컴퓨팅이 필요할 수 있다는 것.

### 네 가지 배포 질문 ⭐

AI 솔루션은 **여러 곳에서 도는 구성요소의 조합**일 수 있어요. "클라우드에 있어요" 같은 **환경 라벨 하나**에 기대지 말고, 네 가지 질문으로 **나눠서** 보세요. 앱·모델 추론·데이터·운영이 **모두 같은 곳에 있다고 가정하는 실수**를 막아 줘요.

<div class="oa-eli">
  <span class="oa-eli__head">🍕 쉽게 말하면 — 배달 피자</span>
  <p>배달 앱(<b>앱 위치</b>)은 내 휴대폰에 있지만, 피자는 가게 오븐(<b>추론 위치</b>)에서 구워요. 앱은 앱 회사가, 오븐은 가게가 관리하죠(<b>운영 주체</b>). 그리고 내 주소·카드 정보가 앱 → 가게 → 배달 기사로 <b>어디까지 어떻게 전달되는지</b>(<b>데이터 이동</b>)는 따로 확인해야 해요. "앱이 내 폰에 있으니 피자도 내가 굽는다"는 틀린 말이에요.</p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 250" role="img" aria-label="네 가지 배포 질문: 앱 위치, 모델 추론 위치, 운영 주체, 데이터 이동과 소매업체 예시">
  <defs>
    <marker id="ar7" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.a7{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.b7{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.c7{fill:#fff7dc;stroke:#e8d48f;stroke-width:1.5}.t7{font-size:12px;fill:#4a4a4a;font-weight:700}.s7{font-size:11px;fill:#7a7a7a}</style>
  </defs>
  <rect x="10" y="10" width="170" height="90" rx="12" class="a7"/><text x="95" y="36" text-anchor="middle" class="t7">① 앱 위치</text><text x="95" y="58" text-anchor="middle" class="s7">사용자가 만나는 앱은</text><text x="95" y="76" text-anchor="middle" class="s7">어디서 도나?</text>
  <rect x="190" y="10" width="170" height="90" rx="12" class="a7"/><text x="275" y="36" text-anchor="middle" class="t7">② 모델 추론 위치</text><text x="275" y="58" text-anchor="middle" class="s7">학습된 모델이 출력을</text><text x="275" y="76" text-anchor="middle" class="s7">만드는 곳은?</text>
  <rect x="370" y="10" width="170" height="90" rx="12" class="a7"/><text x="455" y="36" text-anchor="middle" class="t7">③ 운영 주체</text><text x="455" y="58" text-anchor="middle" class="s7">구성요소별로 설치·감시·</text><text x="455" y="76" text-anchor="middle" class="s7">유지·보안·지원은 누가?</text>
  <rect x="550" y="10" width="160" height="90" rx="12" class="a7"/><text x="630" y="36" text-anchor="middle" class="t7">④ 데이터 이동</text><text x="630" y="58" text-anchor="middle" class="s7">어떤 정보가 어디로,</text><text x="630" y="76" text-anchor="middle" class="s7">어떤 권한·보존 규칙?</text>
  <text x="360" y="128" text-anchor="middle" class="t7">예: 소매업체 지원 앱</text>
  <rect x="40" y="145" width="200" height="70" rx="12" class="b7"/><text x="140" y="172" text-anchor="middle" class="t7">🛒 지원 앱</text><text x="140" y="194" text-anchor="middle" class="s7">고객 클라우드 · 소매업체 운영</text>
  <rect x="480" y="145" width="200" height="70" rx="12" class="c7"/><text x="580" y="172" text-anchor="middle" class="t7">🤖 모델 추론</text><text x="580" y="194" text-anchor="middle" class="s7">OpenAI 호스팅 · OpenAI 운영</text>
  <line x1="240" y1="170" x2="476" y2="170" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar7)"/>
  <line x1="480" y1="192" x2="244" y2="192" stroke="#c99400" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#ar7)"/>
  <text x="360" y="162" text-anchor="middle" class="s7">승인된 요청</text>
  <text x="360" y="210" text-anchor="middle" class="s7">응답 — 이 데이터 경로는 따로 확인</text>
  <text x="360" y="240" text-anchor="middle" class="s7">앱 위치 = 고객 클라우드, 추론 위치 = OpenAI 호스팅 → 서로 다른 답</text>
</svg>
</div>

| 질문 | 확인할 것 | 놓치기 쉬운 점 |
|---|---|---|
| **① 앱 위치** | OpenAI 호스팅 워크스페이스 / 고객의 퍼블릭·프라이빗 클라우드 앱 / 온프레미스 앱 / 기기 위 소프트웨어 | 앱 위치만으로 **추론 위치는 알 수 없음** |
| **② 모델 추론 위치** | OpenAI 호스팅 서비스, 또는 그 배포 경로를 지원하는 모델이라면 고객·호스팅 제공자가 직접 실행 | "앱이 고객 클라우드에 있다 = 고객이 모델을 운영한다"가 **아님** |
| **③ 운영 주체** | 앱 · 연동 · 모델 서비스 · 데이터 저장소 · 기기 구성요소의 주인이 각각 다를 수 있음 | 고객이 운영하는 구성요소가 많을수록 **업데이트·모니터링·장애 대응·지원 책임**도 늘어남 |
| **④ 데이터 이동** | 사용자 · 앱 · 모델 서비스 · 도구 · 저장소 사이에서 무엇이 오가고, 어디서 처리·저장되며 어떤 권한·보존·검토 규칙이 붙는지 | 데이터 경로는 앱·추론 위치와 **별도로** 이해해야 함 |

### 배포 구성 예시 네 가지

| 구성 | 앱 | 추론 | 누가 무엇을 운영 |
|---|---|---|---|
| **OpenAI 호스팅 워크스페이스** | 호스팅 서비스 | 호스팅 서비스 | 제품·추론은 호스팅 서비스. 고객은 워크플로 적합성·접근 규칙·검토 방식을 결정 |
| **고객 클라우드 앱 + OpenAI 호스팅 추론** | 소매업체 클라우드 | OpenAI 호스팅 | 소매업체: 앱·연동 / OpenAI: 모델 서비스. 그 사이로 데이터 이동 |
| **고객 통제 앱 + 고객 실행 추론** | 고객 인프라 | 고객 인프라의 **오픈 웨이트 모델** | 컴퓨팅 환경·모델 서빙·모니터링·업데이트·지원을 **모두 고객이** 책임 |
| **엣지 앱 + 분할 처리** | 현장 기기 | 연결될 때는 원격 추론 | 어느 단계가 로컬/원격인지, **연결이 끊기면** 어떻게 동작하는지 명확히 해야 함 |

이건 **서로 배타적인 환경 라벨이 아니라 구성 예시**예요. 하나의 솔루션이 고객 운영 앱 + OpenAI 호스팅 추론 + 기기 로컬 처리 + 고객 통제 데이터 저장소를 **동시에** 쓸 수도 있어요.

### 현실적인 트레이드오프

트레이드오프는 환경 라벨 하나가 아니라 **앱 위치 · 추론 위치 · 데이터 이동 · 운영 주체의 조합**에서 나와요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">⏱️</span><span class="oa-card__title">성능·지연 시간</span>앱, 추론 서비스, 네트워크 경로, 워크로드, 하드웨어에 따라 달라짐. 실시간 대화 ≠ 백그라운드 문서 검토</div>
  <div class="oa-card"><span class="oa-card__icon">💰</span><span class="oa-card__title">비용·용량</span>호스팅 서비스는 서비스·사용량 기반 비용. 고객 실행 추론은 컴퓨팅·저장·운영·유지보수·엔지니어링 투자. <b>한 항목이 아니라 전체 비용</b>을 비교</div>
  <div class="oa-card"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">거버넌스·데이터 통제</span>데이터 이동, 처리 위치, 저장, 권한, 보존, 검토 요건이 경로를 좌우. 현재 제품·고객 요건으로 <b>검증</b></div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🔧</span><span class="oa-card__title">운영 책임</span>고객·파트너·OpenAI·다른 제공자가 구성요소별로 나눠 운영. <b>통제가 늘면 책임도 는다</b></div>
</div>

<div class="oa-bars">
  <div class="oa-bar"><span>OpenAI 호스팅 워크스페이스</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:20%"></span><span class="oa-bar__val">낮음</span></span></div>
  <div class="oa-bar"><span>고객 클라우드 앱 + 호스팅 추론</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:50%"></span><span class="oa-bar__val">중간</span></span></div>
  <div class="oa-bar"><span>고객 인프라 + 오픈 웨이트 모델 직접 실행</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:90%"></span><span class="oa-bar__val">높음</span></span></div>
  <p class="oa-bars__caption">고객이 직접 지는 운영 책임의 크기 — 개념적 표현이에요 (강의 수치 아님).</p>
</div>

## 2. 학습(Training) vs 추론(Inference)

<div class="oa-eli">
  <span class="oa-eli__head">📚 쉽게 말하면 — 공부와 시험</span>
  <p><b>학습</b>은 시험 전에 <b>교과서로 몇 달 공부하는 것</b>이에요. 시간과 에너지가 엄청 들고, 한 번 해 두면 머리에 남아요. <b>추론</b>은 <b>시험장에서 문제를 받아 답을 쓰는 것</b>이에요. 이미 공부한 걸 써서 매번 답을 만들죠. 그리고 <b>오픈북</b>처럼 시험 중에 참고 자료를 주는 건(맥락 추가) <b>다시 공부하는 것(재학습)과는 다른 일</b>이에요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🏋️ 학습 (Training)</span>
    <ul>
      <li>모델이 데이터에서 <b>패턴을 배우는</b> 단계</li>
      <li>모델을 <b>새로 만들거나 조정(adapt)</b>해 작업을 더 잘하게 함</li>
      <li>보통 <b>큰 컴퓨팅</b>이 필요하고, 실제 서비스에 쓰이기 <b>전에</b> 일어남</li>
      <li>대규모 데이터·인프라부터 목표를 좁힌 조정까지 다양</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">⚙️ 추론 (Inference)</span>
    <ul>
      <li>학습된 모델이 입력으로 <b>출력을 만드는</b> 단계</li>
      <li>질문하기, 문서 요약 맡기기, 초안 요청 — <b>모델을 쓸 때마다</b> 일어남</li>
      <li>요청 보내는 앱과 추론 인프라가 다른 곳일 수 있음 → <b>응답 시간·네트워크·규모·비용</b>이 중요</li>
      <li>사용자가 만나는 AI 경험은 <b>대부분 추론</b></li>
    </ul>
  </div>
</div>

### 흔한 오해: "모델을 학습시켜야 해요"

많은 고객용 AI 경험은 **이미 학습된 모델**을 써요. 필요한 건 모델을 처음부터 학습시키는 게 아니라 **올바른 비즈니스 맥락, 승인된 데이터 출처, 검색(retrieval), 설정, 워크플로 설계**인 경우가 많아요. 고객이 "모델을 학습시켜야 해요"라고 하면 실제로는 "우리 업무 맥락을 이해하고 이 워크플로에 맞게 행동했으면 해요"라는 뜻일 수 있어요.

> 되묻기(의역): **"모델을 학습·조정해야 하는 건가요, 아니면 이미 학습된 모델에 올바른 맥락을 주면 되는 건가요?"**

### 구분이 왜 중요한가

| 관점 | 의미 |
|---|---|
| **비용·컴퓨팅** | 학습과 추론은 비용·컴퓨팅·운영 패턴이 다름 |
| **운영 주체** | 고객은 앱을, 다른 제공자는 추론 서비스를 운영할 수 있음. 고객 실행 추론은 모델·인프라·지원 책임에 따른 **별도의 경로** |
| **구현 경로** | 학습된 모델 + 맥락 추가로 충분한 경우도, 더 깊은 기술 작업이 필요한 경우도 있음 |
| **소통 정확성** | **모든 AI 프로젝트에 새 모델 학습이 필요하다고 암시하지 말 것** |

### 예시 세 가지

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🏋️</span><span class="oa-card__title">학습·조정</span>의료 운영팀이 <b>라벨이 붙은 과거 지원 티켓</b> 대량 데이터로 모델을 조정 → 티켓 카테고리 인식이 좋아짐. 실서비스 <b>전에</b> 데이터로 모델 행동을 바꿈</div>
  <div class="oa-card"><span class="oa-card__icon">⚙️</span><span class="oa-card__title">추론</span>직원이 AI 어시스턴트에 정책 문서 <b>요약</b>을 요청 → 이미 배운 것 + 입력·맥락으로 요약 생성</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📎</span><span class="oa-card__title">맥락 추가</span>회사가 AI 어시스턴트를 <b>승인된 정책 문서</b>에 연결 → 새 모델 학습이 아니라 올바른 맥락·출처·<b>권한</b>을 주는 게 핵심</div>
</div>

## 3. 세 가지 실행 가능성 제약

처음엔 쉬워 보이는 AI 사용 사례도 **응답 시간, 사용량, 데이터 민감도, 프라이버시 기대, 운영 요건**을 따지면 복잡해질 수 있어요.

<div class="oa-eli">
  <span class="oa-eli__head">🍔 쉽게 말하면 — 학교 매점</span>
  <p><b>지연 시간</b>: 쉬는 시간 10분 안에 빵을 받아야 해요(바로 응답). 반면 "내일 아침 도시락 예약"은 천천히 준비해도 돼요(백그라운드). <b>처리량</b>: 학생 30명일 때와 전교생 1,000명이 <b>점심 종 치자마자 몰릴 때</b>는 필요한 직원 수가 완전히 달라요. <b>데이터와 프라이버시</b>: 알레르기 정보가 적힌 학생 명단은 아무나 보면 안 되고, 얼마나 보관할지도 정해야 해요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">⏱️</span><span class="oa-card__title">지연 시간 (Latency)</span>시스템이 <b>응답하는 데 걸리는 시간</b><br><small>빠름 필요: 음성 비서, 실시간 채팅 지원, 자동완성·제품 내 제안 / 느려도 됨: 예약 요약, 배치 문서 검토</small><br><small>영향 요인: 모델 선택, 출력 길이, 작업 복잡도, 시스템 설계, 데이터 검색, 도구 사용, 네트워크</small></div>
  <div class="oa-card"><span class="oa-card__icon">📈</span><span class="oa-card__title">처리량 (Volume)</span>처리해야 할 <b>요청·사용자·문서·메시지·작업의 양</b><br><small>소규모 팀 내부 어시스턴트 ≠ 수백만 명이 쓰는 고객용 기능 / 하루 10건 ≠ 시간당 수천 건</small><br><small>결정 요인: 사용자 수, 사용 빈도, 입력·출력 크기, 작업 주기</small></div>
  <div class="oa-card"><span class="oa-card__icon">🔒</span><span class="oa-card__title">데이터와 프라이버시</span>어떤 정보를 쓰고, <b>어디서 처리될 수 있고, 누가 접근하며, 어떤 요건</b>이 붙는지<br><small>예: 사내 정책 어시스턴트, 고객 지원, 법률 문서 검토, 의료 워크플로, 재무 분석, 직원·고객 데이터 워크플로</small></div>
</div>

**처리율(throughput)** = 일정 시간 동안 시스템이 끝내는 작업량. 하루 총량이 같아도 요청이 **한꺼번에 몰리면** 필요한 용량이 달라져요.

데이터·프라이버시는 앱 위치, 추론 위치, 구성요소 간 정보 이동, 사용 가능한 정보, 접근 가능한 사용자, 보존 기간, 필요한 통제·승인까지 영향을 줘요. 고객 대화에서 **프라이버시·보존·데이터 레지던시·컴플라이언스·요금제별 세부사항을 추측하지 마세요.** 약속 전에 현재 승인된 출처로 검증해야 해요. 파트너의 역할은 **알아채고 적절한 전문가를 부르는 것**이에요.

### 제약은 보통 함께 온다

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🎧 고객용 어시스턴트</span>
    <ul>
      <li>빠르게 응답해야 함 → <b>지연 시간</b></li>
      <li>많은 사용자 → <b>처리량</b></li>
      <li>민감한 고객 데이터 → <b>데이터·프라이버시</b></li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">⚖️ 사내 법률 문서 검토</span>
    <ul>
      <li>즉시 응답은 필요 없을 수 있음</li>
      <li>하지만 민감 정보, 긴 문서, 접근 통제, 보존 기대, 검토 단계 → <b>데이터·프라이버시</b> 중심</li>
    </ul>
  </div>
</div>

같은 워크로드가 **운영 주체** 질문도 일으켜요. 앱은 고객이, 추론은 다른 제공자가 운영할 수도 있고, 고객 운영 구성요소를 더 늘릴 수도 있죠. 선택마다 설치·모니터링·업데이트·장애 대응·지원 책임이 바뀌어요. 지금 할 일은 **무엇이 중요한지, 깊은 설계 전에 무엇을 확인해야 하는지** 찾는 거예요.

## 4. 인프라는 실행 가능성을 어떻게 바꾸나

### 워크로드에서 시작하라

**워크로드** = AI 시스템이 맡을 작업(또는 작업 묶음). 질문 답변, 문서 요약, 요청 분류, 콘텐츠 생성, 음성 처리, 코드 리뷰, 여러 시스템에 걸친 워크플로 지원 등. 선호하는 기술 경로를 **너무 일찍 고르지 말고** 워크로드부터 이해해요.

<ol class="oa-flow">
  <li><strong>누가?</strong>사용자는 누구</li>
  <li><strong>무엇을?</strong>지원할 작업</li>
  <li><strong>언제?</strong>즉시 응답 vs 백그라운드</li>
  <li><strong>무슨 정보?</strong>접근해야 할 데이터</li>
  <li><strong>얼마나?</strong>사용자·요청·문서·작업 수</li>
  <li><strong>어디서·누가?</strong>앱·추론 위치, 데이터 이동, 운영 주체</li>
</ol>

같은 AI 역량도 **고객 맥락에 따라** 인프라 고려사항이 달라요. 예를 들어 소규모 사내 팀을 위한 문서 요약과, 엄격한 접근·프라이버시·감사 요건이 붙은 **규제 대상 고객용 프로세스**의 문서 요약은 완전히 다른 이야기예요. 그래서 솔루션 경로는 익숙한 제품 표면이나 환경 라벨이 아니라 **워크로드와 연결해서** 고려해야 해요.

### 데이터 접근 · 맥락 · 거버넌스

많은 AI 워크로드는 저장된 정보(문서, 지식 출처, 앱 데이터, 로그, 입력·출력, 워크플로 기록)에 기대요. 기업 환경에서 이건 **맥락(context)** 의 일부예요 — AI가 비즈니스·워크플로·데이터·시스템·정책·권한에 대해 알아야 하는 것.

| 맥락이 영향을 주는 것 |
|---|
| AI 시스템이 **접근할 수 있는 정보** |
| 정보가 **저장·처리되는 위치** |
| **누가** 접근할 수 있는지 |
| 어떤 **권한·승인 단계**가 적용되는지 |
| **얼마나 오래** 보존해야 하는지 |
| 어떤 **거버넌스·프라이버시·감사** 요건이 붙는지 |

AI의 가치는 올바른 맥락에서 나오지만, 맥락은 **책임 있게** 다뤄야 해요. 사내 지원 어시스턴트가 승인된 정책 문서에 접근하면 더 유용하지만, 어떤 출처를 쓰는지 · 누가 무엇을 보는지 · 워크플로를 어떻게 검토하는지는 조직이 여전히 통제해야 해요.

"AI 어시스턴트가 필요해요"라는 말만으로는 실행 가능성을 판단할 수 **없어요.** 무엇을 하는지, 앱·추론은 어디서 도는지, 데이터는 어떻게 움직이는지, 얼마나 빨라야 하는지, 몇 명이 쓰는지, 어떤 통제·검토가 필요한지 알아야 해요.

### 간단한 기업 운영 렌즈: 맥락 · 런타임 · 관측 가능성 ⭐

<div class="oa-svg">
<svg viewBox="0 0 720 210" role="img" aria-label="기업 운영 렌즈: 맥락, 런타임, 관측 가능성과 별도 질문인 운영 주체·거버넌스">
  <defs>
    <style>.a7b{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.b7b{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.c7b{fill:#fff7dc;stroke:#e8d48f;stroke-width:1.5}.d7b{fill:#ffffff;stroke:#d8d8d8;stroke-width:1.5;stroke-dasharray:5 4}.t7b{font-size:13px;fill:#4a4a4a;font-weight:700}.s7b{font-size:11px;fill:#7a7a7a}</style>
  </defs>
  <rect x="10" y="10" width="225" height="120" rx="14" class="a7b"/>
  <text x="122" y="38" text-anchor="middle" class="t7b">📚 맥락 (Context)</text><text x="122" y="58" text-anchor="middle" class="s7b">"AI가 무엇을 알아야 하나?"</text>
  <text x="122" y="86" text-anchor="middle" class="s7b">비즈니스 · 워크플로 · 데이터</text><text x="122" y="104" text-anchor="middle" class="s7b">시스템 · 정책 · 권한</text>
  <rect x="248" y="10" width="225" height="120" rx="14" class="b7b"/>
  <text x="360" y="38" text-anchor="middle" class="t7b">⚙️ 런타임 (Runtime)</text><text x="360" y="58" text-anchor="middle" class="s7b">"AI는 어디서 동작하나?"</text>
  <text x="360" y="86" text-anchor="middle" class="s7b">앱 위치 · 추론 위치</text><text x="360" y="104" text-anchor="middle" class="s7b">도구·시스템 · 데이터 이동</text>
  <rect x="486" y="10" width="224" height="120" rx="14" class="c7b"/>
  <text x="598" y="38" text-anchor="middle" class="t7b">🔭 관측 가능성</text><text x="598" y="58" text-anchor="middle" class="s7b">"어떻게 지켜보고 개선하나?"</text>
  <text x="598" y="86" text-anchor="middle" class="s7b">활동 · 성능 · 오류</text><text x="598" y="104" text-anchor="middle" class="s7b">개선 기회에 대한 가시성</text>
  <rect x="120" y="148" width="480" height="50" rx="12" class="d7b"/>
  <text x="360" y="170" text-anchor="middle" class="t7b">➕ 별도 질문: 운영 주체 · 거버넌스</text>
  <text x="360" y="188" text-anchor="middle" class="s7b">누가 무엇을 운영? 권한 · 승인 · 보존 규칙 · 사람 검토 단계는?</text>
</svg>
</div>

이 렌즈는 인프라를 실제 기업 업무와 연결해 줘요. 앞으로 PartnerU의 제품·솔루션 패턴·기술·배포 대화에서 같은 질문이 계속 나와요: **AI가 무엇을 알아야 하나, 앱과 추론은 어디서 도나, 데이터는 어떻게 움직이나, 누가 운영하나, 활동을 어떻게 지켜보고 개선하나.** 이 코스에선 운영 모델을 설계하지 말고, **초기 실행 가능성 신호로 알아보기**만 하면 돼요.

### 연습 — 인프라 고려사항 알아보기

<div class="oa-eli">
  <span class="oa-eli__head">✍️ 세 가지 워크로드를 틀에 넣어 보기</span>
  <p>틀: 앱 위치 / 추론 위치 / 운영 주체와 데이터 이동 / 학습·추론·맥락 추가 중 무엇 / 주요 제약. 그리고 "기술팀이 제대로 설계하려면 어떤 배포 세부사항을 더 확인해야 할까?"</p>
  <p><b>① 사내 AI 워크스페이스</b> — 직원이 OpenAI 호스팅 워크스페이스에서 내부 문서 요약·커뮤니케이션 초안 작성. → 앱·추론 모두 호스팅 서비스, <b>추론</b>(+ 문서를 맥락으로), 주요 확인점은 내부 문서의 <b>데이터·프라이버시</b>와 접근 규칙.</p>
  <p><b>② 소매업체 고객 지원 어시스턴트</b> — 앱은 소매업체 클라우드, 추론은 OpenAI 호스팅, 많은 고객에게 빠르게 응답, 승인된 주문 정보 검색 가능. → 운영은 소매업체(앱·연동) + OpenAI(모델 서비스), <b>추론 + 맥락 추가</b>, 제약은 <b>지연 시간 + 처리량 + 데이터·프라이버시의 조합</b>.</p>
  <p><b>③ 현장 기술자 모바일 도구</b> — 연결이 불안정한 원격지, 앱과 승인된 참고 데이터가 기기에, 추론은 엣지 하드웨어에서 로컬. → 기기 중심 운영, <b>추론</b>(+ 로컬 참고 데이터), 제약은 연결·<b>지연 시간</b>과 기기 위 <b>데이터</b> 관리. (모범 답은 의역·정리)</p>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 소매업체가 자사 클라우드에서 지원 앱을 돌리고, 요청은 OpenAI 호스팅 모델 서비스로 보내요. 이 구성을 가장 잘 설명하는 것은?</summary>
  <p>① 앱을 소유하니 소매업체가 모델 하드웨어를 운영해야 한다 ② 모델 서비스가 호스팅이니 데이터 이동은 고려할 필요 없다 ③ 앱과 모델 추론 모두 고객 클라우드에서 돈다 ④ 앱은 고객 클라우드에서, 모델 추론은 OpenAI 호스팅으로 돈다</p>
  <p><b>정답: ④</b> — 앱 위치와 추론 위치는 별개 질문이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 사용자가 AI 어시스턴트에 정책 문서 요약을 요청하고, 학습된 모델이 입력으로 응답을 만들어요. 이건 무엇의 예일까요?</summary>
  <p>① 하드웨어 가속 ② 추론 ③ 온프레미스 배포 ④ 학습</p>
  <p><b>정답: ②</b> — 학습된 모델이 입력으로 출력을 만드는 건 추론이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 각 워크로드 예시를 가장 분명하게 드러나는 제약과 짝지어 보세요.</summary>
  <p>(가) 지원 어시스턴트가 빠르게 응답하고, 많은 사용자를 받고, 고객 데이터를 써야 함 (나) 음성 비서가 사용자가 말하는 동안 응답해야 함 (다) 워크플로가 민감한 고객 기록을 사용함 (라) 고객용 앱이 수백만 건의 요청을 받을 수 있음</p>
  <p><b>정답:</b> (가) 제약의 조합 · (나) 지연 시간 · (다) 데이터와 프라이버시 · (라) 처리량</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 고객용 어시스턴트가 빠르게 응답하고, 많은 사용자를 받고, 승인된 고객 데이터를 써야 해요. 인프라 고려사항을 가장 잘 설명하는 것은?</summary>
  <p>① 많은 사용자가 쓰니 처리량만 고려하면 된다 ② 어시스턴트일 뿐이니 인프라 고려사항이 없다 ③ 빠른 응답이 필요하니 지연 시간만 고려하면 된다 ④ 지연 시간 · 처리량 · 데이터 접근 · 거버넌스를 함께 고려해야 할 수 있다</p>
  <p><b>정답: ④</b> — 제약은 보통 함께 와요.</p>
</details>

## 정리

- AI 솔루션은 여러 곳에서 도는 구성요소의 조합이다. 환경 라벨 하나 대신 **앱 위치 · 모델 추론 위치 · 운영 주체 · 데이터 이동**을 따로 묻는다.
- **고객 클라우드 앱도 OpenAI 호스팅 추론을 쓸 수 있다.** 앱을 운영한다고 모델까지 운영하는 건 아니다. 고객이 직접 추론을 돌리는 건 그걸 지원하는 **오픈 웨이트 모델** 같은 별도 경로다.
- 트레이드오프는 **성능 · 비용 · 거버넌스·데이터 통제 · 운영 책임**의 조합. 통제가 늘면 책임도 는다.
- **학습** = 모델이 배우거나 조정되는 것(사용 전), **추론** = 학습된 모델이 입력으로 출력을 만드는 것(사용할 때마다). 많은 사용 사례는 새 학습이 아니라 **맥락 · 승인된 출처 · 권한 · 설정 · 워크플로 설계**가 필요하다.
- CPU(범용) · GPU(병렬, 학습·대규모 추론) · NPU(기기 로컬 AI) · 가속기(특수 하드웨어 총칭).
- 실행 가능성 제약 세 가지 — **지연 시간 · 처리량 · 데이터와 프라이버시** — 는 보통 **함께** 나타난다.
- 인프라 대화는 **워크로드에서 시작**한다. **맥락 · 런타임 · 관측 가능성** 렌즈로 묻고, 운영 주체와 거버넌스는 별도로 확인하며, 필요할 때 기술·보안·거버넌스·구현 전문가를 부른다.
