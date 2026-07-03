---
title: "Projects"
title_ko: "Projects로 작업과 지식 정리하기"
course: claude-101
lesson: 4
---

## 학습 목표

- Claude의 Projects가 무엇이고 언제 쓰면 좋은지 이해하기
- 이름, 설명, 공개 범위를 지정해 새 project를 만드는 흐름 익히기
- project knowledge base에 문서와 파일을 추가하는 방법 정리하기
- project instructions로 Claude의 응답 방식과 작업 절차를 고정하는 방법 배우기
- Team/Enterprise 환경에서 project를 공유하고 권한을 관리하는 방식 이해하기

## Projects란?

**Projects**는 특정 업무 흐름을 위한 독립 workspace입니다. 각 project는 자체 memory, chat history, knowledge base, customized instructions를 가질 수 있습니다.

일회성 질문이라면 일반 chat으로 충분합니다. 하지만 반복적으로 같은 자료를 참조하거나, 같은 규칙으로 답변을 받아야 하거나, 팀원들과 같은 맥락을 공유해야 한다면 project가 더 적합합니다.

Projects를 다음처럼 생각하면 됩니다.

```text
특정 업무 영역
  ├─ 관련 대화
  ├─ 참고 문서
  ├─ 프로젝트별 지시사항
  └─ 팀과 공유하는 작업 맥락
```

## 언제 Projects를 쓰면 좋은가?

Project는 계속 이어지는 작업에 특히 유용합니다.

- 반복해서 참조할 자료가 있을 때: 회의록, 설문 결과, 보고서, 과거 데이터
- Claude가 항상 지켜야 할 응답 규칙이 있을 때: 공식 톤, citation 포함, 정해진 template 사용
- 여러 사람이 같은 기반 지식과 instructions로 협업해야 할 때
- 특정 고객, 제품, 캠페인, 연구 주제처럼 오래 유지되는 업무 흐름이 있을 때

핵심은 **같은 맥락을 매번 다시 설명하지 않아도 된다**는 점입니다.

## 첫 Project 만들기

Project를 만드는 흐름은 간단합니다.

1. 왼쪽 sidebar에서 `Projects`를 선택하거나 `claude.ai/projects`로 이동합니다.
2. 오른쪽 위의 `+ New Project`를 클릭합니다.
3. 설명적인 이름을 붙입니다. 예: `Q4 Marketing Campaign`, `Product Documentation`
4. 무엇을 위한 project인지 간단한 설명을 작성합니다.
5. 공개 범위를 선택합니다. 개인용으로 private하게 둘 수도 있고, Claude for Work 사용자는 조직에 공유할 수도 있습니다.

Project 설명은 Claude가 직접 읽는 instruction은 아니지만, 나와 팀원이 project 목적을 이해하는 데 도움이 됩니다.

## Project instructions 작성하기

Project instructions는 이 project 안의 모든 대화에서 Claude가 따라야 할 기본 규칙입니다.

좋은 project instructions에는 보통 다음이 들어갑니다.

- **업무 맥락**: "이 project는 B2B 소프트웨어 제품의 마케팅 콘텐츠 작성을 위한 것이다."
- **작업 절차**: "먼저 대상 독자를 끌어들일 blog 구조를 제안한 뒤 초안을 작성한다."
- **톤과 스타일**: "전문적이지만 대화체를 유지하고, 가능하면 jargon은 피한다."
- **구체 요구사항**: "마케팅 copy 끝에는 항상 call-to-action을 포함한다."

Instructions는 Claude에게 반복적으로 적용되는 행동 규칙입니다. 예를 들어 다음처럼 workflow를 자동화할 수도 있습니다.

```text
회의 transcript를 업로드하면, 항상 다음 template에 맞춰 구조화된 요약을 만들어줘:
1. 핵심 결정
2. 액션 아이템
3. 미해결 질문
4. 후속 일정
```

Project instructions는 user preferences와 styles와 함께 적용됩니다. 따라서 project 단위로 더 구체적인 작업 방식과 기준을 정해두는 용도로 쓰면 좋습니다.

## Knowledge base 만들기

Project knowledge base는 Claude가 project 안의 모든 chat에서 참조할 수 있는 문서 저장소입니다.

업로드할 수 있는 자료 예시는 다음과 같습니다.

- Brand guidelines, style guides, templates
- Research reports, meeting notes, requirements docs
- Claude가 따라 했으면 하는 좋은 작업 예시
- Technical documentation, specifications
- CSV, PDF, DOCX, TXT, HTML 등 다양한 파일

Google Drive와 연결해 문서를 직접 link할 수도 있습니다.

파일 이름은 최대한 설명적으로 붙이는 것이 좋습니다.

```text
좋음: Q4-2025-Sales-Report.pdf
애매함: report.pdf
나쁨: document1.pdf
```

