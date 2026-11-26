---
title: "GitHub Actions and Code Review"
title_ko: "GitHub Actions와 Code Review"
course: claude-code-in-action
lesson: 11
---

## 학습 목표

- 관리형 **Code Review**와 직접 구성하는 **GitHub Action**의 차이를 설명하고 상황에 맞게 고를 수 있다.
- Code Review가 하는 일과 하지 않는 일을 구분할 수 있다.
- `claude-code-action`의 주요 입력과 `claude_args`로 조절하는 항목을 말할 수 있다.

## 한눈에 보는 두 경로

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">Code Review — 켜기만 하는 관리형</span>
    <ul>
      <li>Anthropic이 호스팅. 만들거나 운영할 것이 없음</li>
      <li>PR에 인라인 코멘트로 발견 사항을 게시</li>
      <li>리뷰만 함</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">GitHub Action — 직접 구성</span>
    <ul>
      <li>워크플로 파일을 직접 작성</li>
      <li>코멘트로 구현 지시, 예약 리포트 등 리뷰 이상의 일</li>
      <li>PR 코멘트, 예약 작업, 모든 GitHub 이벤트에서 실행</li>
    </ul>
  </div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">🔍 쉽게 말하면</span>
  <p>Code Review는 <b>외부 검수 서비스</b>입니다. 문제를 찾아 표시해 주지만 직접 고치지는 않고, 합격·불합격 도장도 찍지 않습니다. GitHub Action은 <b>우리 팀에 배치한 작업자</b>입니다. 시키는 일을 실제로 하고, 무엇을 시킬지는 우리가 정합니다.</p>
</div>

## 관리형 경로: Code Review

Claude 조직의 Owner가 Claude Code 관리 설정의 Code review 항목에서 켭니다. 이어서 Claude GitHub 앱을 설치하고, 감시할 저장소와 실행 시점을 정합니다.

실행 시점은 셋 중에서 고릅니다.

- PR이 열릴 때 한 번
- PR에 push가 있을 때마다
- 누군가 `@claude review`라고 코멘트할 때만

켜고 나면 리뷰 에이전트들이 **변경된 줄만이 아니라 전체 코드베이스에 비춰** diff를 분석합니다. 발견 사항은 해당 줄에 심각도 태그와 함께 인라인 코멘트로 달리고, check run에 요약 표가 붙습니다. 중복을 제거하고 순위를 매겨 주므로 잔소리 더미가 아니라 볼 가치가 있는 문제 몇 개를 읽게 됩니다.

### 하는 일과 하지 않는 일

| 항목 | 내용 |
|---|---|
| PR 승인·차단 | **하지 않습니다.** 판단은 사람이 합니다 |
| 수정 push | **하지 않습니다.** 발견 사항만 게시합니다 |
| 사용 가능 플랜 | Team, Enterprise |

발견 사항을 반영하는 것은 내 몫입니다. 고친 뒤 push하기 전에 내 터미널에서 `/code-review`를 실행하면 diff를 자체 리뷰하고, `--fix` 플래그는 그 리뷰의 발견 사항을 작업 트리에 적용합니다.

## 직접 구성 경로: GitHub Action

Claude Code 안에서 `/install-github-app`을 실행해 시작합니다(저장소 admin 권한 필요). GitHub 앱 설치와 API 키 시크릿 설정을 안내해 줍니다. 액션은 `anthropics/claude-code-action@v1`입니다.

| 입력 | 역할 |
|---|---|
| `anthropic_api_key` | Claude API를 호출할 때 필요. `claude_code_oauth_token`(구독)이나 workload identity federation으로 대신할 수 있음 |
| `github_token` | 선택. 생략하면 설치한 Claude GitHub 앱으로 인증 |
| `trigger_phrase` | 코멘트에서 반응할 문구. 기본값 `@claude` |
| `use_bedrock` / `use_vertex` / `use_foundry` | Claude API 대신 클라우드 제공자를 거쳐 실행 |
| `prompt` | 자동 실행용 지시. 설정하면 멘션을 기다리지 않고 이벤트에서 바로 실행 |
| `claude_args` | Claude Code에 그대로 넘기는 CLI 인자 문자열 |

### @claude에 반응하는 워크플로

`.github/workflows/claude.yaml`에 두고, job의 permissions에 `id-token: write`를 줘서 액션이 Claude GitHub 앱으로 로그인할 수 있게 합니다.

{% raw %}
```yaml
- uses: anthropics/claude-code-action@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    trigger_phrase: "@claude"
    claude_args: "--max-turns 5 --model claude-sonnet-5"
```
{% endraw %}

누군가 PR에 `@claude`로 지시를 남기면 액션이 받아서 Claude가 커밋을 push하고 한 일을 코멘트로 설명합니다.

### 예약 실행 워크플로

같은 액션에 cron 트리거를 걸면 일일 리포트를 만들 수 있습니다. 예약 실행은 **스스로 코멘트를 달지 않으므로** 결과는 그 실행의 로그에서 읽습니다. `workflow_dispatch` 트리거를 추가하면 Actions 탭에서 수동으로도 실행할 수 있습니다.

### claude_args로 조절하기

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🔁</span><span class="oa-card__title">--max-turns</span>에이전트 loop에 상한을 둬서 끝없이 돌지 않게 합니다.</div>
  <div class="oa-card"><span class="oa-card__icon">🚦</span><span class="oa-card__title">Permission mode</span>무인 작업은 멈춰서 묻지 않게 합니다. 답할 사람이 없기 때문입니다.</div>
  <div class="oa-card"><span class="oa-card__icon">🧰</span><span class="oa-card__title">Allowed tools</span>작업에 필요한 것만 줍니다. 리포트라면 읽기 전용입니다.</div>
</div>

## 무엇을 고를까

| 상황 | 선택 |
|---|---|
| PR 리뷰 | Code Review를 켜고, 인라인 발견 사항을 보고 고쳐서 push |
| 리뷰 이상의 일(구현, 리포트, 커스텀 CI) | GitHub Action. `/install-github-app`으로 설정 |

관리형으로 시작하고, Claude가 CI에서 코멘트만이 아니라 실제 작업을 해야 하는 순간 액션으로 넘어갑니다.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Code Review가 심각한 버그를 찾으면 PR을 자동으로 차단합니까?</summary>
  <p><b>정답: 아닙니다.</b> Code Review는 승인도 차단도 하지 않고, 수정도 push하지 않습니다. 발견 사항만 게시하고 판단은 사람이 합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. PR 코멘트로 "이 스펙을 구현해 줘"라고 시키려면 Code Review와 GitHub Action 중 무엇이 필요합니까?</summary>
  <p><b>정답: GitHub Action.</b> Code Review는 리뷰만 합니다. 구현처럼 리뷰 이상의 일은 액션으로 합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 예약 실행한 액션의 결과는 어디에서 봅니까?</summary>
  <p><b>정답: 그 실행의 로그.</b> 예약 실행은 스스로 코멘트를 달지 않습니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 무인 리포트 작업의 <code>claude_args</code>에서 조절할 세 가지는 무엇입니까?</summary>
  <p><b>정답: <code>--max-turns</code> 상한, 묻지 않는 permission mode, 읽기 전용으로 제한한 allowed tools.</b></p>
</details>

## 핵심 정리

- PR 리뷰는 관리형 Code Review로 시작합니다. 전체 코드베이스에 비춰 분석하고 인라인 코멘트를 답니다.
- Code Review는 승인·차단·수정을 하지 않습니다.
- 리뷰 이상의 일은 `anthropics/claude-code-action@v1`로 구성하고, 조절은 `claude_args`에서 합니다.
