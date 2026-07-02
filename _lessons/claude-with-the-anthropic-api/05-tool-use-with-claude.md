---
title: "Tool Use with Claude"
title_ko: "도구 사용(Tool Use)"
course: claude-with-the-anthropic-api
lesson: 5
---

## 학습 목표

- 도구 사용의 전체 흐름(정의 → 호출 → 실행 → 결과 반환) 이해하기
- 도구 스키마와 메시지 블록을 다루기
- 여러 도구·멀티턴·서버 측 도구(웹 검색, 텍스트 편집)를 활용하기

## 도구 사용이란?

Claude는 텍스트만 생성해요. **도구(tool)**를 주면 "이 도구를 이 인자로 써줘"라고 **요청**할 수 있어요. 실제 실행은 **여러분의 코드**가 해요. 이걸로 Claude가 날씨 조회, DB 검색, 계산 등 외부 능력을 쓸 수 있게 돼요.

기본 상태의 Claude는 학습 데이터에 들어 있던 지식으로 답합니다. 그래서 현재 날씨, 최신 주가, 사내 DB, 사용자 계정 상태처럼 **실시간 데이터나 외부 시스템**에는 접근할 수 없어요. 사용자가 "지금 샌프란시스코 날씨가 어때?"라고 물으면, 도구가 없는 Claude는 최신 날씨를 알 수 없다고 답해야 합니다.

도구 사용은 이 한계를 해결하는 구조화된 방식이에요. Claude가 직접 인터넷이나 DB에 접근하는 것이 아니라, **필요한 정보를 요청하고, 애플리케이션 서버가 그 요청을 실행한 뒤, 결과를 Claude에게 다시 전달**합니다.

## 도구가 없을 때 생기는 문제

Claude가 아무 도구 없이 현재 정보를 묻는 질문을 받으면 답변이 막힙니다.

```text
사용자: What's the weather in San Francisco, California?
Claude: I don't have access to up-to-date weather information.
```

Claude가 날씨를 설명하는 능력은 있어도, 지금 이 순간의 날씨 데이터는 학습 데이터에 없기 때문이에요. 사용자는 Claude가 도와줄 수 있을 것처럼 기대하지만, 실제로는 최신 데이터 접근 경로가 없어서 경험이 끊깁니다.

## 전체 흐름

```
1. 요청에 tools(도구 목록)를 함께 보냄
2. Claude가 tool_use 블록으로 "이 도구 써줘" 응답 (stop_reason == "tool_use")
3. 내 코드가 도구를 실행
4. 결과를 tool_result 블록으로 되돌려 보냄
5. Claude가 결과를 반영해 최종 답변 생성
```

이 흐름을 더 풀어쓰면 네 단계입니다.

1. **Initial request**: 사용자 질문과 함께 사용할 수 있는 도구 목록/설명을 Claude에게 보냄
2. **Tool request**: Claude가 질문을 분석하고 추가 정보가 필요하다고 판단하면 특정 도구 호출을 요청
3. **Data retrieval**: 내 서버가 외부 API, DB, 파일 시스템 등에서 실제 데이터를 가져옴
4. **Final response**: 도구 결과를 Claude에게 다시 보내고, Claude가 신선한 데이터를 바탕으로 최종 답변 생성

날씨 예시라면 사용자가 현재 날씨를 묻고, Claude는 `get_weather` 도구가 필요하다고 판단해 `location="San Francisco, California"` 같은 인자를 요청합니다. 서버는 실제 weather API를 호출해 현재 기온과 상태를 가져오고, 그 결과를 Claude에게 전달합니다. Claude는 원래 질문과 최신 날씨 데이터를 합쳐 사용자가 읽기 좋은 답변을 만듭니다.

도구 사용의 장점은 명확해요.

- **실시간 정보**: 학습 시점 이후의 최신 데이터 사용
- **외부 시스템 연결**: API, 데이터베이스, 내부 서비스와 연동
- **동적 응답**: 매 요청마다 최신 상태를 반영
- **구조화된 상호작용**: Claude가 어떤 정보가 필요한지 명확한 인자로 요청

## 프로젝트 개요: 리마인더 설정하기

강의에서는 도구 사용을 배우기 위한 실습 프로젝트로 **자연어 리마인더 설정 시스템**을 만듭니다.

목표는 사용자가 이렇게 말할 수 있게 하는 거예요.

