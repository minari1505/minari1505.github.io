---
title: "Artifacts"
title_ko: "Artifacts 만들고 공유하기"
course: claude-101
lesson: 5
---

## 학습 목표

- **Artifacts**가 무엇이고, Claude가 언제 artifact를 만들며, 결과물이 어디에 사는지 이해하기
- design, deck, living document를 artifact로 만들기 (유료 plan)
- artifact를 **직접 편집 · comment · 대화로 요청**하는 세 가지 방법으로 고치기
- artifact를 공유하고 누가 보고 편집할지 정하기
- artifact가 기대대로 생성되지 않을 때 해결하는 방법 정리하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>결과물을 요청</strong>내용이 아니라 deliverable로</li>
  <li><strong>전용 창에 생성</strong>대화 옆에서 실물이 만들어져요</li>
  <li><strong>고치기</strong>직접 편집 · comment · 대화</li>
  <li><strong>공유 · export</strong>link 공유 또는 파일로 내보내기</li>
</ol>

이 레슨을 한 문장으로 줄이면: **대화는 만드는 곳이고, artifact는 계속 고치고 공유하는 결과물이다.**

## 1. Artifacts란?

<div class="oa-eli">
  <span class="oa-eli__head">🎨 쉽게 말하면</span>
  <p>화가와 이야기하는 동안 그림이 <b>말풍선 속이 아니라 옆의 이젤 위에</b> 그려진다고 생각해 보세요. 대화는 흘러가도 그림은 이젤에 남아서, 다시 손보고 다른 사람에게 보여 줄 수 있어요.</p>
</div>

**Artifacts**는 Claude와 함께 만드는 결과물이에요: 문서, deck, design, dashboard, prototype. 긴 코드나 텍스트가 chat 안에 묻히는 대신, 대화 옆의 **전용 창**에서 실물이 만들어지고, 바로 쓰고 다듬을 수 있어요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">💬 chat 답변</span>
    <ul>
      <li>한 번 읽고 지나가는 내용</li>
      <li>긴 코드와 문서가 대화 속에 묻혀요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🖼️ artifact</span>
    <ul>
      <li>그 자체로 독립적인 <b>작업물</b></li>
      <li>편집하고, 재사용하고, 공유할 수 있어요</li>
      <li>대화의 일부이면서 standalone output처럼 다뤄요</li>
    </ul>
  </div>
</div>

### Claude가 artifact를 만드는 기준

Claude는 **그 자체로 서는 것**, 즉 한 번 읽고 마는 게 아니라 편집 · 재사용 · 공유하고 싶을 만한 것을 요청받으면 artifact를 만들어요. 보통 이런 조건이에요.

- 내용이 충분히 크고 self-contained해요
- 보통 15줄 이상으로 의미 있는 단위예요
- 사용자가 편집, 반복 개선, 재사용할 가능성이 높아요
- surrounding conversation 없이도 독립적으로 의미가 있어요
- 나중에 다시 참조하거나 사용할 만한 결과물이에요

Claude가 chat에만 답했다면 명시적으로 요청하면 돼요.

```text
이걸 artifact로 만들어줘.
```

```text
Show me this in an artifact.
```

### 결과물은 어디에 사나요?

| Plan | artifact가 사는 곳 |
|---|---|
| **유료 plan** | artifact가 만든 대화에 묶이지 않아요. 만든 모든 것이 **Artifacts 탭**에 저장돼서 나중에 돌아와 계속 편집하고 공유할 수 있어요 |
| **Free plan** | artifact는 그것을 만든 대화에 남아 있어요 |

## 2. 무엇을 만들 수 있나요?

<div class="oa-eli">
  <span class="oa-eli__head">🏭 쉽게 말하면</span>
  <p>문구점에 <b>포스터 코너, 발표 자료 코너, 문서 코너</b>가 따로 있는 것처럼, Claude에는 가장 흔한 업무 결과물마다 전용 제작 방식이 있어요. 그 밖의 것(차트, 계산기, 코드)은 일반 작업대에서 만들어요.</p>
</div>

### Design · Slides · Docs — 전용 제작 경험

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🖌️</span><span class="oa-card__title">Claude Design</span><b>시각 작업</b>: landing page, one-pager, product mockup, interface prototype</div>
  <div class="oa-card"><span class="oa-card__icon">📽️</span><span class="oa-card__title">Claude Slides</span><b>Presentation</b>: 가진 노트 · 보고서 · 대화에서 outline, 내용, layout까지</div>
  <div class="oa-card"><span class="oa-card__icon">📝</span><span class="oa-card__title">Claude Docs</span><b>Living document</b>: 나와 Claude가 함께 쓰는 rich-text 페이지</div>
