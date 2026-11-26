---
title: "LLM-Agent-Controller: A Universal Multi-Agent Large Language Model System as a Control Engineer"
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
excerpt: "이 논문은 제어공학(Control Theory) 문제를 LLM 기반 멀티에이전트 시스템으로 해결하는 LLM-Agent-Controller를 제안"
---

## 세 줄 요약

- 이 논문은 **제어공학(Control Theory)** 문제를 LLM 기반 **멀티에이전트 시스템**으로 해결하는 **LLM-Agent-Controller**를 제안
- 9개 전문 에이전트를 **Supervisor**가 조율하여, 사용자가 자연어로 질의하면 실시간 설계·분석·시뮬레이션 결과 제공
- 실험에서 83% 이상 문제 해결, 복잡한 공학 분야로 확장 가능성을 입증

## 논문이 쓰여진 배경

- 제어공학은 로봇이나 기계 시스템을 **안정적이고 효율적으로 움직이도록 설계**하는 학문
- 지금까지는 **전문가의 깊은 지식과 복잡한 툴박스**가 필요했음
- 최근 LLM이 다양한 분야에서 문제 해결 능력을 보였지만, 제어공학처럼 **정확성과 안정성이 중요한 분야**에는 최적화된 시스템이 부족
- 따라서 이 논문에서는 LLM을 **제어공학 전문 다중 에이전트 시스템**으로 구성해 **누구나 자연어로 제어 문제를 해결**할 수 있는 방안을 제시

## 기존 연구에서 드러난 한계점

| 분류 | 관련 연구(Agent 시스템) | 부족한 점 | 구체 설명 | 제조 현장 프로젝트 시사점 |
|---|---|---|---|---|
| **1. 시각 정보 처리** | **Keivan et al. (ControlBench)** – LLM 시각 기반 제어 문제 해결 평가 | 시각적 분석 미흡 | Bode/Nyquist 같은 **시각화 기반 제어 분석**에서 LLM들이 취약 (Keivan et al.) | 공정 데이터의 **그래프 기반 모니터링**(생산률 등)에 약할 수 있음 → 보완 필요 |
| **2. 툴 다양성 부족** | **Gou et al. (ControlAgent)** – LLM+Python 계산 에이전트로 컨트롤러 설계 | 툴셋이 제한됨 | ControlAgent 등은 컨트롤러 설계에 특화된 소수의 도구만 제공 → 범용 적용 어려움 (Gou et al.) | 실제 공정에는 다양한 상황/도구 필요 → **범용 툴셋 통합 필수** |
| **3. 전문성 일반화 어려움** | **Jadhav et al. (LLM+FEM)**, **Lu et al. (MDA)** – 특정 설계 영역에 특화된 에이전트 | 특정 작업에는 강하지만, 전체 워크플로우엔 부족 | 일부는 특정 작업(설계, 추론 등)에는 강하나, **전 과정을 end-to-end로 연결하는 데는 미흡** (Jadhav, Lu 등) | **설계~검증~실행**까지 하나의 워크플로우로 연결하는 시스템 필요 |
| **4. 협력 방식 한계** | **Ni et al. (MechAgents)** – FEM 문제 해결 시 다중 LLM 협력, 그러나 고정적 역할 | 다중 에이전트 협력이 미흡하거나 정적 | 에이전트들이 동적으로 협력하지 못하거나, 고정된 역할만 수행함 (Ni et al.) | 상황 변화에 따라 **에이전트 조합과 흐름을 유연하게 바꾸는 구조 필요** |
| **5. 데이터 연동 부족** | 대부분 기존 연구 (ControlAgent, LLM-Controller)에서 외부 데이터 연동 미비 | 외부 지식/문서 연동이 약함 | 대부분 내부 LLM 지식에 의존하며, **RAG 같은 외부 데이터 검색/연동 구조가 없음** | 생산 이력, 설비 데이터 등 **실시간 공장 정보와의 연결 필수** |
| **6. 사용자 접근성 낮음** | 모든 기존 연구에서 공통 한계 – 전문가 중심 UI | 전문가용 시스템 중심 | 대부분 기술적 사용자 대상. **현장 작업자/관리자에게 친화적이지 않음** | 자연어 명령, 대시보드, 다양한 출력 포맷(PDF, 음성 등) 필요 |
| **7. 자가 수정 능력 부족** | **Gou et al. (ControlAgent)**, **Zahedifar et al. (LLM-Controller)** – 오류 검증/수정 구조 미흡 | 오류를 인지하고 수정하는 기능 부족 | 오류를 내도 그대로 출력하거나, 수정 루프가 없음 (일부 연구는 manual feedback 필요) | 실시간 대응·복구 가능한 **자기진단 + 디버깅 구조 필수** |

