---
title: "Validating skills for plugins"
title_ko: "plugin을 위한 skill 검증하기"
course: introduction-to-claude-cowork
lesson: 12
---

## 학습 목표

- **eval**이 무엇이고 skill을 공유 · 의존하기 전에 왜 중요한지 설명하기
- **skill-creator**로 가벼운 eval 돌리기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>현실적 프롬프트</strong>skill-creator가 둘 이상 만들어요</li>
  <li><strong>한 쌍의 출력</strong>skill 사용 vs skill 없이</li>
  <li><strong>나란히 검토</strong>어느 쪽을 실제로 쓸까?</li>
  <li><strong>평이한 말로 피드백</strong>무엇이 빠졌는지 구체적으로</li>
  <li><strong>수정 · 재실행</strong>같은 프롬프트로 다시 확인</li>
</ol>

이 레슨을 한 문장으로 줄이면: **eval은 코드 없는 시험 주행이에요. 남이 걸려 넘어지기 전에 내가 먼저 넘어져 봐요.**

## 1. 왜 중요한가요?

<div class="oa-eli">
  <span class="oa-eli__head">🍲 쉽게 말하면</span>
  <p>내 레시피로 내가 요리하면 <b>"소금 약간"</b>이 얼마인지 손이 알아요. 하지만 친구에게 레시피를 주면 친구는 그 "약간"을 몰라요. 그래서 건네기 전에 <b>레시피만 보고 한번 만들어 보는 시식</b>이 필요해요.</p>
</div>

skill을 만들거나 plugin으로 묶으면, 사실상 **다른 사람이 쓸 작은 제품**을 만드는 거예요. 동료에게 건네는 템플릿 · 스프레드시트 모델 · 체크리스트처럼, 책상을 떠나기 전에 시험 주행할 가치가 있어요.

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🙋 만든 내가 쓸 때</span>
    <ul>
      <li>문제를 우회하는 법을 알아요</li>
      <li>무엇을 물을지 알아요</li>
      <li>어떤 파일을 줄지 알아요</li>
      <li>답이 어때야 하는지 알아요</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🤷 팀원이 쓸 때</span>
    <ul>
      <li>그런 게 하나도 없어요</li>
      <li>요청을 조금 다르게 표현해요</li>
      <li>조금 다른 입력을 줘요</li>
      <li><b>엣지 케이스</b>를 만나요</li>
    </ul>
  </div>
</div>

**엣지 케이스**는 흔치 않지만 실제로 일어나는 상황이에요. 예를 들어 skill이 설계된 범위에서 살짝 벗어난 요청이요. skill이 걸려 넘어지는 곳이 거기이고, 쓰는 사람은 왜인지 몰라요.

**eval(evaluation, 평가)**은 그 걸림을 남이 발견하기 전에 잡는 방법이에요. 단어에 겁먹지 마세요. eval은 그냥 시험 주행(try-out)이에요.

<ol class="oa-flow">
  <li><strong>넣기</strong>현실적인 요청을 넣어요</li>
  <li><strong>보기</strong>나온 것을 봐요</li>
  <li><strong>말하기</strong>Claude에게 고칠 점을 말해요</li>
</ol>

코드도 테스트 스크립트도 없어요. **"이걸 내 이름 걸고 내보낼 만큼 좋은가"**라는 판단만 있으면 돼요.

## 2. eval 시스템 동작 방식

<div class="oa-eli">
  <span class="oa-eli__head">👓 쉽게 말하면</span>
  <p>안경점에서 <b>"1번이 잘 보이세요, 2번이 잘 보이세요?"</b> 하며 렌즈를 번갈아 대 보는 것과 같아요. 하나만 보면 좋은지 모르지만, 나란히 놓으면 차이가 보여요.</p>
</div>

**skill-creator**(Claude의 내장 skill 제작 헬퍼)로 skill을 만들면 eval을 과정의 일부로 안내해요.

skill-creator가 누군가 쓸 법한 **현실적 프롬프트를 둘 이상** 만들고, 각 프롬프트에 대해 **한 쌍의 출력**을 내요.

| 출력 | 무엇인가 | 왜 있나 |
|---|---|---|
| **With skill** | Claude가 내 skill을 쓴 것 | 내 skill의 실제 결과 |
| **Without skill** | 같은 프롬프트를 skill 없이 답한 것 | **비교 기준(baseline).** "이 출력이 괜찮은가"가 아니라 "Claude 혼자 했을 때보다 **나은가**"를 나란히 보게 해 줘요 |

### 검토할 때 답하는 두 가지 질문

각 쌍을 검토하고, review 페이지에서 바로 **평이한 말로 피드백**을 줘요.

| 질문 | 답이 이렇다면 |
|---|---|
| **skill 버전이 내가 쓸 것인가?** | 그렇다면 좋아요. 무엇이 더 나았는지 메모해서 skill이 계속 그렇게 하게 해요 |
| **아니라면 무엇이 빠졌거나 어긋났나?** | 구체적으로 말해요 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">😐 Claude가 손댈 수 없는 피드백</span>
    <ul>
      <li>"이건 좀 아닌 것 같아."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">💪 Claude가 손댈 수 있는 피드백</span>
    <ul>
      <li>"톤이 너무 격식적이야."</li>
      <li>"executive summary를 건너뛰었어."</li>
    </ul>
  </div>
</div>

피드백을 제출하면 Claude가 그에 따라 skill을 수정해요.

### 예시 — meeting-recap skill의 eval (강의의 연습)

