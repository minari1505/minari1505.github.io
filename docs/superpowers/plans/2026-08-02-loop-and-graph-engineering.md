---
title: "Loop and Graph Engineering 구현 계획"
date: 2026-08-02
status: active
---

# Loop and Graph Engineering Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** `강의 정리`에 ELI15 설명, Claude Code·Codex 실전 활용, ELIPhD 분석으로 이어지는 Loop and Graph Engineering 강의 3편을 추가한다.

**Architecture:** 기존 Jekyll lesson collection 구조를 그대로 사용한다. `Agent Engineering` 트랙과 강의 메타데이터를 `_data/courses.yml`에 등록하고, 독립 트랙 페이지 1개와 책임이 분리된 레슨 파일 3개를 만든다.

**Tech Stack:** Jekyll, Liquid, YAML, Markdown, Minimal Mistakes

## Global Constraints

- 모든 새 Markdown 파일에는 YAML frontmatter를 포함한다.
- 기존 Agent Skills 레슨과 사용자가 수정 중인 파일은 변경하지 않는다.
- `04-Archive`는 읽거나 수정하지 않는다.
- 별도의 팩트체크 레슨은 만들지 않는다.
- 검증된 내용과 필요한 한계는 해당 개념 옆에 자연스럽게 배치한다.
- 정량 수치는 출처와 적용 조건이 분명할 때만 사용한다.
- 설명 순서는 ELI15 → 실전 활용 → ELIPhD로 구성한다.

---

### Task 1: Agent Engineering 트랙과 강의 경로 등록

**Files:**
- Modify: `_data/courses.yml`
- Create: `_pages/courses-agent-engineering.md`

**Interfaces:**
- Consumes: `_layouts/lesson.html`이 사용하는 `tracks`, `courses`, `lessons` YAML 구조
- Produces: `/courses/agent-engineering/` 트랙 페이지와 `/courses/loop-and-graph-engineering/<lesson>/` 경로

- [ ] **Step 1: 현재 YAML이 파싱되는지 확인**

Run:

```bash
ruby -e 'require "yaml"; YAML.load_file("_data/courses.yml", aliases: true); puts "YAML OK"'
```

Expected: `YAML OK`

- [ ] **Step 2: 트랙과 강의 메타데이터 추가**

`_data/courses.yml`에 다음 구조를 추가한다.

```yaml
  - slug: agent-engineering
    label: "INDEPENDENT STUDY"
    title: "Agent Engineering"
    description: "AI 에이전트의 반복 실행과 작업 그래프를 이해하고 실무 설계에 적용하는 학습 노트입니다."
    url: /courses/agent-engineering/
```

강의 목록에는 `loop-and-graph-engineering` 강의와 다음 레슨 3개를 등록한다.

```yaml
      - num: "01"
        slug: understanding-loops-and-graphs
        title: "Understanding Loops and Graphs"
        ko: "Loop와 Graph를 쉽게 이해하기"
      - num: "02"
        slug: using-loops-and-graphs
        title: "Using Loops and Graphs"
        ko: "Claude Code·Codex에서 사용하는 방법"
      - num: "03"
        slug: formal-models-and-design-principles
        title: "Formal Models and Design Principles"
        ko: "형식적 모델과 설계 원칙"
```

- [ ] **Step 3: 독립 트랙 페이지 작성**

`_pages/courses-agent-engineering.md`에 frontmatter를 넣고, 기존 `courses-anthropic.md`의 카드 구조를 재사용한다. 설명은 특정 업체의 공식 강의가 아니라 공식 문서·논문·사례를 종합한 학습 노트임을 밝힌다.

- [ ] **Step 4: YAML과 트랙 페이지를 검증**

Run:

```bash
ruby -e 'require "yaml"; d=YAML.load_file("_data/courses.yml", aliases: true); abort unless d["tracks"].any? { |t| t["slug"] == "agent-engineering" }; abort unless d["courses"].any? { |c| c["slug"] == "loop-and-graph-engineering" && c["lessons"].size == 3 }; puts "COURSE DATA OK"'
```

Expected: `COURSE DATA OK`

- [ ] **Step 5: 변경 파일만 커밋**

```bash
git add _data/courses.yml _pages/courses-agent-engineering.md
git commit -m "Add agent engineering course track"
```

### Task 2: ELI15 개념 레슨 작성

**Files:**
- Create: `_lessons/loop-and-graph-engineering/01-understanding-loops-and-graphs.md`

**Interfaces:**
- Consumes: Task 1의 course slug와 lesson 번호·slug
- Produces: loop와 graph의 관계를 비전문가도 구분할 수 있는 첫 레슨

- [ ] **Step 1: frontmatter와 학습 목표 작성**

다음 값을 정확히 사용한다.

```yaml
---
title: "Understanding Loops and Graphs"
title_ko: "Loop와 Graph를 쉽게 이해하기"
course: loop-and-graph-engineering
lesson: 1
tags:
  - Agent Engineering
  - Loop Engineering
  - Graph Engineering
---
```

- [ ] **Step 2: 쉬운 비유와 텍스트 도식 작성**

