---
title: "Realtime, Voice, and Multimodal API Experiences"
title_ko: "실시간·음성·멀티모달 API 경험 — '멋져서'가 아니라 '필요해서' 고르기"
course: openai-api-deployment
lesson: 9
---

## 학습 목표

- 실시간·음성·이미지·멀티모달이 **언제 필요하고**, 언제 텍스트로 충분한지 판단하기
- **speech-to-speech Realtime**과 **체인형 음성 파이프라인**(STT → 텍스트 에이전트 → TTS) 비교하기
- 모달리티를 고르고, 실시간 오디오라면 **WebRTC·WebSocket·SIP** 연결 방식 선택하기
- 사용자 통제·확인·에스컬레이션·fallback·채널 전환이 들어간 **상호작용 흐름** 설계하기
- 모달리티별 안전·개인정보·접근성·동의·운영·관측성 점검 항목 찾기
- 결과를 **Realtime and Multimodal Experience Blueprint**로 정리하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>필요성 판단</strong>사용자 순간(user moment)부터</li>
  <li><strong>패턴 비교</strong>STT·TTS·음성·이미지·멀티모달</li>
  <li><strong>흐름 설계</strong>통제·확인·fallback·전환</li>
  <li><strong>안전장치</strong>안전·개인정보·접근성·관측성</li>
  <li><strong>블루프린트</strong>6가지 질문 → 추천</li>
</ol>

<div class="oa-eli">
  <span class="oa-eli__head">🎒 쉽게 말하면</span>
  <p>학교 가는데 <b>자전거·버스·킥보드·도보</b> 중 뭘 탈지는 "제일 멋진 것"이 아니라 거리·날씨·짐에 따라 정하죠. 비 오는 날 짐이 많으면 버스, 가까우면 걷기.</p>
  <p>API 경험도 같아요. 음성·이미지·실시간은 강력하지만 <b>지연·개인정보·동의·접근성·fallback</b>이라는 "짐"이 같이 따라와요. 그래서 이 코스의 핵심 규칙은 하나예요: <b>"워크플로우 필요를 안전하게 충족하는 가장 단순한 모달리티를 고른다."</b></p>
</div>

## 핵심 용어

| 용어 | 뜻 | 예시 |
|---|---|---|
| **Modality** | 입력·출력의 종류 | 텍스트, 음성, 오디오, 이미지, 생성 이미지 |
| **Multimodal pattern** | 둘 이상의 입출력을 조합 | 음성 입력 + 사진 + 텍스트 안내 |
| **Fallback** | 원래 방식이 안 될 때의 안전한 대체 경로 | 음성 → 텍스트, 더 선명한 사진 요청, 사람 연결 |
| **Channel handoff** | 채널을 옮기는 것 | 통화 → 채팅 상담, 통화 → 이메일 요약 |
| **Consent & media retention** | 녹음·촬영 전 고지/동의, 미디어를 얼마나·어디에·누가 보관하나 | 통화 녹음 안내, 사진 삭제 시점 |
| **Modality observability** | 지연, 말 차례, 음질·화질, 인식 오류, fallback·사람 연결 비율 등 | "사진 다시 찍기" 비율 |

## 1. 멀티모달이 정말 필요한가?

### 사용자 순간(user moment)부터 묻기

"음성을 넣자!"로 시작하지 말고, 먼저 이 질문들에 답해요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__title">무엇을 하려 하나?</span>사용자의 실제 목표</div>
  <div class="oa-card"><span class="oa-card__title">어디에 있나?</span>공항, 공장, 사무실, 운전 중…</div>
  <div class="oa-card"><span class="oa-card__title">무엇을 줄 수 있나?</span>말, 사진, 타이핑</div>
  <div class="oa-card"><span class="oa-card__title">무엇이 필요하나?</span>짧은 안내, 비교표, 확인서</div>
  <div class="oa-card"><span class="oa-card__title">얼마나 빨리?</span>실시간 vs 기다려도 됨</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__title">텍스트만이면 뭐가 실패하나?</span>이 답이 없으면 텍스트로 충분할 가능성 ↑</div>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 비텍스트가 정당화될 수 있는 경우</span>
    <ul>
      <li>타이핑이 어려움 / 손을 못 씀 (장갑 낀 창고 작업자)</li>
      <li>말 자체가 업무 (통화, 통역)</li>
      <li>눈으로 봐야 하는 증거 (장비 패널 사진)</li>
      <li>실시간 주고받기 필요</li>
      <li>접근성 때문에 다른 모달리티가 더 효과적</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 텍스트가 더 안전한 기본값인 경우</span>
    <ul>
      <li>검토·감사·검색이 중요 (법무 정책 Q&A)</li>
      <li>지연을 감당 못 함</li>
      <li>미디어 개인정보·동의가 미해결</li>
      <li>음질·화질이 불안정</li>
      <li>텍스트로도 필요를 더 낮은 위험으로 충족</li>
    </ul>
  </div>
