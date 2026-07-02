---
title: "Features of Claude"
title_ko: "Claude의 기능들"
course: claude-with-the-anthropic-api
lesson: 7
---

## 학습 목표

- Claude의 주요 고급 기능들을 언제 쓰는지 파악하기
- extended thinking, 이미지·PDF 입력, 인용, 프롬프트 캐싱, 코드 실행을 이해하기
- 각 기능의 API 메시지 구조와 비용/지연/호환성 trade-off 파악하기

## 1. Extended Thinking (확장 사고)

Extended thinking은 Claude가 최종 답을 생성하기 전에 복잡한 문제를 더 깊게 검토할 수 있게 하는 고급 추론 기능입니다. 일종의 "scratch paper"처럼, 모델이 답에 도달하기 전의 reasoning 과정을 별도 블록으로 다룹니다.

이 기능을 켜면 응답은 단순한 `text` 블록 하나가 아니라 보통 다음처럼 구조화됩니다.

- `thinking` block: Claude의 추론 과정
- `text` block: 사용자에게 보여줄 최종 답변

장점은 명확합니다.

- 복잡한 수학, 논리, 코딩 문제에서 정확도 향상
- 어려운 문제를 더 체계적으로 검토
- 모델이 어떤 방향으로 생각했는지 더 투명하게 확인 가능

대신 비용도 있습니다.

- thinking token에도 비용이 발생
- 생각하는 시간이 추가되어 latency 증가
- 응답 구조가 복잡해져 코드에서 block 처리가 필요

### 언제 써야 할까?

무조건 켜는 기능은 아닙니다. 먼저 일반 프롬프트로 eval을 돌리고, 프롬프트를 충분히 개선한 뒤에도 정확도가 요구 수준에 못 미칠 때 extended thinking을 고려하는 것이 좋습니다.

즉, 순서는 다음이 자연스럽습니다.

1. 기본 프롬프트 작성
2. eval로 실패 케이스 확인
3. 프롬프트 엔지니어링으로 개선
4. 그래도 복잡한 reasoning에서 부족하면 extended thinking 적용

### 구현

```python
def chat(
    messages,
    system=None,
    temperature=1.0,
    stop_sequences=[],
    tools=None,
    thinking=False,
    thinking_budget=1024,
):
    params = {
        "model": "claude-opus-4-8",
        "max_tokens": 4096,
        "messages": messages,
    }

    if thinking:
        params["thinking"] = {
            "type": "enabled",
            "budget": thinking_budget,
        }

    return client.messages.create(**params)
```

`thinking_budget`은 Claude가 reasoning에 사용할 수 있는 최대 token 수입니다. 최소값은 `1024`이고, `max_tokens`는 thinking budget보다 커야 합니다.

```python
chat(messages, thinking=True, thinking_budget=1024)
```

주의할 점도 있습니다. Extended thinking은 일부 기능과 호환되지 않습니다. 강의에서는 특히 **assistant message prefill**과 **temperature**를 주의하라고 설명합니다. 실제 앱에서는 Anthropic 문서의 feature compatibility 목록을 확인해야 합니다.

### Signature와 redacted thinking

Extended thinking 응답에는 thinking text가 변조되지 않았음을 확인하기 위한 signature가 포함될 수 있습니다. 이는 개발자가 thinking 내용을 임의로 바꿔 모델을 위험한 방향으로 유도하는 일을 막기 위한 안전장치입니다.

때로는 readable thinking 대신 **redacted thinking block**이 올 수 있습니다. 내부 safety system이 reasoning 내용을 민감하다고 판단한 경우입니다. 이때 실제 thinking은 암호화된 형태로 포함되어 있어서, 다음 대화에 전체 메시지를 그대로 넘기면 context를 잃지 않고 이어갈 수 있습니다.

따라서 앱에서는 `thinking` block이 항상 사람이 읽을 수 있다고 가정하지 말고, redacted block도 깨지지 않게 보존해야 합니다.

## 2. 이미지 입력 (Vision)

Claude의 vision 기능을 사용하면 메시지에 이미지를 포함하고, Claude에게 설명, 비교, 객체 수 세기, 차트 해석, 시각적 위험 평가 같은 작업을 맡길 수 있습니다.

