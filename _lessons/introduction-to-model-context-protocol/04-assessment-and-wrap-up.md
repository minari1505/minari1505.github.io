---
title: "Assessment and Wrap Up"
title_ko: "평가 & 마무리"
course: introduction-to-model-context-protocol
lesson: 4
---

## 학습 목표

- MCP server의 세 가지 핵심 primitive를 다시 정리하기
- tools, resources, prompts가 각각 누구에 의해 제어되는지 구분하기
- 어떤 상황에서 어떤 primitive를 선택해야 하는지 판단 기준 만들기
- MCP를 실제 애플리케이션 구조 안에서 어떻게 바라봐야 하는지 마무리하기

## MCP server의 세 가지 핵심 primitive

이번 강의에서 MCP server는 세 가지 핵심 primitive를 제공할 수 있다고 배웠습니다.

![MCP primitive review](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1749849326%2F09_-_011_-_MCP_Review_00.1749849326738.png)

| Primitive | 제어 주체 | 핵심 역할 |
|---|---|---|
| **Tools** | 모델이 제어 | Claude가 필요하다고 판단하면 호출하는 실행 기능 |
| **Resources** | 애플리케이션이 제어 | 앱이 데이터를 가져와 UI나 prompt context에 넣는 정보 |
| **Prompts** | 사용자가 제어 | 사용자가 직접 실행하는 사전 정의 workflow |

핵심은 세 primitive가 모두 "Claude에게 뭔가를 제공한다"는 점에서는 비슷하지만, **누가 언제 사용할지 결정하는가**가 다르다는 것입니다.

## Tools: 모델이 제어하는 기능

Tool은 Claude가 직접 호출 여부를 판단합니다. 사용자가 목표를 말하면 Claude가 필요한 tool을 고르고, 인자를 구성하고, 결과를 받아 다음 답변이나 다음 행동에 사용합니다.

예를 들어 사용자가 이렇게 요청한다고 해봅시다.

```text
JavaScript로 3의 제곱근을 계산해줘.
```

Claude는 이 요청을 처리하기 위해 JavaScript 실행 tool을 써야겠다고 판단할 수 있습니다. 이때 tool 호출을 결정하는 주체는 애플리케이션도 사용자도 아니라 **모델**입니다.

Tool이 적합한 경우는 다음과 같습니다.

- Claude에게 새로운 실행 능력을 주고 싶을 때
- Claude가 상황에 따라 호출 여부를 판단해야 할 때
- API 호출, 계산, 문서 수정, DB query처럼 행동이 필요한 경우
- 여러 tool을 조합해 task를 해결해야 하는 경우

```text
사용자 목표
  ↓
Claude가 필요한 tool 판단
  ↓
tool call 생성
  ↓
MCP server가 실행
  ↓
Claude가 결과를 사용
```

## Resources: 애플리케이션이 제어하는 정보

Resource는 애플리케이션 코드가 제어합니다. 앱이 어떤 resource를 보여줄지, 언제 가져올지, 가져온 내용을 prompt에 어떻게 넣을지 결정합니다.

이 강의 프로젝트에서는 resource를 두 가지 방식으로 사용했습니다.

1. UI의 autocomplete option을 채우기 위해 resource 목록을 가져오기
2. 문서 내용을 읽어 Claude에게 줄 prompt context에 포함하기

Claude의 인터페이스에서 "Google Drive에서 추가" 같은 기능을 떠올리면 이해하기 쉽습니다. 어떤 문서를 보여줄지, 사용자가 선택한 문서를 어떻게 context에 넣을지는 애플리케이션이 처리합니다. Claude는 이미 context에 들어온 내용을 바탕으로 답합니다.

Resource가 적합한 경우는 다음과 같습니다.

- 앱 UI에 보여줄 데이터가 필요할 때
- Claude에게 줄 context를 앱이 미리 구성해야 할 때
- 문서, 파일, JSON, DB record처럼 읽기 중심 데이터가 필요할 때
- 별도 tool round-trip 없이 데이터를 prompt에 바로 포함하고 싶을 때

```text
사용자 입력 또는 UI 이벤트
  ↓
앱이 resource 필요 여부 판단
  ↓
MCP client가 resource 읽기
  ↓
앱이 prompt context 구성
  ↓
Claude에게 전달
```

## Prompts: 사용자가 제어하는 workflow

Prompt는 사용자가 실행을 선택하는 사전 정의 workflow입니다. 버튼, 메뉴, slash command 같은 UI 동작을 통해 사용자가 직접 trigger합니다.

