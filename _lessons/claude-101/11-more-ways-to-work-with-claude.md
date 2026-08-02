---
title: "More ways to work with Claude"
title_ko: "Claude를 사용하는 다른 방법들"
course: claude-101
lesson: 11
---

## 학습 목표

- Claude.ai 외에 Claude를 사용할 수 있는 주요 제품을 구분하기
- Claude Code, Claude in Slack, Claude for Excel, Claude for PowerPoint, Claude for Chrome을 언제 쓰면 좋은지 이해하기
- 작업 환경에 따라 적절한 Claude interface를 선택하는 기준 세우기

## Claude는 intelligence이고, Claude.ai는 한 interface일 뿐

이 강의의 시작에서 Claude는 하나의 intelligence라고 했습니다. Claude.ai는 Claude와 일하는 대표적인 방법이지만 유일한 방법은 아닙니다.

Claude는 사용자가 이미 일하고 있는 환경에 맞춰 여러 specialized tool로 제공됩니다.

- 개발자는 terminal, IDE, browser에서 Claude Code를 사용할 수 있습니다.
- 팀은 Slack 안에서 Claude를 호출할 수 있습니다.
- 재무와 분석 업무는 Excel sidebar에서 Claude를 사용할 수 있습니다.
- 발표 자료 작업은 PowerPoint 안에서 Claude와 함께 할 수 있습니다.
- 웹 탐색과 반복 작업은 Chrome extension으로 처리할 수 있습니다.

중요한 것은 "어떤 Claude가 더 좋은가"가 아니라 **내가 지금 하는 작업이 어디에서 일어나고, 어떤 권한과 맥락이 필요한가**입니다.

## Claude Code

Claude Code는 terminal, IDE, browser, Slack 등 개발자가 일하는 환경에서 작동하는 agentic coding tool입니다.

Claude Code는 codebase를 이해하고, 명령을 실행하고, 자연어 요청을 바탕으로 전체 개발 workflow를 처리할 수 있습니다.

Claude Code가 적합한 상황:

- 자연어로 기능을 설명하면 Claude가 코드를 작성하고, 테스트를 실행하고, commit까지 만들게 하고 싶을 때
- error message를 붙여넣고 codebase를 분석해 bug를 찾고 수정하게 하고 싶을 때
- 낯선 codebase에서 여러 부분이 어떻게 연결되는지 질문하고 싶을 때
- lint error 수정, merge conflict 해결, release note 작성 같은 반복 작업을 자동화하고 싶을 때
- 별도 interface로 옮기기보다 terminal과 기존 IDE 옆에서 작업하고 싶을 때

예시:

```text
이 에러가 어디서 발생하는지 찾아서 원인을 설명하고, 최소 수정으로 고쳐줘. 테스트도 실행해줘.
```

Claude Code는 소프트웨어 개발을 위한 Claude입니다.

## Claude in Slack

Claude는 Slack과 직접 통합될 수 있습니다. 채널과 thread 안에서 Claude를 호출하거나, Slack 맥락을 Claude conversation으로 가져올 수 있습니다.

Claude in Slack이 적합한 상황:

- Slack을 떠나지 않고 메시지 답변 초안을 쓰고 싶을 때
- 긴 thread를 요약하거나 복잡한 논의를 정리하고 싶을 때
- 미팅 준비를 위해 workspace의 관련 대화와 공유 문서를 모으고 싶을 때
- 새 팀에 합류해 channel history를 통해 ongoing project를 이해하고 싶을 때
- bug report나 feature discussion에서 바로 coding task를 넘기고 싶을 때
- 대화 중 업계 trend, 기술 개념, 회사 정보를 빠르게 묻고 싶을 때

예시:

```text
@Claude 이 thread에서 결정된 내용과 남은 액션 아이템을 요약해줘.
```

또는 bug report thread에서 Claude를 tag해 Claude Code session을 시작할 수도 있습니다.

## Claude for Excel

Claude for Excel은 Microsoft Excel 안의 sidebar로 Claude를 불러오는 기능입니다. Spreadsheet를 대화로 분석하고, 이해하고, 수정할 수 있습니다.

Claude for Excel이 적합한 상황:

- 여러 tab으로 구성된 복잡한 workbook에서 formula나 calculation flow를 이해하고 싶을 때
- model의 assumption이나 input을 바꾸되 formula dependency를 유지하고 싶을 때
- `#REF!`, `#VALUE!`, circular reference 같은 spreadsheet error를 tracing하고 싶을 때
- 새 spreadsheet를 만들거나 기존 template에 data를 채우고 싶을 때
- pivot table이나 chart를 빠르게 만들어 data를 시각화하고 싶을 때

예시:

```text
이 workbook에서 revenue forecast가 어떤 sheet와 formula를 거쳐 계산되는지 설명해줘.
```

Finance, operations, analysis 업무에서 특히 유용합니다.

## Claude for PowerPoint

Claude for PowerPoint는 Microsoft PowerPoint 안의 sidebar로 Claude를 사용하는 기능입니다. 기존 template과 brand styling을 유지하면서 slide deck을 작성, 편집, 재구성할 수 있습니다.

