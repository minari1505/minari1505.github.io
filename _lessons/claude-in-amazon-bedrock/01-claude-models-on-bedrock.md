---
title: "Claude models on Amazon Bedrock"
title_ko: "Bedrock의 Claude 모델과 모델 ID"
course: claude-in-amazon-bedrock
lesson: 1
---

> 이 정리는 **Amazon Bedrock 고유 부분**(모델 ID·접속·요청)만 다룹니다. 프롬프트 엔지니어링·eval·tool use·RAG·에이전트 등 일반 API 개념은 [Building with the Claude API](/courses/claude-with-the-anthropic-api/01-introduction/) 코스와 겹쳐 여기서는 생략합니다.

## 학습 목표

- Claude 모델 계열(Fable 5·Opus·Sonnet·Haiku)과 선택 기준 이해하기
- Bedrock에서 각 모델을 가리키는 **모델 ID** 파악하기

## Claude 모델 계열

Claude는 세 가지 핵심 계열(**Opus·Sonnet·Haiku**)과, Opus 위에 있는 새 tier **Claude Fable 5**를 제공합니다. 모두 텍스트 생성·코딩·이미지 분석 등 핵심 능력을 공유하며, 차이는 **지능·속도·비용의 균형**입니다.

- **Fable 5** — 가장 강력한 tier. Opus를 넘어서는 가장 어려운 과제용이며 비용이 상당히 높아, 그만한 가치가 있는 작업에만 선별적으로.
- **Opus** — 핵심 계열 중 가장 유능. 복잡한 추론·계획, 장기 다단계 작업에 강함. 지연·비용은 중간~높음.
- **Sonnet** — 지능·속도·비용의 균형점. 강한 코딩 능력 + 빠른 생성으로 대부분의 실무에 적합.
- **Haiku** — 가장 빠른 모델. 속도·비용 최적화. 단, Opus·Sonnet의 reasoning은 지원하지 않아 실시간 사용자 대면에 적합하고 복잡한 문제 해결엔 덜 적합.

## Bedrock 모델 ID

Bedrock에서는 모델을 **모델 ID**로 지정합니다. 예:

- Fable 5 → `anthropic.claude-fable-5`

각 모델의 정확한 ID는 AWS Bedrock 콘솔의 모델 카탈로그(또는 cross-region inference의 inference profile)에서 확인합니다. 요청 시 이 ID를 `modelId`로 전달합니다 (다음 레슨 참고).

## 모델 선택 & 조합

- **Fable 5** — 가장 어려운 과제(추가 능력이 비용을 정당화할 때)
- **Opus** — 강한 추론이 필요한 복잡한 작업 (속도·비용보다 품질)
- **Haiku** — 속도가 가장 중요할 때 (실시간·대량 처리)
- **Sonnet** — 균형이 필요한 대부분의 애플리케이션

많은 팀은 하나만 쓰지 않고 **같은 앱의 다른 부분에 다른 모델**을 씁니다 — 사용자 대면은 Haiku, 주요 비즈니스 로직은 Sonnet, 깊은 추론은 Opus, 가장 어려운 문제는 선별적으로 Fable 5.

## 핵심 정리

- Claude는 Fable 5·Opus·Sonnet·Haiku 계열을 제공하며, 지능·속도·비용의 균형으로 선택합니다.
- Bedrock에서는 모델을 **모델 ID**(예: `anthropic.claude-fable-5`)로 지정합니다.
- 앱의 부분마다 다른 모델을 조합해 성능과 비용을 최적화할 수 있습니다.
