---
title: "Tool Design and MCP Integration"
title_ko: "툴 설계와 MCP 통합 (도메인 2)"
course: cca-f-exam-prep
lesson: 4
tags:
  - CCA-F
  - Tool Use
  - MCP
---

## 학습 목표

- Tool Use의 흐름과 `tool_choice`, 병렬 호출, `is_error`의 시험 포인트 정리하기
- 툴이 너무 많을 때 무엇이 정답이고 무엇이 함정인지 판단하기
- 구조화 에러, 멱등성, 최소 권한, 타임아웃 같은 MCP 서버 설계 원칙 알기
- MCP 스코프(project / user / local)와 `${VAR}`로 비밀 정보 지키는 법 알기

## 먼저 한 문장으로

**도메인 2의 정답은 대부분 "툴 경계는 좁게, 에러는 구조화, 부수효과 툴은 멱등, 권한은 최소" 쪽에 있습니다.**

Tool Use 기본 코드와 MCP 서버 만들기는 이 사이트의 [Tool Use with Claude](/courses/claude-with-the-anthropic-api/05-tool-use-with-claude/), [Introduction to MCP](/courses/introduction-to-model-context-protocol/01-introduction/) 정리에 있습니다. 여기서는 판단 문제로 나오는 것만 모았습니다.

## Tool Use 흐름에서 자주 틀리는 것

흐름은 앱 → Claude(`tool_use` 블록, `stop_reason="tool_use"`) → 앱이 툴 실행 → `tool_result`를 돌려줌 → Claude 최종 답변입니다. Claude가 직접 외부 API를 부르지 않습니다. 앱이 부릅니다. 이 분업 덕에 감사·샌드박스·타임아웃 제어권이 앱에 남습니다.

- **`tool_result`는 user 역할로 돌려줍니다.** assistant나 system으로 보내면 에러입니다. 시험에 자주 나옵니다.
- **`tool_use_id`**는 `tool_use` 블록의 `id`와 1대1로 맞춥니다.
- 루프의 분기는 `stop_reason`으로 합니다. 응답 텍스트에 "툴"이라는 단어가 있는지로 판단하면 안 됩니다.
- `max_iterations`로 무한 루프를 막습니다. 보통 5~10회.

## description이 곧 판단 재료

툴 정의의 3요소는 `name`, `description`, `input_schema`입니다. 이 중 description은 Claude가 "이 툴을 지금 쓸까"를 결정하는 유일한 근거입니다. "검색한다" 한 줄이면 언제 써야 하는지 모릅니다. "사내 문서를 전문 검색한다. 규정·매뉴얼·과거 안건을 물을 때 쓴다. 재고 확인에는 쓰지 않는다(search_inventory 사용)"처럼 **언제 쓰는지와 언제 안 쓰는지**를 같이 적습니다.

## `tool_choice` 세 가지

| 지정 | 동작 | 쓰는 곳 |
|---|---|---|
| `{"type": "auto"}` (기본) | Claude가 판단 | 일반 대화 에이전트 |
| `{"type": "any"}` | 아무 툴이든 반드시 쓴다 | 툴 호출이 필수인 흐름 |
| `{"type": "tool", "name": "..."}` | 특정 툴을 반드시 쓴다 | 구조화 출력 |

## 병렬 툴 호출

Claude는 한 턴에 여러 툴을 동시에 요청할 수 있습니다. 날씨 + 환율 + 주가처럼 서로 독립이면 빨라집니다. 그런데 A의 결과로 B의 인자가 정해지는 경우(계좌 인출 → 메일 통지)에는 병렬로 돌면 깨집니다. 그때는 끕니다.

```python
tool_choice={"type": "auto", "disable_parallel_tool_use": True}
```

"병렬은 항상 빠르다"는 오답입니다.

## 툴 에러는 `is_error: True`로

툴 실행이 실패하면 예외를 삼키지 말고 `tool_result`에 `is_error: True`와 함께 "무슨 일이 났고 어떻게 하면 되는지"를 담아 돌려줍니다. Claude는 그걸 보고 다른 툴을 시도하거나 사용자에게 알리는 복구 판단을 합니다. 스택 트레이스 같은 내부 정보는 넣지 않습니다.

## 툴이 너무 많다: 수를 줄이는 것이 답

