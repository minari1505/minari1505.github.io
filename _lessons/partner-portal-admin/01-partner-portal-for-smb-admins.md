---
title: "OpenAI's Partner Portal for SMB Admins"
title_ko: "파트너 포털 관리자 가이드 — 가입부터 딜 등록·티어·사용자 관리까지"
course: partner-portal-admin
lesson: 1
---

## 학습 목표

- 파트너 포털 **계정을 만드는 절차**와 가입 시 주의점(회사 도메인 이메일, Partner ID, 역할) 설명하기
- 포털의 주요 메뉴 — **Overview · Customers · Program · Investments · Enablement · Marketing · Support · Admin · Onboarding** — 가 각각 무슨 일을 하는지 말하기
- **Co-sell / Deployment 등록(registration)** 이 무엇이고, 좋은 등록서에 무엇을 담아야 하는지 이해하기
- **티어 대시보드**로 파트너 등급(Registered → Select → Advanced → Elite) 진행 상황을 읽기
- **Admin 역할**만 할 수 있는 일(사용자·역할 관리, 계약서 확인)을 구분하기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>계정 만들기</strong>Sign Up → 이메일 인증 → 비밀번호 설정</li>
  <li><strong>Overview</strong>공지·변경 사항이 모이는 첫 화면</li>
  <li><strong>Customers</strong>영업 기회 확인 + 딜 등록</li>
  <li><strong>Program</strong>티어 진행 상황·검증 레터</li>
  <li><strong>Enablement·Marketing</strong>교육 자료·배지·이벤트 리드</li>
  <li><strong>Admin·Onboarding</strong>사용자·역할·계약·기술 평가</li>
</ol>

이 코스의 한 줄 요약: **파트너 포털은 OpenAI와 파트너가 "누가, 어떤 고객과, 어디까지 왔는지"를 함께 보는 공용 작업대이고, 관리자는 그 작업대의 열쇠 관리인이다.**

<div class="oa-eli">
  <span class="oa-eli__head">🏫 쉽게 말하면</span>
  <p>파트너 포털은 <b>학교 학생회 사무실</b>과 비슷해요. 게시판(Overview)에는 공지가 붙고, 장부(Customers)에는 "우리 반이 어떤 행사를 맡았는지"를 적어 두고, 성적표(Program)로 우리 동아리 등급이 어디쯤인지 봐요. 그리고 <b>사무실 열쇠를 누구에게 줄지 정하는 사람</b>이 바로 관리자(Admin)예요.</p>
</div>

> 참고: 강의 본문 텍스트는 Home · Learn · Transact · Admin 탭이라는 예전 이름을 쓰고, 영상 데모는 Overview · Customers · Program 등 새 메뉴 구조를 보여 줘요. 이 글은 **영상 데모의 메뉴 구조(강의 기준)** 를 따릅니다.

## 1단계 — 계정 만들기

<ol class="oa-flow">
  <li><strong>Sign Up</strong>로그인 화면에서 가입 시작</li>
  <li><strong>기본 정보</strong>이름·회사 이메일·전화번호</li>
  <li><strong>Partner ID · Role</strong>조직에 받은 ID, Sales/Technical 중 선택</li>
  <li><strong>Create</strong>제출 후 인증 메일 확인(스팸함도!)</li>
  <li><strong>Finish Onboarding</strong>메일 버튼 → 비밀번호 설정 → 포털 입장</li>
</ol>

| 입력 항목 | 주의할 점 |
|---|---|
| 이메일 | **회사 도메인 이메일**을 써야 해요. 개인 메일(gmail 등)은 안 돼요. |
| Partner ID | 우리 조직에 미리 발급된 ID (예: `<PARTNER_ID>`) |
| Role | Sales 또는 Technical 중 내 업무에 **가장 가까운 것** 선택 |
| 인증 메일 | "Welcome to the OpenAI Partner Portal" 제목 — 스팸함으로 갈 수 있어요 |

<div class="oa-eli">
  <span class="oa-eli__head">🔑 왜 회사 이메일이어야 할까?</span>
  <p>회사 출입증을 개인 이름표로 만들 수 없는 것과 같아요. 포털은 <b>"이 사람이 어느 파트너 회사 소속인지"</b>를 이메일 도메인으로 확인하거든요. 그래야 같은 회사 사람끼리 한 조직으로 묶여요.</p>
