---
title: "Enterprise Search"
title_ko: "Enterprise Search"
course: claude-101
lesson: 8
---

## 학습 목표

- Enterprise Search가 무엇이고 어떤 질문에 적합한지 이해하기
- admin과 user 각각의 설정 흐름 파악하기
- 조직 데이터가 permission과 security model로 어떻게 보호되는지 정리하기

## Enterprise Search란?

**Enterprise Search**는 Claude for Work 사용자를 위한 조직 지식 검색 기능입니다. Sidebar에 `Ask {Your Org Name}` 같은 전용 항목이 생기고, 회사의 여러 도구와 데이터 source에 흩어진 지식을 찾아 종합할 수 있습니다.

간단히 말하면, Enterprise Search는 조직 전체를 위한 pre-built Project처럼 동작합니다.

```text
회사 문서 / Slack / email / Drive / SharePoint
  ↓
Enterprise Search
  ↓
Claude가 source를 인용하며 종합 답변
```

일반 chat에서 connector를 켜는 것과 달리, Enterprise Search는 정보 수집과 종합에 최적화되어 있고 Anthropic 팀이 구성한 custom instructions를 사용합니다.

Enterprise Search는 Team과 Enterprise plan에서 사용할 수 있으며, workspace admin이 활성화해야 합니다. Free, Pro, Max 사용자는 실습 없이 개념만 이해하면 됩니다.

## 어떤 질문에 적합한가?

Enterprise Search는 여러 source에 흩어진 정보를 찾아야 하거나, 조직 안의 다양한 문서와 대화를 종합해야 할 때 특히 유용합니다.

### 빠르게 맥락 파악하기

```text
내가 없는 동안 어제 무슨 일이 있었어?
```

```text
지난주 비즈니스 전반의 주요 업데이트를 요약해줘.
```

```text
Platform project의 현재 blocker가 뭐야?
```

### 정책과 프로세스 질문

```text
우리 회사의 remote work policy가 뭐야?
```

```text
expense report는 어떻게 제출해?
```

```text
휴가 요청 프로세스가 어떻게 돼?
```

### Research와 analysis

```text
고객들이 경쟁사를 선택하는 주요 이유는 뭐라고 말하고 있어?
```

```text
Q4 product roadmap에 대한 논의를 요약해줘.
```

```text
우리 customer onboarding process에 대한 정보를 찾아줘.
```

### 신규 팀원 onboarding

```text
우리 authentication system은 어떻게 동작해?
```

```text
billing system을 배우려면 누구와 이야기해야 해?
```

```text
engineering team은 deployment에 어떤 tool을 써?
```

### Performance와 project tracking

```text
marketing campaign 관련 논의와 문서를 찾아줘.
```

```text
지난주 leadership meeting의 핵심 결정사항은 뭐였어?
```

```text
Infrastructure initiative에 대한 팀별 contribution을 요약해줘.
```

Claude는 SharePoint 문서, Slack 대화, Gmail thread, Google Drive 파일 같은 연결된 tool을 검색하고, 결과를 하나의 응답으로 종합합니다. 응답에는 source citation이 포함되므로 전체 맥락을 확인하기 쉽습니다.

## Admin 설정 흐름

Enterprise Search는 Team/Enterprise 조직에 기본 제공될 수 있지만, 실제 사용 전 Owner가 초기 설정을 완료해야 합니다.

Admin 설정 흐름은 다음과 같습니다.

1. 왼쪽 sidebar에서 `Ask Your Org`를 클릭합니다.
2. `Set up for your org`를 클릭합니다. 필요 없으면 `Disable`로 끌 수 있습니다.
3. 조직 도구를 연결합니다.
   - Documents connector: Google Drive, SharePoint 등
   - Chat connector: Slack, Microsoft Teams 등
   - Email connector: 권장되지만 선택 사항
