---
title: "API Deployment Practice Application"
title_ko: "종합 실전 — Northstar 사례로 '배포해도 될까?' 판단하기"
course: openai-api-deployment
lesson: 11
---

## 학습 목표

- 실제와 비슷한 기업 API 배포 사례를 읽고 **확인된 증거·가정·열린 질문**을 구분하기
- staging 결과가 **무엇을 증명하고, 무엇을 증명하지 못하는지** 설명하기
- 아키텍처·계약·역량 선택·보안·데이터·검증·관측성·운영 준비도의 **갭 진단**하기
- 다섯 가지 결정(진행 / 조건부 진행 / 출시 전 보완 / 중단·재설계 / 에스컬레이션) 중 하나를 고르기
- 증거·담당자·다음 행동이 담긴 **API Deployment Practice Recommendation** 작성하기

<div class="oa-eli">
  <span class="oa-eli__head">🎯 이 레슨의 한 문장</span>
  <p>고객이 묻습니다: <b>"staging에서 API가 잘 돌아요. 이제 출시해도 되죠?"</b> 이 레슨은 앞선 10개 레슨의 도구를 모두 꺼내 이 질문에 답하는 <b>졸업 시험 같은 사례 연습</b>이에요.</p>
</div>

## 한 장으로 보는 코스 전체 지도

10개 레슨은 결국 **"출시해도 되나?"라는 하나의 판단**을 위한 체크포인트예요.

<div class="oa-svg">
<svg viewBox="0 0 720 360" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="레슨 1~10이 설계, 보호, 검증, 운영, 확장 다섯 묶음으로 모여 최종 배포 판단으로 이어지는 지도">
  <g font-size="12">
    <rect x="10" y="10" width="220" height="150" rx="14" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="120" y="34" text-anchor="middle" font-size="14" font-weight="700" fill="#c99400">① 설계 — 무엇을 만들까</text>
    <text x="24" y="62" fill="#4a4a4a">L01 최소 API · 증거 습관</text>
    <text x="24" y="86" fill="#4a4a4a">L02 아키텍처 브리프</text>
    <text x="24" y="110" fill="#4a4a4a">L03 API 계약 · 인터페이스</text>
    <text x="24" y="134" fill="#4a4a4a">L04 모델 · 역량 선택</text>

    <rect x="250" y="10" width="220" height="150" rx="14" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="360" y="34" text-anchor="middle" font-size="14" font-weight="700" fill="#c99400">② 보호 — 무엇을 지킬까</text>
    <text x="264" y="62" fill="#4a4a4a">L05 보안 · 데이터 · 접근</text>
    <text x="264" y="86" fill="#4a4a4a">L06 컨텍스트 · 검색(RAG)</text>
    <text x="264" y="118" fill="#8a8a8a">승인 소스, 권한 경계,</text>
    <text x="264" y="136" fill="#8a8a8a">로그 · 보관 · 마스킹</text>

    <rect x="490" y="10" width="220" height="150" rx="14" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="600" y="34" text-anchor="middle" font-size="14" font-weight="700" fill="#c99400">③ 검증 — 정말 안전한가</text>
    <text x="504" y="62" fill="#4a4a4a">L07 프롬프트 · eval ·</text>
    <text x="504" y="80" fill="#4a4a4a">      가드레일 · 사람 검토</text>
    <text x="504" y="118" fill="#8a8a8a">정상 케이스 + 경계 ·</text>
    <text x="504" y="136" fill="#8a8a8a">실패 · 에스컬레이션 케이스</text>

    <rect x="10" y="180" width="220" height="100" rx="14" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="120" y="204" text-anchor="middle" font-size="14" font-weight="700" fill="#c99400">④ 운영 — 지켜볼 수 있나</text>
    <text x="24" y="232" fill="#4a4a4a">L08 DevOps · 관측성 ·</text>
    <text x="24" y="250" fill="#4a4a4a">      프로덕션 준비</text>

    <rect x="250" y="180" width="220" height="100" rx="14" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="360" y="204" text-anchor="middle" font-size="14" font-weight="700" fill="#c99400">⑤ 확장 — 꼭 필요한가</text>
    <text x="264" y="232" fill="#4a4a4a">L09 실시간 · 음성 · 멀티모달</text>
    <text x="264" y="250" fill="#4a4a4a">L10 Deep Research · 이미지 · 증류</text>

    <rect x="490" y="180" width="220" height="100" rx="14" fill="#fff3c4" stroke="#e9c75c"/>
    <text x="600" y="210" text-anchor="middle" font-size="15" font-weight="700" fill="#5a4500">L11 배포 판단</text>
    <text x="600" y="234" text-anchor="middle" fill="#5a4500">증거 → 갭 → 결정</text>
    <text x="600" y="256" text-anchor="middle" fill="#5a4500">→ 담당자 → 다음 행동</text>
  </g>
  <g stroke="#c99400" stroke-width="2" fill="none">
    <path d="M230 230 L245 230"/><path d="M470 230 L485 230"/><path d="M600 160 L600 175"/>
  </g>
  <g fill="#c99400"><path d="M485 225 L493 230 L485 235Z"/><path d="M595 175 L600 183 L605 175Z"/></g>
  <text x="360" y="320" text-anchor="middle" font-size="13" fill="#4a4a4a">관통하는 규칙: <tspan font-weight="700">"작동하는 API 경로를 준비된 배포로 착각하지 말 것"</tspan></text>
  <text x="360" y="342" text-anchor="middle" font-size="11" fill="#8a8a8a">L01의 Evidence Pack 습관(증명한 것 / 못한 것)이 L11의 최종 추천서로 자라납니다</text>