Claude for PowerPoint가 적합한 상황:

- outline, document, notes를 첫 slide deck 초안으로 바꾸고 싶을 때
- slide copy를 더 짧고 명확하게 만들고 싶을 때
- speaker notes를 추가하거나 audience에 맞게 tone을 조정하고 싶을 때
- 기존 deck의 section 순서를 바꾸거나 dense slide를 나누고 싶을 때
- deck 전체에 title, bullet style, layout formatting을 일관되게 적용하고 싶을 때
- 특정 slide에 어떤 layout이나 chart type이 적합한지 시각적 제안을 받고 싶을 때

예시:

```text
이 strategy memo를 12장짜리 executive presentation으로 바꿔줘.
기존 template과 brand style은 유지해줘.
```

## Claude for Chrome

Claude for Chrome은 Google Chrome browser sidebar에서 Claude를 사용할 수 있는 extension입니다. Claude가 현재 사용자가 보는 웹페이지를 관찰하고, browser 안에서 직접 action을 수행할 수 있습니다.

Claude for Chrome이 적합한 상황:

- 탐색 중인 article, research paper, web page를 요약하고 싶을 때
- email response 초안 작성이나 inbox 관리 도움을 받고 싶을 때
- 반복적인 form 입력을 자동화하고 싶을 때
- website feature를 테스트하거나 multi-step workflow를 대신 탐색하게 하고 싶을 때
- niche internal tool, CRM, dashboard처럼 connector가 없는 web app에서 context를 가져오고 싶을 때

예시:

```text
이 dashboard에서 지난달 conversion이 떨어진 원인을 찾는 데 필요한 지표를 읽고 요약해줘.
```

주의할 점: Claude for Chrome은 현재 research preview입니다. Anthropic은 신뢰할 수 있는 website에서 low-risk task에 사용하는 것을 권장합니다. 구매나 개인 정보 공유 같은 high-risk action 전에는 permission을 요청하고, 금융 서비스나 adult content 같은 일부 website category는 기본적으로 차단됩니다.

## 도구별 비교

| Tool | 적합한 작업 | 실행 위치 |
|---|---|---|
| **Claude.ai** | 일반 업무, research, writing, analysis, file creation | Web, desktop, mobile apps |
| **Claude Code** | software development, codebase navigation, git workflow | Terminal/command line, IDE, browser |
| **Claude Cowork** | 복잡한 multi-step task, research brief, document creation, file organization, data analysis | Desktop, mobile via Dispatch |
| **Claude / Claude Code in Slack** | 팀 협업, meeting prep, 맥락 안의 빠른 답변 | Slack workspace |
| **Claude for Excel** | spreadsheet analysis, financial modeling, formula debugging | Microsoft Excel sidebar |
| **Claude for PowerPoint** | slide creation, presentation editing, formatting, design | Microsoft PowerPoint sidebar |
| **Claude for Chrome** | web research, email management, browser automation | Chrome browser sidebar |

## 어떤 Claude를 선택할까?

작업 환경을 기준으로 고르면 쉽습니다.

- 대화, 글쓰기, 조사, 파일 생성 → **Claude.ai**
- 코드베이스 안에서 개발 작업 → **Claude Code**
- 여러 source를 모아 긴 지식 작업 → **Claude Cowork**
- 팀 대화 맥락에서 즉시 도움 → **Claude in Slack**
- spreadsheet 분석과 모델링 → **Claude for Excel**
- slide deck 작성과 편집 → **Claude for PowerPoint**
- browser 안의 web workflow → **Claude for Chrome**

같은 Claude intelligence라도, interface가 달라지면 접근할 수 있는 context와 action이 달라집니다.

## 실습 질문

다음 질문으로 내 업무에 맞는 interface를 골라볼 수 있습니다.

- 내 업무 중 대부분은 web chat에서 충분한가, 아니면 특정 tool 안에서 Claude가 필요할까?
- 개발 관련 작업은 Claude.ai보다 Claude Code가 더 적합하지 않은가?
- Slack 대화에서 바로 요약하거나 action item을 뽑으면 좋은 thread는 무엇인가?
- Excel이나 PowerPoint에서 반복적으로 하는 수작업은 무엇인가?
- Browser에서 반복 클릭하거나 여러 페이지를 탐색하는 workflow가 있는가?

## 핵심 정리

Claude는 하나의 intelligence이고, 여러 작업 환경에 맞춰 다양한 interface로 제공됩니다.

- Claude.ai는 범용 대화와 지식 작업의 중심입니다.
- Claude Code는 개발 workflow에 특화되어 있습니다.
- Claude in Slack은 팀 커뮤니케이션 맥락에서 Claude를 호출하게 해줍니다.
- Claude for Excel과 PowerPoint는 Microsoft Office 안에서 직접 작업하게 해줍니다.
- Claude for Chrome은 browser 안의 정보와 action을 Claude에게 연결합니다.

다음 단계에서는 이 강의를 짧게 recap하고, certificate를 받기 위한 quiz로 마무리합니다.
