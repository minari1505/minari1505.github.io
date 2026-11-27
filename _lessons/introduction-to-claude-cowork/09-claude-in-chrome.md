---
title: "Claude in Chrome"
title_ko: "Claude in Chrome"
course: introduction-to-claude-cowork
lesson: 9
---

## 학습 목표

- **Claude in Chrome**이 여는 작업의 종류 알기
- 실제 작업에서 Chrome과 Cowork를 함께 쓰기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>커넥터 없는 웹 도구</strong>대시보드 · 포털 · 로그인 뒤 앱</li>
  <li><strong>Chrome에서 읽고 조작</strong>Claude가 페이지를 직접 다뤄요</li>
  <li><strong>데이터를 Cowork로</strong>브라우저 결과를 넘겨받아요</li>
  <li><strong>결과물 만들기</strong>다른 맥락과 합쳐 폴더에 저장</li>
</ol>

이 레슨을 한 문장으로 줄이면: **"이 맥락을 Claude에게 주고 싶은데 웹에 있네" 싶을 때가 Claude in Chrome의 순간이에요.**

## 1. Claude in Chrome이란?

<div class="oa-eli">
  <span class="oa-eli__head">🌉 쉽게 말하면</span>
  <p>커넥터가 <b>전용 지하 통로</b>라면, Claude in Chrome은 통로가 없는 건물에 <b>정문으로 걸어 들어가는 것</b>이에요. 내가 화면에서 보는 것을 Claude도 보고, 내가 누르는 것을 Claude도 눌러요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🌉</span><span class="oa-card__title">커넥터 없는 도구의 다리</span>브라우저에 사는 것이라면 무엇이든 Claude가 그 페이지를 읽고 조작할 수 있어요. 웹사이트를 읽고, 클릭하고, 탐색해요</div>
  <div class="oa-card"><span class="oa-card__icon">🤝</span><span class="oa-card__title">Cowork와 함께 동작</span>Claude가 브라우저에서 정보를 모으고 행동한 뒤, 그 결과를 Cowork로 가져와 결과물을 만들어요. 하나의 대화, 두 surface</div>
  <div class="oa-card"><span class="oa-card__icon">🛂</span><span class="oa-card__title">통제권은 사용자에게</span>기본적으로 민감한 행동 전에 확인하고, 각 행동을 승인하거나 거부할 수 있어요</div>
</div>

## 2. 무엇을 열어 주나요?

<div class="oa-eli">
  <span class="oa-eli__head">🗝️ 쉽게 말하면</span>
  <p>지금까지는 화면을 보고 <b>숫자를 손으로 옮겨 적어야 했던 곳들</b>이에요. 사내 대시보드, 거래처 사이트, 로그인해야 보이는 페이지. 이제 그 옮겨 적는 일을 맡길 수 있어요.</p>
</div>

| 대상 | 예 | Claude가 하는 일 |
|---|---|---|
| **내부 대시보드** | 재무팀 Tableau, 세일즈 옵스 Looker, 매주 월요일 확인하는 지표가 있는 BI 도구 | 수치를 뽑아 컴퓨터에 내려받고, Cowork 작업의 맥락으로 써요 |
| **벤더 포털 · 고객 시스템** | API 없는 조달 포털, SSO 뒤의 CRM, 모든 티켓을 분류해야 하는 고객 지원 도구 | 포털을 탐색해 필요한 것을 뽑고, 사용자가 하듯이 행동해요 |
| **로그인 뒤 웹 앱** | 브라우저 인터페이스가 있는 것이라면 무엇이든(팀이 직접 만든 도구도) 스크립트화돼요 | "조달 시스템을 열어 Q3 상위 10개 공급업체의 모든 PO를 찾고, 항목을 스프레드시트로 뽑아줘." |
| **결과물로 끝나는 웹 조사** | 탭 10개를 열어 각 내용을 뽑기 | 복붙 없이 브리프로 정리해요 |

> 패턴: **"이 맥락을 Claude에게 주고 싶은데 웹에 있네"**라는 생각이 들 때마다 Claude in Chrome이 답이에요.

## 3. Cowork와 함께 쓰기

<div class="oa-eli">
  <span class="oa-eli__head">🏃 쉽게 말하면</span>
  <p><b>심부름 한 번에 세 군데</b>를 들르는 거예요. 은행(대시보드)에서 명세서를 떼고, 창고(Drive)와 게시판(Slack)에서 관련 자료를 챙겨서, 한 장짜리 보고서로 책상에 올려놔요.</p>
</div>

