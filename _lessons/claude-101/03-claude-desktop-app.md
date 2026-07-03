---
title: "Claude desktop app"
title_ko: "Claude 데스크톱 앱"
course: claude-101
lesson: 3
---

## 학습 목표

- Claude desktop app의 세 가지 모드인 Chat, Cowork, Code를 구분하기
- quick entry, screenshot, scheduled tasks, subagents, local/remote development 같은 핵심 기능 이해하기
- 작업 유형에 따라 어떤 모드를 선택해야 하는지 판단하기

## Claude desktop app의 세 가지 모드

Claude desktop app은 Claude와 일하는 방식을 세 가지로 나눕니다.

1. **Chat**: 빠른 질문, 브레인스토밍, 초안 작성, 대화형 문제 해결
2. **Cowork**: 여러 source를 읽고, 계획을 세우고, 완성된 deliverable을 만드는 agentic work
3. **Code**: 코드 작성, 수정, 테스트, 배포 등 소프트웨어 개발

각 모드는 같은 Claude intelligence를 기반으로 하지만, 사용 목적과 제어 방식이 다릅니다.

```text
Chat   → 빠르고 대화적인 Claude
Cowork → 자료를 모으고 완성물을 만드는 Claude
Code   → 코드베이스 안에서 개발 작업을 수행하는 Claude
```

Cowork와 Code는 내부적으로 Claude Code 기반의 agentic engine을 사용합니다. 로컬 머신에서 실행되고, 독립적인 작업을 이어가며, sub-agent를 띄워 긴 작업을 처리할 수 있습니다.

## Chat

![Claude desktop Chat mode](/assets/images/courses/claude-101/desktop-chat.png)

Chat은 claude.ai에서 익숙한 Claude와 가장 비슷합니다. 질문하고, 아이디어를 브레인스토밍하고, 글을 쓰고, 문제를 대화식으로 풀 때 적합합니다.

desktop app에서는 native app이기 때문에 몇 가지 기능이 더해집니다.

### Quick entry

Mac에서 Option 키를 두 번 누르면 현재 작업 중인 화면 위에 Claude를 띄울 수 있습니다. 앱을 옮겨 다니지 않고도 바로 질문할 수 있고, compact window가 위에 유지됩니다.

### Screenshots and window sharing

화면을 캡처하거나 window를 공유하면 Claude가 지금 보고 있는 것을 직접 볼 수 있습니다. 화면 상태를 말로 길게 설명하는 것보다 빠르고 정확합니다.

### Dictation

타이핑 대신 말로 문제를 설명할 수 있습니다. 키보드에서 떨어져 있거나, 생각을 말로 정리하는 편이 더 빠른 상황에 유용합니다.

### Desktop connectors

로컬 도구와 서비스를 connector로 연결하면 Claude가 내 컴퓨터의 다른 tool과 함께 작업할 수 있습니다.

### Chat이 적합한 상황

- 낯선 dashboard를 보고 metric 의미를 묻고 싶을 때
- 회의 사이에 발표 구조를 빠르게 잡고 싶을 때
- 메모 앱에 흩어진 아이디어를 모아 정리하고 싶을 때
- 짧은 질문과 iterative drafting이 필요한 경우

Chat은 **짧고 빠른 대화형 협업**에 가장 적합합니다.

## Cowork

![Claude desktop Cowork mode](/assets/images/courses/claude-101/desktop-cowork.png)

Cowork는 실제로 시간이 걸리는 업무를 맡기기 위해 설계된 agentic tool입니다. 여러 source에서 정보를 가져오고, 의미를 정리하고, 완성된 문서나 deliverable을 만드는 작업에 적합합니다.

예를 들어 다음 같은 작업이 Cowork에 잘 맞습니다.

- 여러 문서와 Slack, email을 읽고 research brief 작성
- 여러 source를 바탕으로 financial analysis 수행
- 계약서와 관련 문서를 검토하고 risk 정리
- 흩어진 자료를 바탕으로 slide deck 초안 작성

Cowork는 시작 전에 scope, format, constraint를 확인하기 위한 짧은 질문을 할 수 있습니다. 이후 sidebar에서 plan을 보여주고, 진행 중에는 어떤 source를 보고 있는지, 어떤 파일을 만들고 있는지, 계획의 어느 단계인지 확인할 수 있습니다.

