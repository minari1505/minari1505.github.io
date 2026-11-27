---
title: "Hand Claude Cowork your first task"
title_ko: "첫 작업 위임하기"
course: introduction-to-claude-cowork
lesson: 4
---

## 학습 목표

- Cowork 작업을 **위임 → 중간 조정 → 최종 검토**까지 처음부터 끝까지 진행하기
- Cowork의 확인 질문에 더 좋은 출력을 끌어내는 방식으로 답하기
- Claude가 잘못된 방향으로 갈 때 중간에 바로잡기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>위임</strong>결과물 · 입력 · 뉘앙스를 담은 프롬프트</li>
  <li><strong>확인 질문에 답하기</strong>시작 전에 맥락 간극 메우기</li>
  <li><strong>중간 조정</strong>틀어지면 바로 끼어들기</li>
  <li><strong>검토</strong>분별력을 켜고 결과물 확인</li>
  <li><strong>수정 요청</strong>다시 하지 말고 바꿀 부분만</li>
</ol>

이 레슨을 한 문장으로 줄이면: **챗봇에 프롬프트를 넣는 게 아니라, 일하러 들어갔다가 완성물을 들고 돌아올 동료에게 브리핑하는 거예요.**

## 1. 리듬 이해하기

<div class="oa-eli">
  <span class="oa-eli__head">🏠 쉽게 말하면</span>
  <p>인테리어 업자에게 집 수리를 맡기는 것과 비슷해요. 시작 전에 <b>몇 가지를 물어보고</b>, 공사 중에 <b>"거긴 그 색 말고요"</b>라고 말할 수 있고, 끝나면 <b>대화 기록이 아니라 고쳐진 집</b>을 둘러봐요.</p>
</div>

Cowork 작업은 챗봇에 프롬프트를 넣는 것보다, **작업 속으로 사라졌다가 완성물을 들고 돌아올 똑똑한 동료에게 브리핑하는 것**에 가까워요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">❓</span><span class="oa-card__title">착수 전에 물어요</span>사소하지 않은 작업은 대개 확인 질문 한둘로 시작해요. 방해가 아니라 시작 전 맥락 간극을 메우는 방법이에요</div>
  <div class="oa-card"><span class="oa-card__icon">🧭</span><span class="oa-card__title">중간에 조정할 수 있어요</span>방향이 틀리면 끼어들면 돼요. 끝날 때까지 기다렸다 다시 할 필요가 없어요</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📄</span><span class="oa-card__title">완성 산출물이 결과물이에요</span>채팅이 아니에요. Claude는 컴퓨터의 파일을 바꿔요. 마지막에 할 일은 동료의 작업을 검토하듯 결과를 보는 것</div>
</div>

## 2. 첫 작업 위임하기 — 좋은 프롬프트의 3요소

<div class="oa-eli">
  <span class="oa-eli__head">🍳 쉽게 말하면</span>
  <p>요리사에게 "맛있는 거 해 주세요"가 아니라 <b>"4인분 저녁(결과물), 냉장고에 있는 재료로(입력), 한 명은 땅콩 알레르기가 있어요(뉘앙스)"</b>라고 말하는 거예요. 마지막 한마디는 요리사가 냉장고만 봐서는 절대 알 수 없는 정보예요.</p>
</div>

Claude를 열고, 작업의 맥락이 사는 곳(desktop app이면 내 컴퓨터의 폴더, 아니면 project)을 고르고, 관련 맥락을 가진 커넥터를 켠 뒤, 프롬프트를 써요.

| 요소 | 무엇을 쓰나 | 예시 |
|---|---|---|
| **① 결과물을 지정** | 형식 · 길이를 구체화하면 재생성을 아껴요 | "1페이지 브리프", "QBR용 슬라이드", "후보별 메모가 붙은 순위 목록" |
| **② 입력을 지정** | 어떤 폴더, 어떤 채널, 어떤 기간, 어떤 앱. Cowork는 가리킨 맥락만큼만 좋아요 | "Q3 Competitive Review 폴더", "지난 분기 memo를 형식 참고로" |
| **③ 추가 세부 · 뉘앙스를 지정** | 원하는 출력의 종류를 알려 주고, 내 전문성에서 나온, Claude가 못 볼 통찰을 줘요 | "작년 Q3에 연 3개 신규 지점을 반영한 base/best/worst 시나리오를 보고 싶어." |

### 프롬프트 조립 예시 (강의의 interactive)

