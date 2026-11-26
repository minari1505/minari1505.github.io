---
title: "Plan-and-Act: Improving Planning of Agents for Long-Horizon Tasks"
date: 2025-05-29
categories:
  - LLM Agent
tags:
  - Agent
  - Planning
  - Synthetic Data
toc: true
toc_sticky: true
excerpt: "Planner와 Executor를 나누고, 합성 데이터와 동적 재계획으로 긴 작업의 성공률을 끌어올린 프레임워크"
---

> 📄 **Paper**: [arXiv:2503.09572](https://arxiv.org/abs/2503.09572)

## 3줄 요약

- **PLAN-AND-ACT**: Planner와 Executor를 명시적으로 나눈 framework를 제안해요.
- **Planner**는 전체 목표를 단계별 하위 목표로 나누고, **Executor**는 각 단계를 순서대로 실행하면서 필요하면 계획을 고쳐요.
- 여러 benchmark에서 기존 SOTA보다 좋은 성능을 보였어요.

## 논문 선정 이유

진행 중인 프로젝트의 agent는 rule-based로 동작하고 있었어요. AI가 어떤 데이터를 쓸지 직접 판단하고, 그걸 바탕으로 답변을 만드는 agent로 바꾸고 싶었어요. planning agent 구조로의 전환을 고민하던 중에 읽었어요.

## 배경

- agent가 실무에 쓰이려면, 한 번에 끝나지 않는 복잡한 작업을 잘 처리해야 해요.
- LLM은 복잡한 계획을 세우도록 훈련되지 않아서, 계획을 정확하고 정교하게 세우기 어려웠어요.
- ⇒ 계획은 정교하게 세우고, 하위 단계는 정확하고 일관되게 수행하는 것이 목표예요.

## 핵심 아이디어

긴 작업을 잘 수행하도록 **Planning과 Acting을 나누고**, 둘을 **반복적으로 연결**해요.

### 구조

![Plan-and-Act 구조]({{ '/assets/images/papers/plan-and-act/structure.png' | relative_url }})

0. **User Query**: 목표. 예) 집에서 학교 가기
   - Planner가 계획을 만들기 전, Executor가 행동하기 전에 CoT로 추론 과정을 먼저 생각하게 해요.
1. **Plan (Planner)**: 목표를 이루기 위한 단계를 계획해요.
   1. 집에서 나와 버스 정류장으로 가기
   2. 301번 버스를 타고 ㅇㅇ대학교 정류장에서 내리기
   3. 정문에서 공학관까지 걸어가기
2. **Act (Executor)**: Planner의 계획을 보고 행동해요.
   - 예) 1번을 수행할 때: 걷기 → 문 열기 → 계단 내려가기 → 왼쪽으로 10걸음 → …
3. **Critic**: 계획대로 수행됐는지 점검하고, 필요하면 계획을 고쳐요.
   - 실행 중 예상하지 못한 에러가 생기면, 같은 계획을 반복하는 대신 계획을 업데이트해서 목표를 달성하도록 해요.
4. **반복**

### 데이터 증강

![Plan-and-Act 데이터 증강]({{ '/assets/images/papers/plan-and-act/augmentation.png' | relative_url }})

**1. Action Trajectory Generation: Executor 학습용 데이터**

1. 기존 학습 데이터에서 사용자 질문 몇 개를 무작위로 뽑아요.
2. 다른 LLM이 이를 바탕으로 새 질문을 만들어요. 예) "샌프란시스코행 항공편 예약" → "뉴욕에서 런던 가는 기차표 찾기"
3. 다른 LLM agent가 작업을 수행하고 과정을 기록해요.
4. 성공 여부를 확인해요.
5. 성공한 기록만 학습 데이터로 모아요.

**2. Grounded Plan Generation: 1번을 역설계**

1번만으로는 실제 웹사이트나 환경 정보가 없어서, 엉뚱하거나 실행할 수 없는 계획이 나올 수 있어요.

1. 성공한 1번 기록을 LLM에 주고, 그 행동들이 어떤 high-level plan을 따랐을지 역설계하게 해요.
2. 계획의 각 단계가 실제 행동 기록의 어떤 행동과 이어지는지 명시해요.

**3. Synthetic Plan Expansion: Planner·Executor 데이터 비율 맞추기**

- 행동 평균 8개당 plan 데이터가 1개 생겨서, Planner 데이터가 훨씬 적어요.
- 이미 가진 성공한 "질문 – 계획" 데이터를 바탕으로 LLM이 데이터를 늘려요. 논문에서는 이렇게 1만 개를 추가로 만들었어요.

**+ Targeted Plan Augmentation: 약한 부분 집중 보강**

- training·validation에서 실패한 작업 유형을 분석해요.
- 실패 패턴에 맞춘 학습 데이터를 더 만들어 보완해요.

## 결과

![Plan-and-Act 결과]({{ '/assets/images/papers/plan-and-act/results.png' | relative_url }})

| 단계 | 성공률 |
|---|---|
| Planner 없이 Executor만 | 9.85% |
| Planner 도입 | 14.21% |
| 합성 데이터 추가 | 24.24% |
| 합성 planner 데이터 1만 개 추가 | 39.40% |
| 실패 사례 분석 기반 5,000개 추가 | 43.63% |
| Dynamic Replanning 적용 | 53.94% |
| CoT 적용 | **57.58%** |

기본 Executor는 그대로 두고 dynamic replanning을 적용한 Planner만 써도 9.85% → 44.24%로 크게 올랐어요. 실행 능력보다 **계획 능력이 더 중요한 것** 같아요.

## 프로젝트 적용 방안

1. **Planner 도입**: 사용자 질문을 분석해 필요한 데이터 소스(RDB, VectorDB, Web Search 등)를 판단하고, 고수준 계획을 생성
2. **동적 재계획**: 실행 중 환경이 바뀌거나 예상 밖의 상황이 생기면 Planner가 새 계획을 세워 적응
