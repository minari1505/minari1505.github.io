---
title: "Accessing Claude with the API"
title_ko: "API로 Claude 사용하기"
course: claude-with-the-anthropic-api
lesson: 2
---

## 학습 목표

- API 키를 발급받고 첫 요청을 보내기
- 멀티턴 대화·시스템 프롬프트·temperature를 다루기
- 응답 스트리밍과 구조화된 데이터(JSON) 출력을 구현하기

## API 키 발급과 첫 요청

1. Anthropic Console에서 **API 키**를 발급받아요.
2. 키는 코드에 하드코딩하지 말고 **환경 변수** `ANTHROPIC_API_KEY`에 넣어요.

```python
import anthropic

client = anthropic.Anthropic()  # ANTHROPIC_API_KEY 환경 변수를 자동으로 읽음

response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    messages=[{"role": "user", "content": "프랑스의 수도는?"}],
)
print(response.content[0].text)
```

- `model`, `max_tokens`, `messages`는 필수예요.
- 응답의 `content`는 **블록들의 리스트**예요. `block.type`을 확인하고 `text`를 꺼내야 해요.

## 멀티턴 대화

**API는 상태를 저장하지 않아요(stateless).** 매 요청마다 **전체 대화 기록**을 다시 보내야 Claude가 맥락을 기억해요.

```python
messages = [
    {"role": "user", "content": "내 이름은 앨리스야."},
    {"role": "assistant", "content": "안녕하세요 앨리스님!"},
    {"role": "user", "content": "내 이름이 뭐라고 했지?"},  # 이전 맥락 필요
]
```

- 첫 메시지는 반드시 `user`로 시작해요.
- `user`와 `assistant`가 번갈아 나와요 (같은 역할 연속도 허용되며 하나의 턴으로 합쳐짐).

## 시스템 프롬프트

`system` 파라미터로 Claude의 **역할·행동 방식**을 지정해요. 대화 메시지와 별개예요.

```python
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    system="너는 친절한 코딩 튜터야. 항상 Python 예시를 함께 줘.",
    messages=[{"role": "user", "content": "JSON 파일 어떻게 읽어?"}],
)
```

## Temperature

`temperature`는 출력의 **무작위성**을 조절해요 (0에 가까울수록 결정적, 높을수록 다양). 사실 추출·분류엔 낮게, 창의적 생성엔 높게.

> ⚠️ 최신 모델(예: Opus 4.8)에서는 `temperature`가 제거되어 프롬프트로 행동을 유도해요. 강의는 이 개념을 소개하지만, 최신 모델에선 프롬프트 지시로 대체한다고 알아두면 돼요.

## 응답 스트리밍

긴 응답은 완성될 때까지 기다리지 말고 **토큰 단위로 실시간** 받아요. 사용자 경험이 좋아지고, 큰 `max_tokens`일 때 HTTP 타임아웃도 피할 수 있어요.

```python
with client.messages.stream(
    model="claude-opus-4-8",
    max_tokens=1024,
    messages=[{"role": "user", "content": "짧은 이야기 하나 써줘"}],
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)
```

## 구조화된 데이터 (JSON 출력)

애플리케이션에 넣으려면 자유 텍스트가 아니라 **정해진 스키마의 JSON**이 필요할 때가 많아요. `output_config`로 출력 형식을 강제할 수 있어요.

```python
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    messages=[{"role": "user", "content": "존 스미스, john@x.com, Enterprise 플랜"}],
    output_config={
        "format": {
            "type": "json_schema",
            "schema": {
                "type": "object",
                "properties": {
                    "name": {"type": "string"},
                    "email": {"type": "string"},
                    "plan": {"type": "string"},
                },
                "required": ["name", "email", "plan"],
                "additionalProperties": False,
            },
        }
    },
)
```

- Python SDK의 `client.messages.parse()`에 Pydantic 모델을 넘기면 검증까지 자동으로 해줘요.

## 정리

- 키는 환경 변수로, 첫 요청은 `messages.create(model, max_tokens, messages)`.
- API는 stateless → 멀티턴은 **전체 기록을 매번 재전송**.
- `system`으로 역할 지정, 긴 출력은 **스트리밍**, 구조화가 필요하면 **`output_config.format`**.