Claude는 매 턴 등록된 툴 정의를 전부 읽고 고릅니다. 툴이 늘수록 비교 대상이 늘고 비슷한 툴끼리 헷갈립니다.

| 툴 수 | 상태 |
|---|---|
| 5개 이하 | 최적 |
| 6~15개 | 허용 |
| 16개 이상 | 정확도 저하, 토큰 증가 |
| 30개 이상 | 설계를 다시 |

시험 문제: "툴이 40개라 선택 정확도가 떨어졌다. 대책은?"

- 정답: 관련 툴을 서브에이전트로 떼어내고 상위 에이전트는 소수의 툴만 갖는 계층 구조로 만든다.
- 함정: **"모든 description을 한 줄로 줄인다."** 문제는 툴의 개수이지 설명 길이가 아닙니다. 줄여도 개수는 그대로고, 오히려 판단 재료가 사라져 정확도가 더 떨어집니다.
- 그 밖의 오답: 툴을 더 늘린다, 모델을 Opus로 고정한다.

참고로 최근 Anthropic은 수천 개 툴 정의를 처음부터 다 넣지 않고 필요할 때 검색해 오는 Tool Search 기능을 추가했습니다. 시험은 여전히 "수를 줄이는 계층화"를 정답 축으로 봅니다.

## 단일 책임

`everything_tool(action="get"|"update"|"delete"...)` 하나보다 `get_user`, `update_user`, `delete_user`, `search_users`로 나누는 것이 정답입니다. 소프트웨어의 SRP(단일 책임 원칙)와 같습니다. 이름은 `verb_noun`(create_user, get_order)으로, 이름만 봐도 부수효과가 있는지 읽히게 짓습니다. 다만 너무 잘게 나눠도 판단 부하가 늘어서 5~15개 안에서 잡습니다.

## MCP 3요소: 호출하면 뭔가 바뀌는가

| 요소 | 목적 | 예 |
|---|---|---|
| Tools | 부수효과가 있는 실행 | create_issue, send_email, write_file |
| Resources | 읽기 전용 참조 데이터, URI로 지정 | `file://...`, `memo://list` |
| Prompts | 재사용 프롬프트 템플릿, 슬래시 커맨드로 호출 | code-review |

기준은 하나입니다. **호출하면 뭔가 바뀌는가?** 바뀌면 Tool, 읽기만 하면 Resource. "파일 내용 가져오기"를 Tool로 만든 것이 시험의 단골 오답 예시입니다.

그 밖에 자주 나오는 것:

- MCP의 가치는 LLM×툴 연결을 N×M에서 N+M으로 줄인 것. Anthropic 전용이 아니라 오픈 사양이고 OpenAI, Google도 대응합니다.
- 구조는 Host(Claude Code / Desktop) → Client → Server, Transport는 stdio(로컬, 클라이언트가 자식 프로세스로 띄웠다 끄는 방식) / Streamable HTTP(리모트).
- 알림(notifications): 툴 목록이 바뀌었을 때, 진행률 보고 등. 클라이언트가 모두 지원하는 것은 아닙니다.
- Claude Code / Desktop과 연동하려면 MCP 하나뿐. 단일 앱 안의 단순 함수 호출이면 직접 Tool Use로 충분.

## 구조화 에러 응답

`return f"Error: {e}"`는 두 가지가 나쁩니다. 내부 정보가 새고, Claude가 다음 판단을 못 합니다. 대신 이렇게 돌려줍니다.

```python
{
  "status": "error",
  "code": "TRANSIENT_FAILURE",
  "message": "메일 서버가 일시적으로 응답하지 않습니다",
  "retryable": True,
  "retry_after_seconds": 30,
  "suggestion": "30초 후 다시 시도하세요",
}
```

`retryable` 플래그의 효과는 "Claude가 재시도할 에러인지 판단해 적절한 복구 동작을 고르기 쉬워진다"입니다. Anthropic이 대신 재시도해 주는 것도, MCP 서버가 자동 재시도하는 것도 아닙니다.

"에이전트가 같은 툴을 끝없이 부른다"는 증상의 가장 흔한 원인이 바로 이것입니다. 에러가 구조화되어 있지 않아 Claude가 왜 실패했는지, 다시 해도 되는지 모릅니다. `retryable: false`를 명시하면 멈춥니다.

