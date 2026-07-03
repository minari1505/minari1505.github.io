---
title: "Log and progress notifications"
title_ko: "로그·진행 알림"
course: model-context-protocol-advanced-topics
lesson: 3
---

## 학습 목표

- 로그·진행 알림이 사용자 경험에 왜 중요한지 이해하기
- Context 인자를 통한 서버 측 구현 익히기
- 클라이언트 측 callback과 표현 방식 파악하기

## 왜 필요한가

Claude가 시간이 걸리는 도구(주제 조사, 데이터 처리)를 호출하면, 사용자는 작업이 끝날 때까지 아무것도 못 보고 "멈춘 건가?" 궁금해합니다. **로그·진행 알림**을 켜면 진행 바, 상태 메시지, 상세 로그로 **실시간 피드백**을 줄 수 있습니다.

## 서버 측 구현

Python MCP SDK에서는 도구 함수에 자동 제공되는 **Context** 인자로 동작합니다.

```python
@mcp.tool(name="research", description="Research a given topic")
async def research(topic: str = Field(description="Topic to research"), *, context: Context):
    await context.info("About to do research...")
    await context.report_progress(20, 100)
    sources = await do_research(topic)

    await context.info("Writing report...")
    await context.report_progress(70, 100)
    results = await generate_report(sources)
    return results
```

- `context.info()` — 클라이언트에 로그 메시지 전송
- `context.report_progress()` — 현재값·전체값으로 진행 업데이트

## 클라이언트 측 구현

서버는 메시지를 내보내고, **어떻게 보여줄지는 클라이언트가 결정**합니다. callback을 설정합니다.

```python
async def logging_callback(params):
    print(params.data)

async def print_progress_callback(progress, total, message):
    if total is not None:
        print(f"Progress: {progress}/{total} ({progress/total*100:.1f}%)")
    else:
        print(f"Progress: {progress}")

async with stdio_client(server_params) as (read, write):
    async with ClientSession(read, write, logging_callback=logging_callback) as session:
        await session.initialize()
        await session.call_tool(name="add", arguments={"a":1,"b":3},
                                progress_callback=print_progress_callback)
```

logging callback은 **세션 생성 시**, progress callback은 **개별 도구 호출 시** 전달합니다.

## 표현 방식

- **CLI** — 터미널에 메시지·진행 출력
- **웹** — WebSocket, server-sent events, 폴링으로 브라우저에 push
- **데스크톱** — UI의 진행 바·상태 표시 업데이트

> 이 알림 구현은 **완전히 선택 사항**입니다 — 무시하거나, 일부만 보여주거나, 앱에 맞게 표현할 수 있습니다. 순수하게 UX 향상 장치입니다.
>
> **코드 walkthrough**: 강의의 Notifications walkthrough 예제로 위 흐름을 직접 실행해볼 수 있습니다.

## 핵심 정리

- 로그·진행 알림은 장기 작업 중 "무슨 일이 일어나는지"를 사용자에게 보여주는 UX 장치입니다.
- 서버는 Context의 `info()`·`report_progress()`로 내보내고, 클라이언트는 logging/progress callback으로 받아 표현합니다.
- 구현은 선택 사항이며 앱 유형(CLI·웹·데스크톱)에 맞게 자유롭게 표현합니다.
