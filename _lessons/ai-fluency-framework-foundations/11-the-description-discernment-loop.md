---
title: "The Description-Discernment loop"
title_ko: "Description-Discernment 루프"
course: ai-fluency-framework-foundations
lesson: 11
---

## 학습 목표

- Description과 Discernment 기술을 **실제 프로젝트**에 적용하기
- 생산적인 **Description-Discernment 피드백 루프** 돌리기
- 사람이나 AI 혼자서는 얻기 어려운 결과를 **협업으로** 만들어 내기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>계획 검토</strong>프로젝트 계획과 위임 결정 다시 보기</li>
  <li><strong>Description 준비</strong>Product · Process · Performance 기대치 정하기</li>
  <li><strong>루프 실행</strong>설명 → 평가 → 다듬기 → 통합</li>
  <li><strong>완성</strong>모든 작업에 대해 반복</li>
</ol>

이 레슨을 한 문장으로 줄이면: **한 번에 끝내려 하지 말고, "말하고 → 따져 보고 → 고치는" 고리를 여러 번 돌려요.** (예상 소요 30~60분)

## 1. 시작 전 준비

<div class="oa-eli">
  <span class="oa-eli__head">🎬 쉽게 말하면</span>
  <p>촬영에 들어가기 전에 감독과 배우가 <b>대본, 연기 방식, 현장에서의 소통 방식</b>을 먼저 맞추는 것과 같아요. 미리 맞춰 두면 촬영이 훨씬 수월해요.</p>
</div>

**Step 1 — 프로젝트 계획 검토**

- "프로젝트 계획과 Delegation" 레슨에서 만든 계획을 꺼내요.
- 어떤 작업에 사람의 전문성 / AI의 능력 / 협업이 맞는지 정한 **위임 결정**을 빠르게 다시 봐요.
- 그사이 배운 것을 반영해 계획을 **고쳐도 좋아요.**

**Step 2 — Description 접근 준비**

Claude와 대화를 시작해 함께 할 프로젝트를 설명하고, 실행 전에 세 가지를 먼저 정해요.

| 종류 | 미리 정할 것 |
|---|---|
| **Product Description** | 작업마다 Claude에게 필요한 구체적 출력은? 형식 · 스타일 · 길이 · 상세함의 수준은? |
| **Process Description** | Claude가 각 작업에 어떻게 접근해야 하나? 따랐으면 하는 방법 · 프레임워크 · 단계가 있나? |
| **Performance Description** | 이 프로젝트에서 어떤 협업 태도를 원하나? 간결/상세, 도전적/지지적, 아이디어 중심/분석 중심? |

## 2. Description-Discernment 루프