</div>

| 종류 | 특징 |
|---|---|
| **Claude Design** | 내 design system을 가져오면 초안이 우리 brand로 나와요. canvas에서 직접 drag, resize, align할 수 있어요 |
| **Claude Slides** | 모든 slide를 직접 편집하고 내 이미지를 넣을 수 있어요. Claude를 떠나지 않고 발표하거나, PowerPoint · PDF로 export해요 |
| **Claude Docs** | Claude가 눈앞에서 초안을 쓰고, 확인 질문을 하고, comment로 선택의 이유를 설명해요. 동료와 **동시에** 같은 doc을 편집하고 변경은 바로 저장돼요. 연결된 도구의 데이터를 chart와 diagram으로 페이지에 넣을 수 있어요. Google Docs나 `.docx`로 export해요 |

셋은 한 대화 안에서 함께 동작하고, 하나에서 다른 하나를 요청할 수 있어요: "이 doc을 deck으로 바꿔줘", "이 deck으로 one-pager를 만들어줘".

> **Docs 안의 chart는 live feed가 아니라 snapshot이에요.** 스스로 갱신되지 않으니, 최신 수치가 필요하면 Claude에게 연결된 도구에서 새 데이터를 가져와 업데이트해 달라고 요청해요.

**Plan별 사용 가능 여부** — Claude Design, Slides, Docs는 **beta**이고 유료 plan(Pro, Max, Team, Enterprise)에서 쓸 수 있어요.

| Plan | 기본 상태 | 끄고 켜는 곳 |
|---|---|---|
| **Pro · Max** | 기본으로 켜짐 | `Settings > Capabilities`에서 각각 끌 수 있어요 |
| **Team** | 기본으로 켜짐 | admin이 끌 수 있어요 |
| **Enterprise** | 기본으로 꺼짐 | admin이 `Organization settings > Artifacts`에서 켜야 해요 |

보이지 않으면 admin에게 문의해요. admin은 조직에서 artifacts 자체를 켤지도 관리해요.

### 모든 plan에서 만드는 artifact

모든 게 Design · Slides · Docs를 필요로 하진 않아요. 팀 수치 dashboard, 매주 업데이트하는 tracker, onboarding 절차 flowchart, interactive page, 작동하는 코드를 요청하면 Claude가 대화 안에서 바로 artifact로 만들어요. **Free를 포함한 모든 plan**에서요.

| 유형 | 용도 |
|---|---|
| **Documents** | 회의록, 보고서, project plan, blog post 같은 text-heavy output |
| **Code snippets** | Python, JavaScript, C++ 등 여러 언어의 작동 코드 |
| **HTML pages** | landing page, form, interactive demo, quick prototype |
| **SVG images** | logo, icon, illustration, vector graphic |
| **Mermaid diagrams** | flowchart, sequence diagram, Gantt chart, org chart |
| **React components** | calculator, dashboard, game, data visualization 같은 interactive UI |

React component artifact는 단순 mockup이 아니라 실제 logic과 user interaction을 포함할 수 있어요.

### Artifact와 file creation은 달라요

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🖼️ Artifact</span>
    <ul>
      <li>Claude 안에서 바로 열리고 업데이트돼요.</li>
      <li><b>link로 공유</b>해요.</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">📁 File creation</span>
    <ul>
      <li>Word, Excel, PowerPoint, PDF 파일을 만들어요.</li>
      <li><b>download해서 다른 앱에서</b> 열어요.</li>
    </ul>
  </div>
</div>

## 3. 첫 artifact 만들기

<div class="oa-eli">
  <span class="oa-eli__head">🛎️ 쉽게 말하면</span>
  <p>식당에서 조리법을 외울 필요 없이 <b>먹고 싶은 걸 말하면</b> 되는 것처럼, 특별한 명령어 없이 원하는 결과물을 설명하면 돼요.</p>
</div>

Artifact 만들기는 대화만큼 간단해요. 시작하는 곳은 두 가지예요.

- **어느 대화에서든** 원하는 걸 설명해요. composer의 `Output` 메뉴에서 design, slides, doc을 고를 수도 있어요.
- **Artifacts 탭**에서 design, deck, doc을 고르고 필요한 걸 설명해요.

