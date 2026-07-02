---
title: "Prompt Evaluation"
title_ko: "프롬프트 평가(Eval)"
course: claude-with-the-anthropic-api
lesson: 3
---

## 학습 목표

- 프롬프트를 "감"이 아니라 **측정**으로 개선하는 이유 이해하기
- 전형적인 eval 워크플로우와 테스트 데이터셋 만들기
- 모델 기반 채점 vs 코드 기반 채점 구분하기

## 왜 평가(Eval)가 필요할까?

프롬프트를 바꿨을 때 "더 좋아진 것 같다"는 느낌만으로는 확신할 수 없어요. **eval = 프롬프트/모델 변경이 실제로 개선인지 자동으로 측정하는 체계**예요. 회귀(regression)를 잡고, 모델을 바꿔도 품질이 유지되는지 확인할 수 있어요.

AI 애플리케이션을 만들 때 좋은 프롬프트를 쓰는 것은 시작일 뿐이에요. 실제 사용자 입력은 우리가 예상한 것보다 훨씬 다양하고, 몇 번 손으로 테스트해서 잘 된 것처럼 보여도 운영 환경에서는 쉽게 깨질 수 있어요. 그래서 프롬프트를 "잘 쓴다"와 프롬프트가 "잘 작동한다고 측정한다"를 분리해서 봐야 합니다.

## 프롬프트 엔지니어링 vs 프롬프트 평가

**프롬프트 엔지니어링**은 Claude가 요청을 더 잘 이해하고 원하는 형식으로 답하게 만드는 작성 기술이에요.

- 여러 예시를 넣는 multishot prompting
- XML 태그로 입력과 지시를 구조화하기
- 역할, 출력 형식, 제약 조건을 명확히 쓰기

반면 **프롬프트 평가(prompt evaluation)**는 프롬프트를 어떻게 쓸지보다, 그 프롬프트가 실제로 얼마나 잘 작동하는지 **자동 테스트로 측정**하는 일에 초점을 둬요.

- 기대 답변과 비교하기
- 같은 작업을 하는 프롬프트 버전 A/B 비교하기
- 출력에서 오류·누락·형식 위반 찾기

둘은 대체 관계가 아니라 순서가 있는 한 쌍이에요. 엔지니어링으로 후보 프롬프트를 만들고, eval로 성능을 재서 다시 개선합니다.

## 프롬프트를 쓴 뒤의 세 가지 선택지

프롬프트 초안을 만든 뒤 보통 세 가지 길 중 하나를 선택하게 됩니다.

| 선택 | 방식 | 위험 |
|---|---|---|
| 한 번만 테스트 | 예시 하나를 넣어 보고 괜찮으면 끝 | 실제 사용자 입력에서 바로 깨질 수 있음 |
| 몇 번 테스트하고 수정 | 코너 케이스 몇 개를 고쳐가며 조정 | 내가 떠올린 케이스에만 강해짐 |
| eval 파이프라인으로 측정 | 테스트셋 전체를 실행하고 점수로 비교 | 준비 비용은 들지만 신뢰도가 높음 |

1번과 2번은 자연스럽지만 위험한 함정이에요. 개발자는 자신이 만든 기능을 대표적인 입력 몇 개로만 확인하기 쉽고, 사용자는 훨씬 더 예측하기 어려운 방식으로 입력합니다. 특히 고객 지원, 분류, 데이터 추출, 자동화처럼 실패 비용이 있는 기능이라면 "대충 잘 나오는 것 같다"로는 부족해요.

## Evaluation-first 접근

더 안정적인 방식은 프롬프트를 고친 뒤 평가하는 게 아니라, **평가 기준을 먼저 세우고 프롬프트를 반복 개선**하는 거예요.

1. 실제 사용 시나리오를 대표하는 테스트 케이스를 모음
2. 각 케이스의 기대 결과나 채점 기준을 정의
3. 현재 프롬프트를 전체 테스트셋에 실행
4. 점수와 실패 사례를 보고 프롬프트 수정
5. 다시 실행해서 이전 버전보다 나아졌는지 비교

이 방식은 시간과 비용이 더 들지만, 운영 전에 약점을 발견하고 프롬프트 버전을 객관적으로 비교할 수 있게 해줘요. 목표는 사용자가 문제를 발견하기 전에 개발 중에 먼저 잡는 것입니다.

## 전형적인 eval 워크플로우

```
1. 테스트 데이터셋 준비 (입력 + 기대 결과/기준)
2. 각 입력을 현재 프롬프트로 실행
3. 출력을 채점 (모델 기반 또는 코드 기반)
4. 점수 집계 → 통과율/평균 확인
5. 프롬프트 수정 → 다시 실행 → 이전과 비교
```

