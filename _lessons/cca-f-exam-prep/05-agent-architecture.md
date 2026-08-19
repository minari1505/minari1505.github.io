---
title: "Agent Architecture and Orchestration"
title_ko: "에이전트 아키텍처와 오케스트레이션 (도메인 1)"
course: cca-f-exam-prep
lesson: 5
tags:
  - CCA-F
  - Agents
  - Multi-Agent
---

## 학습 목표

- 에이전트의 4요소와 종료 조건 설계 원칙 알기
- 태스크 분해 패턴(Plan-and-Execute / ReAct / Reflexion)과 Best of N을 언제 쓰는지 판단하기
- 싱글에서 멀티로 넘어가는 신호와 Hub & Spoke 설계 원칙 익히기
- 전제 조건 게이트, 핸드오프, `--resume` vs `fork_session` 구분하기

## 먼저 한 문장으로

**도메인 1은 배점 27%로 가장 크고, "이 상황에 싱글인가 멀티인가", "허브와 서브에 무엇을 맡기나", "루프는 어떻게 끝내나"를 묻습니다.**

에이전트 루프 코드와 서브에이전트 만들기는 이 사이트의 [Agents and Workflows](/courses/claude-with-the-anthropic-api/10-agents-and-workflows/), [Introduction to subagents](/courses/introduction-to-subagents/01-what-are-subagents/) 정리에 있습니다.

## 에이전트 = LLM + 툴 + 메모리 + 종료 조건

| 요소 | 역할 | 구현 |
|---|---|---|
| LLM | 생각하고 판단하고 계획한다 | Claude API |
| 툴 | 바깥 세계와 연결 | Tool Use / MCP |
| 메모리 | 이력과 중간 결과 | messages 배열, 외부 저장소 |
| 종료 조건 | 루프를 빠져나온다 | `end_turn` / `max_iterations` / 평가 함수 |

"구성 요소가 아닌 것은?"의 정답은 GPU 하드웨어 같은 선택지입니다.

## 종료 조건: `end_turn`이 주, `max_iterations`는 안전망

루프는 `stop_reason == "end_turn"`으로 정상 종료하는 것이 원칙입니다. `max_iterations`는 툴을 끝없이 부르는 이상 상황에 대비한 안전망이지 주된 종료 수단이 아닙니다.

- 반드시 설정합니다. 없으면 에러 루프에서 비용이 눈덩이처럼 늡니다.
- 정상 경로에서 몇 번 도는지 재고, 그 1.5~2배로 잡습니다. "평소엔 안 닿으니 100으로"는 오답입니다.
- `max_iterations`에 닿으면 정상 종료가 아니라 이상으로 로그에 남기고 원인(프롬프트, 툴 정의)을 조사합니다.

## 태스크 분해 3패턴

| 패턴 | 방식 | 맞는 일 |
|---|---|---|
| Plan-and-Execute | 먼저 계획을 세우고 그 뒤 실행에 집중 | 절차가 예측 가능한 일(리포트, 데이터 분석) |
| ReAct | 생각과 행동을 번갈아 | 불확실한 탐색·조사·대화형 |
| Reflexion | 실행 → 자기 평가 → 수정 | 코드 생성, 문장 교정 |

Plan-and-Execute의 모델 배치: 계획은 Opus, 실행은 Sonnet/Haiku. 계획이 틀리면 뒤가 전부 헛수고라서 계획에 정확도를 씁니다. 계획이 실패할 때 다시 계획하는 장치도 필요합니다.

시험 문제 "불확실한 상황의 탐색형 조사에 맞는 패턴은?" → ReAct.

## Best of N: 언제 쓰나

N번 생성해서 최선을 고르는 패턴입니다. 비용이 N배이므로 편차가 결과에 직결되는 일에만 씁니다. 중요한 메일 본문, 코드 여러 안 중 고르기, 중요 판단. 사용자명 정규화, 100만 건 분류, 숫자 덧셈에는 안 씁니다.

- N은 3~5가 현실적입니다.
- 평가 함수의 질이 결과를 좌우합니다. 평가기도 LLM이면 비용이 또 늡니다.
- 후보 생성 시 temperature를 조금 올려(0.7) 다양성을 줍니다.

레슨 3의 멀티패스 리뷰(한 안을 고쳐 나감)와 구분하세요.

## 계획 → 실행 → 평가

