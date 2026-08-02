---
title: "Software Development Process 구현 계획"
date: 2026-08-02
status: active
---

# Software Development Process Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 하나의 업무 요청 관리 API 예제로 요구사항·기본 설계·상세 설계·현대적 개발 프로세스를 직접 작성하는 4개 레슨을 추가한다.

**Architecture:** Docker plan이 생성한 `Software Engineering` 트랙을 재사용한다. 네 레슨은 동일한 requirement ID, endpoint, entity 이름을 공유해 산출물이 다음 단계와 test로 이어지는 traceability를 보여준다.

**Tech Stack:** Jekyll, Liquid, YAML, Markdown, OpenAPI 3.1, Given-When-Then, ADR

## Global Constraints

- 모든 새 Markdown 파일에는 YAML frontmatter를 포함한다.
- 일본어 원문의 AI 생성 sample은 그대로 번역하지 않고 검증된 완성 예제로 다시 작성한다.
- 전체 강의에서 `팀용 업무 요청 관리 API`와 `REQ-*`, `NFR-*`, `ADR-*` ID를 일관되게 사용한다.
- `Password VARCHAR(50)`과 plaintext password 저장 예시는 사용하지 않는다.
- normalization이 항상 query 성능을 높인다고 설명하지 않는다.
- Agile·DDD가 설계나 문서를 없앤다고 설명하지 않는다.
- availability·latency 수치는 business impact와 workload가 있는 quality scenario로 작성한다.
- 기존 강의와 사용자가 수정 중인 파일은 변경하지 않는다.

---

### Task 1: Software Development Process course 등록

**Files:**
- Modify: `_data/courses.yml`

**Interfaces:**
- Consumes: Docker plan의 `software-engineering` track
- Produces: `software-development-process` course와 레슨 4개 navigation metadata

- [ ] **Step 1: shared track 존재 확인**

Run:

```bash
ruby -e 'require "yaml"; d=YAML.load_file("_data/courses.yml"); abort unless d["tracks"].any?{|x|x["slug"]=="software-engineering"}; puts "SHARED TRACK OK"'
```

- [ ] **Step 2: course metadata 추가**

`num: "02"`, source Zenn URL, `source_label: "일본어 원문 아티클"`과 다음 레슨을 등록한다.

```yaml
- 01-understanding-the-development-process
- 02-writing-requirements
- 03-writing-system-design
- 04-modern-process-and-traceability
```

- [ ] **Step 3: YAML 검증 후 커밋**

```bash
ruby -e 'require "yaml"; d=YAML.load_file("_data/courses.yml"); c=d["courses"].find{|x|x["slug"]=="software-development-process"}; abort unless c && c["lessons"].size==4; puts "PROCESS COURSE DATA OK"'
git add _data/courses.yml
git commit -m "Add development process course metadata"
```

### Task 2: ELI15 개발 프로세스 레슨

**Files:**
- Create: `_lessons/software-development-process/01-understanding-the-development-process.md`

**Interfaces:**
- Consumes: Task 1 metadata
- Produces: 단계별 핵심 질문·입력·결정·출력·검증 주체 표

- [ ] **Step 1: frontmatter와 연속 예제 소개 작성**

`lesson: 1`, title `Understanding the Development Process`, title_ko `개발 프로세스를 쉽게 이해하기`를 사용한다. 식당 주문 비유 후 업무 요청 관리 API 예제로 전환한다.

- [ ] **Step 2: linear map과 feedback loop 작성**

문제→요구사항→설계→구현→검증→운영의 기본 흐름과, 운영·검증 결과가 앞 단계로 돌아가는 loop를 함께 그린다. 요구사항, 기본 설계, 상세 설계를 일본 SI heuristic과 국제적 일반 용어로 나눠 설명한다.

- [ ] **Step 3: 단계 table·오해·source 작성**

각 단계의 질문, 입력, 결정, 산출물, 검증자를 표로 만들고 `Agile이면 설계가 없다`, `승인 후 요구사항은 고정된다`, `문서가 많을수록 안전하다`를 오해로 설명한다. Zenn 원문과 SWEBOK·Agile Manifesto 링크를 둔다.

