---
title: "Code review"
title_ko: "코드 리뷰"
course: claude-code-101
lesson: 7
---

## 학습 목표

- 서브에이전트로 편향 없는 코드 리뷰를 하는 방법 이해하기
- `/commit-push-pr` skill로 커밋~PR 흐름을 한 번에 처리하기
- `--from-pr`로 PR 작업을 이어서 재개하는 방법 익히기

## 서브에이전트로 리뷰하기

PR을 push하기 전에 Claude에게 서브에이전트로 변경 사항을 검토하게 하세요. 서브에이전트는 자체 context window에서 새로운 시선으로 동작하며, 방금 세션 내내 코드를 작성한 main agent의 편향을 갖지 않습니다.

code-reviewer 서브에이전트를 만들 때는 **읽기 전용 도구로 제한**하세요. 리뷰어는 문제를 지적해야지 파일을 수정하면 안 됩니다. 서브에이전트 설정을 저장소에 체크인하면 팀 전체가 같은 리뷰어를 씁니다.

## `/commit-push-pr` Skill

`/commit-push-pr` skill은 커밋, push, PR 생성을 한 단계로 처리합니다. 각각을 수동으로 하는 대신 skill을 실행하면 Claude가 알아서 해줍니다.

CLAUDE.md에 채널이 명시된 Slack MCP 서버가 구성돼 있다면, PR 링크가 팀 채널에 자동으로 게시됩니다.

## `--from-pr`로 세션 연결

Claude가 `gh pr create`로 PR을 만들면 세션이 그 PR에 자동으로 연결됩니다. 나중에 리뷰 코멘트를 처리하거나 실패한 빌드를 고치러 돌아와야 하면 다음을 실행합니다.

```text
claude --from-pr <PR_NUMBER>
```

떠났던 지점에서 그대로 이어집니다.

## 핵심 정리

- push 전에 서브에이전트로 편향 없는 코드 리뷰를 하세요.
- `/commit-push-pr`로 커밋~PR 전체 흐름을 한 번에 처리합니다.
- `--from-pr`로 나중에 PR 작업을 재개할 수 있습니다.
- 작지만 일상 workflow의 마찰을 크게 줄여주는 기능들입니다.