이미지를 보낼 때 기억할 제한이 있습니다.

- 한 요청 전체에서 최대 100개 이미지
- 이미지당 최대 5MB
- 이미지 1개만 보낼 때 최대 8000px x 8000px
- 여러 이미지를 보낼 때 각 이미지 최대 2000px x 2000px
- base64 또는 URL 방식으로 전달 가능
- 이미지 token은 대략 `(width px × height px) / 750`으로 계산

```python
with open("image.png", "rb") as f:
    image_bytes = base64.standard_b64encode(f.read()).decode("utf-8")

add_user_message(messages, [
    {
        "type": "image",
        "source": {
            "type": "base64",
            "media_type": "image/png",
            "data": image_bytes,
        },
    },
    {
        "type": "text",
        "text": "What do you see in this image?",
    },
])
```

대화 흐름은 텍스트만 보낼 때와 같습니다. user message 안에 image block과 text block을 함께 넣고, Claude는 분석 결과를 text block으로 반환합니다.

### 이미지도 프롬프트 엔지니어링이 필요하다

이미지를 넣었다고 해서 단순 질문만으로 항상 정확한 답이 나오지는 않습니다. 예를 들어 구슬 개수를 세는 문제에서 `"How many marbles are in this image?"`만 묻는다면 오답이 나올 수 있어요.

정확도를 높이려면 텍스트 프롬프트와 같은 전략을 써야 합니다.

- 분석 절차를 구체적으로 제시
- one-shot/multi-shot 예시 제공
- 복잡한 시각 작업을 작은 단계로 분해

```text
Analyze this image of marbles and determine the exact count using this methodology:
1. Begin by identifying each unique marble one at a time. Assign each a number as you identify it.
2. Verify your result by counting with a different method. Start from the bottom-left corner and work row by row, from left to right.

What is the exact, verified number of marbles in this image?
```

### 실전 예시: 화재 위험 평가

강의에서는 위성 이미지로 주택 화재 위험을 평가하는 예시를 듭니다. 단순히 `"provide a fire risk score"`라고 하면 충분하지 않습니다. 대신 다음처럼 분석 항목을 분해합니다.

- 주거 건물 식별
- 지붕 위로 뻗은 나무 가지 확인
- 산불 취약 지점 평가
- defensible space 확인
- 1-4 등급으로 최종 fire risk rating 부여

이미지 입력에서도 핵심은 같습니다. 단순 질문보다, Claude가 따라야 할 관찰 절차와 판단 기준을 명확히 주는 편이 훨씬 안정적입니다.

## 3. PDF 입력

Claude는 PDF 파일을 직접 읽고 분석할 수 있습니다. 이미지 처리와 코드 구조는 거의 비슷하지만, block type과 media type이 다릅니다.

```python
with open("earth.pdf", "rb") as f:
    file_bytes = base64.standard_b64encode(f.read()).decode("utf-8")

messages = []

add_user_message(
    messages,
    [
        {
            "type": "document",
            "source": {
                "type": "base64",
                "media_type": "application/pdf",
                "data": file_bytes,
            },
        },
        {"type": "text", "text": "Summarize the document in one sentence"},
    ],
)

chat(messages)
```

이미지 처리 코드에서 PDF 처리 코드로 바꿀 때의 핵심 변경점은 다음과 같습니다.

- 파일 확장자: `.png` → `.pdf`
- 변수명: `image_bytes` → `file_bytes`
- block type: `"image"` → `"document"`
- media type: `"image/png"` → `"application/pdf"`

Claude는 PDF에서 단순 텍스트만 읽는 것이 아니라 다음 정보도 함께 이해할 수 있습니다.

- 문서 전체의 텍스트
- PDF 안의 이미지와 차트
- 표와 데이터 관계
- 문서 구조와 포맷

그래서 PDF 요약, 특정 정보 추출, 표 분석, 문서 기반 QA에 바로 활용할 수 있습니다. 큰 파일을 반복해서 쓴다면 Files API로 한 번 업로드하고 `file_id`로 재사용하는 방식도 고려할 수 있습니다.

## 4. 인용 (Citations)

문서 기반 답변에서 중요한 것은 "Claude가 어디를 보고 그렇게 답했는지"를 보여주는 것입니다. Citations를 켜면 Claude가 답변의 각 주장에 대해 원문 문서의 어느 부분을 근거로 삼았는지 함께 반환합니다.