## 테스트 데이터셋 만들기

- 현실적인 입력 예시를 모아요. 직접 작성하거나 **Claude로 생성**할 수도 있어요.
- 각 케이스에 "무엇이 정답인가" 또는 "어떤 기준을 만족해야 하는가"를 함께 기록해요.

```python
dataset = [
    {"input": "이 리뷰 감정 분류: 최고예요!", "expected": "긍정"},
    {"input": "이 리뷰 감정 분류: 별로였어요", "expected": "부정"},
    # ...
]
```

### 예시 목표: AWS 코드 생성 프롬프트 평가

강의에서는 AWS 관련 작업을 도와주는 프롬프트를 예시로 eval을 만들어요. 목표는 사용자가 작업을 요청하면 세 가지 형식 중 하나로 **깔끔한 결과만** 돌려주는 것입니다.

- Python 코드
- JSON 설정 파일
- 정규표현식(Regex)

중요한 요구사항은 설명, 제목, 꼬리말 없이 바로 복사해서 쓸 수 있는 출력만 반환하는 거예요. 시작 프롬프트는 일부러 단순하게 둡니다.

```python
prompt = f"""
Please provide a solution to the following task:
{task}
"""
```

이 프롬프트는 아직 출력 형식 지시가 없기 때문에 Claude가 설명을 덧붙일 가능성이 높아요. eval은 이런 문제를 숫자와 실패 사례로 드러내는 역할을 합니다.

### Claude로 테스트 데이터 생성하기

테스트 데이터셋은 프롬프트에 넣을 입력들의 모음이에요. 여기서는 각 항목이 `task` 하나를 가진 JSON 객체입니다.

```json
[
  {"task": "Create a Python function that checks whether an S3 bucket name is valid."},
  {"task": "Write an EventBridge JSON rule for EC2 instance state changes."},
  {"task": "Create a regex that matches AWS Lambda function ARNs."}
]
```

데이터셋은 직접 만들 수도 있지만, 다양한 케이스를 빠르게 뽑기 위해 Claude에게 생성시킬 수도 있어요. 평가 데이터 생성에는 빠르고 저렴한 모델을 써도 충분한 경우가 많습니다.

```python
import json

def generate_dataset():
    dataset_prompt = """
Generate an evaluation dataset for prompts that produce Python, JSON,
or Regex for AWS-related tasks.

Return an array of JSON objects. Each object should have one "task" field.

Rules:
- Focus on tasks solvable with one Python function, one JSON object, or one regex.
- Avoid tasks that require long programs.
- Generate 3 objects.
"""

    messages = []
    add_user_message(messages, dataset_prompt)
    add_assistant_message(messages, "```json")
    text = chat(messages, stop_sequences=["```"])
    return json.loads(text.strip())
```

여기서도 **assistant message prefilling + stop sequence**를 사용해 JSON만 받도록 유도합니다.

```python
dataset = generate_dataset()

with open("dataset.json", "w") as f:
    json.dump(dataset, f, indent=2)
```

이렇게 저장해두면 이후 eval 실행 단계에서 같은 테스트셋을 반복해서 사용할 수 있어요.

## Eval 실행하기

데이터셋이 준비되면 각 테스트 케이스를 프롬프트 템플릿에 넣고 Claude를 호출한 뒤, 결과를 채점하는 파이프라인을 만들어요.

### `run_prompt`

하나의 테스트 케이스를 받아 실제 프롬프트를 만들고 Claude의 출력을 반환합니다.

```python
def run_prompt(test_case):
    prompt = f"""
Please solve the following task:

{test_case["task"]}
"""

    messages = []
    add_user_message(messages, prompt)
    return chat(messages)
```

처음에는 프롬프트를 단순하게 유지합니다. 그래야 eval 결과를 보고 어떤 지시가 부족한지 확인할 수 있어요.

### `run_test_case`

테스트 케이스 하나를 실행하고 결과를 기록합니다. 아직 grader를 붙이기 전이라 점수는 임시로 넣어둡니다.

```python
def run_test_case(test_case):
    output = run_prompt(test_case)

    return {
        "output": output,
        "test_case": test_case,
        "score": 10,  # TODO: 실제 grader로 교체
    }
```

### `run_eval`

전체 데이터셋을 순회하며 모든 테스트 결과를 모읍니다.

```python
def run_eval(dataset):
    results = []

    for test_case in dataset:
        result = run_test_case(test_case)
        results.append(result)

    return results
```

```python
with open("dataset.json", "r") as f:
    dataset = json.load(f)