</div>

### 예시: 항공편 지연 시 여행자 앱

공항을 걸으며 시간에 쫓기는 여행자. **"음성이 더 자연스러우니까 음성 비서를 만들자"는 약한 추천**이에요. 강한 추천은 사용자 순간에서 출발해 **섞어서** 설계합니다.

| 조각 | 역할 |
|---|---|
| 🔊 **Systems-to-voice** | "게이트가 바뀌었고, 환승 시간은 충분해요" 같은 급한 음성 알림 |
| 🗣️ **Voice-to-action** | "호텔 체크인 늦출 수 있는 옵션 보여줘" |
| 📝 **Text fallback** | 최종 일정, 확인 번호, 약관, 비교가 필요한 옵션 |
| 🙋 **Human handoff** | 불안한 상황, 결제·정책 예외, 안전하게 해결 불가 |

**막는 조건도 함께 적어요:** 지연이 크면 실시간 음성은 오히려 짜증 · 녹음 동의 규칙 미해결이면 음성 캡처 출시 ✗ · 예약 변경 전 확인을 못 하면 voice-to-action 미준비 · 텍스트로 충족 가능하면 1차 버전엔 풀 실시간 음성 불필요.

## 2. 패턴 비교

| 패턴 | 하는 일 | 유용할 때 | 설계 포인트 |
|---|---|---|---|
| **Realtime** | 낮은 지연으로 즉석 주고받기 | 타이밍·말 끊기·즉답이 중요 | 지연, 끼어들기 처리, fallback, 사용자 통제 |
| **STT** (speech-to-text) | 말 → 글 | 타이핑보다 말이 쉬움, 통화 기록 | 인식 품질, 억양·소음, 동의, **수정 경로** |
| **TTS** (text-to-speech) | 글 → 말 | 핸즈프리, 접근성 | 명료함·속도, 자막, 다시 듣기, 텍스트 대안 |
| **Voice 패턴** | 말이 경험의 중심 | 실시간 지원, 안내형 작업 | 차례, 확인, 동의, 통제 |
| **Image input** | 사진·스크린샷을 증거로 | 눈으로 봐야 하는 업무 | 화질, 빠진 맥락, 개인정보 |
| **Image output** | 시각물 생성·변형 | 시안, 시각 안내 | 검토 기준, 브랜드·권리, 접근성, 사람 승인 |
| **Multimodal** | 여러 입출력 조합 | 정말 여러 채널이 필요할 때 | 복잡도, 각 모달리티의 필요성 |

<div class="oa-eli">
  <span class="oa-eli__head">📝 STT 결과(transcript)는 "검토 가능"하지만 "정확"하진 않아요</span>
  <p>받아쓰기 시험에서 친구가 내 말을 받아 적어준 노트와 같아요. 나중에 볼 수 있어서 좋지만, <b>이름·날짜·수량·약속</b>은 틀릴 수 있으니 확인·수정 경로를 꼭 넣어요.</p>
  <p>TTS도 비슷해요. 길거나 복잡한 내용(약관, 가격, 확인 번호)은 잘못 들을 수 있으니 <b>텍스트로도</b> 제공하고, <b>AI가 생성한 목소리임을 고지</b>해요.</p>
</div>

### 음성 아키텍처 — "어떻게 처리하나" vs "무엇을 돕나"

