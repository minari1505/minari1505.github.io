---
title: "Docker Fundamentals & Production 구현 계획"
date: 2026-08-02
status: active
---

# Docker Fundamentals & Production Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** `강의 정리`에 브라우저에서 명령을 직접 실행하는 Docker 입문부터 프로덕션 원리까지 이어지는 4개 레슨을 추가한다.

**Architecture:** Jekyll lesson collection의 기존 YAML·Liquid 구조를 재사용한다. 새 `Software Engineering` 트랙과 course를 등록하고, 각 레슨은 개념·실습·심화를 한 책임으로 분리한다.

**Tech Stack:** Jekyll, Liquid, YAML, Markdown, Docker CLI, Docker Compose

## Global Constraints

- 모든 새 Markdown 파일에는 YAML frontmatter를 포함한다.
- 로컬 Docker 설치 없이 Play with Docker에서 실습을 시작할 수 있게 한다.
- 명령 block마다 목적, 예상 결과, 확인 또는 정확한 정리 단계를 제공한다.
- 광범위한 `docker system prune`, `docker volume prune`, `docker network prune`은 사용하지 않는다.
- legacy `docker-compose` 대신 `docker compose`를 사용한다.
- Kitematic과 Data Volume Container 패턴은 학습 경로에서 제외한다.
- 공식 Docker·OCI 문서를 우선하고 일본어 원문은 학습 순서의 출발점으로 사용한다.
- 기존 강의와 사용자가 수정 중인 파일은 변경하지 않는다.

---

### Task 1: Software Engineering 트랙과 Docker course 등록

**Files:**
- Modify: `_data/courses.yml`
- Create: `_pages/courses-software-engineering.md`

**Interfaces:**
- Consumes: `_layouts/lesson.html`의 `tracks`, `courses`, `lessons`, `source_label` 구조
- Produces: `/courses/software-engineering/`와 Docker 레슨 4개 navigation metadata

- [ ] **Step 1: 현재 course YAML baseline 확인**

Run:

```bash
ruby -e 'require "yaml"; YAML.load_file("_data/courses.yml"); puts "YAML OK"'
```

Expected: `YAML OK`

- [ ] **Step 2: 새 track과 course 추가**

`tracks`에 `slug: software-engineering`, `label: "INDEPENDENT STUDY"`, `url: /courses/software-engineering/`을 추가한다. `courses`에는 `slug: docker-fundamentals-and-production`, `track: software-engineering`, `num: "01"`, 원문 URL과 `source_label: "일본어 원문 교재"`를 넣고 다음 slug를 등록한다.

```yaml
- 01-understanding-docker
- 02-images-and-dockerfiles
- 03-storage-networking-and-compose
- 04-production-design
```

- [ ] **Step 3: track page 생성**

기존 `_pages/courses-agent-engineering.md`의 카드 구조를 재사용하고, 공식 문서와 실습 자료를 교차해 정리한 독립 학습 노트라고 설명한다.

- [ ] **Step 4: metadata 검증 후 커밋**

Run:

```bash
ruby -e 'require "yaml"; d=YAML.load_file("_data/courses.yml"); t=d["tracks"].find{|x|x["slug"]=="software-engineering"}; c=d["courses"].find{|x|x["slug"]=="docker-fundamentals-and-production"}; abort unless t && c && c["lessons"].size==4; puts "DOCKER COURSE DATA OK"'
git add _data/courses.yml _pages/courses-software-engineering.md
git commit -m "Add software engineering course track"
```

Expected: `DOCKER COURSE DATA OK` and two intended files committed.

### Task 2: ELI15 Docker와 첫 container 레슨

**Files:**
- Create: `_lessons/docker-fundamentals-and-production/01-understanding-docker.md`

**Interfaces:**
- Consumes: Task 1 course slug와 lesson 번호
- Produces: Play with Docker에서 nginx까지 실행·정리한 독자

- [ ] **Step 1: frontmatter와 mental model 작성**

`lesson: 1`, title `Understanding Docker`, title_ko `Docker를 쉽게 이해하고 첫 컨테이너 실행하기`를 사용한다. 이삿짐 포장 비유로 source code, image, container, registry를 설명한 뒤 host kernel을 공유하는 격리 process로 정확히 다시 정의한다.

- [ ] **Step 2: Play with Docker 실습 작성**

