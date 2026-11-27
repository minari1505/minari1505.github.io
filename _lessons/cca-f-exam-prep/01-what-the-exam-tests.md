---
title: "What the CCA-F Exam Tests"
title_ko: "CCA-F 시험은 무엇을 묻는가"
course: cca-f-exam-prep
lesson: 1
tags:
  - CCA-F
  - Certification
---

## 학습 목표

- CCA-F 시험의 형식(문항 수, 시간, 합격선, 채점 방식)을 알기
- 이 시험이 어떤 응시자를 겨냥하는지, 어떤 시나리오가 나오는지 파악하기
- 5개 출제 도메인과 배점을 외우고, 공부 시간을 어디에 쓸지 정하기
- 시나리오 문제를 풀 때 쓰는 두 가지 판단 기준을 익히기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>시험 형식</strong>60문항 · 120분 · 720점 합격</li>
  <li><strong>5개 도메인</strong>배점 27 · 18 · 20 · 20 · 15%</li>
  <li><strong>시나리오</strong>후보 중 4개가 무작위로 출제</li>
  <li><strong>판단 기준 2개</strong>결정적 해법 · 최소 비용의 근본 대책</li>
  <li><strong>공부와 응시</strong>12주 계획 → 모의고사 43/60</li>
</ol>

이 레슨을 한 문장으로 줄이면: **CCA-F는 "Claude를 써 봤는가"가 아니라 "이 상황에서 어떤 설계를 고를 것인가"를 60문항의 시나리오로 묻는 시험이에요.**

## 1. 어떤 시험이고, 누구를 위한 시험인가

<div class="oa-eli">
  <span class="oa-eli__head">🚗 쉽게 말하면</span>
  <p>운전면허 필기시험이 아니라 <b>도로 주행 시험</b>에 가까워요. 표지판 뜻을 외웠는지가 아니라 "비 오는 밤, 앞차가 급정거하면 어떻게 할래요?"를 물어요. 그래서 답은 늘 "이 상황에서는"이라는 조건과 함께 정해져요.</p>
</div>

정답을 외우는 시험이 아니에요. 선택지 대부분은 그럴듯한 오답이고, "이 조건에서 가장 알맞은" 답만 정답이에요. 그래서 왜 그게 정답이고 나머지가 오답인지 스스로 설명할 수 있어야 통과해요.

시험이 다루는 기술은 네 가지예요. 실무에서 Claude 기반 솔루션을 설계하고 구현할 때 생기는 trade-off를 판단할 수 있는지를 봐요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🤖</span><span class="oa-card__title">Claude Agent SDK</span>멀티 에이전트 오케스트레이션, 서브에이전트 위임, 툴 통합, 수명 주기 훅</div>
  <div class="oa-card"><span class="oa-card__icon">⌨️</span><span class="oa-card__title">Claude Code</span><code>CLAUDE.md</code>, MCP 서버, Agent Skills, 계획 모드</div>
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">MCP</span>백엔드 통합을 위한 tool과 resource</div>
  <div class="oa-card"><span class="oa-card__icon">📡</span><span class="oa-card__title">Claude API</span>요청 구조, Tool Use, Message Batches</div>
</div>

### 권장 응시자

커뮤니티 학습 가이드가 정리한 권장 응시자는 Claude 기반 프로덕션 애플리케이션을 설계하고 출시해 본 **솔루션 아키텍트**예요. 아래 영역에서 **6개월 이상 실무 경험**이 있으면 가장 잘 맞아요.

| 영역 | 구체적으로 |
|---|---|
| Claude Agent SDK | 멀티 에이전트 오케스트레이션, 서브에이전트 위임, 툴 통합, 수명 주기 훅 |
| Claude Code | `CLAUDE.md`, MCP 서버, Agent Skills, 계획 모드 |
| MCP | 백엔드 통합을 위한 tool과 resource |
| 프롬프트 엔지니어링 | JSON 스키마, few-shot 예시, 데이터 추출 템플릿 |
| 컨텍스트 윈도우 | 긴 문서 처리, 멀티 에이전트 간 컨텍스트 전달 |
| CI/CD 파이프라인 | 자동 코드 리뷰, 테스트 생성 |
| 에스컬레이션과 신뢰성 | 오류 처리, human-in-the-loop |

