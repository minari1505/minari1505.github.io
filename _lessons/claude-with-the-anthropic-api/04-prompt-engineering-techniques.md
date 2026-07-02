---
title: "Prompt Engineering Techniques"
title_ko: "프롬프트 엔지니어링 기법"
course: claude-with-the-anthropic-api
lesson: 4
---

## 학습 목표

- 좋은 프롬프트의 핵심 원칙 익히기
- 명확·구체·구조화·예시 4가지 기법을 적용하기
- eval 결과를 보며 프롬프트를 반복 개선하는 흐름 이해하기

프롬프트 엔지니어링은 **Claude가 원하는 결과를 내도록 지시를 다듬는 기술**이에요. 앞 레슨의 eval로 개선을 측정하면서 이 기법들을 적용해요.

## 프롬프트 엔지니어링의 기본 흐름

프롬프트 엔지니어링은 한 번에 완벽한 문장을 쓰는 일이 아니라, **초안 → 평가 → 개선 → 재평가**를 반복하는 과정이에요.

1. **목표 설정**: 프롬프트가 무엇을 해야 하는지 정의
2. **초기 프롬프트 작성**: 일부러 단순한 첫 버전을 만듦
3. **평가 실행**: 테스트셋과 grader로 성능 측정
4. **기법 적용**: 명확성, 구체성, 구조화, 예시 등을 추가
5. **재평가**: 점수가 실제로 나아졌는지 확인

핵심은 "좋아 보이는 프롬프트"가 아니라 **eval 점수가 개선되는 프롬프트**를 찾는 거예요. 한 번에 여러 기법을 섞기보다, 한 번에 하나씩 바꾸고 점수 변화를 보는 편이 원인을 파악하기 쉽습니다.

### 예시: 운동선수 식단 계획 프롬프트

강의에서는 운동선수를 위한 하루 식단 계획을 만드는 프롬프트를 예시로 사용해요. 입력은 키, 몸무게, 목표, 식이 제한이고, 출력은 간결하지만 충분한 식단 계획이어야 합니다.

처음에는 `PromptEvaluator` 같은 평가 도구로 데이터셋 생성과 모델 기반 채점을 묶어 둡니다.

```python
evaluator = PromptEvaluator(max_concurrent_tasks=5)
```

동시 실행 수(`max_concurrent_tasks`)는 낮게 시작하는 게 좋아요. API rate limit에 걸리면 eval 자체가 불안정해지기 때문입니다. 개발 중에는 테스트 케이스도 2-3개 정도로 작게 시작하고, 마지막 검증 때 늘립니다.

```python
dataset = evaluator.generate_dataset(
    task_description="Write a compact, concise 1 day meal plan for a single athlete",
    prompt_inputs_spec={
        "height": "Athlete's height in cm",
        "weight": "Athlete's weight in kg",
        "goal": "Goal of the athlete",
        "restrictions": "Dietary restrictions of the athlete",
    },
    output_file="dataset.json",
    num_cases=3,
)
```

초기 프롬프트는 일부러 단순하게 둬서 기준선을 만듭니다.

```python
def run_prompt(prompt_inputs):
    prompt = f"""
What should this person eat?

- Height: {prompt_inputs["height"]}
- Weight: {prompt_inputs["weight"]}
- Goal: {prompt_inputs["goal"]}
- Dietary restrictions: {prompt_inputs["restrictions"]}
"""

    messages = []
    add_user_message(messages, prompt)
    return chat(messages)
```

그다음 평가 기준을 추가해서 실행합니다.

```python
results = evaluator.run_evaluation(
    run_prompt_function=run_prompt,
    dataset_file="dataset.json",
    extra_criteria="""
The output should include:
- Daily caloric total
- Macronutrient breakdown
- Meals with exact foods, portions, and timing
""",
)
```

