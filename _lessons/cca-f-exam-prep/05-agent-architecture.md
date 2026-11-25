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

## 공식 가이드 v1.1로 보완하기

여기부터는 교재에 없거나 짧게만 나온 내용을 공식 시험 가이드 v1.1(2026년 11월)의 과제 설명(Task Statement 1.1~1.7)에 맞춰 채운 부분입니다. 예시 속 숫자와 도구 이름은 이해를 돕기 위한 가정입니다.

### 1. 루프는 `stop_reason`으로 돈다 (1.1)

<div class="oa-eli">
  <span class="oa-eli__head">🍳 쉽게 말하면</span>
  <p>주방장이 "재료 더 가져와"라고 하면 보조가 가져다주고 다시 묻습니다. 주방장이 "다 됐어"라고 할 때까지 반복합니다. 보조는 주방장의 <b>표정을 읽어서</b> 끝났는지 추측하지 않습니다. <code>stop_reason</code>이 바로 그 "더 가져와 / 다 됐어" 신호입니다.</p>
</div>

<div class="oa-svg">
<svg viewBox="0 0 720 230" role="img" aria-label="stop_reason 기반 에이전트 루프">
  <defs>
    <style>.d1b{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.d1g{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.d1t{font-size:14px;fill:#4a4a4a;font-weight:bold}.d1s{font-size:11.5px;fill:#8a8a8a}.d1l{stroke:#c99400;stroke-width:1.8;fill:none}.d1a{font-size:12px;fill:#c99400;font-weight:bold}</style>
    <marker id="d1m" markerWidth="8" markerHeight="8" refX="7" refY="4" orient="auto"><path d="M0,0 L8,4 L0,8 z" fill="#c99400"/></marker>
  </defs>
  <rect x="20" y="30" width="150" height="64" rx="10" class="d1b"/><text x="95" y="57" text-anchor="middle" class="d1t">① 요청 보내기</text><text x="95" y="77" text-anchor="middle" class="d1s">messages + tools</text>
  <rect x="230" y="30" width="170" height="64" rx="10" class="d1b"/><text x="315" y="57" text-anchor="middle" class="d1t">② stop_reason 확인</text><text x="315" y="77" text-anchor="middle" class="d1s">응답의 종료 사유</text>
  <rect x="470" y="30" width="230" height="64" rx="10" class="d1g"/><text x="585" y="57" text-anchor="middle" class="d1t">끝: 최종 답 반환</text><text x="585" y="77" text-anchor="middle" class="d1s">"end_turn"</text>
  <rect x="230" y="150" width="170" height="64" rx="10" class="d1b"/><text x="315" y="177" text-anchor="middle" class="d1t">③ 도구 실행</text><text x="315" y="197" text-anchor="middle" class="d1s">"tool_use"일 때</text>
  <rect x="20" y="150" width="150" height="64" rx="10" class="d1b"/><text x="95" y="177" text-anchor="middle" class="d1t">④ 결과 추가</text><text x="95" y="197" text-anchor="middle" class="d1s">대화 기록에 붙이기</text>
  <path d="M170,62 L226,62" class="d1l" marker-end="url(#d1m)"/>
  <path d="M400,62 L466,62" class="d1l" marker-end="url(#d1m)"/><text x="433" y="52" text-anchor="middle" class="d1a">end_turn</text>
  <path d="M315,94 L315,146" class="d1l" marker-end="url(#d1m)"/><text x="358" y="125" text-anchor="middle" class="d1a">tool_use</text>
  <path d="M230,182 L174,182" class="d1l" marker-end="url(#d1m)"/>
  <path d="M95,150 L95,98" class="d1l" marker-end="url(#d1m)"/>
</svg>
</div>

```python
while True:
    resp = client.messages.create(model=MODEL, messages=messages, tools=tools, max_tokens=2048)
    messages.append({"role": "assistant", "content": resp.content})
    if resp.stop_reason == "end_turn":      # 모델이 끝났다고 알림
        break
    if resp.stop_reason == "tool_use":      # 도구를 실행하고 결과를 돌려줌
        results = run_tools(resp.content)
        messages.append({"role": "user", "content": results})
```

가이드가 꼽는 안티패턴 세 가지입니다.

| 안티패턴 | 왜 틀렸나 |
|---|---|
| 응답 문장을 파싱해 "완료", "done" 같은 말을 찾는다 | 문장은 매번 달라집니다. 신호는 `stop_reason`에 이미 있습니다 |
| 반복 횟수 상한을 **주된** 종료 조건으로 쓴다 | 일이 끝났는지와 무관하게 끊깁니다. 상한은 안전망일 뿐입니다 |
| 어시스턴트가 텍스트를 내면 끝난 것으로 본다 | 텍스트와 도구 호출이 한 응답에 같이 올 수 있습니다 |

위 "종료 조건" 절의 `max_iterations` 안전망은 이와 충돌하지 않습니다. 안전망으로 두는 것은 맞고, 그것을 정상 종료 수단으로 쓰는 것이 오답입니다.

### 2. 서브에이전트는 아무것도 물려받지 않는다 (1.2, 1.3)

Task 도구로 띄운 서브에이전트는 코디네이터의 대화 기록을 보지 못합니다. 필요한 내용은 프롬프트에 직접 넣어야 합니다.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 컨텍스트가 빠진 위임</span>
    <ul>
      <li>"위에서 찾은 내용을 정리해서 보고서로 써 줘."</li>
      <li>서브에이전트에게는 "위"가 없습니다. 추측으로 채웁니다.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 명시적으로 넘긴 위임</span>
    <ul>
      <li>발견 내용 전문을 프롬프트에 넣습니다.</li>
      <li>내용과 메타데이터를 나눕니다: <code>claim</code>, <code>source_url</code>, <code>document</code>, <code>page</code></li>
      <li>목표와 품질 기준을 적습니다. 절차를 하나하나 지시하지 않습니다.</li>
    </ul>
  </div>
</div>

서브에이전트를 띄우려면 코디네이터의 `allowedTools`에 `"Task"`가 들어 있어야 합니다. 빠져 있으면 코디네이터는 위임 자체를 못 합니다.

```python
options = ClaudeAgentOptions(
    allowed_tools=["Task", "Read", "Grep"],          # "Task"가 있어야 위임 가능
    agents={
        "web-researcher": AgentDefinition(
            description="웹에서 최신 자료를 찾는다",
            prompt="검색 결과를 claim / source_url / date 구조로 보고한다",
            tools=["WebSearch", "WebFetch"],            # 역할에 필요한 도구만
        ),
    },
)
```

**병렬 실행**: 코디네이터가 **한 응답 안에서** Task를 여러 번 호출하면 서브에이전트가 동시에 돕니다. 턴마다 하나씩 부르면 순차 실행입니다.

<div class="oa-bars">
  <div class="oa-bar"><span>순차: 턴마다 Task 1개</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:100%"></span><span class="oa-bar__val">20초 + 20초 + 20초 = 60초</span></span></div>
  <div class="oa-bar"><span>병렬: 한 응답에 Task 3개</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:34%"></span><span class="oa-bar__val">약 20초</span></span></div>
  <div class="oa-bars__caption">서브에이전트 하나가 20초 걸린다고 가정한 예시입니다.</div>
</div>

### 3. 분해가 좁으면 주제가 통째로 빠진다 (1.3)

<div class="oa-eli">
  <span class="oa-eli__head">📋 예시: "AI가 창작 산업에 미치는 영향" 보고서</span>
  <p>코디네이터가 주제를 "디지털 아트", "그래픽 디자인", "사진"으로만 나눴습니다. 서브에이전트는 각자 맡은 일을 잘 해냈지만 보고서에는 음악, 글쓰기, 영화가 없습니다. 원인은 서브에이전트가 아니라 <b>코디네이터의 분해</b>입니다. 검색 도구를 바꾸거나 서브에이전트 프롬프트를 고치는 선택지는 오답입니다.</p>
</div>

코디네이터는 한 번 위임하고 끝내지 않습니다.

<ol class="oa-flow">
  <li><strong>분해·위임</strong>범위가 겹치지 않게 나눔</li>
  <li><strong>종합</strong>결과를 합침</li>
  <li><strong>빈틈 평가</strong>빠진 영역이 있나</li>
  <li><strong>재위임</strong>빈틈을 겨냥한 질의</li>
  <li><strong>재종합</strong>보강해서 마무리</li>
</ol>

단순한 질의에 서브에이전트를 전부 돌릴 필요는 없습니다. 질의 복잡도에 따라 어떤 서브에이전트를 부를지도 코디네이터가 정합니다.

### 4. 훅 두 가지: 결과 정규화와 호출 차단 (1.4, 1.5)

| 훅 | 시점 | 하는 일 | 예시 |
|---|---|---|---|
| PostToolUse | 도구 실행 **뒤**, 모델이 결과를 보기 전 | 결과를 일관된 형식으로 변환 | 유닉스 타임스탬프와 ISO 8601 날짜를 한 형식으로, 숫자 상태 코드를 이름으로 |
| 가로채기(실행 전) | 도구 호출이 나가기 **전** | 정책 위반 호출을 막고 대안으로 돌림 | 한도 초과 환불을 막고 사람에게 에스컬레이션 |

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">정규화 전: 도구마다 형식이 다름</span>
    <ul>
      <li><code>created: 1794000000</code></li>
      <li><code>shipped: "2026-11-09T10:30:00Z"</code></li>
      <li><code>status: 3</code></li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">PostToolUse 훅을 거친 뒤</span>
    <ul>
      <li><code>created: "2026-11-06"</code></li>
      <li><code>shipped: "2026-11-09"</code></li>
      <li><code>status: "shipped"</code></li>
    </ul>
  </div>
</div>

차단 훅의 예입니다. 환불 한도를 500달러로 가정하면, 프롬프트에 "500달러 넘는 환불은 하지 마"라고 적는 대신 훅이 호출을 막고 `escalate_to_human`으로 돌립니다.

판단 기준은 레슨 1과 같습니다. **반드시 지켜져야 하면 훅, 권장 수준이면 프롬프트**입니다. 프롬프트 지시는 실패율이 0이 아닙니다.

**사람에게 넘길 때**는 상담원이 대화 기록을 못 볼 수 있다고 가정하고 구조화해서 넘깁니다.

```json
{
  "customer": {"id": "C-1042", "name": "김OO", "tier": "gold"},
  "issue": "주문 #8831 이중 결제",
  "root_cause": "결제 재시도 중 중복 승인",
  "actions_taken": ["get_customer 확인", "lookup_order 확인"],
  "recommended_action": "중복 승인분 89,000원 환불",
  "reason_for_escalation": "환불 금액이 자동 처리 한도 초과"
}
```

한 메시지에 요청이 여러 개(환불 + 주소 변경 + 배송 문의)면 요청별로 나눠 병렬로 조사한 뒤 하나의 답으로 합칩니다.

### 5. 프롬프트 체이닝 vs 동적 분해 (1.6)

| | 프롬프트 체이닝 | 동적 분해 |
|---|---|---|
| 방식 | 미리 정한 단계를 순서대로 | 발견한 내용에 따라 다음 단계를 정함 |
| 맞는 일 | 절차가 예측 가능한 검토 | 끝이 열린 조사 |
| 예시 | 코드 리뷰: 파일별 검사 → 통합 검사 | 레거시 코드에 테스트 추가 |

**큰 코드 리뷰**는 한 번에 다 보게 하면 주의가 분산되어 놓치는 것이 생깁니다. 두 종류의 패스로 나눕니다.

<ol class="oa-flow">
  <li><strong>파일별 패스</strong>파일 하나씩 로컬 문제 검사</li>
  <li><strong>통합 패스</strong>파일 사이 데이터 흐름·인터페이스 검사</li>
  <li><strong>결과 병합</strong>중복과 모순 정리</li>
</ol>

**열린 작업**(레거시 코드에 테스트 추가)은 구조 파악 → 영향 큰 영역 식별 → 우선순위 → 의존성이 드러나면 계획 수정 순서로 갑니다.

### 6. 세션: 이어갈까, 새로 시작할까 (1.7)

| 상황 | 선택 | 이유 |
|---|---|---|
| 어제 분석이 대체로 유효하다 | `claude --resume <세션 이름>` | 이름 붙인 세션을 그대로 이어감 |
| 같은 전제에서 두 가지 접근을 비교하고 싶다 | `fork_session` | 원본을 두고 독립된 가지를 만듦 |
| 도구 결과가 낡았다(파일이 많이 바뀜) | 새 세션 + 구조화 요약 | 낡은 결과를 믿고 추론하는 것을 막음 |
| 파일 몇 개만 바뀌었다 | 재개 + **무엇이 바뀌었는지 알려 줌** | 바뀐 부분만 다시 분석 |

<details class="oa-quiz">
  <summary>Q1. 에이전트가 가끔 고객 확인 없이 환불을 처리합니다. 가장 효과적인 수정은?</summary>
  <p><b>정답: <code>get_customer</code>가 검증된 ID를 돌려주기 전에는 <code>process_refund</code>를 막는 프로그래밍 방식 게이트.</b> 시스템 프롬프트 강조, few-shot 추가는 실패율을 줄일 뿐 0으로 만들지 못합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 리서치 보고서에 특정 분야가 통째로 빠졌습니다. 서브에이전트 출력은 각각 정확합니다. 원인은?</summary>
  <p><b>정답: 코디네이터의 과제 분해가 너무 좁았다.</b> 서브에이전트는 맡은 범위를 잘 수행했으므로 서브에이전트나 검색 도구를 고치는 선택지는 오답입니다.</p>
</details>

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
- 가이드 보완: 루프는 `stop_reason`으로 끝낸다. 서브에이전트에는 컨텍스트를 명시적으로 넘기고 `allowedTools`에 `"Task"`를 넣는다. 반드시 지킬 규칙은 훅으로 강제하고, 큰 리뷰는 파일별 패스와 통합 패스로 나눈다.