이 표는 그대로 **자가 점검표**로 쓸 수 있어요. 손대 본 적 없는 줄이 있으면 그 영역부터 실습해요.

## 2. 시험 형식과 채점

<div class="oa-eli">
  <span class="oa-eli__head">📝 쉽게 말하면</span>
  <p>긴 지문 하나에 문제가 여러 개 딸린 <b>수능 국어 비문학</b>과 닮았어요. 지문(시나리오)을 한 번 제대로 읽어 두면 딸린 문항을 연달아 풀 수 있고, 지문을 대충 읽으면 딸린 문항이 한꺼번에 흔들려요.</p>
</div>

| 항목 | 내용 |
|---|---|
| 정식 이름 | Claude Certified Architect – Foundations (CCA-F, 시험 코드 CCAR-F) |
| 문항 수 / 시간 | 60문항 / 120분 |
| 형식 | 시나리오 하나에 여러 문항이 붙는 구조. 정답 1개를 고르는 객관식과 여러 개를 고르는 복수 응답 문항이 섞여 나옴 |
| 시나리오 수 | 준비된 6개 중 4개가 출제. 어떤 4개인지는 응시할 때마다 다름 |
| 합격선 | 100~1,000점 척도에서 720점 (스케일 점수) |
| 오답 감점 | 없음. 무응답은 오답 처리 |
| 응시료 | USD 125 (2026년 6월 30일 이후 등록 기준) |
| 언어 | 영어 |
| 응시 방식 | 감독 시험. 온라인 프록터(OnVUE, 집에서 응시) 또는 시험장 |
| 유효 기간 | 12개월. 만료 전에 무료 비감독 평가로 갱신 |
| 재응시 대기 | 1차 불합격 후 14일, 2차 후 30일, 3차 후 90일 |

위 표는 공식 시험 가이드 v1.1(2026년 11월) 기준이에요. 세 가지를 짚어 둘게요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔢</span><span class="oa-card__title">복수 응답은 적힌 개수만큼</span>"두 개를 고르시오"라고 적혀 있으면 정확히 두 개를 골라요. 문항을 읽을 때 <b>고를 개수부터</b> 확인해요.</div>
  <div class="oa-card"><span class="oa-card__icon">⚖️</span><span class="oa-card__title">스케일 점수</span>"43문항 맞히면 합격"이 정확한 계산은 아니에요. 문항 난이도에 따라 가중치가 붙어요.</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">✍️</span><span class="oa-card__title">무응답은 오답</span>오답 감점이 없으니 모르는 문제도 소거법으로 하나는 반드시 골라요.</div>
</div>

스케일 점수에 대해 조금 더 적어 둘게요. 모의고사를 풀 때는 43/60(약 72%)을 자가 채점 기준으로 쓰되, 실전에서는 특정 도메인만 잘 보고 나머지를 버리는 전략이 통하지 않는다고 생각하면 돼요.

> **커뮤니티 가이드와 다른 점 두 가지.** 아래 "함께 보면 좋은 자료"의 커뮤니티 학습 가이드는 문제 유형을 "객관식(4개 중 정답 1개)"로만 적고 있고, 시나리오는 "가능한 8개 중 4개(무작위 선택)"라고 적고 있어요. 이 노트의 표(복수 응답 문항 포함, 6개 중 4개)와 달라요. 어느 쪽이 맞는지는 응시 직전에 공식 시험 가이드 최신판으로 확인하세요. 어느 쪽이든 준비 방법은 같아요. 문항마다 고를 개수를 확인하고, 시나리오는 가려서 준비하지 않아요.

## 3. 5개 도메인과 배점