Claude는 파일 이름을 참고해 어떤 자료를 찾고 사용할지 판단하므로, 이름 자체가 retrieval cue 역할을 합니다.

## 큰 knowledge base는 어떻게 처리되는가?

Project에 많은 자료를 넣으면 context window 한계가 걱정될 수 있습니다. Claude Projects는 knowledge base가 커지면 자동으로 **Retrieval Augmented Generation(RAG)** 방식을 사용합니다.

전체 문서를 한 번에 모두 context에 넣는 대신, 질문에 답하는 데 가장 관련 있는 부분을 찾아 가져오는 방식입니다.

```text
사용자 질문
  ↓
Project knowledge에서 관련 정보 검색
  ↓
가장 관련 있는 부분만 context로 사용
  ↓
Claude가 답변 생성
```

강의에서는 project knowledge가 context limit에 가까워지면 RAG mode가 자동으로 켜지고, capacity가 최대 10배까지 확장될 수 있다고 설명합니다. 사용자는 visual indicator를 볼 수 있지만, 기본 사용 경험은 거의 동일합니다.

## Project 안에서 작업하기

Project를 만들고 instructions와 knowledge base를 넣었다면, 이제 project 안에서 Claude와 대화하면 됩니다.

Project 안의 모든 conversation은 다음을 자동으로 활용합니다.

- Project knowledge base
- Project instructions
- Project의 대화 맥락과 memory

즉, 같은 문서를 매번 다시 업로드하거나 같은 규칙을 다시 설명할 필요가 줄어듭니다.

## 협업 기능

Claude for Work, 즉 Team과 Enterprise plan에서는 project를 팀과 공유할 수 있습니다.

### 권한 수준

| 권한 | 의미 |
|---|---|
| **Can view** | project 내용과 knowledge를 보고 chat할 수 있지만 수정은 불가 |
| **Can edit** | instructions, knowledge, members를 수정하고 actively collaborate 가능 |
| **Owner** | project 생성자로서 공유 범위와 전체 설정을 제어 |

### 공유 방법

Project를 공유하려면 다음 흐름을 따릅니다.

1. 공유할 project를 엽니다.
2. project 이름 오른쪽의 `Share project`를 클릭합니다.
3. 이름이나 email로 개별 member를 추가합니다.
4. 여러 email을 붙여 넣어 bulk sharing할 수도 있습니다.
5. 조직 전체에 공유하려면 `Everyone at [organization]` 옵션을 사용할 수 있습니다.

공유된 project는 팀원의 `Shared with me` 영역에 표시되고, 이메일 알림도 전송됩니다.

## Project 예시

어디서 시작할지 모르겠다면 다음 유형을 생각해볼 수 있습니다.

- **Q4 product launch**: 제품 spec, 경쟁 분석, messaging note를 모아 launch 문서 초안 작성
- **Research support**: 경쟁사 review, 사용자 조사, 고객 feedback을 중앙화하고 insight 정리
- **Client account hub**: 고객 brand guideline, 과거 산출물, 커뮤니케이션 이력을 모아 proposal 작성
- **Event planning workspace**: venue contract, speaker bio, attendee data를 모아 행사 운영 문서와 후속 보고서 작성
- **Job description generator**: 과거 JD, team charter, headcount request를 모아 실제 팀 문화에 맞는 JD 작성

## Best practices

Projects를 잘 쓰려면 다음 원칙이 유용합니다.

- 처음부터 모든 것을 넣으려 하지 말고, 구체적인 use case 하나로 시작합니다.
- knowledge base는 주기적으로 업데이트합니다. 오래된 문서는 오래된 답변을 만들 수 있습니다.
- instructions는 모호하게 쓰지 말고 구체적으로 작성합니다.
- 문서 이름은 설명적으로 붙이고, 관련 파일은 함께 묶습니다.
- 질문할 때 특정 문서를 이름으로 언급하면 Claude가 더 잘 focus할 수 있습니다.

```text
Q3 report를 기준으로 고객이 가장 많이 제기한 우려사항을 정리해줘.
```

## 핵심 정리

Projects는 Claude에게 장기적이고 반복적인 업무 맥락을 제공하는 workspace입니다.

- 반복 사용 자료는 project knowledge base에 넣습니다.
- 항상 지켜야 할 작업 방식은 project instructions에 씁니다.
- Team/Enterprise에서는 project를 공유해 팀 전체가 같은 context에서 작업할 수 있습니다.
- knowledge base가 커지면 RAG mode로 관련 정보만 검색해 사용합니다.

Projects는 "자료를 보관하는 폴더"가 아니라, Claude가 특정 업무를 더 일관되게 이해하고 수행하도록 만드는 **맥락 저장소**입니다.