</svg>
</div>

| 레슨 | 산출물 | Northstar에서 쓰이는 곳 |
|---|---|---|
| [01 API 배포 기초](../01-api-deployment-foundations/) | Evidence Pack | staging 증거 "증명한 것 / 못한 것" 구분 |
| [02 아키텍처](../02-api-solution-architecture/) | Architecture Brief | 업무·경계·검색 vs 액션 판단 |
| [03 API 계약](../03-api-contracts-and-core-interfaces/) | Contract Plan | 누락·구버전·지역·제한 소스별 응답 정의 |
| [04 모델·역량 선택](../04-model-and-capability-selection/) | Selection Rationale | 검색 기반 텍스트 + 구조화 출력이 맞는 이유 |
| [05 보안·데이터](../05-api-security-data-handling-and-access/) | Security Review | 역할·지역 검증, 로그 마스킹·보관 |
| [06 검색](../06-context-data-and-retrieval/) | Retrieval Plan | 승인·권한 기반 소스 인벤토리 |
| [07 eval·가드레일](../07-prompts-evals-guardrails-and-human-review/) | Validation Plan | 42개 테스트의 빈틈 |
| [08 운영 준비](../08-devops-observability-and-production-readiness/) | Readiness Checklist | fallback·에스컬레이션 지표, 롤백, 지원 담당 |
| [09 멀티모달](../09-realtime-voice-and-multimodal/) · [10 특화 패턴](../10-specialized-api-pattern-fit/) | Blueprint · Fit Assessment | "이번 파일럿엔 **필요 없음**" 판단 |

## 사례: Northstar Retail Group (가상의 글로벌 리테일러)

<div class="oa-eli">
  <span class="oa-eli__head">🏫 쉽게 말하면</span>
  <p>학교에 "규정 도우미 챗봇"을 만든다고 해 볼게요. 교칙·급식·도서관 규정을 물으면 답해 주죠. 연습 때 자주 묻는 질문엔 잘 답했어요. 그런데 <b>3학년만 해당되는 규정</b>을 1학년에게 알려 주거나, <b>상담실 비공개 문서</b>를 인용하거나, 질문 내용을 누구나 볼 수 있는 기록장에 적어 둔다면? "연습 때 잘 됐어요"만으로는 개학날 공개할 수 없겠죠.</p>
</div>

**요청**: 직원들이 HR·출장·복리후생·IT 정책 질문에 승인된 사내 문서로 빠르고 일관된 답을 받는 **Employee Policy API**를 파일럿으로 출시하고 싶다. 바쁜 복리후생 시즌 전에 하고 싶다는 **일정 압박**도 있음.