## 제안 아키텍처 소개

### (1) 구조

![]({{ '/assets/images/papers/llm-agent-controller/architecture.png' | relative_url }})

![]({{ '/assets/images/papers/llm-agent-controller/workflow.png' | relative_url }})

- **Supervisor(감독관)** : 전체 워크플로우를 관리하며 어떤 에이전트를 호출할지 결정
- **9개의 전문 에이전트**
  - Controller: 제어 설계·분석 중심 역할, 실제 답 생성
    - 기존 LLM 에이전트들의 ‘한정된 툴만 사용 가능’하다는 단점을 보완하기 위해, Python Control Library와 같은 기존 라이브러리를 도구로 통합, 140개 이상의 도구 탑재

    ![]({{ '/assets/images/papers/llm-agent-controller/controller-tools.png' | relative_url }})

    - controller agent가 제어공학 문제 해결 시 사용할 도구
      1. Design Controller Tools : 제어기 설계, 튜닝
      2. Control System Analysis Tools : 시스템의 안정성, 성능 평가
      3. System Representation Tools : 제어 시스템을 수학적으로 모델링
      4. Time-domain Simulation Tools : time domain에서의 시스템 응답 시뮬레이션
  - Controller를 보존하는 8개의 agent

    ![]({{ '/assets/images/papers/llm-agent-controller/agents.png' | relative_url }})

    - Planner: 어떤 툴을 어떤 순서로 실행할지 계획
    - Retriever: 외부 자료 검색 (RAG)
    - Researcher: 인터넷 기반 정보 탐색
    - Reasoner: CoT, ToT 기반 추론
    - Critic: 결과 검토 및 품질 보증
    - Debugger: 오류 수정
    - Memory: 과거 대화 및 작업 기록 관리
    - Communicator: 결과를 원하는 형식·언어로 변환

### (2) 동작 방식

1. 사용자가 자연어로 질문 (예: “내 시스템에 PID 컨트롤러 설계해줘”)
2. Supervisor가 Planner를 통해 툴 순서 설계
3. Controller가 툴 실행 (필요 시 Debugger, Critic 호출)
4. Memory에 저장 후 Communicator를 통해 원하는 형식으로 전달

### (3) 성능 평가

