---
title: "Enterprise Search"
title_ko: "Enterprise Search"
course: claude-101
lesson: 8
---

## 학습 목표

- **Enterprise Search**가 무엇이고 어떤 질문에 잘 맞는지 설명하기
- **admin**과 **user** 각각의 설정 흐름 파악하기
- 조직 데이터가 permission과 security model로 어떻게 보호되는지 정리하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>무엇인가</strong>sidebar의 <code>Ask {Your Org Name}</code> — 조직 전체용 pre-built project</li>
  <li><strong>무엇을 묻나</strong>여러 source에 흩어진 정보를 종합해야 하는 질문</li>
  <li><strong>Admin 설정</strong>Owner가 Documents · Chat · (Email) connector 연결</li>
  <li><strong>User 인증</strong>각자 개인 계정으로 service에 인증</li>
  <li><strong>안전장치</strong>원래 도구에서 내가 볼 수 있는 것만 보여 줘요</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Enterprise Search는 회사 곳곳에 흩어진 지식을 내 권한 범위 안에서 찾아 출처와 함께 종합해 주는 전용 검색 공간이에요.**

> **Plan 안내** — Enterprise Search는 **Team과 Enterprise plan**에서 쓸 수 있고, workspace admin이 활성화해야 해요. Free, Pro, Max 사용자는 실습 없이 개념만 이해하고 넘어가도 돼요.

## 1. Enterprise Search란?

<div class="oa-eli">
  <span class="oa-eli__head">📚 쉽게 말하면</span>
  <p>회사에 <b>모든 부서 서류함의 위치를 아는 사서</b>가 있다고 생각해 보세요. "재택근무 규정이 뭐였죠?"라고 물으면 문서함·메신저·메일을 다 뒤져서 "여기, 여기, 여기에 이렇게 적혀 있어요" 하고 정리해 줘요. 단, <b>내 출입증으로 열 수 있는 서류함만</b> 열어요.</p>
</div>

**Enterprise Search**는 Claude for Work 사용자를 위한 조직 지식 검색 기능이에요. sidebar에 `Ask {Your Org Name}` 같은 전용 항목이 생기고, 회사의 여러 도구와 데이터 source에 묻혀 있는 지식을 찾아 종합해 줘요.

조직 전체를 위한 **pre-built Project**처럼 동작해요. 회사의 knowledge base가 이미 연결되어 있어서, 바로 질문하고 맥락이 반영된 답을 받을 수 있어요.

<ol class="oa-flow">
  <li><strong>흩어진 조직 지식</strong>회사 문서 · Slack · email · Drive · SharePoint</li>
  <li><strong>Enterprise Search</strong>연결된 도구 전체를 검색</li>
  <li><strong>종합 답변</strong>Claude가 source를 인용하며 하나의 응답으로 정리</li>
</ol>

### 일반 connectors와 무엇이 다른가요?

일반 chat에서 connector를 켜는 것과 달리, Enterprise Search는 **정보 수집과 종합에 특화**되어 있고 Anthropic 팀이 구성한 **custom instructions**를 사용해요.

| 구분 | 일반 connectors | Enterprise Search |
|---|---|---|
| 목적 | 특정 chat에서 도구와 데이터 연결 | 조직 지식 검색과 종합에 특화 |
| 설정 | 개인이 connector를 연결 | admin이 조직 단위로 설정한 뒤 사용자가 인증 |
| 범위 | 연결한 tool 중심 | 조직의 documents, chat, email 등 여러 source |
| 응답 방식 | 요청에 따라 tool 사용 | source citation이 있는 종합 답변 |
| 대상 plan | connector별로 다름 | Team / Enterprise |

일반 connectors가 "내가 연결한 도구를 Claude가 쓰는 것"이라면, Enterprise Search는 "조직의 지식 기반을 검색하는 전용 경험"에 가까워요.

## 2. 어떤 질문에 잘 맞나요?

<div class="oa-eli">
  <span class="oa-eli__head">🧩 쉽게 말하면</span>
  <p>답이 문서 한 장에 다 있으면 그 문서를 열면 돼요. 하지만 <b>퍼즐 조각이 문서함, 메신저, 메일에 나뉘어 있을 때</b>는 누군가 모아서 맞춰 줘야 하죠. Enterprise Search는 그 퍼즐을 맞춰 주는 역할이에요.</p>
</div>