<div class="oa-svg">
<svg viewBox="0 0 720 230" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Speech-to-speech Realtime과 체인형 음성 파이프라인 비교">
  <g font-family="inherit" font-size="13">
    <text x="10" y="22" font-weight="700" fill="#4a4a4a">① Speech-to-speech Realtime</text>
    <rect x="10" y="35" width="120" height="44" rx="10" fill="#fff8df" stroke="#f1e4b8"/>
    <text x="70" y="62" text-anchor="middle" fill="#4a4a4a">🎤 사용자 음성</text>
    <rect x="200" y="35" width="320" height="44" rx="10" fill="#ffdd7a" stroke="#d9b850"/>
    <text x="360" y="62" text-anchor="middle" fill="#4a4a4a">모델이 오디오를 직접 듣고 직접 말함</text>
    <rect x="590" y="35" width="120" height="44" rx="10" fill="#fff8df" stroke="#f1e4b8"/>
    <text x="650" y="62" text-anchor="middle" fill="#4a4a4a">🔊 음성 응답</text>
    <path d="M130 57 H195 M520 57 H585" stroke="#c99400" stroke-width="2" marker-end="url(#ar9)"/>
    <text x="360" y="98" text-anchor="middle" font-size="11" fill="#8a8a8a">자연스러운 대화·끼어들기·실시간 도구 사용이 필요할 때</text>

    <text x="10" y="135" font-weight="700" fill="#4a4a4a">② Chained voice pipeline</text>
    <rect x="10" y="148" width="120" height="44" rx="10" fill="#fff8df" stroke="#f1e4b8"/>
    <text x="70" y="175" text-anchor="middle" fill="#4a4a4a">🎤 사용자 음성</text>
    <rect x="160" y="148" width="120" height="44" rx="10" fill="#fff1c2" stroke="#e8cf7a"/>
    <text x="220" y="175" text-anchor="middle" fill="#4a4a4a">STT → 텍스트</text>
    <rect x="310" y="148" width="130" height="44" rx="10" fill="#ffdd7a" stroke="#d9b850"/>
    <text x="375" y="175" text-anchor="middle" fill="#4a4a4a">텍스트 에이전트</text>
    <rect x="470" y="148" width="100" height="44" rx="10" fill="#fff1c2" stroke="#e8cf7a"/>
    <text x="520" y="175" text-anchor="middle" fill="#4a4a4a">TTS</text>
    <rect x="600" y="148" width="110" height="44" rx="10" fill="#fff8df" stroke="#f1e4b8"/>
    <text x="655" y="175" text-anchor="middle" fill="#4a4a4a">🔊 음성 응답</text>
    <path d="M130 170 H155 M280 170 H305 M440 170 H465 M570 170 H595" stroke="#c99400" stroke-width="2" marker-end="url(#ar9)"/>
    <text x="360" y="215" text-anchor="middle" font-size="11" fill="#8a8a8a">중간 텍스트·영구 기록·결정론적 검사/승인·기존 텍스트 에이전트 재사용이 필요할 때</text>
  </g>
  <defs><marker id="ar9" markerWidth="8" markerHeight="8" refX="7" refY="4" orient="auto"><path d="M0,0 L8,4 L0,8 z" fill="#c99400"/></marker></defs>
</svg>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">📞 GPT-Live (강의 시점 기준 기능)</span>
  <p>강의는 <b>말하면서 동시에 들을 수 있고(full duplex)</b>, 별도 백엔드 에이전트가 일하는 동안 대화를 이어가는 GPT-Live를 소개해요. 전화 상담원이 "잠시만요, 확인해 볼게요" 하면서 옆 동료에게 조회를 맡기는 것과 비슷해요.</p>
  <p><b>Client delegation</b>(앱이 대화 기록·작업 상태·백엔드 맥락을 직접 관리, 결과 검토 후 반환)과 <b>Responses delegation</b>(GPT-Live가 맥락을 넘기고 OpenAI 호스팅 모델 호출을 관리)을 세션 시작 때 고르고, 바꾸려면 새 세션이 필요하다고 해요. Live 오디오 프론트엔드는 이미지를 직접 받지 않아서, 이미지는 vision 백엔드가 분석해 텍스트로 넘겨요. <b>말을 끊는다고 백엔드 작업이 자동 취소되진 않으니</b> 앱이 취소 여부를 관리·검증해야 해요. 실제 적용 전 최신 문서 확인 필요.</p>
</div>

### 실시간 오디오 연결 방식

| 방식 | 언제 | 한 줄 이유 |
|---|---|---|
| **WebRTC** | 브라우저·모바일이 직접 마이크 캡처·재생 | 사용자 기기 ↔ 모델 직결 |
| **WebSocket** | 서버가 이미 미디어 파이프라인·콜 시스템에서 원시 오디오를 받음 | 서버 ↔ 모델 |
| **SIP** | 전화(telephony) 음성 에이전트 | 전화망 연결 (번역·전사 지원 여부는 확인) |

