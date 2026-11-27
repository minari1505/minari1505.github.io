---
title: "Installing Claude Code"
title_ko: "Claude Code 설치하기"
course: claude-code-101
lesson: 3
---

## 학습 목표

- 터미널, IDE, 데스크톱, 웹 등 **환경별 설치 방법** 익히기
- 설치 후 **첫 실행과 로그인 흐름** 이해하기
- 자신의 작업 방식에 맞는 **인터페이스 선택 기준** 파악하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>환경 고르기</strong>터미널 · VS Code · JetBrains · Desktop · Web</li>
  <li><strong>설치</strong>환경마다 한두 단계면 끝나요</li>
  <li><strong>첫 실행</strong>프로젝트 디렉터리에서 <code>claude</code></li>
  <li><strong>초기 설정</strong>테마 선택 → 계정 로그인 또는 API key</li>
</ol>

이 레슨을 한 문장으로 줄이면: **어디서든 쉽게 설치되고, 최신 기능이 가장 먼저 오는 곳은 터미널이에요.**

## 1. 터미널에 설치하기

<div class="oa-eli">
  <span class="oa-eli__head">📦 쉽게 말하면</span>
  <p>앱을 공식 설치 파일로 깔면 알아서 업데이트되지만, 다른 경로로 깔면 내가 직접 챙겨야 하는 경우가 있죠. 여기서도 <b>패키지 매니저로 설치하면 기본적으로 자동 업데이트가 안 돼요.</b></p>
</div>

| OS / 셸 | 설치 방법 | 강의가 짚는 주의점 |
|---|---|---|
| macOS · Linux · WSL | `curl` 명령으로 한 번에 설치 | — |
| Homebrew를 선호할 때 | `brew install` | **기본적으로 자동 업데이트가 안 돼요** |
| Windows PowerShell | `Invoke-RestMethod` 명령 | — |
| Windows CMD | `curl` 명령 | — |
| Windows (`winget`) | `winget` 명령 | Homebrew처럼 **기본적으로 자동 업데이트가 안 돼요** |

> 강의 본문은 명령의 **종류**만 소개하고 전체 명령줄은 영상에서 보여 줘요. 정확한 명령은 공식 설치 문서에서 확인하세요.

설치 후에는 `claude` 명령을 실행할 수 있어야 해요. 안 되면 **터미널을 재시작**하세요. 프로젝트 디렉터리로 이동한 뒤 실행해요.

```text
claude
```

### 첫 실행에서 거치는 초기 설정

<ol class="oa-flow">
  <li><strong>색상 테마 선택</strong>터미널에 맞는 테마를 골라요</li>
  <li><strong>로그인 방식 선택</strong>Claude 계정(Pro, Max, Ultra, Team, Enterprise) 또는 API key</li>
  <li><strong>작업 시작</strong>실행한 디렉터리에서 바로 사용</li>
</ol>

- 조직이 **Claude Enterprise** 계정을 쓴다면 꼭 그 옵션을 선택하세요.
- `claude`를 실행한 **디렉터리와 그 하위 폴더 전체**에 접근 권한을 갖게 돼요. 프로젝트 디렉터리로 이동한 뒤 실행하는 이유예요.

## 2. IDE에 설치하기

<div class="oa-eli">
  <span class="oa-eli__head">🪟 쉽게 말하면</span>
  <p>같은 가게에 <b>문이 여러 개</b> 있는 거예요. 에디터 안에서 문을 열고 들어가도 안에 있는 건 터미널과 거의 같은 Claude Code예요.</p>
</div>

### Visual Studio Code

<ol class="oa-flow">
  <li><strong>검색</strong>Extensions 패널에서 "Claude Code"</li>
  <li><strong>확인 후 설치</strong>파란 인증 체크가 있는 Anthropic 제작 확장</li>
  <li><strong>재시작</strong>필요하면 VS Code를 재시작</li>
  <li><strong>열기</strong>명령 팔레트(Ctrl/Cmd + Shift + P) → "Claude Code Open in New Tab", 또는 사이드바의 Claude 로고</li>
</ol>

VS Code 확장은 터미널과 **매우 비슷한 경험**을 줘요. 원하면 설정에서 UI를 끄고 터미널 경험을 그대로 쓸 수도 있어요.

### JetBrains

