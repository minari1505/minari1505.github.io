---
title: "Meet Claude"
title_ko: "Claude 만나보기"
course: claude-101
lesson: 1
---

## 학습 목표

- Claude가 단순한 chatbot이 아니라 사고를 함께 정리하는 AI assistant라는 점 이해하기
- Claude가 잘하는 작업과 접근 가능한 인터페이스 구분하기
- Claude.ai에서 첫 대화를 시작하고 효과적인 prompt를 쓰는 기본 방법 익히기
- 파일과 이미지를 업로드해 Claude에게 더 많은 맥락을 주는 방법 이해하기
- 후속 메시지로 Claude의 답변을 반복 개선하는 습관 만들기

## Course roadmap

Claude 101은 Claude를 처음 쓰는 사람이 Claude와 협업하는 기본기를 익히는 강의입니다. 전체 흐름은 다음처럼 구성됩니다.

![Claude 101 course roadmap](/assets/images/courses/claude-101/course-roadmap.png)

이번 `Meet Claude`에서는 Claude가 무엇인지, 어떻게 말을 걸면 좋은지, 어떻게 하면 더 좋은 결과를 얻을 수 있는지부터 시작합니다.

## Claude는 무엇인가?

Claude는 단순히 질문에 답하는 chatbot이 아닙니다. 글쓰기, 분석, 조사, 코딩, 문제 해결처럼 다양한 지적 작업에서 함께 생각하고 정리하는 **AI assistant**에 가깝습니다.

Claude를 잘 쓰려면 "정답을 묻는 도구"보다 "생각을 함께 전개하는 파트너"로 보는 편이 좋습니다. 사용자는 목적, 맥락, 판단 기준을 제공하고, Claude는 그 정보를 바탕으로 초안 작성, 분석, 구조화, 아이디어 확장을 도와줍니다.

## Claude의 설계 원칙

Claude는 높은 수준에서 **helpful, harmless, honest**를 지향하도록 설계되었습니다.

- **Helpful**: 사용자의 목표를 이해하고 실제로 도움이 되는 답을 제공하려고 함
- **Harmless**: 유해하거나 차별적이거나 불법·비윤리적 행동을 돕는 출력을 피하려고 함
- **Honest**: 모르는 것은 모른다고 말하고, 가능한 한 투명하게 답하려고 함

Anthropic은 이런 접근을 **Constitutional AI**라고 설명합니다. Claude가 인간의 가치와 더 잘 맞고, 예측 가능하며, 대화하기 쉬운 AI system이 되도록 훈련하는 방식입니다.

## Claude가 잘하는 일

Claude는 단순 Q&A를 넘어 업무를 자동화하거나 보강하는 데 강합니다.

### 글쓰기와 콘텐츠 제작

Claude는 소셜 미디어 글, 업무 이메일, 보고서, 발표 자료 같은 글쓰기 작업을 함께 할 수 있습니다. 원하는 성격, 톤, 구조를 지시하면 그 방향에 맞춰 초안을 만들고, 사용자의 피드백을 받아 점점 다듬어갑니다.

좋은 활용 방식은 한 번에 완성본을 기대하는 것이 아니라, 구조와 표현을 함께 반복 개선하는 것입니다.

### 조사와 분석

Claude는 조사 방향을 잡고, 자료를 요약하고, 여러 문서에서 핵심을 뽑아 의미 있는 insight를 찾는 데 도움을 줍니다.

Claude는 큰 context window를 지원하므로 긴 문서나 여러 자료를 한 대화 안에서 함께 고려할 수 있습니다. 일반적으로 200K+ token 수준의 긴 입력을 다룰 수 있고, 지원 모델과 plan에 따라 Pro, Max, Team, Enterprise에서는 더 큰 context를 사용할 수도 있습니다.

즉, 문서 하나만 요약하는 수준을 넘어 많은 자료를 넣고 비교, 정리, 분석하는 작업에 적합합니다.

### 코딩 지원

코딩은 Claude의 강점 중 하나입니다. Claude는 여러 프로그래밍 언어에서 코드 작성, 디버깅, 설명, 리팩터링을 도울 수 있습니다.

개발 workflow에 더 깊게 연결하고 싶다면 별도 강의인 `Claude Code in Action`에서 Claude Code를 다룹니다. Claude Code는 파일 편집, 명령 실행, commit 생성까지 수행할 수 있는 agentic coding tool입니다.