```text
Set a reminder for my doctor's appointment. It's a week from Thursday.
```

Claude는 이를 이해하고 정확한 날짜/시간을 계산한 뒤, 시스템에 리마인더를 등록하고 다음처럼 답해야 합니다.

```text
OK, I will remind you.
```

겉보기에는 간단하지만, 실제로는 Claude의 기본 능력만으로 처리하기 어려운 부분이 있습니다.

### 왜 어려운가?

리마인더를 제대로 설정하려면 세 가지 문제가 해결되어야 해요.

- **제한적인 시간 인식**: Claude가 현재 날짜를 알더라도 정확한 현재 시각까지 항상 안정적으로 아는 것은 아님
- **날짜 계산 문제**: "다음 주 목요일", "3주 뒤", "내일 오후 2시" 같은 계산은 실수할 수 있음
- **리마인더 실행 능력 부재**: Claude에게는 실제 시스템에 리마인더를 저장하는 내장 기능이 없음

이런 제한은 프롬프트를 더 길게 쓴다고 완전히 해결되지 않습니다. 대신 각 한계를 **도구로 보완**합니다.

### 필요한 도구 세 가지

이 프로젝트에서는 세 가지 도구를 하나씩 만듭니다.

| 도구 | 역할 |
|---|---|
| `get_current_datetime` | 현재 날짜와 시간을 정확히 가져오기 |
| `add_duration_to_datetime` | 기준 시각에 기간을 더해 정확한 미래 시각 계산 |
| `set_reminder` | 계산된 시각과 내용을 실제 리마인더로 저장 |

예를 들어 "a week from Thursday"라는 요청을 받으면 Claude는 먼저 현재 시간을 확인하고, 날짜 계산 도구로 정확한 미래 시각을 구한 뒤, 마지막으로 리마인더 설정 도구를 호출합니다.

이 프로젝트의 핵심 원칙은 명확해요. 모델이 약한 일을 프롬프트로 억지로 해결하려 하지 말고, **모델이 판단하고 도구가 정확히 실행**하게 만드는 것입니다.

## 도구 함수와 스키마

각 도구는 **이름 + 설명 + 입력 스키마(JSON Schema)**로 정의해요. 설명이 Claude가 "언제 이 도구를 쓸지" 판단하는 근거예요 — 자세히 쓰세요.

### 도구 함수란?

도구 함수는 Claude가 직접 실행하는 코드가 아니라, **Claude의 요청을 받은 내 애플리케이션이 실행하는 Python 함수**예요. Claude는 "현재 시간이 필요하다", "날씨 데이터가 필요하다"처럼 판단하고 도구 호출을 요청합니다. 그러면 서버 코드가 해당 함수를 실행하고 결과를 Claude에게 되돌려줍니다.

도구 함수를 작성할 때는 일반 함수보다 더 명확하게 만들어야 해요.

- **이름을 구체적으로 쓰기**: 함수명과 파라미터명이 역할을 설명해야 함
- **입력 검증하기**: 빈 값, 잘못된 형식, 허용되지 않는 값은 바로 에러 처리
- **의미 있는 에러 메시지 제공**: Claude가 에러를 보고 올바른 인자로 다시 호출할 수 있음

예를 들어 `"Location cannot be empty"` 같은 에러 메시지는 Claude가 다음 호출에서 location을 제대로 채우도록 돕습니다.

### 첫 번째 도구 함수: 현재 날짜/시간 가져오기

리마인더 프로젝트의 첫 도구는 현재 날짜와 시간을 반환하는 함수입니다.

```python
from datetime import datetime

def get_current_datetime(date_format="%Y-%m-%d %H:%M:%S"):
    if not date_format:
        raise ValueError("date_format cannot be empty")

    return datetime.now().strftime(date_format)
```

기본값은 `2024-01-15 14:30:25`처럼 연-월-일 시:분:초 형식입니다. Claude가 필요한 형식이 다르면 `date_format`을 바꿔 요청할 수 있어요.

```python
get_current_datetime()
# "2024-01-15 14:30:25"

get_current_datetime("%H:%M")
# "14:30"
```

여기서 중요한 점은 함수 자체보다 패턴입니다. **명확한 함수명, 검증 가능한 인자, 실패 시 이해 가능한 에러**를 갖춘 작은 함수를 만들고, 다음 단계에서 이 함수를 Claude가 이해할 수 있도록 JSON Schema로 설명합니다.

