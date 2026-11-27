---
title: "Skills"
title_ko: "Skills"
course: claude-code-101
lesson: 10
---

> 이 레슨은 주로 영상으로 제공되고, 아티클 본문은 전용 강의로 안내하는 한 줄뿐이에요. 아래는 **이 과정의 다른 레슨(Subagents, MCP)에서 skill에 대해 말한 내용**을 모아 정리한 요약이에요.

## 학습 목표

- **Skill**이 무엇인지 이해하기
- Skill이 context를 아끼는 방식(**필요할 때만 전체 로드**) 파악하기
- Skill이 **MCP 서버**, **subagent**와 어떻게 함께 쓰이는지 알기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>평소</strong>skill의 이름과 설명만 context에 있어요</li>
  <li><strong>판단</strong>Claude가 이 skill이 필요하다고 판단해요</li>
  <li><strong>로드</strong>그때 skill 전체 내용을 로드해요</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Skill은 이름과 설명만 올려 두고, 필요할 때만 전체를 펼치는 지침 패키지예요.**

## 1. Skill이란

<div class="oa-eli">
  <span class="oa-eli__head">📖 쉽게 말하면</span>
  <p>요리책의 <b>목차</b>만 머릿속에 넣어 두는 거예요. 김치찌개를 끓일 때가 되면 그제야 <b>그 페이지를 펼쳐요.</b> 책 전체를 외우고 다닐 필요가 없어요.</p>
</div>

**Skill**은 Claude에게 특정 workflow를 가르치는 지침 패키지예요.

| 시점 | context에 올라가는 것 |
|---|---|
| 평소 | skill의 **이름과 설명**만 |
| Claude가 그 skill이 필요하다고 판단했을 때 | skill의 **전체 내용** |

이 방식 덕분에 skill은 여러 개를 갖고 있어도 context 공간을 적게 써요.

## 2. 다른 기능과 함께 쓰기

<div class="oa-eli">
  <span class="oa-eli__head">🔧 쉽게 말하면</span>
  <p>MCP 서버가 <b>공구</b>라면 skill은 <b>사용 설명서</b>예요. 공구만 있어도 쓸 수는 있지만, 설명서가 있으면 훨씬 잘 써요.</p>
</div>

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">🔌</span><span class="oa-card__title">MCP 서버와 짝짓기</span>MCP 서버에, 그 서버의 tool을 <b>잘 쓰는 법을 가르치는 Skill</b>을 함께 두면 좋아요 (MCP 레슨)</div>
  <div class="oa-card"><span class="oa-card__icon">🤝</span><span class="oa-card__title">subagent에 preload</span><code>skills</code> 키에 이름을 나열해 미리 로드해요. 이때는 <b>skill 전체</b>가 context에 로드돼요 (Subagents 레슨)</div>
</div>

### 예전 노트와 달라진 점

| 예전 노트의 설명 | 현재 강의 본문 |
|---|---|
| MCP 서버는 모든 tool 정의를 미리 context에 올리고, Skill은 그렇지 않아서 MCP의 "가벼운 대안"이다 | MCP 서버도 기본적으로 **tool 이름과 instructions만** 올리고, 전체 정의는 필요할 때 로드해요. Skill은 MCP의 대안이라기보다 **MCP 서버와 함께 쓰는 짝**으로 소개돼요 |

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 평소에 skill의 어떤 부분이 context에 올라가 있나요?</summary>
  <p><b>정답: 이름과 설명만.</b> 전체 내용은 Claude가 필요하다고 판단할 때만 로드돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. MCP 서버와 Skill을 함께 쓰면 무엇이 좋은가요?</summary>
  <p><b>정답: Skill이 그 서버의 tool을 잘 쓰는 법을 Claude에게 가르쳐요.</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. subagent에 preload한 skill은 주 대화의 skill과 로딩 방식이 어떻게 다른가요?</summary>
  <p><b>정답: skill 전체가 context에 로드돼요.</b> 주 대화에서는 필요할 때만 전체가 로드돼요.</p>
</details>

## 정리

- Skill은 Claude에게 **특정 workflow를 가르치는 지침 패키지**예요.
- **이름과 설명만** 상시 로드되고, 필요할 때 전체가 로드돼서 context 효율이 좋아요.
- MCP 서버의 tool을 잘 쓰는 법을 가르치는 **짝**으로 쓸 수 있어요.
- subagent에 preload하면 **skill 전체**가 로드된다는 점을 기억하세요.
- 더 깊이 배우고 싶다면 전용 강의 *Introduction to agent skills*를 참고하세요.