</div>

## 2단계 — Overview와 Customers: 공지 보기, 기회 확인하기

**Overview(첫 화면)** 에는 최신 뉴스, 포털·프로그램 변경 사항처럼 "꼭 봐야 할 것"이 모여요. 아직 "Coming Soon"인 메뉴가 열리면 여기서도 알려 주니, 여기저기 눌러 볼 필요가 줄어요.

**Customers** 아래에는 Accounts · Opportunities · Use Cases · Registrations가 있어요 (일부는 Coming Soon, 강의 기준).

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📈</span><span class="oa-card__title">Opportunities</span>내가 <b>co-sell(함께 판매)</b> 또는 <b>deployment(구축)</b>로 기여 중인 영업 기회 목록. 고객사, 영업 단계, 예상 마감일을 볼 수 있어요.</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📝</span><span class="oa-card__title">Registrations</span>"우리가 이 고객과 이런 일을 하고 있어요"라고 OpenAI에 <b>먼저 알리는</b> 등록서를 내는 곳</div>
</div>

### 딜 등록(Registration) — 먼저 손 들기

<div class="oa-eli">
  <span class="oa-eli__head">🙋 쉽게 말하면</span>
  <p>조별 과제에서 "이 주제는 우리 조가 할게요!"라고 <b>미리 손을 드는 것</b>이에요. 먼저 알려 두면 선생님(OpenAI 영업팀)이 겹치지 않게 조율하고, 도와줄 수도 있죠.</p>
</div>

등록 유형은 두 가지예요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🤝 Co-sell 등록</span>
    <ul>
      <li>OpenAI 영업팀과 <b>함께 판매</b>하는 고객</li>
      <li>영업 단계에서의 협업을 알림</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🛠️ Deployment 등록</span>
    <ul>
      <li>고객 환경에 <b>실제로 구축·배포</b>하는 일</li>
      <li>구현 단계에서의 협업을 알림</li>
    </ul>
  </div>
</div>

등록서에 담는 내용 (Submit Registration → Co-sell / Deployment 선택):

| 구역 | 담는 내용 |
|---|---|
| 고객 | 기존 고객 검색 또는 **신규 고객**(이름·주소·국가), MSA(기본 계약) 여부 |
| Use case | AI 전환 영역, 논의된 OpenAI 제품, 경쟁 상황, 단계, 예상 운영 시작일, 예상 연간 매출(ARR) |
| 설명·세부 | **최대한 자세히** — 영업·파트너십 팀이 상황을 이해할수록 협업이 쉬워져요 |
| 첨부 | 보조 자료 업로드 가능 |
| 이해관계자 | 실명·이메일 대신 **직책**(예: CEO, CTO)만 적어도 충분 |

<svg class="oa-svg" viewBox="0 0 720 150" role="img" aria-label="등록서 제출 후 검토 경로">
  <rect x="10" y="50" width="150" height="50" rx="10" fill="#e8f0fe" stroke="#7aa0e0"/>
  <text x="85" y="80" text-anchor="middle" font-size="14">등록서 제출</text>
  <line x1="160" y1="75" x2="250" y2="75" stroke="#888" stroke-width="2"/>
  <rect x="250" y="10" width="220" height="50" rx="10" fill="#f4fbf3" stroke="#8cc084"/>
  <text x="360" y="40" text-anchor="middle" font-size="13">전담 파트너 디렉터 있음 → 담당자 검토</text>
  <rect x="250" y="90" width="220" height="50" rx="10" fill="#fff8e6" stroke="#e0b860"/>
  <text x="360" y="120" text-anchor="middle" font-size="13">전담 없음 → 파트너십 팀 검토</text>
  <line x1="470" y1="75" x2="560" y2="75" stroke="#888" stroke-width="2"/>
  <rect x="560" y="50" width="150" height="50" rx="10" fill="#e8f0fe" stroke="#7aa0e0"/>
  <text x="635" y="80" text-anchor="middle" font-size="13">영업팀과 조율</text>