results = run_eval(dataset)
print(json.dumps(results, indent=2))
```

처음 실행하면 출력이 꽤 장황할 수 있어요. 아직 프롬프트에 "설명 없이 코드/JSON/Regex만 반환하라"는 제약이 없기 때문입니다. 이 실패 사례가 바로 다음 프롬프트 개선의 근거가 됩니다.

## Grader의 종류와 평가 기준

grader는 모델 출력에 대해 측정 가능한 피드백을 돌려주는 장치예요. 보통 `1`부터 `10`까지의 점수처럼 비교 가능한 값을 반환합니다.

| 종류 | 방식 | 잘 맞는 평가 |
|---|---|---|
| 코드 기반 grader | 직접 작성한 코드로 검증 | 형식, 문법, 문자열 포함 여부, JSON 파싱 |
| 모델 기반 grader | 다른 Claude 호출로 평가 | 지시사항 준수, 완성도, 유용성, 안전성 |
| 사람 grader | 사람이 직접 검토 | 깊이, 적절성, 뉘앙스, 최종 품질 확인 |

grader를 만들기 전에 평가 기준을 먼저 정해야 해요. AWS 코드 생성 예시라면 다음 기준을 사용할 수 있습니다.

- **Format**: Python, JSON, Regex만 반환하고 설명을 붙이지 않는가?
- **Valid syntax**: JSON/Python/Regex 문법이 실제로 유효한가?
- **Task following**: 사용자의 작업 요구를 정확히 해결하는가?

앞의 두 기준은 코드 기반 grader가 잘 처리하고, task following처럼 유연한 판단이 필요한 기준은 모델 기반 grader가 더 잘 맞습니다.

## 채점 방법 1 — 코드 기반 (Code-based grading)

정답이 **명확하고 검증 가능**할 때 써요. 문자열 일치, 정규식, JSON 필드 검증, 숫자 범위 등. 빠르고 결정적이에요.

```python
def grade(output, expected):
    return output.strip() == expected  # 정확히 일치하면 통과
```

- 분류, 추출, 형식 검증처럼 "맞다/틀리다"가 분명한 작업에 적합.

코드 생성 eval에서는 "답이 그럴듯한가"뿐 아니라 **실제로 파싱 가능한 코드인가**도 확인해야 해요. AWS 코드 생성 예시에서는 코드 기반 grader가 주로 두 가지를 봅니다.

- **Format**: Python, JSON, Regex만 반환하고 설명·마크다운·머리말이 없는가?
- **Valid syntax**: 의도한 언어로 실제 파싱이 되는가?

반면 "사용자의 작업을 정확히 해결했는가"는 더 유연한 판단이 필요하므로 모델 기반 grader가 맡는 편이 좋아요.

### 문법 검증 함수

Python 표준 라이브러리만으로 JSON, Python, Regex 문법을 확인할 수 있습니다.

```python
import ast
import json
import re

def validate_json(text):
    try:
        json.loads(text.strip())
        return 10
    except json.JSONDecodeError:
        return 0

def validate_python(text):
    try:
        ast.parse(text.strip())
        return 10
    except SyntaxError:
        return 0

def validate_regex(text):
    try:
        re.compile(text.strip())
        return 10
    except re.error:
        return 0
```

각 함수는 파싱에 성공하면 `10`, 실패하면 `0`을 반환합니다. 점수를 더 세밀하게 주고 싶다면 "파싱은 되지만 불필요한 설명이 붙은 경우", "코드 블록 fence가 남은 경우"처럼 추가 규칙을 넣을 수 있어요.

### 테스트 케이스에 `format` 추가하기

어떤 validator를 쓸지 알려면 데이터셋에 기대 출력 형식이 있어야 합니다.

```json
{
  "task": "Create a Python function to validate an AWS IAM username",
  "format": "python"
}
```

데이터셋 생성 프롬프트에도 `format` 필드를 요구하면 돼요.

```text
Return an array of JSON objects with:
- "task": the AWS-related coding task
- "format": one of "python", "json", or "regex"
```

그러면 syntax grader는 `format`에 따라 적절한 검증 함수를 호출할 수 있습니다.

```python
def grade_syntax(output, test_case):
    output_format = test_case["format"]

    if output_format == "json":
        return validate_json(output)
    if output_format == "python":
        return validate_python(output)
    if output_format == "regex":
        return validate_regex(output)

    raise ValueError(f"Unknown format: {output_format}")
