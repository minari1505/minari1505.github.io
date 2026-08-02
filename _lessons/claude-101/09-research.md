---
title: "Research"
title_ko: "Research로 깊이 조사하기"
course: claude-101
lesson: 9
---

## 학습 목표

- Research가 단순 검색이 아니라 체계적인 multi-source investigation이라는 점 이해하기
- 언제 Research를 쓰고, 언제 web search나 extended thinking, Enterprise Search를 써야 하는지 구분하기
- Research가 extended thinking과 함께 어떻게 깊이 있는 report를 만드는지 파악하기
- 복잡한 조사 작업에 적합한 Research prompt 작성 방법 익히기

## Research란?

**Research**는 Claude를 단순 대화형 assistant에서 체계적인 조사자로 확장하는 기능입니다. Research를 켜면 Claude는 질문에 바로 답하는 대신, 여러 각도에서 정보를 찾고, web과 connected integrations를 탐색하고, 결과를 종합해 report를 만듭니다.

단일 web search와 다르게 Research는 agentic하게 작동합니다.

```text
질문 이해
  ↓
조사 계획 수립
  ↓
여러 검색 수행
  ↓
찾은 정보 기반으로 다음 검색 결정
  ↓
자료 종합
  ↓
citation 포함 report 작성
```

숙련된 research assistant가 몇 시간 동안 할 일을 Claude가 몇 분 안에 수행하는 경험에 가깝습니다.

## Research의 핵심 특징

### 여러 검색이 서로 이어짐

Research는 한 번 검색하고 끝내지 않습니다. 찾은 정보를 바탕으로 다음에 무엇을 찾아야 할지 결정합니다. 질문의 다른 각도를 자동으로 탐색하고, 빈틈을 채우며, open question을 체계적으로 좁혀갑니다.

### 수분 안에 포괄적인 답변 생성

대부분의 report는 5-15분 안에 완료됩니다. 더 복잡한 조사는 최대 45분까지 걸릴 수 있습니다. 사람이 수동으로 하면 몇 시간이 걸릴 수 있는 조사 작업을 더 짧은 시간에 수행합니다.

### Extended thinking 자동 활성화

Research를 켜면 extended thinking이 자동으로 활성화됩니다. Claude는 먼저 문제를 작은 조각으로 나누고, 어떤 정보를 찾아야 하는지 계획한 뒤 조사를 진행합니다.

### Citation으로 검증 가능

Research report에는 source citation이 포함됩니다. 따라서 Claude의 findings를 신뢰하되, 필요한 경우 원문으로 돌아가 직접 검증할 수 있습니다.

## Research를 언제 사용할까?

Research는 빠른 답보다 깊이 있는 이해가 필요할 때 적합합니다.

사용하면 좋은 경우:

- 여러 source를 종합한 comprehensive report가 필요할 때
- web과 Google Workspace 같은 connected integrations를 함께 분석해야 할 때
- 수동으로 몇 시간 걸릴 thorough investigation이 필요할 때
- competitor나 vendor option을 비교 분석해야 할 때
- citation이 포함된 검증 가능한 report가 필요할 때

예시:

- Market analysis와 competitive research
- Team offsite나 product launch 같은 복잡한 project planning
- email, calendar, documents에 흩어진 정보 종합
- 여러 source를 참고하는 technical documentation 작성
- 최신 정보와 검증 가능한 source가 필요한 briefing 준비

## 다른 Claude 기능과 구분하기

### Web search가 더 적합한 경우

다음처럼 빠르고 구체적인 사실 하나가 필요하면 web search가 더 좋습니다.

- 오늘의 주가
- 회사 주소
- 특정 사건의 날짜
- 한두 source면 충분한 질문

속도가 포괄성보다 중요하면 web search를 사용합니다.

### Extended thinking이 더 적합한 경우

외부 정보 수집보다 깊은 reasoning이 필요한 문제라면 extended thinking이 더 적합합니다.

- 수학 문제
- code debugging
- 논리 분석
- 복잡한 전략 문제지만 외부 자료가 필요 없는 경우

### Enterprise Search가 더 적합한 경우

조직 내부 지식에서 답을 찾아야 한다면 Enterprise Search가 더 좋습니다.

- 회사 정책과 프로세스
- Slack thread, email, 회의록, 내부 문서
- 회사별 과거 의사결정
- onboarding 관련 질문

Research는 public web과 connected integrations를 활용한 깊은 조사에 강하고, Enterprise Search는 조직 내부 지식 검색에 특화되어 있습니다.

## Research가 작동하는 방식

Research는 네 단계로 이해할 수 있습니다.

### 1. 접근 방식 계획

Research가 켜지면 extended thinking이 활성화됩니다. Claude는 요청을 분석하고, 어떤 정보가 필요한지, 어떤 각도를 조사해야 하는지 계획합니다.

### 2. 여러 검색 수행