연결 방식과 모델 지원은 자주 바뀌니 **구현 직전 최신 문서로 검증**해요.

### 세 가지 음성 상호작용 패턴

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🗣️➡️⚙️</span><span class="oa-card__title">Voice-to-action</span>말로 요청 → 추론·도구 → 범위가 정해진 작업. <b>민감·되돌릴 수 없는 행동은 실행 전 확인</b></div>
  <div class="oa-card"><span class="oa-card__icon">🖥️➡️🔊</span><span class="oa-card__title">Systems-to-voice</span>시스템 맥락을 음성 안내·알림으로. 언제 도움이고 언제 방해인지 정의</div>
  <div class="oa-card"><span class="oa-card__icon">🗣️↔️🗣️</span><span class="oa-card__title">Voice-to-voice</span>언어·작업이 바뀌어도 말로 이어지는 대화 (예: 실시간 통역)</div>
</div>

**세 패턴 공통 안전장치:** AI 음성임을 고지 · 도구 사용 중임을 알림("확인해 볼게요") · 중요한 행동 전 확인 · 멈춤/중지/수정/채널 전환 · 위험·민감·미해결 시 사람 연결.

### 멀티모달 조합 예시

| 조합 | 상황 | 왜 맞나 |
|---|---|---|
| 음성 입력 + 텍스트 출력 | 영업 담당자가 통화 후 말로 업데이트 → CRM용 요약 | 말은 빠르고, 글은 검토·저장이 쉬움 |
| 음성 + 이미지 입력 | 기술자가 장비 사진 + 구두 설명 | 사진은 증거, 말은 사진에 없는 맥락 |
| 이미지 입력 + 음성 안내 | 창고 직원이 파손 라벨을 비추고 손이 바쁜 채로 안내 받음 | 보고 판단 + 핸즈프리 |
| 실시간 음성 + 도구 + 서면 확인 | 항공 상담 중 재예약 옵션 조회 → 확정 전 서면 확인 | 대화 + 조회 + 검토 가능한 승인 |
| 사진 + STT + 구조화 요약 | 손해사정사가 사진·구술 → 청구 요약 | 증거 + 맥락 + 다운스트림 처리 |

멀티모달을 배포하려면 **주 모달리티 · 보조 모달리티 · 전환 시점 · 실패 경로 · 확인 지점 · 검토 경로**를 먼저 적어요.

## 3. 상호작용 흐름 설계

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 흐름 (항공 재예약)</span>
    <ul><li>"여행자가 말하고, 일정을 올리면, 재예약된다."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 흐름</span>
    <ul>
      <li>시작: "옵션 비교는 돕지만 확인 없인 변경 안 해요"</li>
      <li>입력: 음성 + 탑승권 캡처, 이름·편명·날짜 재확인</li>
      <li>불명확하면 추측 대신 짧은 질문</li>
      <li>소수의 적격 옵션 제시</li>
      <li>변경 전 <b>텍스트로 요약 → 확인</b>, 예외·결제는 사람</li>
      <li>"그만" → 새 행동 차단, 진행 중 변경 상태 <b>확인 후</b> 보고</li>
    </ul>
  </div>
</div>

<ol class="oa-flow">
  <li><strong>시작</strong>무엇을 도울 수 있는지 안내</li>
  <li><strong>입력</strong>음성·사진 + 핵심 정보 재확인</li>
  <li><strong>명확화</strong>모호하면 질문</li>
  <li><strong>응답</strong>짧은 음성 + 텍스트 요약</li>
  <li><strong>확인/검토</strong>행동 전 명시적 확인</li>
  <li><strong>fallback·전환</strong>텍스트·사람으로</li>
</ol>

강의의 TypeScript 예제는 `start → capture_symptom → clarify → respond → complete` 단계를 추적하는 상태 머신이에요. 강의가 스스로 짚듯이 이건 **뼈대일 뿐**이고, 실제 구현은 응답 생성·수정 처리·전달·fallback까지 해야 하며 **의도한 결과에 도달했을 때만 complete**로 표시해야 해요.

### 사용자 통제 — 멈춤·중지·수정·확인·전환

