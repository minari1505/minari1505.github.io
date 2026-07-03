---
title: "The CLAUDE.md file"
title_ko: "CLAUDE.md 파일"
course: claude-code-101
lesson: 8
---

## 학습 목표

- CLAUDE.md가 해결하는 문제와 동작 방식 이해하기
- 프로젝트 수준과 사용자 수준 memory 파일의 계층 파악하기
- CLAUDE.md를 간결하고 유용하게 유지하는 팁 익히기

## 어떤 문제를 해결하나

CLAUDE.md 파일 없이 Claude Code를 열면 매번 처음부터 시작합니다. 코드베이스를 다시 탐색하고, 필요한 의존성을 파악하고, 어떤 기능이 이미 구현됐는지 이해해야 합니다. 때로는 가정을 세우는데, 그러면 방향을 잡기 더 어려워집니다.

CLAUDE.md가 이를 해결합니다. 프로젝트 루트에 두는 Markdown 파일로, Claude Code가 세션 시작 때마다 자동으로 읽습니다. **코드베이스의 온보딩 스크립트**라고 생각하면 됩니다. CLAUDE.md의 내용은 prompt에 덧붙여집니다.

## 예시

전형적인 CLAUDE.md는 다음과 같습니다.

```text
# Project
This is a Next.js 15 app using the App Router, Tailwind, and Drizzle ORM.

# Commands
- Dev server: `pnpm dev`
- Run tests: `pnpm test`
- Lint: `pnpm lint`

# Code Style
- Use 2-space indentation
- Prefer named exports
- All API routes go in app/api/
- Use server actions instead of API routes where possible
```

간단합니다. 이제 React 컴포넌트를 만들라고 하면, Claude Code는 Tailwind로 스타일링하고 코드 관례를 따라야 한다는 것을 이미 압니다.

## CLAUDE.md는 팀을 위한 것

CLAUDE.md는 버전 관리에 커밋해 팀이 함께 혜택을 받게 할 수 있고, 그래야 합니다. 대상에 따라 memory 파일에는 계층이 있습니다.

- **프로젝트 수준 CLAUDE.md** — 프로젝트 루트에 위치. 팀과 공유.
- **사용자 수준 CLAUDE.md** — 설정 폴더에 위치. 나만을 위한 것으로, 내 모든 프로젝트에 적용. 개인 선호를 여기 넣습니다.

## 팁

- **수정 사항을 memory에 저장.** "항상 API route 대신 server action을 써라"처럼 같은 지적을 반복한다면, 그 규칙을 memory에 저장하라고 명시적으로 요청하세요. 다음에 프로젝트를 열면 기억합니다.
- **프로젝트 문서 참조.** Claude가 참조하길 원하는 문서가 있으면 `@` 기호와 파일 경로를 씁니다.

  ```text
  ## README.md
  Please read if you need more info: @README.md
  ```

- **없이 시작하기.** 처음엔 CLAUDE.md 없이 프로젝트를 시작해, 어디서 반복적으로 방향을 바로잡아야 하는지 관찰하길 권합니다. 이렇게 하면 CLAUDE.md가 꼭 필요한 정보에만 집중되어 간결해집니다. 준비되면 `/init`으로 Claude가 하나 생성하게 하세요.

## 핵심 정리

- 답답한 Claude Code 세션과 생산적인 세션의 차이는 대개 맥락에서 갈리며, CLAUDE.md가 그 맥락을 제공하는 방법입니다.
- 스택, 선호, 명령으로 시작해 진행하며 필요에 따라 키워가세요.