4. `+ Add more`로 추가 도구를 설정합니다.
5. project 이름을 지정합니다. 이 이름은 모두의 sidebar에 `Ask [Name]`으로 표시됩니다.
6. 설명을 추가하고 `Finish set up`을 클릭합니다.

설정이 끝나면 조직 구성원들이 Enterprise Search를 사용할 수 있습니다.

## User 설정 흐름

Admin이 Enterprise Search를 설정하면 사용자는 sidebar에서 `Ask {Org Name}` project를 볼 수 있습니다.

처음 사용할 때는 다음 단계를 거칩니다.

1. sidebar에서 해당 project를 클릭합니다.
2. guided onboarding flow를 따라 권장 service에 연결합니다.
3. Slack, Google, Microsoft 365 같은 서비스에 개인 계정으로 인증합니다.
4. 조직 지식에 대해 Claude에게 질문을 시작합니다.

더 많은 connector를 활성화할수록 검색 결과가 더 포괄적입니다. 나중에도 project의 Instructions section에서 `Connect`를 눌러 추가 connector를 붙일 수 있습니다.

## 보안과 권한

Enterprise Search는 많은 조직 데이터를 다루므로 보안이 중요합니다. 강의의 핵심 답은 간단합니다.

> Claude는 원래 연결된 tool에서 사용자가 볼 수 있는 것만 보여줍니다.

즉, Enterprise Search는 사용자의 기존 권한을 넘지 않습니다.

- 내가 접근 권한이 없는 문서는 Claude도 볼 수 없습니다.
- 내 대화는 private하게 유지됩니다.
- 연결된 데이터는 별도 index로 저장되거나 복사되지 않습니다.
- 원래 tool의 permission model이 그대로 적용됩니다.

따라서 Enterprise Search는 조직 지식을 넓게 검색하지만, 접근 제어는 기존 system의 권한을 따릅니다.

## Enterprise Search와 일반 connectors의 차이

| 구분 | 일반 connectors | Enterprise Search |
|---|---|---|
| 목적 | 특정 chat에서 도구와 데이터 연결 | 조직 지식 검색과 종합에 특화 |
| 설정 | 개인이 connector를 연결 | admin이 조직 단위로 설정 후 사용자가 인증 |
| 범위 | 연결한 tool 중심 | 조직의 documents, chat, email 등 여러 source |
| 응답 방식 | 요청에 따라 tool 사용 | source citation이 있는 종합 답변 |
| 대상 plan | connector별 상이 | Team/Enterprise |

일반 connectors가 "내가 연결한 도구를 Claude가 쓰는 것"이라면, Enterprise Search는 "조직의 지식 기반을 검색하는 전용 경험"에 가깝습니다.

## 실습 질문

다음 질문을 생각해보면 Enterprise Search의 가치를 판단하기 쉽습니다.

- 평소 동료에게 자주 묻는 질문 중 문서와 대화 검색으로 답할 수 있는 것은 무엇인가?
- 신규 입사자가 빠르게 맥락을 파악하는 데 Enterprise Search가 도움이 될 영역은 어디인가?
- 내 역할에서 연결하면 가장 가치가 큰 data source는 무엇인가?
- 회사의 정책, 프로세스, 과거 의사결정이 어디에 흩어져 있는가?

## 핵심 정리

Enterprise Search는 조직 내부 지식을 찾고 종합하기 위한 Claude for Work 기능입니다.

- Sidebar에 `Ask {Org Name}` 형태의 전용 검색 experience가 생깁니다.
- 여러 connected tools의 문서, 대화, email을 검색하고 종합합니다.
- source citation이 포함되어 검증과 추가 탐색이 쉽습니다.
- Admin이 조직 단위로 설정하고, 사용자는 개인 계정으로 각 service에 인증합니다.
- Claude는 사용자가 원래 접근할 수 있는 데이터만 볼 수 있습니다.

다음 레슨에서는 public web과 connected integrations를 함께 활용해 더 깊은 조사를 수행하는 Research mode를 다룹니다.