<div class="oa-eli">
  <span class="oa-eli__head">🎧 쉽게 말하면</span>
  <p>음악 앱에 <b>일시정지·정지·뒤로 감기</b> 버튼이 없다면 불안하겠죠. 음성 비서도 "잠깐", "그만", "그게 아니라…", "글로 보여줘"가 항상 통해야 해요. 그리고 <b>음악을 끈다고 이미 결제된 앨범이 환불되진 않듯</b>, 말하기를 멈춰도 이미 완료된 행동은 되돌려지지 않아요.</p>
</div>

| 구분해야 할 세 가지 | 의미 |
|---|---|
| 음성 출력 멈춤 | 지금 말하는 걸 끊음 |
| 오디오 캡처 멈춤 | 마이크 끔 → **음성 명령도 못 들으니** 버튼 등 다른 재개 수단 필요 |
| 대기 작업 취소 | 진행 중 도구 실행·예약 변경 취소 → **취소 성공을 검증한 뒤에만** "취소됨"이라고 말함 |

<div class="oa-eli">
  <span class="oa-eli__head">✂️ Truncation (강의 기준 Realtime 동작)</span>
  <p>사용자가 끼어들어 응답이 끊기면, <b>사용자가 실제로 들은 부분까지만</b> 대화 기록에 남겨야 모델의 맥락이 어긋나지 않아요. WebRTC·SIP는 서버가 처리하고, WebSocket은 클라이언트가 재생을 멈추고 들은 지점을 계산해 <code>conversation.item.truncate</code>를 보내요. 앱이 직접 멈출 땐 <code>response.cancel</code>로 응답을 취소하고 미재생 오디오를 비워요(WebRTC·SIP: <code>output_audio_buffer.clear</code>). <b>모델 응답 취소 ≠ 앱의 도구 실행 취소.</b></p>
</div>

### Fallback 문장 공식

> **"[조건]이 생기면, 시스템은 [안전한 동작]을 하고, 사용자는 [다음 행동]을 할 수 있다."**

| 조건 | 안전한 동작 | 사용자 다음 행동 |
|---|---|---|
| 음성이 불명확 | 들은 내용을 되읽음 | 확인하거나 핵심을 타이핑 |
| 사진 화질 낮음 | 확신 있게 해석 못 한다고 설명 | 더 선명한 사진이나 텍스트 설명 |
| 민감한 행동 요청 | 초안만 만들고 사람 검토로 | 검토 결과 대기 |
| 동의 미해결 | 미디어 캡처 안 함 | 텍스트 전용 경로 |
| 지연 과다 | 서면 업데이트로 전환 | 글로 확인 |

Fallback은 실패가 아니라 **신뢰를 지키는 최선의 방법**일 때가 많아요. 강의 예제의 `decideNextStep()`처럼 respond / clarify / fallback / escalate를 **상호작용 로직 안에** 미리 넣어 두고, 실패 후에 즉흥적으로 대응하지 않아요. (예제의 길이 체크·품질 점수 0.7은 설명용 신호일 뿐이에요.)

## 4. 안전·개인정보·접근성·동의·운영 점검

### 안전과 사람 검토 — 세 가지를 정한다

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">✅</span><span class="oa-card__title">허용 행동</span>저위험 응답, 일상 안내, 정보 수집, 요약, 초안</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⛔</span><span class="oa-card__title">하드 경계</span>위험·미지원·정책 밖·역할을 넘는 조언·추론·결정·행동</div>
  <div class="oa-card"><span class="oa-card__icon">🙋</span><span class="oa-card__title">검토·에스컬레이션 지점</span>민감·고영향·불확실·미해결이면 사람이 개입</div>
</div>

| 상황 | 더 안전한 응답 패턴 |
|---|---|
| 명확·저위험·범위 내 | 승인된 워크플로우대로 응답 |
| 불완전·불명확·낮은 확신 | 계속하기 전 명확화 질문 |
| 고객·기록·시스템·결정에 영향 | 확인 그리고/또는 사람 검토 |
| 범위 밖·위험한 안내 요청 | 거절, 안전한 방향 전환, 허용 정보만 |
| 계속 민감·고위험·미해결 | 승인된 사람 담당자에게 에스컬레이션 |