```

### 프롬프트도 더 명확하게 만들기

코드 기반 grader에서 실패가 많이 나오면, 프롬프트에 출력 형식 제약을 더 분명히 넣습니다.

```text
* Respond only with Python, JSON, or a plain Regex.
* Do not add comments, commentary, markdown fences, headers, or footers.
```

또는 assistant message prefill로 Claude가 코드 블록 안에서 바로 이어 쓰는 것처럼 유도할 수도 있어요.

```python
messages = []
add_user_message(messages, prompt)
add_assistant_message(messages, "```code")
text = chat(messages, stop_sequences=["```"])
```

이 기법은 JSON뿐 아니라 Python, Regex처럼 "설명 없이 원본 코드만" 필요한 경우에도 유용합니다.

### 모델 점수와 코드 점수 합치기

최종 점수는 모델 기반 grader의 품질 점수와 코드 기반 grader의 문법 점수를 합쳐서 만들 수 있어요. 가장 단순한 방식은 평균입니다.

```python
def run_test_case(test_case):
    output = run_prompt(test_case)

    model_grade = grade_by_model(test_case, output)
    model_score = model_grade["score"]
    syntax_score = grade_syntax(output, test_case)
    score = (model_score + syntax_score) / 2

    return {
        "output": output,
        "test_case": test_case,
        "model_score": model_score,
        "syntax_score": syntax_score,
        "score": score,
        "reasoning": model_grade["reasoning"],
    }
```

형식이 조금이라도 틀리면 애플리케이션에서 바로 깨지는 작업이라면 syntax score의 가중치를 더 높여도 됩니다. 중요한 건 절대 점수 자체보다, 프롬프트를 바꾼 뒤 점수가 실제로 개선되는지 추적하는 거예요.

## 채점 방법 2 — 모델 기반 (Model-based grading)

정답이 **주관적이거나 여러 형태로 맞을 수 있을 때** 써요. 요약 품질, 톤, 유용성 같은 것. **별도의 Claude 호출**에 채점 기준(rubric)을 주고 점수를 매기게 해요.

```python
grade_prompt = f"""
다음 요약이 원문의 핵심을 정확히 담았는지 1~5점으로 평가하고 이유를 써줘.
원문: {source}
요약: {output}
"""
# Claude에게 채점을 시킴 → 점수 파싱
```

모델 기반 grader는 점수만 요청하면 중간 점수에 몰리는 경향이 있어요. 그래서 강의에서는 **strengths, weaknesses, reasoning, score**를 함께 요청하라고 설명합니다. 모델이 판단 근거를 먼저 정리하게 만들면 점수가 더 안정적이에요.

```python
def grade_by_model(test_case, output):
    eval_prompt = f"""
You are an expert code reviewer. Evaluate this AI-generated solution.

Task:
{test_case["task"]}

Solution:
{output}

Return a JSON object with:
- "strengths": 1-3 key strengths
- "weaknesses": 1-3 key areas for improvement
- "reasoning": concise explanation
- "score": number from 1 to 10
"""

    messages = []
    add_user_message(messages, eval_prompt)
    add_assistant_message(messages, "```json")
    eval_text = chat(messages, stop_sequences=["```"])
    return json.loads(eval_text.strip())
```

이 grader를 `run_test_case`에 연결하면 임시 점수 대신 실제 평가 결과를 저장할 수 있어요.

```python
def run_test_case(test_case):
    output = run_prompt(test_case)
    model_grade = grade_by_model(test_case, output)

    return {
        "output": output,
        "test_case": test_case,
        "score": model_grade["score"],
        "reasoning": model_grade["reasoning"],
    }
```

전체 점수는 평균으로 추적합니다.

```python
from statistics import mean

def run_eval(dataset):
    results = []

    for test_case in dataset:
        result = run_test_case(test_case)
        results.append(result)

    average_score = mean([result["score"] for result in results])
    print(f"Average score: {average_score}")

    return results
```

- 유연하지만 채점 자체가 비용·시간이 들고, rubric을 구체적으로 써야 일관돼요.
- 절대적인 진실이라기보다 **프롬프트 버전 간 비교를 위한 기준선**으로 보는 게 좋아요.

## 채점 방법 선택 기준

| 상황 | 채점 방법 |
|---|---|
| 정답이 명확 (라벨, 숫자, 형식) | **코드 기반** |
| 정답이 주관적 (품질, 톤, 유용성) | **모델 기반** |
| 최종 품질·뉘앙스 확인이 중요 | **사람 기반** |

## 정리

- eval은 프롬프트 개선을 **측정 가능**하게 만드는 체계예요.
- 워크플로우: 데이터셋 → 실행 → 채점 → 집계 → 수정 → 비교.
- 데이터셋은 직접 만들거나 Claude로 생성해 반복 실행 가능하게 저장해요.
- grader는 코드 기반, 모델 기반, 사람 기반으로 나눌 수 있어요.
- **명확한 정답은 코드 기반**, **주관적 품질은 모델 기반**, **최종 판단은 사람 기반** 채점으로.
