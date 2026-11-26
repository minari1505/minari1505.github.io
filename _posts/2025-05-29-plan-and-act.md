---
title: "Plan-And-Act : Improving Planning of Agents for Long-Horizon Tasks"
date: 2025-05-29
categories:
  - LLM Agent
tags:
  - Agent
  - Planning
  - Synthetic Data
toc: true
toc_sticky: true
excerpt: "PLAN-AND-ACT : Planner와 Executor를 명시적으로 분리하는 framework를 제안"
---

<https://arxiv.org/pdf/2503.09572v3>

## 3줄 요약

- PLAN-AND-ACT : Planner와 Executor를 명시적으로 분리하는 framework를 제안
- Planner : 전체 목표를 단계별 하위 목표로 나누기 / Executor : 각 단계를 순차적으로 실행하며 필요 시 계획 수정
- 다양한 benchmark에서 SOTA 모델보다 더 나은 성능을 입증함

## 논문 선정 이유

- 기존 사내 프로젝트의 agent는 rule-based로 작동하고 있음. 이제는 AI 가 직접 어떤 데이터를 쓸 지 판단하고, 이를 기반으로 답변을 생성하는 지능적인 agent를 만들고자 함. 이를 위해 planning agent 구조로의 전환을 고민중

## 배경

- agent가 실무에 쓰이기 위해서는 한 번에 끝나지 않는 복잡한 작업을 잘 처리해야 함.
- LLM은 복잡한 계획을 세우도록 훈련되어있지 않았기 때문에, 계획 자체를 정확하고 정교하게 세우는 것이 어려웠음

  ⇒ PLAN-AND-ACT 방법을 사용함으로써 계획을 정교하게 세우고, 하위의 단계를 정확하고 일관되게 수행할 수 있길 원함

## 핵심 아이디어

- LLM agent가 장기적인 작업을 더 잘 수행하도록, Planning과 Acting을 나눔
- Planning과 Acting을 반복적으로 연결함

### 구조

![]({{ '/assets/images/papers/plan-and-act/structure.png' | relative_url }})

0\. **User Query** : 목표

ex) 집에서 학교 가기

<span style="color:gray">\*\* PLAN-AND-ACT에서 CoT를 활용함. Planner가 계획을 만들기 전, Executor가 행동하기 전 ‘추론과정’을 먼저 생각하게 함</span>

1\. **Plan(Planner)**

<details markdown="1">
<summary>prompt</summary>

![]({{ '/assets/images/papers/plan-and-act/planner-prompt-1.png' | relative_url }})

![]({{ '/assets/images/papers/plan-and-act/planner-prompt-2.png' | relative_url }})

</details>

- 사용자가 원하는 목표를 달성하기 위해, 작업을 단계별로 계획

  ex) 1\. 집에서 나와서 버스 정류장 가기<br>
  2\. 301번 버스 타고 ㅇㅇ대학교 정류장에서 내리기<br>
  3\. 학교 정문에서 공학관까지 걸어가기

2\. **Act(Executor)**

<details markdown="1">
<summary>prompt</summary>

![]({{ '/assets/images/papers/plan-and-act/executor-prompt.png' | relative_url }})

</details>

- planner가 세운 계획을 보고 행동

ex) Planner model의 1번을 수행할 때 : 왼발, 오른발 번갈아 걷기 → 문 열기 → 계단 내려가기 → 왼쪽으로 10걸음 걷기 → …

3\. **Critic**

- Act에서 Plan대로 잘 수행됐는지 점검. 필요하면 Plan을 수정함
- Plan을 수행하는 과정에서 예상치 못한 에러가 발생할 수 있음. 이런 경우, Executor가 해당 Plan을 반복하기 보단 계획을 업데이트해 목표(User Query)를 성공적으로 수행할 수 있도록 함

4\. **반복**

### 데이터 증강

![]({{ '/assets/images/papers/plan-and-act/augmentation.png' | relative_url }})

1\. **Executor 모델을 학습시키기 위한 데이터 증강 (Action Trajectory Generation)**

(1) 기존 질문 활용

- 가지고 있는 학습 데이터에서 사용자 질문 몇가지를 랜덤으로 뽑기

(2) 위 질문들을 바탕으로 다른 LLM이 새로운 질문을 생성

ex) 샌프란시스코행 항공편 예약 → 뉴욕에서 런던까지 가는 기차표 찾기

(3) 다른 LLM agent가 해당 작업을 수행, 수행한 것을 기록

(4) 성공 여부 확인

(5) 성공한 데이터만 학습 데이터로 수집

2\. **1번을 역설계한 데이터 증강 (Grounded Plan Generation)**

- 1번만으로는 실제 웹사이트나 환경에 대한 정보가 없으므로, 엉뚱하거나 실행 불가능한 계획을 만들 수 있음

(1) 성공적인 1번 기록을 LLM에게 주고, 이 행동들이 어떤 high-level plan을 따랐을 지 역설계하도록 요청

(2) 계획을 만들 때, 각 계획 단계가 실제 행동 기로의 어떤 행동과 연결되는지 명시

3\. **Planner - Executor 데이터의 비율 맞추기 (Synthetic Plan Expansion)**

- 평균 8번의 행동에 대해 1개의 plan 데이터가 생성되므로, Executor 데이터에 비해 Planner 데이터가 훨씬 부족
- 기존에 가지고 있는, 성공한 ‘사용자 질문 - 계획’ 데이터를 바탕으로 LLM을 통해 데이터 증강(이 논문에선 이렇게 10000개의 추가 데이터를 생성했다고 함)

**+Target Plan Augmentation** : 모델이 특히 어려워하는 부분을 집중적으로 보강

<details markdown="1">
<summary>prompt</summary>

![]({{ '/assets/images/papers/plan-and-act/target-prompt-1.png' | relative_url }})

![]({{ '/assets/images/papers/plan-and-act/target-prompt-2.png' | relative_url }})

</details>

- training, validation 과정에서 실제로 실패했던 작업 유형 분석
- 실패 패턴 파악해, 맞춤형 training data를 더 많이 생성해 해당 부분 보완

## 결과

![]({{ '/assets/images/papers/plan-and-act/results.png' | relative_url }})

- Planner 없이 Executor만 사용 : 9.85%
- Planner 도입 : 9.85% → 14.21%
- 합성 데이터 생성을 통해 planner 학습 데이터를 늘렸을 때 성능 향상 확인
  - 합성 데이터 추가 : 24.24%
  - 10000개의 추가 합성 planner 데이터 추가 : 27.10% → 39.40%
  - 실패 사례 분석 기반 5,000개 추가 : 39.40% → 43.63%
- Dynamic Replanning 적용 : 43.63% → 53.94%
  - 기본 executor를 사용했을 때에도, dynamic replanning을 적용한 planner만으로 성능이 크게 향상되었다고 함 (9.85% → 44.24%). <span style="color:gray">실행 능력보다 계획 능력이 중요한 듯</span>
- CoT 적용 : 53.94% → 57.58%

## 현재 프로젝트로의 적용 방안

1. **Planner 도입**: 사용자의 질문을 분석하여 필요한 데이터 소스(RDB, VectorDB, Web Search 등)를 판단하고, 이를 기반으로 고수준 계획을 생성
2. **동적 재계획**: 실행 중 환경의 변화나 예기치 않은 상황이 발생할 경우, Planner가 새로운 계획을 생성하여 적응할 수 있도록 동적 재계획