### 문제 해결과 reasoning

Claude는 복잡한 인지 작업, 수학 문제, 전략적 사고, 분석, 연구에 사용할 수 있습니다. 최근 Opus와 Sonnet 계열 모델은 빠른 응답과 깊은 reasoning을 위한 extended thinking을 함께 지원하는 hybrid model로 소개됩니다.

Extended thinking은 Claude가 문제를 단계적으로 검토해야 하는 작업에 특히 유용합니다.

### 새로운 것 배우기

Claude는 새로운 기술을 배우거나 낯선 분야를 탐색할 때도 쓸 수 있습니다. 사용자의 이해 수준과 학습 속도에 맞춰 설명 방식을 바꿀 수 있고, Learning mode처럼 바로 답을 주기보다 reasoning 과정을 안내하는 경험도 제공합니다.

## Claude를 사용할 수 있는 곳

Claude는 하나의 intelligence이고, 이 intelligence를 여러 인터페이스에서 사용할 수 있습니다.

| 인터페이스 | 적합한 작업 |
|---|---|
| **Claude.ai / mobile / desktop app** | 대화, 글쓰기, 조사, 분석, 파일 생성과 편집 |
| **Claude Code** | 개발자 workflow, 파일 수정, 명령 실행, commit 생성 |
| **Claude and Slack** | 팀 채널, thread, DM, 공유 파일을 바탕으로 한 대화와 조사 |
| **Claude for Excel** | Excel workbook 분석, 수정, 공식 설명, multi-tab navigation |

이 강의는 주로 Claude.ai 사용법에 초점을 둡니다. Claude.ai에서는 질문하기, 아이디어 브레인스토밍, 문서 생성과 편집, 파일 업로드 기반 분석 같은 기본 업무 흐름을 익힙니다.

## Claude.ai에서 첫 대화 시작하기

Claude.ai를 열면 화면 아래쪽에 text input area가 있는 깔끔한 인터페이스를 볼 수 있습니다. 여기에 prompt를 입력하면 대화가 시작됩니다.

Prompt는 아주 단순한 질문일 수도 있고, 파일을 함께 만들거나 긴 보고서를 작성하는 복잡한 요청일 수도 있습니다.

예를 들어 다음처럼 시작할 수 있습니다.

```text
새 기능의 코드명을 10개 브레인스토밍해줘.
```

또는 더 복잡하게 요청할 수도 있습니다.

```text
첨부한 회의록을 바탕으로 결정사항, 액션 아이템, 리스크를 정리해줘.
```

## 좋은 prompt의 기본

Claude에게 말할 때 가장 좋은 방식은 동료에게 설명하듯 자연스럽고, 간결하고, 대화하듯 말하는 것입니다.

다만 좋은 결과를 얻으려면 다음 세 가지를 생각하면 좋습니다.

### 1. 무대 설정하기

내 역할, 목표, 업무 맥락을 알려줍니다.

```text
나는 B2B SaaS 회사의 마케팅 리드야.
Series A 투자자용 pitch deck을 준비하고 있어.
```

### 2. 작업 정의하기

Claude가 어떤 행동을 해야 하는지 명확히 말합니다.

```text
독립 영화 스트리밍 시장의 현재 동향, 경쟁사 포지셔닝, 성장 기회를 조사해줘.
```

### 3. 규칙 지정하기

원하는 형식, 톤, 길이, 예시, 제약 조건을 알려줍니다.

```text
최신 웹 조사와 citation을 포함하고,
executive summary, market analysis, competitive landscape, growth opportunities 섹션으로 구성한
최대 5페이지 전문 보고서 형식으로 작성해줘.
```

이 세 가지를 합치면 Claude가 훨씬 더 정확하게 기대 결과를 이해합니다.

## 예시 prompt 분석

강의의 예시 prompt는 다음 구조를 갖습니다.

```text
I'm the marketing lead at an indie streaming startup, and we're preparing an investor pitch deck for Series A investors.
Can you research the current state of the independent film streaming market and identify key trends, competitor positioning, and growth opportunities?
Use current web research with citations and structure it as a professional report of up to 5 pages, with an executive summary, market analysis, competitive landscape, and growth opportunities.
```

이 prompt에는 세 요소가 모두 들어 있습니다.

- **무대 설정**: indie streaming startup의 marketing lead, Series A 투자자용 pitch deck 준비
- **작업 정의**: 시장 상태, trend, competitor positioning, growth opportunity 조사
- **규칙 지정**: citation 포함, 전문 보고서 형식, 최대 5페이지, 특정 섹션 구성

