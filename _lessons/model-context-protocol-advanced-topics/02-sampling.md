---
title: "Sampling"
title_ko: "Sampling"
course: model-context-protocol-advanced-topics
lesson: 2
---

## 학습 목표

- Sampling이 무엇이고 어떤 문제를 푸는지 이해하기
- Sampling의 요청 흐름과 이점 파악하기
- 서버·클라이언트 양쪽 구현 방식 익히기

## Sampling이란

**Sampling**은 서버가 연결된 MCP 클라이언트를 통해 Claude 같은 언어 모델에 접근하게 해줍니다. 서버가 Claude를 직접 호출하는 대신, **클라이언트에게 대신 호출해달라고 요청**합니다. 이로써 텍스트 생성의 책임과 비용이 서버에서 클라이언트로 옮겨갑니다.

## 어떤 문제를 푸나

Wikipedia에서 정보를 가져오는 research 도구가 있는 MCP 서버를 상상해봅시다. 모은 데이터를 보고서로 요약하려면 두 가지 선택지가 있습니다.

- **Option 1: 서버에 Claude 직접 접근 부여** — 서버가 자체 API key, 인증, 비용, Claude 연동 코드를 모두 관리. 되지만 복잡도가 큽니다.
- **Option 2: Sampling 사용** — 서버가 prompt를 만들어 클라이언트에게 "Claude를 대신 호출해줄래?"라고 요청. 이미 Claude 연결을 가진 클라이언트가 호출하고 결과를 돌려줍니다.

![Sampling 시퀀스 — 서버가 prompt를 만들어 클라이언트에게 Claude 호출을 요청하고 결과를 받는 Option #2 흐름](/assets/images/courses/model-context-protocol-advanced-topics/sampling.png)

## 동작 흐름

1. 서버가 작업 완료 (예: Wikipedia 문서 수집)
2. 서버가 텍스트 생성용 prompt 생성
3. 서버가 클라이언트에 sampling 요청 전송
4. 클라이언트가 그 prompt로 Claude 호출
5. 클라이언트가 생성된 텍스트를 서버로 반환
6. 서버가 그 텍스트를 응답에 사용

## 이점

- **서버 복잡도 감소** — 언어 모델과 직접 연동할 필요 없음
- **비용 부담 이전** — 토큰 비용을 클라이언트가 부담
- **API key 불필요** — 서버가 Claude 자격증명을 가질 필요 없음
- **공개 서버에 이상적** — 공개 서버가 모든 사용자의 AI 비용을 떠안지 않음

## 구현

**서버 측** — 도구 함수에서 `create_message`로 텍스트 생성 요청:

```python
@mcp.tool()
async def summarize(text_to_summarize: str, ctx: Context):
    prompt = f"Please summarize the following text:\n{text_to_summarize}"
    result = await ctx.session.create_message(
        messages=[SamplingMessage(role="user",
            content=TextContent(type="text", text=prompt))],
        max_tokens=4000,
        system_prompt="You are a helpful research assistant",
    )
    if result.content.type == "text":
        return result.content.text
    raise ValueError("Sampling failed")
```

**클라이언트 측** — 서버 요청을 처리할 sampling callback 작성 후 세션 초기화 시 전달:

```python
async def sampling_callback(context, params):
    text = await chat(params.messages)  # Anthropic SDK로 Claude 호출
    return CreateMessageResult(role="assistant", model=model,
        content=TextContent(type="text", text=text))

async with ClientSession(read, write, sampling_callback=sampling_callback) as session:
    await session.initialize()
```

> **코드 walkthrough**: 강의의 `sampling.zip` 예제로 위 서버·클라이언트 구현을 직접 실행해볼 수 있습니다.

## 핵심 정리

- Sampling은 서버가 클라이언트를 통해 Claude를 호출하게 해, AI 연동 복잡도와 비용을 클라이언트로 옮깁니다.
- 특히 **공개 MCP 서버**에서 가치가 큽니다 — 각 클라이언트가 자기 AI 사용 비용을 부담합니다.
- 구현은 서버의 `create_message`와 클라이언트의 `sampling_callback` 양쪽 코드가 필요합니다.
