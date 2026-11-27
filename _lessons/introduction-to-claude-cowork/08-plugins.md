---
title: "Plugins: Encode your team's expertise"
title_ko: "Plugins: 팀의 전문성 담기"
course: introduction-to-claude-cowork
lesson: 8
---

## 학습 목표

- **plugin**을 정의하고 무엇을 묶는지 이해하기
- plugin의 **두 가지 형태** 알기
- 실제 작업에 plugin을 설치하거나 커스터마이징하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>Directory 확인</strong>이미 있는 plugin부터 찾아요</li>
  <li><strong>Install</strong>커넥터를 승인하면 skill이 바로 사용 가능</li>
  <li><strong>Customize</strong>팀의 템플릿 · 정의 · 단계에 맞게</li>
  <li><strong>없으면 직접 만들기</strong>skill 하나에서 작게 시작</li>
</ol>

이 레슨을 한 문장으로 줄이면: **plugin은 하나의 "일"을 중심으로 묶은 skill 패키지이고, 전문성이 사람이 아니라 설치를 따라 이동해요.**

## 1. plugin이란?

<div class="oa-eli">
  <span class="oa-eli__head">🧰 쉽게 말하면</span>
  <p>skill이 드라이버 하나라면, plugin은 <b>"전기 기사용 공구 가방"</b>이에요. 그 일에 필요한 공구가 한 가방에 들어 있어서, 가방만 받으면 신입도 선배와 같은 장비로 시작해요.</p>
</div>

**Plugin**은 하나의 "일(job)"을 중심으로 묶은 skill 세트예요. 한 사람의 플레이북이던 팀의 워크플로가, 누구나 설치할 수 있는 **공유 도구 세트**가 돼요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📘</span><span class="oa-card__title">Skills</span>skill이 플레이북 하나라면, plugin은 여러 개예요</div>
  <div class="oa-card"><span class="oa-card__icon">🔌</span><span class="oa-card__title">Connectors</span>그 skill들이 의존하는 커넥터</div>
  <div class="oa-card"><span class="oa-card__icon">🤖</span><span class="oa-card__title">Subagents</span>skill이 한 부분을 자체 맥락에서 처리하도록 띄우는 목적형 헬퍼. 예: 조사 단계용 research subagent, 초안 단계용 drafting subagent</div>
</div>

plugin은 **팀의 방식**을 Claude에게 가르쳐요.

| 설치하면 | Claude가 아는 것 |
|---|---|
| **finance plugin** | 팀이 주식을 분석하는 방식 |
| **legal plugin** | 팀의 계약 플레이북 |

**전문성이 사람이 아니라 설치를 따라 이동해요.**

Anthropic은 흔한 역할(finance, legal, sales, marketing, customer support, product management 등)용 plugin을 게시해요. 그대로 설치하거나, 커스터마이징하거나, 직접 만들 수 있어요.

## 2. 두 종류의 plugin

<div class="oa-eli">
  <span class="oa-eli__head">🍱 쉽게 말하면</span>
  <p>하나는 <b>밀키트</b>예요. 손질 → 조리 → 플레이팅까지 순서대로 한 끼를 완성해요. 다른 하나는 <b>양념 세트</b>예요. 서로 순서는 없지만 주방에서 가장 자주 손이 가는 것들을 모아 둔 거예요.</p>
</div>

둘 다 유용하고 둘 다 흔해요.

| | 형태 1: 처음부터 끝까지의 프로세스 | 형태 2: 팀이 가장 많이 쓰는 skill 모음 |
|---|---|---|
| **언제** | 순차 단계가 많은 작업 | 팀이 반복해서 하는 여러 일 |
| **묶는 방식** | 각 단계의 skill을 묶어 전체가 하나로 돌게 해요 | 서로 의존하지 않는, 가장 자주 손이 가는 skill을 묶어요 |
| **예** | monthly-close plugin = 실적 수집 + 분산 표 작성 + board memo 초안 | finance plugin = 분산 분석 + 재무 모델링 + 투자 메모 초안 + 분기 보고서 |
| **효과** | 누구든 설치하면 전체 프로세스를 우리 방식대로 얻어요 | 새 팀원이 하나만 설치하면 팀 도구 세트 전체를 가져요 |