## 멱등성(Idempotency)

같은 요청을 여러 번 보내도 결과가 같은 성질입니다. 에이전트 루프에서는 재시도, 병렬 실행, 타임아웃 후 재시도로 같은 툴이 두 번 불리는 일이 실제로 생깁니다. 송금 툴이 멱등하지 않으면 돈이 두 번 나갑니다.

구현은 `idempotency_key`를 받아서, 같은 키로 다시 오면 과거 결과를 그대로 돌려주는 것입니다. 키에는 TTL(24시간~7일)을 둡니다.

- 읽기 전용 툴은 원래 멱등합니다. 쓰기·전송·과금 툴이야말로 멱등성이 필요합니다.
- "description에 '두 번 부르지 마세요'라고 적는다"는 오답입니다. 확률적 부탁이지 보장이 아닙니다.

## 최소 권한

GitHub 읽기만 하는 MCP 서버에 admin 토큰을 주면, Claude가 잘못 판단했을 때 피해가 토큰 범위만큼 커집니다. `repo:read`로 좁힌 토큰이 정답입니다. 에이전트는 "신뢰 경계 바깥의 제3자"로 취급합니다. 토큰은 환경 변수나 Secrets Manager로 전달하고 코드에 하드코딩하지 않습니다.

## 타임아웃과 재시도

- connect(연결까지, 5초 정도)와 read(읽기 완료까지, 작업에 맞게)를 따로 잡습니다. 타임아웃 없음은 무한 대기, 1초 고정은 정상 요청도 끊습니다.
- 4xx는 재시도 안 함(고쳐도 같음). 5xx는 지수 백오프 + 지터. 429는 `Retry-After` 존중.

## MCP 스코프와 `${VAR}`

`claude mcp add <name> --scope <scope> -- <command>`로 등록 위치를 고릅니다.

| 스코프 | 저장 위치 | 공유 범위 |
|---|---|---|
| project | 리포지토리 루트 `.mcp.json` | Git으로 팀 전원 공유 |
| user | `~/.claude.json` | 내 모든 프로젝트 |
| local (기본값) | `~/.claude.json` 안에 프로젝트별로 | 이 프로젝트에서 나만 |

같은 이름이 여러 스코프에 있으면 local > project > user 순으로 우선합니다. 팀 공통은 project, 개인 인증 정보만 local로 덮어쓰는 조합이 실무에서 흔합니다.

project 스코프로 `.mcp.json`을 커밋하면 API 키가 문제입니다. 값에 키를 직접 쓰면 리포지토리 히스토리에 영구히 남습니다. 대신 참조만 적습니다.

```json
{ "mcpServers": { "my-api": { "command": "my-mcp-server",
    "env": { "API_KEY": "${MY_API_KEY}" } } } }
```

실행 시점에 각자 호스트의 환경 변수로 치환됩니다. `${VAR:-default}`처럼 기본값도 됩니다. 시험 문제 "팀과 공유하되 키는 커밋하고 싶지 않다" → **project 스코프 + `${VAR}`**. user 스코프 파일을 Slack으로 뿌리기, private 리포지토리니까 직접 적기, local로 각자 매번 입력하기는 전부 오답입니다.

두 가지 더: 설정 파일 최상위 키는 `mcpServers`이고 이게 없으면 로드되지 않습니다. project 스코프 `.mcp.json`은 팀원이 처음 실행할 때 승인 프롬프트가 뜹니다.

## 공식 가이드 v1.1로 보완하기

교재에 없거나 짧게만 나온 내용을 공식 시험 가이드 v1.1(2026년 11월)의 과제 설명(Task Statement 2.1~2.5)에 맞춰 채운 부분입니다. 예시 속 도구 이름과 숫자는 이해를 돕기 위한 가정입니다.

### 1. 설명이 겹치면 엉뚱한 도구가 불린다 (2.1)

<div class="oa-eli">
  <span class="oa-eli__head">🗄️ 쉽게 말하면</span>
  <p>서랍 두 개에 "서류"와 "문서"라고 붙여 두면 어디에 넣을지 매번 헷갈립니다. "세금 영수증"과 "임대 계약서"라고 붙이면 헷갈리지 않습니다. 모델도 도구 설명만 보고 고르기 때문에 이름과 설명이 비슷하면 잘못 고릅니다.</p>