예: 사진 기반 보험 청구 접수 비서는 정보 수집·피해 요약·다음 단계 안내는 하되, **보상 여부·지급액 결정은 거절**(`refuse`)하고, 기록 업데이트는 `review`, 민감 건은 `escalate`, 흐린 사진은 `clarify`. 결정 라벨을 반환하는 것만으로는 행동이 막히지 않으니 **앱이 실제로 강제**해야 해요.

### 개인정보와 미디어 처리

사진엔 배경 정보가, 녹음엔 다른 사람 목소리가, 전사본은 보관·삭제 대상 기록이 될 수 있어요.

| 검토 영역 | 확인할 것 |
|---|---|
| 목적 | 어떤 미디어를 왜 캡처하나 |
| 접근·저장 | 누가 보고, 어디에, 얼마나 보관 |
| 사용자 통제 | 사진 삭제·교체, 해석 수정 가능? |
| 파생물·로그 | 전사본·요약·임베딩·라벨·구조화 필드, 로그에 남기면 안 되는 것 |
| 금지 | 절대 녹음·저장·재생·로깅·재사용·보관하지 말 것 |

### 접근성 — 모두가 말하고 듣고 보는 건 아니다

| 필요 | 설계 대응 |
|---|---|
| 들을 수 없거나 오디오를 원치 않음 | 자막, 전사본, 읽을 수 있는 텍스트 |
| 시각 출력에 의존 불가 | 비시각적 대안, 텍스트 설명 |
| 말로 전한 세부가 중요 | 나중에 볼 수 있게 텍스트로도 |
| 너무 빠름 | 멈춤·반복·느리게·채널 전환 |
| 선호 모달리티가 안 맞음 | 텍스트, 사람 연결, 다른 승인 채널 |

<div class="oa-eli">
  <span class="oa-eli__head">♿ 한 사람에게 도움이 되는 게 다른 사람을 배제할 수 있어요</span>
  <p>음성 안내는 손이 바쁜 사람에겐 최고지만, 시끄러운 공장에서 일하는 사람이나 글로 다시 확인해야 하는 사람에겐 맞지 않아요. 경고·조건·확인 내용 같은 <b>작업에 필수인 정보는 텍스트 대안에 모두</b> 들어가야 해요.</p>
</div>

### 모달리티별 관측성 신호

API가 살아 있어도 경험은 실패할 수 있어요. 핵심 질문: **"이 모달리티가 워크플로우를 돕고 있나, 새로운 마찰·위험·지원 부담을 만들고 있나?"**

| 신호 영역 | 무엇을 보나 | 무엇을 결정하나 |
|---|---|---|
| 반응성·흐름 | 지연, 말 차례, 끼어들기, 겹침, 수정 | 흐름 조정, 통제 강화, 단순 fallback |
| 입력 품질·해석 | 음질·화질, 전사·해석 오류, 낮은 확신 | 명확화, 재촬영, 채널 전환, 사람 |
| 워크플로우·의존성 지연 | 도구·검색·다운스트림 지연 | 실시간에 허용 가능한가 |
| Fallback·전환 | 텍스트 전환·재시도·포기·사람 요청 비율 | fallback이 정상 작동 중인가, 설계·신뢰 문제인가 |
| 안전·개인정보·접근성·동의 플래그 | 미해결 동의, 접근성 blocker, 민감 행동 | 계속 / 보완 / 에스컬레이션 / 중지 |
| 품질·개선 증거 | 피드백, 검토자 메모, 반복 실패 | 확대 전 무엇을 고칠까 |

### 예시: 사진+음성 보험 청구 접수

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 추천</span>
    <ul><li>"사진과 음성이 접수를 쉽게 만드니 진행하자."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 추천</span>
    <ul>
      <li>멀티모달 적합성은 그럴듯함</li>
      <li>하지만 동의·보관·접근성 fallback·사람 검토 경계·관측성 신호가 정해지기 전엔 <b>확대 불가</b></li>
      <li>합성/승인 테스트 데이터, 최종 결정 없음, 명확한 사용자 통제로 <b>제한적 프로토타입</b>은 가능</li>
    </ul>
  </div>
</div>

## 5. Realtime and Multimodal Experience Blueprint