한 작업자가 글을 고쳐 제출하는 loop와, 팀장이 조사·검토·통합 작업을 배치하는 graph를 예로 든다. 다음 관계를 명시한다.

```text
Loop: 목표 → 실행 → 확인 → 수정 ─┐
          ↑                    │
          └────────────────────┘

Graph: 시작 → 작업 분배 ┬→ 조사 ─┐
                       ├→ 검토 ─┼→ 통합 → 종료
                       └→ 테스트 ┘
```

- [ ] **Step 3: 핵심 오해와 선택 기준 작성**

`graph가 loop를 대체한다`, `멀티 에이전트면 항상 빠르다`, `그래프는 반드시 여러 에이전트다`를 오해로 설명한다. 마지막에 단일 호출·loop·graph 비교표와 세 문장 요약을 둔다.

- [ ] **Step 4: 레슨 구조 검증**

Run:

```bash
ruby -e 'require "yaml"; s=File.read("_lessons/loop-and-graph-engineering/01-understanding-loops-and-graphs.md"); fm=s.split("---",3)[1]; d=YAML.safe_load(fm); abort unless d["course"]=="loop-and-graph-engineering" && d["lesson"]==1; %w[Loop Graph].each { |x| abort unless s.include?(x) }; puts "LESSON 1 OK"'
```

Expected: `LESSON 1 OK`

- [ ] **Step 5: 레슨만 커밋**

```bash
git add _lessons/loop-and-graph-engineering/01-understanding-loops-and-graphs.md
git commit -m "Add introductory loop and graph lesson"
```

### Task 3: Claude Code·Codex 실전 레슨 작성

**Files:**
- Create: `_lessons/loop-and-graph-engineering/02-using-loops-and-graphs.md`

**Interfaces:**
- Consumes: Task 2의 쉬운 용어와 단일 호출 → loop → graph 복잡도 단계
- Produces: 작업 구조 선택표, loop·graph 설계 절차, 실전 프롬프트 예시

- [ ] **Step 1: frontmatter와 학습 목표 작성**

`course: loop-and-graph-engineering`, `lesson: 2`를 사용하고 Task 2와 동일한 태그 3개를 넣는다.

- [ ] **Step 2: loop 실전 구성 작성**

`목표 → 상태 → 행동 → 검증 → 종료 판단`을 설명한다. 테스트 실패 수정 예시에는 최대 반복 횟수, 시간·token 예산, 같은 오류의 반복 감지, 사람이 확인해야 하는 조건을 포함한다.

- [ ] **Step 3: graph 실전 구성 작성**

node, edge, router, reducer, checkpoint, contract를 쉬운 말로 정의한다. 문서 조사·비교 작업을 fan-out/fan-in 예시로 들고, 공유 파일 충돌과 누락된 결과를 예방하는 규칙을 포함한다.

- [ ] **Step 4: Claude Code·Codex용 자연어 요청 예시 작성**

모델 전용 마법 키워드 대신 다음 정보를 명시하는 예시를 제공한다.

```text
1. 독립적으로 조사할 작업을 먼저 구분한다.
2. 서로의 결과가 필요 없는 작업만 병렬로 진행한다.
3. 각 결과는 같은 형식으로 저장한다.
4. 모든 결과가 모였는지 확인한 뒤 통합한다.
5. 테스트와 출처 검증이 실패하면 최대 2회만 수정한다.
```

- [ ] **Step 5: 출처와 레슨 구조 검증 후 커밋**

Anthropic의 `Building effective agents`, Claude Code workflows, LangGraph Graph API 공식 링크를 관련 문단에 배치한다.

Run:

```bash
ruby -e 'require "yaml"; s=File.read("_lessons/loop-and-graph-engineering/02-using-loops-and-graphs.md"); d=YAML.safe_load(s.split("---",3)[1]); abort unless d["lesson"]==2; %w[router reducer checkpoint contract].each { |x| abort unless s.downcase.include?(x) }; abort unless s.scan(%r{https://}).size >= 3; puts "LESSON 2 OK"'
git add _lessons/loop-and-graph-engineering/02-using-loops-and-graphs.md
git commit -m "Add practical loop and graph lesson"
```

Expected: `LESSON 2 OK` and a commit containing only lesson 2.

### Task 4: ELIPhD 전문 레슨 작성

**Files:**
- Create: `_lessons/loop-and-graph-engineering/03-formal-models-and-design-principles.md`

**Interfaces:**
- Consumes: Task 2·3의 개념과 실전 용어
- Produces: 형식적 모델, 실패 조건, 비용 모델, 설계 체크리스트를 담은 심화 레슨

- [ ] **Step 1: frontmatter와 전문 학습 목표 작성**

`course: loop-and-graph-engineering`, `lesson: 3`을 사용하고 `ELIPhD`라는 학습 수준을 서문에서 설명한다.

- [ ] **Step 2: loop의 형식적 모델 작성**

상태 `s_t`, 전이 함수 `T`, 관측 `o_t`, 정책 `π`, 검증 함수 `V`, 종료 술어 `τ`를 정의한다. 수렴은 보장되지 않으므로 반복 상한, no-progress 판정, 외부 검증 기준이 필요함을 설명한다.

