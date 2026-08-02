---
title: "Connectors"
title_ko: "Connectors로 Claude 확장하기"
course: claude-101
lesson: 7
---

## 학습 목표

- Connectors가 무엇이고 Claude와의 작업에서 왜 중요한지 이해하기
- Connector directory에서 도구를 찾고 첫 연결을 설정하는 방법 익히기
- 연결된 도구를 Claude 대화에서 효과적으로 사용하는 방법 정리하기
- Web connectors와 desktop extensions의 차이 이해하기

## Connectors란?

**Connectors**는 Claude가 사용자의 실제 업무 도구, 데이터, 맥락에 접근할 수 있게 해주는 연결 기능입니다.

Connector가 없으면 사용자는 매번 정보를 복사해서 Claude에게 붙여넣어야 합니다. Connector를 사용하면 Claude가 Google Drive, Notion, Slack, Asana 같은 서비스에서 필요한 정보를 직접 찾거나, 권한에 따라 특정 작업을 수행할 수 있습니다.

간단히 말하면 connector는 Claude를 "대답하는 assistant"에서 **내가 매일 쓰는 도구와 같은 정보를 보는 collaborator**로 바꿉니다.

## Connectors가 중요한 이유

Claude가 더 좋은 답을 하려면 맥락이 필요합니다. 업무 맥락은 보통 chat 안에만 있지 않고 여러 도구에 흩어져 있습니다.

- 문서는 Google Drive나 Notion에 있음
- 의사결정은 Slack thread에 있음
- task는 Asana, Linear, Jira에 있음
- 고객 정보는 Salesforce에 있음
- 결제나 매출 데이터는 Stripe, PayPal에 있음

Connectors는 이런 정보를 Claude가 대화 안에서 활용할 수 있게 합니다.

## Connectors가 할 수 있는 일

Connector와 부여한 권한에 따라 Claude는 다음 작업을 할 수 있습니다.

- 파일 검색
- 문서 가져오기
- 데이터 분석
- 새 content 생성
- record 업데이트
- 연결된 앱 안에서 task 실행

모든 connector가 동일한 권한을 갖는 것은 아닙니다. 어떤 connector는 읽기 중심이고, 어떤 connector는 작성이나 수정 action도 지원할 수 있습니다.

## MCP와 Connectors

Connectors는 **Model Context Protocol(MCP)**을 기반으로 합니다.

MCP는 AI를 위한 USB-C 같은 표준으로 볼 수 있습니다. 하나의 일관된 interface를 통해 Claude가 여러 application과 연결될 수 있게 합니다.

```text
Claude
  ↓
MCP 기반 Connector
  ↓
Google Drive / Notion / Slack / Asana / 로컬 앱 ...
```

MCP가 open standard이기 때문에 개발자는 다양한 도구를 위한 connector를 만들 수 있고, Claude는 이를 일관된 방식으로 사용할 수 있습니다.

## Connector의 두 가지 유형

### Web connectors

Web connectors는 cloud service와 Claude를 연결합니다.

예:

- Gmail
- Notion
- Slack
- Asana
- Linear
- Stripe
- Google Drive

브라우저 기반 Claude에서도 사용할 수 있고, 서비스 로그인과 권한 승인을 통해 연결합니다.

### Desktop extensions

Desktop extensions는 Claude Desktop app에서 로컬로 실행되는 확장입니다. Claude가 내 컴퓨터의 local files, native applications, macOS/Windows 기능과 상호작용할 수 있게 합니다.

예:

- 로컬 파일 접근
- browser control
- Figma 같은 native application integration

Desktop extensions는 웹 인터페이스가 아니라 Claude Desktop app이 필요합니다.

## Connector 찾기

Anthropic은 추천 connector directory를 제공합니다.

```text
claude.ai/directory
```

또는 chat window의 왼쪽 아래 `+` 버튼을 누른 뒤 `Connectors`를 선택해 connector를 찾을 수 있습니다.

Directory는 보통 두 탭으로 구성됩니다.

| 탭 | 설명 |
|---|---|
| **Web** | Gmail, Notion, Slack, Asana, Linear, Stripe 같은 cloud service |
| **Desktop extensions** | Claude Desktop app을 통해 로컬에서 실행되는 도구 |

## Web connector 설정하기

Cloud service를 연결하는 기본 흐름은 다음과 같습니다.

1. `claude.ai/directory`로 이동하거나 chat에서 `+ > Connectors`를 선택합니다.
2. 추가하려는 connector를 찾습니다.
3. `Connect`를 클릭합니다.
4. 해당 서비스의 login page로 이동해 기존 계정으로 로그인합니다.
5. Claude가 요청하는 권한을 검토하고 승인합니다.
6. Claude로 돌아와 간단한 요청으로 연결을 테스트합니다.