</svg>

제출된 등록서는 제출자·상태·등록 유형·제출일 등으로 **필터링**해서 추적할 수 있어요.

## 3단계 — Program: 우리 회사의 파트너 등급 보기

<div class="oa-eli">
  <span class="oa-eli__head">🎮 쉽게 말하면</span>
  <p>게임의 <b>랭크 시스템</b>과 같아요. 브론즈 → 실버 → 골드처럼, 파트너도 <b>Registered → Select → Advanced → Elite</b>로 올라가요. 랭크를 올리려면 "점수(매출)"와 "업적(배지)"이 둘 다 필요해요.</p>
</div>

<ol class="oa-flow">
  <li><strong>Registered</strong>처음 시작</li>
  <li><strong>Select</strong>요건 충족 시</li>
  <li><strong>Advanced</strong>더 높은 매출·배지</li>
  <li><strong>Elite</strong>최상위</li>
</ol>

**티어 대시보드**에서 볼 수 있는 것 (강의 기준):

| 항목 | 의미 |
|---|---|
| 프로그램 기간 | 1년 단위 (데모: 7월 1일 ~ 다음 해 6월 30일) |
| 평가 시작일 · 비례 적용(proration) | 중간에 합류하면 요건이 기간에 비례해 줄어요 (예: 6개월 뒤 합류 → 50%) |
| All-up revenue | co-sell·deployment로 함께 만든 매출 |
| PartnerU badges | 구성원들이 딴 교육 배지 수 |
| Specializations | 곧 요건에서 **제외 예정**이라고 안내됨 |
| 현재 티어 / 다음 티어 토글 | 지금 등급 유지 상황과 다음 등급까지 거리 비교 |

<div class="oa-bars">
  <div class="oa-bar"><span>매출 요건</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:40%"></span><span class="oa-bar__val">진행 중</span></span></div>
  <div class="oa-bar"><span>배지 요건</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:65%"></span><span class="oa-bar__val">진행 중</span></span></div>
  <div class="oa-bar"><span>Specializations</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:5%"></span><span class="oa-bar__val">요건에서 빠질 예정</span></span></div>
  <p class="oa-bars__caption">개념적 표현이에요 (실제 수치 아님). 티어 유지·승급에는 매출과 배지가 핵심이라는 점만 기억하세요.</p>
</div>

**Partner Validation Letter** 버튼을 누르면 "우리는 OpenAI의 ○○ 티어 파트너다"라는 **확인서를 직접 발급**받을 수 있어요. 고객이나 협력사에 증빙할 때 유용해요.

**Investments** 메뉴는 앞으로 자격이 되는 파트너의 펀딩 요청·지급 상태를 보여 줄 예정이에요 (강의 기준).

## 4단계 — Enablement · Marketing · Support

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎓</span><span class="oa-card__title">OpenAI Academy</span>고객용 학습 자료. 제품 전반을 폭넓게 배우기 좋아요.</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🏅</span><span class="oa-card__title">PartnerU</span><b>파트너 전용</b> 교육. 제품별 배지(보안, Codex, ChatGPT, API 등)를 따는 곳</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">Agora</span>영업 덱, 제품 업데이트, 공지 같은 정적 자료 모음</div>
</div>

- **Badges**: 회사가 딴 배지, 배지 트랙, 개인별 완료·진행 상태를 보고 CSV로 내보낼 수 있어요. 배지는 **12개월 뒤 만료**되니 "곧 만료" 필터가 쓸모 있어요.
- **Marketing**: OpenAI와 공동 이벤트를 하면 참석 리드 목록이 올라와요. 리드에서 바로 **Create registration**을 누르면 이벤트에서 모은 정보가 미리 채워진 등록서가 시작되고, 인연이 없으면 **Disqualify**로 목록에서 지워요.
- **Support**: 아직 미오픈(강의 기준). 그동안은 파트너 지원 메일(`<partner-support 메일 주소>`)로 문의해요.

## 5단계 — Admin: 관리자만 할 수 있는 일

