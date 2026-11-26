---
title: "Plugins"
title_ko: "Plugin — 설정을 팀에 배포하기"
course: claude-code-in-action
lesson: 13
---

## 학습 목표

- plugin이 무엇을 묶어 배포하는지, marketplace가 어떤 역할을 하는지 설명할 수 있다.
- plugin을 설치하기 전에 무엇을 확인해야 하는지 말할 수 있다.
- 내 `.claude` 디렉터리를 plugin으로 패키징하는 구조와 manifest를 설명할 수 있다.

## 한눈에 보는 두 가지 규칙

<div class="oa-cards">
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">🔎</span><span class="oa-card__title">쓸 때: 설치 전에 읽는다</span>plugin은 내 권한으로 코드를 실행합니다. hook, agent, MCP 서버를 먼저 봅니다.</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📦</span><span class="oa-card__title">만들 때: 동작하면 바로 패키징한다</span>manifest 하나, 설치 한 번.</div>
</div>

## plugin이란

plugin은 **설치 가능한 하나의 단위**입니다. 손으로 공유하던 것들을 한 버전으로 묶습니다.

| 묶는 것 | 예 |
|---|---|
| 주요 구성 요소 | skill, subagent, hook, MCP 서버 설정 |
| 그 밖 | LSP 서버, 백그라운드 모니터, 테마, `settings.json`의 일부 |

<div class="oa-eli">
  <span class="oa-eli__head">📱 쉽게 말하면</span>
  <p>plugin은 <b>앱</b>, marketplace는 <b>앱스토어</b>입니다. 파일을 하나씩 복사해 주고받는 대신 스토어에서 한 번에 설치하고 업데이트를 받습니다. 그리고 앱을 설치할 때처럼, 이 앱이 <b>무슨 권한으로 무엇을 하는지</b> 먼저 확인합니다.</p>
</div>

세션 안에서 이름으로 설치합니다.

```text
/plugin install github@claude-plugins-official
```

명령을 실행하면 먼저 plugin의 세부 정보가 열려서 검토하고 scope를 고를 수 있습니다. 설치 후 필요하면 Claude Code가 `/reload-plugins`를 실행합니다.

## 팀용 marketplace 추가

팀이라면 비공개 marketplace를 한 번 추가하는 편이 낫습니다.

```text
/plugin marketplace add your-org/claude-plugins
```

추가한 뒤의 설치는 모두 이곳을 통해 해석됩니다. 탐색, 버전 추적, 업데이트가 각자의 노트북에 흩어지지 않고 한곳에 모입니다.

## 설치 전에 읽는다

가장 중요한 부분입니다. **plugin은 내 컴퓨터에서 내 권한으로 코드를 실행합니다.** hook은 일치하는 모든 도구 호출에서 실행됩니다. skill 때문에 plugin을 설치해도, 읽었든 안 읽었든 그 plugin의 PreToolUse hook과 Stop hook이 함께 들어옵니다.

예를 들어 커뮤니티 plugin이 매번 외부 네트워크로 호출하는 Stop hook을 담고 있어도 내 설정에서는 아무 경고가 뜨지 않습니다. plugin을 피할 이유가 아니라 **먼저 볼 이유**입니다.

| 알아 둘 점 | 내용 |
|---|---|
| 설치 화면 | 무엇이 설치되는지와 context 비용 추정, 그리고 서드파티 plugin 내용은 Anthropic이 통제하지 않는다는 경고를 보여 줍니다 |
| 커뮤니티 marketplace | 작성자가 Anthropic에 제출해서 올라갑니다 |
| 공식 marketplace | 별도 경로로 선별됩니다 |
| 원칙 | 목록에 있다는 것이 신뢰할 수 있다는 뜻은 아닙니다. 정말 신뢰하는 출처에서만 설치합니다 |

## 구성 요소는 내 것과 나란히 실행된다

