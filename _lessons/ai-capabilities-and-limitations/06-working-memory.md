---
title: "Working Memory"
title_ko: "작업 기억 (context window)"
course: ai-capabilities-and-limitations
lesson: 6
---

## 학습 목표

- context window를 **고정 크기 그릇**으로 이해하고, 긴 문서·긴 대화·세션 간 기억에 무엇을 의미하는지 설명하기
- 다른 속성의 점진적 저하와 달리 이 속성이 갖는 **"절벽(cliff)"** 성격 인식하기
- "맥락을 지렛대로" 쓰는 전략(중요 자료 앞배치, 긴 작업 쪼개기, 핵심 맥락 재공급) 적용하기
- 이 한계를 다루는 제품 기능(memory, compaction, projects/workspaces, 큰 window) 인식하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>고정 크기 작업 공간</strong>context window 안의 것만 참조</li>
  <li><strong>맥락은 지렛대</strong>내 자료·제약을 넣으면 바로 반영</li>
  <li><strong>절벽</strong>넘치면 조용히 잘리고, 중간은 흐려짐</li>
  <li><strong>백지 시작</strong>세션이 바뀌면 0부터, 교정해도 모델은 안 바뀜</li>
  <li><strong>대응</strong>앞배치 · 쪼개기 · 재공급 · memory/projects</li>
</ol>

이 레슨을 한 문장으로 줄이면: **AI는 지금 context window 안에 있는 것에만 반응하고, 이 한계는 서서히 나빠지는 게 아니라 "되다가 갑자기 안 돼요".**

## 1. context window — AI의 작업 기억

<div class="oa-eli">
  <span class="oa-eli__head">🗄️ 쉽게 말하면</span>
  <p>크기가 정해진 <b>책상</b>이라고 생각해 보세요. 책상 위에 펼쳐 둔 서류는 바로 보고 일할 수 있지만, 책상에 안 올라간 서류는 없는 것과 같아요. 그리고 서류를 계속 올리다 보면 어느 순간 <b>앞에 있던 것이 소리 없이 바닥으로 떨어져요</b>.</p>
</div>

AI가 주의를 두는 모든 것은 **context window**라는 고정 크기 작업 공간 안에 있어요. 그 안의 것은 참조할 수 있고, 밖의 것은 참조할 수 없어요. 이 제약은 다른 속성들과 달리 **딱딱한 경계**를 가져요 — **되다가 갑자기 안 돼요.**

{% include capability-spectrum.html title="Working Memory" subtitle="What is the AI paying attention to right now?" %}

> **읽기 전 질문 (강의의 Before you read)**: 50쪽짜리 계약서를 AI에게 검토시켰어요. 결과를 얼마나 꼼꼼히 확인해야 할까요? Trust it → Spot-check → Check details → Verify carefully → High risk 중 한 지점을 먼저 골라 보세요.

## 2. 능력과 한계

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍🍳 쉽게 말하면</span>
  <p>실력 좋은 <b>출장 요리사</b>예요. 레시피와 재료를 건네주면 바로 내 입맛대로 만들어 줘요. 하지만 다음 날 다시 부르면 <b>어제 일을 기억하지 못해서</b> 레시피를 또 건네줘야 해요.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 이것이 가능하게 하는 것 (능력)</span>
    <ul>
      <li>세션 안에서 즉각적 적응 (스타일 가이드·샘플이 바로 반영됨)</li>
      <li>내 자료로 작업 (내 문서, 내 제약 조건)</li>
      <li>구체성을 통한 정밀함 — <b>맥락이 곧 지렛대(context is leverage)</b></li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">⚠️ 특징적으로 실패하는 곳 (한계)</span>
    <ul>
      <li><b>딱딱한 길이 한계</b> — 초과 시 조용히 잘림(silent truncation)</li>
      <li><b>중간에서 길을 잃음(lost in the middle)</b> — 주의는 window 전체에 균일하지 않음</li>
      <li><b>기본적으로 지속 기억 없음</b> — 각 세션은 0에서 시작</li>
      <li><b>사용자로부터 학습하지 않음</b> — 교정은 모델이 아니라 context만 바꿈</li>
    </ul>
  </div>
