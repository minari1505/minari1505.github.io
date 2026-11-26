---
title: "LLM-Agent-Controller: A Universal Multi-Agent LLM System as a Control Engineer"
date: 2025-08-11
categories:
  - LLM Agent
tags:
  - Agent
  - Multi-Agent
  - Evaluation
  - Control Engineering
toc: true
toc_sticky: true
excerpt: "Supervisor가 9개 전문 에이전트를 조율해 제어공학 문제를 자연어로 푸는 멀티에이전트 시스템과, 에이전트별 평가 지표"
---

## 세 줄 요약

- **제어공학(Control Theory)** 문제를 LLM 기반 **멀티에이전트 시스템**으로 푸는 **LLM-Agent-Controller**를 제안해요.
- **Supervisor**가 9개 전문 에이전트를 조율해서, 사용자가 자연어로 물으면 설계·분석·시뮬레이션 결과를 바로 돌려줘요.
- 실험에서 83% 이상의 문제를 풀었고, 복잡한 공학 분야로 확장할 수 있음을 보였어요.

## 배경

- 제어공학은 로봇이나 기계 시스템이 **안정적이고 효율적으로 움직이도록 설계**하는 학문이에요.
- 지금까지는 **전문가의 깊은 지식과 복잡한 툴박스**가 필요했어요.
- LLM이 여러 분야에서 문제 해결 능력을 보였지만, 제어공학처럼 **정확성과 안정성이 중요한 분야**에 맞춘 시스템은 부족했어요.
- 그래서 LLM을 **제어공학 전문 멀티에이전트 시스템**으로 구성해, **누구나 자연어로 제어 문제를 풀 수 있게** 하는 방안을 제시해요.

## 기존 연구의 한계

| 분류 | 관련 연구 | 부족한 점 | 제조 현장 적용 시 시사점 |
|---|---|---|---|
| **1. 시각 정보 처리** | Keivan et al. (ControlBench) | Bode/Nyquist 같은 시각화 기반 분석에 취약 | 생산률 같은 그래프 기반 모니터링을 보완해야 함 |
| **2. 툴 다양성 부족** | Gou et al. (ControlAgent) | 컨트롤러 설계용 소수 도구만 제공 → 범용 적용이 어려움 | 실제 공정에는 다양한 도구가 필요 → 범용 툴셋 통합 |
| **3. 전문성 일반화** | Jadhav et al. (LLM+FEM), Lu et al. (MDA) | 특정 작업에는 강하지만 전체 과정을 end-to-end로 잇지 못함 | 설계~검증~실행을 하나의 워크플로우로 연결 |
| **4. 협력 방식** | Ni et al. (MechAgents) | 에이전트 역할이 고정적이고 동적으로 협력하지 못함 | 상황에 따라 에이전트 조합과 흐름을 유연하게 바꾸는 구조 |
| **5. 데이터 연동** | ControlAgent, LLM-Controller 등 | 내부 LLM 지식에만 의존, RAG 같은 외부 연동 없음 | 생산 이력·설비 데이터 등 실시간 정보와 연결 |
| **6. 사용자 접근성** | 기존 연구 공통 | 전문가용 UI 중심 | 자연어 명령, 대시보드, PDF·음성 같은 다양한 출력 |
| **7. 자가 수정** | ControlAgent, LLM-Controller | 오류를 인지·수정하는 루프가 없음 | 실시간 대응을 위한 자기진단 + 디버깅 구조 |

## 제안 아키텍처

### 구조

![LLM-Agent-Controller 아키텍처]({{ '/assets/images/papers/llm-agent-controller/architecture.png' | relative_url }})

![LLM-Agent-Controller 동작 흐름]({{ '/assets/images/papers/llm-agent-controller/workflow.png' | relative_url }})

- **Supervisor(감독관)**: 전체 워크플로우를 관리하고 어떤 에이전트를 부를지 결정해요.
- **Controller**: 제어 설계·분석의 중심이고 실제 답을 만들어요.
  - 기존 에이전트의 "도구가 적다"는 단점을 보완하려고, Python Control Library 같은 라이브러리를 도구로 묶어 **140개 이상의 도구**를 넣었어요.

![Controller 도구 구성]({{ '/assets/images/papers/llm-agent-controller/controller-tools.png' | relative_url }})

| 도구 묶음 | 역할 |
|---|---|
| Design Controller Tools | 제어기 설계, 튜닝 |
| Control System Analysis Tools | 시스템의 안정성·성능 평가 |
| System Representation Tools | 제어 시스템을 수학적으로 모델링 |
| Time-domain Simulation Tools | time domain에서의 시스템 응답 시뮬레이션 |

**Controller를 돕는 8개 에이전트**

![보조 에이전트 구성]({{ '/assets/images/papers/llm-agent-controller/agents.png' | relative_url }})