### 증거를 세 칸으로 나누기

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">✅</span><span class="oa-card__title">확인된 증거</span>staging에서 실행 · health check · 정책 답변 endpoint · 흔한 질문 35개 통과 · 요청 수·지연 추적 · 첫 릴리스는 <b>읽기 전용</b>(티켓 생성·기록 변경 없음)</div>
  <div class="oa-card"><span class="oa-card__icon">🤔</span><span class="oa-card__title">합리적 가정</span>포털이 사용자를 인증할 것 · 승인된 핸드북·IT 정책은 최신일 것 · 검색 기반 패턴이 업무에 맞을 것</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">❓</span><span class="oa-card__title">열린 질문</span>역할·지역 검증 방법 · 로그 마스킹·보관·접근 · 지역별 소스 승인 · 제한 문서 처리 · 지원·롤백 담당</div>
</div>

> 흔한 실수 두 가지: **staging 성공 = 배포 준비**로 보기, 그리고 반대로 **빈칸 하나마다 전체 재설계**로 가기.

## 1. 아키텍처·계약·선택 갭

현재 요청: `question`, `user_id`, `user_role`, `region`, `channel` → 응답: `answer`, `source_titles`, `confidence_label`, `next_step`.

**패턴 자체는 맞아요.** 승인 문서 기반의 검색 + 짧은 구조화 답변은 직원 셀프서비스에 적합해요. 문제는 **위험한 경우의 동작이 정의되지 않은 것**이에요.

| 정책 케이스 | 지금의 빈칸 | 계약 개선 예 |
|---|---|---|
| 소스 없음 | 무엇을 반환할지 미정 → 추측 위험 | `missing_source` + fallback/에스컬레이션 경로 |
| 구버전 소스 | 제목만 있고 승인·버전 정보 없음 | `source_status`, `source_version` |
| 지역별 정책 | 지역이 답에 어떻게 반영됐는지 안 보임 | `region_scope`, 지역 누락·충돌 시 동작 |
| 역할별 정책 | 역할 적용 여부가 응답에 없음 | 역할 적용 범위·정책 한계 필드 |
| 제한 콘텐츠 | 거절·에스컬레이션 정의 없음 | `restricted_content`, `escalation_route` |
| 모호한 질문 | 언제 되물을지 미정 | 구조화된 clarification 프롬프트 |
| 범위 밖 질문 | out-of-scope 동작 없음 | `unsupported_request` + 안전한 설명 |
| 검색 실패 | 시스템 오류가 정책 답변 문제처럼 보임 | 에러 상태 + 사용자 안전 fallback |

<div class="oa-eli">
  <span class="oa-eli__head">🏷️ confidence_label 주의</span>
  <p>이 값은 <b>보정된 모델 신뢰도 점수가 아니라</b> Northstar가 정의할 업무 라벨이에요. 누가 붙이는지, 각 값의 근거는 무엇인지, 어떤 포털·사람 리뷰 동작으로 이어지는지 출시 전에 정해야 해요.</p>
</div>

### 이번 파일럿에 **넣지 않을** 것들 (강의 기준)

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 유지: 읽기 전용 정책 답변</span>
    <ul>
      <li>검색 기반 텍스트 응답 + 구조화 출력 + fallback</li>
      <li>백엔드가 검색 → 답변 → 반환을 직접 조율</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⛔ 근거 없음 → 제외</span>
    <ul>
      <li>실시간·음성·시각·리서치·특화 최적화</li>
      <li>Decisions: 라벨(예: "출장")만으론 "이 호텔 예약해도 돼?"에 답 못 함</li>
      <li>Agents API 관리형 세션: 조율 주체·상태 저장이 바뀌는데 필요성 증거 없음 (강의 기준 US 데이터 레지던시만, ZDR 비대상)</li>
      <li>자동 티켓 생성 → 별도 출시 결정 필요</li>
    </ul>
  </div>
</div>

## 2. 보안·데이터·거버넌스 갭

"내부용이니까 안전하다"는 착각이에요. 이 API는 직원 ID·역할·지역·질문 원문·검색된 정책 조각·생성 답변·로그를 모두 다뤄요.

<ol class="oa-flow">
  <li><strong>입력</strong>역할·지역을 호출 앱이 주는 대로 <b>신뢰</b> → 서버 검증 없음</li>
  <li><strong>검색</strong>지역·용도별로 승인 안 된 소스가 섞임</li>
  <li><strong>출력</strong>승인·지역 적용·제한 여부가 안 보여 권위 있게 읽힘</li>
  <li><strong>로그</strong>질문 원문·사용자 ID·스니펫·답변 저장, 마스킹·보관·접근 <b>미확정</b></li>
