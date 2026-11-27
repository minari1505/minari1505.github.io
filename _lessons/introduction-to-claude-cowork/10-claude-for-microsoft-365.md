---
title: "Claude for Microsoft 365"
title_ko: "Claude for Microsoft 365"
course: introduction-to-claude-cowork
lesson: 10
---

## 학습 목표

- **Word, Excel, PowerPoint, Outlook** 안에서 Claude가 하는 일 알기
- 하나의 Claude 대화로 **앱 간에 작업 넘기기**
- Cowork에서 할지, 문서 안에서 할지 판단하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>Outlook</strong>브리프가 도착해요</li>
  <li><strong>Word</strong>회사 템플릿으로 memo 시작</li>
  <li><strong>Excel</strong>memo 뒤의 숫자 모델</li>
  <li><strong>PowerPoint</strong>고객 템플릿의 덱으로</li>
  <li><strong>다시 Outlook</strong>회의 초대 초안</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Claude가 문서 안에 살고, 하나의 대화가 앱에서 앱으로 맥락을 날라요.**

## 1. 핵심

<div class="oa-eli">
  <span class="oa-eli__head">✍️ 쉽게 말하면</span>
  <p>대필 작가에게 원고를 메일로 보냈다 받는 게 아니라, <b>작가가 내 옆자리에 앉아 내가 열어 둔 바로 그 문서에 같이 손을 대는 것</b>이에요. 복사해서 붙여 넣는 왕복이 사라져요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📄</span><span class="oa-card__title">Claude가 문서 안에 살아요</span>Word, Excel, PowerPoint, Outlook에 add-in으로 나타나, 열려 있는 파일과 공간에서 작업해요</div>
  <div class="oa-card"><span class="oa-card__icon">🔀</span><span class="oa-card__title">하나의 대화가 맥락을 날라요</span>Excel에서 분석을 만들어 PowerPoint에 넘겨 슬라이드로. Word memo를 소스 삼아 Outlook 답장 초안으로</div>
  <div class="oa-card"><span class="oa-card__icon">🧭</span><span class="oa-card__title">다른 순간을 위한 다른 도구</span>Cowork는 여러 소스에서 완성 결과물을 만들고, M365 안의 Claude는 이미 작업 중인 파일을 다듬고 · 디버깅하고 · 형태를 잡고, 그 파일의 맥락을 다른 문서로 날라요</div>
</div>

## 2. 앱별로 하는 일

<div class="oa-eli">
  <span class="oa-eli__head">🛠️ 쉽게 말하면</span>
  <p>같은 조수가 <b>방마다 다른 도구</b>를 들어요. 계산실(Excel)에서는 계산기, 발표장(PowerPoint)에서는 디자인 자, 서재(Word)에서는 빨간 펜, 우편실(Outlook)에서는 편지 분류함을 들어요.</p>
</div>

| 앱 | Claude가 하는 일 | 강력한 한 수 |
|---|---|---|
| **Excel** | 데이터 분석, 수식 작성, `#REF!` 오류 · 순환 참조 디버깅, 모델을 안 깨고 시나리오 테스트, 템플릿에서 시트 생성, 특정 셀 인용 | "Q3 시트의 실적을 같은 워크북의 Q3 계획과 비교해, F열에 항목별 분산 코멘트를 써줘." |
| **PowerPoint** | 슬라이드 마스터 · 폰트 · 색 구성을 읽고 거기에 맞춰 슬라이드 생성. **네이티브 · 편집 가능 차트**(붙여 넣은 이미지 아님). 선택한 슬라이드에서 작동 | "방금 Excel에서 한 분석을 우리 덱 템플릿으로 QBR용 3슬라이드 덱으로 만들어줘." |
| **Word** | 제자리에서 초안 · 수정 · 재포맷. 코멘트 · 변경 내용 추적(tracked changes)과 작업. 연결 소스에서 맥락을 끌어와 초안을 근거화 | "이 memo 본문과 appendix에서 참조한 source data를 바탕으로 executive summary 초안을 써줘." |
| **Outlook** | 나머지 작업 맥락으로 수신 메일 분류. 이전 스레드 · 캘린더 · 최근 결정을 반영한 답장 초안 | (강의에 별도 예시 없음) |

## 3. 하나의 대화로 앱 간 이동

<div class="oa-eli">
  <span class="oa-eli__head">🏃‍♀️ 쉽게 말하면</span>
  <p><b>이어달리기</b>예요. 주자(앱)는 바뀌어도 바통(맥락)은 그대로 넘어가요. 다음 주자가 "무슨 일이었죠?"라고 처음부터 다시 묻지 않아요.</p>
</div>

앱 간 이동이야말로 M365 + Claude가 단일 앱 작업과 다르게 느껴지는 지점이에요. 문서 하나에서만 일하는 게 아니라, 그 맥락을 다음 문서로 가져가요.