| 에이전트 | 역할 |
|---|---|
| Planner | 어떤 도구를 어떤 순서로 실행할지 계획 |
| Retriever | PDF·URL 같은 외부 자료 검색 (RAG) |
| Researcher | 인터넷 기반 정보 탐색 |
| Reasoner | 문제 성격에 따라 CoT 또는 ToT로 추론 |
| Critic | 결과 검토, 품질 보증 |
| Debugger | 오류 감지와 수정 |
| Memory | 과거 대화·작업 기록 저장과 조회 |
| Communicator | 결과를 원하는 형식·언어(PDF, 음성, 번역 등)로 변환 |

### 동작 방식

1. 사용자가 자연어로 질문해요. 예) "내 시스템에 PID 컨트롤러 설계해줘"
2. Supervisor가 Memory를 먼저 확인하고, Planner로 도구 실행 순서를 설계해요.
3. Controller가 도구를 실행해요. 필요하면 Debugger, Critic을 불러요.
4. 결과를 Memory에 저장하고, Communicator가 원하는 형식으로 전달해요.

모든 에이전트는 공통 출력 형식을 따라요. `Question → Thought → Action → Action Input → Observation`을 반복하다가 `Final Answer`로 끝나는 ReAct 스타일이에요. 에이전트별 prompt는 이 공통 형식에 역할 설명(prefix)과 사용할 도구 목록을 붙이는 구조예요.

## 성능 평가

### 평가 지표 10가지

에이전트마다 "제대로 했으면 1, 아니면 0"인 indicator 함수를 두고, 호출 횟수로 나눈 평균을 점수로 써요.

| 지표 | 평가 대상 | 1이 되는 조건 |
|---|---|---|
| **Efficiency** | Controller | 호출될 때마다 올바른 답을 냄 |
| **Routing** | 에이전트 간 이동 | 다음 에이전트로 가는 조건부 edge를 올바르게 고름 |
| **Arrangement** | 전체 실행 순서 | 응답을 만들기 위한 에이전트 배열이 적절함 |
| **Planning** | Planner | 계획한 도구와 Controller가 실제 실행한 도구가 일치 |
| **Judgement** | Critic | 정답은 통과시키고 오답은 거부함 |
| **Self-Correcting** | Debugger | 오류를 감지하고 고침 |
| **Footprint** | Memory | 대화를 제대로 저장하거나 불러옴 |
| **Delivery** | Communicator | 요청한 형식·언어로 전달함 |
| **Completion** | 시스템 전체 | 최종 답이 올바름 |
| **Total** | 시스템 전체 | 위 지표를 종합한 전체 성능 |

예를 들어 Efficiency는 이렇게 계산해요.

```text
Efficiency = (1/τ) · Σ_i [ (1/N_i) · Σ_j 𝕀(Correct Answer)_ij ]

τ    : 시스템 실행 횟수
N_i  : i번째 실행에서 Controller가 호출된 횟수
𝕀    : i번째 실행의 j번째 호출에서 올바른 답이면 1, 아니면 0
```

### 결과

![범주별 결과]({{ '/assets/images/papers/llm-agent-controller/results.png' | relative_url }})

- 랜덤성을 줄이려고 범주마다 20번씩 실행했어요.
- **System Representation(0.95)**, **Time-domain Simulation(0.94)**에서 특히 높았어요.
- **Controller Design(0.83)**은 상대적으로 약했어요. Efficiency, Planning, Self-Correcting 점수가 낮았고, 복잡하고 결정론적인 작업에 취약했어요.
- 모든 작업이 25초 미만, 0.002달러 미만으로 끝나서 시간·비용 면에서 효율적이었어요.

![LLM별 비교]({{ '/assets/images/papers/llm-agent-controller/llm-comparison.png' | relative_url }})

| 모델 | 특징 |
|---|---|
| GPT-4o | 정확도(0.89)와 속도가 가장 높지만 비용이 큼 |
| Claude 3.7 Sonnet | 안정적이고 메모리 관리가 우수 |
| DeepSeek-V3 | 저비용·빠른 속도, 하지만 정확도가 낮음 |

## 한계

- 멀티에이전트 구조에서 생기는 누적 오류(cumulative errors)
- 에이전트 사이에 입력을 넘길 때 쓰는 범용 파싱 함수의 한계
- 에이전트가 많을수록 prompt와 도구 설명을 설계하기가 복잡해짐

## 결론

- 제어공학 문제를 자동으로 분석·설계·시뮬레이션하는 **범용 멀티에이전트 LLM 시스템**이에요.
- 자연어 질의만으로 전문가 수준의 제어 설계가 가능해서, **비전문가도 제어 문제를 풀 수 있어요.**
- 비용·속도·정확도 사이에 트레이드오프가 있어요.
- 비전 모델 결합, prompt 최적화, 안정성 향상을 거치면 **기계·로봇공학 전반으로 확장**할 수 있어요.

**프로젝트에 가져갈 점**

- Efficiency, Routing 같은 **에이전트별 다중 지표 평가 체계**
- **에이전트 역할 세분화** (Planner / Critic / Debugger / Memory 분리)