처음 점수가 낮아도 괜찮아요. 강의 예시처럼 `2.3 / 10` 같은 낮은 기준선이 나오는 것은 자연스럽습니다. 중요한 건 상세 리포트를 보고 어떤 요구사항이 빠졌는지 확인한 뒤, 프롬프트를 한 단계씩 개선하는 거예요.

## 1. 명확하고 직접적으로 (Be clear and direct)

Claude에게 **무엇을 원하는지 분명하게** 말하세요. 사람 신입에게 지시하듯, 애매함을 없애는 게 핵심이에요.

- ❌ "이 텍스트에 대해 뭔가 해줘"
- ⭕ "이 텍스트를 3문장으로 요약하고, 핵심 용어 3개를 굵게 표시해줘"

프롬프트에서 특히 중요한 부분은 **첫 문장**이에요. 첫 줄이 Claude에게 전체 작업의 방향을 잡아주기 때문입니다. 좋은 첫 줄은 단순하고, 바로 해야 할 일을 말하며, 질문보다 지시문에 가깝습니다.

### Clear: 쉽게 이해되는 말로 쓰기

명확하다는 것은 복잡한 배경 설명을 길게 늘어놓기보다, 원하는 작업을 바로 말하는 거예요.

- ❌ "태양을 이용해서 지붕 위에 설치하는 그런 장치들 있잖아, 그게 어떻게 되는지 알고 싶어"
- ⭕ "태양광 패널이 작동하는 방식을 3문단으로 설명해줘."

### Direct: 질문보다 지시로 쓰기

Claude에게는 "이런저런 배경이 있는데 어떻게 생각해?"보다, `Write`, `Create`, `Generate`, `Identify`처럼 행동 동사로 시작하는 지시가 더 잘 맞습니다.

- ❌ "지열 에너지에 대해 읽고 있었는데 흥미롭더라. 어떤 나라들이 쓰고 있어?"
- ⭕ "지열 에너지를 사용하는 국가 3곳을 식별하고, 각 국가의 발전량 통계를 포함해줘."

식단 계획 예시도 같은 방식으로 개선할 수 있어요.

```text
Generate a one-day meal plan for an athlete that meets their dietary restrictions.
```

이 첫 줄은 Claude에게 바로 세 가지를 알려줍니다.

- 해야 할 행동: `Generate`
- 만들어야 할 것: 하루 식단 계획
- 핵심 조건: 운동선수용, 식이 제한 반영

강의 예시에서는 이렇게 첫 줄을 명확하고 직접적으로 바꾸는 것만으로 eval 점수가 `2.32`에서 `3.92`로 올랐어요. 작은 문장 변경이지만, Claude가 추측해야 하는 부분을 줄이면 결과가 바로 좋아질 수 있습니다.

## 2. 구체적으로 (Be specific)

원하는 **형식·길이·범위·제약**을 명시하세요. Claude는 지시를 문자 그대로 따르므로, 빠뜨린 조건은 추측하게 돼요.

- 출력 형식: "JSON으로", "불릿 5개로"
- 길이: "100자 이내로"
- 관점/톤: "초보자도 이해하도록", "격식 있는 어조로"
- 하지 말 것: "코드 설명은 빼고 코드만"

구체성은 크게 두 종류로 나눌 수 있어요.

### 출력 품질 가이드라인

첫 번째는 결과물이 가져야 할 속성을 나열하는 방식입니다.

- 응답 길이
- 구조와 형식
- 반드시 포함할 요소
- 톤과 스타일
- 제외해야 할 내용

예를 들어 "숨겨진 재능을 발견하는 인물의 짧은 이야기를 써줘"라고만 하면 Claude가 길이, 등장인물 수, 장르, 갈등의 강도를 모두 추측해야 해요. 대신 다음처럼 기준을 주면 결과가 훨씬 안정적입니다.

```text
Write a short story under 1,000 words.
The story should:
- Focus on one main character
- Include one supporting character
- Show a clear action that reveals the hidden talent
- End with a concrete consequence of that discovery
```

