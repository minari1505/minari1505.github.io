---
title: "Setting up Claude Cowork"
title_ko: "Claude Cowork 설정하기"
course: introduction-to-claude-cowork
lesson: 2
---

## 학습 목표

- Claude에게 **일할 곳** 정해 주기: desktop app에서는 내 컴퓨터의 폴더, 또는 project
- 작업이 있는 앱을 **커넥터**로 연결하기
- Claude가 무엇을 하기 전에 묻고 무엇은 묻지 않는지 이해해 **안심하고 위임**하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>설치 · 로그인</strong>desktop app을 열고 sign in</li>
  <li><strong>폴더 지정</strong>작업이 사는 곳을 좁게 골라요</li>
  <li><strong>커넥터 연결</strong>이메일, 메신저, 저장소, CRM</li>
  <li><strong>권한 모드 확인</strong>기본은 Ask before acting</li>
  <li><strong>작은 작업 맡겨 보기</strong>폴더 요약으로 시험 주행</li>
</ol>

이 레슨을 한 문장으로 줄이면: **폴더로 범위를 정하고, 커넥터로 맥락을 주고, 권한 모델을 믿고 맡겨요.**

## 1. 시작 준비

<div class="oa-eli">
  <span class="oa-eli__head">🪑 쉽게 말하면</span>
  <p>새 동료가 오면 먼저 <b>자리와 출입증</b>부터 마련해 주죠. Cowork 설정도 같아요. 앱을 깔고 로그인하는 게 출입증이고, 폴더를 정해 주는 게 자리예요.</p>
</div>

Cowork는 **Claude desktop app(Mac · Windows), 웹, Claude 모바일 앱**에서 쓸 수 있어요. 이 레슨은 desktop app을 써요. Claude가 **내 컴퓨터의 파일로 직접 작업**할 수 있는 곳이기 때문이에요.

- 아직 없다면 `claude.com/download`에서 설치하고, 앱을 열어 로그인해요.
- **유료 Claude plan**이 필요해요.
- Help Center의 *Get started with Claude Cowork* 글이 설정을 단계별로 안내해요.

## 2. 폴더 지정하기

<div class="oa-eli">
  <span class="oa-eli__head">🗄️ 쉽게 말하면</span>
  <p>새 동료에게 사무실 전체 열쇠를 주지 않고 <b>"이 프로젝트 서랍만 쓰세요"</b>라고 하는 것과 같아요. 서랍이 작을수록 엉뚱한 서류를 건드릴 일이 없어요.</p>
</div>

프롬프트 바에서 **Work in a project or folder**를 클릭하고 컴퓨터의 폴더를 골라요. 이것이 새 작업마다 하는 **가장 중요한 설정 선택**이에요. 폴더가 곧 작업이 사는 곳이기 때문이에요. Claude는 폴더 안의 모든 파일(Word, Excel, PDF, PowerPoint 등)을 읽고, 완성된 출력을 같은 위치에 저장해요.

### 가장 작은 폴더를 골라요

Claude는 전체 문서 폴더가 필요하지 않아요. 그 작업에 필요한 파일이 든 폴더면 돼요. 강의의 예시는 "Q3 competitive memo를 써 달라"고 하기 전에 어느 폴더를 고를지 묻는데, 단계마다 범위에 들어오는 파일 수가 이렇게 달라져요.

| 폴더 | 범위에 들어오는 파일 수 (강의 예시) |
|---|---|
| `~` (Home) | 24,782 |
| Documents | 8,910 |
| Work | 6,400 |
| Clients | 4,100 |
| **Q3 Competitive Review** | **12** ← 이 작업에 맞는 폴더 |

> **작업에 필요한 것을 담은 가장 작은 폴더**를 골라요. 그 밖의 것이 필요해지면 나중에 폴더를 더 추가할 수 있어요.

### 폴더에서 Claude가 할 수 있는 일

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">📎 메시지에 파일 첨부</span>
    <ul>
      <li>Claude가 거기 있는 것을 <b>읽기만</b> 해요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">📁 작업 폴더</span>
    <ul>
      <li><b>읽기 AND 쓰기</b> 권한이에요</li>
      <li>파일을 열고, 수정하고, 새로 만들고, 정리해요</li>
      <li>완성된 작업을 같은 곳에 다시 저장해요</li>
    </ul>
  </div>
</div>

- **맥락이 있는 실제 폴더**를 골라요. 원본 자료, 관련 문서, 템플릿이 있을 때 가장 잘 작동해요.

### 클라우드 파일은 다르게 동작해요

커넥터마다 Claude가 할 수 있는 범위가 달라요.