여러 source에 걸쳐 있거나, 조직 안의 다양한 문서와 대화를 종합해야 하는 질문에 특히 유용해요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">⏩</span><span class="oa-card__title">빠르게 맥락 파악</span>자리를 비운 사이의 일, 지난주 업데이트, 현재 blocker</div>
  <div class="oa-card"><span class="oa-card__icon">📜</span><span class="oa-card__title">정책·프로세스</span>remote work policy, expense report, 휴가 요청</div>
  <div class="oa-card"><span class="oa-card__icon">🔬</span><span class="oa-card__title">Research·analysis</span>고객이 경쟁사를 고르는 이유, roadmap 논의 요약</div>
  <div class="oa-card"><span class="oa-card__icon">🌱</span><span class="oa-card__title">신규 팀원 onboarding</span>system 동작 방식, 누구에게 물어야 하는지, 쓰는 tool</div>
  <div class="oa-card"><span class="oa-card__icon">📈</span><span class="oa-card__title">Performance·project tracking</span>관련 논의와 문서, 핵심 결정사항, 팀별 contribution</div>
</div>

### 빠르게 맥락 파악하기

```text
내가 없는 동안 어제 무슨 일이 있었어?
```

```text
지난주 비즈니스 전반의 주요 업데이트를 요약해줘.
```

```text
Platform project의 현재 blocker가 뭐야?
```

### 정책과 프로세스 질문

```text
우리 회사의 remote work policy가 뭐야?
```

```text
expense report는 어떻게 제출해?
```

```text
휴가 요청 프로세스가 어떻게 돼?
```

### Research와 analysis

```text
고객들이 경쟁사를 선택하는 주요 이유는 뭐라고 말하고 있어?
```

```text
Q4 product roadmap에 대한 논의를 요약해줘.
```

```text
우리 customer onboarding process에 대한 정보를 찾아줘.
```

### 신규 팀원 onboarding

```text
우리 authentication system은 어떻게 동작해?
```

```text
billing system을 배우려면 누구와 이야기해야 해?
```

```text
engineering team은 deployment에 어떤 tool을 써?
```

### Performance와 project tracking

```text
marketing campaign 관련 논의와 문서를 찾아줘.
```

```text
지난주 leadership meeting의 핵심 결정사항은 뭐였어?
```

```text
Infrastructure initiative에 대한 팀별 contribution을 요약해줘.
```

질문을 하면 Claude는 SharePoint 문서, Slack 대화, Gmail thread, Google Drive 파일 같은 연결된 tool 전체를 검색하고, 결과를 하나의 응답으로 종합해요. **항상 source를 인용**하니까 전체 맥락을 직접 확인할 수 있어요.

## 3. 설정은 두 단계 — Admin 먼저, 그다음 User

<div class="oa-eli">
  <span class="oa-eli__head">🏢 쉽게 말하면</span>
  <p>새 사옥에 입주할 때 <b>관리팀이 먼저 건물 출입 시스템을 깔고</b>, 그다음 <b>직원 각자가 자기 사원증을 등록</b>하죠. Enterprise Search도 Owner가 조직 설정을 끝낸 뒤, 사용자가 각자 계정으로 인증해요.</p>
</div>

| 단계 | 누가 | 무엇을 하나 |
|---|---|---|
| **① 조직 설정** | Admin (Owner) | 연결할 도구를 고르고, project 이름과 설명을 정해요. 끝나면 조직 구성원 모두에게 열려요 |
| **② 개인 인증** | User | sidebar에서 project를 열고, 각 service에 **개인 계정**으로 인증한 뒤 질문을 시작해요 |

### Admin 설정 흐름

Enterprise Search project는 Team/Enterprise 조직에 **기본으로 켜져 있지만**, 구성원이 실제로 쓰려면 Owner가 초기 설정을 끝내야 해요.

1. 왼쪽 sidebar에서 `Ask Your Org`를 클릭해요.
2. `Set up for your org`를 클릭해요. 필요 없으면 `Disable`로 끌 수 있어요.
3. 조직 도구를 연결해요.
   - **Documents** connector: Google Drive, SharePoint 등 — 필수
   - **Chat** connector: Slack, Microsoft Teams 등 — 필수
   - **Email** connector: 권장되지만 선택 사항
4. `+ Add more`로 팀에 필요한 추가 도구를 설정해요.
5. project 이름을 지정해요. 이 이름이 모두의 sidebar에 `Ask [Name]`으로 표시돼요.
6. 설명을 추가하고 `Finish set up`을 클릭해요.

설정이 끝나면 조직 구성원 모두가 Enterprise Search를 쓸 수 있어요.

### User 설정 흐름

Admin이 설정을 끝내면 sidebar에 `Ask {Org Name}` project가 별표(starred)로 표시돼요.

<ol class="oa-flow">
  <li><strong>열기</strong>sidebar에서 해당 project 클릭</li>
  <li><strong>Onboarding</strong>guided onboarding flow를 따라 권장 service에 연결</li>
  <li><strong>인증</strong>Slack, Google, Microsoft 365 등 검색할 service마다 개인 계정으로 인증</li>
  <li><strong>질문</strong>조직 지식에 대해 Claude에게 묻기</li>
</ol>

