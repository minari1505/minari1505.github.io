---
title: "Troubleshooting and AI Fluency"
title_ko: "문제 해결과 AI Fluency"
course: claude-101
lesson: 2
---

## 학습 목표

- Claude를 처음 쓸 때 흔히 겪는 문제를 알아보고 **해결 방법** 익히기
- 첫 prompt를 완성 요청이 아니라 대화의 시작점으로 보는 **iteration mindset** 이해하기
- **AI Fluency**와 **4D Framework**의 핵심 개념 정리하기
- 내 업무 흐름에 Claude가 잘 맞는지 확인하는 간단한 **eval** 방법 배우기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>기대와 다른 답</strong>실패가 아니라 조정할 기회</li>
  <li><strong>문제별 처방</strong>맥락 · 길이 · 형식 · 검증 · 톤</li>
  <li><strong>Iteration mindset</strong>첫 prompt는 대화의 시작</li>
  <li><strong>AI Fluency 4D</strong>맡기기 · 설명 · 분별 · 책임</li>
  <li><strong>간단한 eval</strong>내 업무에 맞는지 직접 확인</li>
</ol>

이 레슨을 한 문장으로 줄이면: **Claude가 추측해야 하는 부분을 줄이고, 결과를 보고 고치고, 중요한 건 직접 검증한다.**

## 1. Claude가 기대와 다르게 답할 때

<div class="oa-eli">
  <span class="oa-eli__head">💇 쉽게 말하면</span>
  <p>미용실에서 "알아서 잘라 주세요"라고 하면 마음에 안 들 확률이 높아요. <b>"앞머리는 눈썹 위, 옆은 귀가 보이게"</b>라고 말해야 원하는 머리가 나오죠. Claude의 답이 어긋났다면 대부분 내 설명에 빈칸이 있었다는 신호예요.</p>
</div>

Claude를 쓰다 보면 답이 너무 일반적이거나, 길이가 안 맞거나, 형식을 안 지키거나, 그럴듯하지만 틀린 정보가 나올 때가 있어요. 이건 정상이고, 실패라기보다 **요청 방식을 조정할 기회**예요.

### 흔한 문제와 해결 방법

| 문제 | 왜 생기나 | 시도해 볼 것 |
|---|---|---|
| 답변이 너무 일반적임 | 내 상황에 대한 맥락이 부족해요 | 대상, 역할, 제약 조건을 추가하기 |
| 답변이 너무 길거나 짧음 | Claude가 적절한 길이를 추측해요 | "두 문단", "100단어 이하", "포괄적인 분석이 필요해. 길이 제한 없음"처럼 명시하기 |
| 원하는 형식을 따르지 않음 | 무엇을 원하는지는 알지만 어떻게 보여 줄지 몰라요 | 말로만 하지 말고 **보여 주기**. 형식 예시를 주거나 "섹션마다 굵은 제목이 달린 bullet point로"처럼 구조를 설명하기 |
| 자신감 있어 보이지만 틀린 정보 | 특정 사실이나 niche topic에서 plausible but incorrect 정보가 가끔 생성돼요 | 고위험 작업의 핵심 사실은 **독립 검증**하기. source/citation이나 confidence level 요청하기. web search를 켜서 최신 정보에 근거하게 하기 |
| 톤이 맞지 않음 | Claude는 기본적으로 helpful하고 professional한 톤으로 답해요 | "더 대화체로", "권위 있고 공식적으로"처럼 평이한 말로 톤 설명하기. 원하는 스타일의 글 예시 주기 |

### 예시 — 일반적인 답을 구체적인 답으로

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 너무 일반적인 prompt</span>
    <ul>
      <li>"프로젝트 지연에 대한 이메일을 써줘."</li>
      <li>→ 누구에게, 얼마나, 어떤 상황인지 Claude가 전부 추측해야 해요.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 상황과 독자를 알려 주는 prompt</span>
    <ul>
      <li><b>독자</b>: 엔터프라이즈 고객</li>
      <li><b>상황</b>: 소프트웨어 통합 2주 지연, 이번이 두 번째</li>
      <li><b>톤</b>: 전문적이되 사과하는 톤</li>
    </ul>
  </div>
