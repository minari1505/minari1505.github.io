---
title: "Skills: Teach Claude Cowork your way"
title_ko: "Skills: Cowork에 내 방식 가르치기"
course: introduction-to-claude-cowork
lesson: 7
---

## 학습 목표

- **skill**을 정의하고 Claude가 어떻게 사용하는지 설명하기
- skill이 포함할 수 있는 **네 가지 building block** 알기
- 내 반복 프로세스로 skill 하나 만들기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>반복 프로세스 고르기</strong>매번 다시 설명하기 지겨운 일</li>
  <li><strong>Claude와 함께 만들기</strong>질문에 실제 예시로 답해요</li>
  <li><strong>skill 폴더 설치</strong>SKILL.md + 필요한 파일</li>
  <li><strong>자동으로 사용</strong>맞는 작업이 오면 Claude가 로드</li>
  <li><strong>교정으로 업데이트</strong>말로 고쳐 달라고 하면 돼요</li>
</ol>

이 레슨을 한 문장으로 줄이면: **skill은 "이 일은 이렇게 해"를 담은 폴더이고, 맞는 순간에 Claude가 알아서 꺼내 써요.**

## 1. skill이란?

<div class="oa-eli">
  <span class="oa-eli__head">📖 쉽게 말하면</span>
  <p>할머니의 <b>김치찌개 레시피 카드</b>를 생각해 보세요. 카드가 있으면 누가 끓여도 그 맛이 나요. 그리고 좋은 주방 보조는 "김치찌개 해 줘"라는 말만 들어도 <b>알아서 그 카드를 꺼내요.</b></p>
</div>

**Skill**은 재사용 가능한 플레이북, 즉 파일과 리소스가 든 **폴더**예요. 특정 종류의 일을 내가 원하는 방식대로 하도록 Claude에게 가르쳐요. 그 skill에 맞는 작업을 시작하면 Claude가 플레이북을 로드해 따라요.

| 사용 방식 | 어떻게 |
|---|---|
| **자동 (기본)** | 이름으로 부를 필요가 없어요. Claude가 작업이 설치된 skill과 맞는다고 판단하면 자동으로 로드해요. 필요한 바로 그 순간에 써요 |
| **명시적** | 원하면 직접 불러요: "board memo 작성 skill 써줘" |

## 2. skill 안의 네 가지

<div class="oa-eli">
  <span class="oa-eli__head">🧰 쉽게 말하면</span>
  <p>신입에게 일을 넘길 때 주는 것들이에요. <b>업무 설명서</b>(instructions), <b>회사 로고와 양식</b>(assets), <b>"이 정도로 써 주세요" 하는 잘된 샘플</b>(references), 그리고 <b>매번 똑같이 돌려야 하는 계산기</b>(scripts).</p>
</div>

skill은 긴 지시문 하나 이상이에요. 아래 네 종류의 파일이 함께 작동해서, 실제 프로세스를 Claude가 팀처럼 실행할 수 있을 만큼 담아내요.

<div class="oa-cards">
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">📝</span><span class="oa-card__title">Instructions (SKILL.md)</span>skill이 무엇을 하고, 언제 쓰며, 어떻게 하는지 알려 주는 브리프. 새 동료용 런북처럼, 실제로 일할 수 있을 만큼 구체적으로</div>
  <div class="oa-card"><span class="oa-card__icon">🎨</span><span class="oa-card__title">Assets</span>로고, 브랜드 템플릿, 슬라이드 마스터, 폰트. 진짜 같은 출력을 만드는 원자재</div>
  <div class="oa-card"><span class="oa-card__icon">⭐</span><span class="oa-card__title">References</span>좋은 출력 예시, 스타일 가이드, 조항 라이브러리, 새 팀원에게 기준으로 건넬 과거 작업. Claude가 이 일에서 "좋다"가 무엇인지 배우는 방법</div>
  <div class="oa-card"><span class="oa-card__icon">⚙️</span><span class="oa-card__title">Scripts</span>매번 같게 일어나야 하는 부분을 위한 작은 코드: 분산 계산, 구조화 비교, 차트 포매터, 문서 리포매터</div>
</div>

한 줄로 정리하면 이래요.

| 파일 | 역할 |
|---|---|
| **Instructions** | 무엇을 할지 **알려 줘요** |
| **Assets** | **원자재**를 줘요 |
| **References** | 좋은 결과가 어떤 모습인지 **보여 줘요** |
| **Scripts** | 반복되는 부분을 **매번 같은 방식으로** 돌려요 |

### 필요한 것만 넣어요

skill은 이 중 어떤 조합이든 될 수 있어요.

| 조합 | 예 |
|---|---|
| **SKILL.md 하나뿐** | 어떤 프로세스에는 이것으로 충분해요 |
| **SKILL.md + 브랜드 asset 폴더** | 정해진 양식으로 나와야 하는 결과물 |
| **넷 다** | 지침, 원자재, 기준, 계산이 모두 필요한 프로세스 |

> 조합은 일을 따라가요. **필요한 것만 넣고 그 이상은 넣지 않는 것**이 원칙이에요.

강의는 세 가지 예시 skill 폴더(`meeting-recap`, `board-memo`, `variance-analysis`)를 보여 주면서, 프로세스에 따라 폴더 안에 들어가는 것이 달라진다는 점을 짚어요.