견고한 설계의 기본형입니다. 계획(Opus)이 목표를 스텝으로 나누고, 실행(Sonnet/Haiku)이 스텝마다 툴을 부르고, 평가(Sonnet/Opus)가 결과가 목표를 만족하는지 판정합니다. 불합격이면 계획으로 돌아갑니다.

## 싱글로 충분한가, 멀티로 갈 것인가

**싱글로 충분한 경우**: 단일 도메인, 툴 15개 이하, 순차 처리로 끝남, 입출력이 단순.

**멀티로 전환할 신호**:

- 툴 수가 16개를 넘어 판단 부하가 커진다 (가장 대표적인 신호)
- 서로 다른 전문 영역이 섞여 있다
- 독립된 작업을 병렬로 돌릴 수 있다
- 프롬프트가 비대해진다

"사용자가 늘었다", "월말이 됐다", "모델이 업데이트됐다"는 기술적 근거가 아니라 오답입니다.

멀티에는 오버헤드가 있습니다. API 호출 횟수, 토큰, 레이턴시, 디버깅 난이도가 다 늡니다. **싱글로 충분하면 싱글로.** "멀티 에이전트는 항상 고성능"은 오답입니다.

## 멀티 에이전트 3패턴

| 패턴 | 구조 | 장점 | 단점 |
|---|---|---|---|
| Hub & Spoke | 중앙 오케스트레이터가 모든 판단, 전문 서브에게 위임 | 단순, 모니터링 쉬움 | 허브가 단일 장애점 |
| Hierarchy | CEO → 매니저 → 워커, 여러 단 | 대규모를 단계적으로 분할 | 레이턴시 누적 |
| Network | 에이전트끼리 직접 통신 | 유연 | 디버깅 어렵고 루프에 빠지기 쉬움 |

**CCA-F 최빈출은 Hub & Spoke**입니다. "가장 권장되는 멀티 에이전트 패턴"을 물으면 이것입니다. Hub & Spoke와 Hierarchy의 차이는 계층이 1단인가 여러 단인가. "Network가 가장 유연하니 우수하다"는 오답입니다.

## Hub & Spoke 설계 원칙

**허브는 위임과 통합만 한다.** 허브가 직접 API를 부르고 요약하고 품질 체크까지 하면 서브를 추가·교체하기 어려워집니다. 허브의 책임은 "어디에 맡길지 판단"과 "결과 통합"입니다.

**모델 배치**: 허브 Opus, 서브 Sonnet/Haiku. 허브는 판단 1회 + 통합이라 호출이 적고, 서브는 실행 N회라 많습니다.

**서브 → 허브 보고는 구조화 객체로.** `{status, summary, data, confidence, next_action}` 같은 형태. 자유 텍스트 장문이나 추론 과정 전문을 넘기면 허브 컨텍스트 품질이 떨어집니다.

**서브의 경계는 system 프롬프트에 명시.** "글 집필은 writer의 일, 품질 판단은 reviewer의 일, 다른 서브를 직접 부르지 말고 반드시 허브를 경유"처럼 적습니다. 서브가 서브를 직접 부르면 Hub & Spoke가 Network로 변질됩니다.

**병렬 vs 순차**: 독립된 조사(3개 키워드 시장 조사)는 병렬, A의 결과가 B의 입력이면 순차.

## Task 툴, AgentDefinition, allowedTools

교재는 이해를 위해 `delegate_to_research_agent` 같은 커스텀 툴로 허브&스포크를 짭니다. 하지만 시험은 Claude Code / Agent SDK의 정규 용어로 묻습니다.

| 커스텀 구현 | 정규 API |
|---|---|
| 허브가 `delegate_to_*` 툴을 정의해 부른다 | 내장 **Task 툴**로 서브에이전트 실행 |
| 서브를 Python 함수로 | **AgentDefinition**(`.claude/agents/`)으로 정의 |
| 서브가 쓰는 툴을 함수 안에 고정 | **allowedTools**로 서브 단위 제한 |

Task 툴로 실행된 서브에이전트는 독립된 컨텍스트를 갖습니다. 큰 로그 조사를 부모 messages에 다 붙여 넣는 대신 서브에게 통째로 맡기면 부모 컨텍스트가 깨끗하게 남습니다.

시험 문제 "부모 컨텍스트를 어지럽히지 않고 서브 권한을 최소화하려면?" → Task 툴 + `allowedTools`를 Read, Grep으로 한정. `tools=["*"]`로 다 열어 주는 것은 오답.