</div>

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 겹치는 설명</span>
    <ul>
      <li><code>analyze_content</code>: "콘텐츠를 분석한다"</li>
      <li><code>analyze_document</code>: "문서를 분석한다"</li>
      <li>웹 페이지 요청이 문서 도구로 가는 일이 반복됩니다.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 목적별로 나누고 경계를 적음</span>
    <ul>
      <li><code>extract_web_results</code>: "URL의 웹 페이지에서 본문과 게시일을 뽑는다. 입력은 https URL. 업로드된 파일에는 쓰지 않는다."</li>
      <li><code>summarize_pdf_report</code>: "업로드된 PDF 보고서를 요약한다. 입력은 file_id. 웹 URL에는 쓰지 않는다."</li>
    </ul>
  </div>
</div>

좋은 설명에 들어가는 네 가지입니다.

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">📥</span><span class="oa-card__title">입력 형식</span>"주문 번호는 #으로 시작하는 6자리"</div>
  <div class="oa-card"><span class="oa-card__icon">💬</span><span class="oa-card__title">예시 질의</span>"내 주문 어디쯤 왔어?" 같은 요청에 사용</div>
  <div class="oa-card"><span class="oa-card__icon">🚧</span><span class="oa-card__title">경계와 예외</span>취소된 주문은 조회되지 않음</div>
  <div class="oa-card oa-card--hl"><span class="oa-card__icon">↔️</span><span class="oa-card__title">비슷한 도구와의 차이</span>고객 정보는 <code>get_customer</code>, 주문은 이 도구</div>
</div>

한 가지 더: 설명을 잘 써도 **시스템 프롬프트의 키워드**가 이를 덮어쓸 수 있습니다. "고객 관련 질문은 항상 get_customer부터"처럼 적혀 있으면 주문 질문도 그쪽으로 끌려갑니다. 오라우팅이 나면 도구 설명과 시스템 프롬프트를 같이 점검합니다.

시험 문제 "비슷한 두 도구 사이에서 선택이 자주 틀린다" → **도구 설명 개선**이 첫 번째 답입니다. 모델을 더 큰 것으로 바꾸거나 라우팅 분류기를 새로 붙이는 것은 과한 해법입니다.

### 2. 에러는 네 종류로 나눠서 알려 준다 (2.2)

| 범주 | 예시 | 재시도 | 에이전트가 할 일 |
|---|---|---|---|
| 일시적(transient) | 타임아웃, 서비스 일시 중단 | 가능 | 잠시 뒤 재시도 |
| 검증(validation) | 날짜 형식이 틀림 | 입력을 고치면 가능 | 인자를 고쳐 다시 호출 |
| 비즈니스(business) | 환불 기한 초과 | 불가 | 고객에게 설명하거나 에스컬레이션 |
| 권한(permission) | 접근 권한 없음 | 불가 | 다른 경로를 찾거나 에스컬레이션 |

MCP 도구는 결과의 `isError` 플래그로 실패를 알리고, 내용에 구조화된 정보를 담습니다.

```json
{
  "isError": true,
  "content": [{
    "type": "text",
    "text": "{\"errorCategory\": \"business\", \"isRetryable\": false, \"message\": \"구매 후 30일이 지나 환불할 수 없습니다. 교환은 가능합니다.\"}"
  }]
}
```

`"Operation failed"` 같은 일반 에러만 돌려주면 에이전트는 재시도할지, 입력을 고칠지, 사람에게 넘길지 판단할 수 없습니다. 같은 호출을 반복하는 흔한 원인입니다.

위 절의 `is_error: True`는 Claude API의 `tool_result` 블록에 쓰는 필드이고, `isError`는 MCP 도구 결과의 필드입니다. 이름이 비슷하지만 위치가 다릅니다.

### 3. "실패했다"와 "없었다"는 다르다 (2.2)

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">접근 실패: 조회를 못 함</span>
    <ul>
      <li><code>isError: true</code>, <code>errorCategory: "transient"</code></li>
      <li>"검색 서비스 타임아웃"</li>
      <li>재시도할지 결정해야 합니다.</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">정상적인 빈 결과: 조회는 성공</span>
    <ul>
      <li><code>isError: false</code>, <code>results: []</code></li>
      <li>"조건에 맞는 주문 없음"</li>
      <li>재시도해도 같습니다. 고객에게 그대로 알립니다.</li>
    </ul>
  </div>