- **평가 지표** : Efficiency, Routing, Planning, Completion 등 10개의 세부 지표
  1. **Efficiency Score** : 시스템의 전체 실행횟수동안 Controller Agent가 호출될 때 마다 올바른 답변을 제공한 비율의 평균

     ![]({{ '/assets/images/papers/llm-agent-controller/metric-efficiency.png' | relative_url }})

     - `τ` : LLM-Agent-Controller 시스템이 실행된 횟수
     - `N_i` : 시스템을 한 번 실행하는 동안 Controller Agent가 호출된 횟수
     - `𝕀(Correct Answer)_ij` : Indicator func. Controller Agent가 i번째 실행의 j번째 호출에서 ‘올바른 답변’을 제공했다면 1, 아니라면 0
  2. **Routing Score** : Agent가 적절한 다음 agent로 routing하는지 평가

     ![]({{ '/assets/images/papers/llm-agent-controller/metric-routing.png' | relative_url }})

     - `𝕀(·)` : 다음 agent로의 조건부 edge가 올바르게 선택되면 1, 아니면 0
     - Q : 단일 실행동안 다음 agent로의 routing 결정 횟수
  3. **Arrangement Score** : 응답을 얻기 위해 agent를 최적의 순서로 호출했는지 평가

     ![]({{ '/assets/images/papers/llm-agent-controller/metric-arrangement.png' | relative_url }})

     - `𝕀(·)` : 시스템에서 실행된 agent가 응답을 생성하기 위한 적절한 배열인 경우 1, 아니면 0
     - Q : 단일 실행동안 활용된 agent의 총 횟수
  4. **Planning Score** : Controller agent를 위한 최적의 계획을 세웠는지 평가

     ![]({{ '/assets/images/papers/llm-agent-controller/metric-planning.png' | relative_url }})

     - `𝕀(·)` : Planner Agent가 계획한 도구가 Controller agent가 실행한 도구와 일치하면 1, 아니면 0
     - `N` : 단일 실행동안 Planner Agent가 호출된 총 횟수
  5. **Judgement Score :** Controller Agnet의 output을 검증하는 Critic Agent의 성능 평가

     ![]({{ '/assets/images/papers/llm-agent-controller/metric-judgement.png' | relative_url }})

     - `∨` : 논리 OR 함수
     - `𝕀(·)` : Critic Agnet가 정답을 올바르게 검증하거나 오답을 거부하는 경우 1, 아니면 0
     - `N` : 시스템의 단일 실행동안 Critic Agnet가 활성화되는 총 횟수
  6. **Self-Crorrcting Score :** Controller Agent 실행 중 버그나 문제 발생 시, Debugger Agent가 오류를 감지하고 수정하는 성능 평가

     ![]({{ '/assets/images/papers/llm-agent-controller/metric-self-correcting.png' | relative_url }})

     - `𝕀(·)` : Debugger Agent가 오류를 성공적으로 감지 & 수정하면 1, 아니면 0
     - `N` : 단일 시행 주기 동안 Debugger Agent가 트리거되는 횟수
  7. **Footprint Score** : 질문에 대한 답변이 이미 저장되어있을 때, memory Agent가 새로운 대화를 성공적으로 저장 or 이전 대화를 가져오는 성능 평가

     ![]({{ '/assets/images/papers/llm-agent-controller/metric-footprint.png' | relative_url }})

     - `𝕀(·)` : Memory Agent가 메세지를 성공적으로 저장하거나 가져오면 1, 아니면 0
     - `N` : 단일 실행동안 Memory Agent가 관여하는 횟수
  8. **Delivery Score** : Communicator Agent가 사용자가 요청한 형식으로 답변을 전달했는지 평가

     ![]({{ '/assets/images/papers/llm-agent-controller/metric-delivery.png' | relative_url }})

     - `𝕀(·)` : Communicator Agent가 답변을 올바른 형식이나 언어로 전달했다면 1, 아니면 0
     - `N` : 단일 실행동안 Communicator Agent가 활성화되는 빈도
  9. **Completion Score :** 모든 실행에서 LLM-Agent-Controller가 작업을 올바르게 완료한 비율

     ![]({{ '/assets/images/papers/llm-agent-controller/metric-completion.png' | relative_url }})

     - `𝕀(·)` : 최종 답변이 올바르면 1, 아니면 0
  10. **Total Score :** 모든 실행에서 LLM-Agent-Controller 시스템 내 agent의 전체 성능

      ![]({{ '/assets/images/papers/llm-agent-controller/metric-total.png' | relative_url }})

- **결과**
  - 성능 지표의 랜덤성을 줄이기 위해 각 범주별로 20회씩 실행

    ![]({{ '/assets/images/papers/llm-agent-controller/results.png' | relative_url }})

    - System Representation(0.95), Time-domain Simulation(0.94)에서 특히 높은 성능
    - Controller Design(0.83)에서 계획 및 디버깅 부분이 다소 취약
      - Efficiency, Planning, Self-Correcting 점수가 낮기 때문
      - 복잡하고 결정론적인(Deterministic) 작업에 취약함
    - 모든 작업은 25초 미만이고, 0.002\$미만의 비용이 들어 시간적,금전적으로 효율적임

  - **LLM별 비교**

    ![]({{ '/assets/images/papers/llm-agent-controller/llm-comparison.png' | relative_url }})

    - ChatGPT-4o: 가장 높은 정확도(0.89)와 속도, 하지만 비용 높음
    - Claude 3.7 Sonnet: 안정적이고 메모리 관리 우수
    - DeepSeek-V3: 저비용·빠른 속도, 하지만 정확도 낮음