참고: Claude Code v2.1.63부터 Task 툴의 이름이 Agent 툴로 바뀌었습니다. 시험 자료에는 Task로 나오니 두 이름 다 알아 둡니다.

## 안티패턴 4가지

| 안티패턴 | 증상 | 대책 |
|---|---|---|
| 무한 루프 | `while True`에 `max_iterations` 없음 | 상한 설정, 같은 툴 연속 호출 경고, 비용 상한 |
| 환각 행동 | 없는 툴 이름을 부른다 | 구조화 에러로 알림, "쓸 수 있는 툴은 A, B, C뿐" 명시 |
| 과잉 툴 호출 | 단순 질문에도 검색 여러 번 | description에 쓸 때·안 쓸 때 명기 |
| 상태 유출 | 앞 사용자 정보가 다음 세션에 섞임 | 세션마다 messages 독립, 기밀은 로그에서 마스킹 |

"에이전트가 같은 툴을 끝없이 부른다"의 가장 흔한 원인은 구조화되지 않은 에러입니다(레슨 4).

## 전제 조건 게이트

송금·삭제·공개처럼 되돌릴 수 없는 작업 앞에서 "잔액을 확인했는가" 같은 전제를 LLM 판단에 맡기지 않고 **코드로 결정론적으로** 검사하는 장치입니다.

```python
def transfer_tool(args):
    if not args.get("balance_checked"):
        return {"status": "error", "code": "PREREQUISITE_NOT_MET",
                "retryable": False, "suggestion": "먼저 check_balance를 실행하세요"}
    if args["amount"] > DAILY_LIMIT:
        return {"status": "error", "code": "LIMIT_EXCEEDED", "retryable": False}
    return do_transfer(args)
```

system 프롬프트에 "반드시 잔액을 확인한 다음 송금해"라고 적기만 하는 것, temperature 0으로 안정시키기, max_iterations 늘리기는 오답입니다. 레슨 1의 판단 기준 1(반드시 → 결정적 해법)이 여기서 그대로 쓰입니다. 레슨 7의 PreToolUse 훅도 같은 발상입니다.

## 핸드오프

다른 에이전트나 사람에게 일을 넘길 때는 구조화해서 넘깁니다. 완료 항목(확정된 사실·산출물), 남은 태스크, 제약·전제. 모호하게 넘기면 받는 쪽이 전제를 잘못 짚습니다.

## `--resume` vs `fork_session`

| 기능 | 뜻 | 쓰는 곳 |
|---|---|---|
| `--resume` (CLI) | 직전 세션을 같은 상태 그대로 계속 | 중단한 작업 재개 |
| `fork_session` (SDK) | 세션 상태를 복제해 새 분기를 만든다. 원본은 그대로 | 공통 전제에서 여러 시도를 병렬로 |

"원본을 깨뜨리지 않고 여러 분기를 병렬로 시도" → fork. SDK에서는 `resume="<세션 ID>", fork_session=True`처럼 어느 세션에서 가지를 칠지를 한 쌍으로 지정합니다.

## 설계 리뷰 문제 대비

"다음 구현의 문제 5가지"의 단골 정답 세트입니다.

1. `while True`, `max_iterations` 없음
2. 모든 태스크를 Opus로
3. 툴 50개를 전부 넘김
4. `except: result = "error"`로 에러를 삼킴 → 구조화 에러
5. `tool_result`를 assistant 역할로 돌려줌 → user 역할

## 핵심 정리

- 에이전트 = LLM + 툴 + 메모리 + 종료 조건. `end_turn`이 주, `max_iterations`는 안전망(정상 경로의 1.5~2배).
- 계획은 Opus, 실행은 Sonnet/Haiku. 탐색형은 ReAct, 예측 가능하면 Plan-and-Execute.
- Best of N은 편차가 중요한 일에만, N=3~5.
- 툴 16개 초과·전문 영역 혼재·병렬 가능 → 멀티. 싱글로 충분하면 싱글.
- Hub & Spoke가 최빈출. 허브는 위임·통합만(Opus), 서브는 전문 영역만(Sonnet/Haiku), 보고는 구조화 객체, 서브끼리 직접 호출 금지.
- 정규 용어: Task 툴, AgentDefinition, allowedTools.
- 되돌릴 수 없는 작업은 코드 게이트. 이어서 하기는 `--resume`, 복제해서 분기는 `fork_session`.
