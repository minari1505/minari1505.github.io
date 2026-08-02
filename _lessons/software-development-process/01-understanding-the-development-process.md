---
title: "Understanding the Development Process"
title_ko: "개발 프로세스를 쉽게 이해하기"
course: software-development-process
lesson: 1
tags:
  - Software Engineering
  - Requirements
  - System Design
---

## 학습 목표

- 개발 단계를 외우지 않고 각 단계가 답해야 할 질문으로 이해하기
- 요구사항·기본 설계·상세 설계의 경계를 상황에 맞게 판단하기
- 선형 절차와 실제 개발의 feedback loop를 함께 보기
- 다음 강의에서 사용할 ‘팀용 업무 요청 관리 API’ 문제 이해하기

## ELI15: 식당에서 주문 하나를 처리하려면

“배고프니 음식을 주세요”만으로 주방은 움직이기 어렵습니다.

- 무엇을 해결할까: 손님이 점심을 먹고 싶다.
- 무엇을 제공할까: 메뉴 선택, 주문, 결제, 상태 확인이 필요하다.
- 어떻게 나눌까: 홀은 주문을 받고 주방은 조리하고 결제기는 돈을 받는다.
- 내부에서 어떻게 처리할까: 주문 번호를 만들고 조리 순서를 저장한다.
- 제대로 됐는지 어떻게 알까: 주문한 음식과 받은 음식이 같은지 확인한다.
- 문을 연 뒤 무엇을 볼까: 지연, 품절, 결제 실패를 관찰한다.

Software 개발도 같습니다. 바로 code부터 쓰면 “무엇을 만들지”와 “어떻게 만들지”가 한꺼번에 섞입니다. 개발 프로세스는 답해야 할 질문을 순서와 책임으로 나눈 지도입니다.

## 이번 과정의 연속 예제

회사에서 업무 요청이 메신저·email로 흩어져 담당자와 진행 상태를 찾기 어렵다고 합시다. 우리는 다음 기능을 가진 **팀용 업무 요청 관리 API**를 설계합니다.

- 직원이 업무 요청을 등록한다.
- 운영자가 담당자를 배정한다.
- 담당자가 허용된 순서로 상태를 바꾼다.
- 사용자가 요청 목록과 변경 이력을 조회한다.

2강에서는 `REQ-*`, `NFR-*` 요구사항을 쓰고, 3강에서는 같은 ID를 API·data model·test로 연결합니다. 4강에서는 변경이 생겼을 때 이 연결을 따라 영향 범위를 찾습니다.

## 기본 흐름: 직선으로 그리되 실제로는 되돌아간다

```text
문제 → 요구사항 → 설계 → 구현 → 검증 → 운영
 ↑        ↑         ↑       ↑       ↑       │
 └────────┴─────────┴───────┴───────┴───────┘ feedback
```

왼쪽에서 오른쪽으로 갈수록 모호한 business 문제를 실행 가능한 system으로 바꿉니다. 그러나 검증 실패와 운영 data는 앞 단계의 잘못된 가정까지 되돌려 보냅니다. 따라서 이 그림은 한 번 통과하고 끝나는 conveyor belt가 아니라 학습 loop입니다.

## 단계마다 답해야 할 질문

| 단계 | 핵심 질문 | 주요 입력 | 결정·산출물 | 주 검증자 |
|---|---|---|---|---|
| 문제 발견 | 누구의 어떤 손실을 줄이는가? | 현장 관찰, 문의, 지표 | problem statement, outcome | 사용자·업무 책임자 |
| 요구사항 | 무엇을 만족하면 성공인가? | 문제, 제약, 정책 | scope, REQ/NFR, acceptance criteria | product owner·사용자·QA |
| 기본 설계 | system을 어떤 책임과 경계로 나눌까? | 요구사항, 품질 목표 | component, data flow, API·DB 개요, ADR | architect·개발·보안·운영 |
| 상세 설계 | 각 부분이 정확히 어떤 contract로 동작할까? | 기본 설계, 기술 제약 | schema, interface, transaction, error, algorithm | 구현자·reviewer |
| 구현 | 설계를 실행 가능한 변경으로 어떻게 만들까? | 설계, coding standard | code, migration, configuration | 동료 개발자·자동 검사 |
| 검증 | 요구사항과 품질 목표를 만족하는가? | REQ/NFR, build | test result, defect, release evidence | QA·사용자·보안 |
| 운영 | 실제 환경에서 가치와 안정성이 유지되는가? | 배포물, telemetry | metric, alert, incident, 개선 backlog | 운영자·service owner |