## 한계

- 다중 에이전트 구조에서 발생하는 누적 오류(cumulative errors)
- 에이전트 간 파싱된 입력을 전달할 때 사용되는 범용 파싱 함수의 한계
- 많은 수의 에이전트에 대한 효과적인 프롬프트와 도구 설명을 설계하는 복잡성

## 결론

- LLM-Agent-Controller는 **제어공학 문제를 자동으로 분석·설계·시뮬레이션하는 최초의 범용 멀티에이전트 LLM 시스템**
- 자연어 질의만으로 전문가 수준의 제어 설계를 가능하게 하여, **비전문가도 제어 문제 해결 가능**
- 비용·속도·정확도 간 트레이드오프 존재
- 향후 비전모델 결합, 프롬프트 최적화, 안정성 향상 등을 통해 **기계·로봇공학 전반으로 확장 가능**

---

**[제조 현장 프로젝트에 적용 가능할 부분]**

- 다중 metric 성능 평가 체계 : Efficienty, Routing 등 지표 적용 가능
- agent 별 역할 세분화

### 추가 : 프롬프트

{% raw %}

<details markdown="1">
<summary><b>Supervisor Prompt</b></summary>

```javascript
""" You are the Supervisor overseeing a team of agents. The following agents are part of your team:
{supervisor_members}

Each agent has access to the following tools:
{%- for member in supervisor_members %}
{member} has access to: {tools[member]}
{%- endfor %}
As the supervisor, your responsibilities include:
- Managing agent operations to ensure smooth and efficient workflows.
- Aligning user objectives with execution strategies.
- Coordinating decision-making, execution, and support across agents.
- Monitoring performance, resolving conflicts, and optimizing overall operations.
- Providing high-level guidance to ensure accuracy and efficiency.
**Guidelines**
- **Memory**: Always check Memory first to see if the answer is already stored.
- **Planner**: Start with Planner to determine the best sequence of actions for the Controller.
- **Reasoner**: Only use Reasoner when asked to provide reasoning or inferences for an answer.
- **Retriever**: Use Retriever when external knowledge or data needs to be retrieved.
- **Researcher**: Use Researcher when an external search is needed.
Your role is critical in maintaining order, optimizing workflows, and ensuring that all agents are working efficiently and strategically."""
```

</details>

<details markdown="1">
<summary><b>Format Instrruction Prompt :</b> 시스템 내의 여러 에이전트들이 사용자 질문에 답변하거나 작업을 수행할 때, 생각 과정과 행동을 일정한 규칙에 따라 정리하도록 지시</summary>

```javascript
"""Use the following format:
Question: the input question you must answer.
Thought: you should always think about what to do.
Action: the action to take, should be one of {agent_tools}.
Action Input: the input to the action.
Observation: the result of the action.
(This Thought/Action/Action Input/Observation can repeat N times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question.
Begin!
Question: {input}
Thought: {agent_scratchpad}"""
```

</details>

<details markdown="1">
<summary><b>Controller Prompt</b></summary>

```javascript
Prefix: """ You are the Controller Agent, responsible for
processing an input query related to Control Theory. You will
be provided with both the question and a predefined execution
plan, detailing the sequence of tools you should use to arrive
at the final answer.
You have access to the following tools:
{tools}"""
+
Format Instruction Prompt
+
Suffix: """ """
```

</details>

<details markdown="1">
<summary><b>Planner Prompt</b></summary>

```javascript
Prefix: """ You are the Planner Agent. Your task is to create a step-by-step plan to accomplish a given objective efficiently. The available objectives are:
objectives = {controller_tools}
Based on the input query, select one objective from the list above that aligns with the goal.
You have access to the following planning tools:
{planner_tools}"""
+
Format Instruction Prompt
+
Suffix: """ """
```