실제 예시예요. 팀의 고객 건강 대시보드가 로그인 뒤에 있고 커넥터가 없어요. 금요일까지 노랑 · 빨강 계정 전체의 1페이지 요약이 필요해요. Cowork에서 이렇게 말해요.

```text
Chrome에서 고객 건강 대시보드를 열어 노랑·빨강 계정을 전부 뽑고, 각 계정에 대해 Drive의 고객 폴더에서
지난 30일 활동과 Slack #customer-success의 최근 스레드를 가져와, 금요일 통화 전에 볼 1페이지 요약을 만들어줘.
```

<ol class="oa-flow">
  <li><strong>브라우저 단계 넘기기</strong>Claude가 Claude in Chrome에 넘겨요</li>
  <li><strong>대시보드에서 추출</strong>상태 필터 → 기간 선택 → export</li>
  <li><strong>데이터 되받기</strong>결과가 Cowork로 넘어와요</li>
  <li><strong>맥락 더하기</strong>Drive와 Slack에서 보조 자료</li>
  <li><strong>요약 만들기</strong>폴더에 요약 문서가 생겨요</li>
</ol>

**하나의 위임, 세 개의 맥락 소스**(대시보드, Drive, Slack)예요.

## 4. 유의점

<div class="oa-eli">
  <span class="oa-eli__head">🚪 쉽게 말하면</span>
  <p>Claude는 <b>내가 열어 준 문으로만</b> 들어가요. 문은 내가 직접 열어야 하고(로그인), 일단 들어가면 내가 볼 수 있는 건 다 볼 수 있으니 <b>어느 방까지 보여 줄지</b>는 내가 정해야 해요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔑</span><span class="oa-card__title">로그인 상태여야 해요</span>Claude가 대신 로그인해 주지 못해요. 인증이 필요하면 브라우저에서 내가 한 번 로그인하고, Claude는 이미 인증된 session에서 작업해요</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">👁️</span><span class="oa-card__title">웹에서의 접근을 신중히</span>커넥터처럼 Claude는 사용자가 보는 것을 봐요. 열린 웹에서는 내가 접근 가능한 모든 것이 포함돼요. 민감한 사이트는 Claude가 조작할 범위를 좁히고, 승인 전에 행동을 검토해요</div>
</div>

> 최신 설정은 *Get started with Claude in Chrome*을 참고하세요. Claude in Chrome은 일부 enterprise plan에서는 제공되지 않을 수 있어요(현재 강의에서는 이 안내가 Lesson 2에 있어요).

### 지금 해보기

지금 맡고 있는 일 중 **커넥터가 없는 브라우저 기반 도구**에 사는 작업 하나를 골라요. 위의 대시보드 예시처럼 Cowork에 맡기고 Claude가 Chrome에서 일하게 해 보세요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Claude in Chrome은 어떤 도구를 위한 것인가요?</summary>
  <p><b>정답: 커넥터가 없는, 브라우저에 사는 도구.</b> 내부 대시보드, 벤더 포털, 로그인 뒤 웹 앱처럼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. SSO 로그인이 필요한 사내 대시보드예요. Claude에게 계정 정보를 주고 대신 로그인하게 하면 되나요?</summary>
  <p><b>정답: 아니요.</b> Claude는 대신 로그인하지 못해요. 내가 브라우저에서 한 번 로그인하면 Claude가 그 인증된 session에서 작업해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 대시보드 예시에서 "하나의 위임, 세 개의 맥락 소스"의 세 소스는 무엇인가요?</summary>
  <p><b>정답: Chrome의 고객 건강 대시보드, Drive의 고객 폴더, Slack의 #customer-success 채널.</b> 브라우저 단계는 Claude in Chrome이, 나머지와 결과물 조립은 Cowork가 처리해요.</p>
</details>

## 정리

- Claude in Chrome은 **커넥터 없는 웹 도구**(대시보드 · 포털 · 로그인 뒤 앱)를 Cowork에 연결하는 다리예요.
- 브라우저에서 정보를 모으고 행동한 뒤 **Cowork에서 결과물을 만드는 하나의 흐름**으로 동작해요.
- 기본적으로 **민감한 행동 전에 확인**하고, 각 행동을 승인하거나 거부할 수 있어요.
- **로그인은 사용자가** 해요. Claude는 이미 인증된 session에서만 작업해요.
- Claude는 내가 보는 것을 봐요. **민감한 사이트는 범위를 좁히고** 승인 전에 행동을 검토해요.

다음 레슨에서는 많은 작업이 실제로 도착하는 M365 앱(Word, Excel, PowerPoint, Outlook) 안의 Claude를 봐요.