<div class="oa-eli">
  <span class="oa-eli__head">🏺 쉽게 말하면</span>
  <p>도자기를 빚을 때 <b>모양을 잡고, 돌려 보며 살피고, 다시 다듬는</b> 과정을 반복하죠. 마지막에 유약을 고르고 서명하는 건 도예가 본인이에요.</p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 250" role="img" aria-label="Describe, Discern, Refine, Integrate가 반복되는 루프">
  <defs>
    <marker id="ar_afl11" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.b1{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.b3{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.h1{font-size:15px;font-weight:700;fill:#333}.t1{font-size:12px;fill:#4a4a4a}.s1{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect class="b1" x="10" y="40" width="160" height="90" rx="10"/>
  <text class="h1" x="90" y="70" text-anchor="middle">① Describe</text>
  <text class="t1" x="90" y="94" text-anchor="middle">무엇을 · 어떻게 ·</text>
  <text class="t1" x="90" y="112" text-anchor="middle">어떤 태도로</text>
  <line x1="174" y1="85" x2="192" y2="85" stroke="#c99400" stroke-width="2" marker-end="url(#ar_afl11)"/>
  <rect class="b1" x="200" y="40" width="160" height="90" rx="10"/>
  <text class="h1" x="280" y="70" text-anchor="middle">② Discern</text>
  <text class="t1" x="280" y="94" text-anchor="middle">결과물 · 과정 ·</text>
  <text class="t1" x="280" y="112" text-anchor="middle">행동을 평가</text>
  <line x1="364" y1="85" x2="382" y2="85" stroke="#c99400" stroke-width="2" marker-end="url(#ar_afl11)"/>
  <rect class="b1" x="390" y="40" width="160" height="90" rx="10"/>
  <text class="h1" x="470" y="70" text-anchor="middle">③ Refine</text>
  <text class="t1" x="470" y="94" text-anchor="middle">피드백 주고</text>
  <text class="t1" x="470" y="112" text-anchor="middle">설명을 조정</text>
  <line x1="554" y1="85" x2="572" y2="85" stroke="#c99400" stroke-width="2" marker-end="url(#ar_afl11)"/>
  <rect class="b3" x="580" y="40" width="130" height="90" rx="10"/>
  <text class="h1" x="645" y="70" text-anchor="middle">④ Integrate</text>
  <text class="t1" x="645" y="94" text-anchor="middle">내 판단을 더해</text>
  <text class="t1" x="645" y="112" text-anchor="middle">최종 결정</text>
  <path d="M470,134 L470,180 L90,180 L90,138" fill="none" stroke="#c99400" stroke-width="2" stroke-dasharray="5 4" marker-end="url(#ar_afl11)"/>
  <text class="s1" x="280" y="200" text-anchor="middle">만족할 때까지 다시 설명 → 평가 → 다듬기</text>
  <text class="s1" x="360" y="235" text-anchor="middle">작업 하나가 끝나면 다음 작업에서 같은 루프를 다시 돌려요</text>
</svg>
</div>

계획한 작업마다 아래 네 단계를 거쳐요.

| 단계 | 할 일 |
|---|---|
| **① Describe (설명)** | 원하는 것을 구체적으로(Product) · 접근 방식을 안내하고(Process) · 과정에서 어떻게 상호작용할지 지정(Performance) |
| **② Discern (평가)** | 출력 자체를 평가(Product) · 작업에 접근한 방식을 평가(Process) · Claude의 행동이 지금 필요한 도움에 맞는지 살핌(Performance) |
| **③ Refine (다듬기)** | 무엇이 잘됐고 무엇이 아닌지 피드백 · 필요하면 설명을 명확히 하거나 조정 · 만족할 때까지 반복 요청 |
| **④ Integrate (통합)** | 내 관점 · 창의성 · 도메인 지식을 더함 · 무엇을 남기고 고치고 버릴지 **최종 결정** · 최종 결과물에 **책임** |

> ④가 빠지면 루프가 아니라 "AI가 준 걸 그대로 받아쓰기"가 돼요. 최종 결정과 책임은 사람에게 있어요.

### 돌아보기 (Reflection)

- 가장 좋은 결과로 이어진 설명에는 어떤 패턴이 있었나요?
- Description과 Discernment 중 어느 쪽에 노력이 더 들었나요? 왜 그랬을까요?
- 실제 실행은 처음 세운 계획과 어떻게 달랐나요? 중간에 무엇을 조정했나요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 루프의 순서로 맞는 것은?</summary>
  <p>① 평가 → 설명 → 통합 → 다듬기 ② <b>설명 → 평가 → 다듬기 → 통합</b> ③ 통합 → 설명 → 평가 → 다듬기 ④ 설명 → 통합 → 평가 → 다듬기</p>
  <p><b>정답: ②</b> — Describe → Discern → Refine → Integrate.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. "통합(Integrate)" 단계에서 사람이 하는 일이 <b>아닌</b> 것은?</summary>
  <p>① 내 관점과 도메인 지식을 더한다 ② 무엇을 남기고 버릴지 최종 결정한다 ③ 최종 결과물에 책임을 진다 ④ <b>AI가 낸 결과를 검토 없이 그대로 제출한다</b></p>
  <p><b>정답: ④</b></p>
</details>

<details class="oa-quiz">
  <summary>Q3. 처음 세운 프로젝트 계획은 실행 중에 바꾸면 안 된다. (O / X)</summary>
  <p><b>정답: X</b> — 그사이 배운 것을 반영해 계획을 고쳐도 좋아요.</p>
</details>

## 정리

- 계획한 프로젝트를 **Description + Discernment**로 실제로 실행해 보는 레슨이에요.
- 실행 전에 **Product · Process · Performance** 기대치를 Claude와 먼저 맞춰요.
- 작업마다 **설명 → 평가 → 다듬기 → 통합** 루프를 돌려요.
- 만족할 때까지 **반복**하는 것이 정상이에요.
- 마지막에는 내 전문성과 판단을 더하고, **최종 결정과 책임**은 내가 져요.