| 항목 | 내용 |
|---|---|
| **기본 Google Drive · M365 커넥터** | 많은 커넥터가 그렇듯 **읽기 · 검색 전용**이에요 |
| **다른 커넥터** | 만들기나 편집까지 가능한 것도 있어요 |
| **확인 방법** | 커넥터를 켤 때 각 커넥터의 설명을 확인해요 |
| **문서를 만들고 고치는 곳** | Claude가 파일을 직접 편집하는 곳은 작업 폴더예요. 문서를 만들고 반복 수정하는 건 여기서 해요 |

> 파일이 내 컴퓨터에 있다면 **desktop app에서 작업을 시작**해요. 웹이나 휴대폰에서 시작한 작업은 커넥터에는 닿지만 **로컬 폴더에는 닿지 못해요.**

## 3. 커넥터 추가

<div class="oa-eli">
  <span class="oa-eli__head">🔌 쉽게 말하면</span>
  <p>커넥터는 <b>멀티탭</b> 같아요. 한 번 꽂아 두면(설정), 작업마다 필요한 스위치만 켜고 끄면 돼요. 켜진 스위치만큼 Claude의 손이 닿아요.</p>
</div>

커넥터는 Claude가 이미 작업이 일어나는 앱(이메일, 캘린더, 팀 메신저, CRM, 클라우드 저장소)에 닿는 방법이에요. **Customize** 영역에서 한 번 설정하고, 작업마다 필요한 것을 켜고 꺼요.

대부분 먼저 설정하는 커넥터예요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📧</span><span class="oa-card__title">이메일 · 캘린더</span>M365 Outlook 또는 Gmail. 회의 맥락 추출, 후속 이메일 작성, 과거 스레드 찾기</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">메신저</span>Slack 또는 M365 Teams. 채널 히스토리 검색, 팀 논의 종합</div>
  <div class="oa-card"><span class="oa-card__icon">☁️</span><span class="oa-card__title">클라우드 저장소</span>SharePoint/OneDrive via M365, Google Drive, Box. 로컬에 없는 문서 접근</div>
  <div class="oa-card"><span class="oa-card__icon">📇</span><span class="oa-card__title">CRM · 프로젝트 도구</span>Notion, HubSpot, Salesforce, Asana, Linear 등. 팀이 쓰는 것, 실제 데이터가 있는 곳에 따라</div>
</div>

켜면 프롬프트에서 자연스럽게 참조해요. "런칭에 대해 Slack에서 팀이 뭐라 했는지 확인해줘"나 "지난 분기 고객 후속 이메일 찾아줘"처럼요. Claude가 어디를 볼지 알아요.

### 커넥터는 작업 단위로 켜요

각 작업은 **켜 둔 커넥터만** 써요. 강의의 예시 작업은 이래요: "이번 주 Slack 스레드, 캘린더의 회의, 열린 이메일 스레드, Drive의 기획 문서로 월요일 status update 초안을 써줘."

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🔌 아무것도 연결 안 함</span>
    <ul>
      <li>업로드하거나 붙여 넣은 것에 대해서는 추론할 수 있어요</li>
      <li>하지만 이 소스들은 <b>하나도 못 봐요</b></li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 필요한 커넥터를 켬</span>
    <ul>
      <li>켠 소스에서 이 작업에 필요한 맥락을 끌어와요</li>
      <li>닿는 범위와 <b>할 수 있는 일의 종류</b>가 함께 달라져요</li>
    </ul>
  </div>
</div>

> 커넥터가 없는 도구(내부 대시보드, 벤더 포털, 로그인 뒤 웹 앱)는 **Claude in Chrome**이 다리 역할을 해요. Claude가 페이지를 직접 읽고 조작하게 해 주는 브라우저 extension이에요(Module 3에서 다뤄요). "커넥터 없음"이 "데이터에 닿을 수 없음"은 아니에요. 다만 Claude in Chrome은 일부 enterprise plan에서는 제공되지 않을 수 있어요.

## 4. 권한 모델

<div class="oa-eli">
  <span class="oa-eli__head">✍️ 쉽게 말하면</span>
  <p>신입에게 일을 맡길 때 <b>"밖으로 나가는 메일은 보내기 전에 나한테 보여 줘"</b>라고 하죠. 믿음이 쌓이면 그 확인을 줄여요. 하지만 <b>서류 파쇄만큼은 언제나 내 서명</b>이 있어야 해요.</p>
</div>

폴더를 고르면 그 안의 읽기 · 쓰기가 자동으로 허가돼요. 그 안의 작업에 대해 Cowork는 두 권한 모드를 가져요.

| 모드 | 동작 | 이럴 때 좋아요 |
|---|---|---|
| **Ask before acting** (기본) | 외부에 영향을 주는 각 행동(이메일 전송, 메시지 게시, 파일 공유) 전에 멈추고 확인을 구해요 | 새 도구, 낯선 파일, 가까이 지켜보고 싶은 모든 것 |
| **Act without asking** | 그런 행동에도 멈추지 않아요. 더 빠르지만 더 위험해요 | 내가 직접 지켜보고 있고, 신뢰하는 파일 · 사이트 · 도구로 작업할 때만 |