아래 흐름을 목적·예상 결과와 함께 제공한다.

```bash
docker version
docker run --rm hello-world
docker run -d --name se-nginx -p 8080:80 nginx:alpine
docker ps
docker logs se-nginx
curl http://localhost:8080
docker stop se-nginx
docker rm se-nginx
```

- [ ] **Step 3: troubleshooting과 source 추가**

Docker daemon 연결 실패, `port is already allocated`, container name 충돌을 다루고 Docker `What is a container?`, `Publishing ports`, Play with Docker 링크를 연결한다.

- [ ] **Step 4: 구조 검증 후 커밋**

Run:

```bash
ruby -e 'require "yaml"; s=File.read("_lessons/docker-fundamentals-and-production/01-understanding-docker.md"); d=YAML.safe_load(s.split("---",3)[1]); abort unless d["lesson"]==1; %w[docker\ run docker\ ps docker\ logs docker\ stop docker\ rm].each{|x| abort unless s.include?(x)}; puts "DOCKER LESSON 1 OK"'
git add _lessons/docker-fundamentals-and-production/01-understanding-docker.md
git commit -m "Add introductory Docker lesson"
```

### Task 3: Image와 Dockerfile 레슨

**Files:**
- Create: `_lessons/docker-fundamentals-and-production/02-images-and-dockerfiles.md`

**Interfaces:**
- Consumes: Task 2의 image/container 용어
- Produces: `se-web:1.0` image와 container를 직접 build한 독자

- [ ] **Step 1: layer·tag·registry 설명 작성**

image를 read-only layer와 metadata의 집합으로 설명하고 mutable tag와 immutable digest를 구분한다.

- [ ] **Step 2: 완성된 실습 파일 제공**

`index.html`, `.dockerignore`, 다음 Dockerfile을 모두 완성된 code block으로 제공한다.

```dockerfile
FROM python:3.13-slim
WORKDIR /app
COPY index.html .
USER 65532:65532
EXPOSE 8000
CMD ["python", "-m", "http.server", "8000"]
```

- [ ] **Step 3: build·cache·run·inspect 실습 작성**

```bash
docker build -t se-web:1.0 .
docker image ls se-web
docker history se-web:1.0
docker run -d --name se-web -p 8000:8000 se-web:1.0
curl http://localhost:8000
docker inspect se-web
docker stop se-web
docker rm se-web
docker image rm se-web:1.0
```

HTML만 변경해 재build하며 cache hit 차이를 확인한다.

- [ ] **Step 4: Dockerfile 의미·보안·source 보완 후 커밋**

`EXPOSE`가 publish를 수행하지 않음, `COPY`와 `ADD`, build context, non-root의 한계를 설명한다. Dockerfile·Build cache·`.dockerignore` 공식 링크를 포함한다.

Run:

```bash
rg -n 'FROM python:3.13-slim|USER 65532:65532|docker build|docker history|docker image rm' _lessons/docker-fundamentals-and-production/02-images-and-dockerfiles.md
git add _lessons/docker-fundamentals-and-production/02-images-and-dockerfiles.md
git commit -m "Add Docker image and Dockerfile lesson"
```

### Task 4: Storage·Network·Compose 레슨

**Files:**
- Create: `_lessons/docker-fundamentals-and-production/03-storage-networking-and-compose.md`

**Interfaces:**
- Consumes: Task 2·3의 container와 image 명령
- Produces: named volume, user-defined network, healthchecked Compose stack 실습

- [ ] **Step 1: named volume persistence 실습 작성**

`se-course-data` volume을 만들고 첫 alpine container가 `/data/message.txt`를 쓴 뒤 두 번째 container가 읽게 한다. bind mount와 named volume의 owner·portability 차이를 설명하고 정확한 `docker volume rm se-course-data`를 정리 단계에 둔다.

- [ ] **Step 2: network·DNS 실습 작성**

`se-course-net`, `se-net-web`를 만들고 `curlimages/curl` container가 `http://se-net-web`로 통신하게 한다. default bridge의 legacy link 대신 user-defined bridge DNS를 사용한다.

- [ ] **Step 3: compose.yaml과 healthcheck 작성**

`web`은 `nginx:alpine`, `checker`는 `curlimages/curl`을 사용한다. `web` healthcheck와 `depends_on: condition: service_healthy`를 포함하고 `docker compose config`, `up -d`, `ps`, `logs`, `run --rm checker`, `down` 순서를 제공한다.