### plugin 안을 들여다보면 (강의의 예시 "Legal" plugin)

강의는 plugin 상세 페이지 mock을 보여 줘요. function's toolkit 형태의 **Legal** plugin과 end-to-end pipeline 형태의 **Experiment Readout** plugin 두 가지예요.

| 항목 | Legal plugin 예시 |
|---|---|
| **Source** | Marketplace (Anthropic & Partners) |
| **Description** | 법무팀이 가장 많이 하는 계약 · 검토 업무 |
| **Skills** | `/nda-review` house playbook에 맞춰 NDA redline · `/contract-summary` 계약의 핵심 조건 · 날짜 · 의무 추출 · `/clause-library` 조항별 사전 승인 fallback 문구 찾기 · `/regulatory-check` 관할별 이슈 표시 · `/counterparty-research` 공개 filing과 과거 딜 이력 수집 |
| **Connectors** | Box, Egnyte, Slack, M365, Atlassian |

> **Stay in the loop.** plugin은 Claude가 워크플로를 실행하게 해 주지만, 출력을 검토하는 건 여전히 사용자 몫이에요.

핵심은 어느 쪽이든 **plugin은 워크플로를 중심으로 묶은 패키지**라는 점이에요. 강의가 든 예는 이래요.

- "customer success 팀의 renewal prep"
- "우리 fund의 equity research"
- "CFO office의 월간 board cycle"

## 3. marketplace에서 설치

<div class="oa-eli">
  <span class="oa-eli__head">📲 쉽게 말하면</span>
  <p>휴대폰 <b>앱 스토어</b>에서 앱을 받는 것과 같아요. 찾고, 설치를 누르고, 권한 요청(커넥터)을 승인하면 바로 쓸 수 있어요.</p>
</div>

Anthropic은 지식 노동에서 가장 흔한 역할용 plugin을 게시하고 유지해요. 그대로 써도 되고 팀에 맞게 다듬어도 되는 **출발점**이에요.

<ol class="oa-flow">
  <li><strong>Customize → Plugins</strong>Cowork에서 열어요</li>
  <li><strong>찾기</strong>내 작업에 맞는 plugin을 둘러봐요</li>
  <li><strong>Install</strong>설치를 눌러요</li>
  <li><strong>커넥터 승인</strong>plugin이 쓰는 커넥터를 승인해요</li>
  <li><strong>바로 사용</strong>plugin의 skill이 즉시 사용 가능해져요</li>
</ol>

## 4. 팀에 맞게 커스터마이징

<div class="oa-eli">
  <span class="oa-eli__head">👔 쉽게 말하면</span>
  <p>기성복은 <b>좋은 출발점이지 최종 답은 아니에요.</b> 소매 길이를 줄이고 허리를 잡으면 내 옷이 되죠. marketplace plugin도 팀의 치수에 맞게 수선해요.</p>
</div>

marketplace plugin은 **강력한 기본값이지 최종 답은 아니에요.** 안의 skill · 커넥터는 워크플로의 일반적인 버전이에요. 팀에는 고유한 템플릿 · 정의 · 단계가 있고, 설치한 어떤 plugin이든 거기에 맞게 다듬을 수 있어요.

설치 후 **Customize → Plugins → [Plugin 이름] → Customize.** 새 Cowork 작업이 열려 Claude와 함께 plugin을 손봐요. 특정 asset을 가리키거나, 맥락을 공유하거나, 기반으로 삼을 예시를 업로드해 starter prompt를 줄 수 있어요. 그러면 Claude가 팀 맥락에 맞게 plugin을 업데이트해요.

```text
지난 red-lined NDA 3개야. 이 plugin의 /nda-triage skill을 이 포맷·톤에 맞게 업데이트해줘.
```

Claude가 plugin을 **제자리에서** 고쳐요. 팀의 실제 작업에 맞게 다듬을수록 더 큰 leverage가 나와요.

## 5. 직접 만들기

