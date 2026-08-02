---
title: "Accessing Claude on Vertex AI"
title_ko: "Vertex에서 Claude 접속과 요청"
course: claude-with-google-vertex
lesson: 2
---

> 이 정리는 **Vertex AI 고유 부분**만 다룹니다. 이후 개념(멀티턴·시스템 프롬프트·streaming·tool use·RAG·에이전트 등)은 일반 API와 동일하므로 [Building with the Claude API](/courses/claude-with-the-anthropic-api/01-introduction/) 코스를 참고하세요.

## 학습 목표

- Vertex를 통한 요청 흐름과 "서버 경유" 원칙 이해하기
- `anthropic[vertex]` SDK 설치와 `AnthropicVertex` 클라이언트 생성하기
- Vertex 모델 ID 형식을 알고 첫 요청 만들기·응답 파싱하기

## 요청 흐름과 서버 경유 원칙

사용자 입력 → AI 응답은 5단계를 거칩니다: **Request to Server → Request to Vertex → Model Processing → Response to Server → Response to Client.**

⚠️ **클라이언트 코드에서 직접 API를 호출하지 마세요.** 요청에는 비밀 자격증명이 필요한데, 클라이언트 코드에 노출되면 누구나 볼 수 있습니다. 항상 **내가 통제·보안하는 서버를 중개자**로 두고 그 서버가 Vertex에 요청하게 합니다.

서버는 **Anthropic SDK** 또는 Google의 공식 Vertex SDK로 통신합니다. Anthropic은 Python·TypeScript·Go·Ruby용 공식 SDK를 제공합니다.

## SDK 설치

Jupyter 노트북에서 Vertex 지원 포함 Anthropic SDK를 설치합니다.

```python
%pip install "anthropic[vertex]"
```

`[vertex]` 부분이 Google Cloud Vertex AI 연결에 필요한 구성요소를 포함시킵니다.

## 클라이언트 생성

```python
from anthropic import AnthropicVertex

client = AnthropicVertex(region="global", project_id="your-project-id")
model = "claude-sonnet-4@20250514"
```

- `project_id` — 실제 Google Cloud 프로젝트 ID (콘솔의 프로젝트 선택기에서 확인)
- **Vertex 모델 ID 형식** — `claude-sonnet-4@20250514`처럼 `모델명@버전날짜` (Bedrock의 `anthropic.claude-...`와 다름). 변수로 두면 반복 입력을 아낄 수 있습니다.

## 요청 만들기 — `messages.create`

`create` 함수의 핵심 파라미터 세 가지: `model`, `max_tokens`(응답 길이 상한 — 목표가 아니라 예산), `messages`(대화 기록).

```python
message = client.messages.create(
    model=model,
    max_tokens=1000,
    messages=[
        {"role": "user", "content": "What is quantum computing? Answer in one sentence"}
    ]
)
```

각 메시지는 `role`(`"user"` 또는 `"assistant"`)과 `content`(실제 텍스트)를 가진 딕셔너리입니다.

## 응답 추출

응답은 메타데이터가 많은 복잡한 객체입니다. Claude가 생성한 텍스트만 얻으려면:

```python
message.content[0].text
```

## 핵심 정리

- 항상 **서버를 중개자**로 두고, 서버가 Anthropic SDK로 Vertex에 요청합니다 (클라이언트에서 직접 호출 금지).
- 설치는 `%pip install "anthropic[vertex]"`, 클라이언트는 `AnthropicVertex(region=..., project_id=...)`로 생성합니다.
- Vertex 모델 ID는 `claude-sonnet-4@20250514` 형식이며, 요청은 `client.messages.create(...)`, 텍스트는 `message.content[0].text`로 추출합니다.