```text
고객 onboarding process를 보여주는 flowchart를 만들어줘.
```

```text
월별 지출을 입력하면 category별 breakdown을 보여주는 interactive dashboard를 만들어줘.
```

```text
생산성 앱 landing page를 hero section과 feature list가 포함되게 디자인해줘.
```

```text
새 initiative에 재사용할 project brief template을 작성해줘.
```

| 이렇게 요청하면 | 이런 artifact가 만들어져요 |
|---|---|
| "이 회의록을 팀 회의용 5장짜리 deck으로 바꿔줘" | slide별로 편집하고 발표할 수 있는 **deck** |
| "onboarding 개편 project brief를 팀과 공유할 한 쪽짜리 doc으로 써줘" | 팀이 함께 편집하는 **living doc** |
| "생산성 앱 landing page를 hero section과 feature list로 디자인해줘" | 편집 가능한 canvas 위의 **design** |
| "월별 지출을 입력하고 breakdown을 보는 interactive dashboard를 만들어줘" | **interactive dashboard** (모든 plan에서 가능) |

앞의 세 가지는 내 plan에서(Team · Enterprise는 admin이) design, deck, doc 경험을 켜 둔 경우예요.

> Claude는 **이미 가진 context**를 써요: 공유한 파일, 지금까지의 대화, project knowledge, 연결된 도구. 전부 다시 넣어 줄 필요가 없어요. 방금 함께 분석을 마쳤다면 deck은 그 생각에서 출발해요.

### Artifact 창에서 할 수 있는 일

Artifact가 생성되면 대화 오른쪽에 전용 창이 열려요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">👀</span><span class="oa-card__title">Preview 보기</span>결과물이 어떻게 보이는지 확인</div>
  <div class="oa-card"><span class="oa-card__icon">🧾</span><span class="oa-card__title">Code 보기</span>Claude가 생성한 underlying code 확인</div>
  <div class="oa-card"><span class="oa-card__icon">📋</span><span class="oa-card__title">Copy</span>다른 곳에 붙여 넣기 위해 내용 복사</div>
  <div class="oa-card"><span class="oa-card__icon">⬇️</span><span class="oa-card__title">Download</span>파일로 저장</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🔁</span><span class="oa-card__title">Iterate</span>Claude에게 변경을 요청해 계속 개선. 모든 게 편집 가능한 상태로 남아요</div>
</div>

## 4. Artifact 고치기 — 세 가지 방법

<div class="oa-eli">
  <span class="oa-eli__head">✏️ 쉽게 말하면</span>
  <p>동료와 문서를 고칠 때 <b>내가 직접 고치거나, 포스트잇을 붙여 부탁하거나, 말로 요청</b>하죠. Artifact도 똑같이 세 가지를 섞어 쓸 수 있어요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">⌨️</span><span class="oa-card__title">① 직접 편집</span>doc에 바로 타이핑, slide 편집, design canvas에서 drag · resize. 내 편집과 Claude의 편집이 나란히 들어가요</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">② Claude에게 comment</span>바꾸고 싶은 요소 · slide · 구절에 comment를 남기면 Claude가 <b>그 자리에서</b> 고쳐요. doc에서는 comment에 Claude를 @-mention하면 무엇을 왜 바꿨는지 답해요</div>
  <div class="oa-card"><span class="oa-card__icon">🗣️</span><span class="oa-card__title">③ 그냥 요청</span>대화를 이어 가면 Claude가 artifact를 다음 버전으로 고치고, 바꾼 내용을 모두 보여 줘요</div>
</div>

- 직접 편집과 comment는 design, deck, doc 경험의 일부예요.
- 오타 수정이나 로고 위치 조정은 직접 하면 변경을 설명하고 다시 쓰기를 기다릴 필요가 없어요.

> **Mobile에서는** 어느 대화에서든 artifact를 요청하고 Artifacts 탭에서 전체 화면으로 열 수 있어요. 하지만 **직접 편집, comment, 공유 설정 변경은 desktop이나 web**에서 해요.

## 5. 공유와 export

<div class="oa-eli">
  <span class="oa-eli__head">🔐 쉽게 말하면</span>
  <p>내 작업물은 <b>기본적으로 잠긴 서랍</b> 안에 있어요. 누구에게 열쇠를 줄지, 보기만 할지 고칠 수도 있을지는 내가 정해요. 그리고 공개되는 건 서랍 속 물건 하나지, 내 책상 전체(대화)가 아니에요.</p>