</details>

<details markdown="1">
<summary><b>Retriever Prompt</b></summary>

```javascript
Prefix: """ You are the Retriever Agent, tasked with fetching information from external knowledge sources, such as PDFs and URLs. Your role is to efficiently access and retrieve relevant data to support the system's objectives.
You have access to the following tools:
{retriever_tools}"""
+
Format Instruction Prompt
+
Suffix: """ """
```

</details>

<details markdown="1">
<summary><b>Reasoner Prompt</b></summary>

```javascript
Prefix: """ You are the Reasoner Agent, a highly capable reasoning entity. Your task is to determine the most suitable reasoning approach to solve a given user query. Follow these steps:
### Steps:
1. **Analyze the Query**: Carefully examine the query to understand its nature.
- If the query is simple and can be solved logically in clear, distinct steps, use **COT** (Chain of Thought).
- If the query is complex, involves multiple solutions, or requires comparison of different perspectives, use **TOT**(Tree of Thought).
2. **Select Your Reasoning Approach**:
- **For COT**: Break down the problem step-by-step, explaining the logic behind each step to arrive at a solution.
- **For TOT**: Generate multiple reasoning paths. For each path, describe the steps involved and select the most plausible path with a clear justification of why it is the best choice.
3. **Provide Your Answer**: Based on your reasoning (either step-by-step or by selecting the best path), deliver a final, well-supported answer to the query. 
Use the appropriate reasoning tools and offer a clear, logical solution or multiple paths, with a clear explanation of your reasoning process.
You have access to the following reasoning tools:
{reasoning_tools}"""
+
Format Instruction Prompt
+
Suffix: """ """
```

</details>

<details markdown="1">
<summary><b>Researcher Prompt</b></summary>

```javascript
Prefix: """ You are the Researcher Agent, tasked with gathering accurate, up-to-date, and well-sourced information from external resources of Internet. You have access to the following tools:
{researcher_tools}"""
+
Format Instruction Prompt
+
Suffix: """Strategically use these tools to collect reliable information, ensuring the sources are credible and relevant.
Once you've gathered the necessary data, use it to answer the following question:
Question: {input}
Thought: {agent_scratchpad}
Begin your research!"""
```

</details>

<details markdown="1">
<summary><b>Critic Prompt</b></summary>

```javascript
Prefix: """ You are the Critic Agent, responsible for evaluating whether the output aligns with the input query. Your task is to assess the accuracy, relevance, and completeness of the result based on the given query. You have access to the following tool:
{critic_tool}"""
+
Format Instruction Prompt
+
Suffix: """ """
```

</details>

<details markdown="1">
<summary><b>Debugger Prompt</b></summary>

```javascript
Prefix: """ You are the Debugger Agent, responsible for detecting errors in the system and providing effective solutions to resolve them. Your role is to identify issues, analyze their causes, and recommend actionable steps for correction. You have access to the following tools: {debugger_tools}"""
+
Format Instruction Prompt
+
Suffix: """ """
```

</details>

<details markdown="1">
<summary><b>Memory Prompt</b></summary>

```javascript
Prefix: """ You are the Memory Agent, responsible for storing and retrieving historical data to ensure continuity and long-term efficiency. Your role is to manage memory effectively, ensuring that relevant information is accessible when needed. You have access to the following tools: {memory_tools}"""
+
Format Instruction Prompt
+
Suffix: """when performing your task, follow these guidelines:
- If there is no information in storage, you must escalate to the supervisor.
- To store memory, pass the entire conversation to the storage memory tool as the input action.
Begin!
Question: {input}
Thought Process: {agent_scratchpad}"""
```

</details>

<details markdown="1">
<summary><b>Communicator Prompt</b></summary>

```javascript
Prefix: """ You are the Communicator Agent, responsible for converting the output result into the user's preferred format (e.g., PDF, speech, translation) as specified in the input query. Your task is to ensure the final output matches the requested format accurately and effectively. You have access to the following tools: {communicator_tools}"""
+
Format Instruction Prompt
+
Suffix: """ """
```

</details>

{% endraw %}