- [ ] **Step 3: graph의 형식적 모델 작성**

`G=(V,E)`와 node contract를 정의하고 chain, DAG, cyclic graph를 비교한다. 위상 정렬, 임계 경로, fan-in 병목, 라우터의 reject 경로, 부분 실패, checkpoint·resume, 멱등성을 다룬다.

- [ ] **Step 4: 성능과 품질의 한계 작성**

멀티 에이전트가 추가 token을 사용하는 점과, 동일한 추론 예산에서는 단일 에이전트가 대등하거나 우세할 수 있다는 연구를 함께 설명한다. topology만으로 품질 향상을 주장하지 않고 `품질 이득 - 조정 비용 - 추가 계산 비용` 관점으로 평가한다.

- [ ] **Step 5: 공식 문서·논문과 설계 체크리스트 추가**

다음 출처를 관련 주장 가까이에 연결한다.

- `https://www.anthropic.com/engineering/building-effective-agents`
- `https://www.anthropic.com/engineering/multi-agent-research-system`
- `https://arxiv.org/abs/2604.11378`
- `https://arxiv.org/abs/2604.02460`
- `https://arxiv.org/abs/2503.13657`
- `https://microsoft.github.io/autogen/stable/user-guide/agentchat-user-guide/graph-flow.html`
- `https://adk.dev/agents/workflow-agents/`

마지막 체크리스트는 실제 의존성, 병렬 안전성, state owner, node contract, 종료 조건, 예산, 검증 기준, 재시도·멱등성, 관찰 가능성 항목을 포함한다.

- [ ] **Step 6: 레슨 검증 후 커밋**

Run:

```bash
ruby -e 'require "yaml"; s=File.read("_lessons/loop-and-graph-engineering/03-formal-models-and-design-principles.md"); d=YAML.safe_load(s.split("---",3)[1]); abort unless d["lesson"]==3; %w[G=(V,E) 멱등성 종료].each { |x| abort unless s.include?(x) }; abort unless s.scan(%r{https://}).size >= 7; puts "LESSON 3 OK"'
git add _lessons/loop-and-graph-engineering/03-formal-models-and-design-principles.md
git commit -m "Add advanced loop and graph lesson"
```

Expected: `LESSON 3 OK` and a commit containing only lesson 3.

### Task 5: 전체 탐색·빌드·내용 품질 검증

**Files:**
- Verify: `_data/courses.yml`
- Verify: `_pages/courses-agent-engineering.md`
- Verify: `_lessons/loop-and-graph-engineering/*.md`

**Interfaces:**
- Consumes: Tasks 1–4의 완성된 트랙과 레슨
- Produces: 빌드 가능하고 링크 구조가 일치하는 강의 묶음

- [ ] **Step 1: 변경 범위와 frontmatter 확인**

Run:

```bash
git status --short
for f in _pages/courses-agent-engineering.md _lessons/loop-and-graph-engineering/*.md; do sed -n '1,12p' "$f"; done
```

Expected: 기존 사용자 변경 외에 계획된 파일만 표시되고, 모든 파일이 `---` frontmatter로 시작한다.

- [ ] **Step 2: course 데이터와 파일 slug 대조**

Run:

```bash
ruby -e 'require "yaml"; d=YAML.load_file("_data/courses.yml", aliases: true); c=d["courses"].find { |x| x["slug"]=="loop-and-graph-engineering" }; abort unless c; c["lessons"].each { |l| p="_lessons/#{c["slug"]}/#{l["num"]}-#{l["slug"]}.md"; abort "missing #{p}" unless File.exist?(p) }; puts "LESSON PATHS OK"'
```

Expected: `LESSON PATHS OK`

- [ ] **Step 3: 금지된 placeholder와 링크 형식 확인**

Run:

```bash
rg -n "TBD|TODO|나중에 작성|출처 필요" _pages/courses-agent-engineering.md _lessons/loop-and-graph-engineering || true
rg -n 'http://' _pages/courses-agent-engineering.md _lessons/loop-and-graph-engineering || true
```

Expected: 출력 없음.

- [ ] **Step 4: Jekyll 전체 빌드**

Run:

```bash
bundle exec jekyll build
```

Expected: exit code 0, 새 트랙과 세 레슨이 `_site/courses/` 아래 생성됨.

- [ ] **Step 5: 생성 경로 확인**

Run:

```bash
test -f _site/courses/agent-engineering/index.html
test -f _site/courses/loop-and-graph-engineering/01-understanding-loops-and-graphs/index.html
test -f _site/courses/loop-and-graph-engineering/02-using-loops-and-graphs/index.html
test -f _site/courses/loop-and-graph-engineering/03-formal-models-and-design-principles/index.html
```

Expected: exit code 0.

- [ ] **Step 6: 계획 상태를 완료로 갱신하고 커밋**

이 문서의 `status`를 `completed`로 바꾸고 실행한 checkbox를 모두 `[x]`로 변경한다.

```bash
git add docs/superpowers/plans/2026-08-02-loop-and-graph-engineering.md
git commit -m "Complete loop and graph engineering implementation plan"
```