<div class="oa-eli">
  <span class="oa-eli__head">🎒 쉽게 말하면</span>
  <p>다섯 과목 평균으로 합격이 정해지는 시험에서 배점이 낮은 과목 하나를 통째로 버리면, 나머지 과목에서 거의 만점을 받아야 해요. <b>배점이 큰 과목에 시간을 더 쓰되, 과락 과목은 만들지 않는 것</b>이 전략이에요.</p>
</div>

| 도메인 | 배점 | 60문항 기준 |
|---|---|---|
| 1. 에이전트 아키텍처와 오케스트레이션 | 27% | 약 16문항 |
| 2. 툴 설계와 MCP 통합 | 18% | 약 11문항 |
| 3. Claude Code 설정과 워크플로우 | 20% | 약 12문항 |
| 4. 프롬프트 엔지니어링과 구조화 출력 | 20% | 약 12문항 |
| 5. 컨텍스트 관리와 신뢰성 | 15% | 약 9문항 |

<div class="oa-bars">
  <div class="oa-bar"><span>1. 에이전트 아키텍처</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:27%"></span><span class="oa-bar__val">27%</span></span></div>
  <div class="oa-bar"><span>2. 툴 설계와 MCP</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:18%"></span><span class="oa-bar__val">18%</span></span></div>
  <div class="oa-bar"><span>3. Claude Code</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:20%"></span><span class="oa-bar__val">20%</span></span></div>
  <div class="oa-bar"><span>4. 프롬프트·구조화 출력</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:20%"></span><span class="oa-bar__val">20%</span></span></div>
  <div class="oa-bar"><span>5. 컨텍스트·신뢰성</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:15%"></span><span class="oa-bar__val">15%</span></span></div>
  <p class="oa-bars__caption">도메인별 배점이에요. 가장 낮은 도메인 5도 약 9문항이에요.</p>
</div>

도메인 1·3·4만 합쳐도 전체의 3분의 2예요. 다만 배점이 가장 낮은 도메인 5도 취약하면 그대로 9문항을 잃어요. 모든 도메인에서 "평균 이상"을 목표로 잡는 편이 안전해요.

이 코스의 레슨 2~7은 이 다섯 도메인을 순서대로 다뤄요. 이미 이 사이트의 다른 Anthropic 강의 정리에 있는 내용(API 기본 문법, MCP 서버 만들기, Claude Code 기본 조작 등)은 반복하지 않고, **시험이 판단을 묻는 지점**만 모았어요.

## 4. 어떤 시나리오가 나오나

<div class="oa-eli">
  <span class="oa-eli__head">🎲 쉽게 말하면</span>
  <p>시험장에서 <b>주제 카드 여러 장 중 4장을 뽑는</b> 방식이에요. 어떤 카드가 나올지 모르니 "이 카드는 안 나오겠지" 하고 건너뛸 수 없어요. 대신 카드마다 어떤 기술을 묻는지 알아 두면 지문을 읽는 속도가 달라져요.</p>
</div>

문항은 도메인별로 따로 나오지 않고 **시나리오에 묶여** 나와요. 준비된 시나리오 중 4개가 무작위로 뽑혀요. 아래는 커뮤니티 학습 가이드가 정리한 시나리오 목록이에요.

