---
title: "Formal Models and Design Principles"
title_ko: "형식적 모델과 설계 원칙"
course: loop-and-graph-engineering
lesson: 3
tags:
  - Agent Engineering
  - Loop Engineering
  - Graph Engineering
---

## 학습 목표

- loop를 상태 전이 시스템과 종료 술어로 형식화하기
- graph를 node contract와 실행 의미론을 가진 유향 그래프로 분석하기
- 동시성, 임계 경로, fan-in, 부분 실패와 재시도의 비용을 설명하기
- graph·멀티 에이전트 도입 효과를 동일한 계산 예산에서 평가하기

## ELIPhD란 무엇인가

앞의 두 레슨에서는 “어떻게 이해하고 사용할 것인가”에 집중했습니다. 이번에는 같은 개념을 시스템 설계자의 언어로 다시 정의합니다.

여기서 중요한 질문은 이름이 아닙니다.

- 시스템 상태가 어디에 존재하는가?
- 어떤 전이가 허용되는가?
- 실행이 언젠가 끝난다는 것을 어떻게 보장하는가?
- 동시 실행이 결과의 의미를 바꾸지 않는가?
- 성능 향상이 topology 때문인지 추가 계산량 때문인지 어떻게 구분하는가?

`loop engineering`과 `graph engineering`은 아직 엄밀하게 합의된 학술 표준어가 아닙니다. 하지만 상태 전이, workflow graph, orchestration, multi-agent coordination처럼 그 안에 들어 있는 문제들은 오래전부터 연구되고 구현되어 왔습니다.

## 1. Loop의 형식적 모델

시간 `t`에서 에이전트의 loop를 다음처럼 표현할 수 있습니다.

```text
s_t : 현재 상태
o_t = O(s_t) : 상태에서 얻은 관측
a_t = π(s_t, o_t, g) : 목표 g를 고려해 선택한 행동
s_(t+1) = T(s_t, a_t, e_t) : 행동과 환경 반응으로 갱신된 상태
```

- `s_t`에는 작업 산출물, 대화 기록, 도구 결과, 남은 예산이 들어갑니다.
- `O`는 테스트 출력, 검색 결과, 파일 내용처럼 에이전트가 볼 수 있는 정보를 만듭니다.
- `π`는 다음 행동을 고르는 정책(policy)입니다. LLM 호출이 이 역할을 맡을 수 있습니다.
- `T`는 상태 전이 함수입니다. 파일 수정, API 호출, 데이터 저장처럼 환경을 실제로 바꿉니다.
- `e_t`는 네트워크 오류나 외부 시스템 변경처럼 에이전트가 완전히 통제할 수 없는 환경 사건입니다.

Loop는 종료 술어 `τ`가 참이 될 때 멈춥니다.

```text
τ(s_t) = V(s_t)
      OR t ≥ max_iterations
      OR cost(s_t) ≥ budget
      OR elapsed(s_t) ≥ timeout
      OR no_progress(s_(t-k), ..., s_t)
      OR human_escalation(s_t)
```

`V`는 성공 검증 함수입니다. 테스트 통과, 스키마 일치, 필요한 출처 수 충족처럼 외부에서 검사할 수 있어야 합니다.

### “될 때까지 반복”은 종료 증명이 아니다

LLM 정책 `π`는 확률적이며, 상태 공간은 크고, 전이 함수 `T`에는 외부 환경이 개입합니다. 따라서 일반적인 agent loop가 최적해나 고정점에 수렴한다고 가정할 수 없습니다.

Loop는 다음 상태를 오갈 수 있습니다.

```text
s_A → s_B → s_A → s_B → ...
```

예를 들어 한 수정은 테스트 A를 통과시키지만 테스트 B를 실패하게 하고, 다음 수정은 그 반대를 만들 수 있습니다. 이를 막으려면 단순한 반복 횟수뿐 아니라 **진전(progress)의 정의**가 필요합니다.

```text
progress_t = (
  실패 테스트 수 감소,
  새로운 증거 추가,
  미해결 요구사항 감소,
  동일 오류 반복 여부
)
```

성공 기준은 모델의 자기 확신보다 환경의 피드백에 두는 편이 안전합니다. Anthropic의 [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents)는 agent가 환경의 ground truth를 확인하고, 명확한 stopping condition을 사용하도록 권합니다.