connector를 더 많이 켤수록 검색 결과가 더 포괄적이에요. 나중에도 project의 **Instructions section**에서 `Connect`를 눌러 connector를 추가할 수 있어요.

## 4. 이렇게 많은 데이터, 안전한가요?

<div class="oa-eli">
  <span class="oa-eli__head">🔐 쉽게 말하면</span>
  <p>사서가 서류를 복사해서 자기 창고에 쌓아 두는 게 아니에요. 물어볼 때마다 <b>내 출입증을 들고 원래 서류함에 가서</b> 보고 와요. 그래서 내가 못 여는 서류함은 사서도 못 열어요.</p>
</div>

강의의 답은 간단해요. **네, 안전해요.**

> Enterprise Search는 원래 연결된 tool에서 **내가 이미 접근 권한을 가진 것만** 보여 줘요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🚪</span><span class="oa-card__title">기존 권한 그대로</span>내가 접근 권한이 없는 문서는 Claude도 볼 수 없어요. 원래 tool의 permission model이 그대로 적용돼요.</div>
  <div class="oa-card"><span class="oa-card__icon">🤫</span><span class="oa-card__title">대화는 private</span>내 대화는 private하게 유지돼요.</div>
  <div class="oa-card"><span class="oa-card__icon">🗄️</span><span class="oa-card__title">별도 저장 없음</span>연결된 데이터는 별도 index로 저장되거나 복사되지 않아요.</div>
</div>

정리하면 Enterprise Search는 조직 지식을 넓게 검색하지만, **접근 제어는 기존 system의 권한을 따라요.**

### 스스로 물어볼 질문

- 평소 동료에게 자주 묻는 질문 중 문서와 대화 검색으로 답할 수 있는 건 무엇인가요?
- 신규 입사자가 빠르게 맥락을 파악하는 데 Enterprise Search가 도움이 될 영역은 어디인가요?
- 내 역할에서 연결하면 가장 가치가 큰 data source는 무엇인가요?
- 회사의 정책, 프로세스, 과거 의사결정은 어디에 흩어져 있나요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Enterprise Search를 쓸 수 있는 조건으로 맞는 것은?</summary>
  <p>① Free plan에서도 누구나 바로 사용 ② Pro plan에서 개인이 직접 활성화 ③ Team/Enterprise plan에서 workspace admin이 활성화 ④ Claude Desktop app에서만 사용</p>
  <p><b>정답: ③</b> — Team과 Enterprise plan 기능이고, Owner가 초기 설정을 끝내야 구성원이 쓸 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. Admin 설정에서 반드시 골라야 하는 connector 종류는?</summary>
  <p>① Email만 ② Documents와 Chat ③ Documents, Chat, Email 모두 ④ 아무것도 필수가 아님</p>
  <p><b>정답: ②</b> — Documents(Google Drive, SharePoint 등)와 Chat(Slack, Microsoft Teams 등)은 필수이고, Email은 권장이지만 선택이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 동료만 볼 수 있는 비공개 문서에 답이 있어요. 내가 Enterprise Search로 물으면?</summary>
  <p>① 조직 기능이라 모든 문서가 검색돼요 ② admin이 연결했으니 내용이 보여요 ③ 내가 원래 접근 권한이 없는 문서라 Claude도 볼 수 없어요 ④ 요약만 보여 줘요</p>
  <p><b>정답: ③</b> — 원래 tool의 permission model이 그대로 적용돼요. 데이터를 별도로 index하거나 복사해 두지도 않아요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 일반 chat에서 connector를 켜는 것과 Enterprise Search의 차이로 맞는 것은?</summary>
  <p><b>정답:</b> Enterprise Search는 정보 수집·종합에 특화되어 있고, Anthropic 팀이 구성한 custom instructions를 쓰며, admin이 조직 단위로 설정해요. 응답에는 항상 source citation이 붙어요.</p>
</details>

## 정리

- **Enterprise Search**는 조직 내부 지식을 찾고 종합하는 Claude for Work 기능으로, sidebar에 `Ask {Org Name}` 형태의 전용 project가 생겨요.
- 여러 connected tools의 문서, 대화, email을 검색해 **하나의 응답으로 종합**하고, source citation을 붙여 검증과 추가 탐색이 쉬워요.
- **Team/Enterprise plan** 기능이에요. Owner가 조직 단위로 설정(Documents·Chat 필수, Email 권장)하고, 사용자는 개인 계정으로 각 service에 인증해요.
- connector를 많이 켤수록 결과가 포괄적이에요. 나중에 Instructions section에서 추가할 수 있어요.
- Claude는 **내가 원래 접근할 수 있는 데이터만** 볼 수 있어요. 대화는 private하고, 데이터는 별도로 index·저장되지 않아요.

다음 레슨에서는 public web과 connected integrations를 함께 활용해 더 깊은 조사를 수행하는 Research를 다뤄요.
