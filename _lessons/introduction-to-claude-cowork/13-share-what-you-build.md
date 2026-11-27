---
title: "Share what you build with your team"
title_ko: "만든 것을 팀과 공유하기"
course: introduction-to-claude-cowork
lesson: 13
---

## 학습 목표

- plugin이 **Enterprise 조직**에 어떻게 배포되는지 설명하기
- 공유 plugin을 시간이 지나도 건강하게 유지하는 **습관** 적용하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>자리를 얻은 skill</strong>eval을 거쳐 여러 사람에게 통해요</li>
  <li><strong>plugin으로 묶기</strong>Lesson 8에서 배운 대로</li>
  <li><strong>marketplace 소유자에게 핸드오프</strong>팀 리드 · enablement · IT</li>
  <li><strong>게시 · 도달 방식 선택</strong>Available부터 Required까지</li>
  <li><strong>유지</strong>소유자 · eval · 이름 · 검토 주기</li>
</ol>

이 레슨을 한 문장으로 줄이면: **노트북 한 대에서 시작한 skill이, 팀원이 설치해서 돌릴 수 있는 것이 돼요.**

## 1. 팀 전체로 워크플로 확장

<div class="oa-eli">
  <span class="oa-eli__head">🍳 쉽게 말하면</span>
  <p>우리 집에서만 해 먹던 레시피가 <b>여러 사람 입맛에도 통한다는 게 확인되면</b>, 이제 사내 식당 메뉴판에 올릴 차례예요.</p>
</div>

이 시점이면 팀에는 **자리를 얻은 skill**이 몇 개 있어요.

<ol class="oa-flow">
  <li><strong>개인 방식에서 시작</strong>누군가의 일하는 방식이었어요</li>
  <li><strong>eval 통과</strong>시험 주행을 거쳤어요</li>
  <li><strong>여러 사람에게 통함</strong>한 사람 이상의 사용 사례에서 버텨요</li>
</ol>

이를 팀 전체로 효율적으로 확장하려면 **plugin으로 묶어요**(Lesson 8). 이 레슨은 그다음 단계, 필요한 모두에게 그 plugin을 전달하는 것이에요.

## 2. 조직에서 plugin 배포

<div class="oa-eli">
  <span class="oa-eli__head">🏪 쉽게 말하면</span>
  <p>회사의 <b>사내 앱 스토어</b>를 떠올려 보세요. 내가 직접 진열대에 올리는 게 아니라, <b>스토어 관리자에게 가져가면</b> 관리자가 올려 줘요. 올릴 때 "원하는 사람만 받기"인지 "전 직원 필수"인지도 관리자가 정해요.</p>
</div>

큰 회사에서 권장되는 방식은 **조직의 private marketplace**를 통하는 거예요. admin이 관리하는, 회사 승인 plugin 카탈로그예요.

실제로 배포는 **핸드오프**예요. marketplace를 소유한 사람(팀 리드, enablement나 운영 담당, IT)에게 plugin을 가져가면 그들이 게시해요. 게시할 때 다른 모두에게 **어떻게 도달할지**를 골라요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📚</span><span class="oa-card__title">Available</span>회사 Directory에 나타나고 원하는 사람이 설치해요. 대부분의 팀 plugin에 맞아요</div>
  <div class="oa-card"><span class="oa-card__icon">📥</span><span class="oa-card__title">Installed by default</span>모두에게 이미 설치돼 있고, 끌 수 있어요</div>
  <div class="oa-card"><span class="oa-card__icon">📌</span><span class="oa-card__title">Required</span>설치돼 있고 계속 켜져 있어요. 모두가 같게 실행해야 하는 compliance 점검 등에 써요</div>
  <div class="oa-card"><span class="oa-card__icon">🙈</span><span class="oa-card__title">Hidden</span>marketplace에 있지만 Directory에 안 보여요. 스테이징이나 제한된 롤아웃용이에요</div>
</div>

| 도달 방식 | Directory에 보이나 | 처음부터 설치돼 있나 | 끌 수 있나 |
|---|---|---|---|
| **Available** | 네 | 아니요 (원하면 설치) | 네 |
| **Installed by default** | 네 | 네 | 네 |
| **Required** | 네 | 네 | 아니요 |
| **Hidden** | 아니요 | — | — |

현재 강의 본문은 Available · Installed by default · Required 세 가지를 설명하고, Hidden은 interactive의 install level 목록에 함께 나와요.

### 팀원 입장에서는

| 항목 | 내용 |
|---|---|
| **어디에 보이나** | Directory에, 공개 Anthropic plugin과 나란히, **우리 회사에서 온 것**으로 라벨링돼 나타나요 |
| **할 수 있는 것** | 쓰고, 끌 수 있어요 (required 제외) |
| **할 수 없는 것** | 편집은 못 해요 |
| **업데이트** | 유지 관리자에게서 자동으로 흘러와요 |

### 내 조직은 어떤 상황인가요?

내가 보게 되는 것은 admin이 무엇을 설정해 뒀는지에 따라 달라요. marketplace가 돌아가고 넘길 소유자가 분명한 조직도 있고, 아직 켜지 않은 조직도 있어요.