| 칸 | 고를 수 있는 예 |
|---|---|
| **Deliverable** | four-page memo · slide for the QBR · ranked list with notes |
| **Inputs** | Q3 Competitive Review 폴더 · 지난 분기 memo를 format reference로 · analyst-call PDF만 |
| **Nuance** | leadership offsite의 executive team용, 새 pricing tier 결정을 돕도록 · recommendation부터 제시 · 검증할 수 없는 것은 표시 |

세 칸이 다 채워지면 완전한 위임이에요: 결과물, 입력, 그리고 **Cowork가 추측할 수 없는 맥락**. 한 칸을 비워 두면 Cowork가 대개 되물어요.

> 챗봇에 익숙하다면 앞단 작업이 많게 느껴질 거예요. 실제로 그래요. 대신 왕복이 "아 그리고 이것도…"를 다섯 번 하는 게 아니라 **시작 전에 몰려요.**

## 3. 확인 질문에 답하기

<div class="oa-eli">
  <span class="oa-eli__head">🧾 쉽게 말하면</span>
  <p>택배 기사가 출발 전에 <b>"문 앞에 둘까요, 경비실에 맡길까요?"</b>라고 묻는 것과 같아요. 지금 5초 답하면, 나중에 엉뚱한 곳에서 상자를 찾아 헤맬 일이 없어요.</p>
</div>

Claude는 작업에 들어가기 전에 목표의 모호함을 없애려고 질문해요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">💬 채팅할 때</span>
    <ul>
      <li>하면서 협업해요</li>
      <li>맥락이 턴마다 조금씩 나와요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🤝 Cowork에 위임할 때</span>
    <ul>
      <li>Claude가 떠났다가 완성된 결과물을 들고 돌아와요</li>
      <li>걸림돌이 될 만한 이해의 간극은 <b>뛰어들기 전에 질문으로</b> 나와요</li>
    </ul>
  </div>
</div>

대부분 쉬워요. Claude가 옵션 몇 개를 제시하면 맞는 것을 클릭하고, 맞는 게 없으면 내 말로 직접 써요. 건너뛸(skip) 수도 있지만, 그러면 Claude가 **추측해야** 해요.

강의의 예시 질문이에요.

> "이 폴더에 memo가 둘 보입니다 — `Q2-board-memo.docx`와 `Q2-board-memo-FINAL.docx`. 어느 것에 맞출까요?"
> ① Q2-board-memo-FINAL.docx (Recommended) ② Q2-board-memo.docx ③ 둘 다 비슷하니 아무거나

## 4. 중간에 조정하기

<div class="oa-eli">
  <span class="oa-eli__head">🚕 쉽게 말하면</span>
  <p>택시가 엉뚱한 길로 들어서면 목적지에 도착할 때까지 기다렸다가 다시 타지 않죠. <b>"기사님, 다음에서 우회전이요"</b>라고 바로 말해요.</p>
</div>

작업 중 Claude의 계획과 진행을 지켜보세요. 잘못된 소스 · 형식 · 톤으로 가면 **끼어들어요.** 크게 어긋나면 실행을 중단하고, 배운 것을 반영해 프롬프트를 다듬어 다시 시작할 수도 있어요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">😐 Chat의 습관</span>
    <ul>
      <li>응답이 끝날 때까지 기다려요</li>
      <li>그다음 재생성해요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">💪 Cowork의 방식</span>
    <ul>
      <li>돌아가는 중에 계획을 바로잡아요</li>
      <li>현재 단계가 <b>재시작 없이</b> 업데이트되고, Cowork는 있던 자리에서 이어 가요</li>
    </ul>
  </div>
</div>

Chat의 습관을 버리세요. Cowork는 방향 수정에 맞게 설계됐고, 재지시 비용이 낮아요.

강의 시뮬레이션에서는 "leadership offsite용 Q3 competitive memo 초안"을 맡긴 작업이 이렇게 진행돼요. 4번 단계가 돌아가는 중에 메시지를 보내 방향을 바꿀 수 있어요.

<ol class="oa-flow">
  <li><strong>읽기</strong>source PDF와 지난 분기 memo</li>
  <li><strong>대조</strong>가격 변경 cross-reference</li>
  <li><strong>개요</strong>경쟁사 움직임 세 가지</li>
  <li><strong>초안 작성</strong>Q2 board-memo 템플릿에 맞춰 ← 여기서 steer</li>
  <li><strong>검토</strong>원래 프롬프트와 대조</li>
</ol>

## 5. 완성물 검토하기