| 이동 | 내가 하는 말 | 일어나는 일 |
|---|---|---|
| **Outlook → Word** | "이 브리프를 Word에서 열고 우리 회사 템플릿으로 memo 시작해줘" | Word가 첨부를 연 채, 이메일 스레드를 사이드바에 이미 로드한 채 실행돼요. Claude는 보낸 사람이 무엇을 요청했는지 이미 알아요 |
| **Word → Excel** | "옵션 2의 시장 규모 모델을 만들어줘" | Excel이 열리고, Claude가 방금 Word에서 읽은 브리프의 가정을 끌어와, 확인할 수 있는 수식이 든 다중 탭 모델을 만들어요 |
| **Excel → PowerPoint** | "이걸 고객 템플릿의 운영위(steering-committee) 덱으로 만들어줘" | 슬라이드 마스터 위에, Excel의 최신 숫자가 든 네이티브 · 편집 가능 차트로 덱이 만들어져요 |
| **다시 Outlook** | "목요일 전 팀과 30분 잡아줘" | 참석자가 포함된 초대 초안이 만들어지고, 내가 Send를 누르기를 기다려요 |

## 4. M365 vs Cowork, 언제 무엇을

<div class="oa-eli">
  <span class="oa-eli__head">🏗️ 쉽게 말하면</span>
  <p>Cowork는 여러 자재상에서 재료를 모아 <b>집을 짓는 시공팀</b>이고, M365 안의 Claude는 지어진 집 안에서 <b>벽지를 고치고 가구를 옮기는 인테리어 담당</b>이에요. 대부분의 집은 둘 다 필요해요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🤝 Cowork를 쓸 때</span>
    <ul>
      <li>여러 소스에서 끌어와 <b>결과물로 끝나는</b> 작업</li>
      <li>20개 소스 파일로 브리프 만들기</li>
      <li>CRM + Slack 3채널로 보고서 조립</li>
      <li>일정에 맞춰 워크플로 실행</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">📄 M365 안의 Claude를 쓸 때</span>
    <ul>
      <li><b>Office 파일 자체</b>에서 작업할 때</li>
      <li>제자리 편집</li>
      <li>한 앱에서 다음 앱으로 맥락 나르기</li>
    </ul>
  </div>
</div>

대부분의 실제 작업은 **둘 다** 써요.

<ol class="oa-flow">
  <li><strong>Cowork</strong>덱 첫 초안을 만들어요</li>
  <li><strong>PowerPoint</strong>슬라이드 안에서 Claude와 다듬어요</li>
  <li><strong>Cowork</strong>데이터를 모델로 끌어와요</li>
  <li><strong>Excel</strong>눈에 띈 이상치를 Claude에게 조사시켜요</li>
</ol>

두 surface는 깔끔하게 넘겨받아요. 어느 쪽에서 열든 **파일은 같은 파일**이에요.

## 5. 지금 해보기

| 할 일 | 메모 |
|---|---|
| **add-in 설치** | 가장 많이 쓰는 M365 앱용 Claude add-in을 설치해요. 설정과 plan별 제공 여부는 *Work across Excel, PowerPoint, and Word* 글을 참고해요 |
| **Outlook은 따로** | Claude for Outlook은 **별도의 beta**이고 listing도 따로 있어요. *Claude for Outlook* 글을 참고해요 |
| **관리형 환경이면** | add-in을 IT 팀이 관리한다면 IT에 확인해요 |
| **한 가지 동작 시도** | 이번 주 실제 문서로 한 가지를 해 봐요: 문단 다듬기, 수식 디버깅, Word 구절로 슬라이드 만들기 |

Claude가 내가 보고 있는 바로 그 문서 위에서 움직이는 걸 처음 보면, "chat에서 만들고 붙여 넣기"와의 차이가 와닿아요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Claude는 M365 앱 안에 어떤 형태로 들어가나요?</summary>
  <p><b>정답: add-in.</b> Word, Excel, PowerPoint, Outlook 안에 나타나 열려 있는 파일에서 작업해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. PowerPoint에서 Claude가 만드는 차트는 붙여 넣은 이미지인가요?</summary>
  <p><b>정답: 아니요.</b> 네이티브이고 편집 가능한 차트예요. 슬라이드 마스터 · 폰트 · 색 구성도 읽고 맞춰요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Salesforce와 Slack 3개 채널에서 정보를 모아 보고서를 조립해야 해요. Cowork와 M365 안의 Claude 중 무엇이 맞나요?</summary>
  <p><b>정답: Cowork.</b> 여러 소스에서 끌어와 결과물로 끝나는 작업이에요. 만들어진 파일을 제자리에서 다듬을 때는 M365 안의 Claude를 써요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Word에서 memo를 쓰다가 "옵션 2의 시장 규모 모델을 만들어줘"라고 하면 Excel은 가정을 어디서 가져오나요?</summary>
  <p><b>정답: Claude가 방금 Word에서 읽은 브리프에서.</b> 하나의 대화가 앱 간에 맥락을 나르기 때문에 다시 설명할 필요가 없어요.</p>
</details>

## 정리

- Claude는 Word · Excel · PowerPoint · Outlook 안에 **add-in**으로 들어가 열린 파일에서 작동해요.
- **하나의 대화가 앱 간 맥락을 날라**, Outlook → Word → Excel → PowerPoint → Outlook 같은 이동이 매끄러워요.
- **여러 소스 · 결과물이면 Cowork, 파일 자체를 다듬으면 M365**예요. 대부분은 둘을 함께 써요.
- 어느 surface에서 열든 **파일은 같은 파일**이에요.
- Claude for Outlook은 **별도의 beta**예요. add-in이 관리형이면 IT에 확인해요.

Module 4에서는 Cowork에 실제 일을 맡길 때 중요한 것들을 다뤄요: 안전하게 일하기, 만든 것 검증하기, 팀과 공유하기.