<div class="oa-eli">
  <span class="oa-eli__head">🌱 쉽게 말하면</span>
  <p>공구 가방을 처음부터 가득 채워 사는 사람은 없어요. <b>가장 자주 쓰는 드라이버 하나</b>부터 넣고, 필요할 때마다 하나씩 더하다 보면 남에게 빌려줄 만한 가방이 돼요.</p>
</div>

기존 plugin에 맞지 않는 워크플로가 있으면 Cowork와 함께 만들 수 있어요. Cowork가 워크플로에 필요한 skill을 묶고, 의존하는 커넥터를 포함시키고, 내 Cowork에 쉽게 설치할 수 있게 패키징해요.

<ol class="oa-flow">
  <li><strong>skill 하나</strong>가장 반복적인 작업부터</li>
  <li><strong>또 하나</strong>그다음 반복 작업</li>
  <li><strong>서너 개 + 커넥터</strong>중요한 커넥터가 모이면</li>
  <li><strong>공유할 가치</strong>plugin이 돼요 (공유는 Lesson 13)</li>
</ol>

> 관리자가 이미 조직용 plugin을 게시했을 수 있으니, 만들기 전에 **Directory(Customize → Plugins)**를 확인하세요.

## 6. 지금 해보기

내 작업에 맞는 plugin을 찾아봐요. 새 Cowork 대화에서 이렇게 입력해요.

```text
/setup-claude
```

짧은 인터뷰가 시작돼요. Claude가 하는 일의 종류를 묻고, 가장 맞는 plugin을 제안해요. 대화에서 바로 추가해 테스트해 볼 수 있고, 설치한 뒤에는 팀에 맞게 커스터마이징해요.

처음 정리할 때 강의는 이 명령을 아래처럼 안내했어요. 현재 강의 페이지는 `/setup-claude`로 표기해요.

```text
/setup-cowork
```

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. skill과 plugin의 관계를 한마디로 하면?</summary>
  <p><b>정답: skill이 플레이북 하나라면 plugin은 하나의 일을 중심으로 묶은 여러 skill + 그것들이 의존하는 커넥터 · subagent예요.</b></p>
</details>

<details class="oa-quiz">
  <summary>Q2. "실적 수집 → 분산 표 작성 → board memo 초안"을 묶은 monthly-close plugin은 두 형태 중 어느 쪽인가요?</summary>
  <p><b>정답: 형태 1, 처음부터 끝까지의 프로세스를 묶은 plugin.</b> 순차 단계의 skill을 묶어 전체가 하나로 돌아요. 서로 의존하지 않는 자주 쓰는 skill 모음이 형태 2예요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. marketplace에서 설치한 plugin의 NDA 검토 형식이 우리 팀과 달라요. 어떻게 하나요?</summary>
  <p><b>정답: Customize → Plugins → [Plugin 이름] → Customize로 가서 팀의 실제 예시를 주고 업데이트를 요청해요.</b> marketplace plugin은 기본값이지 최종 답이 아니에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 새 plugin을 직접 만들기 전에 먼저 할 일은?</summary>
  <p><b>정답: Directory(Customize → Plugins)를 확인해요.</b> 관리자가 이미 조직용 plugin을 게시했을 수 있어요.</p>
</details>

## 정리

- plugin은 하나의 일을 중심으로 묶은 **skill(+커넥터 · subagent) 패키지**로, 팀의 방식을 설치로 전파해요.
- 형태는 둘이에요: **처음부터 끝까지의 프로세스형**과 **자주 쓰는 skill 모음형.**
- **Customize → Plugins**에서 찾아 Install하고 커넥터를 승인하면 skill이 바로 사용 가능해져요.
- marketplace plugin은 **기본값**이에요. Customize로 팀의 템플릿 · 정의 · 단계에 맞게 다듬어요.
- 맞는 게 없으면 **skill 하나부터 작게** 만들어요. 그 전에 Directory를 먼저 확인해요.
- plugin이 워크플로를 실행해도 **출력 검토는 사용자 몫**이에요.

다음 모듈에서는 Cowork를 데스크톱 너머로 넓혀요: 브라우저, 그리고 많은 작업이 도착하는 M365 앱 안으로.