- [ ] **Step 4: 검증 후 커밋**

```bash
rg -n '문제.*요구사항.*설계.*구현.*검증.*운영|기본 설계|상세 설계|feedback' _lessons/software-development-process/01-understanding-the-development-process.md
git add _lessons/software-development-process/01-understanding-the-development-process.md
git commit -m "Add introductory development process lesson"
```

### Task 3: 요구사항 정의 실습 레슨

**Files:**
- Create: `_lessons/software-development-process/02-writing-requirements.md`

**Interfaces:**
- Consumes: Task 2의 업무 요청 관리 API 문제 정의
- Produces: REQ-001~004, NFR-001~003과 acceptance criteria가 있는 완성 requirement brief

- [ ] **Step 1: one-page requirement brief 작성**

문제, outcome, metric, scope, non-goals, stakeholder, assumption, risk를 실제 값으로 채운 template을 제공한다.

- [ ] **Step 2: 기능·quality requirement 작성**

요청 등록, 담당자 배정, 상태 변경, 목록 조회를 `REQ-001`~`REQ-004`로 정의한다. latency, audit, authentication을 workload·measurement·threshold가 포함된 `NFR-001`~`NFR-003` quality scenario로 쓴다.

- [ ] **Step 3: user story·business rule·acceptance criteria 작성**

`REQ-003` 상태 변경을 Given-When-Then 정상·권한 실패·잘못된 전이 사례로 완성한다. user story가 requirement 전체를 대체하지 않는 이유를 설명한다.

- [ ] **Step 4: validation·change 관리·source 작성**

review 질문, approval owner, version, change request, trace ID를 포함한다. ISO/IEC/IEEE 29148 공식 overview 또는 standards page, SWEBOK, OWASP authentication 관련 source를 연결한다.

- [ ] **Step 5: ID·내용 검증 후 커밋**

```bash
for id in REQ-001 REQ-002 REQ-003 REQ-004 NFR-001 NFR-002 NFR-003; do rg -q "$id" _lessons/software-development-process/02-writing-requirements.md || exit 1; done
rg -q 'Given.*When.*Then' _lessons/software-development-process/02-writing-requirements.md
git add _lessons/software-development-process/02-writing-requirements.md
git commit -m "Add requirements engineering practice lesson"
```

### Task 4: 기본·상세 설계 실습 레슨

**Files:**
- Create: `_lessons/software-development-process/03-writing-system-design.md`

**Interfaces:**
- Consumes: Task 3의 REQ·NFR ID와 업무 요청 domain
- Produces: component diagram, OpenAPI sample, ADR, module contract, traceability matrix

- [ ] **Step 1: context·component·trust boundary 작성**

client, API, PostgreSQL, identity provider, audit sink를 text diagram으로 만들고 각 component 책임과 data flow를 설명한다.

- [ ] **Step 2: OpenAPI 3.1과 data model 작성**

`POST /requests`, `PATCH /requests/{requestId}/status`, `GET /requests`의 핵심 request·response·error schema를 완성한다. `WorkRequest`, `StatusTransition`, `AuditEvent` entity와 password를 저장하지 않는 identity boundary를 설명한다.

- [ ] **Step 3: ADR과 상세 contract 작성**

`ADR-001` 상태 전이 검증 위치를 작성하고 대안·결과를 기록한다. module interface, transaction boundary, optimistic concurrency, idempotency, retry·error mapping, logging·test case를 정의한다.

- [ ] **Step 4: traceability matrix와 갱신 규칙 작성**

REQ/NFR→design component→endpoint/module→test ID를 매핑한다. code와 문서 불일치를 줄이는 review·versioning 규칙을 포함한다.

- [ ] **Step 5: source·구조 검증 후 커밋**

OpenAPI Specification, C4 model, ADR 원 저자 자료, OWASP Password Storage source를 연결한다.