식단 계획 예시에서는 구체적인 가이드라인을 넣는 것만으로 eval 점수가 `3.92`에서 `7.86`까지 올랐다고 설명해요.

```text
Guidelines:
1. Include accurate daily calorie amount
2. Show protein, fat, and carb amounts
3. Specify when to eat each meal
4. Use only foods that fit restrictions
5. List all portion sizes in grams
6. Keep budget-friendly if mentioned
```

### 사고 과정/작업 단계 지정

두 번째는 Claude가 따라야 할 절차를 지정하는 방식입니다. 복잡한 문제를 바로 답하게 하지 말고, 어떤 관점들을 차례로 검토해야 하는지 안내해요.

예를 들어 영업팀 성과가 떨어진 이유를 분석한다면, 그냥 "왜 매출이 떨어졌어?"라고 묻기보다 다음처럼 단계를 줍니다.

```text
Analyze why the sales team's performance dropped.

Consider these factors in order:
1. Market and seasonal metrics
2. Industry-wide changes
3. Individual sales rep performance
4. Internal organizational changes
5. Customer feedback and churn signals

Then summarize the three most likely causes.
```

대부분의 프롬프트에는 출력 품질 가이드라인을 넣는 것이 좋고, 문제 해결·의사결정·원인 분석처럼 여러 관점을 검토해야 하는 작업에는 단계 지시를 함께 넣는 것이 좋습니다.

## 3. XML 태그로 구조화 (Structure with XML tags)

입력의 여러 부분을 **XML 태그로 감싸** 경계를 분명히 하세요. Claude가 "무엇이 지시이고 무엇이 데이터인지" 헷갈리지 않아요.

```text
다음 <document> 안의 내용을 <instructions> 규칙대로 요약해줘.

<instructions>
- 3문장 이내
- 존댓말
</instructions>

<document>
{긴 문서 내용...}
</document>
```

- 태그 이름은 자유롭게 정하되, 지시에서 언급한 이름과 일치시켜요.
- 출력도 태그로 감싸달라고 하면 후처리가 쉬워져요.

XML 태그는 특히 프롬프트 안에 긴 문서, 코드, 로그, 표, 사용자 데이터처럼 서로 다른 종류의 텍스트가 섞일 때 효과적이에요. 경계가 없으면 Claude가 어디까지가 지시이고 어디부터가 분석 대상인지 헷갈릴 수 있습니다.

예를 들어 20페이지 분량의 sales record를 분석해야 한다면, 그냥 붙여넣는 것보다 다음처럼 감싸는 편이 좋아요.

```text
Analyze the sales records in <sales_records>.
Return the top 3 trends and 2 risks.

<sales_records>
{sales records...}
</sales_records>
```

코드와 문서를 함께 줄 때도 태그가 중요합니다.

```text
Debug the code in <my_code> using the reference material in <docs>.

<my_code>
{code...}
</my_code>

<docs>
{documentation...}
</docs>
```

태그 이름은 공식 규칙이 있는 게 아니므로, 내용의 의미가 드러나게 직접 만들면 됩니다.

- `<sales_records>`가 `<data>`보다 더 명확함
- `<athlete_information>`은 키·몸무게·목표·식이 제한이 한 묶음임을 알려줌
- `<my_code>`와 `<docs>`는 코드와 참고 문서를 분리함

식단 계획 프롬프트도 이렇게 구조화할 수 있어요.

```text
<athlete_information>
- Height: 6'2"
- Weight: 180 lbs
- Goal: Build muscle
- Dietary restrictions: Vegetarian
</athlete_information>

Generate a meal plan based on the athlete information above.
```

짧은 프롬프트에서는 변화가 작을 수 있지만, 프롬프트가 길어지고 여러 변수를 보간할수록 XML 태그의 효과가 커집니다.

