---
title: "Accessing Claude with the API"
title_ko: "API로 Claude 사용하기"
course: claude-with-the-anthropic-api
lesson: 2
---

## 학습 목표

- 사용자가 "전송"을 누른 순간부터 응답이 화면에 뜨기까지, **요청의 전체 생명주기** 이해하기
- Claude 내부의 처리 4단계(토큰화 → 임베딩 → 맥락화 → 생성) 파악하기
- API 키 발급, 멀티턴 대화, 시스템 프롬프트, temperature, 스트리밍, 구조화 출력 다루기

> 이 모듈은 Skilljar 세부 레슨 순서를 그대로 따라가요: Accessing the API → Getting an API key → Making a request → Multi-Turn conversations → System prompts → Temperature → Response streaming → Structured data

---

## 1. Accessing the API — 요청의 전체 생명주기

Claude로 애플리케이션을 만들 때, **요청이 어디를 거쳐 어떻게 처리되는지** 알면 아키텍처 결정과 디버깅이 훨씬 쉬워져요. 사용자가 채팅창에서 "전송"을 누른 순간부터 따라가 봅시다.

### 5단계 요청 흐름

모든 상호작용은 예측 가능한 5단계를 거쳐요.

![5단계 요청 흐름](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748623275%2F03_-_001_-_Accessing_the_API_03.1748623275310.png)

1. 클라이언트(웹/모바일 앱) → **내 서버**로 요청
2. 내 서버 → **Anthropic API**로 요청
3. **모델이 처리** (아래에서 자세히)
4. API → 내 서버로 응답
5. 내 서버 → 클라이언트로 응답 전달 → UI에 표시

### 왜 반드시 서버를 거칠까? 🔒

**클라이언트 코드에서 Anthropic API를 직접 호출하면 절대 안 돼요.**

- API 요청에는 **비밀 API 키**가 필요해요.
- 클라이언트 코드에 키를 넣으면 누구나 추출해서 **무단 요청**을 할 수 있어요 (심각한 보안 취약점).
- 그래서 앱은 **내 서버**에 요청을 보내고, 서버가 **안전하게 보관된 키**로 Anthropic API와 통신해요.

### 요청에 반드시 들어가는 것

서버에서 API를 부를 땐 공식 SDK(Python, TypeScript/JavaScript, Go, Ruby 등)를 쓰거나 순수 HTTP 요청을 보내요. 어느 쪽이든 필수 필드는 같아요.

| 필드 | 역할 |
|---|---|
| **API Key** | 요청 인증 |
| **Model** | 사용할 모델 이름 |
| **Messages** | 사용자 입력이 담긴 메시지 목록 |
| **Max Tokens** | 생성할 토큰 수 상한 |

### Claude 내부 처리 4단계

Anthropic이 요청을 받으면 Claude는 네 단계를 거쳐요: **토큰화 → 임베딩 → 맥락화 → 생성**.

![Claude 내부 처리 4단계](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748623277%2F03_-_001_-_Accessing_the_API_08.1748623277503.png)

**① 토큰화 (Tokenization)** — 입력 텍스트를 **토큰**이라는 작은 조각으로 분해해요. 토큰은 단어 전체일 수도, 단어의 일부·공백·기호일 수도 있어요. (단순하게는 "단어 하나 ≈ 토큰 하나"로 생각해도 좋아요.)

**② 임베딩 (Embedding)** — 각 토큰을 **의미를 담은 긴 숫자 목록(벡터)**으로 변환해요. 임베딩은 그 단어의 *가능한 모든 의미*를 담은 숫자 정의라고 보면 돼요. 예를 들어 "quantum"은 물리학의 최소 단위, 양자역학, 아주 작은 것, 양자 컴퓨팅 등 여러 의미를 동시에 품고 있어요.

**③ 맥락화 (Contextualization)** — 주변 단어들을 반영해 각 임베딩을 다듬어서, **문맥상 가장 알맞은 의미**가 부각되도록 숫자 표현을 조정해요. "quantum computing"이라는 문맥이라면 양자 컴퓨팅 쪽 의미가 살아나는 거죠.

**④ 생성 (Generation)** — 맥락화된 임베딩이 출력층을 통과하면서 **다음에 올 수 있는 단어들의 확률**이 계산돼요. Claude는 항상 확률 1등 단어만 고르지 않고, **확률 + 통제된 무작위성**을 섞어 자연스럽고 다양한 응답을 만들어요. 단어를 하나 고르면 시퀀스에 붙이고, 다음 단어를 위해 이 과정 전체를 반복해요.