### 상태에는 서로 다른 수명이 있다

모든 정보를 하나의 대화 history에 넣으면 편하지만, 실제 상태는 수명이 다릅니다.

| 상태 종류 | 예시 | 적절한 수명 |
|---|---|---|
| Step-local | 이번 도구 호출의 임시 출력 | 한 step |
| Run-local | 시도 횟수, 현재 계획, 남은 예산 | 한 실행 |
| Task-persistent | checkpoint, 승인 결과, 생성 파일 | 중단·재개 사이 |
| Long-term | 사용자 선호, 조직 규칙 | 여러 작업 |

남은 예산을 대화 문장으로만 관리하면 resume할 때 초기화될 수 있습니다. 실행 예산과 checkpoint는 모델 문맥 밖의 내구성 있는 상태로 관리하는 편이 낫습니다.

## 2. Graph의 형식적 모델

Agent graph를 유향 그래프 `G=(V,E)`로 놓겠습니다.

- `V`는 node의 집합입니다.
- `E ⊆ V×V`는 실행 또는 데이터 의존성을 나타내는 edge의 집합입니다.
- 각 node `v`는 입력 상태를 출력 상태로 바꾸는 함수 `f_v`로 볼 수 있습니다.

```text
f_v : Input_v × State_v → Output_v × State'_v × Status_v
```

실전에서 node contract는 최소한 다음을 정의해야 합니다.

```text
Contract_v = {
  required_inputs,
  output_schema,
  allowed_side_effects,
  timeout,
  retry_policy,
  success_condition,
  failure_status
}
```

그래프 그림만 있고 contract가 없다면 그것은 실행 가능한 설계라기보다 개념도에 가깝습니다.

### Chain, DAG, Cyclic graph

#### Chain

```text
v1 → v2 → v3
```

각 node가 직전 결과를 필요로 합니다. latency는 대략 각 단계의 실행 시간을 더한 값이므로 병렬 이득은 없습니다.

#### DAG

```text
     ┌→ v2 ─┐
v1 ─┤       ├→ v4
     └→ v3 ─┘
```

순환이 없는 유향 그래프(Directed Acyclic Graph)입니다. 위상 정렬(topological ordering)을 이용해 의존성이 준비된 node를 실행할 수 있습니다. `v2`와 `v3`가 자원까지 독립적이라면 병렬 실행할 수 있습니다.

#### Cyclic graph

```text
v1 → v2 → v3
     ↑     │
     └─────┘
```

검증 실패 시 앞 단계로 돌아가는 loop가 들어 있습니다. 이 경우 DAG의 위상 정렬만으로 실행할 수 없고, cycle별 종료 조건과 전체 실행 예산이 필요합니다.