### Folder access

컴퓨터의 folder를 지정하면 Claude가 그 안의 파일을 읽고, 관련 자료를 찾고, 완성된 결과를 같은 위치에 저장할 수 있습니다.

### Scheduled tasks

반복 작업을 일정에 맞춰 실행할 수 있습니다.

예:

- 매일 아침 Slack과 calendar에서 daily briefing 만들기
- 매주 shipped item 요약하기
- 아침 inbox triage 수행하기

앱이 열려 있고 컴퓨터가 켜져 있으면 일정에 맞춰 실행되고, 예정 시각에 닫혀 있었다면 다시 돌아왔을 때 따라잡을 수 있습니다.

### Subagents

복잡한 작업을 여러 하위 작업으로 나눠 background worker에게 맡길 수 있습니다. 예를 들어 여러 source에서 research brief를 만들어야 한다면 Claude가 subtasks로 나누고, 각각을 subagent에게 맡긴 뒤 결과를 종합합니다.

### Dispatch

모바일 앱에서 desktop의 파일, connectors, plugins, desktop apps를 활용하는 Cowork 작업을 이어갈 수 있는 persistent conversation thread입니다. 사용하려면 desktop과 mobile app이 모두 필요하고, 컴퓨터가 켜져 있으며 desktop app이 열려 있어야 합니다.

### Projects

Cowork의 Projects는 관련 작업을 전용 workspace로 묶습니다. 각 project는 파일, context, instruction, memory를 가질 수 있습니다. Claude.ai의 Projects와 비슷하지만, desktop 로컬 작업을 중심으로 구성됩니다.

### Browser use

Claude in Chrome을 연결하면 Claude가 웹사이트를 탐색하고, 페이지와 상호작용하며, 필요한 정보를 task로 가져올 수 있습니다. API가 없는 웹페이지에서 경쟁사 가격을 확인하거나 여러 사이트에서 자료를 모으는 작업에 유용합니다.

### Computer use

connector나 plugin으로 해결할 수 없는 경우 Claude가 컴퓨터 화면을 직접 조작할 수 있습니다. 클릭, 타이핑, 앱 열기 같은 작업을 사람처럼 수행합니다.

Claude는 보통 다음 우선순위로 접근합니다.

1. connectors
2. Chrome
3. screen interaction

더 빠르고 안정적인 방법을 먼저 고르는 방식입니다. 앱 접근 전에는 permission prompt가 표시되고, blocklist로 접근 금지 대상을 지정할 수 있습니다.

### Plugins

Plugins는 Claude가 기본적으로 갖지 않은 능력을 추가합니다. 예를 들어 live financial data를 가져오거나, 회사 내부 knowledge base를 검색하거나, 특정 compliance framework 안에서 작업하게 할 수 있습니다.

### Protected environment

Cowork는 컴퓨터 안의 제한된 공간에서 실행됩니다. 사용자가 공유한 folder 안에서는 파일을 읽고 만들고 수정할 수 있지만, 공유하지 않은 영역에는 접근할 수 없습니다.

### Cowork가 적합한 상황

- 회의록, Slack, email, deck에서 지난 의사결정을 찾아 정리해야 할 때
- 여러 웹사이트를 조사해 structured brief를 만들어야 할 때
- 50개 이상의 문서, 계약서, 재무 자료를 함께 검토해야 할 때
- 매일 반복하는 briefing, inbox triage, status update를 자동화하고 싶을 때

Cowork는 **여러 source를 읽고 오래 걸리는 지식 작업을 완성물로 만드는 모드**입니다.

## Code

![Claude desktop Code mode](/assets/images/courses/claude-101/desktop-code.png)

Code 탭은 Claude Code의 기능을 desktop app 안에서 제공합니다. 소프트웨어 개발을 위한 full development environment에 가깝습니다.

Code에서 Claude는 코드베이스 안에서 직접 작업합니다.

- 기존 코드 읽기
- 파일 작성과 수정
- 명령 실행
- 테스트 실행
- visual diff로 변경 확인
- terminal output 확인
- git으로 version 추적

