---
title: "Skills"
title_ko: "Skills"
course: claude-code-101
lesson: 10
---

> 이 레슨은 주로 영상으로 제공되며, 아티클 본문은 전용 강의로 안내하는 짧은 내용입니다. 아래는 이 과정 다른 레슨에서 다룬 skill 개념을 바탕으로 정리한 요약입니다.

## 학습 목표

- Skill이 무엇이고 MCP 서버와 어떻게 다른지 이해하기
- Skill이 context를 아끼는 방식(온디맨드 로딩) 파악하기

## Skill이란

**Skill**은 Claude에게 특정 workflow를 가르치는 지침 패키지입니다. 이름과 설명만 context에 로드되고, Claude는 실제로 그 skill이 필요하다고 판단할 때에만 전체 내용을 로드합니다.

이 온디맨드 로딩 방식 덕분에, 모든 도구 정의를 미리 context에 올리는 MCP 서버와 달리 Skill은 context 공간을 훨씬 아낍니다. context 관리 관점에서 MCP 서버의 가벼운 대안으로 활용할 수 있습니다.

## 핵심 정리

- Skill은 이름·설명만 상시 로드되고 필요 시 전체가 로드되는, context 효율적인 지침 패키지입니다.
- 더 깊이 배우고 싶다면 전용 강의 *Introduction to agent skills*를 참고하세요.