</div>

타임아웃을 빈 목록으로 돌려주면 에이전트는 "주문이 없다"고 고객에게 잘못 안내합니다.

서브에이전트는 일시적 에러를 **자기 선에서 먼저 복구**(재시도)합니다. 그래도 안 풀린 에러만 코디네이터에 올리고, 그때는 부분 결과와 시도한 내용을 함께 올립니다.

### 4. 도구는 역할별로 나눠 준다 (2.2, 2.3)

도구가 18개인 에이전트 하나보다 4~5개씩 가진 에이전트 여러 개가 도구를 더 정확히 고릅니다.

| 에이전트 | 가진 도구 | 갖지 않는 도구 |
|---|---|---|
| 검색 서브에이전트 | `web_search`, `load_document` | 보고서 작성 도구 |
| 분석 서브에이전트 | `load_document`, `extract_tables` | 웹 검색 |
| 종합 서브에이전트 | `write_report`, `verify_fact` | 웹 검색, 문서 로드 |

역할 밖 도구를 주면 실제로 씁니다. 종합 에이전트에게 웹 검색을 주면 종합하다 말고 검색을 시작합니다.

두 가지 설계 요령입니다.

- **범용 도구를 제한된 도구로 바꿉니다.** 아무 URL이나 받는 `fetch_url` 대신, 허용된 문서 URL인지 검증하는 `load_document`를 줍니다.
- **자주 쓰는 교차 역할 기능은 범위를 좁힌 도구로 줍니다.** 종합 에이전트가 사실 하나를 확인하려고 매번 코디네이터를 거쳐 검색 에이전트를 부르면 왕복이 늘어납니다. 단순 확인용 `verify_fact`만 주고, 복잡한 조사는 계속 코디네이터를 거치게 합니다.

### 5. `tool_choice`로 순서를 강제한다 (2.3)

문서 처리에서 메타데이터 추출이 반드시 먼저여야 한다면, 첫 턴에만 강제 지정을 씁니다.

<ol class="oa-flow">
  <li><strong>1턴: 강제 지정</strong><code>{"type": "tool", "name": "extract_metadata"}</code></li>
  <li><strong>결과 수신</strong>메타데이터가 대화 기록에 들어감</li>
  <li><strong>2턴 이후: auto</strong>모델이 보강 도구를 스스로 고름</li>
</ol>

대화형 답변 대신 도구 호출을 반드시 받아야 하는데 어느 도구인지는 모델에게 맡기려면 `"any"`입니다.

### 6. MCP 리소스와 서버 선택 (2.4)

**리소스는 "무엇이 있는지" 알려 주는 목록**으로 씁니다. 이슈 요약, 문서 계층, 데이터베이스 스키마를 리소스로 노출하면 에이전트가 구조를 알아내려고 탐색용 도구를 여러 번 부를 필요가 없습니다.

<div class="oa-bars">
  <div class="oa-bar"><span>리소스 없음</span><span class="oa-bar__track"><span class="oa-bar__fill oa-bar__fill--low" style="width:100%"></span><span class="oa-bar__val">list_tables → describe × 5 → 쿼리 (7회)</span></span></div>
  <div class="oa-bar"><span>스키마 리소스 제공</span><span class="oa-bar__track"><span class="oa-bar__fill" style="width:15%"></span><span class="oa-bar__val">쿼리 (1회)</span></span></div>
  <div class="oa-bars__caption">테이블 5개인 데이터베이스를 가정한 예시입니다.</div>
</div>

| 상황 | 선택 |
|---|---|
| Jira, GitHub 같은 표준 서비스 연동 | 이미 있는 커뮤니티 MCP 서버 사용 |
| 우리 팀만의 배포 절차, 사내 시스템 | 커스텀 서버 개발 |

여러 서버를 설정하면 모든 서버의 도구가 연결 시점에 발견되어 **한꺼번에** 쓸 수 있게 됩니다. 이때 MCP 도구 설명이 부실하면 에이전트는 익숙한 내장 도구(Grep 등)를 대신 씁니다. 더 적합한 MCP 도구가 선택되게 하려면 설명을 자세히 씁니다.

