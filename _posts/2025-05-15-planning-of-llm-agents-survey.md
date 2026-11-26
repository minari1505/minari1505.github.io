---
title: "Understanding the planning of LLM agents- A survey"
date: 2025-05-15
categories:
  - LLM Agent
tags:
  - Agent
  - Planning
  - Survey
toc: true
toc_sticky: true
excerpt: "LLM이 어떤 방식으로 ‘planning’를 잘 할 수 있게 도와주는지 다양한 방법들을 소개함"
---

<https://arxiv.org/pdf/2402.02716v1>

## 3줄 요약

- LLM이 어떤 방식으로 ‘planning’를 잘 할 수 있게 도와주는지 다양한 방법들을 소개함
- planning을 잘 하기 위해 사용하는 전략을 5가지 유형으로 분류함
- 각 방법의 장단점 정리

## 논문 선정 이유

- 사내 프로젝트에서 agent를 만들어 볼 기회가 생김
  - user input을 받았을 때 분기처리 진행 후 적합한 agent를 실행
- agent 뉴비로써 agent의 종류와 방법에 대해 궁금했음

## 배경

- 요즘 chatGPT 같은 언어모델을 활용해 agent를 만들려는 사람들이 많이 생김
- agent가 일을 잘하기 위해서는 Planning(계획 세우기)가 아주 중요함
- 기존에는 수학공식같은 정형화된 방법(PDDL, Planning Domain Definition Language)이나 RL(Reinforcement Learning)처럼 시간과 비용이 많이 드는 방법을 사용했으나, 이제는 LLM이 자연어로도 똑똑하게 planning을 짤 수 있다고 해서 관심을 많이 받는 상황

## 전략 5가지

![]({{ '/assets/images/papers/planning-survey/taxonomy.png' | relative_url }})

**1. Task Decomposition(작업 나누기)**

- 복잡한 일을 작게 나누어 처리
  - ex) 요리하기 / 재료 준비 → 조리하기 → 플레이팅
- **복잡한 멀티스텝 작업(요리, 프로그래밍) 등에 유용**
- 방식

  ![]({{ '/assets/images/papers/planning-survey/decomposition.png' | relative_url }})

  1. 먼저 다 나눈 다음 순서대로 처리(Decomposition-first)
     - 종류
       - HuggingGPT
       - Plan-and-Solve : zero-shot CoT를 개선. ‘Let’s first devise a plan’, ‘Let’s carry out the plan’의 두 단계 prompt 사용
       - ProgPrompt : 자연어 task를 code 문제로 변환, planning을 함수 호출 생성으로 만듦
  2. 중간중간 나누며 처리 :
     - 종류
       - CoT계열(CoT, zero-shot CoT) : few-shot 예시를 통해 추론과정 유도
       - ReAct : 추론 & planning 분리, 번갈아 수행
       - PAL, PoT : 추론과정을 code로 표현
       - Visual ChatGpt : ReAct + 이미지 처리능력

**2. Muli-plan Selection(다양한 계획 중 고르기)**

- 여러 계획을 먼저 만든 다음, 그 중 제일 좋은 것을 선택하는 방식
  - ex) 시험 문제 답을 여러개 떠올린 후, 그 중 가장 확실한 것을 고르기
- **계획이 불완전하거나 답이 하나가 아닐 때 유용**
- LLM의 decoding process에서 sampling전략을 사용하거나, prompt로 다양한 계획 생성 지시
  - 종류
    - Self-consistency : sampling
    - ToT : sampling, propose
    - GoT : Thought의 변환 & 집계를 추가해 ToT 확장
    - LLM-MCTS
    - RAP : LLM을 MCTS의 heuristic policy 함수로 활용

**3. External Planner-Aided(외부 도구 도움 받기)**

- LLM 혼자 하기 힘든 경우, 외부의 계획 수립 프로그램(PDDL 등)과 협업
  - ex) 문제를 LLM이 정리, 실제 계산은 다른 프로그램이 진행