</ol>

### 소스 인벤토리 → 파일럿 처리

| 소스 | 상태 | 파일럿 처리 |
|---|---|---|
| HR Employee Handbook v4.1 | 승인·최신 | ✅ 포함 (담당·버전·리뷰 주기 확인) |
| IT Acceptable Use Policy v2.8 | 승인·최신 | ✅ 포함 |
| Travel and Expense Policy v3.2 | 미국·캐나다 승인, **영국 업데이트 대기** | 🟡 승인 지역만. 영국 질문은 fallback/사람 연결 |
| Benefits Enrollment Guide 2026 | 미국 승인, 캐나다 검토 중, 영국 불명확 | 🟡 승인된 지역판만. 캐나다·영국은 답하지 않음 |
| Employee Relations Investigation Procedure | **제한** | ⛔ 검색에서 제외, 또는 HR Legal 승인된 권한 게이트로 에스컬레이션 |

> "문서가 존재한다" ≠ "API가 써도 된다." 가장 큰 통제 갭은 설정 하나가 아니라 **사용자 컨텍스트 × 소스 권한 × 로깅**의 조합이에요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__title">🔑 로그인 ≠ 권한</span>ChatGPT 로그인 같은 새 진입로가 생겨도, 그 사람이 <b>어느 지역·역할의 어떤 정책</b>을 볼 수 있는지는 Northstar가 따로 검증해야 해요.</div>
  <div class="oa-card"><span class="oa-card__title">🗄️ ZDR + PSP 제안 (강의 기준)</span>안전 모니터링 데이터 처리 방식일 뿐, Northstar 앱이 직접 쓰는 <b>원본 로그 문제는 그대로</b> 남아요.</div>
  <div class="oa-card"><span class="oa-card__title">🧩 새 인터페이스 제안</span>Plugin Extensions·MCP Events·Sites·호스팅 브라우저 등은 모두 <b>평가할 제안</b>이지 이번 파일럿에 승인된 추가 기능이 아니에요.</div>
</div>

## 3. 검증·관측성·운영 갭

### 42개 테스트, 숫자보다 "무엇을 빠뜨렸나"

<div class="oa-bars">
  <div class="oa-bar"><span>흔한 질문 통과</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:83.3%"></span><span class="oa-bar__val">35 / 42</span></span></div>
  <div class="oa-bar"><span>모호한 질문 — fallback 들쭉날쭉</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:9.5%"></span><span class="oa-bar__val">4 / 42</span></span></div>
  <div class="oa-bar"><span>제한·지역 케이스 — 답하면 안 되는데 답함</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:7.1%"></span><span class="oa-bar__val">3 / 42</span></span></div>
  <p class="oa-bars__caption">사례 파일의 실제 테스트 결과. 그 밖에 관리자 전용 정책·제한 직원관계 문서·누락/구버전/충돌 소스는 <b>테스트 자체가 없음</b>.</p>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🔇 조용한 실패</span>
  <p>health check는 "가게 문이 열려 있나"만 알려 줘요. 잘못된 지역 정책으로 답하거나, 제한 주제에 자신 있게 답하거나, 에스컬레이션을 놓쳐도 <b>서버는 멀쩡히 200을 돌려줘요.</b> 그래서 답변 품질을 보는 신호가 따로 필요해요.</p>
</div>

| 있는 신호 | 없는 신호 |
|---|---|
| health check, 요청 수, 지연 | 검색 실패율·소스 누락률·소스 버전 |
| | fallback 비율과 이유, 에스컬레이션·사람 연결 비율 |
| | 지역·역할별 오류 패턴, 제한 주제 시도 |
| | 릴리스 버전, 비용·사용량·rate limit·quota 압박 |
| | 알림 임계값, 장애 대응, 롤백·비활성화, 지원 담당 |

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__title">⚡ Ultrafast 같은 처리 모드 (강의 기준)</span>생성 속도만 다뤄요. 비교하려면 모델·프롬프트·소스·권한·테스트·합격 기준을 고정하고 <b>합격 답변까지의 총시간·총비용</b>으로. 승인 안 된 정책으로 빨리 답하면 여전히 불합격.</div>
  <div class="oa-card"><span class="oa-card__title">💧 텍스트 워터마킹 (강의 기준)</span>"AI가 썼는지" 출처를 다룰 뿐, <b>정책이 맞는지</b>는 해결 못 해요. 짧은 답은 탐지도 어렵고, 탐지기 접근도 제한적. 실패한 fallback 테스트는 그대로 고쳐야 해요.</div>