## 4. 예시 제공 (Providing examples / few-shot)

원하는 입출력 **예시 1~몇 개**를 보여주면, 말로 설명하기 어려운 형식·스타일을 정확히 전달할 수 있어요. 이걸 **few-shot 프롬프팅**이라고 해요.

```text
입력: "배송이 빨라요" → 출력: {"sentiment": "긍정", "topic": "배송"}
입력: "품질이 별로" → 출력: {"sentiment": "부정", "topic": "품질"}
입력: "가격이 합리적이네요" → 출력:
```

- 예시는 엣지 케이스나 원하는 출력 형식을 보여줄 때 특히 효과적이에요.

예시는 "말로 설명하기 어려운 요구사항"을 보여주는 가장 강한 방법 중 하나예요. 특히 애매한 입력, 풍자, 복잡한 JSON 형식, 특정 톤처럼 규칙만으로 전달하기 어려운 부분에 효과적입니다.

### One-shot vs Multi-shot

- **One-shot**: 예시 하나로 패턴을 보여줌
- **Multi-shot**: 여러 예시로 다양한 케이스와 예외를 보여줌

단순한 출력 형식만 알려주면 one-shot으로 충분할 수 있고, 엣지 케이스가 많거나 오답 패턴이 다양하면 multi-shot이 좋습니다.

### 풍자 감정 분석 예시

트윗 감정 분석에서 풍자는 흔한 실패 사례예요. 표면적으로는 긍정 단어가 있어도 실제 의미는 부정일 수 있습니다.

```text
Classify the sentiment of the tweet as Positive or Negative.
Treat sarcasm carefully.

<example>
<sample_input>
Great game tonight!
</sample_input>
<ideal_output>
Positive
</ideal_output>
</example>

<example>
<sample_input>
Oh yeah, I really needed a flight delay tonight! Excellent!
</sample_input>
<ideal_output>
Negative
</ideal_output>
<why>
The positive words are sarcastic because the situation is clearly frustrating.
</why>
</example>

<tweet>
Yeah, sure, that was the best movie I've seen since Plan 9 from Outer Space.
</tweet>
```

예시도 XML 태그로 구조화하면 `sample_input`, `ideal_output`, `why`가 각각 무엇인지 분명해집니다.

### eval에서 좋은 예시 찾기

eval을 돌린 뒤 가장 점수가 높았던 출력은 few-shot 예시 후보가 됩니다. 점수 `10` 또는 가장 높은 점수를 받은 입력/출력 쌍을 찾아 프롬프트에 넣으면 Claude가 "이 작업에서 이상적인 답변이 어떤 모습인지" 더 잘 이해해요.

다만 입력/출력만 붙이지 말고, 왜 좋은 예시인지 짧게 설명하면 더 좋습니다.

```text
<ideal_output>
[Your example output here]
</ideal_output>

This example is well-structured, gives exact food quantities,
and respects the athlete's goal and restrictions.
```

좋은 예시를 고를 때는 다음 기준을 봅니다.

- 실제로 자주 나오는 실패 케이스를 다루는가?
- 원하는 출력 형식과 톤을 정확히 보여주는가?
- 현재 작업과 충분히 비슷한가?
- 예시가 너무 길어서 본문 지시를 압도하지 않는가?

## 실전 팁

- 한 번에 완벽을 노리지 말고, **eval로 측정하며 반복**하세요.
- 최신 모델은 지시를 **매우 문자 그대로** 따라요. "반드시(CRITICAL)", "무조건" 같은 과한 표현은 오히려 과잉 반응을 유발할 수 있으니 담백하게 쓰세요.

## 정리

- **명확·직접**: 원하는 걸 분명히. **구체**: 형식·길이·제약 명시.
- **XML 태그**: 지시와 데이터의 경계를 구분. **예시**: 말보다 보여주기(few-shot).
- 항상 eval로 개선을 검증하며 반복해요.