예:

```text
내 Google Drive에 접근할 수 있어?
```

연결 후에는 부여한 권한에 따라 Claude가 해당 서비스에서 검색, 읽기, 일부 action 수행을 할 수 있습니다.

## Desktop extension 설치하기

Desktop extension은 Claude Desktop app에서 설정합니다.

1. Claude Desktop app을 설치합니다.
2. 앱에서 `Settings > Extensions`로 이동합니다.
3. 사용 가능한 extensions를 둘러봅니다.
4. 원하는 extension을 `Install`합니다.
5. extension별 추가 설정을 완료합니다.

Desktop extension은 로컬 파일이나 native app과 연결되므로, web connector보다 local environment와 더 가까운 작업에 적합합니다.

## 업무에서 Connectors 사용하기

Connector를 연결하면 Claude가 대화 중 관련 도구를 고려할 수 있습니다.

### Project management

Asana, Linear, Jira 같은 도구와 연결하면 다음처럼 물을 수 있습니다.

```text
이번 주 마감인 가장 우선순위 높은 task가 뭐야?
```

```text
Q4 budget proposal review task를 새로 만들어줘.
```

```text
제품 launch project의 현재 상태를 요약해줘.
```

### Communication

Slack, Gmail과 연결하면 대화와 이메일 맥락을 찾을 수 있습니다.

```text
vendor contract에 대해 논의한 email thread를 찾아줘.
```

```text
#marketing 채널의 최신 메시지에 답장 초안을 써줘.
```

```text
어제 논의에서 timeline에 대해 팀이 결정한 내용을 요약해줘.
```

### Documentation

Notion, Google Drive, Confluence와 연결하면 문서 기반 작업이 쉬워집니다.

```text
우리 brand voice guideline을 documentation에서 찾아줘.
```

```text
지난주 product review 회의록을 요약해줘.
```

```text
우리 style guide는 contraction 사용에 대해 뭐라고 해?
```

### Business tools

Stripe, PayPal, Salesforce 같은 도구는 business data에 접근하는 데 유용합니다.

```text
지난 분기 revenue trend를 보여줘.
```

```text
Acme Corp opportunity 상태가 어떻게 돼?
```

```text
$1,000를 넘는 최근 transaction을 나열해줘.
```

## 보안과 권한

Connector를 연결한다는 것은 Claude에게 외부 서비스의 데이터를 읽거나, 경우에 따라 수정할 권한을 주는 것입니다.

중요한 원칙은 다음과 같습니다.

### Scoped access

권한은 connector가 필요한 범위로 제한됩니다. 각 application menu에서 개별 권한을 켜거나 끌 수 있습니다.

### Claude sees what you see

Claude는 사용자가 접근 권한을 가진 데이터만 볼 수 있습니다. 예를 들어 work email을 연결해도 CEO의 inbox를 볼 수 있는 것은 아닙니다. 내가 접근할 수 있는 데이터만 Claude가 볼 수 있습니다.

### 언제든 해제 가능

Claude settings나 third-party service의 security settings에서 연결을 해제할 수 있습니다.

### 신뢰할 수 있는 connector 사용

Skills와 마찬가지로 custom connector도 만들거나 설치할 수 있습니다. 외부 source의 connector는 신뢰할 수 있는 곳에서만 설치하고, 어떤 권한을 요청하는지 확인해야 합니다.

## 실습 질문

다음 질문을 생각해보면 connector 활용처를 찾기 쉽습니다.

- 내가 매일 쓰는 도구 중 Claude와 연결하면 가장 가치가 큰 것은 무엇인가?
- 지금은 복사해서 붙여넣는 정보 중 connector로 자동 처리할 수 있는 것은 무엇인가?
- 여러 source의 데이터를 합쳐야 해서 시간이 오래 걸리는 workflow는 무엇인가?

## 핵심 정리

Connectors는 Claude가 실제 업무 도구와 데이터에 접근하게 해줍니다.

- Web connectors는 cloud service와 연결합니다.
- Desktop extensions는 Claude Desktop app에서 로컬 도구와 연결합니다.
- Connectors는 MCP를 기반으로 하며, 다양한 도구를 일관된 방식으로 Claude에 붙일 수 있게 합니다.
- 권한은 connector별로 관리되며, Claude는 사용자가 접근 가능한 데이터만 볼 수 있습니다.
- 연결은 언제든 해제할 수 있고, custom connector는 신뢰할 수 있는 source에서만 사용해야 합니다.

다음 레슨에서는 조직 지식을 검색하고 종합하는 Claude for Work 기능인 Enterprise Search를 다룹니다.