| 상황 | 경로 | 다음 한 걸음 |
|---|---|---|
| **marketplace가 있고, 소유자를 알아요** | Hand-off. 길은 이미 닦여 있어요. 내 일은 설정이 아니라 넘기는 것 | plugin을 marketplace 소유자에게 가져가요. **누구를 위한 것인지, 어떻게 도달해야 하는지** 말해 줘요. 그들이 게시할 때 install level을 골라요 |
| **있는지 잘 모르겠어요** | Discovery | 조직에 marketplace가 있는지, 누가 소유하는지부터 알아봐요 |
| **내가 admin이에요** | Setup | marketplace를 설정하는 쪽이에요 |

(강의 화면에서는 첫 번째 탭의 상세 내용만 확인했어요. 나머지 두 탭의 "다음 한 걸음"은 탭 이름과 본문 설명에서 옮긴 요약이에요.)

어느 경로든 목표는 같아요: **노트북 한 대에서 시작한 skill이, 팀원이 설치해서 돌릴 수 있는 것이 되는 것.**

## 3. 유지 습관

<div class="oa-eli">
  <span class="oa-eli__head">🧊 쉽게 말하면</span>
  <p>공용 냉장고는 <b>담당자가 없으면</b> 금세 유통기한 지난 음식으로 차요. 이름표를 붙이고, 담당을 정하고, 정기적으로 비워야 계속 쓸 만해요.</p>
</div>

공유 plugin이 **조용히 낡지 않게** 하는 관행이에요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🧑‍✈️</span><span class="oa-card__title">소유자 한 명</span>모든 공유 plugin에는 지정된 사람이 있어야 해요. 변경을 검토하고, 수정 후 eval을 돌리고, 업데이트 · 폐기를 정해요</div>
  <div class="oa-card"><span class="oa-card__icon">🚦</span><span class="oa-card__title">게시 전 항상 eval</span>eval loop를 관문으로 삼아요. 중요한 케이스가 변경 후 버티지 못하면 모두에게 push하지 않아요</div>
  <div class="oa-card"><span class="oa-card__icon">🏷️</span><span class="oa-card__title">이름을 구체적으로</span>skill과 plugin 이름이 조직 안에서 충돌하지 않게 해요</div>
  <div class="oa-card"><span class="oa-card__icon">🗓️</span><span class="oa-card__title">검토 주기 설정</span>분기별 정도가 합리적 시작점이에요. 무엇이 설치됐고, 실제로 쓰이고, 낡았는지 봐요. 아무도 안 쓰는 것은 폐기하고, 개선 지점은 반영해요</div>
</div>

### 이름 짓기 예시

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 충돌하기 쉬운 이름</span>
    <ul>
      <li><code>meeting-prep</code></li>
      <li>조직의 plugin directory 곳곳에 있는 다른 meeting-prep skill 셋과 충돌할 수 있어요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 구체적인 이름</span>
    <ul>
      <li><code>sales-customer-renewal-prep</code></li>
      <li>충돌하지 않아요</li>
    </ul>
  </div>
</div>

### 생각해 보기

나와 가까운 팀원들이 오늘 Cowork에서 가장 의존하는 skill을 떠올려 보세요.

- 그중 팀 plugin으로 가장 먼저 묶을 두세 개는 무엇인가요?
- 그 plugin을 가져갈 사람, marketplace에 올려 줄 수 있는 사람은 조직에서 누구인가요?

두 번째 질문의 답을 아직 모른다면, **그걸 알아내는 것이 진짜 다음 단계**예요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 큰 회사에서 plugin을 공유하는 권장 방식은 무엇인가요?</summary>
  <p><b>정답: 조직의 private marketplace.</b> admin이 관리하는 회사 승인 plugin 카탈로그예요. 내가 직접 게시하는 게 아니라 marketplace 소유자에게 핸드오프해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 모든 직원이 똑같이 실행해야 하는 compliance 점검 plugin에는 어떤 도달 방식이 맞나요?</summary>
  <p><b>정답: Required.</b> 설치돼 있고 계속 켜져 있어서 끌 수 없어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 팀원이 회사에서 배포한 plugin을 직접 편집할 수 있나요?</summary>
  <p><b>정답: 아니요.</b> 쓰고 끌 수는 있지만(required 제외) 편집은 못 해요. 업데이트는 유지 관리자에게서 흘러와요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. skill 이름을 <code>meeting-prep</code>으로 지으면 왜 문제가 될 수 있나요?</summary>
  <p><b>정답: 조직 안의 다른 meeting-prep skill과 충돌할 수 있어요.</b> <code>sales-customer-renewal-prep</code>처럼 구체적으로 지어요.</p>
</details>

## 정리

- 자리를 얻은 skill을 **plugin으로 묶고**, 조직 **private marketplace** 소유자에게 **핸드오프**해 배포해요.
- 게시자는 **Available / Installed by default / Required / Hidden** 중 도달 방식을 골라요.
- 팀원에게는 회사에서 온 것으로 라벨링돼 Directory에 보이고, 쓰고 끌 수 있지만 **편집은 못 해요.**
- **소유자 한 명, 게시 전 eval, 구체적 이름, 정기 검토**, 이 습관이 공유 plugin을 건강하게 유지해요.
- marketplace에 올려 줄 사람이 누구인지 모른다면, 그걸 찾는 게 다음 단계예요.

마지막 레슨에서는 지금까지의 흐름을 짧게 되짚고, 계속 이어 갈 다음 동작을 골라요.