Cowork가 공유된 folder 안의 제한된 workspace에서 동작한다면, Code는 project의 filesystem, terminal, development tools에 더 직접적으로 접근합니다.

## Local과 Remote 개발

Code에서는 작업이 어디에서 실행될지 선택할 수 있습니다.

### Local

내 컴퓨터의 folder를 선택하면 Claude가 그 파일을 직접 다룹니다. 로컬 tool에 접근하고, 개발 서버를 실행하고, 브라우저에서 preview할 수 있습니다.

로컬 환경이 필요한 프로젝트나 즉시 확인이 필요한 frontend 작업에 적합합니다.

### Remote

GitHub repository를 연결하면 Claude가 cloud environment에서 작업합니다. 앱을 닫아도 session이 계속될 수 있어 큰 refactor를 맡기고 나중에 돌아와 확인할 수 있습니다.

큰 코드베이스를 다루거나 개발 환경을 로컬 머신에서 분리하고 싶을 때 유용합니다.

## Code의 세 가지 interaction mode

Code 탭에는 Claude가 얼마나 자율적으로 작업할지 조절하는 세 가지 mode가 있습니다.

| Mode | 동작 방식 |
|---|---|
| **Ask** | Claude가 모든 변경을 제안하고 승인 전까지 수정하지 않음 |
| **Code** | 파일 변경은 자동 적용하지만 terminal command 실행 전에는 확인 |
| **Plan** | 파일을 건드리기 전에 전체 접근 방식을 먼저 설명 |

Ask는 통제력이 가장 높고, Code는 구현 속도가 빠르며, Plan은 큰 작업 전에 방향을 검토하기 좋습니다.

## 세 모드 비교

| 구분 | Chat | Cowork | Code |
|---|---|---|---|
| 최적화된 작업 | 빠른 대화, 아이디어 탐색, 초안 작성, 학습 | 복잡하고 지속적인 research, analysis, 파일 정리, 완성 문서 제작 | 소프트웨어 개발, 테스트, 실행, 배포 |
| 주요 기능 | Quick entry, dictation | 로컬 folder, plugins, subagents, scheduled tasks | Ask/Code/Plan, visual diffs, git integration, local/remote environment |
| 확장 | Connectors, Skills, Claude in Chrome | Connectors, Skills, Claude in Chrome, Plugins, Computer Use | Connectors, Skills, Claude in Chrome, Plugins, Hooks |

## 어떤 모드를 선택할까?

간단한 기준은 다음과 같습니다.

- 빠르게 묻고 답하며 생각을 정리한다 → **Chat**
- 여러 자료를 읽고 완성된 산출물을 만든다 → **Cowork**
- 코드베이스에서 개발 작업을 한다 → **Code**

업무의 복잡도와 필요한 권한도 함께 고려해야 합니다.

- 화면 하나를 보고 질문하는 수준이면 Chat
- 여러 tool과 파일을 연결해 긴 작업을 맡기려면 Cowork
- terminal, git, code diff가 필요하면 Code

## 실습 질문

다음 질문을 스스로 점검해보면 좋습니다.

- 내가 Claude를 가장 자주 쓰는 작업은 Chat, Cowork, Code 중 어디에 가까운가?
- 최근 여러 source에서 정보를 모아야 했던 프로젝트가 있었나?
- 그 작업을 Cowork에 맡겼다면 workflow가 어떻게 달라졌을까?
- 개발 작업에서 Ask, Code, Plan 중 어떤 mode가 가장 편할까?

## 핵심 정리

Claude desktop app은 작업의 성격에 맞춰 세 가지 모드를 제공합니다.

- **Chat**은 빠르고 대화적인 협업에 적합합니다.
- **Cowork**는 여러 source를 활용해 복잡한 지식 작업을 끝까지 수행하는 데 적합합니다.
- **Code**는 코드베이스 안에서 개발 작업을 수행하는 데 적합합니다.

좋은 선택 기준은 "Claude가 얼마나 오래, 얼마나 많은 도구와 context를 써야 하는가?"입니다. 짧은 대화는 Chat, 긴 지식 작업은 Cowork, 소프트웨어 개발은 Code로 시작하면 됩니다.

다음 모듈에서는 Projects를 활용해 작업과 지식을 구조화하는 방법을 다룹니다.