</div>

```text
프로젝트 지연에 대한 이메일을 써줘.
```

위 prompt 대신 이렇게 써요.

```text
엔터프라이즈 고객에게 소프트웨어 통합이 2주 지연된다는 이메일을 써줘.
그들은 지금까지 기다려줬지만 이번이 두 번째 지연이야.
전문적이되 사과하는 톤으로 작성해줘.
```

> Claude가 더 잘 답하게 하려면 **Claude가 추측해야 하는 부분을 줄이면** 돼요.

## 2. Iteration mindset

<div class="oa-eli">
  <span class="oa-eli__head">🍲 쉽게 말하면</span>
  <p>찌개를 끓일 때 한 번에 간을 딱 맞추는 사람은 드물어요. <b>맛을 보고, 소금을 조금 넣고, 다시 맛을 보죠.</b> Claude의 첫 답변도 완성된 요리가 아니라 첫 간 보기예요.</p>
</div>

가장 중요한 전환은 첫 prompt를 "완성본 요청"이 아니라 **대화의 시작**으로 보는 거예요. 첫 prompt가 완벽한 결과를 내는 일은 드물고, 그래도 괜찮아요.

<ol class="oa-flow">
  <li><strong>초안 요청</strong>첫 초안은 출발점</li>
  <li><strong>검토</strong>좋은 점과 부족한 점 확인</li>
  <li><strong>구체적 피드백</strong>무엇을 어떻게 바꿀지</li>
  <li><strong>예시 · 맥락 추가</strong>필요하면 보강</li>
  <li><strong>새로 시작</strong>너무 엇나가면 새 chat</li>
</ol>

피드백은 구체적일수록 좋아요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">😐 괜찮지만 막연한 피드백</span>
    <ul>
      <li>"짧게 만들어줘."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">💪 더 좋은 피드백</span>
    <ul>
      <li>"첫 두 문단은 빼고, 결론을 더 행동 지향적으로 바꿔줘."</li>
    </ul>
  </div>
</div>

**언제 새로 시작할지도 알아야 해요.** 대화가 엇나갔다면 방향을 되돌리려 애쓰는 것보다 새 chat에서 더 명확한 prompt로 다시 시작하는 편이 빠를 때가 있어요.

## 3. AI Fluency와 4D Framework

<div class="oa-eli">
  <span class="oa-eli__head">🚗 쉽게 말하면</span>
  <p>운전을 잘한다는 건 페달 위치를 아는 게 아니에요. <b>언제 속도를 줄이고, 어디를 살피고, 사고가 나면 누가 책임지는지</b> 아는 거죠. AI Fluency도 버튼 위치가 아니라 판단력이에요.</p>
</div>

**AI Fluency**는 AI 도구와 효과적으로 협업하는 능력이에요. 어떤 버튼을 누르는지 아는 게 아니라, 다양한 상황에서 AI를 잘 쓰는 **판단력**을 기르는 거예요.

**4D Framework**는 Rick Dakan 교수(Ringling College of Art and Design)와 Joseph Feller 교수(University College Cork)의 연구 협업에서 나왔고, AI와 효과적으로 협업하기 위한 네 가지 핵심 역량을 제시해요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🤝</span><span class="oa-card__title">Delegation</span>무엇을 사람이 하고, 무엇을 AI가 하며, 어떻게 나눌지 <b>결정</b>하기</div>
  <div class="oa-card"><span class="oa-card__icon">🗣️</span><span class="oa-card__title">Description</span>원하는 결과, 과정, 행동 방식을 AI에게 명확히 <b>설명</b>하기</div>
  <div class="oa-card"><span class="oa-card__icon">🔍</span><span class="oa-card__title">Discernment</span>AI의 출력, 과정, 행동을 비판적으로 <b>평가</b>하기</div>
  <div class="oa-card"><span class="oa-card__icon">🧾</span><span class="oa-card__title">Diligence</span>AI를 <b>책임 있고 윤리적으로</b> 사용하기</div>
</div>