| # | 시나리오 | 무엇을 만드는 상황인가 | 쓰이는 도구·기술 |
|---|---|---|---|
| 1 | 고객 지원 에이전트 | 반품, 청구 분쟁, 계정 문제를 처리하는 에이전트. 목표는 적절한 에스컬레이션과 함께 **첫 문의 해결률 80% 이상** | Claude Agent SDK, MCP 툴(`get_customer`, `lookup_order`, `process_refund`, `escalate_to_human`) |
| 2 | Claude Code로 코드 생성 | 코드 생성, 리팩터링, 디버깅, 문서화를 빠르게 하기 | 사용자 지정 슬래시 명령, `CLAUDE.md` 구성, 계획 모드를 언제 쓸지 판단 |
| 3 | 멀티 에이전트 리서치 시스템 | 코디네이터가 웹 리서치, 문서 분석, 종합, 보고서 생성 서브에이전트에게 위임. **인용(citation)이 포함된** 완전한 보고서가 결과물 | 코디네이터-서브에이전트 위임, 컨텍스트 전달 |
| 4 | 개발자 생산성 도구 | 낯선 코드베이스 탐색, 보일러플레이트 생성, 반복 작업 자동화 | 내장 툴(Read, Write, Bash, Grep, Glob), MCP 서버 |
| 5 | CI를 위한 Claude Code | 자동 코드 리뷰, 테스트 생성, PR 피드백을 CI/CD 파이프라인에 통합 | **false positive를 줄이는** 프롬프트 설계 |
| 6 | 구조화 데이터 추출 | 비정형 문서에서 정보를 뽑고 높은 정확도 유지 | JSON 스키마 검증, 엣지 케이스 처리 |
| 7 | 대화형 AI 아키텍처 패턴 | 멀티턴 대화 시스템 설계 | 컨텍스트 윈도우 관리, 턴 간 지시 유지, 메모리 전략, 안전한 실행을 위한 툴 설계, 모호하거나 충돌하는 입력 처리 |
| 8 | 에이전틱 AI 도구 | 응시자 제보로 존재만 확인됨 | 가이드에 내용이 아직 정리되어 있지 않음 |

읽을 때 주의할 점이에요.

- **시나리오 개수는 출처마다 달라요.** 2번 섹션의 표는 "6개 중 4개"이고, 커뮤니티 가이드는 "8개 중 4개"로 적고 위 여덟 개를 나열해요. 8번은 가이드 스스로 "제보는 있지만 내용이 비어 있다"고 밝힌 항목이에요.
- **어떤 4개가 뽑힐지는 응시할 때마다 달라요.** 특정 시나리오만 골라 준비하는 전략은 통하지 않아요.
- 시나리오 안에 **목표 수치나 제약**(해결률 80%, false positive 최소화, citation 포함)이 적혀 있으면 그게 정답의 근거가 돼요. 7번 섹션 "시나리오 읽는 법"과 이어져요.

## 5. 시험 범위 밖인 것

공식 가이드가 범위 밖으로 명시한 주제가 있어요. 여기에 시간을 쓰지 않아도 돼요.

- 모델 파인튜닝
- 과금·인증 절차의 세부
- 벡터 DB 구현, 임베딩
- 스트리밍 구현 세부
- Computer Use, 이미지 분석
- Constitutional AI, RSP 같은 Anthropic 내부 기법
- 프롬프트 캐시의 구현 세부(1.25배, 10~20% 같은 숫자)

단, "개념"은 알아 둬야 해요. 예를 들어 프롬프트 캐시가 뭔지, RAG가 뭔지는 선택지에 등장해요. 구현 세부만 범위 밖이에요.

## 6. 문제를 풀 때 쓰는 두 가지 판단 기준

<div class="oa-eli">
  <span class="oa-eli__head">🚪 쉽게 말하면</span>
  <p>"출입 금지" 팻말은 부탁이고, <b>잠긴 문</b>은 보장이에요. 대부분은 팻말만으로 충분하지만 "절대 들어오면 안 되는" 방이라면 문을 잠가야 해요. 그렇다고 모든 방에 금고 문을 달 필요는 없어요. 이 두 가지 감각이 판단 기준 두 개예요.</p>
</div>

교재 저자가 실전에서 점수 차이가 가장 크게 벌어진다고 짚은 지점이에요. 시나리오 문제 대부분이 이 둘 중 하나로 갈려요.

### 기준 1. 확률적 해법 vs 결정적 해법

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🙏 확률적 해법 (부탁)</span>
    <ul>
      <li>프롬프트에 "~하지 마세요"라고 적어서 모델에게 부탁하기</li>
      <li>예: "배포 전 테스트를 반드시 통과시킬 것"을 <code>CLAUDE.md</code>에 문장으로 적기</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🔒 결정적 해법 (보장)</span>
    <ul>
      <li>훅, 코드 게이트, 스키마 검증으로 시스템이 막기</li>
      <li>예: 훅으로 배포 커맨드를 차단하기</li>
    </ul>
  </div>