Claude에게 더 좋은 답을 받는 핵심은 "많이 말하기"가 아니라 **Claude가 판단하는 데 필요한 맥락과 기준을 정확히 주는 것**입니다.

## 파일과 이미지로 맥락 추가하기

Claude는 대화에 업로드한 문서와 이미지를 함께 고려할 수 있습니다.

지원되는 파일 예시는 다음과 같습니다.

- PDF
- DOCX
- CSV
- TXT
- PNG, JPEG 같은 일반 이미지 형식

실무에서는 다음처럼 활용할 수 있습니다.

- 문서를 업로드하고 핵심 요약 요청
- 이미지를 공유하고 내용 설명 또는 분석 요청
- spreadsheet를 첨부하고 데이터 trend 파악 요청
- 코드를 업로드하고 동작 설명 또는 bug 찾기 요청

파일을 업로드하면 Claude는 가능한 범위에서 내용을 parsing하고, 대화 안에서 해당 파일을 attachment로 표시합니다. 이후 사용자는 그 파일에 대해 질문하거나 분석을 요청할 수 있습니다.

## 개인 선호와 반복 대화

Claude는 한 번의 prompt로 끝나는 도구가 아닙니다. 실제 힘은 짧은 prompt를 이어가며 대화식으로 개선할 때 나옵니다.

첫 답변이 기대와 다르면 다음처럼 이어갈 수 있습니다.

- 더 자세히 요청: "두 번째 포인트를 더 확장해줘."
- 더 짧게 요청: "좋은데 더 간결하게 만들어줘."
- 톤 수정: "너무 격식 있어. 더 대화체로 바꿔줘."
- 방향 수정: "내가 말한 건 X가 아니라 Y였어. 다시 설명할게."
- 이전 prompt 수정: pencil icon으로 내가 보낸 메시지를 편집해 다시 제출

또한 Claude에는 개인화 기능도 있습니다.

### Memory

Memory는 대화에서 나온 중요한 맥락을 저장해 이후 대화에 반영합니다. 예를 들어 사용자가 B2B 회사의 마케팅 담당자라고 알려주면, Claude가 이후에도 그 맥락을 기억할 수 있습니다.

저장된 memory는 Settings에서 확인, 수정, 삭제할 수 있고, 로그인한 기기 간에 동기화됩니다.

### Styles

Styles는 Claude가 말하는 방식을 조정하는 기능입니다. concise, formal, explanatory 같은 preset을 고르거나, 직접 원하는 writing style을 정의할 수 있습니다.

한 번 style을 설정하면 여러 대화에서 일관되게 적용됩니다.

## 실습 질문

다음으로 넘어가기 전에 현재 업무에서 Claude를 thinking partner로 쓸 수 있는 일을 몇 가지 떠올려보면 좋습니다.

- 이번 주 일정 중 Claude가 도와줄 수 있는 작업은 무엇인가?
- 반복해서 작성하는 문서나 메시지가 있는가?
- 분석해야 할 자료나 회의록이 있는가?
- 더 좋은 초안을 만들기 위해 함께 브레인스토밍할 주제가 있는가?

Claude에게 직접 이렇게 물어볼 수도 있습니다.

```text
내 이번 주 캘린더를 보고 Claude가 도와줄 수 있는 작업 후보를 찾아줘.
```

## 핵심 정리

Claude는 AI intelligence를 제공하지만, 의미 있는 결과를 만드는 맥락과 전문성은 사용자가 제공합니다.

- Claude는 helpful, harmless, honest를 지향하도록 설계된 AI assistant입니다.
- Claude는 글쓰기, 조사, 분석, 코딩, reasoning, 학습 지원에 강합니다.
- Claude에게는 동료에게 말하듯 자연스럽고 구체적으로 요청하는 것이 좋습니다.
- 좋은 prompt는 역할과 목표를 알려주고, 작업을 정의하고, 결과물의 규칙을 지정합니다.
- 파일과 이미지를 업로드하면 Claude가 더 많은 맥락을 고려할 수 있습니다.
- Claude의 진짜 힘은 일회성 prompt보다 반복 대화와 피드백에서 나옵니다.

다음 레슨에서는 Claude에게 원하는 톤, 형식, 접근 방식을 더 정확히 지시하는 방법을 다룹니다.