### 도구 스키마란?

함수만 만들어서는 Claude가 그 함수를 어떻게 호출해야 하는지 알 수 없어요. 그래서 **도구 스키마(tool schema)**로 함수의 이름, 설명, 입력 인자를 알려줍니다.

스키마는 크게 세 부분으로 구성됩니다.

| 필드 | 역할 |
|---|---|
| `name` | Claude가 호출할 도구 이름 |
| `description` | 도구가 무엇을 하고 언제 써야 하는지 설명 |
| `input_schema` | 함수 인자 구조를 JSON Schema로 설명 |

JSON Schema는 AI 전용 기술이 아니라 오래전부터 쓰이던 데이터 검증 규격이에요. Claude tool use에서는 함수 인자를 명확하게 설명하고 검증하기 좋은 형식이라 사용합니다.

좋은 description은 중요합니다. Claude는 이 설명을 보고 도구를 쓸지 말지 판단해요.

- 도구가 무엇을 하는지 3-4문장 정도로 설명
- 언제 사용해야 하는지 명시
- 어떤 데이터를 반환하는지 설명
- 각 인자의 의미와 형식을 자세히 적기

### `get_current_datetime` 스키마 예시

```python
get_current_datetime_schema = {
    "name": "get_current_datetime",
    "description": (
        "Returns the current date and time formatted according to the specified format. "
        "Use this tool when the user asks about the current time or when another tool "
        "requires an exact current datetime as a starting point."
    ),
    "input_schema": {
        "type": "object",
        "properties": {
            "date_format": {
                "type": "string",
                "description": (
                    "Python strftime format string used to format the returned datetime. "
                    "Defaults to year-month-day hour:minute:second."
                ),
                "default": "%Y-%m-%d %H:%M:%S",
            }
        },
        "required": [],
    },
}
```

이름은 `function_name`과 `function_name_schema`를 맞춰두면 관리하기 쉽습니다.

```python
from anthropic.types import ToolParam

get_current_datetime_schema = ToolParam(get_current_datetime_schema)
```

`ToolParam`은 필수는 아니지만, Python 타입 체크와 SDK 사용 시 실수를 줄이는 데 도움이 됩니다.

직접 스키마를 쓰기 어렵다면 Claude에게 함수 코드와 Anthropic tool use 문서를 함께 주고 "이 함수에 맞는 tool calling JSON schema를 작성해줘"라고 요청해도 됩니다. 다만 생성된 스키마의 description과 required 필드는 반드시 직접 검토하세요.

### 날씨 도구 스키마 예시

```python
tools = [{
    "name": "get_weather",
    "description": "특정 도시의 현재 날씨를 조회한다. 날씨/기온을 물으면 사용.",
    "input_schema": {
        "type": "object",
        "properties": {
            "location": {"type": "string", "description": "도시 이름"},
        },
        "required": ["location"],
    },
}]
```

## 메시지 블록 다루기 & 결과 반환

응답 `content`에서 `tool_use` 블록을 찾아 실행하고, 결과를 `tool_result`로 보내요. 이때 **`tool_use_id`를 정확히 맞춰야** 해요.

도구를 사용할 수 있게 하려면 API 호출에 `tools` 파라미터를 넣습니다.

```python
messages = []
messages.append({
    "role": "user",
    "content": "What is the exact time, formatted as HH:MM:SS?",
})

response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1000,
    messages=messages,
    tools=[get_current_datetime_schema],
)
```

도구가 없을 때는 응답이 단순한 텍스트 블록 하나인 경우가 많았지만, 도구를 쓰면 Claude의 assistant 메시지가 **여러 content block**을 가질 수 있어요.

- **text block**: "현재 시간을 확인해볼게요"처럼 사람이 읽는 설명
- **tool_use block**: 내 코드가 실행해야 할 도구 이름과 인자

`tool_use` 블록에는 보통 다음 정보가 들어 있습니다.

- `id`: 이 도구 호출을 추적하는 식별자
- `name`: 호출할 도구 이름
- `input`: 함수 인자 딕셔너리
- `type`: `"tool_use"`

멀티턴 대화에서는 이 구조를 그대로 보존해야 합니다. Claude는 서버에 대화 상태를 저장하지 않으므로, 다음 요청을 보낼 때 이전 assistant 메시지의 `response.content` 전체를 history에 넣어야 해요.