> **두 모드 모두에서** Claude는 파일을 **영구 삭제하기 전에 항상 확인**해요. 이 확인은 건너뛸 수 없어요. 예외는 없어요.

사용자가 함께 통제하는 것도 있어요.

- **커넥터와 MCP**: Claude가 어떤 것에 닿을 수 있는지, 각각이 얼마나 자주 허가를 물을지
- **웹 접근**: 조직의 admin이 웹 검색을 끌 수 있고, 사용자는 Claude in Chrome이 방문할 수 있는 사이트를 제한할 수 있어요

기본 설정을 넘어 접근을 넓히기 전에, 그 커넥터나 웹사이트를 얼마나 신뢰하는지 먼저 따져 봐요.

이 모든 것의 목적은 하나예요: **나를 놀라게 할 행동을 하지 않을 거라는 걸 알고** 큰 작업을 맡길 수 있게 하는 것. "채팅이 아니라 위임"이 실제로 가능한 건 권한 모델이 뒤에서 일하고 있기 때문이에요.

## 5. 지금 해보기

넘어가기 전에 작은 작업 하나를 위임해 보세요. 설정이 제대로 됐는지 확인하고, Cowork가 어떤 느낌인지 알 수 있어요.

<ol class="oa-flow">
  <li><strong>실제 작업이 든 폴더 고르기</strong>고객 계정, 진행 중인 결과물 등. 원치 않으면 내용을 새 폴더로 복사</li>
  <li><strong>앱 하나 연결</strong>보통 이메일 · 클라우드 저장소가 가장 가치가 커요</li>
  <li><strong>폴더 요약 요청</strong>아래 프롬프트처럼</li>
  <li><strong>내 지식과 대조</strong>무엇을 잡았고, 놓쳤고, 틀렸는지</li>
</ol>

```text
이 폴더의 모든 것을 살펴보고, 알게 된 내용을 brief로 써줘. 폴더에 무엇이 있는지, 문서들이 서로 어떻게 관련되는지, 놀라운 인사이트나 공유할 만한 다른 정보가 있는지.
```

장난감 파일이 아니라 **실제 자료**를 써요. Cowork는 가진 맥락만큼만 유용하고, 이 연습의 목적은 그걸 직접 느끼는 거예요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Q3 competitive memo 작업을 맡기려고 해요. Documents 폴더와 "Q3 Competitive Review" 폴더 중 어느 것을 가리켜야 하나요?</summary>
  <p><b>정답: Q3 Competitive Review 폴더.</b> 작업에 필요한 것을 담은 가장 작은 폴더를 골라요. 필요하면 나중에 폴더를 더 추가할 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. "Act without asking" 모드로 바꾸면 Claude가 확인 없이 파일을 영구 삭제할 수 있나요?</summary>
  <p><b>정답: 아니요.</b> 영구 삭제 전 확인은 두 모드 모두에서 항상 뜨고 건너뛸 수 없어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 기본 Google Drive 커넥터로 Claude가 Drive 문서를 직접 편집할 수 있다고 가정해도 되나요?</summary>
  <p><b>정답: 안 돼요.</b> 기본 Google Drive · M365 커넥터는 읽기 · 검색 전용이에요. 커넥터마다 다르니 켤 때 설명을 확인하고, 문서를 만들고 고치는 일은 작업 폴더에서 해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 휴대폰에서 시작한 Cowork 작업이 내 노트북의 로컬 폴더 파일을 읽을 수 있나요?</summary>
  <p><b>정답: 아니요.</b> 웹이나 휴대폰에서 시작한 작업은 커넥터에는 닿지만 로컬 폴더에는 닿지 못해요. 로컬 파일이 필요하면 desktop app에서 시작해요.</p>
</details>

## 정리

- Cowork는 desktop app · 웹 · 모바일에서 쓸 수 있고, **내 컴퓨터의 파일로 작업하려면 desktop app**이에요. 유료 plan이 필요해요.
- **작업 폴더 지정이 가장 중요한 설정**이에요. 필요한 것을 담은 가장 작은, 맥락이 있는 실제 폴더를 골라요.
- 폴더는 **읽기 AND 쓰기**예요. 클라우드 커넥터는 읽기 · 검색 전용인 경우가 많으니 설명을 확인해요.
- **커넥터**는 Customize에서 한 번 설정하고 작업마다 켜고 꺼요. 커넥터 없는 웹 도구는 Claude in Chrome이 다리가 돼요.
- 기본 권한 모드는 **Ask before acting**이고, **영구 삭제는 어느 모드에서든 항상 확인**해요. 그래서 큰 작업을 안심하고 위임할 수 있어요.

다음 레슨에서는 내 일상 업무 중 Cowork에 가장 잘 맞는 일을 알아보는 패턴을 배워요.
