---
title: "Projects"
title_ko: "Projects로 작업과 지식 정리하기"
course: claude-101
lesson: 4
---

## 학습 목표

- Claude의 **Projects**가 무엇이고 언제 쓰면 좋은지 이해하기
- 이름, 설명, 공개 범위를 지정해 새 project를 만드는 흐름 익히기
- **project instructions**로 Claude의 응답 방식과 작업 절차를 고정하기
- project **knowledge base**에 문서와 파일을 추가하는 방법 정리하기
- Team/Enterprise 환경에서 project를 공유하고 권한을 관리하는 방식 이해하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>Project 만들기</strong>이름 · 설명 · 공개 범위</li>
  <li><strong>Instructions 쓰기</strong>항상 지킬 규칙과 절차</li>
  <li><strong>Knowledge base 쌓기</strong>반복 참조할 문서</li>
  <li><strong>Project 안에서 대화</strong>매번 다시 설명하지 않기</li>
  <li><strong>팀과 공유</strong>같은 맥락에서 협업</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Projects는 자료 폴더가 아니라, 같은 맥락을 매번 다시 설명하지 않게 해 주는 맥락 저장소다.**

## 1. Projects란?

<div class="oa-eli">
  <span class="oa-eli__head">🗄️ 쉽게 말하면</span>
  <p>일반 chat이 지나가다 묻는 <b>길거리 질문</b>이라면, Project는 담당자가 정해진 <b>전용 사무실</b>이에요. 서류철(knowledge base)이 책장에 꽂혀 있고, 벽에는 업무 규칙(instructions)이 붙어 있어서 들어갈 때마다 처음부터 설명할 필요가 없어요.</p>
</div>

**Projects**는 특정 업무 흐름을 위한 독립 workspace예요. 각 project는 자체 memory, chat history, knowledge base, customized instructions를 가져요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">관련 대화</span>한 주제나 업무 영역의 chat을 모아요</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📚</span><span class="oa-card__title">Knowledge base</span>Claude가 모든 chat에서 참조할 문서</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📋</span><span class="oa-card__title">Instructions</span>project별 지시사항</div>
  <div class="oa-card"><span class="oa-card__icon">👥</span><span class="oa-card__title">공유 맥락</span>팀과 함께 쓰는 작업 맥락</div>
</div>

```text
특정 업무 영역
  ├─ 관련 대화
  ├─ 참고 문서
  ├─ 프로젝트별 지시사항
  └─ 팀과 공유하는 작업 맥락
```

### 언제 Projects를 쓰면 좋은가요?

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">💬 일반 chat으로 충분해요</span>
    <ul>
      <li>일회성 질문</li>
      <li>다시 볼 일 없는 자료</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🗄️ Project가 더 적합해요</span>
    <ul>
      <li><b>반복해서 참조할 자료</b>가 있을 때: 회의록, 설문 결과, 보고서, 과거 데이터</li>
      <li><b>항상 지켜야 할 응답 규칙</b>이 있을 때: 공식 톤, citation 포함, 정해진 template 사용</li>
      <li><b>여러 사람</b>이 같은 기반 지식과 instructions로 협업해야 할 때</li>
      <li>특정 고객, 제품, 캠페인, 연구 주제처럼 <b>오래 유지되는 업무 흐름</b>이 있을 때</li>
    </ul>
  </div>
</div>

> 핵심은 **같은 맥락을 매번 다시 설명하지 않아도 된다**는 점이에요. 같은 파일을 다시 올릴 필요도 없어요.

## 2. 첫 Project 만들기

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면</span>
  <p>새 사무실에 입주하는 순서와 같아요. <b>문패를 달고</b>(이름 · 설명), <b>업무 규칙을 붙이고</b>(instructions), <b>책장을 채우면</b>(knowledge base) 끝이에요. 몇 분이면 돼요.</p>
</div>

### Step 1 — Project 설정하기

<ol class="oa-flow">
  <li><strong>Projects 열기</strong>왼쪽 sidebar의 <code>Projects</code> 또는 <code>claude.ai/projects</code></li>
  <li><strong>+ New Project</strong>오른쪽 위 버튼 클릭</li>
  <li><strong>이름 붙이기</strong>설명적인 이름으로</li>
  <li><strong>설명 쓰기</strong>무엇을 위한 project인지</li>
  <li><strong>공개 범위 선택</strong>private 또는 조직 공유</li>