PDF 문서에 citations를 켜려면 document block에 `title`과 `citations`를 추가합니다.

```python
{
    "type": "document",
    "source": {
        "type": "base64",
        "media_type": "application/pdf",
        "data": file_bytes,
    },
    "title": "earth.pdf",
    "citations": {"enabled": True},
}
```

`title`은 사용자에게 보여줄 문서 이름이고, `citations: {"enabled": True}`는 Claude에게 출처 추적을 요청하는 설정입니다.

응답의 citation에는 보통 다음 정보가 들어갑니다.

- `cited_text`: Claude 답변을 뒷받침하는 원문 텍스트
- `document_index`: 여러 문서를 넣었을 때 몇 번째 문서인지
- `document_title`: 문서 제목
- `start_page_number`: 인용이 시작된 페이지
- `end_page_number`: 인용이 끝난 페이지

plain text 문서에서도 citations를 사용할 수 있습니다.

```python
{
    "type": "document",
    "source": {
        "type": "text",
        "media_type": "text/plain",
        "data": article_text,
    },
    "title": "earth_article",
    "citations": {"enabled": True},
}
```

텍스트 문서에서는 페이지 번호 대신 character position이 반환되어, 원문 어디에서 근거를 찾았는지 알 수 있습니다.

Citations는 다음 상황에서 특히 유용합니다.

- 사용자가 답변의 정확성을 검증해야 할 때
- 권위 있는 문서를 근거로 답변해야 할 때
- RAG 또는 문서 QA에서 출처 투명성이 중요할 때
- 사용자가 원문 맥락을 더 살펴볼 수 있어야 할 때

UI에서는 citation marker에 hover하면 원문 일부, 문서 제목, 페이지 번호를 보여주는 식으로 만들 수 있습니다. 이렇게 하면 Claude가 단순히 "그럴듯한 답"을 하는 것이 아니라, 근거를 제시하는 문서 연구 도우미처럼 동작합니다.

## 5. 프롬프트 캐싱 (Prompt Caching)

Prompt caching은 이전 요청에서 Claude가 이미 처리한 입력의 계산 결과를 재사용해 응답 속도를 높이고 비용을 줄이는 기능입니다.

일반 요청에서는 Claude가 응답을 생성하기 전에 입력을 먼저 많이 처리합니다.

1. 프롬프트를 token으로 나눔
2. 각 token을 내부 표현으로 변환
3. 주변 문맥을 반영해 context를 구성
4. 그다음 출력 생성 시작

문제는 응답을 보낸 뒤 이 계산 결과를 버린다는 점입니다. 사용자가 같은 긴 문서를 놓고 후속 질문을 여러 번 하면, Claude는 매번 같은 문서를 다시 처리해야 합니다.

Prompt caching은 이 계산 결과를 cache에 저장해둡니다. 이후 같은 prefix가 다시 들어오면 Claude는 이미 처리한 부분을 다시 계산하지 않고 cache에서 읽습니다.

장점:

- 반복 요청이 더 빨라짐
- cache hit 구간의 비용 감소
- 긴 문서 분석, 반복 편집, 고정 시스템 프롬프트 재사용에 유리

한계:

- cache는 영구 저장이 아니라 제한된 시간만 유지됨
- 같은 content가 반복될 때만 효과가 있음
- 요청마다 바뀌는 내용이 앞쪽에 들어가면 cache hit가 깨짐

예를 들어 같은 긴 문서를 두고 "요약해줘", "더 짧게", "표로 바꿔줘"처럼 여러 번 요청하는 워크플로우에 잘 맞습니다.

### 캐싱의 핵심 규칙

캐싱은 자동으로 켜지지 않습니다. 직접 **cache breakpoint**를 지정해야 합니다.

- 작업 결과는 기본적으로 캐시되지 않음
- 캐시하고 싶은 블록에 breakpoint를 추가해야 함
- breakpoint까지의 모든 내용이 캐시됨
- 후속 요청에서 breakpoint까지의 내용이 완전히 동일해야 cache hit

