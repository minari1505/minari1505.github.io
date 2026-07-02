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

API에 요청을 보내려면 **비밀 API 키**가 필요해요. 발급 과정은 5단계예요.

**Step 1 — Anthropic Console 접속**

브라우저에서 [console.anthropic.com](https://console.anthropic.com/)으로 이동해 Anthropic 계정으로 로그인해요.

![Anthropic Console 대시보드](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749846091%2FScreenshot+2025-06-13+at+2.20.45%E2%80%AFPM.1749846091092.png)

**Step 2 — 'Get API Keys' 버튼 클릭**

대시보드 오른쪽 위에 있어요.

![Get API Keys 버튼](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749846107%2FScreenshot+2025-06-13+at+2.21.16%E2%80%AFPM.1749846106943.png)

**Step 3 — 'Create Key' 버튼 클릭**

![Create Key 버튼](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749846149%2FScreenshot+2025-06-13+at+2.22.10%E2%80%AFPM.1749846149203.png)

**Step 4 — 워크스페이스와 키 이름 입력**

워크스페이스는 'Default', 이름은 키를 식별하기 위한 것이니 용도에 맞게 (강의에서는 'Anthropic Course').

![키 이름 입력](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749846167%2FScreenshot+2025-06-13+at+2.22.14%E2%80%AFPM.1749846166954.png)

**Step 5 — 키 복사 ⚠️**

팝업에 키가 표시되는데, **딱 한 번만 보여줘요.** 반드시 복사해서 안전한 곳에 보관하세요. 실수로 창을 닫았다면 그 키를 삭제하고 새로 만들면 돼요.

---

## 3. 첫 요청 보내기 (Making a request)

강의에서는 Jupyter 노트북 기준으로 4단계 셋업을 거쳐요.

```bash
pip install anthropic python-dotenv
```

```text
# .env 파일 (버전 관리에서 제외할 것!)
ANTHROPIC_API_KEY="발급받은_키"
```

```python
from dotenv import load_dotenv
import anthropic

load_dotenv()                      # .env에서 키를 안전하게 로드
client = anthropic.Anthropic()     # ANTHROPIC_API_KEY를 자동으로 읽음
model = "claude-opus-4-8"          # 모델명은 변수로 정의해 재사용

response = client.messages.create(
    model=model,
    max_tokens=1000,
    messages=[{"role": "user", "content": "양자 컴퓨팅이 뭐야?"}],
)
print(response.content[0].text)    # 전체 응답에서 텍스트만 추출
```

- 필수 인자는 **`model` · `max_tokens` · `messages`** 세 가지.
- `max_tokens`는 **목표 길이가 아니라 안전 상한**이에요 — 응답이 그보다 짧게 끝날 수 있어요.
- 응답 객체엔 메타데이터가 함께 오므로, 텍스트만 필요하면 `response.content[0].text`로 꺼내요.

---

## 4. 멀티턴 대화 (Multi-Turn conversations)

**API는 메시지를 전혀 저장하지 않아요(stateless).** 각 요청은 독립적이고 이전 교환의 기억이 없어요. 해결책은 두 가지:

1. 코드에서 **메시지 리스트를 직접 관리**하고
2. 후속 요청마다 **전체 대화 기록을 함께 전송**하기

강의에서는 이를 위한 헬퍼 함수 3개를 만들어요.

```python
def add_user_message(messages, text):
    messages.append({"role": "user", "content": text})

def add_assistant_message(messages, text):
    messages.append({"role": "assistant", "content": text})

def chat(messages):
    response = client.messages.create(
        model=model, max_tokens=1000, messages=messages)
    return response.content[0].text

messages = []
add_user_message(messages, "내 이름은 앨리스야.")
answer = chat(messages)
add_assistant_message(messages, answer)   # 응답도 기록에 추가!
add_user_message(messages, "내 이름이 뭐라고 했지?")
print(chat(messages))                     # 전체 기록 덕분에 맥락 유지
```

- 기록 없이 후속 질문을 보내면 **맥락 없는 엉뚱한 답**이 돌아와요.
- 첫 메시지는 `user`, 이후 `user`/`assistant`가 번갈아 나와요.

> ✏️ 이 뒤엔 **Chat exercise** — 배운 헬퍼 함수로 간단한 챗봇을 직접 만들어보는 실습이 이어져요.

---

## 5. 시스템 프롬프트 (System prompts)

`system` 파라미터로 Claude에게 **역할**을 부여해 응답의 스타일·태도를 제어해요. **"무엇을" 답할지가 아니라 "어떻게" 답할지**를 바꾸는 도구예요.

```python
system_prompt = """너는 인내심 많은 수학 튜터야.
학생에게 정답을 바로 주지 말고, 힌트와 유도 질문으로 스스로 생각하게 도와줘."""

response = client.messages.create(
    model=model,
    max_tokens=1000,
    system=system_prompt,
    messages=[{"role": "user", "content": "2x + 5 = 13 풀어줘"}],
)
```

- 구조: 첫 줄에서 **역할 지정** ("You are a patient math tutor") → 이어서 구체적 행동 지침.
- 같은 질문이라도 역할에 따라 다르게 다뤄져요 — 수학 튜터는 풀이 대신 힌트를 줘요.
- 구현 팁: `system`이 없을 수도 있으니 params 딕셔너리를 만들어 **조건부로 system 키를 추가**하고 `**params`로 넘기면 재사용성이 좋아요.

---

## 6. Temperature

`temperature`는 **0~1 사이 값**으로 토큰 선택의 무작위성을 조절해요. 생성 과정(토큰화 → 확률 계산 → 토큰 선택)에서 **확률 분포를 직접 조작**하는 파라미터예요.

| 값 | 동작 | 용도 |
|---|---|---|
| **0** | 항상 최고 확률 토큰 선택 (결정적) | 데이터 추출, 사실 기반 작업, 일관성 |
| **1에 가깝게** | 낮은 확률 토큰도 선택될 가능성 ↑ | 브레인스토밍, 창작, 마케팅 문구 |

- 높은 temperature가 **다른 출력을 보장하진 않아요** — 변화의 *확률*을 높일 뿐.

> ⚠️ 최신 모델(Opus 4.8 등)에서는 `temperature` 파라미터가 제거되어 프롬프트로 행동을 유도해요. 개념은 알아두되, 최신 모델에선 프롬프트 지시로 대체된다고 기억하세요.

---

## 7. 응답 스트리밍 (Response streaming)

AI 응답은 10~30초 걸릴 수 있는데, 사용자는 즉각적인 피드백을 기대해요. **스트리밍 = 생성되는 텍스트를 청크 단위로 실시간 표시**하는 기법이에요.

동작 방식: 요청 → 즉시 초기 응답(텍스트 없음, 확인용) → **이벤트 스트림**으로 텍스트 청크 전달 → 프론트엔드에 실시간 표시.

| 이벤트 | 의미 |
|---|---|
| `message_start` | 시작 확인 |
| `content_block_start` | 텍스트 생성 시작 |
| `content_block_delta` | **실제 텍스트 청크** (가장 중요!) |
| `content_block_stop` / `message_stop` | 생성 완료 |

```python
# 간편한 방법: stream() 헬퍼 + text_stream
with client.messages.stream(
    model=model, max_tokens=1000,
    messages=[{"role": "user", "content": "짧은 이야기 하나 써줘"}],
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)
    final = stream.get_final_message()   # DB 저장용 전체 메시지 조립
```

- 저수준 방법: `messages.create(..., stream=True)`로 원시 이벤트 이터레이터를 직접 처리할 수도 있어요.

---

## 8. 구조화된 데이터 (Structured data)

Claude는 JSON·코드를 만들 때 자연스럽게 설명·헤더·마크다운을 덧붙여요. 하지만 애플리케이션에는 **파싱 가능한 원본 데이터만** 필요할 때가 많죠. 강의에서 가르치는 기법은 **어시스턴트 메시지 프리필 + stop sequence** 조합이에요.

```python
messages = [
    {"role": "user", "content": "이벤트 규칙을 JSON으로 만들어줘"},
    {"role": "assistant", "content": "```json"},   # ① 프리필: 이미 시작한 척
]
response = client.messages.create(
    model=model, max_tokens=1000,
    messages=messages,
    stop_sequences=["```"],                        # ② 닫는 구분자에서 정지
)
# 결과: 순수 JSON만! 여는/닫는 ``` 도, 설명도 없음
data = json.loads(response.content[0].text.strip())
```

동작 원리:

1. **프리필** — Claude는 ` ```json `을 자기가 이미 쓴 것으로 간주하고 **그 지점부터 이어서** 생성해요. (응답 방향을 조종하는 프리필 기법의 응용)
2. **Stop sequence** — 닫는 ` ``` `를 생성하는 순간 즉시 멈추고, 그 문자열은 출력에 포함되지 않아요.

JSON뿐 아니라 Python 코드, 리스트 등 **모든 구조화 출력**에 쓸 수 있는 패턴이에요.

> ⚠️ **최신 API 참고**: 최신 모델(Opus 4.8 등)은 마지막 assistant 턴 프리필이 지원되지 않아요(400 에러). 같은 목적은 **구조화 출력**(`output_config.format`에 JSON 스키마 지정)으로 달성해요 — 스키마 검증까지 보장돼서 더 안정적이에요. 강의의 프리필+stop sequence는 "왜 이런 기법이 필요한가"를 이해하는 관점으로 보세요.

> ✏️ 이 뒤엔 **Structured data exercise**와 이 모듈 전체를 확인하는 **Quiz**가 이어져요.

---

## 정리

- 요청은 **클라이언트 → 내 서버 → Anthropic API** 5단계, 키는 서버에만 (Console에서 발급, 한 번만 표시!).
- 셋업: `pip install anthropic python-dotenv` → `.env`에 키 → `load_dotenv()` → 클라이언트 생성.
- API는 stateless → 멀티턴은 **헬퍼 함수로 기록을 쌓고 매번 전체 재전송**.
- `system` = 역할 부여로 "어떻게" 답할지 제어, `temperature` = 0(일관) ~ 1(창의).
- 긴 응답은 **스트리밍**(`content_block_delta`가 핵심), 구조화 출력은 **프리필+stop sequence** (최신 모델은 `output_config.format`).

*도식·스크린샷 출처: [Anthropic Academy — Building with the Claude API](https://anthropic.skilljar.com/claude-with-the-anthropic-api)*