- [ ] **Step 4: 상태·secret·오류 설명과 source 추가 후 커밋**

`depends_on`의 의미, environment variable과 secret 차이, Compose 단일 host 범위를 설명한다. Docker storage, network, Compose, startup order 문서를 연결한다.

Run:

```bash
rg -n 'docker volume create se-course-data|docker network create se-course-net|docker compose config|condition: service_healthy|docker compose down' _lessons/docker-fundamentals-and-production/03-storage-networking-and-compose.md
git add _lessons/docker-fundamentals-and-production/03-storage-networking-and-compose.md
git commit -m "Add Docker storage networking and Compose lesson"
```

### Task 5: ELIPhD 프로덕션 Docker 레슨

**Files:**
- Create: `_lessons/docker-fundamentals-and-production/04-production-design.md`

**Interfaces:**
- Consumes: Tasks 2–4의 CLI와 component mental model
- Produces: runtime·security·supply-chain·orchestration 판단 체크리스트

- [ ] **Step 1: runtime과 OCI 형식 모델 작성**

namespace, cgroup, mount, capability, OCI Image Spec·Runtime Spec, content-addressed layer와 copy-on-write를 container lifecycle과 연결한다.

- [ ] **Step 2: PID 1·signal·resource 실습 작성**

`docker stop`의 TERM→grace period→KILL 흐름, exec-form CMD, `--init`, memory·CPU limit을 설명한다. `docker stats`와 제한된 container의 inspect 명령을 제공한다.

- [ ] **Step 3: production Dockerfile과 supply chain 작성**

multi-stage build, BuildKit secret mount, non-root·rootless, read-only filesystem, tmpfs, capability drop, seccomp, SBOM·provenance·scan을 구분한다. secret을 `ARG`, `ENV`, `COPY`로 image layer에 넣지 말아야 하는 이유를 설명한다.

- [ ] **Step 4: orchestration 경계와 checklist 작성**

Compose, Swarm, Kubernetes, ECS를 feature catalog가 아닌 scheduling, desired state, health replacement, rolling update, secret·policy 요구로 비교한다. 적용 판단 checklist를 넣는다.

- [ ] **Step 5: 공식 source와 레슨 검증 후 커밋**

Docker security, rootless, multi-stage, build secrets, OCI specs URL을 관련 문단 가까이에 연결한다.

Run:

```bash
ruby -e 'require "yaml"; s=File.read("_lessons/docker-fundamentals-and-production/04-production-design.md"); d=YAML.safe_load(s.split("---",3)[1]); abort unless d["lesson"]==4; %w[namespace cgroup PID\ 1 BuildKit rootless OCI].each{|x| abort unless s.include?(x)}; abort unless s.scan(%r{https://}).size>=7; puts "DOCKER LESSON 4 OK"'
git add _lessons/docker-fundamentals-and-production/04-production-design.md
git commit -m "Add production Docker design lesson"
```

### Task 6: Docker course 통합 검증

**Files:**
- Verify: `_data/courses.yml`
- Verify: `_pages/courses-software-engineering.md`
- Verify: `_lessons/docker-fundamentals-and-production/*.md`
- Modify: `docs/superpowers/plans/2026-08-02-docker-course.md`

**Interfaces:**
- Consumes: Tasks 1–5
- Produces: build 가능한 Docker course와 완료된 plan

- [ ] **Step 1: slug·frontmatter·금지 패턴 검사**

Run Ruby로 course lesson path와 frontmatter 번호를 대조한다. `docker-compose`, `Kitematic`, `Data Volume Container`, 광범위한 `docker system prune`이 본문에 없는지 `rg`로 확인한다.

- [ ] **Step 2: 외부 링크와 Jekyll build 검사**

모든 lesson의 HTTPS URL을 추출해 HTTP 2xx·3xx 또는 명시적 403만 허용한다. `bundle exec jekyll build`를 실행하고 track·Docker lesson 4개 HTML 경로를 `test -f`로 확인한다.

- [ ] **Step 3: 계획 완료와 커밋**

이 문서의 `status`를 `completed`, 모든 checkbox를 `[x]`로 바꾸고 커밋한다.

```bash
git add docs/superpowers/plans/2026-08-02-docker-course.md
git commit -m "Complete Docker course implementation plan"
```
