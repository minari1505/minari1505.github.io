---
title: "Software Development Process 강의 설계"
date: 2026-08-02
status: approved
---

# Software Development Process 강의 설계

## 목적

개발 입문자가 아이디어를 바로 coding으로 옮기지 않고 문제·요구사항·설계·구현·검증·운영으로 이어지는 흐름을 이해하게 한다. 하나의 예제를 통해 실제 문서를 작성하고, 마지막에는 전통적 단계 모델과 iterative·incremental·agile 개발의 차이, 추적성과 변경 비용까지 분석할 수 있어야 한다.

## 대상 독자와 예제

- 개발 과정의 문서 이름은 들어봤지만 각 단계에서 무엇을 결정하는지 모르는 개발자
- AI coding 도구에 구현을 맡기기 전에 요구와 완료 기준을 명확히 하고 싶은 개발자
- 전체 강의에서 `팀용 업무 요청 관리 API`를 하나의 연속 예제로 사용한다.
- 예제에는 요청 등록·상태 변경·담당자 배정·목록 조회 기능과 인증·감사·성능 요구를 포함한다.

## 배치

- `Software Engineering` 독립 트랙의 두 번째 course로 등록한다.
- course slug는 `software-development-process`를 사용한다.
- 원문은 일본어 Zenn 글 [요구사항 정의, 기본 설계, 상세 설계의 흐름 복습](https://zenn.dev/nyanchu/articles/27a3f95d98df45)으로 표시한다.
- 원문 저자가 AI 생성 sample과 프로젝트별 차이를 명시한 점을 강의 서두에서 밝힌다.

## 레슨 구성

### 01. 개발 프로세스를 쉽게 이해하기

- ELI15 비유로 문제 발견→요구사항→설계→구현→검증→배포·운영을 설명한다.
- 요구사항 정의는 왜·무엇을, 설계는 어떤 구조와 제약으로 해결할지, 구현은 실행 가능한 system으로 옮기는 일임을 설명한다.
- 일본 SI 문맥의 기본 설계·상세 설계 구분이 보편 표준은 아니며 조직별 경계가 다름을 명시한다.
- waterfall처럼 보이는 그림과 iterative feedback loop를 함께 제시한다.
- 각 단계의 핵심 질문, 입력, 결정, 출력, 검증 주체를 표로 정리한다.

### 02. 요구사항 정의 직접 작성하기

- 현재 문제, business outcome, 성공 metric, 범위, 하지 않을 일을 작성한다.
- stakeholder와 user를 구분하고 직접 확인해야 할 assumption을 기록한다.
- 기능 요구사항과 quality attribute·constraint를 구분한다.
- user story는 요구사항 전체가 아니며, acceptance criteria와 business rule을 함께 써야 함을 설명한다.
- 예제의 one-page requirement brief, use case, Given-When-Then acceptance criteria를 직접 완성한다.
- performance·availability·security 수치는 임의 등급이 아니라 workload와 business impact에서 도출한다.
- 요구사항 review, 승인, 변경 이력과 trace ID를 포함한다.

### 03. 기본 설계와 상세 설계 직접 작성하기

- context boundary, 주요 component, data flow, trust boundary를 기본 설계 수준에서 작성한다.
- API endpoint와 OpenAPI 일부, domain/data model, error contract를 작성한다.
- 중요한 선택을 ADR로 남기고 채택·거절 대안과 이유를 기록한다.
- 상세 설계에서 module interface, validation, transaction boundary, concurrency, failure·retry, logging, test case를 정의한다.
- 요구사항 ID→설계 결정→API·module→test case의 traceability 표를 만든다.
- 설계 문서를 code와 별개로 영구 고정하지 않고 구현·운영 feedback에 맞춰 갱신한다.

### 04. ELIPhD: 현대적 개발 프로세스

- phase-gate, waterfall, iterative, incremental, agile의 차이를 delivery cadence와 feedback latency 관점에서 비교한다.
- requirements engineering의 elicitation, analysis, specification, validation, management를 설명한다.
- architecture design과 detailed design의 경계를 decision scope, reversibility, blast radius로 분석한다.
- traceability graph, change impact analysis, configuration baseline과 decision record를 설명한다.
- DDD가 상세 설계를 없애는 것이 아니라 model과 code의 alignment를 강조한다는 점을 설명한다.
- Agile Manifesto의 `working software over comprehensive documentation`이 문서가 필요 없다는 뜻이 아님을 설명한다.
- AI coding이 구현 latency를 줄여도 잘못된 요구사항·경계·검증 기준의 재작업까지 없애지는 못한다는 점을 다룬다.
- 문서량이 아니라 decision risk, handoff cost, regulation, team topology에 맞춰 artifact를 선택하는 체크리스트를 제공한다.

## 실습 산출물

독자가 다음 Markdown·YAML code block을 복사해 자기 프로젝트에 적용할 수 있게 한다.

- one-page requirement brief
- scope와 non-goals
- stakeholder·assumption·risk 목록
- 기능 요구사항과 측정 가능한 quality scenario
- user story와 Given-When-Then acceptance criteria
- context·component text diagram
- OpenAPI endpoint sample
- ADR sample
- module·error·transaction contract
- requirement-to-test traceability matrix
- change request와 impact analysis checklist

## 원문 보완 원칙

- 원문의 sample과 FAQ가 일부 생성형 AI로 작성됐음을 숨기지 않으며, 검증된 학습 예제로 다시 작성한다.
- `Password VARCHAR(50)` 같은 schema는 사용하지 않는다. password는 현대적 password hashing으로 처리하고 plaintext·복호화 가능한 형태로 저장하지 않는다.
- normalization이 항상 query 성능을 높인다고 설명하지 않는다. integrity·redundancy와 read performance의 trade-off를 다룬다.
- DDD·Agile에서는 상세 설계가 불필요하다는 일반화를 피한다.
- 기본 설계=How, 상세 설계=code 직전이라는 구분은 유용한 heuristic으로만 사용한다.
- availability, latency, capacity 수치는 sample 등급표에서 고르지 않고 SLI·SLO·workload·failure impact로 정의한다.
- 요구사항 승인을 일회성 종착점으로 설명하지 않고 변경 관리와 지속적 validation을 포함한다.

## 우선 출처

- ISO/IEC/IEEE 29148 요구사항 engineering 개요와 접근 가능한 공식 자료
- SWEBOK requirements·design·engineering process 영역
- Agile Manifesto 원문
- C4 model과 ADR의 원 저자 자료
- OpenAPI Specification
- OWASP Password Storage Cheat Sheet
- 원문 Zenn article과 연결된 IPA 비기능 요구사항 자료

## 변경 파일

- `_data/courses.yml`: `Software Development Process` course와 레슨 4개 등록
- `_lessons/software-development-process/01-understanding-the-development-process.md`
- `_lessons/software-development-process/02-writing-requirements.md`
- `_lessons/software-development-process/03-writing-system-design.md`
- `_lessons/software-development-process/04-modern-process-and-traceability.md`

Docker 강의와 공유하는 `_pages/courses-software-engineering.md`를 재사용한다. 모든 새 Markdown 파일에는 YAML frontmatter를 포함한다.

## 검증 기준

- Jekyll build가 성공하고 course·레슨 4개 경로가 생성된다.
- 하나의 예제가 네 레슨에서 같은 용어·요구사항 ID·API 이름을 사용한다.
- 원문에서 보완한 security·database·Agile·DDD 설명이 공식 자료와 모순되지 않는다.
- 모든 template과 sample은 placeholder가 아니라 완성된 예시 값을 포함한다.
- 각 산출물이 다음 단계에서 어떻게 소비되고 test와 연결되는지 설명한다.
- ELI15 설명이 ELIPhD 정의와 모순되지 않는다.
