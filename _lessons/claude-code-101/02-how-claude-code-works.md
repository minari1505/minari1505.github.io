---
title: "How Claude Code works"
title_ko: "Claude Code의 동작 원리"
course: claude-code-101
lesson: 2
---

## 학습 목표

- agentic loop의 단계를 이해하기
- context window, tools, permissions가 Claude Code에서 어떤 역할을 하는지 파악하기
- Claude Code가 일반 chat 앱과 근본적으로 다른 이유 설명하기

## agentic loop

Claude Code는 일반적인 chat 애플리케이션과 다릅니다. 내부 동작을 이해하면 더 효과적으로 쓸 수 있고, 그 핵심은 **agentic loop**로 설명됩니다.

![agentic loop 다이어그램: 사용자의 prompt가 맥락 수집(Gather context) → 행동(Take action) → 결과 검증(Verify results)의 loop로 흐르며, 언제든 개입·유도·맥락 추가가 가능하다](/assets/images/courses/claude-code-101/agentic-loop.jpg)

1. 사용자가 Claude Code에 prompt를 입력합니다.
2. Claude가 모델과 상호작용하며 필요한 맥락을 모읍니다. 모델은 텍스트 또는 Claude Code가 실행할 수 있는 tool call을 반환합니다.
3. **행동**을 취합니다 — 예: 파일 수정, 명령 실행.
4. 결과를 **검증**하고, 그것이 prompt의 목표를 달성했는지 판단합니다.
5. 달성했다면 종료하고 다음 prompt를 기다립니다. 아니라면 결과가 완성되고 검증 가능할 때까지 loop를 반복합니다.

이 loop 내내 사용자는 맥락을 추가하거나, 중단하거나, 방향을 조정해 목표로 이끌 수 있습니다.

## Context

Claude에는 **context window**가 있어, 대화·파일 내용·명령 출력 등을 얼마나 저장하고 참조할 수 있는지가 결정됩니다. 한계에 도달하면 Claude Code는 대화를 **compact**해서, 제거하거나 요약할 수 있는 부분을 자동으로 정해 context를 다시 사용 가능한 크기로 줄입니다.

## Tools

**Tools**는 agent가 동작하는 근간입니다. 대부분의 AI assistant는 텍스트를 받아 텍스트를 내보낼 뿐이지만, tool은 Claude Code가 작업을 진전시키기 위해 **언제 코드를 실행할지**를 결정하게 해줍니다. 파일 읽기 도구, 웹 검색 도구 등 무엇이든 될 수 있으며, Claude Code는 의미 이해를 바탕으로 언제 tool을 호출하고 그 출력을 어떻게 쓸지 판단합니다.

## Permissions

Claude Code에는 여러 권한 모드가 있습니다.

- **기본 동작**: 파일을 수정하거나 shell 명령을 실행하기 전에 명시적으로 허가를 요청합니다.
- **Auto-accept**: 파일은 묻지 않고 수정하지만, 명령은 여전히 승인이 필요합니다.
- **Plan mode**: 읽기 전용 도구만 사용해 작업을 시작하기 전에 행동 계획을 세웁니다.

이 모든 것은 설정 파일에서 구성할 수 있습니다. 권한을 건너뛸 때는 주의해야 합니다 — Claude Code에 명령 실행의 전권을 주면 실수가 발생하기 전에 잡아내기 어려워집니다.

## 핵심 정리

- Claude Code는 agentic loop, 관리되는 context window, tools, 구성 가능한 permissions를 터미널 안에서 결합합니다.
- 코드베이스를 읽고, 행동하고, 스스로 결과를 검증한다는 점이 chat 창과 근본적으로 다른 부분입니다.
