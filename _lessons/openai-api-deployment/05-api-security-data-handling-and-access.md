---
title: "API Security, Data Handling, and Access Controls"
title_ko: "API 보안·데이터 처리·접근 통제 — 데이터·접근·행동의 경계 긋기"
course: openai-api-deployment
lesson: 5
---

## 학습 목표

- API 워크플로우의 **데이터 이동**을 그리고, 민감도·소유자·최소화·로깅·보존을 판단하기
- **인증 / 인가 / 최소 권한 / secret 관리**를 구분하기
- 자동으로 해도 되는 행동, **승인이 필요한 행동, 막아야 할 행동**을 나누기
- 승인 게이트·감사 가능성(auditability)·로깅 경계·보완 조치(remediation) 정의하기
- **API Security and Data Handling Review**로 배포 준비도 추천하기

## 한눈에 보는 코스 흐름 — 5개의 리뷰 렌즈

<ol class="oa-flow">
  <li><strong>데이터 경계</strong>무엇을 만지고, 얼마나 민감한가</li>
  <li><strong>접근·secret 경계</strong>누가 부르고, 뭘 할 수 있나</li>
  <li><strong>행동·승인 경계</strong>자동 / 승인 필요 / 금지</li>
  <li><strong>증거 갭·보완</strong>무엇이 빠졌고 누가 고치나</li>
  <li><strong>준비도 추천</strong>Ready ~ Pause</li>
</ol>

<div class="oa-eli">
  <span class="oa-eli__head">🏫 쉽게 말하면</span>
  <p>학교 행정실을 떠올려 보세요. <b>어떤 서류가 들어오고 어디에 보관되는지</b>(데이터 경계), <b>누가 사무실 열쇠를 갖고 어느 서랍까지 열 수 있는지</b>(접근 경계), <b>성적 정정처럼 교장 결재가 필요한 일은 무엇인지</b>(승인 경계)가 정해져 있어야 사고가 안 나요.</p>
  <p>이 코스는 보안을 "설계"하는 게 아니라, 이 경계들을 <b>눈에 보이게</b> 만들어 담당자가 검토할 수 있게 하는 법을 배워요. 법률·컴플라이언스 조언은 범위 밖이에요.</p>
</div>

## 1. 데이터 경계 — 데이터가 어디로 흐르나

보안 리뷰는 **모델 호출이 아니라 데이터 이동 전체 경로**에서 시작해요.

