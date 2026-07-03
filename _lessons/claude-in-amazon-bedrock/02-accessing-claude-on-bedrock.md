---
title: "Accessing Claude on Amazon Bedrock"
title_ko: "Bedrock에서 Claude 접속과 요청"
course: claude-in-amazon-bedrock
lesson: 2
---

> 이 정리는 **Amazon Bedrock 고유 부분**만 다룹니다. 이후 개념(멀티턴·시스템 프롬프트·streaming·tool use·RAG·에이전트 등)은 일반 API와 동일하므로 [Building with the Claude API](/courses/claude-with-the-anthropic-api/01-introduction/) 코스를 참고하세요.

## 학습 목표

- Bedrock을 통한 요청 흐름 이해하기
- boto3 `bedrock-runtime` 클라이언트로 첫 요청 만들기
- **모델 ID의 리전 가용성** 문제와 **inference profile(cross-region inference)**로 해결하기
- `converse` API의 메시지 구조와 응답 파싱 익히기

## 요청 흐름

Bedrock 기반 챗 앱에서 메시지가 거치는 경로:

1. 사용자가 웹 인터페이스로 메시지 제출
2. 내 서버가 그 텍스트를 담은 요청 수신
3. 서버가 **Bedrock 클라이언트**로 AWS Bedrock에 요청 (사용자 메시지 + **모델 ID** 포함)
4. 선택한 모델이 텍스트 생성
5. AWS Bedrock이 assistant 메시지로 응답 반환
6. 서버가 응답을 사용자 브라우저로 전달

## Bedrock 클라이언트 설정

`boto3`로 Bedrock runtime 서비스에 연결하는 클라이언트를 만듭니다.

```python
import boto3

client = boto3.client("bedrock-runtime", region_name="us-west-2")
```

첫 요청에 필요한 세 요소: **Bedrock Runtime Client**(연결), **Model ID**(실행할 모델), **User Message**(입력 텍스트).

## 모델 ID와 리전 가용성

⚠️ **모든 모델이 모든 AWS 리전에 있는 것은 아닙니다.** 선택한 리전에 없는 모델을 실행하려 하면 "모델이 존재하지 않는다"는 모호한 오류가 납니다. 예: Sonnet이 `us-west-2`엔 있는데 `us-east-1`에서 요청하면 실패.

### Inference profile로 해결

**Inference profile**은 요청을 모델이 실제로 호스팅된 리전으로 **자동 라우팅**해 리전 가용성 문제를 해결합니다. 어느 모델이 어느 리전에 있는지 추적할 필요 없이, inference profile이 여러 리전(예: `us-west-2`, `us-east-2`)에 걸쳐 알아서 올바른 리전으로 보냅니다.

> inference profile ID는 AWS Bedrock 콘솔의 메인 모델 카탈로그가 아니라 **"Cross-region inference"** 아래에서 찾아 복사합니다.

## User 메시지 구조

```python
user_message = {
    "role": "user",
    "content": [
        {"text": "What's 1+1?"}
    ]
}
```

`content`가 **리스트**인 이유는 한 메시지에 텍스트·이미지 등 여러 유형을 담을 수 있기 때문입니다(멀티모달 요청 가능).

## 요청 보내기 — `converse`

```python
response = client.converse(
    modelId=model_id,
    messages=[user_message]
)
```

응답에는 많은 메타데이터가 있고, 생성된 텍스트만 얻으려면 구조를 파고듭니다.

```python
response["output"]["message"]["content"][0]["text"]
```

## 메시지 유형

- **User 메시지** — 모델에 넣을 내용 (`role: "user"`)
- **Assistant 메시지** — 모델이 생성한 내용 (`role: "assistant"`)

둘 다 role + content 리스트로 **같은 구조**입니다. Bedrock이 돌려주는 assistant 메시지도 user 메시지와 동일한 형식이라, user/assistant를 번갈아 이어붙여 멀티턴 대화를 쉽게 만들 수 있습니다.

## 핵심 정리

- Bedrock 접속은 `boto3.client("bedrock-runtime", region_name=...)`로 시작합니다.
- 모델은 리전마다 가용성이 달라, **inference profile(cross-region inference)**로 자동 라우팅해 문제를 피합니다.
- 요청은 `converse(modelId, messages=[...])`로 보내고, 텍스트는 `response["output"]["message"]["content"][0]["text"]`로 추출합니다.
- user/assistant 메시지는 같은 구조라 멀티턴 대화를 쉽게 구성할 수 있습니다.
