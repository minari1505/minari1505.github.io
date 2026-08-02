---
title: "Hooks"
title_ko: "Hooks"
course: claude-code-101
lesson: 12
---

## 학습 목표

- hook이 무엇이고 왜 "결정론적(deterministic)"인지 이해하기
- 주요 hook 이벤트와 구성 방법 파악하기
- PreToolUse로 위험한 작업을 차단하고 팀과 hook을 공유하는 방법 익히기

## 왜 hook을 쓰나

**Hooks**는 Claude Code 생명주기의 특정 지점에서 명령을 실행하게 해줍니다. 이 과정의 다른 모든 것과 결정적으로 다른 점은, hook은 **결정론적**이라 항상 실행된다는 것입니다.

CLAUDE.md에 "파일 편집 후 Prettier를 실행하라"고 적어둘 수 있고 대개는 실행되지만, 가끔은 안 됩니다. hook은 예외 없이 **매번** 실행되게 만듭니다. 대표적 활용:

- 파일 편집 후 자동 포매팅
- 규정 준수를 위한 모든 실행 명령 로깅
- 프로덕션 파일 수정 같은 위험한 작업 차단
- Claude가 작업을 끝냈을 때 알림 보내기

## 동작 방식

Hook은 `settings.json`에 구성합니다. 이벤트를 고르고, 선택적으로 어떤 도구에 적용할지 matcher를 설정하고, 실행할 명령을 지정합니다. 사용 가능한 이벤트:

- **PreToolUse** — tool call 전에 실행
- **PostToolUse** — tool call 완료 후 실행
- **UserPromptSubmit** — prompt 제출 시, Claude가 처리하기 전에 실행
- **Stop** — Claude가 응답을 마칠 때 실행
- **Notification** — Claude가 알림을 보낼 때 실행

`/hooks` 명령이나 `settings.json` 직접 편집으로 구성합니다.

## 실전 예시

가장 흔한 hook은 편집 후 자동 포매팅입니다. matcher를 `"Edit|MultiEdit|Write"`로 한 PostToolUse hook을 설정하면 Claude가 파일을 수정할 때마다 발동합니다. 명령은 파일 확장자를 확인해 적절한 포매터를 실행합니다 — TypeScript엔 Prettier, Go엔 gofmt 등.

## PreToolUse로 차단하기

PreToolUse hook은 tool call이 실행되기 전에 차단할 수 있습니다. hook은 도구 이름과 입력을 JSON으로 stdin에서 받고, exit code로 동작이 결정됩니다.

- **exit code 0** — 정상 진행.
- **exit code 2** — 작업 차단. stderr 메시지가 Claude에게 피드백으로 전달돼, 왜 막혔는지 알고 조정할 수 있습니다.
- **그 외 exit code** — 차단하지 않는 오류. 사용자에게 표시되지만 멈추지는 않습니다.

이렇게 하드 규칙을 강제합니다. 프로덕션 config 디렉터리 쓰기 차단, `rm -rf` 포함 bash 명령 차단, main 커밋 차단 등 팀에 필요한 것을 "권고"가 아닌 "보장"으로 만듭니다.

## 팀과 hook 공유

`.claude/settings.json`에 구성한 hook은 프로젝트 수준이며 저장소에 체크인할 수 있습니다. 팀 전체가 같은 hook을 자동으로 갖게 됩니다. 명령에서 `CLAUDE_PROJECT_DIR` 환경 변수를 써서 프로젝트에 저장된 스크립트를 참조하면, Claude의 현재 작업 디렉터리와 무관하게 동작합니다.

## 핵심 정리

- Hook은 Claude Code 동작에 결정론적 통제를 줍니다.
- PostToolUse는 자동 포매팅·로깅에, PreToolUse는 위험한 작업 차단에 씁니다.
- `/hooks`나 settings.json으로 구성하고, 저장소에 체크인해 팀과 공유하세요.
- 무언가가 실패 없이 매번 일어나야 한다면, prompt가 아니라 hook에 넣으세요.
