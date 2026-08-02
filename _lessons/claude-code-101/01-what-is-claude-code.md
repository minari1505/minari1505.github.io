---
title: "What is Claude Code?"
title_ko: "Claude Code란?"
course: claude-code-101
lesson: 1
---

## 학습 목표

- Claude Code가 Claude.ai와 어떻게 다른지 이해하기
- AI Agent의 개념과 agentic loop의 기본 아이디어 파악하기
- Claude Code가 실제로 할 수 있는 일과 효과적으로 쓰기 위한 전제 이해하기

## Claude Code란?

Claude Code는 코드베이스를 이해하고, 파일을 수정하고, 명령을 실행하고, 기존 개발 도구와 연동해 작업을 더 빠르게 끝내주는 **agentic coding tool**입니다. 터미널, Visual Studio Code, Claude Desktop app, 웹, JetBrains IDE에서 사용할 수 있습니다.

## Claude.ai와 무엇이 다른가?

Claude.ai를 써봤다면 Claude Code와의 차이가 궁금할 수 있습니다. 가장 큰 차이는 Claude Code가 **파일, 터미널, 코드베이스 전체에 직접 접근**한다는 점입니다. 코드를 복사·붙여넣기하며 오가는 대신, Claude Code가 직접 들어가서 작업을 수행합니다.

핵심 차별점은 Claude Code가 **AI Agent로 동작**한다는 것입니다.

### Agent란?

AI Agent는 자신의 환경과 상호작용하며 정해진 목표를 달성하기 위해 행동을 수행하는 소프트웨어입니다. 근본적으로는 **large language model이 실시간 loop 안에서 동작**하는 방식이며, agent는 목표 달성을 위해 도구, 외부 서비스, 심지어 다른 AI Agent에도 접근할 수 있습니다.

## Claude Code가 할 수 있는 일

실제로는 다음과 같이 동작합니다.

- **코드베이스를 읽고 이해하기** — 특정 기능을 설명해달라거나 버그를 코드 전반에서 추적해달라고 요청할 수 있습니다.
- **프로젝트 전반의 파일 수정** — 함수를 리팩터링하고 그것을 참조하는 모든 파일을 함께 업데이트할 수 있습니다.
- **터미널 명령 실행** — 빌드 스크립트 실행, 테스트 수행, 패키지 설치를 하고 그 출력을 바탕으로 다음 행동을 결정합니다.
- **웹 검색** — 문서나 최신 API 레퍼런스가 필요하면 직접 찾아봅니다.

## 효과적으로 쓰기 위한 세 가지

Claude Code를 잘 쓰려면 다음 세 개념을 기억하면 좋습니다.

- **context window** — Claude의 작업 기억입니다. 많은 것을 담을 수 있지만 한 번에 전부는 아닙니다. 여기서 "agentic"의 의미가 드러나는데, Claude는 코드베이스 전체를 context에 올리지 않고도 답을 찾는 전략적 방법을 스스로 찾습니다.
- **권한을 물어봄** — 기본적으로 Claude Code는 명령을 실행하거나 변경을 가하기 전에 사용자에게 확인합니다. hands-on이든 hands-off든 항상 사용자가 통제권을 갖습니다.
- **실수할 수 있음** — 다른 도구와 마찬가지로 완벽하지 않습니다. 의도를 오해하거나, 버그를 넣거나, 과하게 설계할 수 있습니다. 과정에 계속 참여하면 이런 문제를 일찍 잡아낼 수 있습니다.

## 핵심 정리

- Claude Code는 코드베이스를 읽고, 파일을 수정하고, 명령을 실행하고, 외부 도구와 연결해 더 빠르게 출시하도록 돕는 agentic coding tool입니다.
- Claude.ai와 달리 파일·터미널·코드베이스에 직접 접근하며, AI Agent로서 loop 안에서 도구를 활용합니다.
- 터미널, VS Code, JetBrains, Claude Desktop app에서 바로 사용할 수 있습니다.