캐시 breakpoint는 `cache_control: {"type": "ephemeral"}`로 지정합니다. 짧은 문자열 shorthand에는 이 필드를 붙일 자리가 없으므로, text block의 longhand 형식을 써야 합니다.

```python
{
    "type": "text",
    "text": long_context,
    "cache_control": {"type": "ephemeral"},
}
```

캐시의 핵심은 **prefix 일치**입니다. breakpoint 앞부분이 한 글자라도 바뀌면 그 지점 이후 캐시는 무효화됩니다. 예를 들어 같은 문서 앞에 `"please"` 한 단어만 추가해도, 그 breakpoint까지의 cache를 다시 만들어야 할 수 있습니다.

### Cache breakpoint의 범위

Breakpoint는 한 메시지 안에서만 동작하는 것이 아닙니다. 나중 메시지의 block에 breakpoint를 두면, 그 이전의 user/assistant 메시지까지 포함해 캐시될 수 있습니다. 그래서 긴 대화의 일정 지점까지 context를 고정하고, 그 뒤의 새 질문만 바꾸는 구조에 활용할 수 있습니다.

캐시 breakpoint를 붙일 수 있는 곳은 text block만이 아닙니다.

- system prompt
- tool definition
- image block
- tool use / tool result block
- 일반 message block

특히 system prompt와 tool schema는 요청마다 잘 바뀌지 않으므로 캐싱 효과가 큽니다.

### 캐시 순서와 제한

Claude는 내부적으로 요청 구성 요소를 다음 순서로 처리합니다.

```text
tools → system → messages
```

이 순서를 이해해야 breakpoint를 잘 놓을 수 있습니다. tools가 안정적이고 system prompt가 자주 바뀐다면, tools 쪽 cache는 살아 있고 system prompt 이후만 다시 처리될 수 있습니다.

요청 하나에 cache breakpoint는 최대 4개까지 둘 수 있습니다. 예를 들어 tool schema에 하나, system prompt에 하나, 긴 대화 히스토리 중간에 하나를 둘 수 있어요.

또 하나 중요한 제한은 최소 길이입니다. 캐시하려는 내용은 합산해서 최소 1024 token 이상이어야 합니다. 아주 짧은 `"Hi there!"` 같은 메시지는 캐싱 대상이 되지 않습니다.

### Prompt caching in action

실제로는 tool schema와 system prompt가 좋은 캐시 대상입니다. 여러 도구를 쓰는 앱에서는 tool schema만 해도 꽤 많은 token을 차지하고, coding assistant 같은 앱은 system prompt도 수천 token이 될 수 있습니다.

Tool schema를 캐싱할 때는 원본 tool 정의를 직접 수정하지 말고 복사본에 `cache_control`을 붙이는 편이 안전합니다.

```python
if tools:
    tools_clone = tools.copy()
    last_tool = tools_clone[-1].copy()
    last_tool["cache_control"] = {"type": "ephemeral"}
    tools_clone[-1] = last_tool
    params["tools"] = tools_clone
```

마지막 tool에 breakpoint를 붙이면, 그 앞의 tool schema 전체가 캐시 대상이 됩니다. 원본 `tools[-1]`을 직접 수정하지 않는 이유는 나중에 도구 순서를 바꾸거나 같은 tool list를 재사용할 때 side effect를 줄이기 위해서입니다.

System prompt는 문자열이 아니라 cache control을 붙일 수 있는 text block 형태로 보냅니다.

```python
if system:
    params["system"] = [
        {
            "type": "text",
            "text": system,
            "cache_control": {"type": "ephemeral"},
        }
    ]
```

캐시가 제대로 동작하는지는 response usage에서 확인합니다.

- 첫 요청: `cache_creation_input_tokens`가 증가하면 cache write
- 후속 요청: `cache_read_input_tokens`가 증가하면 cache hit
- 내용 변경: 새 `cache_creation_input_tokens`가 나타나면 해당 부분을 다시 캐시

예를 들어 tools는 그대로이고 system prompt만 바뀌었다면, tools 구간은 cache read가 되고 system prompt 구간은 새로 cache write가 될 수 있습니다. 이처럼 prompt caching은 전체 요청을 통째로 캐시하는 것이 아니라, breakpoint와 처리 순서에 따라 일부만 재사용할 수 있습니다.