</div>

"반드시", "절대", "규정상"이 붙은 요구가 있으면 결정적 해법이 정답이에요. 모델은 지시를 따를 확률이 높을 뿐, 100%는 아니기 때문이에요. 위 예시에서 "배포 전 테스트를 반드시 통과시켜야 한다"면 `CLAUDE.md`에 문장을 적는 것(A)보다 훅으로 배포 커맨드를 차단하는 것(B)이 정답이에요.

### 기준 2. 화려한 해법 vs 최소 비용의 근본 대책

| 상황 | 정답 쪽 | 오답 쪽 |
|---|---|---|
| 툴이 40개라서 선택 정확도가 떨어진다 | 툴 수를 줄인다 | 모델을 Opus로 바꾼다 |
| 단순 분류 100만 건 | Haiku | "최신 모델이니까 Opus" |
| 단일 도메인, 툴 10개짜리 작업 | 싱글 에이전트 | 멀티 에이전트 |

과잉 설계를 피하는 쪽이 정답인 경우가 많아요.

## 7. 시나리오 읽는 법

선택지를 보기 전에 시나리오에서 **제약 조건**을 먼저 찾아요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">💰</span><span class="oa-card__title">비용 vs 품질</span>비용 최우선인가, 품질 최우선인가</div>
  <div class="oa-card"><span class="oa-card__icon">⏱️</span><span class="oa-card__title">실시간 vs 배치</span>실시간 응답인가, 야간 배치인가</div>
  <div class="oa-card"><span class="oa-card__icon">🛡️</span><span class="oa-card__title">보안·규제</span>보안·규제 요구가 있는가</div>
  <div class="oa-card"><span class="oa-card__icon">📈</span><span class="oa-card__title">규모</span>트래픽 규모는 어느 정도인가</div>
</div>

이 조건이 곧 정답의 근거예요. 같은 "리뷰 분류" 시나리오라도 "비용 최우선"이면 Haiku, "판정 실수 비용이 크다"면 다른 답이 돼요.

선택지 문장에서는 두 가지를 조심해요.

- "가장 적절한", "가장 효과적인"이라는 표현. 여러 개가 맞아 보여도 조건에 가장 잘 맞는 하나를 고르라는 뜻이에요.
- "항상", "모든 경우에" 같은 절대 표현이 들어간 선택지. 오답일 확률이 높아요.

## 8. 시간 배분

60문항 120분이니 1문항당 평균 2분이에요. 다만 시나리오 지문을 읽는 시간이 앞쪽에 몰려요. 시나리오 하나(대략 10~15문항)를 20~30분 안에 끝낸다는 감각으로 가요.

<ol class="oa-flow">
  <li><strong>지문 먼저</strong>시나리오의 제약 조건을 찾기</li>
  <li><strong>1분 규칙</strong>1분 넘게 헤매는 문제는 표시하고 넘어가기</li>
  <li><strong>한 바퀴 뒤 복귀</strong>뒤 문항이 앞 문항의 힌트가 되는 경우가 있음</li>
  <li><strong>마지막 15분</strong>검토에 쓰기. 빈칸은 남기지 않기</li>
</ol>

뒤 문항이 앞 문항의 힌트가 되는 경우가 있어서, 한 바퀴 돈 뒤 돌아오면 풀리는 문제가 생겨요.

한국에서 응시한다면 영어 지문을 시간 안에 읽는 연습이 별도로 필요해요. 모의고사를 풀 때 시간을 재는 이유예요.

## 9. 공부 순서 (12주 모델)

교재가 제안하는 순서예요. 총 100시간 정도로 잡혀 있어요.