</ol>

- **이름 예시**: `Q4 Marketing Campaign`, `Product Documentation`
- **설명**: Claude는 이 설명을 직접 보지 않아요. 하지만 나와 팀원이 project 목적을 이해하는 데 도움이 돼요.
- **공개 범위**: 개인용으로 private하게 두거나, Claude for Work 사용자는 조직에 공유할 수 있어요.

### Step 2 — Project instructions 작성하기

Project instructions는 이 project 안의 **모든 대화**에서 Claude가 따르는 기본 규칙이에요. `Instructions`를 클릭해 패널을 열고, 다 쓴 뒤 `Save instructions`를 눌러요.

| 넣을 내용 | 예시 |
|---|---|
| **업무 맥락** | "이 project는 B2B 소프트웨어 제품의 마케팅 콘텐츠 작성을 위한 것이다." |
| **작업 절차** | "먼저 대상 독자를 끌어들일 blog 구조를 제안한 뒤 초안을 작성한다." |
| **톤과 스타일** | "전문적이지만 대화체를 유지하고, 가능하면 jargon은 피한다." |
| **구체 요구사항** | "마케팅 copy 끝에는 항상 call-to-action을 포함한다." |

Instructions로 workflow를 자동화할 수도 있어요. 이 project에서 Claude의 행동을 **프로그래밍한다**고 생각하면 돼요.

```text
회의 transcript를 업로드하면, 항상 다음 template에 맞춰 구조화된 요약을 만들어줘:
1. 핵심 결정
2. 액션 아이템
3. 미해결 질문
4. 후속 일정
```

Project instructions는 내가 설정한 user preferences, styles와 **함께** 적용돼요. 그러니 project 단위로 더 구체적인 작업 방식과 기준을 정해 두는 용도로 쓰면 좋아요.

### Step 3 — Knowledge base 만들기

Knowledge base는 Claude가 project 안의 모든 chat에서 참조하는 문서 저장소예요. project 메인 페이지 오른쪽의 files 메뉴에서 `+` 버튼으로 추가해요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎨</span><span class="oa-card__title">Reference 문서</span>Brand guidelines, style guides, templates</div>
  <div class="oa-card"><span class="oa-card__icon">📑</span><span class="oa-card__title">배경 자료</span>Research reports, meeting notes, requirements docs</div>
  <div class="oa-card"><span class="oa-card__icon">⭐</span><span class="oa-card__title">좋은 작업 예시</span>Claude가 따라 했으면 하는 결과물</div>
  <div class="oa-card"><span class="oa-card__icon">⚙️</span><span class="oa-card__title">기술 문서</span>Technical documentation, specifications</div>
</div>

- **파일 형식**: PDF, DOCX, CSV, TXT, HTML 등 다양한 파일을 올릴 수 있어요.
- **Google Drive**: 연결해서 문서를 직접 link할 수도 있어요.

파일 이름은 최대한 설명적으로 붙여요.

```text
좋음: Q4-2025-Sales-Report.pdf
애매함: report.pdf
나쁨: document1.pdf
```

> Claude는 **파일 이름을 참고해** 어떤 자료를 찾고 쓸지 판단해요. 이름 자체가 retrieval cue 역할을 해요.

## 3. 큰 knowledge base는 어떻게 처리되나요?

<div class="oa-eli">
  <span class="oa-eli__head">📖 쉽게 말하면</span>
  <p>도서관 사서는 질문을 받을 때마다 <b>책을 전부 읽지 않아요.</b> 색인을 찾아 관련된 책의 관련된 쪽만 펼치죠. 자료가 많아지면 Claude도 사서처럼 필요한 부분만 찾아 읽어요.</p>
</div>

Project에 자료를 많이 넣으면 context window 한계가 걱정될 수 있어요. Projects는 knowledge base가 커지면 자동으로 **Retrieval Augmented Generation(RAG)** 방식을 써요. 전체 문서를 한 번에 context에 넣는 대신, 질문에 답하는 데 가장 관련 있는 부분을 찾아 가져와요. 어느 파일을 볼지 내가 말해 줄 필요가 없어요.