Claude는 단일 query만 실행하지 않습니다. 첫 검색 결과를 보고 다음에 조사할 내용을 결정합니다. 유망한 단서를 따라가고, 부족한 부분을 채우는 방식입니다.

### 3. Findings 종합

web과 connected integrations에서 가져온 정보를 하나의 report로 정리합니다. Gmail, Google Calendar, Google Drive 같은 연결된 도구가 있다면 내부 context도 함께 활용할 수 있습니다.

### 4. Citation 제공

Research report의 claim은 source에 연결됩니다. 이를 통해 사용자는 정보의 출처를 확인하고, 필요하면 더 깊이 들어갈 수 있습니다.

## Research 사용 방법

1. Chat interface 왼쪽 아래의 `+` 버튼을 클릭합니다.
2. 메뉴에서 `Research`를 선택합니다.
3. Research가 활성화되면 강조 표시됩니다.
4. Prompt를 입력하고 제출합니다.
5. Claude가 background에서 검색과 분석을 수행하며 progress indicator를 보여줍니다.

주의할 점: Research가 작동하려면 web search가 켜져 있어야 합니다. web search가 꺼져 있다면 같은 `+` 메뉴에서 활성화할 수 있습니다.

## 효과적인 Research prompt 작성법

Research는 5-45분이 걸릴 수 있으므로, prompt를 잘 쓰는 것이 중요합니다.

### 목표를 구체적으로 쓰기

```text
EV market에 대해 알려줘.
```

보다:

```text
전기차 배터리 시장을 분석해줘.
주요 player, 기술 trend, 투자 의사결정에 영향을 줄 수 있는 supply chain challenge를 식별해줘.
```

가 더 좋습니다.

### 원하는 구조 지정하기

Claude가 findings를 어떤 구조로 정리할지 미리 알려줍니다.

```text
Team offsite 장소 후보를 비교해줘.
다음 섹션으로 정리해줘:
1. 위치와 접근성
2. meeting space와 amenities
3. catering options
4. 가격 고려사항
```

### 제약 조건 포함하기

Budget, timeline, geography, audience, required format 같은 제약을 넣으면 Research가 더 관련성 높은 방향으로 좁혀집니다.

### Research prompt 자체를 Claude와 다듬기

조사 질문을 어떻게 잡아야 할지 모르겠다면 Research를 켜기 전에 Claude에게 prompt를 같이 다듬어달라고 요청할 수 있습니다.

```text
이 주제에 대해 Research mode로 조사하려고 해.
좋은 Research prompt로 바꿔줘.
```

## Connected integrations와 함께 쓰기

Google Workspace나 다른 integrations가 연결되어 있으면 Research가 더 강력해집니다. Claude는 web research와 함께 email, calendar, documents의 context를 가져올 수 있습니다.

예시:

```text
Project X에 대해 email과 Slack에서 논의된 내용을 요약하고,
비슷한 initiative에 대한 industry best practices를 조사해줘.
```

```text
다음 주 내 calendar commitments를 검토하고,
내가 만날 각 회사에 대해 조사해줘.
```

```text
우리 pricing strategy 관련 내부 문서를 모두 찾고,
경쟁사 positioning과 비교해줘.
```

원하는 source가 있다면 prompt에 명시할 수 있습니다.

```text
내 Google Drive에서 관련 context를 가져와줘.
```

```text
이 주제에 대한 최근 email의 insight도 포함해줘.
```

Web search를 끄고 connected tools만 대상으로 internal-only research를 할 수도 있습니다. 예를 들어 "Q3 launch에 대해 우리 팀이 Slack과 Docs에서 논의한 내용"처럼 내부 자료만 필요한 경우에 유용합니다.

## 실습 질문

다음 질문을 생각해보면 Research 활용처를 찾기 쉽습니다.

- 내 업무에서 여러 source를 모아야 하는 research task는 무엇인가?
- Google Workspace 같은 connected integrations와 Research를 결합하면 workflow가 어떻게 달라질까?
- 조사 시간이 너무 오래 걸려 미뤄둔 복잡한 질문은 무엇인가?
- 빠른 web search로 충분한 질문과 Research가 필요한 질문을 어떻게 구분할 수 있을까?

## 핵심 정리

Research는 Claude가 여러 source를 탐색하고 종합해 깊이 있는 report를 만드는 기능입니다.

- 단일 검색이 아니라 여러 검색을 이어가는 agentic investigation입니다.
- Extended thinking이 자동 활성화되어 조사 계획을 세웁니다.
- 대부분의 report는 5-15분, 복잡한 조사는 최대 45분 정도 걸릴 수 있습니다.
- Citation이 포함되어 findings를 검증하기 쉽습니다.
- Web과 connected integrations를 함께 활용할 수 있습니다.
- 빠른 사실 확인은 web search, 외부 정보 없는 깊은 추론은 extended thinking, 조직 내부 지식 검색은 Enterprise Search가 더 적합합니다.

다음 섹션에서는 지금까지 배운 기능이 실제 역할별 use case에서 어떻게 조합되는지 살펴봅니다.