</div>

Artifact는 **공유하기 전까지 private**이에요.

**Design · deck · doc 공유**: 이름이나 email로 사람을 추가하고 각자 무엇을 할 수 있는지 골라요.

| 권한 | 대상 |
|---|---|
| **View · Comment · Edit** | design, deck |
| **View · Edit** | doc |

조직의 모든 사람과 link를 공유할 수도 있어요. 공유받은 사람은 artifact를 열려면 **Claude account가 필요**해요.

### Plan별 공유 범위

| Plan | 할 수 있는 것 |
|---|---|
| **Free** | copy, download, 또는 publish해서 link를 가진 누구나 볼 수 있게 하기 |
| **Pro · Max** | 나만 보기 또는 link를 가진 누구에게나 공유 중 선택. design이나 deck을 **public artifact로 publish**할 수 있어요 |
| **Team · Enterprise** | link는 기본적으로 **조직 안**에 머물러요. 조직 밖 공유("anyone with the link")는 Owner나 Primary Owner가 `Organization settings > Artifacts`에서 **External sharing**을 켠 뒤에만 가능해요 |

> Publish해도 **artifact만 public**이 되고, 원래 대화는 private으로 유지돼요. 그래도 link를 가진 누구나 볼 수 있다는 점은 기억해요.

### 이전 버전 강의에 있던 publish 설명

아래는 이 노트를 처음 작성할 때 강의에 있던 내용이에요. 현재 강의 페이지 본문에는 없어서 따로 모아 둬요.

- Team과 Enterprise 사용자가 조직 내부에 공유한 artifact는 조직 안에 머물고, 접근하려면 team authentication이 필요해요.
- Publish하면 **선택한 artifact version만** public이 돼요.
- 다른 사람이 artifact를 **remix**해서 자신의 Claude conversation에서 수정할 수 있어요.
- artifact 오른쪽 위의 `Share` 또는 `Publish` 버튼을 쓰고, 나중에 언제든 **unpublish**할 수 있어요.
- publish한 artifact는 search engine에 indexing되지 않아 Google 검색 결과에 나타나지 않아요.

### Export — 작업이 Claude 밖으로 나가야 할 때

| 만든 것 | Export 형식 |
|---|---|
| **Deck** | `.pptx`, `.pdf` |
| **Design** | `.pptx`, `.pdf`, `.html` |
| **Doc** | Google Docs, `.docx` |

어떤 artifact든 artifact 창에서 바로 copy하거나 download할 수도 있어요.

## 6. Artifact를 잘 만드는 prompt

<div class="oa-eli">
  <span class="oa-eli__head">🪑 쉽게 말하면</span>
  <p>목수에게 "의자 하나요"라고 하면 아무 의자나 나와요. <b>"아이 방에 둘 낮은 의자, 모서리는 둥글게"</b>라고 하면 쓸 사람에게 맞는 의자가 나와요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">😐 내용만 요청</span>
    <ul>
      <li>"Q3 결과를 요약해줘."</li>
      <li>→ chat 답변이 나와요.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">💪 결과물을 요청</span>
    <ul>
      <li>"Q3 결과를 leadership team용 한 쪽짜리 doc으로 만들어줘."</li>
      <li>→ 공유하고 계속 작업할 수 있는 artifact가 나와요.</li>
    </ul>
  </div>
</div>

### 원하는 기능을 구체적으로 말하기

```text
Budget tracker를 만들어줘.
```

이것도 괜찮지만, 이렇게 쓰면 더 좋아요.

```text
월별 예산 tracker를 만들어줘.
지출을 category별로 입력할 수 있고,
pie chart로 breakdown을 보여주며,
예산을 초과하면 warning을 보여줘.
```

### 사용자를 설명하기

Artifact를 누가 쓸지 알려 주면 Claude가 적절한 design choice를 해요. 아래 두 요청은 다른 결과를 만들어야 해요.

```text
이 flowchart는 신입 직원 onboarding용이야.
```

```text
이 flowchart는 engineering team의 incident response 절차용이야.
```

### 점진적으로 개선하기

한 번에 모든 기능을 넣으려 하기보다 한 번에 하나씩 추가하거나 수정해요.