<ol class="oa-flow">
  <li><strong>사용자 질문</strong>project 안에서 질문</li>
  <li><strong>검색</strong>project knowledge에서 관련 정보 찾기</li>
  <li><strong>선별</strong>가장 관련 있는 부분만 context로</li>
  <li><strong>답변 생성</strong>Claude가 답해요</li>
</ol>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">📦 knowledge가 작을 때</span>
    <ul>
      <li>자료를 한 번에 모두 context에 올려요.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🔎 context limit에 가까워지면 (RAG mode)</span>
    <ul>
      <li>자동으로 켜져요. 전부 올리는 대신 <b>검색해서 관련 부분만</b> 가져와요.</li>
      <li>capacity가 <b>최대 10배</b>까지 확장되고 응답 품질은 유지돼요.</li>
      <li>RAG가 켜지면 visual indicator가 보이지만, 사용 경험은 거의 같아요.</li>
    </ul>
  </div>
</div>

## 4. Project 안에서 작업하기

<div class="oa-eli">
  <span class="oa-eli__head">☕ 쉽게 말하면</span>
  <p>단골 카페에 가면 <b>"늘 먹던 걸로요"</b> 한마디면 돼요. Project 안의 대화는 Claude가 이미 내 자료와 규칙을 알고 있는 단골 대화예요.</p>
</div>

Project를 만들고 instructions와 knowledge base를 넣었다면, 이제 그 안에서 Claude와 대화하면 돼요. Project 안의 모든 conversation은 다음을 자동으로 활용해요.

- Project **knowledge base**
- Project **instructions**
- Project의 대화 맥락과 **memory**

같은 문서를 매번 다시 올리거나 같은 규칙을 다시 설명할 필요가 줄어들어요.

### Best practices

| 원칙 | 왜 · 어떻게 |
|---|---|
| **좁게 시작하고 넓히기** | 처음부터 모든 걸 넣지 말고 구체적인 use case 하나로 시작해요. 내용은 나중에 더하면 돼요 |
| **Knowledge base를 최신으로** | 오래된 문서는 오래된 답변을 만들어요. 주기적으로 검토하고 업데이트해요 |
| **Instructions는 구체적으로** | 모호한 instruction은 일관성 없는 결과로 이어져요 |
| **문서 이름은 설명적으로** | 관련 파일은 함께 묶어요. Claude는 파일 이름과 근접성으로 문서 간 관계를 이해해요 |
| **문서를 이름으로 언급하기** | 질문할 때 특정 문서를 이름으로 말하면 Claude가 검색을 더 잘 좁혀요 |

```text
Q3 report를 기준으로 고객이 가장 많이 제기한 우려사항을 정리해줘.
```

### Project 예시

어디서 시작할지 모르겠다면 이런 유형을 생각해 보세요.

| Project | 넣는 자료 | Claude가 돕는 일 |
|---|---|---|
| **Q4 product launch** | 제품 spec, 경쟁 분석, messaging note | launch 관련 질문과 문서 초안 작성 |
| **Research support** | 경쟁사 review, 사용자 조사, 고객 feedback | source 종합, 보고서 초안, 일관된 insight 정리 |
| **Client account hub** | 고객 brand guideline, 과거 산출물, 커뮤니케이션 이력 | 고객의 톤과 맥락에 맞춘 proposal · 보고서 작성 |
| **Event planning workspace** | venue contract, speaker bio, attendee data | 행사 운영 문서, 참석자 안내, 후속 보고서 작성 |
| **Job description generator** | 과거 JD, team charter, headcount request | 실제 팀 업무와 문화에 맞는 JD 작성 |

## 5. 팀과 협업하기

<div class="oa-eli">
  <span class="oa-eli__head">🔑 쉽게 말하면</span>
  <p>사무실 열쇠를 나눠 주는 일이에요. <b>둘러보고 이야기만 할 수 있는 방문증</b>(Can view), <b>서류와 규칙을 고칠 수 있는 직원증</b>(Can edit), <b>누가 들어올지 정하는 마스터키</b>(Owner)가 있어요.</p>
</div>