```bash
rg -n 'openapi: 3.1|POST /requests|PATCH /requests/\{requestId\}/status|ADR-001|optimistic|traceability' _lessons/software-development-process/03-writing-system-design.md
git add _lessons/software-development-process/03-writing-system-design.md
git commit -m "Add system design practice lesson"
```

### Task 5: ELIPhD 현대적 개발 프로세스 레슨

**Files:**
- Create: `_lessons/software-development-process/04-modern-process-and-traceability.md`

**Interfaces:**
- Consumes: Tasks 2–4의 단계·artifact·trace ID
- Produces: process 선택·change impact·AI coding 검증 checklist

- [ ] **Step 1: process model 비교 작성**

phase-gate, waterfall, iterative, incremental, agile을 feedback latency, batch size, change cost, governance gate로 비교한다. 선형 단계와 실제 feedback loop를 분리한다.

- [ ] **Step 2: requirements engineering과 traceability 형식화**

elicitation, analysis, specification, validation, management를 설명하고 requirement→decision→component→test→operation signal graph와 change impact traversal을 정의한다.

- [ ] **Step 3: architecture·detailed design·DDD 경계 작성**

decision scope, reversibility, blast radius, coordination cost로 문서 수준을 선택한다. DDD의 model-code alignment와 Agile Manifesto의 문서 trade-off를 원문에 맞게 설명한다.

- [ ] **Step 4: 원문 위험 사례와 AI coding 보완 작성**

plaintext password, normalization=성능 향상, 임의 availability grade, 상세 설계 생략 일반화를 올바른 trade-off로 교체한다. AI coding에서 specification oracle, executable acceptance test, review·observability의 역할을 설명한다.

- [ ] **Step 5: source·checklist·검증 후 커밋**

SWEBOK, Agile Manifesto, OWASP Password Storage, OpenAPI, C4·ADR source를 관련 주장 옆에 둔다. decision risk, handoff, regulation, team topology, change frequency checklist를 제공한다.

```bash
ruby -e 'require "yaml"; s=File.read("_lessons/software-development-process/04-modern-process-and-traceability.md"); d=YAML.safe_load(s.split("---",3)[1]); abort unless d["lesson"]==4; %w[phase-gate iterative incremental traceability DDD OWASP].each{|x|abort unless s.include?(x)}; abort unless s.scan(%r{https://}).size>=7; puts "PROCESS LESSON 4 OK"'
git add _lessons/software-development-process/04-modern-process-and-traceability.md
git commit -m "Add advanced development process lesson"
```

### Task 6: Development Process course 통합 검증

**Files:**
- Verify: `_data/courses.yml`
- Verify: `_lessons/software-development-process/*.md`
- Modify: `docs/superpowers/plans/2026-08-02-development-process-course.md`

**Interfaces:**
- Consumes: Tasks 1–5와 완료된 Docker course
- Produces: Software Engineering track의 두 course·8 lessons와 완료된 plan

- [ ] **Step 1: course path·frontmatter·ID consistency 검사**

Ruby로 두 course의 8개 lesson path와 frontmatter를 대조한다. REQ·NFR·endpoint·entity 이름이 lesson 2·3·4에서 모순되지 않는지 `rg`와 diff review로 확인한다.

- [ ] **Step 2: 위험 pattern과 외부 링크 검사**

`Password VARCHAR`, `plaintext password`, `Agile.*문서.*필요 없다`, `normalization.*항상.*성능` 같은 금지 표현을 검사한다. 모든 HTTPS URL을 추출해 접근성을 확인한다.

- [ ] **Step 3: 전체 Jekyll build와 생성 경로 검사**

`bundle exec jekyll build` 후 Software Engineering track, Docker 4개, Development Process 4개 HTML 경로를 `test -f`로 확인한다.

- [ ] **Step 4: 계획 완료와 커밋**

이 문서의 `status`를 `completed`, 모든 checkbox를 `[x]`로 바꾸고 커밋한다.

```bash
git add docs/superpowers/plans/2026-08-02-development-process-course.md
git commit -m "Complete development process implementation plan"
```