```python
messages.append({
    "role": "assistant",
    "content": response.content,
})
```

텍스트만 꺼내서 저장하면 `tool_use` 블록이 사라지고, 다음 단계에서 Claude가 어떤 도구 호출 결과를 기다리고 있었는지 알 수 없게 됩니다.

```python
response = client.messages.create(
    model="claude-opus-4-8", max_tokens=1024,
    tools=tools,
    messages=[{"role": "user", "content": "파리 날씨 어때?"}],
)

for block in response.content:
    if block.type == "tool_use":
        result = get_weather(block.input["location"])   # 내 코드가 실행
        followup = client.messages.create(
            model="claude-opus-4-8", max_tokens=1024, tools=tools,
            messages=[
                {"role": "user", "content": "파리 날씨 어때?"},
                {"role": "assistant", "content": response.content},   # tool_use 포함
                {"role": "user", "content": [
                    {"type": "tool_result", "tool_use_id": block.id, "content": result},
                ]},
            ],
        )
```

전체 흐름은 다음과 같습니다.

1. 사용자 메시지와 tool schema를 Claude에게 보냄
2. Claude가 text block + tool_use block이 포함된 assistant 메시지를 반환
3. 내 코드가 tool_use block에서 도구 이름과 입력값을 읽고 실제 함수 실행
4. 실행 결과를 `tool_result` block으로 Claude에게 다시 보냄
5. Claude가 도구 결과를 바탕으로 최종 답변 생성

기존에 `add_user_message()`, `add_assistant_message()` 같은 헬퍼를 사용했다면, 이제 문자열뿐 아니라 content block 리스트도 저장할 수 있게 바꿔야 합니다. 도구 사용에서는 메시지 구조를 보존하는 것이 기능의 일부입니다.

### 도구 결과 보내기

Claude가 `tool_use` 블록을 반환하면, 내 코드는 해당 블록의 `input`을 꺼내 실제 함수를 호출합니다.

```python
tool_request = response.content[1]
tool_output = get_current_datetime(**tool_request.input)
```

`input`은 딕셔너리이므로 Python의 `**` unpacking으로 키워드 인자처럼 넘길 수 있어요.

함수 실행 결과는 `tool_result` 블록으로 감싸서 **user 메시지**에 넣어 보냅니다.

```python
messages.append({
    "role": "user",
    "content": [{
        "type": "tool_result",
        "tool_use_id": tool_request.id,
        "content": tool_output,
        "is_error": False,
    }],
})
```

여기서 `tool_use_id`는 반드시 원래 `tool_use` 블록의 `id`와 같아야 합니다. Claude가 여러 도구를 한 번에 요청할 수도 있기 때문에, 이 ID가 있어야 어떤 결과가 어떤 요청에 대응되는지 알 수 있어요.

후속 요청에도 `tools` 스키마를 계속 포함해야 합니다. 지금은 새 도구 호출을 기대하지 않더라도, Claude가 대화 기록 안의 `tool_use`/`tool_result` 참조를 이해하는 데 스키마가 필요합니다.

```python
final_response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1000,
    messages=messages,
    tools=[get_current_datetime_schema],
)
```

도구가 실패했을 때도 결과 블록은 보내야 합니다.

```python
{
    "type": "tool_result",
    "tool_use_id": tool_request.id,
    "content": "Error: date_format cannot be empty",
    "is_error": True,
}
```

Claude는 에러 메시지를 보고 인자를 고쳐 다시 호출하거나, 사용자에게 문제를 설명할 수 있습니다.

## 멀티턴 & 여러 도구

- Claude는 결과를 보고 **또 다른 도구**를 요청할 수 있어요 → `stop_reason == "end_turn"`이 될 때까지 반복하는 **에이전틱 루프**를 돌려요.
- 한 응답에 **여러 `tool_use` 블록**이 올 수 있어요(병렬). 이때는 결과를 **하나의 user 메시지에 모아서** 되돌려 보내야 해요.
- SDK의 **tool runner**(베타)를 쓰면 이 루프를 자동으로 처리해줘요.

예를 들어 사용자가 "오늘부터 103일 뒤가 무슨 요일이야?"라고 묻는다면 Claude는 한 번의 도구 호출로 끝내지 못할 수 있어요.

1. `get_current_datetime`으로 현재 날짜를 가져옴
2. 그 결과를 보고 `add_duration_to_datetime`으로 103일을 더함
3. 계산 결과를 바탕으로 최종 답변 생성

