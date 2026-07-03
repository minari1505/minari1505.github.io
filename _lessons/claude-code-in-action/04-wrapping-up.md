---
title: "Wrapping up"
title_ko: "마무리 & 정리"
course: claude-code-in-action
lesson: 4
---

## 학습 목표

- Claude Code를 단순한 코드 생성기가 아니라 **tool use 기반 개발 agent**로 이해하기
- 이번 강의에서 다룬 실전 사용 패턴을 한 번에 정리하기
- 실제 프로젝트에서 Claude Code를 더 안전하고 일관되게 쓰기 위한 기준 세우기

## Claude Code를 어떻게 봐야 할까?

Claude Code의 핵심은 "모델이 코드를 써준다"가 아니라, 모델이 **맥락을 읽고, 계획을 세우고, 도구를 사용해 행동한 뒤, 결과를 다시 관찰하는 반복 루프**를 실행한다는 점입니다.

언어 모델만으로는 파일을 읽거나 명령을 실행하거나 브라우저를 조작할 수 없습니다. Claude Code는 `Read`, `Edit`, `Bash`, MCP tool 같은 외부 도구를 연결해 이 한계를 넘습니다.

```text
사용자 요청
  ↓
Claude가 필요한 맥락 수집
  ↓
계획 또는 다음 행동 결정
  ↓
도구 실행
  ↓
실행 결과 관찰
  ↓
수정, 검증, 최종 응답
```

따라서 Claude Code를 잘 쓰려면 "좋은 프롬프트"만 생각하면 부족합니다. 어떤 맥락을 주고, 어떤 도구를 허용하며, 어떤 검증 루프를 붙일지까지 함께 설계해야 합니다.

## 이번 강의에서 정리한 것

### 1. Claude Code의 동작 원리

코딩 assistant는 개발자처럼 문제를 해결합니다.

1. 관련 파일과 에러를 읽어 **맥락을 수집**
2. 수정 방향과 검증 방법을 정해 **계획 수립**
3. 파일 편집, 명령 실행, 테스트로 **행동과 검증**

Claude의 강점은 여러 도구를 조합해 이 루프를 안정적으로 이어갈 수 있다는 점입니다.

### 2. 실습 환경에서 Claude Code 쓰기

실제 프로젝트에서는 다음 습관이 중요합니다.

- `CLAUDE.md`에 프로젝트 규칙, 실행 방법, 테스트 명령을 남기기
- `@file`로 필요한 파일을 명시해 맥락을 정확히 전달하기
- 스크린샷으로 UI 변경 지점을 구체적으로 알려주기
- 복잡한 작업은 `/plan`으로 먼저 탐색하게 하기
- 어려운 추론은 `/effort` 또는 `ultrathink`로 더 깊게 생각하게 하기
- 대화가 길어지면 `/compact`, `/clear`, `/rewind`로 맥락을 정리하기

좋은 사용법은 Claude에게 많은 말을 하는 것이 아니라, **필요한 맥락과 검증 기준을 정확히 주는 것**에 가깝습니다.

### 3. 반복 작업을 자동화하기

Claude Code는 기본 명령만 쓰는 도구가 아닙니다. 프로젝트에 맞게 확장할 수 있습니다.

- `.claude/commands/*.md`로 custom command 만들기
- `$ARGUMENTS`로 명령에 인자 전달하기
- Playwright MCP server로 브라우저 조작 능력 추가하기
- GitHub integration으로 issue, PR, code review 흐름에 Claude 연결하기

반복되는 작업은 대화로 매번 설명하기보다 command나 MCP server로 고정해두는 편이 더 안정적입니다.

### 4. Hook과 SDK로 안전장치 만들기

Claude Code가 강력해질수록 안전장치도 중요합니다.

Hook은 tool call 전후에 내가 정한 command를 실행해 Claude의 행동을 감시하거나 보정합니다.

- `PreToolUse`: 실행 전 검사. 민감 파일 접근 차단에 적합
- `PostToolUse`: 실행 후 검사. type check, test, lint 실행에 적합

예를 들어 `.env` 읽기를 막거나, 파일 수정 후 `tsc --noEmit`을 돌려 type error를 즉시 Claude에게 되돌려줄 수 있습니다.

Agent SDK는 Claude Code의 agent loop를 코드에서 직접 실행하게 해줍니다. 이를 이용하면 별도의 Claude 인스턴스로 변경 사항을 검토하게 하거나, 프로젝트별 자동 리뷰 흐름을 만들 수 있습니다.

## 실전 체크리스트

Claude Code를 프로젝트에 도입할 때는 다음 순서로 시작하면 좋습니다.

1. `CLAUDE.md`에 설치, 실행, 테스트, 코드 스타일을 적는다.
2. 자주 하는 작업을 custom command로 만든다.
3. 브라우저 확인이 필요한 프로젝트라면 Playwright MCP를 붙인다.
4. 반드시 지켜야 하는 규칙은 hook이나 permission으로 강제한다.
5. 파일 수정 후 자동으로 실행할 test, linter, type checker를 정한다.
6. GitHub integration은 작은 repo나 단순 task부터 시험한다.

## 핵심 정리

Claude Code의 생산성은 모델 성능만으로 결정되지 않습니다. 실제로는 다음 세 가지가 함께 맞아야 합니다.

- **맥락**: Claude가 프로젝트를 제대로 이해할 수 있는가?
- **도구**: 필요한 파일, 명령, 브라우저, 외부 시스템에 접근할 수 있는가?
- **검증**: Claude가 한 작업이 맞는지 확인할 수 있는가?

이 세 가지를 설계하면 Claude Code는 단순한 채팅형 코딩 도구를 넘어, 프로젝트 안에서 반복 작업을 처리하고, 변경을 검증하고, 개발 흐름에 자연스럽게 들어오는 assistant가 됩니다.

## 마무리

Claude Code를 잘 쓰는 핵심은 "Claude에게 모든 것을 맡긴다"가 아닙니다. 사람이 프로젝트의 규칙과 검증 기준을 정하고, Claude가 그 안에서 도구를 사용해 빠르게 움직이도록 만드는 것입니다.

작은 작업에서는 파일 읽기와 수정만으로도 충분합니다. 하지만 프로젝트가 커질수록 `CLAUDE.md`, custom command, MCP server, hook, SDK 같은 확장 지점을 활용해야 Claude Code를 더 예측 가능하고 안전하게 운영할 수 있습니다.