</div>

### 출시 게이트 현황

| 게이트 | 상태 |
|---|---|
| 확장된 eval 커버리지 | 🔴 미완료 |
| 가드레일·fallback 검증 | 🔴 미완료 |
| 소스 담당자 승인 | 🟡 일부 지역·제한 소스 미완료 |
| 로그 통제 | 🔴 미확정 |
| 역할·지역 처리 | 🔴 미증명 |
| 관측성 | 🟡 부분적 |
| 지원 경로 | 🔴 담당자 없음 |
| 장애 대응·롤백 | 🔴 미제시 |
| 알려진 한계 문서화 | 🔴 미완료 |

> **지원 담당자가 없으면, 출시의 결과를 책임질 사람도 없다.**

## 4. 최종 결정 — 다섯 가지 중 하나

<div class="oa-svg">
<svg viewBox="0 0 720 210" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="다섯 가지 결정 중 Northstar에 맞는 것은 출시 전 보완">
  <g font-size="12" text-anchor="middle">
    <rect x="8" y="20" width="132" height="120" rx="12" fill="#fff5f3" stroke="#f3cfc6"/>
    <text x="74" y="48" font-weight="700" fill="#4a4a4a">Proceed</text>
    <text x="74" y="70" fill="#6b6b6b">진행</text>
    <text x="74" y="104" fill="#b0563a">✗ 블로커 다수</text>
    <rect x="150" y="20" width="132" height="120" rx="12" fill="#fff5f3" stroke="#f3cfc6"/>
    <text x="216" y="48" font-weight="700" fill="#4a4a4a">Proceed with</text>
    <text x="216" y="64" font-weight="700" fill="#4a4a4a">conditions</text>
    <text x="216" y="86" fill="#6b6b6b">조건부 진행</text>
    <text x="216" y="114" fill="#b0563a">✗ 아직 갭이</text>
    <text x="216" y="130" fill="#b0563a">통제·소유 안 됨</text>
    <rect x="292" y="8" width="136" height="144" rx="12" fill="#fff3c4" stroke="#c99400" stroke-width="2.5"/>
    <text x="360" y="40" font-weight="700" fill="#5a4500" font-size="13">Remediate</text>
    <text x="360" y="58" font-weight="700" fill="#5a4500" font-size="13">before release</text>
    <text x="360" y="82" fill="#5a4500">출시 전 보완</text>
    <text x="360" y="116" fill="#2f6b2a" font-weight="700">✓ Northstar 정답</text>
    <rect x="438" y="20" width="132" height="120" rx="12" fill="#fff5f3" stroke="#f3cfc6"/>
    <text x="504" y="48" font-weight="700" fill="#4a4a4a">Pause &amp;</text>
    <text x="504" y="64" font-weight="700" fill="#4a4a4a">redesign</text>
    <text x="504" y="86" fill="#6b6b6b">중단·재설계</text>
    <text x="504" y="114" fill="#b0563a">✗ 핵심 패턴은</text>
    <text x="504" y="130" fill="#b0563a">타당함</text>
    <rect x="580" y="20" width="132" height="120" rx="12" fill="#fffdf5" stroke="#f1e4b8"/>
    <text x="646" y="48" font-weight="700" fill="#4a4a4a">Escalate</text>
    <text x="646" y="70" fill="#6b6b6b">에스컬레이션</text>
    <text x="646" y="104" fill="#8a6d00">△ 팀이 소스·로그·</text>
    <text x="646" y="120" fill="#8a6d00">권한을 못 풀면</text>
  </g>
  <text x="360" y="190" text-anchor="middle" font-size="12" fill="#4a4a4a">유용한 업무 + 실제 staging 진전 → 패턴은 지키고, 블로커를 고친 뒤 파일럿에 노출</text>
</svg>
</div>

### 추천서 구조 — 이렇게 쓰면 실행도 되고 방어도 돼요

