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

## 전체 흐름

```
1. 요청에 tools(도구 목록)를 함께 보냄
2. Claude가 tool_use 블록으로 "이 도구 써줘" 응답 (stop_reason == "tool_use")
3. 내 코드가 도구를 실행
4. 결과를 tool_result 블록으로 되돌려 보냄
5. Claude가 결과를 반영해 최종 답변 생성
```

## 도구 함수와 스키마

각 도구는 **이름 + 설명 + 입력 스키마(JSON Schema)**로 정의해요. 설명이 Claude가 "언제 이 도구를 쓸지" 판단하는 근거예요 — 자세히 쓰세요.

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

## 멀티턴 & 여러 도구

- Claude는 결과를 보고 **또 다른 도구**를 요청할 수 있어요 → `stop_reason == "end_turn"`이 될 때까지 반복하는 **에이전틱 루프**를 돌려요.
- 한 응답에 **여러 `tool_use` 블록**이 올 수 있어요(병렬). 이때는 결과를 **하나의 user 메시지에 모아서** 되돌려 보내야 해요.
- SDK의 **tool runner**(베타)를 쓰면 이 루프를 자동으로 처리해줘요.

## Fine-grained tool calling

도구 입력이 스트리밍으로 점진적으로 전달되는 방식이에요. 큰 입력을 만들 때 지연을 줄일 수 있어요.

## 서버 측 도구 (Anthropic이 실행)

내 코드가 실행하는 도구와 달리, **Anthropic 인프라에서 바로 실행**되는 도구도 있어요.

- **텍스트 편집 도구(text editor)**: 파일 보기/생성/편집. (클라이언트 측 — 실제 파일 조작은 내 코드가 구현)
- **웹 검색 도구(web search)**: 최신 정보를 검색해 인용과 함께 반환. `tools`에 선언만 하면 Claude가 알아서 질의·결과 처리.

```python
tools = [{"type": "web_search_20260209", "name": "web_search"}]
```

## 정리

- 도구 사용 = Claude가 **요청**, 내 코드가 **실행**, 결과를 `tool_result`로 반환.
- 도구는 이름+설명+스키마로 정의, `tool_use_id`를 맞춰 결과 연결.
- 여러 도구 결과는 **한 메시지에 모아서**, 반복은 에이전틱 루프(또는 tool runner)로.
- 웹 검색 등 **서버 측 도구**는 선언만 하면 Anthropic이 실행.
