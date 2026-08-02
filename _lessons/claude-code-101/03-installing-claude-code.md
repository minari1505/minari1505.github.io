---
title: "Installing Claude Code"
title_ko: "Claude Code 설치하기"
course: claude-code-101
lesson: 3
---

## 학습 목표

- 터미널, IDE, 데스크톱, 웹 등 환경별 Claude Code 설치 방법 익히기
- 설치 후 첫 실행과 로그인 흐름 이해하기
- 자신의 작업 방식에 맞는 인터페이스 선택 기준 파악하기

## 터미널

macOS, Linux, WSL에서는 `curl` 명령으로 한 번에 설치할 수 있습니다. Homebrew를 선호하면 `brew install`도 되지만, 이 방식은 자동 업데이트를 지원하지 않습니다.

Windows에서는 몇 가지 선택지가 있습니다. PowerShell에서는 `Invoke-RestMethod`, CMD에서는 `curl`을 사용합니다. `winget` 명령도 있지만 Homebrew처럼 자동 업데이트는 되지 않습니다.

설치 후에는 `claude` 명령을 실행할 수 있어야 합니다. 안 되면 터미널을 재시작하세요. 프로젝트 디렉터리로 이동한 뒤 실행합니다.

```text
claude
```

색상 테마 선택, 계정 로그인(Pro, Max, Enterprise) 또는 API key 사용 같은 초기 설정을 거칩니다. 조직이 Claude Enterprise 계정을 쓴다면 그 옵션을 선택하세요. `claude`를 실행한 디렉터리와 그 하위 폴더 전체에 접근 권한을 갖게 됩니다.

## Visual Studio Code

Extensions 패널에서 "Claude Code"를 검색하고, 파란 인증 체크가 있는 Anthropic 제작 확장을 찾아 설치합니다. 설치 후 VS Code를 재시작해야 할 수 있습니다. 실행되면 명령 팔레트(Ctrl/Cmd + Shift + P)에서 "Claude Code Open in New Tab"을 검색하거나, 사이드바의 Claude 로고를 클릭합니다.

VS Code 확장은 터미널과 매우 유사한 경험을 제공합니다. 원하면 설정에서 UI를 끄고 터미널 경험을 그대로 쓸 수도 있습니다.

## JetBrains

JetBrains Marketplace에서 Claude Code 플러그인을 설치하고 IDE를 재시작합니다. 다시 열면 Claude 로고가 보이고, 클릭하면 에디터와 나란히 동작하는 터미널 경험 창이 열립니다.

## Desktop

Claude Desktop을 설치하고 로그인하면 상단에 "Code" 토글이 보입니다. chat 쪽과 느낌은 비슷하지만, 특정 폴더에서 작업하고 권한을 바꾸며 클라우드 환경에서도 작업할 수 있습니다.

## Web

웹에서는 `claude.ai/code`로 가거나 chat 앱 사이드바의 "Code" 라벨을 클릭해 접근합니다. 데스크톱 앱과 유사하게 동작하지만 GitHub 저장소로 제한됩니다.

## 어떤 것을 써야 할까?

- 최신 기능을 가장 먼저 쓰고 싶다면 **터미널**이 최선입니다 — 기능이 여기서 먼저 출시됩니다.
- **IDE 통합**은 코드 에디터와 더 밀접한 경험을 원할 때 거의 동일한 경험을 제공합니다.
- **Desktop**은 다른 일을 하는 동안 Claude를 백그라운드에서 돌리기에 좋습니다.
- **Web**은 GitHub 저장소로 원격 작업하고 싶을 때 좋은 선택입니다.

## 핵심 정리

- Claude Code는 터미널, VS Code, JetBrains, 데스크톱, 웹 어디서든 간단히 설치할 수 있습니다.
- 설치 후 `claude`를 실행하면 테마·로그인 등 초기 설정을 거치고, 실행한 디렉터리에 접근합니다.
- 최신 기능은 터미널에서 먼저 나오며, 나머지는 취향과 작업 방식에 맞게 고르면 됩니다.