| 섹션 | Northstar 예시 (요약) |
|---|---|
| 고객 상황 | 직원 정책 셀프서비스, 복리후생 시즌 전 일정 압박, staging에서 흔한 질문 동작 |
| 결정 | **Remediate before release** |
| 증거 위치 | 확인: staging·endpoint·35개 통과 / 가정: 포털 인증 / 미확인: 역할·지역 검증, 로그 통제 |
| 준비도 갭 | 계약 실패 동작, 지역 소스 승인, 제한 문서, 역할·지역 신뢰, 로그, eval, 관측성, 지원 |
| 범위·제외 | 읽기 전용 답변만. 티켓 생성·기록 변경·음성·시각·리서치·특화 최적화 제외 |
| 담당·다음 행동 | 보안·플랫폼(검증·로그), 소스 담당자(승인), HR Legal(제한 문서), 검증 담당(eval), HR Ops·지원(운영) |
| 결정을 바꿀 증거 | 보완된 실패 케이스와 회귀 테스트의 **재통과**, 릴리스·소스 버전과 연결된 증거, 나머지 게이트 충족 |

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">⬆️ "조건부 진행"으로 바뀌려면</span>
    <ul>
      <li>소스 담당 승인 + 제한 소스 제외</li>
      <li>안전한 로그 통제 + 역할·지역 검증</li>
      <li>확장된 eval + 관측성 신호</li>
      <li>출시 승인 + 지원 담당 지정 (제한된 파일럿)</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⚠️ "에스컬레이션"으로 바뀌려면</span>
    <ul>
      <li>제한 소스·로그·개인정보·지역 정책 문제를</li>
      <li>실무 팀 권한으로 해결할 수 없을 때</li>
    </ul>
  </div>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Northstar 사례에서 "확인된 증거"는?</summary>
  <p><b>정답:</b> staging의 health check와 흔한 질문용 정책 답변 endpoint. (최종 프로덕션 승인 ✗ / 모든 직원 데이터 로그 보관·마스킹 승인 ✗ / 미국·캐나다·영국 모든 복리후생 질문에 안전 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 파일럿 출시 안전성에 가장 직접 영향을 주는 설계 갭은?</summary>
  <p><b>정답:</b> 누락·구버전·지역별·제한 정책 콘텐츠에 대한 동작이 정의되지 않은 것. (자동 티켓 생성 없음 ✗ / 음성 인터페이스 없음 ✗ / 시각 워크플로우 없음 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 무조건적 파일럿 출시를 가장 강하게 막아야 할 엔터프라이즈 블로커는?</summary>
  <p><b>정답:</b> 직원 데이터에 대한 로그 통제가 확정되지 않은 것. (health check 있음, 내부 전용, 티켓 생성 제외는 오히려 긍정적이거나 중립적인 사실)</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 검증·관측성·지원 담당 갭이 있는 상황의 가장 강한 준비도 결정은?</summary>
  <p><b>정답:</b> <b>Remediate before release</b> — 사용자 노출 전 블로커. ("관측성은 출시 후 추가하면 된다"는 조건부 진행 ✗)</p>
</details>

<details class="oa-quiz">
  <summary>Q5. 최종 추천으로 가장 강한 것은?</summary>
  <p><b>정답:</b> 블로커별 <b>담당자를 지정한</b> Remediate before release. (일정 압박 때문에 조건부 진행 ✗ / 실시간 음성으로 재설계 ✗)</p>
</details>

## 정리 — 코스 전체에서 가져갈 것

<ol class="oa-flow">
  <li><strong>업무 필요부터</strong>기능이 아니라 문제</li>
  <li><strong>계약·역량 확인</strong>실패 동작까지 정의</li>
  <li><strong>데이터·접근 보호</strong>권한 · 로그 · 소스</li>
  <li><strong>동작 검증</strong>정상 + 경계 + 실패</li>
  <li><strong>운영 가시화</strong>조용한 실패를 보이게</li>
  <li><strong>증거만큼만 추천</strong>담당자 + 다음 행동</li>
</ol>

- **작동하는 API 경로 ≠ 준비된 배포.** staging 성공은 유용한 증거지만 출시 허가가 아니다.
- 증거는 **확인 / 가정 / 열린 질문** 세 칸으로 나눈다.
- 결정은 다섯 가지 중 하나로 명시하고, **블로커·담당자·다음 행동·결정을 바꿀 증거**를 함께 적는다.
- 필요 없는 복잡도(음성·시각·리서치·자동 액션)는 블로커를 풀어 주지 않는다 — 오히려 늘린다.