### 7. 내장 도구 고르기 (2.5)

| 도구 | 쓰는 때 | 예시 |
|---|---|---|
| Grep | 파일 **내용** 검색 | 함수 이름, 에러 메시지, import 문 찾기 |
| Glob | 파일 **경로** 패턴 | `**/*.test.tsx` 파일 목록 |
| Read / Write | 파일 전체 읽기·쓰기 | 설정 파일 통째로 읽기 |
| Edit | 일부만 수정 | 고유한 문자열을 찾아 바꾸기 |
| Bash | 명령 실행 | 테스트 실행, git 명령 |

Edit는 바꿀 문자열이 파일 안에서 **고유**해야 합니다. 같은 줄이 여러 번 나오면 실패합니다.

<ol class="oa-flow">
  <li><strong>Edit 시도</strong><code>return null;</code>을 바꾸려 함</li>
  <li><strong>실패</strong>같은 문자열이 4곳에 있음</li>
  <li><strong>대안</strong>Read로 전체를 읽고 Write로 다시 씀</li>
</ol>

코드베이스를 파악할 때는 처음부터 모든 파일을 읽지 않습니다. Grep으로 진입점을 찾고, Read로 import를 따라가며 흐름을 추적합니다. 래퍼 모듈을 거쳐 쓰이는 함수를 추적할 때는 먼저 export된 이름을 모두 나열하고 이름마다 검색합니다.

<details class="oa-quiz">
  <summary>Q1. 검색 서브에이전트가 타임아웃이 났는데 빈 목록을 성공으로 돌려줬습니다. 무엇이 문제입니까?</summary>
  <p><b>정답: 접근 실패와 정상적인 빈 결과를 구분하지 않았다.</b> 코디네이터는 "자료가 없다"고 판단해 그대로 보고서를 씁니다. 실패 유형, 시도한 질의, 부분 결과를 구조화해서 올려야 합니다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 종합 에이전트가 단순 사실 확인 때문에 코디네이터를 자주 왕복해 지연이 큽니다. 가장 알맞은 수정은?</summary>
  <p><b>정답: 범위를 좁힌 <code>verify_fact</code> 도구를 종합 에이전트에 준다.</b> 모든 검색 도구를 주면 역할 밖 사용이 생기고, 왕복을 그대로 두면 지연이 남습니다.</p>
</details>

## 설계 리뷰 문제 대비

"다음 툴의 문제점 5가지를 쓰라"는 서술형 대비용 체크리스트입니다.

```python
@mcp.tool()
def db_op(query: str) -> str:
    """데이터베이스 조작"""
    conn = connect_with_admin_credentials()
    return str(conn.execute(query).fetchall())
```

1. 임의 SQL을 받는 복합 책임 → `search_users_by_email` 같은 단일 책임으로 분할
2. SQL 인젝션 → 파라미터화 쿼리
3. admin 자격 증명 → 읽기 전용 권한
4. 예외를 그대로 터뜨림 → 구조화 에러
5. description 부실 + `str(...)`로 내부 구조 노출 → 용도 명시, 공개 가능한 필드만 반환

## 핵심 정리

- `tool_result`는 user 역할, `tool_use_id` 1대1, 분기는 `stop_reason`, 병렬은 의존 관계 있으면 끈다.
- 툴이 많으면 개수를 줄인다(서브에이전트·계층화). description을 줄이는 건 함정.
- Tool은 부수효과, Resource는 읽기, Prompt는 템플릿. "호출하면 바뀌는가"로 판정.
- 에러는 `{status, code, message, retryable, suggestion}`. 무한 호출의 원인은 대개 비구조화 에러.
- 부수효과 툴은 `idempotency_key`. 권한은 최소. 타임아웃은 connect/read 분리.
- 팀 공유 + 키 보호 = project 스코프 + `${VAR}`.
- 가이드 보완: 겹치는 설명은 오라우팅을 만든다. 에러는 4범주와 `isRetryable`로 알리고, 접근 실패와 빈 결과를 구분한다. 도구는 역할별 4~5개로 나누고, Edit가 실패하면 Read 후 Write로 돌린다.