| 역량 | 포함되는 것 |
|---|---|
| **Delegation** | 내 목표와 AI의 능력을 이해하고, 협업 방식을 전략적으로 선택하기 |
| **Description** | 출력물을 명확히 정의하고, AI의 과정을 안내하고, 원하는 행동과 상호작용을 지정하기 |
| **Discernment** | 품질 · 정확성 · 적절성을 평가하고, 개선할 부분 찾기 |
| **Diligence** | AI system과 상호작용을 신중히 선택하고, 투명성을 유지하고, AI가 도운 작업에 책임지기 |

### 지금까지 배운 것과 4D의 연결

이미 이 역량들을 연습해 왔어요.

| 내가 한 일 | 해당 역량 |
|---|---|
| 역할과 목표를 알려 주는 prompt framework (무대 설정 · 작업 정의 · 규칙 지정) | **Description** |
| 결과를 검토하고 고치는 것 | **Discernment** |
| 고위험 작업에서 사실을 검증하는 것 | **Diligence** |
| 어떤 일을 Claude에게 맡길지 정하는 것 | **Delegation** |

> Claude를 잘 쓰는 건 prompt 기술만의 문제가 아니에요. **무엇을 맡기고, 어떻게 설명하고, 어떻게 검토하고, 어떤 책임을 질지**를 함께 판단하는 일이에요.

네 역량을 더 깊이 배우고 싶다면 무료 `AI Fluency` 강의가 실습과 실제 적용 사례로 다뤄요.

## 4. 내 업무에 Claude가 잘 맞는지 평가하기

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍🍳 쉽게 말하면</span>
  <p>새 직원에게 바로 중요한 일을 통째로 맡기지 않아요. <b>내가 이미 해 본 일</b>을 몇 개 시켜 보고, 내 결과와 비교해서 어디까지 믿고 맡길지 정하죠. eval이 바로 그 수습 기간 과제예요.</p>
</div>

Claude를 업무에 더 많이 쓰다 보면 이런 질문이 생겨요.

> Claude가 이 특정 작업을 정말 잘할까?

이때 **Discernment**가 중요해지고, 그 도구가 **eval**(evaluation의 줄임말)이에요. Evals는 내 업무에서 중요한 특정 작업을 Claude가 얼마나 잘 수행하는지 **체계적으로 확인하는 방법**이에요.

### 왜 eval이 중요한가요?

내 업무는 고유해요. Claude가 마케팅 copy 초안은 잘 써도, 내 분야의 기술 문서에는 더 많은 안내가 필요할 수 있어요. 간단한 eval을 돌리면:

- 내 workflow에서 Claude가 **가장 큰 가치를 주는 곳**을 알게 돼요.
- **맥락이나 예시를 더 줘야 하는 작업**을 찾을 수 있어요.
- 반복 작업에서 Claude의 결과물에 대한 **확신**이 쌓여요.

### 간단한 eval 방법

거창한 인프라는 필요 없어요.

<ol class="oa-flow">
  <li><strong>① 예시 모으기</strong>반복 작업 예시 5-10개</li>
  <li><strong>② 테스트 prompt 만들기</strong>실제 업무 맥락 포함</li>
  <li><strong>③ 결과 비교하기</strong>Claude 결과 vs 내 예시</li>
  <li><strong>④ 접근 방식 개선하기</strong>prompt · 예시 · 검토 단계 조정</li>
</ol>

| 단계 | 구체적으로 |
|---|---|
| **① 예시 모으기** | 내가 반복적으로 하는 작업의 예시를 5-10개 모아요: 직접 작성한 이메일, 기존 보고서, 내가 만든 분석 자료, 자주 쓰는 문서 양식 |
| **② 테스트 prompt 만들기** | 비슷한 결과물을 만들도록 prompt를 써요. 실제로 그 일을 할 때 내가 갖고 있을 맥락을 넣어요 |
| **③ 결과 비교하기** | 핵심 정보를 잘 담았나? 톤과 스타일이 적절한가? 빠졌거나 개선할 내용은? 사람이 반드시 검토해야 할 부분은 어디인가? 예시를 더 주면 좋아질까? |
| **④ 접근 방식 개선하기** | 더 많은 맥락 추가, 좋은 결과 예시 제공, 원하는 형식 명확히 지정, 사람이 검토해야 하는 단계 분리 |

