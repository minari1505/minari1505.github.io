---
title: "Designing effective subagents"
title_ko: "효과적인 서브에이전트 설계"
course: introduction-to-subagents
lesson: 3
---

## 학습 목표

- 효과적인 서브에이전트를 만드는 네 가지 요소 이해하기: 좋은 description, 출력 형식 정의, 장애물 보고, 도구 접근 제한
- description이 "언제 실행할지"뿐 아니라 "무엇을 시킬지"까지 형성함을 이해하기
- 서브에이전트 유형별 적절한 도구 범위 판단하기

## config 데이터는 어떻게 쓰이나

메인 에이전트에 메시지를 보내면, **사용 가능한 모든 서브에이전트의 name과 description이 system prompt에 포함**됩니다. 이것으로 메인 에이전트가 어떤 서브에이전트를 언제 띄울지 결정합니다.

description은 두 번째 역할도 합니다. 메인 에이전트가 서브에이전트를 띄울 때 작업을 시작할 **입력 prompt를 작성**하는데, 그 지침으로 description을 사용합니다. 즉 description은 **언제 실행할지**뿐 아니라 **무엇을 하라고 지시받을지**까지 형성합니다.

![code-quality-reviewer.md의 name·description — description이 서브에이전트 동작을 형성](/assets/images/courses/introduction-to-subagents/designing-1.png)

### 입력 prompt를 형성하는 description 쓰기

code review 서브에이전트를 예로, 일반적 description이면 메인 에이전트가 "get diff로 현재 변경을 찾아라" 같은 모호한 입력 prompt를 씁니다. description에 **"검토할 파일을 정확히 알려줘야 한다"**를 넣으면, 메인 에이전트가 실제 파일을 나열한 훨씬 구체적인 입력 prompt를 씁니다. (web search 서브에이전트에 "인용 가능한 출처를 반환하라"를 넣는 것도 같은 원리.)

![description에 따라 메인 에이전트가 서브에이전트에 보내는 구체적 입력 prompt](/assets/images/courses/introduction-to-subagents/designing-2.png)

## 출력 형식 정의 (가장 중요)

서브에이전트에 할 수 있는 **가장 중요한 개선은 system prompt에 출력 형식을 정의**하는 것입니다. 두 가지 효과가 있습니다.

- **자연스러운 종료 지점**을 만듦 — 각 섹션을 채우면 끝났음을 앎
- **너무 오래 도는 것을 방지** — 출력이 정의되지 않으면 언제 충분히 조사했는지 판단하지 못해 필요 이상으로 김

code review 서브에이전트의 구조화된 출력 예시:

```text
1. Summary: 검토 내용과 전반 평가 요약
2. Critical Issues: 즉시 고쳐야 할 보안 취약점·데이터 무결성 위험·논리 오류
3. Major Issues: 품질 문제·아키텍처 불일치·중대한 성능 우려
4. Minor Issues: 스타일 불일치·문서 누락·사소한 최적화
5. Recommendations: 개선·리팩터링·모범 사례 제안
6. Approval Status: merge/deploy 준비 여부 명확히
```

![구조화된 출력 형식이 담긴 서브에이전트 config (Summary~Approval, Obstacles Encountered)](/assets/images/courses/introduction-to-subagents/designing-3.png)

## 장애물 보고

서브에이전트가 작업 중 발견한 우회책(의존성 문제 해결, 특정 플래그 필요 등)은 **반환 요약에 반드시 나타나야** 합니다. 안 그러면 메인 스레드가 같은 해법을 다시 찾느라 시간·토큰을 낭비합니다. 출력 형식에 **"Obstacles Encountered"** 섹션을 추가하면 이 정보가 안정적으로 드러납니다.

```text
7. Obstacles Encountered: 검토 중 만난 장애물 보고 — setup 이슈, 발견한 우회책,
   환경 특이점, 특별한 플래그·설정이 필요했던 명령, 문제를 일으킨 의존성·import.
```

## 도구 접근 제한

서브에이전트마다 모든 도구가 필요하진 않습니다. 실제 필요한 것만 주면 **의도치 않은 부작용을 막고**, 여러 서브에이전트가 있을 때 **역할이 명확**해집니다.

- **Research / read-only** — Glob, Grep, Read만. 파일을 실수로 수정할 수 없음.
- **Code reviewer** — `git diff`를 실행할 Bash는 필요하지만 Edit/Write는 불필요.
- **Styling / 코드 수정 에이전트** — 실제로 코드를 바꾸는 게 일이므로 Edit/Write 부여.

## 핵심 정리

효과적인 서브에이전트의 네 가지 특징:

- **구체적인 description** — 언제 실행되고 어떤 지시를 받을지 둘 다 조종
- **구조화된 출력** — 언제 끝날지 알고, 메인 스레드가 쓸 수 있는 정보를 반환
- **장애물 보고** — 우회책·특이점·문제를 재발견하지 않도록 형식에 포함
- **제한된 도구 접근** — research는 read-only, reviewer는 bash, 코드 변경 에이전트만 edit/write