</div>

### 한계를 밀어내는 제품 기능

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">💾</span><span class="oa-card__title">Memory</span>세션을 넘어 사실을 유지</div>
  <div class="oa-card"><span class="oa-card__icon">🗜️</span><span class="oa-card__title">Compaction / 요약</span>오래된 턴을 압축해 공간 확보</div>
  <div class="oa-card"><span class="oa-card__icon">📁</span><span class="oa-card__title">Projects / workspaces</span>상시 문서를 안정적으로 context에 유지</div>
  <div class="oa-card"><span class="oa-card__icon">🧩</span><span class="oa-card__title">Skills</span>필요할 때까지 context 사용 최소화</div>
  <div class="oa-card"><span class="oa-card__icon">📐</span><span class="oa-card__title">더 큰 context window</span>절벽을 더 멀리 밀어냄</div>
</div>

강의의 정리는 여기에 **multi-agent workflow**도 함께 꼽아요. 모두 이 절벽을 더 멀리 밀어내려고 존재하는 장치예요.

## 3. "절벽" 성격

<div class="oa-eli">
  <span class="oa-eli__head">⛰️ 쉽게 말하면</span>
  <p>다른 속성은 <b>내리막길</b>이라 서서히 나빠지는 게 느껴져요. 이 속성은 <b>안개 낀 절벽</b>이에요. 평지처럼 잘 걷다가 한 발짝에 떨어지는데, 경고 표지판이 항상 있는 것도 아니에요.</p>
</div>

이 속성의 실패는 점진적 저하가 아니라 **절벽**이에요. **조용한 잘림(silent truncation)**이 대표적 실패 방식이며, 항상 경고받지는 못해요. 또한 모델은 내 교정으로부터 **학습하지 않고**, 오직 현재 context에 있는 것에만 반응해요.

| 구간 | 조건 |
|---|---|
| **능력 구간** | 자료가 여유 있게 들어가고, 세션이 최신이며, 관련 맥락을 직접 제공할 때 |
| **한계 구간** | 아주 긴 문서·대화, 세션 간 연속성 기대, 핵심 정보를 긴 입력의 중간에 묻을 때 |

**맥락을 지렛대로 쓰는 전략**

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">⬆️</span><span class="oa-card__title">앞배치 (front-loading)</span>중요한 자료를 앞에 둬요</div>
  <div class="oa-card"><span class="oa-card__icon">✂️</span><span class="oa-card__title">쪼개기 (chunking)</span>긴 작업을 나눠요</div>
  <div class="oa-card"><span class="oa-card__icon">🔁</span><span class="oa-card__title">재공급</span>핵심 맥락을 다시 넣어 줘요</div>
</div>

> **4D 연결**: Working Memory는 **Description**이 작용하는 대상이에요. window 작동 방식을 알면 맥락을 어떻게 구조화할지, 언제 앞배치하고 언제 새로 시작할지 알 수 있어요.

## 4. Try it out — Context Degradation

"다 넣어 주면 더 잘하겠지"라는 직감이 항상 맞지는 않아요. 체험 페이지는 단어 15개를 약 1.5초씩 보여 주는 기억력 테스트로 시작해요. 해 보면 **중간에 있던 단어부터 사라져요.**