![다음 단어 확률과 생성](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748623279%2F03_-_001_-_Accessing_the_API_13.1748623279317.png)

### 언제 생성을 멈출까

토큰을 하나 만들 때마다 Claude는 계속할지 멈출지 검사해요.

![생성 중단 조건](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748623280%2F03_-_001_-_Accessing_the_API_15.1748623279963.png)

- **Max tokens 도달** — 내가 지정한 상한에 닿았나?
- **자연 종료** — 문장이 끝났다는 end-of-sequence 토큰을 생성했나?
- **Stop sequence** — 미리 정의한 중단 문구를 만났나?

### API 응답 구조

생성이 끝나면 API가 구조화된 응답을 돌려줘요.

![API 응답 구조](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748623281%2F03_-_001_-_Accessing_the_API_17.1748623281653.png)

| 필드 | 내용 |
|---|---|
| **Message** | 생성된 텍스트 |
| **Usage** | 입력/출력 토큰 수 (비용 계산의 근거) |
| **Stop Reason** | 생성이 끝난 이유 |

내 서버가 이 응답을 받아 생성된 텍스트를 클라이언트로 전달하면, 사용자 화면에 답변이 나타나요.

### 이 흐름을 알면 좋은 점

- API 키를 보호하는 **안전한 아키텍처** 설계
- 용도에 맞는 **토큰 상한** 설정
- **stop reason별 처리** 로직 구현
- 문제가 생겼을 때 **파이프라인 어디서 났는지** 짚어내는 디버깅

> 💡 전부 외울 필요는 없어요 — 목표는 Claude API에서 계속 만나게 될 **용어와 전체 흐름에 익숙해지는 것**!

---

## 2. API 키 발급 (Getting an API key)

1. Anthropic Console에서 **API 키**를 발급받아요.
2. 키는 코드에 하드코딩하지 말고 **환경 변수** `ANTHROPIC_API_KEY`에 넣어요.

```python
import anthropic

client = anthropic.Anthropic()  # ANTHROPIC_API_KEY 환경 변수를 자동으로 읽음
```

---

## 3. 첫 요청 보내기 (Making a request)

```python
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    messages=[{"role": "user", "content": "프랑스의 수도는?"}],
)
print(response.content[0].text)
```

- `model`, `max_tokens`, `messages`는 필수예요.
- 응답의 `content`는 **블록들의 리스트**예요. `block.type`을 확인하고 `text`를 꺼내야 해요.

---

## 4. 멀티턴 대화 (Multi-Turn conversations)

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

---

## 5. 시스템 프롬프트 (System prompts)

`system` 파라미터로 Claude의 **역할·행동 방식**을 지정해요. 대화 메시지와 별개예요.

```python
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    system="너는 친절한 코딩 튜터야. 항상 Python 예시를 함께 줘.",
    messages=[{"role": "user", "content": "JSON 파일 어떻게 읽어?"}],
)
```

---

## 6. Temperature

`temperature`는 출력의 **무작위성**을 조절해요 (0에 가까울수록 결정적, 높을수록 다양). 사실 추출·분류엔 낮게, 창의적 생성엔 높게.

> ⚠️ 최신 모델(예: Opus 4.8)에서는 `temperature`가 제거되어 프롬프트로 행동을 유도해요. 개념은 알아두되, 최신 모델에선 프롬프트 지시로 대체된다고 기억하면 돼요.

---

## 7. 응답 스트리밍 (Response streaming)

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

---

## 8. 구조화된 데이터 (Structured data)

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

---

## 정리

- 요청은 **클라이언트 → 내 서버 → Anthropic API** 5단계를 오가고, 키는 서버에만 둔다.
- Claude 내부: **토큰화 → 임베딩 → 맥락화 → 생성**, 중단 조건은 max_tokens/자연 종료/stop sequence.
- 응답엔 **message·usage·stop_reason**이 담겨 온다.
- API는 stateless → 멀티턴은 **전체 기록을 매번 재전송**.
- `system`으로 역할 지정, 긴 출력은 **스트리밍**, 형식이 필요하면 **구조화 출력**.

*도식 이미지 출처: [Anthropic Academy — Building with the Claude API](https://anthropic.skilljar.com/claude-with-the-anthropic-api)*