LangGraph [Graph API](https://docs.langchain.com/oss/python/langgraph/graph-api), Microsoft AutoGen의 [GraphFlow](https://microsoft.github.io/autogen/stable/user-guide/agentchat-user-guide/graph-flow.html), Google ADK의 [Workflow Agents](https://adk.dev/agents/workflow-agents/)는 표현 방식은 다르지만 순차 실행, 병렬 실행, 분기와 loop를 명시한다는 공통점이 있습니다. 즉, graph 기반 orchestration은 2026년에 갑자기 생긴 구현 방식이 아닙니다.

## 3. Edge는 단순한 화살표가 아니다

Edge는 서로 다른 의미를 가질 수 있습니다.

- **Data dependency:** 앞 node의 출력이 뒤 node의 입력이다.
- **Control dependency:** 특정 조건을 만족해야 뒤 node를 실행한다.
- **Resource dependency:** 같은 파일, DB lock, API quota 때문에 동시에 실행할 수 없다.
- **Approval dependency:** 사람이 승인해야 다음 단계로 간다.

두 작업 사이에 data edge가 없더라도 같은 파일을 수정한다면 독립적이지 않습니다. 반대로 시각적으로 순서대로 그려 놓았더라도 실제 출력 의존성이 없다면 병렬 후보입니다.

독립성을 다음처럼 생각할 수 있습니다.

```text
independent(v_i, v_j) =
  no_data_dependency
  AND disjoint_or_coordinated_side_effects
  AND sufficient_shared_resource_capacity
  AND order_invariant_result
```

마지막 조건인 `order_invariant_result`가 중요합니다. 두 node의 실행 순서를 바꿨을 때 최종 의미가 달라진다면 완전히 독립적이지 않습니다.

## 4. 병렬화와 임계 경로

Node `v`의 실행 시간을 `w(v)`라고 합시다. 모든 작업을 순차 실행한 시간은 대략 다음과 같습니다.

```text
T_serial = Σ w(v)
```

자원이 충분하고 scheduling overhead가 없다는 이상적인 조건에서 graph의 최소 완료 시간은 임계 경로(critical path)의 길이보다 짧을 수 없습니다.

```text
T_parallel ≥ max_path Σ w(v)
```

실제 시간에는 추가 비용이 붙습니다.

```text
T_real = T_critical_path
       + T_scheduling
       + T_serialization
       + T_queueing
       + T_merge
       + T_retry
```

따라서 graph를 그렸다는 사실만으로 속도 향상이 보장되지 않습니다. 다음 조건이 맞아야 합니다.

- 임계 경로 밖에 충분히 큰 독립 작업이 있다.
- 병렬 실행에 필요한 모델·도구·API capacity가 있다.
- 작업 분배와 결과 직렬화 비용이 작다.
- fan-in에서 통합할 context가 지나치게 크지 않다.
- 재시도 때문에 완료한 작업을 함께 다시 실행하지 않는다.

## 5. Fan-in은 정보 손실 지점이다

여러 node의 결과를 한 context에 단순 연결하면 context가 길어지고, 중요한 차이가 묻히며, 출처와 결론의 연결이 사라질 수 있습니다. Reducer는 요약기가 아니라 **정합성 검사기**여야 합니다.

Reducer의 입력을 다음처럼 구조화할 수 있습니다.

```text
Result_i = {
  node_id,
  status,
  claims[],
  evidence[],
  artifacts[],
  unresolved[],
  cost,
  provenance
}
```

Reducer는 다음 불변식(invariant)을 검사합니다.

```text
received_node_ids = expected_node_ids
every claim has provenance
failed nodes are explicit
conflicts are preserved until resolved
```

요약 과정에서 서로 충돌하는 결과를 하나의 매끄러운 문장으로 합치면 정보가 아니라 불확실성을 삭제하게 됩니다.

## 6. Router는 분류기가 아니라 정책 경계다

Router `R(x)`가 입력을 경로 집합 `P` 중 하나로 보낸다고 합시다.

```text
R : X → P ∪ {unknown, reject, human_review}
```

모델 기반 router는 같은 입력에도 다른 결과를 낼 수 있습니다. 프롬프트가 같다고 해서 결정성이 보장되지는 않습니다. 다음을 함께 설계해야 합니다.

- 경로별 명확한 포함·제외 기준
- `unknown` 또는 `none of the above`
- confidence가 낮을 때의 처리
- 위험도가 높은 입력의 human review
- 대표 입력과 경계 사례를 담은 routing eval
- 모델·프롬프트 변경 시 회귀 검사

판단이 모호하지 않은 조건은 코드로 처리합니다. 예를 들어 `tests/` 아래 파일인지, HTTP status가 500인지, 필수 필드가 없는지는 모델 판단이 필요하지 않습니다.

## 7. 부분 실패와 재시도

병렬 graph에서는 전체 성공과 전체 실패 사이에 많은 상태가 생깁니다.

```text
Node A: complete
Node B: partial
Node C: timed_out
Node D: complete
```

Graph runtime은 이 상태를 숨기지 말아야 합니다. 선택지는 다음과 같습니다.

- 실패 node만 재시도한다.
- `partial` 결과를 표시한 채 reducer를 실행한다.
- 필수 node가 실패하면 전체 실행을 중단한다.
- 사람의 판단을 요청한다.
- 이전 checkpoint부터 다른 경로로 재개한다.

### 멱등성(Idempotency)

Node를 재시도해도 부작용이 중복되지 않아야 합니다.

```text
f(f(s)) = f(s)
```

엄밀히 모든 작업이 멱등일 필요는 없지만, 재시도 가능한 node는 같은 요청을 두 번 실행해도 안전하도록 만드는 것이 좋습니다.

- 결제 요청에는 idempotency key를 사용합니다.
- 파일 생성은 임시 파일에 쓴 뒤 원자적으로 교체합니다.
- DB 갱신은 실행 ID와 상태를 함께 기록합니다.
- 외부 메시지 전송은 “이미 전송했는가?”를 확인합니다.

Checkpoint에는 출력만이 아니라 side effect의 완료 여부도 들어가야 합니다. 그렇지 않으면 resume 과정에서 외부 행동을 반복할 수 있습니다.

## 8. Verifier의 독립성

작업자(worker)와 검증자(verifier)를 분리하면 자기 정당화 편향을 줄일 수 있습니다. 하지만 “둘은 절대 context를 공유하면 안 된다”는 규칙은 너무 강합니다.

Verifier도 다음 공통 정보는 알아야 합니다.

- 원래 요구사항
- 허용된 변경 범위
- 평가 기준과 테스트 방법
- 입력 데이터의 provenance

분리해야 하는 것은 **작업자의 결론을 사실로 전제하는 문맥**입니다. 좋은 verifier는 작업자의 설명만 평가하지 않고 원본 요구사항, diff, 테스트 결과와 외부 증거를 직접 확인합니다.

검증을 여러 층으로 나눌 수도 있습니다.

```text
정적 검사 → 실행 테스트 → 요구사항 대조 → 출처·최신성 확인 → 사람 승인
```

각 층은 실패의 종류가 다르므로 하나의 “검토해줘” 프롬프트보다 진단 가능성이 높습니다.

## 9. 멀티 에이전트 효과를 해석하는 법

Graph와 multi-agent는 같은 말이 아닙니다. Graph는 제어 구조이고, multi-agent는 여러 모델 실행을 사용하는 배치 방식입니다.

Anthropic의 [Multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)은 orchestrator-worker 구조가 복잡한 조사에서 병렬 검색 공간을 넓힐 수 있다고 설명합니다. 동시에 agent가 일반 chat보다 약 4배, multi-agent system이 약 15배의 token을 사용했다고 밝힙니다. 이 수치는 해당 시스템의 관측치이지 모든 작업의 보편 법칙은 아닙니다.

반대 방향의 결과도 있습니다. [Multi-Agent Collaboration Mechanisms](https://arxiv.org/abs/2604.02460)은 thinking-token 예산을 같게 맞춘 다중 단계 추론 실험에서 단일 에이전트가 여러 멀티 에이전트 구성과 대등하거나 더 나을 수 있음을 보고합니다. 이 결과 역시 특정 모델·과제·평가 설정 안에서 해석해야 하지만, topology 효과와 추가 계산량 효과를 분리해야 한다는 경고는 중요합니다.

[MAST](https://arxiv.org/abs/2503.13657)는 7개 멀티 에이전트 프레임워크의 1,600개 이상 실행 trace를 분석해 시스템 설계, 에이전트 간 불일치, 작업 검증에 걸친 14개 실패 유형을 제안합니다. 역할을 늘리는 것만으로 coordination 문제가 사라지지 않는다는 뜻입니다.

한편 [From Agent Loops to Structured Graphs](https://arxiv.org/abs/2604.11378)는 agent loop에서 구조화된 graph로 이어지는 설계 연속체를 정리한 position paper입니다. 70개 시스템을 분류하지만 production 구현이나 topology 우월성을 입증하는 실험 논문은 아닙니다. 개념 지도로 활용하되 성능 증거로 해석해서는 안 됩니다.

## 10. 무엇을 측정해야 하는가

Graph 도입 전후를 비교할 때 wall-clock latency 하나만 보면 부족합니다.

| 지표 | 질문 |
|---|---|
| Correctness | 외부 테스트·평가 기준을 얼마나 만족했는가? |
| Coverage | 필요한 하위 문제와 출처를 빠뜨리지 않았는가? |
| Wall-clock latency | 사용자가 기다린 시간은 줄었는가? |
| Compute cost | 총 token, 모델 호출, 도구 호출은 얼마나 늘었는가? |
| Coordination cost | 분배·직렬화·통합에 얼마나 썼는가? |
| Recovery cost | 부분 실패 후 얼마나 적게 다시 실행했는가? |
| Human correction | 사람이 수정하거나 다시 지시한 횟수는 얼마인가? |
| Reproducibility | 같은 입력에서 허용 가능한 범위의 결과가 나오는가? |

실용적인 의사결정 함수는 다음처럼 생각할 수 있습니다.

```text
Net value = quality gain
          + latency value
          + recovery value
          - extra compute cost
          - coordination cost
          - operational risk
```

이 값은 조직과 작업에 따라 달라집니다. 한 번의 보고서에서 5분을 줄이는 것과, 매일 수천 건을 처리하는 pipeline에서 5분을 줄이는 것은 가치가 다릅니다.

## 11. 설계 체크리스트

### 문제와 의존성

- [ ] 단일 호출이나 단일 loop로 충분하지 않은 이유가 측정 가능한가?
- [ ] 각 edge가 data, control, resource, approval 중 무엇을 의미하는지 설명할 수 있는가?
- [ ] 병렬 node의 실행 순서를 바꿔도 최종 의미가 유지되는가?
- [ ] 임계 경로와 fan-in 병목을 알고 있는가?

### 상태와 Contract

- [ ] step-local, run-local, persistent state를 구분했는가?
- [ ] 각 node의 입력·출력·실패 schema가 명시됐는가?
- [ ] 상태를 갱신할 단일 owner 또는 충돌 해결 규칙이 있는가?
- [ ] provenance와 미해결 사항이 reducer까지 보존되는가?

### 종료와 예산

- [ ] 성공 검증 함수 `V`가 모델의 자기 평가와 분리돼 있는가?
- [ ] iteration, time, token, 비용의 hard cap이 있는가?
- [ ] no-progress와 oscillation을 감지하는가?
- [ ] nested loop의 최악 실행 횟수를 계산했는가?

```text
최악 실행 횟수 ≈ outer_cap × inner_cap × branch_count
```

### 실패와 복구

- [ ] `complete`, `partial`, `failed`, `unknown` 상태가 구분되는가?
- [ ] 재시도 가능한 node의 side effect가 멱등한가?
- [ ] checkpoint가 데이터와 외부 side effect를 함께 기록하는가?
- [ ] 필수 node가 실패했을 때의 정책이 있는가?

### 평가와 운영

- [ ] 동일한 token·도구 예산의 단일 에이전트 baseline과 비교했는가?
- [ ] routing 경계 사례를 포함한 eval set이 있는가?
- [ ] node별 latency, 비용, 실패, 재시도를 trace할 수 있는가?
- [ ] topology 변경이 품질·비용·운영 위험에 미친 영향을 측정하는가?

## 결론

Loop engineering의 핵심은 **수렴을 바라기보다 종료와 검증을 설계하는 것**입니다. Graph engineering의 핵심은 **작업을 많이 나누는 것이 아니라 실제 의존성과 상태 전이를 명시하는 것**입니다.

Graph는 잘 설계된 loop를 대체하지 않습니다. 여러 loop가 상호작용할 때 생기는 라우팅, 동시성, 상태 소유권, 부분 실패와 통합 문제를 더 분명하게 다루는 상위 구조입니다.

마지막 판단 기준은 단순합니다.

> 더 복잡한 topology가 동일한 평가 기준에서 품질, 시간 또는 복구 가능성을 실제로 개선하는가?

그 답을 측정할 수 없다면, 아직 graph를 추가할 이유도 충분하지 않습니다.

## 참고 자료

- [Anthropic — Building effective agents](https://www.anthropic.com/engineering/building-effective-agents)
- [Anthropic — How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)
- [LangGraph — Graph API](https://docs.langchain.com/oss/python/langgraph/graph-api)
- [Microsoft AutoGen — GraphFlow](https://microsoft.github.io/autogen/stable/user-guide/agentchat-user-guide/graph-flow.html)
- [Google ADK — Workflow Agents](https://adk.dev/agents/workflow-agents/)
- [From Agent Loops to Structured Graphs](https://arxiv.org/abs/2604.11378)
- [Multi-Agent Collaboration Mechanisms](https://arxiv.org/abs/2604.02460)
- [MAST: A Multi-Agent System Failure Taxonomy](https://arxiv.org/abs/2503.13657)