| 주차 | 내용 |
|---|---|
| 1~2주 | 환경 구축, Claude 기초, API 기초, 프롬프트 기초 |
| 3주 | 구조화 출력, Tool Use |
| 4~5주 | MCP 입문, MCP 설계, MCP 서버 구현 프로젝트 |
| 6~7주 | 에이전트 기초, 멀티 에이전트 |
| 8주 | 컨텍스트 관리, 신뢰성 |
| 9~10주 | Claude Code 기초, 확장(스킬·훅), CI/CD |
| 11주 | 멀티 에이전트 설계 프로젝트, 모의고사 절반 |
| 12주 | 모의고사 전체, 취약 도메인 복습, 응시 |

모의고사를 풀고 나서 다음 행동을 정하는 기준은 이래요.

| 모의고사 결과 | 다음 행동 |
|---|---|
| 43문항 이상 | 실전 응시 |
| 36~42문항 | 가장 낮은 도메인의 본문으로 돌아가 복습한 뒤 재응시 |
| 그 아래 | 전체를 한 번 더 |

가장 낮은 도메인이 배점 15%짜리 도메인 5라도 그 장으로 돌아가는 것이 맞아요. 배점 높은 도메인 1만 복습하고 응시하는 것은 오답 선택지예요.

## 함께 보면 좋은 자료