<ol class="oa-flow">
  <li><strong>사용자 순간</strong>누가·어디서·무엇을·얼마나 빨리·텍스트로 충분?</li>
  <li><strong>모달리티</strong>패턴 + 음성 아키텍처 + 연결 방식 + 기각한 대안</li>
  <li><strong>흐름</strong>입력→명확화→응답→확인→행동→fallback</li>
  <li><strong>통제·안전장치</strong>경계·검토·고지·동의·접근성·미디어</li>
  <li><strong>검증·모니터링</strong>지연·인식·화질·fallback·사람 연결</li>
  <li><strong>추천</strong>다음 단계·이유·blocker·담당·바꿀 증거</li>
</ol>

추천은 <b>"인상적이라서·데모가 잘 돼서"가 아니라</b> 워크플로우 필요·사용자 맥락·입출력 요구·준비 조건에 근거해요. 가능한 결론: 프로토타입 / 조건부 진행 / 경험 단순화 / 검증까지 보류 / 불필요한 모달리티로 기각.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 보험 청구에 음성+사진을 넣고 싶다. 1차 파일럿은 텍스트 폼으로도 필요한 정보를 모을 수 있고, 오디오·사진 동의·보관 규칙은 아직 승인 전이다. 최선의 결정은?</summary>
  <p><b>정답:</b> 1차 파일럿은 <b>더 단순한 텍스트 워크플로우</b>로, 멀티모달 버전은 동의·보관 규칙 해결 전까지 보류. (고객이 좋아할 것 같아서 출시 ✗ / 음성만 남기기 ✗ / 사진은 덜 민감하니 사진만 ✗ — 사진에도 사람·번호판·주소가 찍혀요)</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 짝 맞추기 — 워크플로우 필요와 맞는 패턴</summary>
  <p>고객 통화를 편집 가능한 텍스트로 → <b>Speech-to-text</b><br>말로 요청해 범위가 정해진 도구 행동 → <b>Voice-to-action</b><br>시스템 맥락으로 음성 안내 → <b>Systems-to-voice</b><br>사진·스크린샷을 증거로 → <b>Image input</b><br>음성 + 사진 + 서면 확인 조합 → <b>Multimodal pattern</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 현장 지원 비서 흐름에 멈춤·잘못 들은 내용 수정·텍스트 전환·사람 에스컬레이션이 없다. 가장 큰 갭은?</summary>
  <p><b>정답:</b> <b>사용자 통제·수정·채널 전환·에스컬레이션 지점</b>이 빠짐. (기능 부족 ✗ / 핸즈프리니까 글 전부 제거 ✗ / 추천 단계 자동 실행 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 계정 조회·요금 조정이 가능한 실시간 음성 상담 파일럿. 요금 조정 전 확인, 화난 고객의 사람 연결, 전사 오류·fallback 추적이 없다. 다음 단계는?</summary>
  <p><b>정답:</b> 확인·사람 연결·관측성이 정의될 때까지 <b>파일럿을 제한</b>. (처리 시간 단축되니 출시 ✗ / 단순 질문 고객만 ✗ / 개인정보 위해 관측성 제거 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q5. 바쁜 매장 카운터에서 파손 상품 반품을 처리하는 직원용 비서. 가장 강한 설계 근거는?</summary>
  <p><b>정답:</b> <b>음성 + 이미지 + 서면 확인, 그리고 fallback·에스컬레이션.</b> (환불 경로 자동 선택 ✗ / 흐린 사진에도 계속 ✗ / 더 재밌게 생성 이미지 ✗)</p>
</details>

## 정리

- **가장 단순한 모달리티**가 워크플로우 필요를 안전하게 충족하면 그걸 고른다 — 텍스트일 수도 있다.
- 패턴(STT·TTS·voice-to-action·systems-to-voice·voice-to-voice·이미지 입출력·멀티모달)은 기능이 아니라 **사용자가 하려는 일**로 고른다.
- 음성은 **아키텍처**(speech-to-speech Realtime vs STT→에이전트→TTS 체인)와 **연결 방식**(WebRTC·WebSocket·SIP)을 따로 정한다.
- 흐름엔 **멈춤·중지·수정·확인·전환·에스컬레이션**이 처음부터 들어가야 하고, 말을 멈춰도 완료된 행동은 되돌아가지 않는다.
- 안전 경계·미디어 처리·동의·접근성·모달리티별 관측성까지 갖춘 뒤 블루프린트의 **6가지 질문**으로 추천한다.
- GPT-Live, delegation 모드, Realtime 이벤트 이름 등은 **강의 시점 기준**이므로 구현 전 최신 문서로 확인한다.