이것이 skill이 팀의 일하는 방식을 codify하는 데 유용한 이유예요. Cowork는 나를 대신해 행동할 수 있는 coworker이고, skill은 그 일을 **해야 하는 방식대로** 하게 만드는 방법이에요.

## 3. Claude로 skill 만들기

<div class="oa-eli">
  <span class="oa-eli__head">🎤 쉽게 말하면</span>
  <p>매뉴얼을 혼자 끙끙대며 쓰는 대신, <b>인터뷰어에게 내 방식을 말로 설명하면 인터뷰어가 매뉴얼로 정리해 주는 것</b>과 같아요.</p>
</div>

가장 빠른 방법은 Claude와 함께 만드는 거예요. Cowork에서 새 대화를 열고 이렇게 말해요.

```text
[매번 다시 설명하기 지겨운 반복 프로세스]를 위한 skill을 만들고 싶어. 뭐가 필요한지 안내해줘.
```

Claude가 몇 가지를 물어요.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">🎯</span><span class="oa-card__title">무엇을 하나</span>skill이 무엇을 해야 하는지</div>
  <div class="oa-card"><span class="oa-card__icon">⏱️</span><span class="oa-card__title">언제 발동하나</span>어떤 작업에서 trigger돼야 하는지</div>
  <div class="oa-card"><span class="oa-card__icon">🏆</span><span class="oa-card__title">좋은 출력</span>좋은 결과가 어떤 모습인지</div>
  <div class="oa-card"><span class="oa-card__icon">📚</span><span class="oa-card__title">어떤 리소스</span>skill을 만드는 데 참고할 자료</div>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">😐 뭉뚱그린 답</span>
    <ul>
      <li>"그냥 보기 좋게 정리해 주면 돼."</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">💪 구체적인 답</span>
    <ul>
      <li>실제 작업 예시를 가리켜요</li>
      <li>실제 템플릿을 가리켜요</li>
      <li>실제 과거 출력을 가리켜요</li>
    </ul>
  </div>
</div>

결과는 SKILL.md와 필요한 asset · reference · script가 든, **설치 준비된 skill 폴더**예요.

### 설치한 뒤

| 하고 싶은 것 | 방법 |
|---|---|
| **skill 찾기** | 설치 후 **Customize**에서 찾을 수 있어요 |
| **skill 고치기** | 교정을 주고 업데이트를 요청하면 돼요. Claude가 **제자리에서** skill을 업데이트해요 |

수정 요청은 이런 식이에요.

```text
10만 달러 넘고 두 단계 미끄러진 딜을 표시하는 단계 추가해줘. 그건 항상 중요해.
```

### 어디서든 같은 방식으로 작동해요

skill은 프로젝트 안 대화를 포함해 **어떤 대화에서든** 같은 방식으로 작동해요. variance analysis용으로 만든 skill은 기본 Cowork 세션에서든 특정 finance project 안에서든, variance analysis가 작업일 때마다 나타나요.

### 생각해 보기

반복하는 프로세스 하나를 떠올려 보세요. 돌리는 보고서, 항상 쓰는 형식, 따르는 체크리스트. 적어 두세요. 그것이 첫 skill 후보예요. 지금 만들 필요는 없고, 시간이 날 때 Claude와 함께 만들면 돼요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 설치한 skill을 쓰려면 매번 이름으로 불러야 하나요?</summary>
  <p><b>정답: 아니요.</b> Claude가 작업이 설치된 skill과 맞는다고 판단하면 자동으로 로드해요. 원하면 "board memo 작성 skill 써줘"처럼 명시적으로 부를 수도 있어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. skill에 들어갈 수 있는 네 가지 파일 종류는 무엇인가요?</summary>
  <p><b>정답: Instructions(SKILL.md) · Assets · References · Scripts.</b> 순서대로 무엇을 할지, 원자재, "좋음"의 기준, 매번 같게 돌려야 하는 부분을 담아요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. SKILL.md 파일 하나만 있는 skill은 불완전한 건가요?</summary>
  <p><b>정답: 아니요.</b> 어떤 프로세스에는 그것으로 충분해요. 필요한 것만 넣고 그 이상은 넣지 않는 게 원칙이에요.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. 이미 설치한 skill에 단계를 하나 추가하고 싶어요. 어떻게 하나요?</summary>
  <p><b>정답: Claude에게 교정을 말하고 skill을 업데이트해 달라고 해요.</b> Claude가 제자리에서 skill을 고쳐요.</p>
</details>

## 정리

- skill은 특정 종류의 일을 **내 방식대로** 하도록 가르치는, 파일이 든 재사용 폴더예요.
- **맞는 작업에서 자동으로** 사용되고, 원하면 이름으로 부를 수도 있어요.
- **instructions · assets · references · scripts**를 필요에 따라 조합해요. SKILL.md 하나만으로도 충분할 수 있어요.
- 가장 빠른 제작법은 **Claude와 대화하며 만드는 것**이에요. 실제 예시 · 템플릿 · 과거 출력을 가리키며 구체적으로 답해요.
- 설치한 skill은 **Customize**에 있고, 고칠 때는 교정을 말하면 Claude가 제자리에서 업데이트해요.

다음 레슨에서는 여러 skill과 커넥터를 하나의 일을 중심으로 묶어 설치할 수 있게 만든 plugin을 다뤄요.