예를 들어 문서 관리 MCP server가 `/format` prompt를 제공한다면, 사용자는 이 command를 선택해 특정 문서를 Markdown으로 재정리하는 workflow를 시작할 수 있습니다.

Prompt가 유용한 이유는 사용자가 직접 긴 지시문을 잘 작성하지 않아도 된다는 점입니다. MCP server 작성자가 미리 평가하고 다듬은 고품질 prompt를 제공하면, 사용자는 클릭이나 command 하나로 안정적인 workflow를 실행할 수 있습니다.

Prompt가 적합한 경우는 다음과 같습니다.

- 반복되는 작업을 workflow로 제공하고 싶을 때
- 사용자가 명시적으로 실행해야 하는 기능일 때
- prompt engineering 지식을 server 작성자가 대신 담아두고 싶을 때
- 문서 요약, 포맷팅, 보고서 생성처럼 정해진 절차가 있는 경우

```text
사용자가 slash command 또는 버튼 선택
  ↓
앱이 MCP prompt 요청
  ↓
prompt에 인자 삽입
  ↓
Claude에게 완성된 message 전달
  ↓
Claude가 workflow 수행
```

## 선택 기준

세 primitive 중 무엇을 쓸지 헷갈릴 때는 "누가 제어해야 하는가?"를 먼저 보면 됩니다.

| 질문 | 선택 |
|---|---|
| Claude가 필요할 때 자율적으로 호출해야 하는가? | **Tool** |
| 앱이 데이터를 가져와 UI나 context에 넣어야 하는가? | **Resource** |
| 사용자가 명시적으로 시작하는 workflow인가? | **Prompt** |

조금 더 실전적으로 보면 다음과 같습니다.

- Claude에게 새로운 능력을 주고 싶다 → tool
- 앱 화면에 보여줄 데이터가 필요하다 → resource
- 사용자가 선택할 수 있는 고정 workflow를 만들고 싶다 → prompt
- 문서를 수정하거나 외부 API를 호출해야 한다 → tool
- 문서 내용을 처음부터 context에 넣고 싶다 → resource
- "문서 포맷팅", "보고서 생성" 같은 command를 제공하고 싶다 → prompt

## Claude 인터페이스에서 보는 예시

Claude의 공식 인터페이스에서도 세 primitive의 개념을 볼 수 있습니다.

- 채팅 입력창 아래의 workflow 버튼들: prompts에 가까운 개념
- Google Drive 같은 문서 연결 기능: resources에 가까운 개념
- 코드 실행, 계산, 외부 기능 호출: tools에 가까운 개념

물론 실제 내부 구현은 제품마다 다를 수 있지만, 애플리케이션을 설계할 때 이 구분은 유용합니다.

## 전체 강의 정리

이번 MCP 입문 강의에서는 다음 흐름을 살펴봤습니다.

1. MCP가 외부 서비스 통합 부담을 줄이는 표준 통신 계층이라는 점
2. MCP server가 tools, resources, prompts를 제공한다는 점
3. Python SDK의 `FastMCP`와 decorator로 server를 쉽게 만들 수 있다는 점
4. MCP Inspector로 server 기능을 독립적으로 테스트할 수 있다는 점
5. MCP client가 `list_tools`, `call_tool`, `read_resource`, `get_prompt`로 server 기능을 앱에 연결한다는 점
6. tools, resources, prompts는 각각 모델, 앱, 사용자가 제어한다는 점

## 핵심 정리

MCP를 단순히 "tool을 연결하는 방법"으로만 보면 좁게 이해하게 됩니다. MCP는 AI 애플리케이션이 외부 시스템과 상호작용하는 방식을 세 가지 레벨로 나눠 표준화합니다.

- **Tools**는 Claude에게 행동 능력을 줍니다.
- **Resources**는 앱이 Claude에게 줄 context를 구성하게 해줍니다.
- **Prompts**는 사용자가 실행할 수 있는 검증된 workflow를 제공합니다.

이 세 가지를 적절히 나누면, 외부 시스템 통합이 더 재사용 가능하고 유지보수하기 쉬운 구조가 됩니다. MCP server는 서비스별 기능을 표준화해 제공하고, MCP client는 이를 애플리케이션과 Claude workflow에 연결합니다.

결국 MCP의 핵심 가치는 **통합 로직을 애플리케이션 내부에 흩뿌리지 않고, 표준화된 서버로 분리해 재사용하는 것**입니다.