### 예시 — 데이터 분석 eval

<ol class="oa-flow">
  <li><strong>Dataset 찾기</strong>내가 과거에 직접 분석한 것</li>
  <li><strong>Prompt 작성</strong>Claude에게 같은 분석 요청</li>
  <li><strong>비교</strong>Claude 결과 vs 내 기존 분석</li>
  <li><strong>Pattern 확인 · 개선</strong>약한 지점에 맞춰 prompt 수정</li>
</ol>

비교할 때는 pattern을 봐요. Claude가 **숫자는 맞추지만 전체 pattern을 놓치는지**, 혹은 trend는 잘 잡지만 세부 계산이 약한지 확인하고, 그에 맞춰 prompt를 고쳐요.

이런 가벼운 eval은 Claude를 **어디에 믿고 쓸 수 있는지**, **어디에서 review와 refinement에 집중해야 하는지** 감각을 길러 줘요.

### 실습 질문

- 위의 흔한 문제 중 이미 겪어 본 것은 무엇인가요? 다음번에는 어떤 방법을 써 볼 수 있을까요?
- 반복 업무 중 간단한 eval로 Claude의 적합성을 확인해 볼 작업은 무엇인가요?
- 4D Framework는 내가 Claude와 협업하는 방식을 어떻게 설명해 주나요?

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Claude가 내용은 맞게 썼는데 원하는 형식을 따르지 않았어요. 가장 효과적인 처방은?</summary>
  <p>① 같은 요청을 다시 보낸다 ② <b>형식 예시를 보여 주거나 구조를 구체적으로 설명한다</b> ③ 답변 길이를 줄여 달라고 한다</p>
  <p><b>정답: ②</b> — Claude는 무엇을 원하는지는 알지만 어떻게 보여 줄지 모르는 상태예요. 말로만 하지 말고 보여 주세요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 자신감 있게 들리는 답변에 틀린 사실이 섞여 있었어요. 다음부터 어떻게 해야 하나요?</summary>
  <p><b>정답: 중요한 사실은 독립적으로 검증해요.</b> 추가로 source/citation이나 confidence level을 요청하고, web search를 켜서 최신 정보에 근거하게 할 수 있어요. 4D 중 Diligence에 해당해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 짝 맞추기 — 4D 역량과 행동</summary>
  <p>"이 작업은 Claude에게, 최종 판단은 내가" → <b>Delegation</b><br>"독자, 톤, 형식을 prompt에 명확히 적기" → <b>Description</b><br>"초안의 정확성과 품질을 따져 보기" → <b>Discernment</b><br>"AI가 도운 결과물에 내가 책임지고 사실을 검증하기" → <b>Diligence</b></p>
</details>

<details class="oa-quiz">
  <summary>Q4. 간단한 eval의 첫 단계는 무엇인가요?</summary>
  <p>① 전용 평가 인프라 구축 ② <b>내가 반복적으로 하는 작업의 예시를 5-10개 모으기</b> ③ 가장 어려운 작업을 바로 Claude에게 맡기기</p>
  <p><b>정답: ②</b> — 내가 직접 한 결과물이 있어야 Claude의 결과와 비교할 기준이 생겨요. 거창한 인프라는 필요 없어요.</p>
</details>

## 정리

- 답변이 일반적이면 **맥락**을 추가하고, 길이와 형식은 **명확히 지정**해요. 톤은 평이한 말이나 예시로 설명해요.
- 중요한 사실은 **독립적으로 검증**해요. 그럴듯하지만 틀린 정보가 나올 수 있어요.
- 첫 prompt는 **대화의 시작점**이에요. 구체적으로 피드백하고, 너무 엇나가면 새 chat에서 다시 시작해요.
- AI Fluency는 **Delegation · Description · Discernment · Diligence**를 함께 기르는 능력이에요.
- 간단한 **eval**(예시 5-10개 → 테스트 prompt → 비교 → 개선)로 Claude가 내 업무에 얼마나 잘 맞는지 확인할 수 있어요.

다음 레슨에서는 Claude desktop app에서 일하는 세 가지 방식인 Chat, Cowork, Code를 살펴봐요.