<div class="oa-svg">
<svg viewBox="0 0 720 210" role="img" aria-label="API 워크플로우의 데이터 이동: 입력, 처리, 저장, 로그, 출력">
  <defs>
    <marker id="ar5" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.b{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.h{fill:#fff3c4;stroke:#c99400;stroke-width:1.5}.t{font-size:13px;fill:#4a4a4a}.s{font-size:11px;fill:#8a8a8a}.w{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}</style>
  </defs>
  <rect x="10" y="70" width="130" height="60" rx="10" class="b"/><text x="75" y="96" text-anchor="middle" class="t">① 들어오는 데이터</text><text x="75" y="114" text-anchor="middle" class="s">헬프데스크 티켓 필드</text>
  <rect x="200" y="70" width="140" height="60" rx="10" class="h"/><text x="270" y="96" text-anchor="middle" class="t">② 처리·변환</text><text x="270" y="114" text-anchor="middle" class="s">모델 호출 · 요약 생성</text>
  <rect x="400" y="10" width="140" height="56" rx="10" class="b"/><text x="470" y="34" text-anchor="middle" class="t">③ 저장</text><text x="470" y="52" text-anchor="middle" class="s">티켓에 요약 기록</text>
  <rect x="400" y="140" width="140" height="56" rx="10" class="w"/><text x="470" y="164" text-anchor="middle" class="t">④ 로그 ⚠️</text><text x="470" y="182" text-anchor="middle" class="s">새로운 노출 면</text>
  <rect x="590" y="70" width="120" height="60" rx="10" class="b"/><text x="650" y="96" text-anchor="middle" class="t">⑤ 나가는 데이터</text><text x="650" y="114" text-anchor="middle" class="s">다른 시스템·사람</text>
  <line x1="140" y1="100" x2="196" y2="100" stroke="#c99400" stroke-width="1.6" marker-end="url(#ar5)"/>
  <line x1="340" y1="90" x2="396" y2="45" stroke="#c99400" stroke-width="1.6" marker-end="url(#ar5)"/>
  <line x1="340" y1="112" x2="396" y2="160" stroke="#c99400" stroke-width="1.6" marker-end="url(#ar5)"/>
  <line x1="340" y1="100" x2="586" y2="100" stroke="#c99400" stroke-width="1.6" marker-end="url(#ar5)"/>
</svg>
</div>

각 단계마다: 무엇이 들어오나 · 누가 보내나 · 무엇이 저장/로그되나 · 다른 시스템이나 경계 밖으로 나가나 · **각 소스와 목적지의 소유자는 누구인가**.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 문장</span>
    <ul><li>"API가 지원 데이터를 써서 요약을 돌려준다."</li></ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 문장</span>
    <ul><li>제목·설명·요청자 역할·우선순위·제품 영역·내부 메모를 받음 → 내부 요약·다음 행동 생성 → <b>직원 확인 후에만</b> 티켓에 기록 → 로그엔 요청 ID·시각·엔드포인트·상태 코드·지연·에러 분류만, <b>티켓 본문·요청자 정보·secret·생성 문장은 제외</b></li></ul>
  </div>
</div>

### 민감도 분류 — 확실하지 않으면 높은 쪽으로

<div class="oa-bars">
  <div class="oa-bar"><span>🔑 Secret·자격 증명</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:100%"></span><span class="oa-bar__val">API 키, 비밀번호, 토큰, 연결 문자열</span></span></div>
  <div class="oa-bar"><span>🩺 개인·규제 데이터</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:85%"></span><span class="oa-bar__val">개인정보, 규제 대상 정보</span></span></div>
  <div class="oa-bar"><span>🏢 고객 기밀</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:65%"></span><span class="oa-bar__val">고객별 기록·문서·계정 맥락</span></span></div>
  <div class="oa-bar"><span>📁 내부 데이터</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:40%"></span><span class="oa-bar__val">사내용 업무 정보</span></span></div>
  <div class="oa-bar"><span>🌐 공개 데이터</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:12%"></span><span class="oa-bar__val">공개 승인된 정보</span></span></div>
  <p class="oa-bars__caption">막대 길이 = 보호 필요도 (개념적 표현). 고객·관할별 분류는 반드시 소유자 확인.</p>
</div>

평범해 보이는 지원 티켓에도 이름·직원 정보·결제·건강 관련 내용이 들어올 수 있어요. **추측하지 말고 "미해결"로 표시해 책임자에게 넘겨요.**

<div class="oa-eli">
  <span class="oa-eli__head">🏥 의료 데이터(PHI) 예시</span>
  <p>보호 대상 건강정보를 OpenAI API로 보내려면 먼저 OpenAI와 <b>BAA</b>(Business Associate Agreement)가 필요해요. 조건을 갖춘 API 조직은 셀프서비스로 표준 계약을 수락할 수 있고, API HIPAA 적격을 위해선 <b>Modified Retention</b> 구성도 필요해요. 하지만 계약 수락만으로 "모든 엔드포인트 적격"이나 "앱 전체가 HIPAA 준수"가 되는 건 아니에요 — 고객 시스템·로그·저장소까지 포함한 전체 흐름을 보안·개인정보·법무가 승인해야 해요.</p>
</div>

### 데이터 최소화

필요한 것만 **수집·전송·저장·반환·로그**해요. 입력만 줄이고 출력에서 다 노출하면 소용없어요. 예: 계정 브리핑 API는 고객 불만 전문 대신 "미해결 청구 에스컬레이션 1건 있음"이면 충분할 수 있어요.

### OpenAI 플랫폼의 데이터 처리 — 두 가지 저장 질문을 분리하세요

| 구분 | 무엇 | 기억할 점 |
|---|---|---|
| **학습 사용** | API 데이터로 모델 학습? | 조직이 명시적으로 opt-in하지 않으면 **사용 안 함** |
| **Abuse monitoring 로그** | 정책 위반 감시용 플랫폼 로그 | 기본 생성, 프롬프트·응답·메타데이터 포함 가능, **최대 30일** 보관(예외 있음) |
| **Application state** | 일부 기능이 작업 유지를 위해 보관하는 데이터 | 기간은 엔드포인트·기능·요청 설정·객체 수명에 따라 다름 |
| **Modified Abuse Monitoring / ZDR** | 승인 기반 보존 통제(조직·프로젝트 단위) | **켜져 있다고 가정 금지.** ZDR이어도 모든 엔드포인트가 적격·무상태가 되는 건 아님. ZDR에선 Responses·Chat Completions의 <code>store</code>가 false로 취급 |

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🤖</span><span class="oa-card__title">Agents API</span>세션 상태를 보관. 도구를 자체 샌드박스에서 돌려도 세션 상태는 관리형 API에 있음. <b>US 레지던시만, ZDR 비적격</b> → 비-US 레지던시나 ZDR 요구는 못 맞춤</div>
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">Decisions API</span>별도 적격성 확인 필요. 다른 엔드포인트에서 승인된 보존 방식이 자동 적용된다고 보면 안 됨</div>
  <div class="oa-card"><span class="oa-card__icon">🗄️</span><span class="oa-card__title">ZDR with PSP</span>안전 검토 기록을 <b>고객 클라우드 저장소</b>에 암호화 보관. 연결 등록만으론 정책 변경 X, <b>검증 통과</b>로 활성화. 기록은 최소 30일 유지, 저장소 상태는 담당자가 계속 모니터링</div>
</div>

<p style="font-size:0.8rem;color:#8a8a8a">※ 데이터 통제 적격성·엔드포인트 동작·보존 세부는 바뀔 수 있어요. 구현·고객 안내 전 최신 공식 Data controls 문서로 확인하고 날짜를 기록하세요.</p>

## 2. 접근·secret 경계

<div class="oa-eli">
  <span class="oa-eli__head">🎟️ 인증 vs 인가 (다시 한번)</span>
  <p><b>인증</b>: "누구세요?" → 학생증 확인. <b>인가</b>: "이 교실·이 서랍을 열 수 있나요?" → 권한 확인. 학생증이 진짜라고 교무실 금고를 열 수 있는 건 아니죠. <b>인증 성공 ≠ 안전</b>이에요.</p>
</div>

호출자가 사용자·앱·서비스 계정·내부/외부 시스템·다른 API 중 무엇인지 적고, **유효한 토큰이 있어도 이 레코드를 읽고 저 시스템에 쓸 수 있는지는 백엔드가 따로 판단**해야 해요.

<div class="oa-eli">
  <span class="oa-eli__head">🔐 Sign in with ChatGPT</span>
  <p>사용자가 참여 앱에 ChatGPT 계정으로 로그인하고, 원하면 AI 요청에 자기 플랜 사용량을 쓰도록 <b>별도로</b> 선택할 수 있어요(크레딧 사용은 또 별도 동의). 하지만 <b>플랜 선택은 "누가 비용을 내나"</b>이고, <b>티켓을 읽고 수정할 권한은 티켓 시스템 규칙</b>이 정해요. 둘은 따로 기록해야 해요.</p>
</div>

### Secret을 안전하게 다루는 법

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 하면 안 되는 것</span>
    <ul>
      <li>키 하드코딩, 저장소·로그·프롬프트·스크린샷에 노출</li>
      <li><b>브라우저·모바일 클라이언트 코드에 OpenAI 키 배포</b></li>
      <li>팀원 개인 API 키 공유</li>
      <li>테스트용 토큰을 운영에 재사용</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 해야 하는 것</span>
    <ul>
      <li>환경 변수·관리형 secret 저장소, <b>백엔드 경유</b> 호출</li>
      <li>사람은 개인 접근, 워크로드는 <b>프로젝트 범위 서비스 계정</b></li>
      <li>실험·스테이징·운영 자격 증명 분리, 사용량 모니터링</li>
      <li>만료 전 교체 → 앱 갱신 → 검증 → 옛 키 폐기 / 노출 의심 시 즉시 회전·폐기</li>
    </ul>
  </div>
</div>

지원되는 워크로드라면 장기 키 대신 **workload identity federation**(신뢰된 워크로드 ID를 단기 토큰으로 교환)도 검토 대상이에요.

### 최소 권한 (Least privilege)

티켓 카테고리를 읽고 라벨을 쓰는 게 전부라면, 모든 레코드 읽기·첨부 내보내기·사용자 프로필 수정·삭제 권한은 **필요 없어요.** 넓은 권한의 공유 서비스 계정은 프로토타입은 쉽게 만들지만 **승인받기는 어렵게** 해요.

| OpenAI 플랫폼 RBAC 포인트 | 내용 |
|---|---|
| 조직(Organization) vs 프로젝트(Project) | 조직 역할은 모든 프로젝트에 적용, 프로젝트 역할은 그 안에서만 → **가능하면 프로젝트 수준** |
| 유효 권한 = 역할들의 **합집합** | 여러 역할이 합쳐져 의도치 않은 권한이 생길 수 있음 |
| 환경 분리 | 실험·스테이징·운영을 **다른 프로젝트**로 |
| 두 겹 확인 | 사용자 역할이 업로드를 허용해도 **키 권한이 읽기 전용이면 거부**. 키만 넓혀도 역할이 없으면 거부 |
| 검증 | 비-오너 계정으로 허용 요청·거부 요청을 각각 테스트, 안 쓰는 역할·키 정기 정리 |

<div class="oa-eli">
  <span class="oa-eli__head">🧩 ChatGPT Sites의 도구 접근</span>
  <p><b>방문자 연결 도구</b>는 방문자 <b>본인의</b> 연결된 앱을 본인 권한 안에서 읽어요(제작자 연결이 아님, 예약 작업은 재사용 불가). <b>Site 호스팅 플러그인(MCP 도구)</b>은 Site 자체 데이터나 승인된 외부 API를 다뤄요. 리뷰에선 각 동작이 <b>누구의 데이터</b>를 바꾸고 결과를 <b>누가</b> 받는지 구분해요.</p>
</div>

## 3. 행동·승인 경계

API가 **정보를 만드는 것**에서 **시스템을 바꾸는 것**으로 넘어가는 순간 리스크가 달라져요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🟢</span><span class="oa-card__title">허용 (Allowed)</span>내부 요약 초안 생성 · 직원 검토용 분류 · 다음 단계 제안 · 원본을 바꾸지 않는 플래그</div>
  <div class="oa-card"><span class="oa-card__icon">🟡</span><span class="oa-card__title">승인 필요 (Approval-only)</span>요약을 원본에 기록 · 우선순위·상태·담당자 변경 · 다른 큐로 라우팅 · 에스컬레이션 · 고객용 문구 준비 · 파일럿 밖 확대</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔴</span><span class="oa-card__title">금지 (Disallowed)</span>고객 메시지 자동 발송 · 티켓 자동 종료 · 원본 삭제·덮어쓰기 · 계정 상태 변경 · 미승인 데이터 소스 사용</div>
</div>

모든 승인 필요 행동엔 **승인 담당자**(시스템·데이터·워크플로우 소유자, 보안·개인정보 검토자 등)와 **감사 증거**(요청 ID, 원본 레코드 ID, 호출자, 검토자, 승인 결정, 시각, 실행된 행동, 에러, 에스컬레이션 경로)가 있어야 해요.

### "우회할 수 없는" 승인 게이트

<div class="oa-eli">
  <span class="oa-eli__head">🚪 쉽게 말하면</span>
  <p>"선생님 허락 받고 나가세요"라고 칠판에 써 놔도, 뒷문이 열려 있으면 그냥 나갈 수 있죠. 진짜 통제는 <b>문을 잠그는 것</b>이에요.</p>
  <p>Agents API의 호스팅 브라우저(computer use)는 새 웹사이트 origin 접근 전에 사용자 승인을 받아요. 하지만 그건 "사이트 방문" 승인일 뿐, <b>티켓 저장마다의 승인</b>을 강제하지 않아요. 확인용 함수 도구를 만들어도 에이전트가 웹사이트에서 직접 저장할 수 있다면 막지 못해요. 웹페이지에 적힌 지시도 신뢰할 수 없는 콘텐츠라 권한을 줄 수 없어요.</p>
</div>

해결책: 호스팅 브라우저가 **저장할 수 없는 자원으로만 제한**하거나, 애플리케이션이 통제하는 브라우저 런타임에서 **검토자가 그 특정 변경을 승인할 때까지 쓰기를 차단**. 리뷰엔 **차단하는 구성요소 이름**과 다른 도구·API 경로로도 우회 불가하다는 설명, 그리고 증명할 테스트가 있어야 해요.

계획된 테스트 예: 비운영 환경의 가상 티켓으로 **승인 전 저장 시도** → 차단 구성요소가 막고 → 티켓 필드 전후 비교로 "바뀌지 않음" 확인. (세션 완료나 승인 기록만으로는 실제로 무엇이 저장됐는지 알 수 없어요.) 실행 전엔 planned로 표시하고, 막을 구성요소가 없으면 **쓰기를 비활성화한 채** 설계를 다시 해요.

### 나중에 통제가 강제될 위치

| 통제 필요 | 나중에 강제될 곳 | 리뷰에 적을 것 |
|---|---|---|
| 호출자 인증 | API 게이트웨이, 백엔드, IdP | 누가 호출하나, 인증 패턴 담당자 |
| 인가 | 앱 로직, IAM, RBAC, 정책 규칙 | 필요한 접근, 과도한 접근, 운영 범위 승인자 |
| Secret 보호 | 관리형 secret 저장소, CI/CD 주입 | 어디 저장, 환경 분리, 회전·폐기 담당 |
| 로깅 경계 | 로깅 규칙, 미들웨어, 로그 파이프라인 필터 | 남길 것 / 남기면 안 될 것 / 검증자 |
| 승인 게이트 | 워크플로우 오케스트레이션, 검토 UI, 큐 | 무엇을, 누가, 승인 증거는 |
| 금지 행동 | 허용 목록, 도구 레지스트리, 권한 거부 | 이번 릴리스에서 막는 것, 재검토 담당 |
| 에스컬레이션 | 보안·개인정보·법무·플랫폼 프로세스 | 트리거, 수신자, 일시정지/조건부 진행 |

## 4. 감사 가능성 — "다 로그"가 아니라 "맞는 증거만"

로그는 디버깅·감사에 유용하지만 **새로운 노출 면**이 될 수 있어요. 운영 증거와 민감 콘텐츠를 **분리**해요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 남길 것 (운영 증거)</span>
    <ul>
      <li><code>request_id</code>, <code>caller_id</code>, <code>environment</code>, <code>endpoint</code></li>
      <li><code>source_record_id</code>, <code>action</code>, <code>approval_status</code></li>
      <li><code>status_code</code>, <code>latency_ms</code>, <code>error_category</code>, <code>timestamp</code></li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🚫 남기지 말 것 (또는 마스킹)</span>
    <ul>
      <li>원본 프롬프트·티켓 전문·내부 메모</li>
      <li>API 키·토큰·자격 증명</li>
      <li>고객 이메일 등 개인정보</li>
      <li>민감 정보가 섞일 수 있는 생성 응답</li>
    </ul>
  </div>
</div>

주의: 상태 200 + <code>approval_status: pending</code>은 **"초안이 만들어졌다"**는 뜻이지 승인되거나 티켓에 쓰였다는 증거가 아니에요. 승인된 업데이트를 증명하려면 **연결된 승인 결정 + 실행 결과**가 필요해요. 식별자도 민감 레코드와 연결될 수 있으니 접근·보존 기간은 소유자가 정해요.

<div class="oa-eli">
  <span class="oa-eli__head">💧 textGrain 텍스트 워터마크</span>
  <p>지원 모델에서 켜면 단어 선택에 통계적 패턴을 남겨요(숨은 문자 아님). 감지되면 "OpenAI 모델이 생성·처리했을 가능성"만 시사할 뿐, <b>저자·수정 여부·내용의 사실성은 알려 주지 않아요.</b> 짧거나 많이 고쳐진 글은 신호가 안 잡힐 수 있고, 감지기 접근은 승인된 연구·학술 기관으로 제한돼요. 그러니 워터마크가 <b>요약을 원본 티켓과 대조하는 검토를 대신할 수 없어요.</b></p>
</div>

## 5. 보완 조치와 준비도 추천

각 이슈마다: **무엇을 바꾸나 · 누가 고치나 · 고친 뒤 어떤 증거로 확인하나**.

<div class="oa-vs">
  <div class="oa-vs__bad"><span class="oa-vs__label">❌ 막연한 보완</span><ul><li>"로깅 개선"</li><li>"보안 나중에 검토"</li></ul></div>
  <div class="oa-vs__good"><span class="oa-vs__label">✅ 실행 가능한 보완</span><ul><li>"앱 로그에서 원본 티켓 본문 제거, 요청 ID·시각·엔드포인트·상태·지연·에러 분류만 유지, 스테이징에서 마스킹 검증, 로그 접근 담당자 지정"</li></ul></div>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 120" role="img" aria-label="준비도 추천 4단계">
  <defs><style>.l{font-size:14px;font-weight:700;fill:#4a4a4a}.d{font-size:11px;fill:#6b6b6b}</style></defs>
  <rect x="10" y="20" width="165" height="80" rx="12" fill="#e6f5e3" stroke="#cfe8c9"/>
  <text x="92" y="50" text-anchor="middle" class="l">🟢 Ready</text><text x="92" y="72" text-anchor="middle" class="d">차단 갭 없음</text>
  <rect x="190" y="20" width="165" height="80" rx="12" fill="#fff3c4" stroke="#f1e4b8"/>
  <text x="272" y="50" text-anchor="middle" class="l">🟡 조건부 Ready</text><text x="272" y="72" text-anchor="middle" class="d">통제·담당·검증 문서화 시</text>
  <rect x="370" y="20" width="165" height="80" rx="12" fill="#ffe6d6" stroke="#f3cfc6"/>
  <text x="452" y="50" text-anchor="middle" class="l">🟠 출시 전 보완</text><text x="452" y="72" text-anchor="middle" class="d">차단 이슈 수정 필수</text>
  <rect x="550" y="20" width="160" height="80" rx="12" fill="#fde0dc" stroke="#f3b9ae"/>
  <text x="630" y="50" text-anchor="middle" class="l">🔴 Pause/Escalate</text><text x="630" y="72" text-anchor="middle" class="d">상위 의사결정 필요</text>
</svg>
</div>

| 추천 | 전형적인 상황 |
|---|---|
| **Ready** | 데이터 경계 명확, 접근 범위 한정, secret 승인, 행동 경계 정의, 로그에 민감 정보 없음 |
| **Ready with conditions** | 내부·제한 범위인데 로그 검토 담당·보존 기간 등 특정 항목만 확인 필요 |
| **Remediate before launch** | 로그에 민감 데이터, 과도한 권한, 위험한 secret 처리, 시스템 변경에 승인 게이트 없음, 운영 접근 불명확, 데이터 소유자 미정 |
| **Pause or escalate** | 관할별 데이터 문제, 고객 정책 불명확, 규제 데이터 불확실, 민감한 자동 행동, "이걸 해도 되나" 자체가 불확실 |

### 예시: 약한 리뷰 고쳐 쓰기

강의의 연습 상황 — 파일럿 직전의 티켓 분류 API인데: 전체 요청·응답 로깅, 공유 서비스 계정이 모든 티켓 읽기·쓰기, 토큰이 로컬 설정 파일에, 승인 담당 없음, **브라우저 데모에 개발자 개인 OpenAI 키 노출**, RBAC·키 권한·회전 담당 미검토.

<div class="oa-vs">
  <div class="oa-vs__bad"><span class="oa-vs__label">❌ 원래 리뷰</span><ul><li>"지원 티켓 데이터 사용. 서비스 계정으로 접근 통제. 로그 활성화. 사람 검토 필요. <b>파일럿 준비 완료.</b>"</li></ul></div>
  <div class="oa-vs__good"><span class="oa-vs__label">✅ 고친 리뷰 (요지)</span><ul><li>필드 단위 데이터 명시, 민감도 <b>미확인 → 소유자 검토</b></li><li>클라이언트 노출 키 = <b>차단 이슈</b> → 백엔드 경유, 프로젝트 범위 서비스 계정, 키 회전</li><li>전체 로깅 제거·마스킹 검증, 권한 축소, 환경 분리, 승인 담당 지정</li><li>추천: <b>Remediate before launch</b></li></ul></div>
</div>

"Agents API로 옮기자(ZDR 유지)" → Agents API는 ZDR 비적격이라 요구 미충족. "워터마크로 요약 검증을 대신하자" → 사실성 증거가 아님. 둘 다 위의 접근·자격 증명 문제를 해결하지도 못해요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. "지원 티켓만 쓰니까 보안 리뷰는 필요 없다"(전체 요청·응답 로깅 중)는 주장에 대한 가장 좋은 대응은?</summary>
  <p><b>정답:</b> 배포 전에 <b>데이터 이동·민감도·소유자·최소화</b>를 매핑한다. (지원 티켓은 보통 저위험이라 수용 ✗ / 테스트에서 동작하면 승인 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 유효한 서비스 계정으로 인증에 성공했다. 배포 전에 여전히 필요한 질문은?</summary>
  <p><b>정답:</b> 이 서비스 계정은 <b>무엇에 접근하고 무엇을 할 수 있도록 인가</b>되어 있나? (인증 ≠ 인가)</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 내부 요약 생성 + 고객 레코드 자동 업데이트. 승인 담당과 승인 증거가 정의되지 않았다. 가장 안전한 다음 단계는?</summary>
  <p><b>정답:</b> 승인 담당자와 증거가 정의될 때까지 <b>자동 업데이트를 차단</b>한다. (로그가 있으니 허용 ✗ / 로깅 전부 제거 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 승인된 테스트 티켓, 안전한 자격 증명, 환경 분리, 업데이트 전 직원 승인, 로그 민감정보 제외까지 갖췄다. 감사 이벤트 보존 기간과 주간 로그 검토 담당만 미정. 추천은?</summary>
  <p><b>정답: Ready with conditions.</b> 모든 빈칸이 차단 이슈는 아니에요.</p>
</details>

## 정리

- 보안 리뷰는 **데이터 이동 전체 경로**에서 시작한다. 불확실한 민감도는 높게 잡고 소유자에게 넘긴다.
- OpenAI 플랫폼: **abuse monitoring 로그 ≠ application state.** MAM·ZDR은 켜져 있다고 가정하지 말고, 엔드포인트별 적격성을 확인한다.
- **인증 ≠ 인가.** 키는 백엔드에만, 워크로드는 프로젝트 범위 서비스 계정, 권한은 역할 × 키 두 겹으로 최소화.
- 행동은 **허용 / 승인 필요 / 금지**로 나누고, 승인 게이트는 **우회 불가한 구성요소**로 강제한다.
- 로그는 운영 증거만. 보완 조치는 구체적으로, 추천은 **Ready · 조건부 · 출시 전 보완 · Pause** 중 증거에 맞게.