<ol class="oa-flow">
  <li><strong>기본 구조</strong>먼저 뼈대 만들기</li>
  <li><strong>Chart 추가</strong>그다음 시각화</li>
  <li><strong>Warning state 추가</strong>그다음 예외 상태</li>
  <li><strong>Styling 조정</strong>마지막에 다듬기</li>
</ol>

이렇게 하면 어떤 변경이 잘 작동하고 어떤 변경이 문제를 만드는지 쉽게 확인할 수 있어요.

### 그 밖의 팁

- **이미 한 작업 위에 쌓기**: 가장 좋은 artifact는 대화의 끝, Claude가 이미 문제를 이해한 시점에 나와요.
- **필요할 때 데이터 새로 고치기**: doc에 넣은 chart와 diagram은 스스로 업데이트되지 않아요. 최신 수치가 필요하면 새 데이터를 가져와 chart를 업데이트해 달라고 요청해요.

### Troubleshooting

| 증상 | 시도할 것 |
|---|---|
| Claude가 chat에만 답했어요 | "artifact로 만들어줘"라고 명시해요 |
| 결과물이 너무 단순해요 | 기능, 사용자, format을 더 구체적으로 설명해요 |
| UI가 마음에 들지 않아요 | style reference나 tone을 추가해요 |
| 복잡한 artifact가 잘 안 나와요 | 한 번에 만들지 말고 단계별로 개선해요 |
| 재사용할 목적이에요 | export/download 가능한 형태를 요청해요 |

### 실습 질문

- 손으로 가장 자주 만드는 deliverable(status doc, 팀 deck, one-pager) 중 artifact로 시작할 수 있는 것은 무엇인가요?
- 반복 업무 중 interactive artifact로 만들면 재사용할 수 있는 것은 무엇인가요?
- 내 업무 프로세스 중 flowchart나 diagram으로 보면 더 명확한 것은 무엇인가요?
- 빠르게 테스트해 보고 싶은 prototype이나 내부 tool이 있나요?
- 문서 template으로 만들어 두면 매번 시간을 줄일 수 있는 것은 무엇인가요?
- 다음 작업물을 누구와 공유할 건가요? 그 사람은 보기, comment, 함께 편집 중 무엇을 할 수 있어야 하나요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Claude가 결과물을 artifact가 아니라 chat 안에만 답했어요. 어떻게 하나요?</summary>
  <p><b>정답: "이걸 artifact로 만들어줘"라고 명시적으로 요청해요.</b> 처음부터 "요약해줘" 대신 "한 쪽짜리 doc으로 만들어줘"처럼 deliverable을 요청하는 것도 방법이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. Artifact를 고치는 세 가지 방법은 무엇인가요?</summary>
  <p><b>정답: 직접 편집 · Claude에게 comment · 대화에서 요청.</b> 셋을 자유롭게 섞어 쓸 수 있어요. 오타 같은 작은 수정은 직접 하는 게 빨라요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Artifact를 publish하면 그 artifact를 만든 대화도 공개되나요?</summary>
  <p><b>정답: 아니요.</b> artifact만 public이 되고 대화는 private으로 유지돼요. 다만 link를 가진 누구나 그 artifact를 볼 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Artifact와 file creation의 실질적인 차이는 무엇인가요?</summary>
  <p><b>정답: artifact는 Claude 안에서 열리고 업데이트되며 link로 공유해요. file creation은 Word · Excel · PowerPoint · PDF 파일을 download해서 다른 앱에서 열게 해 줘요.</b></p>
</details>

## 정리

- Artifacts는 Claude와 만든 결과물을 chat 밖의 **독립적인 작업물**로 다루게 해 줘요. 유료 plan에서는 Artifacts 탭에 저장돼요.
- **Claude Design · Slides · Docs**(beta, 유료 plan)는 design, deck, living document 전용 경험이에요. dashboard, flowchart, interactive page, 코드는 **모든 plan**에서 artifact로 만들 수 있어요.
- 고치는 방법은 세 가지예요: **직접 편집 · comment · 대화로 요청**.
- Artifact는 공유 전까지 private이고, publish해도 대화는 공개되지 않아요. 공유 범위는 plan마다 달라요.
- 좋은 artifact를 만들려면 **deliverable을 요청하고, 기능과 사용자를 구체적으로 설명하고, 점진적으로 개선**해요.

다음 레슨에서는 Claude에게 반복 가능한 전문 workflow를 가르치는 Skills를 다뤄요.