이런 경우 애플리케이션은 Claude가 더 이상 도구를 요청하지 않을 때까지 루프를 돌아야 합니다. 도구 요청 여부는 `response.stop_reason == "tool_use"`로 확인할 수 있어요.

```python
def run_conversation(messages, tools):
    while True:
        response = chat(messages, tools=tools)
        add_assistant_message(messages, response)

        if response.stop_reason != "tool_use":
            break

        tool_result_blocks = run_tools(response)
        add_user_message(messages, tool_result_blocks)

    return messages
```

### 헬퍼 함수 업데이트

도구를 쓰기 시작하면 `chat()`이 텍스트만 반환하면 부족합니다. 전체 message 객체를 반환해야 `content` 블록과 `stop_reason`을 모두 다룰 수 있어요.

```python
def chat(messages, system=None, temperature=1.0, stop_sequences=None, tools=None):
    params = {
        "model": "claude-opus-4-8",
        "max_tokens": 1000,
        "messages": messages,
    }

    if temperature is not None:
        params["temperature"] = temperature
    if stop_sequences:
        params["stop_sequences"] = stop_sequences
    if tools:
        params["tools"] = tools
    if system:
        params["system"] = system

    return client.messages.create(**params)
```

메시지 추가 헬퍼도 문자열뿐 아니라 message 객체나 block 리스트를 받을 수 있어야 합니다.

```python
from anthropic.types import Message

def add_user_message(messages, message):
    messages.append({
        "role": "user",
        "content": message.content if isinstance(message, Message) else message,
    })

def add_assistant_message(messages, message):
    messages.append({
        "role": "assistant",
        "content": message.content if isinstance(message, Message) else message,
    })
```

최종 답변을 화면에 보여줄 때는 text block만 모으면 됩니다.

```python
def text_from_message(message):
    return "\n".join(
        block.text for block in message.content if block.type == "text"
    )
```

### 여러 도구 요청 처리

Claude는 한 응답에서 여러 `tool_use` 블록을 요청할 수 있습니다. 모든 요청을 실행하고, 대응되는 `tool_result` 블록들을 하나의 user 메시지로 모아 보내야 해요.

```python
def run_tools(message):
    tool_requests = [
        block for block in message.content if block.type == "tool_use"
    ]
    tool_result_blocks = []

    for tool_request in tool_requests:
        try:
            tool_output = run_tool(tool_request.name, tool_request.input)
            tool_result_blocks.append({
                "type": "tool_result",
                "tool_use_id": tool_request.id,
                "content": json.dumps(tool_output),
                "is_error": False,
            })
        except Exception as e:
            tool_result_blocks.append({
                "type": "tool_result",
                "tool_use_id": tool_request.id,
                "content": f"Error: {e}",
                "is_error": True,
            })

    return tool_result_blocks
```

도구 이름과 실제 함수는 별도 라우터에서 연결합니다.

```python
def run_tool(tool_name, tool_input):
    if tool_name == "get_current_datetime":
        return get_current_datetime(**tool_input)
    if tool_name == "add_duration_to_datetime":
        return add_duration_to_datetime(**tool_input)
    if tool_name == "set_reminder":
        return set_reminder(**tool_input)

    raise ValueError(f"Unknown tool: {tool_name}")
```

이렇게 하면 새 도구를 추가할 때 대화 루프는 건드리지 않고 라우팅 함수만 확장하면 됩니다.

### 여러 도구를 대화에 추가하기

리마인더 시스템에는 세 도구가 모두 필요합니다.

- `get_current_datetime`: 현재 날짜/시간 확인
- `add_duration_to_datetime`: 기준 시각에 기간을 더해 미래 시각 계산
- `set_reminder`: 계산된 시각에 리마인더 저장

핵심 인프라가 준비되면 여러 도구를 추가하는 일은 단순합니다. `chat()` 호출에 넘기는 `tools` 목록에 스키마를 모두 넣고, `run_tool()` 라우터에 함수 연결만 추가하면 됩니다.

```python
tools = [
    get_current_datetime_schema,
    add_duration_to_datetime_schema,
    set_reminder_schema,
]

messages = run_conversation(messages, tools=tools)
```

또는 루프 안에서 직접 호출한다면 다음처럼 세 스키마를 모두 전달합니다.