포털의 역할은 **Sales · Technical · Admin** 세 가지예요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">👥 Sales · Technical</span>
    <ul>
      <li>업무 성격을 나타내는 라벨에 가까움</li>
      <li>교육, 딜 등록, 기회·use case 보기 등 <b>기능 접근은 곧 동일해질 예정</b></li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🔐 Admin</span>
    <ul>
      <li>Admin 탭 접근 — <b>권한이 가장 큼</b></li>
      <li>사용자 목록 보기·필터링, 역할 변경</li>
      <li>관리자 수 제한 없음 (필요한 만큼 지정 가능)</li>
    </ul>
  </div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🗝️ 관리자 = 열쇠 관리인</span>
  <p>열쇠를 아무에게나 복사해 주면 편하지만 위험하죠. Admin은 <b>누가 들어와 있고, 누가 어떤 역할인지</b>를 최신으로 유지하는 사람이에요. 퇴사자나 역할이 바뀐 사람을 정리하는 것도 관리자 몫이에요.</p>
</div>

**Agreements** 영역에는 최신 파트너 네트워크 개요, 정책 가이드, 새 계약 조건이 **버전 날짜와 함께** 올라와요. 바뀌면 Overview에서도 알려 줘요.

## 6단계 — Onboarding · Quick links · My profile

**Onboarding 탭 — 기술 평가(technical assessment)**: 아직 온보딩을 마치지 않은 Registered 파트너는 AI 솔루션 구축 경험(고객 규모, 구현한 use case 등)을 묻는 양식을 내요.

<ol class="oa-flow">
  <li><strong>양식 제출</strong>구축 경험 서술</li>
  <li><strong>1~5점 채점</strong>OpenAI가 점수 부여</li>
  <li><strong>결과</strong>통과 / 불통과 / 피드백과 함께 반려</li>
  <li><strong>재제출</strong>최대 3번까지</li>
</ol>

- **Quick links**: Co-sell 등록, Deployment 등록, Training(PartnerU · Agora · Academy)으로 바로 가는 지름길
- **My profile**: 이름 수정, **training email**(회사를 옮겨도 배지를 이어 가기 위한 보조 이메일) 추가, 파트너 소식 수신 설정 on/off

## 확인 문제

이 코스에는 별도의 지식 체크가 없어서, 강의 내용으로 복습 문제를 만들었어요.

<details class="oa-quiz">
  <summary>Q1. 파트너 포털 가입 시 이메일로 알맞은 것은?</summary>
  <p>① 개인 gmail ② 회사 도메인 이메일 ③ 아무 이메일이나 ④ 팀 공용 메일</p>
  <p><b>정답: ②</b> — 회사 도메인 이메일이어야 같은 파트너 조직으로 묶여요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 등록서의 이해관계자 칸에는 무엇을 적으면 충분할까?</summary>
  <p><b>정답:</b> 실명·이메일이 아니라 <b>직책</b>(CEO, CTO 등). OpenAI가 자기 쪽 담당자와 비교할 수 있을 정도면 돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 티어 유지·승급의 핵심 요건 두 가지는? (곧 빠지는 것 하나도)</summary>
  <p><b>정답:</b> <b>매출(all-up revenue)</b>과 <b>PartnerU 배지</b>. Specializations는 요건에서 제외될 예정이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Sales·Technical과 Admin 역할의 가장 큰 차이는?</summary>
  <p><b>정답:</b> Admin만 Admin 탭에서 <b>사용자·역할을 관리</b>하고 계약서 영역을 볼 수 있어요. Sales와 Technical은 기능 접근이 곧 같아질 예정이에요.</p>
</details>

## 정리

- 가입은 **회사 이메일 + Partner ID + 역할** → 인증 메일 → 비밀번호 설정.
- **Overview**는 공지판, **Customers**는 기회 확인과 **co-sell / deployment 등록**의 자리.
- 등록서는 자세할수록 좋고, 이해관계자는 **직책**만 적어도 된다.
- **Program** 티어 대시보드로 매출·배지 진행을 보고, 확인서를 직접 발급받는다.
- **Admin**은 사용자·역할·계약을 최신으로 유지하는 열쇠 관리인. 배지는 12개월마다 만료되니 Badges 화면을 자주 확인하자.