- 공식 시험 가이드 PDF: 출제 범위와 샘플 문제
- Anthropic Academy 코스: 이 사이트의 다른 강의 정리가 그 내용이에요
- [커뮤니티 학습 가이드 (한국어)](https://github.com/paullarionov/claude-certified-architect/blob/main/guide_ko.md): 공식 시험 가이드를 바탕으로 커뮤니티가 정리한 학습 가이드. 이론 정리와 해설이 붙은 연습 문제가 있어요. 공식 자료가 아니라서 이 노트와 숫자가 다른 곳이 있어요(위 2·4번 섹션 참고)
- claudecertifications.com: 무료 연습문제 25문항
- MCP 공식 사양: [modelcontextprotocol.io](https://modelcontextprotocol.io/)

커뮤니티 가이드가 꼽은 공식 문서를 기술별로 묶으면 이래요. 레슨을 읽다가 원문이 필요할 때 찾아가는 용도예요.

| 기술 | 공식 문서 |
|---|---|
| Claude API | [Messages](https://platform.claude.com/docs/en/api/messages) · [Tool Use](https://platform.claude.com/docs/en/build-with-claude/tool-use) · [Message Batches](https://platform.claude.com/docs/en/build-with-claude/message-batches) · [Prompt Engineering](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview) · [Extended Thinking](https://platform.claude.com/docs/en/build-with-claude/extended-thinking) |
| Claude Agent SDK | [Overview](https://platform.claude.com/docs/en/agent-sdk/overview) · [Hooks](https://platform.claude.com/docs/en/agent-sdk/hooks) · [Subagents](https://platform.claude.com/docs/en/agent-sdk/subagents) · [Sessions](https://platform.claude.com/docs/en/agent-sdk/sessions) |
| MCP | [Tools](https://modelcontextprotocol.io/docs/concepts/tools) · [Resources](https://modelcontextprotocol.io/docs/concepts/resources) · [Servers](https://modelcontextprotocol.io/docs/concepts/servers) |
| Claude Code | [Overview](https://code.claude.com/docs/en/overview) · [CLAUDE.md와 Memory](https://code.claude.com/docs/en/memory) · [Skills(슬래시 명령 포함)](https://code.claude.com/docs/en/skills) · [Hooks](https://code.claude.com/docs/en/hooks) · [Sub-agents](https://code.claude.com/docs/en/sub-agents) · [MCP 통합](https://code.claude.com/docs/en/mcp) · [GitHub Actions](https://code.claude.com/docs/en/github-actions) · [GitLab CI/CD](https://code.claude.com/docs/en/gitlab-ci-cd) · [Headless](https://code.claude.com/docs/en/headless) |
| 코드 예제 | [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook) |

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 금융사 시나리오에 "고객 계좌 정보는 규정상 절대 외부 API로 나가면 안 된다"는 조건이 있어요. 가장 알맞은 설계는?</summary>
  <p>① 시스템 프롬프트에 "계좌 정보를 외부로 보내지 마세요"를 굵게 적는다 ② few-shot 예시로 올바른 행동을 세 번 보여 준다 ③ 훅이나 코드 게이트로 외부 호출 인자를 검사해 차단한다 ④ 더 큰 모델로 바꾼다</p>
  <p><b>정답: ③</b> — "규정상 절대"는 보장이 필요한 요구예요. 프롬프트 지시는 따를 확률이 높을 뿐이라서 시스템이 막는 결정적 해법을 골라요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 모의고사에서 60문항 중 40문항을 맞혔고, 도메인 5(컨텍스트 관리와 신뢰성)가 가장 낮았어요. 다음 행동으로 가장 알맞은 것은?</summary>
  <p>① 기준에 가까우니 바로 응시한다 ② 배점이 가장 큰 도메인 1만 복습하고 응시한다 ③ 도메인 5 본문으로 돌아가 복습한 뒤 모의고사를 다시 본다 ④ 전체를 처음부터 다시 본다</p>
  <p><b>정답: ③</b> — 36~42문항 구간은 "가장 낮은 도메인 복습 후 재응시"예요. 배점이 15%여도 약 9문항이 걸려 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 종료 5분 전인데 표시해 둔 문제 세 개가 남았고, 그중 하나는 "두 개를 고르시오" 문항이에요. 어떻게 하는 것이 맞을까요?</summary>
  <p>① 확신이 없으니 비워 둔다 ② 소거법으로 세 문제 모두 답을 고르고, 복수 응답 문항은 정확히 두 개를 고른다 ③ 복수 응답 문항은 맞을 확률을 높이려고 세 개를 고른다 ④ 한 문제에 5분을 다 쓴다</p>
  <p><b>정답: ②</b> — 오답 감점이 없고 무응답은 오답으로 처리돼요. 복수 응답 문항은 문항에 적힌 개수만큼 골라요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 동료가 "나는 Claude Code만 쓰니까 고객 지원 에이전트나 멀티 에이전트 리서치 시나리오는 건너뛰겠다"고 해요. 무엇이 문제일까요?</summary>
  <p><b>정답: 시나리오는 준비된 후보 중 4개가 무작위로 출제되고, 어떤 4개인지는 응시할 때마다 달라요.</b> 건너뛴 시나리오가 뽑히면 그 시나리오에 딸린 문항을 통째로 잃어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q5. 에이전트에 툴 40개가 등록되어 있고 엉뚱한 툴을 고르는 일이 잦아요. 선택지 중 가장 알맞은 것은?</summary>
  <p>① 모델을 Opus로 올린다 ② 툴을 묶어 수를 줄이고 역할별로 나눈다 ③ 에이전트를 하나 더 붙여 선택 결과를 재검토하게 한다 ④ temperature를 올린다</p>
  <p><b>정답: ②</b> — 원인은 툴의 개수예요. 원인을 그대로 둔 채 더 비싼 모델이나 구성 요소를 얹는 것은 과잉 설계예요.</p>
</details>

## 정리

- 60문항 120분, 720/1000점 합격, 오답 감점 없음, 무응답은 오답. 영어.
- 권장 응시자는 Claude 기반 프로덕션 앱을 설계·출시해 본 솔루션 아키텍트(실무 6개월 이상). Agent SDK · Claude Code · MCP · Claude API가 범위예요.
- 도메인 1(27%)·3(20%)·4(20%)이 3분의 2, 도메인 5(15%)도 버릴 수 없어요.
- 시나리오는 후보 중 4개가 무작위로 나와요(이 노트 기준 6개, 커뮤니티 가이드 기준 8개). 가려서 준비하지 않아요.
- 판단 기준 두 개: "반드시"는 결정적 해법(훅·게이트·스키마), 과잉 설계보다 최소 비용의 근본 대책.
- 시나리오에서 제약 조건(비용/품질, 실시간/배치, 규제, 규모)을 먼저 찾고 선택지를 봐요. 모의고사 43/60이 응시 기준이고, 그 아래면 최저 도메인으로 돌아가요.