```python
response = chat(
    messages,
    tools=[
        get_current_datetime_schema,
        add_duration_to_datetime_schema,
        set_reminder_schema,
    ],
)
```

라우터는 도구 이름을 실제 함수에 연결합니다.

```python
def run_tool(tool_name, tool_input):
    if tool_name == "get_current_datetime":
        return get_current_datetime(**tool_input)
    elif tool_name == "add_duration_to_datetime":
        return add_duration_to_datetime(**tool_input)
    elif tool_name == "set_reminder":
        return set_reminder(**tool_input)

    raise ValueError(f"Unknown tool: {tool_name}")
```

테스트는 여러 도구가 필요한 요청으로 하는 것이 좋아요.

```text
Set a reminder for my doctor's appointment.
It's 177 days after Jan 1st, 2050.
```

이 요청은 Claude가 적어도 두 단계를 거치게 만듭니다.

1. `add_duration_to_datetime`으로 `2050-01-01 + 177 days`를 계산
2. `set_reminder`로 계산된 날짜에 리마인더 저장

대화 기록을 보면 user 메시지, assistant의 text/tool_use 블록, tool_result 메시지, 후속 assistant 메시지가 순서대로 쌓입니다. Claude는 이 히스토리를 보고 이전 도구 결과를 바탕으로 다음 도구를 호출하거나 최종 답변을 만듭니다.

새 도구를 추가할 때의 패턴은 항상 같습니다.

1. 도구 함수 구현
2. 도구 스키마 작성
3. `tools` 목록에 스키마 추가
4. `run_tool()` 라우터에 함수 연결

이 구조를 유지하면 도구가 늘어나도 대화 루프와 메시지 처리 코드는 그대로 둘 수 있습니다.

## Fine-grained tool calling

도구 사용과 streaming을 함께 쓰면 Claude가 도구 인자를 생성하는 과정을 실시간으로 받을 수 있습니다. 일반 텍스트 스트리밍에서 `ContentBlockDelta`를 처리하듯, 도구 인자 스트리밍에서는 `InputJsonEvent`를 처리해야 해요.

`InputJsonEvent`에는 핵심 값이 두 개 있습니다.

- `partial_json`: 이번에 새로 도착한 JSON 조각
- `snapshot`: 지금까지 누적된 전체 JSON 문자열

```python
for chunk in stream:
    if chunk.type == "input_json":
        print(chunk.partial_json)
        current_args = chunk.snapshot
```

여기서 중요한 점은 기본 streaming이 **Claude가 만든 모든 조각을 즉시 보내는 방식이 아니라는 것**입니다. Anthropic API는 기본적으로 도구 인자 JSON을 어느 정도 버퍼링하고 검증한 뒤 보냅니다.

예를 들어 도구 인자가 이런 구조라고 해봅시다.

```json
{
  "abstract": "This paper presents a novel...",
  "meta": {
    "word_count": 847,
    "review": "This paper introduces QuanNet..."
  }
}
```

기본 동작에서는 API가 최상위 key-value 쌍이 완성될 때까지 기다립니다. `abstract` 값 전체가 완성되면 스키마에 맞는지 검증하고, 그동안 쌓아둔 chunk를 한꺼번에 보냅니다. 그다음 `meta` 객체도 같은 방식으로 처리해요.

그래서 streaming을 켰는데도 **잠깐 멈췄다가 한 번에 많이 도착하는 느낌**이 날 수 있습니다. Claude가 느린 것이 아니라, API가 유효한 JSON 조각 단위로 버퍼링하고 있기 때문입니다.

### `fine_grained=True`

더 빠른 실시간 업데이트가 필요하다면 fine-grained tool calling을 켤 수 있습니다.

```python
run_conversation(
    messages,
    tools=[save_article_schema],
    fine_grained=True,
)
```

이 옵션의 핵심은 **API 쪽 JSON 검증을 끄는 것**입니다.

- Claude가 생성하는 chunk를 더 빨리 받음
- 최상위 key-value 완성까지 기다리는 버퍼링 지연이 줄어듦
- 사용자는 더 자연스러운 실시간 진행 상황을 볼 수 있음
- 대신 애플리케이션 코드가 잘못된 JSON을 직접 처리해야 함

예를 들어 fine-grained 모드에서는 `meta` 객체 전체가 끝나기 전에도 `word_count` 일부를 받을 수 있습니다. 반응성은 좋아지지만, 중간 `snapshot`이 아직 파싱 가능한 JSON이 아닐 수 있어요.