프롬프트는 이래요: "오늘 product sync의 recap을 써줘. 노트는 `notes/2026-05-01-product-sync.md`에 있어." 팀의 house rule 네 가지로 두 출력을 비교해요.

| House rule | With skill | Without skill |
|---|---|---|
| 결정 사항부터 제시 | ✅ | ❌ |
| 모든 action item에 담당자와 날짜 | ✅ | ❌ |
| 미결 질문(open questions)을 따로 표시 | ✅ | ❌ |
| 150단어 이내 | ✅ | ✅ |
| **합계** | **4/4** | **1/4** |

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ With skill</span>
    <ul>
      <li><b>Decisions</b> · <b>Action items</b> · <b>Open questions</b>로 구획이 나뉘어요</li>
      <li>"Maya — 금요일 5월 9일까지 rollout plan 확정"처럼 담당자와 날짜가 붙어요</li>
    </ul>
  </div>
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ Without skill</span>
    <ul>
      <li>"생산적인 논의를 했다"로 시작하는 줄글이에요</li>
      <li>"Devon이 instrumentation을 살펴보겠다고 했다"처럼 날짜가 없어요</li>
      <li>미결 질문이 문단 속에 묻혀요</li>
    </ul>
  </div>
</div>

각 쌍마다 **실제로 보낼 버전을 고르고**, Claude에게 줄 **피드백을 한 줄** 써요. 그게 loop의 전부예요.

## 3. skill 반복하기

<div class="oa-eli">
  <span class="oa-eli__head">🧂 쉽게 말하면</span>
  <p>찌개 간을 맞출 때 소금, 설탕, 고춧가루를 <b>한꺼번에 넣으면 뭐가 맛을 바꿨는지 몰라요.</b> 하나 넣고 맛보고, 또 하나 넣고 맛봐요.</p>
</div>

**피드백이 곧 수정이에요.** 제출하면 Claude가 skill을 업데이트해요(지침 재작성, 예시 조정, 요구 사항 다듬기). 그다음 같은 프롬프트를 다시 돌려 변화가 붙었는지 봐요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">1️⃣</span><span class="oa-card__title">한 번에 하나씩 바꾸기</span>장황하고 섹션도 빠졌다면, 더 중요한 하나를 고쳐 재실행한 뒤 다음을 봐요. 무엇이 실제로 바늘을 움직였는지 알 수 있어요</div>
  <div class="oa-card"><span class="oa-card__icon">🔁</span><span class="oa-card__title">관문이 아니라 loop</span>수정 후에도 마음에 안 들면 다시 돌려요. 대부분의 skill은 한두 라운드면 준비돼요</div>
  <div class="oa-card"><span class="oa-card__icon">🏁</span><span class="oa-card__title">첫 패스에 훌륭하면 끝</span>eval은 통과 의례가 아니라 확신이 필요할 때를 위한 것이에요</div>
</div>

### 출시 기준

나 자신에게든 팀원에게든, skill을 내보내는 기준은 **완벽한 eval이 아니에요.**

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 기준이 아닌 것</span>
    <ul>
      <li>모든 케이스에서 완벽한 eval</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 실제 기준</span>
    <ul>
      <li>내가 중요히 여기는 케이스가 <b>baseline보다 뚜렷이 나아요</b></li>
      <li>아직 못 다루는 케이스를 <b>스스로 명명</b>했어요</li>
    </ul>
  </div>
</div>

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. eval을 돌리려면 코드나 테스트 스크립트를 써야 하나요?</summary>
  <p><b>정답: 아니요.</b> eval은 시험 주행이에요. 현실적인 요청을 넣고, 나온 것을 보고, 고칠 점을 평이한 말로 말하면 돼요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. skill-creator는 왜 skill 없이 답한 출력도 함께 보여 주나요?</summary>
  <p><b>정답: 비교 기준이기 때문이에요.</b> 출력이 괜찮은지가 아니라, Claude가 혼자 했을 때보다 내 skill이 실제로 더 낫게 만드는지를 나란히 볼 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 첫 라운드에서 출력이 너무 장황하고 섹션 하나도 빠져 있었어요. 어떻게 고치나요?</summary>
  <p><b>정답: 더 중요한 하나를 먼저 고치고 재실행한 뒤 다음을 봐요.</b> 한 번에 하나씩 바꿔야 무엇이 실제로 효과가 있었는지 알 수 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. skill을 팀에 내보내도 되는 기준은 무엇인가요?</summary>
  <p><b>정답: 내가 중요히 여기는 케이스가 baseline보다 뚜렷이 낫고, 아직 못 다루는 케이스를 스스로 명명한 것.</b> 완벽한 eval이 기준은 아니에요.</p>
</details>

## 정리

- skill이나 plugin은 **남이 쓸 작은 제품**이에요. 팀원은 내가 아는 우회법을 모르고, 엣지 케이스에서 걸려 넘어져요.
- eval은 **코드 없는 시험 주행**이에요: 현실적 요청을 넣고, 결과를 보고, 고칠 점을 말해요.
- skill-creator가 **skill 버전과 없는 버전을 나란히** 보여 줘서, 내 skill이 만드는 실제 차이를 판단하게 해요.
- 피드백은 **구체적으로**, 변경은 **한 번에 하나씩** 해요.
- **피드백 → 수정 → 재실행** loop를 한두 번 돌리면 대부분 공유 준비가 돼요. 첫 패스에 이미 좋으면 거기서 끝이에요.

다음 레슨에서는 "나에게 통한다"에서 "팀에게 통한다"로 넘어가요.