Claude for Work, 즉 **Team과 Enterprise plan**에서는 project를 팀과 공유할 수 있어요. 모두가 같은 context, instructions, 쌓인 지식의 혜택을 받아요.

### 권한 수준

| 권한 | 의미 |
|---|---|
| **Can view** | project 내용과 knowledge를 보고 chat할 수 있지만 수정은 못 해요. 토론 권한이 있는 read-only예요 |
| **Can edit** | instructions와 knowledge를 수정하고, 다른 members를 관리하고, 적극적으로 협업할 수 있어요 |
| **Owner** | project 생성자로서 공유 범위와 전체 설정을 제어해요. 특정 사람에게 공유하거나 조직 전체에 보이게 할 수 있어요 |

### 공유 방법

<ol class="oa-flow">
  <li><strong>Project 열기</strong>공유할 project</li>
  <li><strong>Share project</strong>project 이름 오른쪽 버튼</li>
  <li><strong>Member 추가</strong>이름이나 email로 개별 추가</li>
  <li><strong>또는 bulk sharing</strong>여러 email을 붙여 넣기</li>
  <li><strong>또는 조직 전체</strong><code>Everyone at [organization]</code></li>
</ol>

- 공유된 project는 팀원의 `Shared with me` 영역에 표시되고, **이메일 알림**도 가요.
- `Everyone at [organization]`으로 공유하면 조직 안에서 project를 찾아볼 수 있게 돼요.

### 실습 질문

- 지속되는 업무 중 전용 project와 유지되는 context가 도움이 될 일은 무엇인가요?
- Claude에게 정기적으로 다시 올리거나 다시 설명하게 될 문서는 무엇인가요?
- 팀에 있다면, 공유된 지식과 instructions가 도움이 될 project가 있나요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 다음 중 일반 chat보다 Project가 더 적합한 경우는?</summary>
  <p>① 단어 뜻 하나 물어보기 ② <b>매주 같은 brand guideline을 참조해 마케팅 글을 쓰기</b> ③ 한 번 보고 말 기사 요약하기</p>
  <p><b>정답: ②</b> — 반복해서 참조할 자료와 항상 지킬 규칙이 있는, 계속 이어지는 작업이에요. 일회성 질문은 일반 chat으로 충분해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. Project 설명(description)에 적은 내용은 Claude의 답변에 직접 반영되나요?</summary>
  <p><b>정답: 아니요.</b> Claude는 설명을 직접 보지 않아요. 설명은 나와 팀원이 project 목적을 이해하기 위한 거예요. Claude의 행동을 정하려면 <b>project instructions</b>에 써야 해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Knowledge base가 context limit에 가까워지면 어떤 일이 일어나나요?</summary>
  <p><b>정답: RAG mode가 자동으로 켜져요.</b> 전체를 한 번에 올리는 대신 질문과 관련 있는 부분만 검색해 가져오고, capacity가 최대 10배까지 확장돼요. 사용 경험은 거의 같아요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 팀원이 project의 자료를 보고 chat은 하되, instructions와 knowledge는 못 고치게 하려면 어떤 권한을 주나요?</summary>
  <p><b>정답: Can view.</b> Can edit은 instructions · knowledge · members까지 수정할 수 있어요. 공유 자체는 Team과 Enterprise plan에서 가능해요.</p>
</details>

## 정리

- Projects는 자체 memory, chat history, knowledge base, instructions를 가진 **독립 workspace**예요.
- 반복해서 쓰는 자료는 **knowledge base**에 넣고, 파일 이름은 설명적으로 붙여요.
- 항상 지켜야 할 작업 방식은 **project instructions**에 구체적으로 써요.
- knowledge base가 커지면 **RAG mode**로 관련 정보만 검색해 쓰고, capacity가 최대 10배까지 늘어요.
- Team/Enterprise에서는 project를 공유해 팀 전체가 같은 context에서 일해요 (Can view · Can edit · Owner).
- Projects는 "자료를 보관하는 폴더"가 아니라, Claude가 특정 업무를 더 일관되게 이해하고 수행하게 만드는 **맥락 저장소**예요.

다음 레슨에서는 Claude와 함께 만드는 결과물인 Artifacts를 다뤄요.