```python
try:
    parsed_args = json.loads(chunk.snapshot)
except json.JSONDecodeError:
    print("Received invalid JSON, continuing...")
```

Claude가 `"word_count": undefined`처럼 JSON으로는 유효하지 않은 값을 만들 수도 있습니다. 기본 모드라면 API 검증이 이런 문제를 막거나 문자열로 감싸려고 시도하지만, fine-grained 모드에서는 그 보호막이 줄어듭니다.

따라서 fine-grained tool calling은 다음 상황에서만 쓰는 것이 좋아요.

- 도구 인자 생성 과정을 사용자에게 실시간으로 보여줘야 할 때
- 긴 도구 입력을 만들고 있어 기본 버퍼링 지연이 눈에 띌 때
- partial result를 빨리 받아 후속 처리를 시작해야 할 때
- invalid JSON, 중간 JSON, 재시도 처리를 직접 구현할 수 있을 때

대부분의 애플리케이션은 기본 검증 모드로 충분합니다. fine-grained는 안정성보다 반응성이 더 중요한 경우에 선택하는 옵션입니다.

## 텍스트 편집 도구

Claude에는 직접 스키마를 처음부터 작성하지 않아도 되는 built-in text editor tool이 있습니다. 이 도구는 Claude가 파일과 디렉터리를 텍스트 에디터처럼 다룰 수 있게 해줍니다.

할 수 있는 일은 꽤 넓습니다.

- 파일 또는 디렉터리 내용 보기
- 파일의 특정 줄 범위 보기
- 파일 안의 문자열 교체
- 새 파일 생성
- 특정 줄에 텍스트 삽입
- 최근 파일 편집 되돌리기

다만 이름 때문에 오해하기 쉬운 부분이 있어요. **Claude가 파일 시스템을 직접 조작하는 것은 아닙니다.** text editor tool의 스키마와 사용 방식은 Claude가 알고 있지만, 실제 파일 읽기/쓰기/수정은 여러분의 애플리케이션 코드가 구현해야 합니다.

일반 도구에서는 두 가지를 모두 직접 준비합니다.

1. Claude에게 보여줄 JSON Schema
2. 실제로 실행될 함수 구현

text editor tool은 1번의 상당 부분이 Claude에 내장되어 있습니다. 대신 2번, 즉 파일을 열고 수정하는 실제 실행 로직은 여전히 내가 작성해야 합니다.

### 모델별 도구 버전

요청에는 작은 schema stub만 넣습니다. Claude는 이 stub을 보고 내부적으로 전체 text editor tool 스펙을 이해합니다. 도구 버전 문자열은 모델에 따라 달라질 수 있으므로, 실제 프로젝트에서는 Anthropic 문서의 최신 버전을 확인해야 합니다.

```python
def get_text_edit_schema(model):
    if model.startswith("claude-3-7-sonnet"):
        return {
            "type": "text_editor_20250124",
            "name": "str_replace_editor",
        }
    elif model.startswith("claude-3-5-sonnet"):
        return {
            "type": "text_editor_20241022",
            "name": "str_replace_editor",
        }
```

사용자는 다음처럼 요청할 수 있습니다.

```text
Open the ./main.py file and summarize its contents.
```

Claude는 text editor tool로 파일 보기를 요청하고, 애플리케이션은 실제 파일 내용을 읽어 `tool_result`로 돌려줍니다. Claude는 그 결과를 바탕으로 요약을 작성합니다.

수정 요청도 가능합니다.

```text
Open the ./main.py file and write out a function to calculate pi to the 5th digit.
Then create a ./test.py file to test your implementation.
```

이 경우 Claude는 기존 파일을 보고, 필요한 내용을 교체하거나 새 파일 생성을 요청할 수 있습니다. 즉, 내 애플리케이션 안에 Claude 기반 코드 편집 경험을 넣을 수 있어요.

text editor tool이 특히 유용한 경우는 다음과 같습니다.

- 자체 앱 안에서 파일 편집 기능을 제공해야 할 때
- 코드 에디터가 없는 환경에서 Claude에게 파일 작업을 맡기고 싶을 때
- AI 코드 편집 흐름을 직접 통제하고 싶을 때

핵심은 권한과 안전장치입니다. 파일 시스템을 실제로 건드리는 코드는 내 서버에서 실행되므로, 허용 경로, 되돌리기, diff 표시, 사용자 승인 같은 안전장치를 같이 설계해야 합니다.