<ol class="oa-flow">
  <li><strong>설치</strong>JetBrains Marketplace에서 Claude Code 플러그인</li>
  <li><strong>재시작</strong>IDE를 재시작</li>
  <li><strong>열기</strong>Claude 로고를 클릭하면 에디터와 나란히 동작하는 터미널 경험 창이 열려요</li>
</ol>

## 3. Desktop과 Web

<div class="oa-eli">
  <span class="oa-eli__head">☁️ 쉽게 말하면</span>
  <p>Desktop은 <b>내 컴퓨터의 폴더</b>를 열어 주는 작업실이고, Web은 <b>GitHub 저장소</b>만 들어갈 수 있는 원격 작업실이에요.</p>
</div>

| | Desktop | Web |
|---|---|---|
| **접근 방법** | Claude Desktop 설치·로그인 후 상단의 **"Code" 탭** | `claude.ai/code`로 가거나 chat 앱 사이드바의 **"Code" 라벨** 클릭 |
| **느낌** | chat 쪽과 비슷해요 | 데스크톱 앱과 비슷하게 동작해요 |
| **할 수 있는 것** | 특정 폴더에서 작업, 권한 변경, 클라우드 환경에서 작업 | GitHub 저장소로 원격 작업 |
| **제한** | — | **GitHub 저장소로 제한**돼요 |

## 4. 어떤 것을 써야 할까요?

<div class="oa-eli">
  <span class="oa-eli__head">🚲 쉽게 말하면</span>
  <p>출근길에 자전거를 탈지 지하철을 탈지는 취향과 상황 문제죠. 도착지는 같아요. <b>내 작업 방식에 맞는 걸</b> 고르면 돼요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">⌨️</span><span class="oa-card__title">터미널</span>최신 기능을 가장 먼저 쓰고 싶다면 최선이에요. <b>기능이 여기서 먼저 출시</b>돼요</div>
  <div class="oa-card"><span class="oa-card__icon">🧩</span><span class="oa-card__title">IDE 통합</span>코드 에디터와 더 밀접한 느낌을 원할 때. 거의 동일한 경험을 줘요</div>
  <div class="oa-card"><span class="oa-card__icon">🖥️</span><span class="oa-card__title">Desktop</span>다른 일을 하는 동안 Claude를 <b>백그라운드</b>에서 돌리기에 좋아요</div>
  <div class="oa-card"><span class="oa-card__icon">🌐</span><span class="oa-card__title">Web</span>GitHub 저장소로 <b>원격 작업</b>하고 싶을 때 좋은 선택이에요</div>
</div>

정답은 없어요. 어떻게 쓸지는 각자에게 달려 있어요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Homebrew나 <code>winget</code>으로 설치할 때 알아 둬야 할 점은?</summary>
  <p><b>정답: 기본적으로 자동 업데이트가 되지 않아요.</b> 강의가 두 방법에만 따로 붙인 주의점이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. <code>claude</code>를 프로젝트의 하위 폴더 <code>src/</code>에서 실행하면 접근 범위는 어떻게 되나요?</summary>
  <p><b>정답: <code>src/</code>와 그 하위 폴더 전체.</b> 실행한 디렉터리와 그 아래에 접근 권한을 가져요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 새 기능을 가장 먼저 써 보고 싶다면 어떤 인터페이스가 좋은가요?</summary>
  <p><b>정답: 터미널.</b> 기능이 터미널에서 먼저 출시돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Web 버전 Claude Code의 제한은 무엇인가요?</summary>
  <p><b>정답: GitHub 저장소로 제한돼요.</b> 동작 자체는 데스크톱 앱과 비슷해요.</p>
</details>

## 정리

- Claude Code는 **터미널, VS Code, JetBrains, Desktop, Web** 어디서든 간단히 설치할 수 있어요.
- 터미널 설치는 `curl`(macOS·Linux·WSL·CMD) 또는 `Invoke-RestMethod`(PowerShell)이고, **`brew install`·`winget`은 기본적으로 자동 업데이트가 안 돼요.**
- 설치 후 `claude`를 실행하면 **테마 선택 → 로그인(Pro, Max, Ultra, Team, Enterprise 또는 API key)**을 거치고, 실행한 디렉터리와 하위 폴더에 접근해요.
- VS Code는 인증 체크가 있는 Anthropic 확장, JetBrains는 Marketplace 플러그인을 설치해요.
- **최신 기능은 터미널에서 먼저** 나오고, 나머지는 취향과 작업 방식에 맞게 고르면 돼요.
