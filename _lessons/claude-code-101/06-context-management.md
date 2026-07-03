---
title: "Context management"
title_ko: "context 관리"
course: claude-code-101
lesson: 6
---

## 학습 목표

- context window가 무엇이고 무엇이 그 공간을 차지하는지 이해하기
- compaction과 `/compact`, `/clear`, `/context` 명령의 차이 파악하기
- context 공간을 아끼는 실전 습관 익히기

## context window란?

Context는 Claude의 작업 기억입니다. 읽은 파일, 실행한 명령, 보낸 메시지 하나하나가 모두 context window 공간을 차지합니다.

![context window 다이어그램: 토큰 격자에서 일부는 사용되고 대부분은 사용 가능한 상태](/assets/images/courses/claude-code-101/context-window.jpg)

prompt를 입력하거나 파일을 읽거나 tool call과 그 결과를 받을 때마다 context window에 더해집니다. 공간은 유한하므로 어떻게 쓰는지 최적화하는 것이 중요해집니다.

## 가득 차면 일어나는 일

한계에 가까워지면 context window가 자동으로 **compact**됩니다. Compaction은 중요한 세부를 요약하고 불필요한 tool call 결과를 제거해 공간을 확보합니다. 다만 이 과정에서 세부 정보를 잃을 수 있습니다.

## 명령어

- `/compact` — 그 시점까지 전부를 수동으로 compact합니다. 이전 작업 기억은 유지하면서 context 공간을 비우고 싶을 때 유용합니다.
- `/clear` — 이전 세션 기억 없이 완전히 처음부터 시작합니다. 모든 것을 제거합니다.
- `/context` — 현재 context 상태를 확인합니다. context 크기, 가장 많은 공간을 차지하는 범주, 구성 비율을 보여주는 시각 그래픽을 볼 수 있습니다.

## 언제 무엇을 쓸까

- **`/compact`**: 특정 기능을 작업하다 context 한계에 부딪혔지만 계속 이어가야 할 때. 현재 기능과 관련된 맥락을 유지하는 게 중요합니다.
- **`/clear`**: 새 기능을 시작할 때. 이전 대화가 새 작업에 편향을 주지 않게 합니다. 세션을 넘어 Claude가 기억해야 할 것은 CLAUDE.md에 넣어, 매번 처음부터 재발견하지 않게 합니다.

## context 공간을 아끼는 팁

- **구체적으로 쓰기.** 모호한 prompt는 짧아 보여도 장기적으로 더 많은 context를 씁니다. 명확한 지시가 없으면 Claude가 코드베이스를 더 탐색하고 스스로 추론해야 하는데, 이것이 상세한 prompt보다 훨씬 많은 공간을 잡아먹습니다.
- **MCP 서버 관리.** MCP 서버는 사용하지 않아도 기본적으로 모든 도구를 context에 올립니다. 현재 프로젝트와 무관한 서버는 꺼두세요. 앞으로 올리지 않는 "Skills"를 대안으로 쓸 수도 있습니다.
- **서브에이전트 사용.** 서브에이전트는 main agent와 병렬로 돌지만 완전히 별도의 context window를 가집니다. "인증 엔드포인트가 어디 있지?"처럼 답만 필요한 작업은 서브에이전트가 처리하고 요약만 돌려줘, 주 context를 깨끗하게 유지합니다.

## 핵심 정리

- Claude Code에서 context 관리는 매우 중요합니다. 긴 세션은 `/compact`로 요약하고, 새 시작은 `/clear`로 합니다.
- context를 효과적으로 쓰려면 prompt를 구체적으로 쓰고, `/context`로 무엇이 소비되는지 확인하고, 답만 필요한 작업은 서브에이전트에 위임하세요.