## 웹 검색 도구

웹 검색 도구는 Claude가 최신 정보나 전문 정보를 찾기 위해 인터넷 검색을 사용할 수 있게 해주는 built-in tool입니다. 조직 설정에서 Web Search tool 사용이 활성화되어 있어야 하고, 다른 클라이언트 도구와 달리 **검색 실행 자체는 Anthropic이 처리**합니다.

즉, 여러분이 검색 API를 직접 붙이거나 검색 결과 파서를 구현할 필요는 없습니다. 간단한 schema를 `tools`에 넣으면 Claude가 필요할 때 검색하고, 결과와 인용을 이용해 답변합니다.

```python
web_search_schema = {
    "type": "web_search_20250305",
    "name": "web_search",
    "max_uses": 5,
}
```

`max_uses`는 Claude가 수행할 수 있는 검색 횟수를 제한합니다. 한 번 검색해도 여러 결과가 오지만, Claude가 후속 검색이 필요하다고 판단할 수 있으므로 비용과 지연을 제어하려면 제한을 두는 편이 좋습니다.

### 응답 블록 구조

Claude가 웹 검색을 사용하면 응답에는 여러 종류의 블록이 섞일 수 있습니다.

- **text block**: Claude의 설명 또는 최종 답변
- **ServerToolUseBlock**: Claude가 사용한 실제 검색 질의
- **WebSearchToolResultBlock**: 검색 결과 묶음
- **WebSearchResultBlock**: 개별 검색 결과의 제목과 URL
- **citation block**: Claude 답변의 특정 문장을 뒷받침하는 출처 정보

이 구조 덕분에 UI에서 "Claude가 무엇을 검색했는지", "어떤 출처를 사용했는지", "어느 문장이 어떤 자료에 근거하는지"를 보여줄 수 있습니다.

### 검색 도메인 제한

특정 도메인만 검색하게 제한할 수도 있습니다.

```python
web_search_schema = {
    "type": "web_search_20250305",
    "name": "web_search",
    "max_uses": 5,
    "allowed_domains": ["nih.gov"],
}
```

의학, 건강, 연구처럼 출처 품질이 중요한 주제에서는 임의의 블로그보다 `nih.gov` 같은 신뢰 가능한 도메인으로 제한하는 편이 낫습니다. 반대로 최신 뉴스나 제품 정보처럼 넓은 탐색이 필요한 경우에는 제한을 줄이거나 두지 않을 수 있어요.

### UI에 렌더링할 때

웹 검색 응답은 단순 텍스트로만 다루기보다 블록 타입에 맞춰 보여주는 것이 좋습니다.

- text block은 일반 답변으로 렌더링
- 검색 결과는 출처 목록으로 표시
- citation은 답변 문장 옆에 도메인, 제목, URL, 인용 텍스트와 함께 표시

이렇게 하면 사용자가 Claude의 답을 그대로 믿는 것이 아니라, 어떤 자료에 근거했는지 확인할 수 있습니다. 웹 검색 도구의 가치는 "최신 정보 접근"뿐 아니라 **출처 투명성**에도 있습니다.

웹 검색 도구가 잘 맞는 작업은 다음과 같습니다.

- 최신 사건이나 최근 변경 사항 확인
- Claude 학습 데이터에 없을 수 있는 전문 정보 검색
- 사실 확인 및 출처 찾기
- 근거가 필요한 리서치 작업

정리하면, text editor tool은 built-in schema를 쓰지만 실제 파일 조작은 내 코드가 실행하는 도구이고, web search tool은 선언만 하면 Anthropic이 검색 실행과 결과 처리를 맡는 서버 측 도구입니다.

## 정리

- 도구 사용 = Claude가 **요청**, 내 코드가 **실행**, 결과를 `tool_result`로 반환.
- 도구는 이름+설명+스키마로 정의, `tool_use_id`를 맞춰 결과 연결.
- 여러 도구 결과는 **한 메시지에 모아서**, 반복은 에이전틱 루프(또는 tool runner)로.
- fine-grained tool calling은 반응성을 높이지만 JSON 검증 책임이 애플리케이션으로 넘어옴.
- text editor tool은 내 앱이 실제 파일 작업을 구현하고, web search tool은 Anthropic이 검색을 실행함.
