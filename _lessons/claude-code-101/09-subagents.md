---
title: "Subagents"
title_ko: "서브에이전트"
course: claude-code-101
lesson: 9
---

## 학습 목표

- 서브에이전트가 context 관리에 어떻게 도움을 주는지 이해하기
- `/agents`로 나만의 서브에이전트를 만드는 흐름 파악하기
- 지속 메모리, skill preload 같은 추가 커스터마이징 옵션 알기

## 어떻게 동작하나

Claude Code에서 context 관리는 중요합니다. context window의 상당 부분이 코드베이스를 탐색하는 tool call이나 조사용 웹 검색에 소비되는데, 그 탐색에서 발견한 내용이 정작 개발 중인 주 기능과 늘 관련 있는 것은 아닙니다.

여기서 **서브에이전트**가 등장합니다. Claude는 "이 코드베이스를 탐색해줘" 같은 작업을 처리할 서브에이전트를 띄웁니다. 서브에이전트는 자체 context window에서 병렬로 돌며 탐색 작업을 전부 수행하고, 끝나면 발견을 요약해 Claude에게 돌려줍니다.

결과적으로, 답에 이르기까지의 여정 전체로 주 context를 어지럽히지 않고 원하던 답만 얻습니다.

## 나만의 서브에이전트 만들기

서브에이전트는 YAML frontmatter가 있는 Markdown 파일로 정의됩니다. 가장 쉬운 시작은 Claude가 하나 생성하게 하는 것입니다.

```text
/agents
```

그런 뒤 "Create new agent"를 선택합니다. 에이전트의 범위 선택, 목적 정의, 접근 도구 선택, 색상 지정 같은 단계를 거칩니다. Claude가 서브에이전트의 이름·설명·prompt를 생성하며, 이는 어떤 prompt에서 서브에이전트를 호출할지도 Claude에게 알려줍니다.

## 추가 커스터마이징

- **지속 메모리(Persistent memory)** — 서브에이전트가 대화를 넘어 메모리를 유지하게 합니다. 같은 프로젝트에 꾸준히 쓸 때 유용합니다.
- **Skill preload** — `skill` 키를 추가하고 skill 이름을 나열해 서브에이전트에 skill을 미리 로드합니다. 주 대화의 skill과 달리, 여기서는 skill 전체가 context에 로드된다는 점에 유의하세요.

## 핵심 정리

- context window를 깨끗하게 유지하는 것이 Claude Code에서 생산성을 지키는 최고의 방법 중 하나입니다.
- 서브에이전트로 무거운 작업을 백그라운드 에이전트에 맡기고, 주 context window에는 답만 돌려받으세요.
- 더 깊이 알고 싶다면 전용 강의 *Introduction to subagents*를 참고하세요.
