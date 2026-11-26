---
title: "Understanding the Planning of LLM Agents: A Survey"
date: 2025-05-15
categories:
  - LLM Agent
tags:
  - Agent
  - Planning
  - Survey
toc: true
toc_sticky: true
excerpt: "LLM 에이전트의 planning 전략을 5가지 유형으로 나누고 장단점을 비교한 서베이"
---

> 📄 **Paper**: [arXiv:2402.02716](https://arxiv.org/abs/2402.02716)

## 3줄 요약

- LLM이 planning을 잘하도록 돕는 여러 방법을 소개하는 서베이
- planning 전략을 **5가지 유형**으로 분류
- 각 유형의 장단점 정리

## 논문 선정 이유

- 프로젝트에서 agent를 만들어 볼 기회가 생겼어요. 사용자 입력을 받아 분기 처리한 뒤, 적합한 agent를 실행하는 구조였어요.
- agent 입문자로서 agent의 종류와 방법이 궁금했어요.

## 배경

- ChatGPT 같은 언어모델로 agent를 만들려는 시도가 많아졌어요.
- agent가 일을 잘하려면 **Planning(계획 세우기)**이 중요해요.
- 예전에는 PDDL(Planning Domain Definition Language) 같은 정형화된 방법이나, 시간과 비용이 많이 드는 강화학습(RL)을 썼어요. 이제는 LLM이 자연어만으로도 계획을 짤 수 있어서 주목받고 있어요.

## 전략 5가지

![LLM-Agent planning taxonomy]({{ '/assets/images/papers/planning-survey/taxonomy.png' | relative_url }})

### 1. Task Decomposition (작업 나누기)

복잡한 일을 작게 나눠서 처리해요. 예를 들어 요리는 재료 준비 → 조리 → 플레이팅으로 나눌 수 있어요.
**요리, 프로그래밍처럼 단계가 여러 개인 복잡한 작업**에 유용해요.

![Decomposition-first vs Interleaved decomposition]({{ '/assets/images/papers/planning-survey/decomposition.png' | relative_url }})

**(a) Decomposition-first: 먼저 다 나눈 다음 순서대로 처리**

- **HuggingGPT**
- **Plan-and-Solve**: zero-shot CoT를 개선한 방법. "Let's first devise a plan", "Let's carry out the plan" 두 단계 prompt를 사용
- **ProgPrompt**: 자연어 task를 코드 문제로 바꿔서, planning을 함수 호출 생성 문제로 만듦

**(b) Interleaved: 중간중간 나누면서 처리**

- **CoT 계열 (CoT, zero-shot CoT)**: few-shot 예시로 추론 과정을 유도
- **ReAct**: 추론과 행동을 나눠서 번갈아 수행
- **PAL, PoT**: 추론 과정을 코드로 표현
- **Visual ChatGPT**: ReAct에 이미지 처리 능력을 더함

### 2. Multi-plan Selection (여러 계획 중 고르기)

계획을 여러 개 만든 다음 가장 좋은 것을 골라요. 시험 문제의 답을 여러 개 떠올린 뒤 가장 확실한 것을 고르는 것과 같아요.
**계획이 불완전하거나 정답이 하나가 아닐 때** 유용해요.
LLM 디코딩 단계의 sampling 전략을 쓰거나, prompt로 다양한 계획을 만들라고 지시해요.

- **Self-consistency**: sampling
- **ToT (Tree of Thoughts)**: sampling, propose
- **GoT (Graph of Thoughts)**: thought의 변환과 집계를 추가해 ToT를 확장
- **LLM-MCTS**
- **RAP**: LLM을 MCTS의 heuristic policy 함수로 활용

### 3. External Planner-Aided (외부 도구의 도움 받기)

LLM 혼자 풀기 어려운 문제는 외부 planner와 협업해요. LLM이 문제를 정리하고, 실제 계산은 다른 프로그램이 해요.
**수학, 로봇 제어, 논리 퍼즐처럼 정형화된 문제**에 유용해요.

**Symbolic Planner (기호 기반)**: 사람이 미리 정한 규칙과 언어(PDDL)로 계획을 세워요. LLM은 자연어 문제를 PDDL 형식으로 바꿔요. 정확도가 중요한 작업에 맞아요.

- **LLM+P**: LLM이 문제를 PDDL로 정리하고, 외부 PDDL planner가 실행
- **LLM+PDDL**: LLM이 만든 계획을 사람이 검토·수정한 뒤 그걸 바탕으로 계획 수립
- **LLM+ASP**: PDDL 대신 ASP 논리 규칙 언어 사용

**Neural Planner (신경망 기반)**: 학습 데이터로 훈련한 모델이 계획을 짜요. LLM이 환경을 설명하면 신경망이 다음 행동을 추천해요. 게임처럼 복잡하고 빠른 판단이 필요한 곳에 맞아요.

- **DRRN**: RL 기반으로 어떤 문장이 더 좋은지 판단
- **DT (Decision Transformer)**: 사람의 과거 행동 데이터로 학습한 transformer
- **SwiftSage**: DT + LLM. 쉬운 건 DT가, 어려운 건 LLM이 처리

### 4. Reflection and Refinement (반성과 수정)

한 번에 완벽한 계획을 못 세워도, 실수를 되짚어 보고 다시 시도해요. Cursor의 agent가 코드를 짜고 실행해 보고, 에러가 나면 고치는 과정과 같아요.
**할루시네이션이나 추론이 부족한 문제**를 풀 때 유용해요.

- **Self-refine**: 계획 → 피드백 → 수정 반복
- **Reflexion**: ReAct를 확장. 에러를 감지하면 self-reflection으로 무엇이 틀렸는지 설명하고 고침
- **CRITIC**: knowledge base, 검색 엔진 같은 외부 도구로 계획을 검증·수정
- **InteRecAgent**: ReChain 메커니즘으로 여러 번 틀린 예시를 모아 학습

### 5. Memory-augmented Planning (기억력 강화)

별도의 memory 모듈로 planning 능력을 높여요. 과거의 경험이나 지식을 기억했다가 다시 꺼내 써요.
**장기적인 agent 학습, 캐릭터 시뮬레이션**에 유용해요.

**RAG 기반: 문서를 검색해서 참고**

- **MemGPT**: LLM 메모리를 컴퓨터처럼 RAM과 디스크로 나눠 저장하고 불러옴
- **MemoryBank**: 중요한 경험을 벡터로 저장했다가 비슷한 상황에서 꺼내 씀
- **Generative Agents**: 가상 캐릭터가 대화와 경험을 기억해 자연스럽게 행동

**Fine-tuning 기반: 과거 경험을 모델에 학습**

- **CALM**: 게임 환경의 실제 행동 데이터로 LLM을 학습
- **AgentTuning**: 다양한 작업 계획을 대화 형태로 학습시켜, 미리 경험해 본 것처럼 행동
- **TDT**: 과거 행동 데이터로 학습한 모델

## 각 전략의 장단점

| 전략 | 장점 | 단점 |
|---|---|---|
| Task Decomposition | 복잡한 문제를 구조화할 수 있음 | 너무 잘게 나누면 context limit에 걸림 |
| Multi-plan Selection | 여러 선택지 중 최적을 고를 수 있음 | 연산량 증가, 시간·비용 부담 |
| External Planner | 정확하고 정형화된 문제에 강함 | 외부 도구가 필요하고 유연성이 떨어짐 |
| Reflection & Refinement | 오류를 고칠 수 있고 학습 효과가 큼 | 반복 때문에 느리고 비효율적일 수 있음 |
| Memory-Augmented | 지속적인 학습·기억 가능 | 기억 관리와 품질 유지가 어렵고 비용이 큼 |

## 결론: 프로젝트에 적용한다면

- 사용자 입력을 분기 처리하는 부분은 **Task Decomposition**, 그중에서도 **decomposition-first** 방식이 맞아 보여요.
- 적합한 DB를 불러오는 SQL을 만들 때는 **Multi-plan Selection**을 써 봐도 좋겠어요.