<div class="oa-svg">
<svg viewBox="0 0 720 250" role="img" aria-label="목록 위치에 따른 회상 확률의 U자 곡선: 처음과 끝은 높고 중간은 낮다">
  <defs>
    <style>.ax6{stroke:#b8b2a0;stroke-width:1.5}.cv6{fill:none;stroke:#c99400;stroke-width:3}.h6{font-size:14px;font-weight:700;fill:#333}.t6{font-size:12px;fill:#4a4a4a}.s6{font-size:11px;fill:#8a8a8a}.z6{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5}</style>
  </defs>
  <rect class="z6" x="250" y="40" width="220" height="150" rx="10"/>
  <line class="ax6" x1="70" y1="190" x2="660" y2="190"/>
  <line class="ax6" x1="70" y1="30" x2="70" y2="190"/>
  <path class="cv6" d="M80,55 C200,60 240,165 360,168 C480,165 520,60 650,55"/>
  <text class="h6" x="110" y="45" text-anchor="middle">Primacy</text>
  <text class="h6" x="360" y="110" text-anchor="middle">Lost in the middle</text>
  <text class="h6" x="620" y="45" text-anchor="middle">Recency</text>
  <text class="t6" x="110" y="80" text-anchor="middle">처음: 더 많이 되뇜</text>
  <text class="t6" x="360" y="130" text-anchor="middle">어느 쪽 이점도 없음</text>
  <text class="t6" x="620" y="80" text-anchor="middle">끝: 아직 생생함</text>
  <text class="s6" x="365" y="212" text-anchor="middle">목록(context) 안에서의 위치 →</text>
  <text class="s6" x="40" y="115" text-anchor="middle" transform="rotate(-90 40 115)">회상 확률 →</text>
  <text class="s6" x="360" y="238" text-anchor="middle">개념도예요 (실제 수치 아님). 강의의 serial position effect 그림을 다시 그린 것</text>
</svg>
</div>

이 현상의 이름은 **serial position effect**예요. 목록의 처음 항목은 primacy(더 많이 되뇜), 끝 항목은 recency(아직 생생함)의 이점을 얻고, 중간은 둘 다 못 얻어요. 체험 페이지에 따르면 LLM도 같은 패턴을 보여요.

- 2023년 Stanford 연구자들이 긴 context window의 여러 위치에 핵심 사실을 놓아 봤더니, **맨 앞이나 맨 끝**에 있을 때 정확도가 가장 높았고 **중간에 묻혔을 때는 30% 넘게 떨어졌어요.**
- 우연한 특이점이 아니라 구조적인 현상이에요. transformer의 attention 패턴이 context window의 **가장자리에 더 큰 가중**을 둬요.
- 그래서 20쪽 문서를 붙여 넣고 11쪽 내용을 물으면, 1쪽이나 20쪽 내용보다 놓칠 가능성이 커요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 위험한 패턴</span>
    <ul>
      <li>System prompt</li>
      <li>Chat message 1, 2, …</li>
      <li><b>⚠ 핵심 지시가 여기 묻힘</b></li>
      <li>… Chat message 18</li>
      <li>최신 user message</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 더 안전한 패턴</span>
    <ul>
      <li>System prompt</li>
      <li><b>★ 핵심 지시 (맨 앞에)</b></li>
      <li>Chat message 1, 2, … 18</li>
      <li>최신 user message</li>
      <li><b>★ 핵심 지시 (반복)</b></li>
    </ul>
  </div>
</div>

실용 조언은 단순해요: **가장 중요한 지시는 context의 처음과 끝에 둬요.** 반드시 지켜져야 하는 제약이라면 system prompt 앞부분에 말하고 끝 가까이에서 다시 말해요. 그 사이의 모든 것에 모델이 같은 무게를 줄 거라 기대하지 않아요.

> **더 많은 context ≠ 더 좋은 결과.** context를 하나 더 넣을 때마다 다른 조각이 중간(attention의 사각지대)으로 더 밀려나요. 무엇을 넣을지만이 아니라 **어디에 둘지, 무엇을 뺄지**가 context engineering의 핵심 긴장이에요. 과감히 고르고, 전략적으로 배치하고, 중요한 것은 반복해요.

## 5. 실습 — Before-and-After

맥락은 지렛대예요. 같은 작업도 맞는 맥락을 주면 평범한 초안에서 정말 쓸 만한 결과로 바뀌어요.

1강 목록에서 **나만 가진 맥락**이 도움이 되는 작업을 골라요: 스타일 가이드, 잘된 과거 예시, 내 역할이나 독자에 특화된 제약. 그리고 이 작업의 "좋은 결과물"이 어떤 모습인지, 낯선 사람도 평가할 수 있을 만큼 분명하게 두세 줄로 적어요.

| Probe | 방법 | 볼 것 |
|---|---|---|
| **1. 콜드 스타트 vs 맥락** | 맥락 없이 요청만 던져 출력 저장 → 새 대화에서 스타일 가이드·예시·제약을 앞배치해 다시 요청 | 두 출력을 내 "좋음" 정의에 대 보고 간격 재기 |
| **2. 중간에서 길 잃기** | 긴 문서(또는 참고 자료 몇 문단을 이어 붙인 것) 중간에 중요한 지시 하나를 묻고, 그 지시에 정답이 달린 질문 → 지시를 맨 위로 옮겨 다시 질문 | 묻힌 지시를 잡았는가. 위치를 바꾸면 달라지는가 |
| **3. 백지 상태** | 내 업무 맥락을 가르치거나 틀린 것을 교정한 뒤, 완전히 새 대화에서 그걸 기억한다고 가정한 질문 | 0에서 시작하는 모습 |

실습 뒤에는 작업 목록에 **세 번째 주석**을 달아요: 어떤 작업이 상시 맥락(project, 저장된 지침, 업로드한 참고 문서)을 갖춰야 돌릴 가치가 있고, 어떤 작업은 맥락 없이도 잘되는지 표시해요.

**도전 과제**: 쓰는 도구에 memory나 project 기능이 있으면 Probe 1의 맥락으로 하나 설정하고 작업을 다시 실행해요. 콜드 스타트 버전과 노력·품질을 비교해요.

**돌아보기**

- 맥락을 앞배치한 것이 출력 품질을 얼마나 바꿨나요? 예상보다 간격이 컸나요?
- 같은 설명을 반복하지 않으려고 이번 주에 설정해 둘 상시 맥락 하나는?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Working Memory가 다른 세 속성과 다른 점은?</summary>
  <p>① 한계가 전혀 없다 ② <b>점진적 저하가 아니라 절벽처럼 실패한다</b> ③ 학습 데이터에만 좌우된다 ④ 지시가 길수록 좋아진다</p>
  <p><b>정답: ②</b> — 대표적 실패 방식은 silent truncation이고, 항상 경고를 받는 것도 아니에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 어제 대화에서 AI의 실수를 교정했는데, 오늘 새 대화에서 같은 실수를 한다. 이유는?</summary>
  <p><b>정답: 교정은 모델이 아니라 그 대화의 context만 바꾸기 때문이에요.</b> 기본적으로 지속 기억이 없어서 각 세션은 0에서 시작해요. memory나 projects 같은 기능이 이 간극을 메워요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 반드시 지켜져야 하는 제약은 긴 context의 어디에 두는 것이 좋은가요?</summary>
  <p>① 중간쯤에 한 번 ② 아무 데나, 위치는 상관없다 ③ <b>맨 앞에 두고 끝 가까이에서 다시 말한다</b> ④ 가장 긴 문서 안에 섞어 둔다</p>
  <p><b>정답: ③</b> — attention은 window 전체에 균일하지 않고 가장자리에 더 큰 가중이 실려요 (lost in the middle).</p>
</details>

<details class="oa-quiz">
  <summary>Q4. "context는 많이 넣을수록 좋다." (O / X)</summary>
  <p><b>정답: X</b> — 하나를 더 넣을 때마다 다른 조각이 중간으로 밀려나요. 고르고, 배치하고, 중요한 것을 반복하는 쪽이 답이에요.</p>
</details>

## 정리

- Working Memory는 AI가 참조할 수 있는 **고정 크기 context window**를 갖는다는 사실이에요.
- 능력 구간은 자료가 여유 있고 맥락을 직접 제공할 때, 한계 구간은 아주 길거나 세션 간 연속성을 기대하거나 핵심을 중간에 묻을 때예요.
- 이 속성은 점진적 저하가 아니라 **절벽**이며, 조용한 잘림이 주요 실패 방식이에요. 항상 경고받지는 못해요.
- 모델은 교정으로부터 학습하지 않고 현재 context에만 반응해요.
- memory, compaction, projects, 더 큰 window, multi-agent workflow는 모두 이 절벽을 더 멀리 밀어내는 장치예요.
- **4D 연결**: Working Memory는 **Description**이 작용하는 대상이에요 — window 작동 방식을 알면 언제 맥락을 앞배치하고 언제 새로 시작할지 알 수 있어요.