정리하면 안정적인 내용은 앞에 두고, 매번 바뀌는 내용은 뒤로 밀어야 합니다. timestamp, random ID, 사용자별 임시 값이 앞쪽에 섞이면 cache hit가 쉽게 깨집니다.

## 6. 코드 실행 & Files API

Files API와 code execution은 따로 보면 별개의 기능이지만, 함께 쓰면 Claude에게 복잡한 데이터 처리 작업을 위임할 수 있습니다.

### Files API

Files API는 파일을 메시지에 base64로 직접 넣는 대신, 먼저 업로드하고 이후 요청에서 `file_id`로 참조하는 방식입니다.

흐름은 다음과 같습니다.

1. 이미지, PDF, CSV, 텍스트 파일 등을 별도 API 호출로 업로드
2. 고유한 file ID가 들어 있는 metadata 객체를 받음
3. 이후 메시지에서는 raw file data 대신 file ID를 참조

같은 파일을 여러 번 쓰거나, 매 요청마다 base64로 넣기 부담스러운 큰 파일을 다룰 때 유용합니다.

### Code execution tool

Code execution은 Anthropic 서버 측 도구입니다. 내가 Python 실행 환경을 직접 만들 필요 없이, 미리 정의된 tool schema만 넣으면 Claude가 격리된 Docker container 안에서 Python 코드를 실행할 수 있습니다.

특징은 다음과 같습니다.

- 격리된 Docker container에서 실행
- 네트워크 접근 없음. 외부 API 호출 불가
- 한 응답 안에서 Claude가 여러 번 코드를 실행할 수 있음
- 실행 결과를 Claude가 읽고 최종 답변에 반영

네트워크가 없기 때문에, container 안으로 데이터를 넣고 결과 파일을 꺼내는 주요 통로가 Files API입니다.

### Files API + code execution 워크플로우

대표적인 사용 흐름은 CSV 데이터 분석입니다.

```python
file_metadata = upload("streaming.csv")
```

그다음 메시지에 업로드한 파일을 `container_upload` block으로 포함합니다.

```python
messages = []
add_user_message(
    messages,
    [
        {
            "type": "text",
            "text": """Run a detailed analysis to determine major drivers of churn.
            Your final output should include at least one detailed plot summarizing your findings.""",
        },
        {"type": "container_upload", "file_id": file_metadata.id},
    ],
)
```

Code execution tool을 함께 전달합니다.

```python
chat(
    messages,
    tools=[{"type": "code_execution_20250522", "name": "code_execution"}],
)
```

Claude는 CSV를 읽고, 필요한 Python 코드를 작성하고, 실행하고, 결과를 해석합니다. 분석 중간에 여러 번 코드를 실행하면서 결과를 점진적으로 개선할 수도 있습니다.

응답에는 여러 block이 섞입니다.

- text block: Claude의 설명과 분석
- server tool use block: Claude가 실행하기로 한 코드
- code execution tool result block: 코드 실행 결과
- code execution output block: 생성된 파일 정보

Claude가 차트나 리포트 파일을 만들면 container에 저장되고, 응답의 `code_execution_output` block에서 file ID를 찾을 수 있습니다.

```python
download_file("file_id_from_response")
```

활용 범위는 데이터 분석에만 한정되지 않습니다.

- 이미지 처리와 변환
- 문서 파싱과 변환
- 수학 계산과 모델링
- 차트, 표, 리포트 생성

핵심은 Claude가 단순히 코드 제안을 하는 것이 아니라, 실제로 코드를 실행하고 결과를 보고 다시 판단할 수 있다는 점입니다. Files API는 그 실행 환경과 내 애플리케이션 사이의 입력/출력 통로 역할을 합니다.

## 정리

- **thinking**: 어려운 문제에는 extended thinking으로 reasoning budget을 부여.
- **이미지·PDF**: 멀티모달 입력. 이미지/PDF도 구조화된 프롬프트가 중요.
- **인용**: 답변의 출처를 함께 받아 문서 기반 답변의 투명성 확보.
- **캐싱**: 반복되는 큰 컨텍스트를 breakpoint 기반 prefix cache로 절약.
- **코드 실행**: Files API로 데이터를 넣고, 샌드박스에서 Claude가 직접 Python 실행.