plugin은 내 설정을 덮어쓰지 않고 **옆에서 함께** 실행됩니다.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">주의할 점</span>
    <ul>
      <li><b>hook은 쌓입니다.</b> plugin의 PreToolUse hook과 내 PreToolUse hook이 둘 다 매 호출마다 실행됩니다.</li>
      <li>plugin의 <code>settings.json</code>에서 <code>agent</code> 키를 설정하면 plugin의 subagent가 <b>메인 스레드로 올라옵니다</b>. system prompt, 도구 제한, 모델까지 함께 바뀌어 Claude Code의 기본 동작이 달라질 수 있습니다.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">안심할 점</span>
    <ul>
      <li>skill, agent, command는 plugin 이름으로 <b>네임스페이스</b>가 붙어 내 것과 충돌하지 않습니다.</li>
      <li>plugin의 <code>settings.json</code>은 좁은 범위만 반영됩니다. agent 키와 subagent status line 키 두 가지입니다.</li>
    </ul>
  </div>
</div>

설치한 plugin이 무엇을 추가했는지는 plugin 패널에서 확인하고 관리·제거할 수 있습니다.

## 내 plugin 패키징하기

동작하는 `.claude` 디렉터리가 있으면 구조를 바꿀 필요가 없습니다. plugin은 이미 쓰던 형태를 그대로 씁니다.

```text
my-plugin/
├── .claude-plugin/
│   └── plugin.json     # manifest (선택)
├── skills/
│   └── review/SKILL.md # skill마다 폴더 하나
├── agents/
│   └── reviewer.md     # subagent마다 마크다운 파일 하나
├── hooks/
│   └── hooks.json
└── .mcp.json
```

Claude Code는 **디렉터리 관례**로 구성 요소를 찾습니다.

### manifest

```json
{
  "name": "svg-splitter-review",
  "version": "0.1.0",
  "description": "Reviews the SVG Splitter repo",
  "author": { "name": "Lewis Menelaws" }
}
```

| 항목 | 설명 |
|---|---|
| manifest 자체 | 선택입니다. 없어도 디렉터리 관례로 구성 요소를 찾습니다 |
| `name` | 유일한 필수 필드. skill이 `/plugin-name:skill-name`으로 네임스페이스됩니다 |
| `version` | 다른 의존성처럼 버전을 매깁니다. 팀 전체의 업데이트와 버전 추적이 이것으로 작동합니다 |

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. skill 하나만 쓰려고 plugin을 설치했습니다. 그 plugin의 hook은 어떻게 됩니까?</summary>
  <p><b>정답: 함께 설치되어 일치하는 모든 도구 호출에서 실행됩니다.</b> 그래서 설치 전에 hook, agent, MCP 서버를 확인합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. plugin에도 PreToolUse hook이 있고 내 설정에도 있습니다. 어느 쪽이 실행됩니까?</summary>
  <p><b>정답: 둘 다.</b> hook은 쌓입니다. 어느 한쪽이 다른 쪽을 대체하지 않습니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. manifest에서 유일한 필수 필드는 무엇이고, 어떤 효과가 있습니까?</summary>
  <p><b>정답: <code>name</code>.</b> skill을 <code>/plugin-name:skill-name</code>으로 네임스페이스해서 다른 skill과 충돌하지 않게 합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 팀원들이 <code>.claude</code> 폴더를 서로 복사해 쓰다가 버전이 어긋났습니다. 어떻게 해결합니까?</summary>
  <p><b>정답: plugin으로 패키징하고 팀 marketplace를 통해 배포합니다.</b> 버전 추적과 업데이트가 한곳에 모입니다.</p>
</details>

## 핵심 정리

- plugin은 skill, subagent, hook, MCP 설정을 한 단위로 묶어 배포합니다.
- plugin은 내 권한으로 코드를 실행하므로 설치 전에 내용을 읽습니다. 목록에 있다고 신뢰할 수 있는 것은 아닙니다.
- plugin의 hook은 내 hook과 함께 실행되고, skill·agent·command는 네임스페이스로 분리됩니다.
- 패키징은 기존 `.claude` 구조 그대로이며, manifest의 필수 필드는 `name`뿐입니다.