- **수학, 로봇제어, 논리 퍼즐같은 정형화된 문제에 유용**
- 방식
  - Symbolic Planner(기호 기반 플래너)
    - 사람이 미리 정해둔 규칙&언어(PDDL)을 이용해 계획을 세우는 방식. 자연어 문제를 LLM이 PDDL 형식으로 변경
    - **수학문제 해결처럼 정확도를 요구하는 작업에 유용**
    - 종류
      - LLM+P : LLM이 PDDL 형식으로 문제를 정리하고, 외부 PDDL 플래너가 실행
      - LLM+PDDL : LLM이 만든 계획을 사람이 검토해서 수정하고, 그걸 기반으로 계획을 세움
      - LLM+ASP : PDDL 대신 ASP 논리 규칙 언어를 써서 계획 수립
  - Neural Planner(신경망 기반 플래너)
    - 딥러닝 모델이 training data를 바탕으로 계획을 짜는 방식. LLM이 환경을 설명하고, 신경망이 직접 다음 행동을 추천함
    - **게임처럼 복잡하고 빠른 판단이 필요한 곳에 유용**
    - 종류
      - DRRN : RL 기반, 어떤 문장이 더 좋은지 판단하는 모델
      - DT(Decision Transformer) : 과거 사람의 행동 데이터를 보고 배운 transformer
      - SwiftSage : DT + LLM. 쉬운건 DT가, 어려운 건 LLM이 처리함

**4. Reflection and Refinement(반성 & 수정)**

- 한 번에 완벽한 계획을 세우지 못하더라도, 실수를 되짚어보고 재시도
  - ex) Cursor의 agent 사용 시 스스로 코드를 짜고 실행해보고, 에러가 나면 고치는 과정
- **할루시네이션 & 불충분한 추론 문제 해결 시 유용**
- 종류
  - Self-refine : 계획 → 피드백 → 수정 반복
  - Reflexion : ReAct 확장. 에러 감지 시 self reflection해서 잘못된 점을 설명하고 고침
  - CRITIC : knowledge bases, search engines 등 외부 도구를 활용해 계획 검증 & 수정
  - InteRecAgent : ReChain 메커니즘 사용, 여러 번 틀린 예시를 모아 학습

**5. Memory-augmented Planning(기억력 강화)**

- 추가 memory 모듈을 사용해 planning 능력 향상시키는 방법. 과거의 경험이나 지식을 기억하고 있다가 다시 사용할 수 있도록 도와줌
- **장기적인 agent 학습, 캐릭터 시뮬레이션에 유용**
- 방식
  - RAG 기반 : 문서를 검색해 참고함
    - 종류
      - MemGPT : LLM 메모리를 컴퓨터처럼 RAM & 디스크로 나누어 저장/불러오기
      - MemoryBank : 중요한 경험들을 벡터로 저장, 추후 비슷한 상황에 맞춰 꺼내 사용
      - Generative Agents : 가상의 캐릭터가 대화, 경험 등을 기억해 자연스럽게 행동함
  - fine-tuning : 과거 경험을 모델에 학습시킴
    - 종류
      - CALM : 게임 환경에서 실제 행동 데이터를 모아 LLM에 학습시킴
      - AgentTuning : 다양한 작업 계획을 대화 형태로 LLM에 학습 → 미리 경험한 것처럼 행동
      - TDT : 과거 행동 데이터를 이용해 학습한 모델

## 각 전략의 장단점

| 전략 | 장점 | 단점 |
|---|---|---|
| Task Decomposition | 복잡한 문제를 구조화할 수 있음 | 너무 많이 나누면 context limit에 걸림 |
| Multi-plan Selection | 다양한 선택지 중 최적 선택 가능 | 연산량 증가, 시간/비용 부담 |
| External Planner | 정확하고 정형화된 문제에 강함 | 외부 도구가 필요, 유연성 떨어짐 |
| Reflection & Refinement | 오류 수정 가능, 학습 효과 높음 | 반복 때문에 느리고 비효율적일 수 있음 |
| Memory-Augmented | 지속적인 학습/기억 가능 | 기억 관리 및 품질 유지가 어렵고 비용 많이 듦 |

## 결론

- user input을 분기처리 하는 부분 : Task Decomposition → decomposition-first가 적합
- 적합한 DB를 불러오는 SQL문을 만들 때, Multi-plan Selection 을 사용해도 좋을듯
