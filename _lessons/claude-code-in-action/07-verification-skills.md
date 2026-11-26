---
title: "Verification Skills"
title_ko: "검증 skill"
course: claude-code-in-action
lesson: 7
---

## 학습 목표

- 가장 먼저 만들 skill이 **검증 skill**인 이유를 설명할 수 있다.
- skill 폴더에 `SKILL.md` 외에 무엇을 담고 왜 그렇게 나누는지 말할 수 있다.
- 규칙을 CLAUDE.md, skill, hook 중 어디에 둘지 고를 수 있다.

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>변경 완료</strong>예: 리팩터링이 끝남</li>
  <li><strong>skill 발동</strong>description과 일치해 스스로 실행</li>
  <li><strong>테스트 실행</strong>전체 스위트</li>
  <li><strong>diff 읽기</strong>테스트가 약해지지 않았는지 확인</li>
  <li><strong>보고</strong>통과/실패와 근거</li>
</ol>

## 왜 검증 skill부터 만드는가

Claude의 작업을 확인하는 평소 방식을 떠올려 봅니다. 리팩터링을 시키고, 끝나면 테스트를 돌려 달라고 하거나 diff를 직접 읽습니다. 문제는 이 확인이 **내가 잊지 않고 요청하는 것**에 달려 있다는 점입니다. 한 번 건너뛰면 나쁜 코드가 그대로 들어갑니다.

검증 skill은 이 의존을 없앱니다. 변경이 skill의 description과 맞으면 요청 없이 실행되고, 매번 같은 단계를 밟습니다.

<div class="oa-eli">
  <span class="oa-eli__head">✈️ 쉽게 말하면</span>
  <p>조종사의 <b>이륙 전 체크리스트</b>와 같습니다. 베테랑도 기억에 의존하지 않고 매번 같은 목록을 순서대로 확인합니다. 검증 skill은 Claude가 "다 됐다"고 말하기 전에 자동으로 펼치는 체크리스트입니다.</p>
</div>

마지막 확인 단계가 중요합니다. 테스트가 초록색이라는 것만으로는 부족합니다. 테스트가 **무엇이든 통과하도록 슬쩍 느슨해졌을 수** 있기 때문입니다.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ "완료"가 아닌 것</span>
    <ul>
      <li>diff를 읽어 보니 코드가 맞아 보인다</li>
      <li>테스트가 통과했다(테스트를 고쳤는지는 안 봄)</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ "완료"인 것</span>
    <ul>
      <li>검증 관문을 실제로 실행하고 관찰했다</li>
      <li>결과를 근거와 함께 명시했다</li>
    </ul>
  </div>
</div>

같은 형태가 팀이 반복하는 모든 절차에 쓰입니다. 릴리스 체크리스트, 마이그레이션 절차, PR 전 점검이 그렇습니다. 기준은 간단합니다. **같은 여러 단계 지시를 두 번 입력했다면 그것은 skill입니다.**

## skill 폴더에 담는 것

```text
.claude/skills/verify-change/
├── SKILL.md        # 무엇을 할지 (짧게)
├── reference.md    # 자세한 자료 (필요할 때만 읽음)
└── check.sh        # 모든 관문을 실행하는 스크립트
```

| 파일 | 역할 | context 비용 |
|---|---|---|
| `SKILL.md` | 이름, 발동 조건(description), 절차 | description만 상시 로드 |
| `reference.md` | 긴 설명. `SKILL.md`에서 링크 | 필요할 때만 읽음 |
| 스크립트 | 관문 실행 도구 | 내용을 context에 싣지 않고 **실행** |

`SKILL.md`는 간결하게 두고, 긴 설명과 실행 스크립트는 옆 파일로 밀어냅니다.

## 어떤 규칙을 어디에 두나

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📌</span><span class="oa-card__title">CLAUDE.md</span>항상 적용되는 컨벤션. 이름 규칙, 파일 위치</div>
  <div class="oa-card"><span class="oa-card__icon">📒</span><span class="oa-card__title">Skill</span>특정 종류의 작업에 묶인 절차와 참고 자료</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⛓️</span><span class="oa-card__title">Hook</span>Claude가 건너뛸 수 없어야 하는 규칙</div>
</div>

CLAUDE.md와 skill은 둘 다 Claude가 **따르는 지시**이고, hook은 **실제로 실행되는 코드**입니다. 건너뛰는 것이 용납되지 않는 규칙은 지시 준수에 맡기지 않습니다.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. 검증 skill이 테스트를 실행한 뒤 diff까지 읽는 이유는 무엇입니까?</summary>
  <p><b>정답: 테스트가 통과하도록 약해지지 않았는지 확인하기 위해서입니다.</b> 초록색 결과만으로는 테스트가 느슨해졌는지 알 수 없습니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. skill 폴더의 스크립트는 context를 얼마나 차지합니까?</summary>
  <p><b>정답: 내용은 차지하지 않습니다.</b> Claude는 스크립트를 context에 싣지 않고 실행합니다. 그래서 skill이 자체 도구를 가질 수 있습니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. "파일 이름은 kebab-case로 짓는다"와 "릴리스 전 점검 6단계"는 각각 어디에 둡니까?</summary>
  <p><b>정답: 앞은 CLAUDE.md, 뒤는 skill.</b> 항상 적용되는 컨벤션은 CLAUDE.md, 특정 작업에 묶인 절차는 skill입니다.</p>
</details>

## 핵심 정리

- 검증 skill은 "확인을 잊지 않고 요청하기"에 대한 의존을 없앱니다.
- 완료는 관문을 실행하고 관찰한 결과로 판단합니다. 테스트가 약해졌는지도 봅니다.
- `SKILL.md`는 짧게, 깊은 자료와 스크립트는 옆 파일에 둡니다.
- 프로젝트의 `.claude/skills`에 커밋하면 팀 전체가 같은 검증을 물려받습니다.