산출물 이름보다 중요한 것은 decision과 검증 가능성입니다. 작은 팀은 한 문서에 여러 단계를 담을 수 있고, 규제 환경은 각 결정을 별도 문서와 승인으로 관리할 수 있습니다.

## 요구사항·기본 설계·상세 설계는 어디서 나뉠까?

일본 SI 현장에서 자주 쓰는 구분은 대략 다음과 같습니다.

- 요구사항 정의: system이 제공할 가치·기능·품질·제약
- 기본 설계(외부 설계): 화면, API, data, component처럼 system 외부와 큰 구조
- 상세 설계(내부 설계): class·module·transaction·error handling처럼 구현에 가까운 결정

유용한 출발점이지만 국제적으로 고정된 경계는 아닙니다. 조직에 따라 high-level design/low-level design, architecture/design, solution design 같은 이름을 씁니다. 다음 질문으로 경계를 잡는 편이 안전합니다.

1. 이 결정의 독자는 누구인가?
2. 잘못됐을 때 몇 팀과 system에 영향을 주는가?
3. 구현자가 추가 추측 없이 test 가능한 code를 만들 수 있는가?
4. 변경 비용이 크거나 규제상 승인 기록이 필요한가?

## 세 가지 흔한 오해

### “Agile이면 설계하지 않는다”

Agile Manifesto는 documentation의 가치가 없다고 하지 않습니다. 동작하는 software를 더 중시한다고 말합니다. 큰 설계를 한 번에 얼리는 대신 가까운 결정을 충분히 설계하고 feedback으로 갱신합니다.

### “요구사항 승인이 끝나면 바뀌면 안 된다”

승인은 당시 baseline에 대한 합의입니다. 새로운 정보로 변경할 수 있으며, 변경 이유·영향·승인자·version을 추적해야 합니다. 변화 자체보다 몰래 바뀌는 것이 위험합니다.

### “문서가 많을수록 안전하다”

Code와 다른 문서는 위험한 두 번째 진실이 됩니다. 문서는 중요한 decision, boundary, contract, 검증 기준처럼 미래 독자가 다시 추론하기 비싼 내용을 우선합니다.

## 바로 적용하는 작업 순서

새 기능을 시작할 때 issue나 design document에 다음 여섯 문장을 먼저 채웁니다.

```text
문제: 업무 요청이 채널별로 흩어져 담당자와 상태를 추적할 수 없다.
성공: 등록된 요청의 95%가 1영업일 안에 담당자를 가진다.
범위: 등록, 배정, 상태 변경, 목록 조회.
제외: 일정 관리, 비용 정산, 외부 고객용 UI.
큰 결정: API, identity provider, PostgreSQL, audit sink 경계를 둔다.
검증: acceptance test와 운영 metric을 요구사항 ID에 연결한다.
```

이 여섯 줄이 충분한 설계서는 아닙니다. 하지만 문제와 solution을 섞기 전에 대화를 시작하게 해 줍니다.

## 핵심 정리

1. 개발 프로세스는 문서 목록이 아니라 불확실성을 줄이는 질문과 feedback 구조입니다.
2. 문제 → 요구사항 → 설계 → 구현 → 검증 → 운영으로 설명할 수 있지만 실제 흐름은 반복됩니다.
3. 기본·상세 설계의 이름과 경계는 조직마다 다르므로 decision 범위와 독자로 판단합니다.
4. Agile도 설계·문서·승인이 필요하며, 필요한 시점과 양을 조절합니다.

## 참고 자료

- [Zenn — 要件定義、基本設計、詳細設計](https://zenn.dev/nyanchu/articles/27a3f95d98df45)
- [IEEE Computer Society — SWEBOK](https://www.computer.org/education/bodies-of-knowledge/software-engineering)
- [Manifesto for Agile Software Development](https://agilemanifesto.org/)
- [Principles behind the Agile Manifesto](https://agilemanifesto.org/principles.html)