<div class="oa-eli">
  <span class="oa-eli__head">🔎 쉽게 말하면</span>
  <p>실력은 믿지만 <b>아직 함께 일한 지 얼마 안 된 신입의 첫 보고서</b>를 읽는 것처럼 보세요. 대충 넘기지도, 전부 의심하지도 않고, 숫자와 인용은 원본을 펴서 맞춰 봐요.</p>
</div>

끝나면 앱에서 파일을 미리 볼 수 있어요. 파일은 폴더에도 있으니 거기서 열어 봐도 돼요. **신뢰하지만 아직 잘 모르는 사람의 초안을 읽듯** 분별력을 켜고 검토하세요. 특히 처음 몇 번은요.

| 확인할 것 | 어떻게 |
|---|---|
| **실제 목표를 충족하나?** | 요청한 것과 미묘하게 다른 걸 만들지 않았는지 봐요 |
| **사실이 정확한가?** | 수치 · 인용이 있으면 소스로 확인해요. 먼저 Claude에게 **참조한 문서를 밝히게** 한 뒤 직접 대조해요 |
| **지어낸 티가 나나?** | 입력에서 추적되지 않는 특정 날짜 · 이름 · 인용은 경고 신호예요 |

### 고칠 때는

| 상태 | 할 일 |
|---|---|
| **대체로 맞음** | 처음부터 다시 하지 말고 바꿀 부분을 말해요. Claude는 대화를 기억하고, 재생성보다 빠르게 수정해요 |
| **핵심에서 틀림** | 프롬프트에 핵심 맥락이 빠져 있던 거예요. Claude에게 새 맥락을 가리키고 조정해 달라고 해요 |

### 지금 해보기

Lesson 3에서 고른 작업(또는 지금 하나 골라서)을 방금 본 과정대로 Cowork에 넘겨 보세요: 결과물 · 입력 · 뉘앙스를 지정하고, 확인 질문에 구체적으로 답하고, 벗어나면 조정하고, 분별력을 켜고 결과를 검토해요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 좋은 Cowork 프롬프트가 지정하는 세 가지는 무엇인가요?</summary>
  <p><b>정답: 결과물(deliverable) · 입력(inputs) · 추가 세부와 뉘앙스.</b> 비워 두면 Cowork가 대개 되묻지만, 처음에 채워 두면 왕복이 시작 전에 끝나요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. Claude가 작업 중 잘못된 소스를 읽고 있는 게 보여요. 가장 좋은 대응은?</summary>
  <p><b>정답: 끝나길 기다리지 말고 바로 끼어들어 방향을 바꿔요.</b> Cowork는 중간 수정에 맞게 설계됐고, 있던 자리에서 이어 가요. 크게 어긋났다면 중단하고 프롬프트를 다듬어 다시 시작해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 완성된 memo에 입력 파일 어디에서도 찾을 수 없는 구체적인 날짜와 인용이 있어요. 어떻게 봐야 하나요?</summary>
  <p><b>정답: 경고 신호예요.</b> Claude에게 참조한 문서를 밝히게 하고 직접 소스와 대조해요. 입력에서 추적되지 않는 날짜 · 이름 · 인용은 지어낸 것일 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 초안이 대체로 맞는데 톤만 조금 달라요. 새 대화에서 처음부터 다시 맡기는 게 좋을까요?</summary>
  <p><b>정답: 아니요.</b> 바꿀 부분만 말하면 돼요. Claude는 대화를 기억하고, 재생성보다 수정이 빨라요.</p>
</details>

## 정리

- **위임 → 확인 질문 응답 → 중간 조정 → 분별 있는 검토**가 하나의 흐름이에요.
- 좋은 프롬프트는 **결과물 · 입력 · 뉘앙스**를 명시해요. 앞단 작업이 늘지만 왕복이 시작 전에 몰려요.
- 확인 질문은 방해가 아니라 **맥락 간극을 메우는 방법**이에요. 건너뛰면 Claude가 추측해요.
- 틀어지면 **기다리지 말고 끼어들어요.** 재지시 비용이 낮아요.
- 결과물은 채팅이 아니라 **파일**이에요. 목표 충족, 사실 정확성, 지어낸 흔적을 확인해요.
- 첫 실행이 가장 많은 것을 가르쳐 줘요. 일단 진짜 작업으로 해보세요.

Module 2에서는 잘 통했던 맥락, 반복하고 싶은 워크플로, 팀이 의존하는 전문성을 Cowork에 남겨서 매번 다시 주지 않아도 되게 만들어요.
