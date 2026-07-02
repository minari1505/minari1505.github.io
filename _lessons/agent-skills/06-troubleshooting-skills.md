---
title: "Troubleshooting skills"
title_ko: "트러블슈팅"
course: agent-skills
lesson: 6
---

## 학습 목표

- Skill이 트리거되지 않을 때 / 너무 자주 트리거될 때 진단 순서 익히기
- description 컨텍스트 예산과 잘림 문제 이해하기
- "트리거됐다 ≠ 잘 동작한다" — skill을 제대로 평가하는 방법 배우기

## Skill이 트리거되지 않을 때

순서대로 확인하세요.

1. description에 사용자가 **자연스럽게 말할 키워드**가 들어있는지 확인
2. `What skills are available?`라고 물어서 skill이 목록에 있는지 확인
3. description에 가깝게 **요청을 바꿔 말해보기**
4. user-invocable이라면 `/skill-name`으로 직접 호출해보기

> ⚠️ frontmatter YAML이 깨져 있으면 본문은 빈 메타데이터로 로드됩니다. `/skill-name` 직접 호출은 되는데 자동 매칭이 안 된다면 이 경우일 수 있어요. `claude --debug`로 파싱 에러를 확인하세요.

## Skill이 너무 자주 트리거될 때

1. description을 **더 구체적으로** 좁히기
2. 수동 호출만 원한다면 `disable-model-invocation: true` 추가

## description이 잘려 보일 때

skill 이름은 항상 컨텍스트에 포함되지만, skill이 많아지면 description은 **컨텍스트 예산**(기본: 모델 컨텍스트 윈도우의 1%)에 맞춰 잘리거나 드롭됩니다. 이때 Claude가 매칭에 쓸 키워드가 사라질 수 있어요.

- `/doctor`로 어떤 skill의 description이 잘리거나 드롭됐는지 확인
- 예산이 넘치면 **가장 적게 쓰는 skill부터** 드롭되므로, 자주 쓰는 skill은 보호됩니다
- 대응: 핵심 사용 사례를 description **맨 앞에** 쓰기(항목당 1,536자 캡), 설정으로 예산 올리기(`skillListingBudgetFraction`), 안 쓰는 skill은 `skillOverrides`에서 `"name-only"`나 `"off"`로

## Skill이 도중에 말을 안 듣는 것 같을 때

Skill 내용은 세션 내내 컨텍스트에 남지만, 모델이 다른 접근을 선택할 수는 있습니다.

- skill의 description과 지시를 더 강하게 다듬기
- 반드시 지켜져야 하는 동작이면 **hook으로 결정론적으로 강제**하기
- 컴팩션 이후에는 오래된 skill이 잘려나갈 수 있으니 **재호출**로 전체 내용 복원하기

## 평가: 트리거 ≠ 품질

Skill이 트리거되는 걸 봤다는 건 "Claude가 찾았다"는 뜻이지, "의도대로 동작했다"는 뜻이 아닙니다. 두 가지를 **따로** 측정하세요.

1. **트리거 정확도**: 원하는 프롬프트에서 호출되는가? (+ 원치 않는 프롬프트에서 호출되지 않는가?)
2. **출력 품질**: 호출됐을 때 결과물이 기대에 맞는가?

측정 방법은 **베이스라인 비교**입니다. 현실적인 프롬프트 몇 개를 skill이 있는 세션 / 비활성화한 세션에서 각각 **새 세션으로** 돌려 결과를 비교하세요. (새 세션이어야 skill 작성 중 남은 컨텍스트가 결과를 왜곡하지 않아요.)

### skill-creator 플러그인으로 자동화

공식 `skill-creator` 플러그인이 이 평가 루프를 자동화해줍니다.

```text
/plugin install skill-creator@claude-plugins-official
```

- **테스트 케이스**: 프롬프트·입력 파일·기대 동작을 `evals/evals.json`에 저장
- **격리 실행**: 테스트마다 서브에이전트를 띄워 깨끗한 컨텍스트에서 실행
- **채점 + 벤치마크**: skill 있음/없음의 통과율·시간·토큰을 비교
- **버전 A/B 비교**: 수정이 정말 개선인지 블라인드 비교로 확인
- **Description 튜닝**: 트리거돼야 할/되지 말아야 할 프롬프트를 생성해 적중률을 재고 description 수정안 제안

## 정리

- 안 되면: description 키워드 점검 → 목록 확인 → 바꿔 말하기 → `/이름` 직접 호출 → `--debug`.
- 너무 자주 되면: description 좁히기 or `disable-model-invocation: true`.
- description 잘림은 `/doctor`로 진단, 핵심을 맨 앞에 쓰는 습관이 예방책.
- 트리거 정확도와 출력 품질을 분리해서, 새 세션 베이스라인 비교로 평가할 것.

---

🎓 여기까지 **Introduction to agent skills** 강의 정리 끝! 수고했어요.
