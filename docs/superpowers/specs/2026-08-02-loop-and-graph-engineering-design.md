---
title: "Loop and Graph Engineering 강의 설계"
date: 2026-08-02
status: approved
---

# Loop and Graph Engineering 강의 설계

## 목적

Claude Code·Codex를 사용하기 시작한 개발자가 loop engineering과 graph engineering을 쉽게 구분하고, 실제 작업에 어떤 구조를 선택할지 판단할 수 있게 한다.

## 배치

- `강의 정리` 아래에 `Agent Engineering` 독립 트랙을 추가한다.
- 강의 slug는 `loop-and-graph-engineering`으로 사용한다.
- Anthropic 공식 강의 트랙과 분리해 여러 공식 문서·논문·사례를 종합한 자체 학습 노트임을 명확히 한다.

## 레슨 구성

### 01. Loop와 Graph를 쉽게 이해하기

- ELI15 수준의 일상 비유로 시작한다.
- loop를 `한 작업자가 결과를 확인하며 반복 개선하는 구조`로 설명한다.
- graph를 `여러 작업자와 작업 사이의 순서·분기·병렬 실행·합류를 표시한 작업 지도`로 설명한다.
- 둘은 대체 관계가 아니며, graph 안에 여러 loop가 들어갈 수 있음을 보여준다.
- 체인, 루프, 분기, fan-out/fan-in을 텍스트 도식으로 비교한다.

### 02. Claude Code·Codex에서 사용하는 방법

- 단일 루프의 구성 요소인 목표, 상태, 행동, 검증, 종료 조건, 예산을 설명한다.
- 그래프의 구성 요소인 node, edge, router, reducer, checkpoint, contract를 실전 언어로 설명한다.
- 문서 작성, 코드 수정, 조사·비교, 테스트 실패 수정 사례를 단계별로 제시한다.
- 단일 호출 → loop → graph 순서로 복잡도를 올리는 선택 기준을 제공한다.
- 독립성이 없는 작업을 억지로 병렬화하거나, 종료 조건 없이 반복시키는 실패 사례와 예방책을 포함한다.

### 03. ELIPhD: 형식적 모델과 설계 원칙

- loop를 상태 전이 시스템과 고정점 탐색 관점에서 설명한다.
- graph를 상태를 공유하거나 전달하는 유향 그래프, DAG, 순환 그래프로 구분한다.
- 의존성 분석, 동시성, fan-in 병목, 라우팅 오류, 부분 실패, 재시도와 멱등성, checkpoint·resume을 다룬다.
- verifier 독립성, 외부 평가 기준, token·시간·비용 예산, 종료 가능성을 분석한다.
- 멀티 에이전트의 이득이 그래프 구조 자체가 아니라 추가 계산량이나 역할 분리에서 생길 수 있다는 한계를 포함한다.
- 마지막에 실무 설계 체크리스트와 선택 표를 제공한다.

## 설명 방식

- 먼저 쉬운 말과 구체적인 비유를 사용하고, 같은 개념을 뒤에서 전문 용어로 다시 정의한다.
- AI가 작성한 듯한 과장된 문구를 피하고 사람이 강의하듯 자연스럽게 쓴다.
- 핵심 관계는 짧은 텍스트 도식과 비교표로 표현한다.
- 새 용어는 처음 등장할 때 한국어 설명과 영문 원어를 함께 적는다.
- 정량 수치는 출처와 적용 조건이 분명할 때만 사용한다.

## 출처 원칙

- Claude Code workflows, Anthropic의 에이전트 패턴, LangGraph·AutoGen·Google ADK 공식 문서를 우선한다.
- loop·graph 관련 논문은 주장과 실험 결과를 구분해 인용한다.
- 두 X 글과 Graph Engineering PDF는 주제의 출발점과 실무 아이디어로 활용하되 권위 있는 표준처럼 표현하지 않는다.
- 별도의 팩트체크 레슨은 만들지 않는다. 검증 결과 중 학습에 필요한 조건과 한계만 해당 개념 옆에 배치한다.

## 변경 파일

- `_data/courses.yml`: 새 트랙과 강의 등록
- `_pages/courses-agent-engineering.md`: Agent Engineering 트랙 페이지 생성
- `_lessons/loop-and-graph-engineering/01-understanding-loops-and-graphs.md`
- `_lessons/loop-and-graph-engineering/02-using-loops-and-graphs.md`
- `_lessons/loop-and-graph-engineering/03-formal-models-and-design-principles.md`

모든 새 Markdown 파일에는 YAML frontmatter를 포함한다. 기존 Agent Skills 레슨과 사용자가 수정 중인 파일은 변경하지 않는다.

## 검증 기준

- Jekyll 빌드가 성공해야 한다.
- 강의 허브에서 새 트랙과 세 레슨으로 이동할 수 있어야 한다.
- 모든 레슨의 frontmatter와 `courses.yml`의 번호·slug가 일치해야 한다.
- 출처 링크가 실제 주장과 연결되어야 한다.
- 쉬운 설명을 읽은 뒤 개념을 구분할 수 있고, 전문가 설명을 읽은 뒤 설계상의 비용과 실패 조건까지 판단할 수 있어야 한다.
