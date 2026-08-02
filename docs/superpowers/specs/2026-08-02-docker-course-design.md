---
title: "Docker Fundamentals & Production 강의 설계"
date: 2026-08-02
status: approved
---

# Docker Fundamentals & Production 강의 설계

## 목적

Docker를 처음 접하는 개발자가 브라우저 실습 환경에서 컨테이너를 직접 실행하고, 이미지·Dockerfile·volume·network·Compose를 사용할 수 있게 한다. 마지막에는 Docker의 Linux·OCI 기반 동작 원리와 프로덕션 보안·운영 판단까지 설명할 수 있어야 한다.

## 대상 독자와 선수 지식

- 터미널 명령을 한 번 이상 사용한 개발 입문자
- Web API나 간단한 서버 프로그램의 개념을 아는 독자
- 로컬 Docker 설치 없이 Play with Docker에서 시작할 수 있게 구성한다.
- Docker Desktop 사용자는 같은 명령을 로컬 terminal에서 실행할 수 있음을 안내한다.

## 배치

- `강의 정리` 아래에 `Software Engineering` 독립 트랙을 새로 만든다.
- course slug는 `docker-fundamentals-and-production`을 사용한다.
- 원문 교재는 일본어 [입문 Docker](https://y-ohgi.com/introduction-docker/)로 표시하되, 명령과 설명은 현재 Docker 공식 문서 기준으로 보완한다.
- 같은 트랙에 별도 `Software Development Process` 강의를 함께 등록한다.

## 레슨 구성

### 01. Docker를 쉽게 이해하고 첫 컨테이너 실행하기

- ELI15 비유로 image, container, registry, host를 구분한다.
- container를 가벼운 VM이라고 단정하지 않고, host kernel을 공유하며 격리된 process라는 정확한 mental model로 연결한다.
- Play with Docker 접속, instance 생성, 제한 시간과 데이터 소멸 가능성을 설명한다.
- `docker version`, `docker run --rm hello-world`, nginx 실행, port publishing을 직접 수행한다.
- `docker ps`, `docker logs`, `docker stop`, `docker rm`의 예상 결과와 오류 해결법을 포함한다.

### 02. Image와 Dockerfile 직접 만들기

- image layer, tag, registry, pull과 build의 관계를 설명한다.
- 최소한의 정적 웹 또는 HTTP 애플리케이션을 만들고 Dockerfile로 build한다.
- `FROM`, `WORKDIR`, `COPY`, `RUN`, `USER`, `EXPOSE`, `CMD`의 build-time/run-time 차이를 설명한다.
- `.dockerignore`, layer cache, 변경 빈도가 낮은 파일을 먼저 복사하는 이유를 실습으로 확인한다.
- mutable tag의 위험과 digest pinning의 목적을 소개한다.
- legacy `docker build` 설명 대신 BuildKit 기반 현재 명령을 사용한다.

### 03. Volume·Network·Docker Compose 실습

- writable container layer가 임시 상태라는 점을 확인하고 named volume과 bind mount를 비교한다.
- legacy Data Volume Container 패턴은 사용하지 않는다.
- 사용자 정의 bridge network를 만들고 container name 기반 DNS 통신을 확인한다.
- `compose.yaml`에 application, database 또는 web service를 정의한다.
- `docker compose up`, `ps`, `logs`, `exec`, `down`을 직접 실행한다.
- `depends_on`은 준비 완료를 자동 보장하지 않음을 설명하고 healthcheck를 사용한다.
- 환경 변수와 secret을 구분하고 민감 정보를 image와 Git에 넣지 않는다.

### 04. ELIPhD: 프로덕션 Docker 설계

- Linux namespace, cgroup, mount, capability와 container runtime 격리를 설명한다.
- OCI Image Spec·Runtime Spec과 Docker Engine의 관계를 설명한다.
- content-addressable layer, overlay filesystem, copy-on-write와 build cache를 연결한다.
- PID 1, signal forwarding, zombie reaping, graceful shutdown을 다룬다.
- multi-stage build, non-root user, rootless mode, read-only filesystem, capability 축소, seccomp를 설명한다.
- BuildKit secret mount, SBOM, image signing·provenance, vulnerability scanning과 base image update 정책을 다룬다.
- Compose는 단일 host 중심의 orchestration 도구이며, multi-host scheduling·self-healing 요구에서는 Kubernetes·ECS 등과 비교해야 함을 설명한다.
- 성능, 보안, 재현성, 운영 복잡도의 trade-off를 판단하는 체크리스트를 제공한다.

## 실습 규칙

- 모든 명령 block 앞에 목적을 적고, 뒤에 예상 결과와 확인 명령을 둔다.
- 명령은 복사해서 순서대로 실행할 수 있어야 한다.
- container·network·volume 이름은 강의 전체에서 충돌하지 않는 고정 이름을 사용한다.
- 정리 명령은 해당 실습에서 만든 resource만 정확히 지정하며, 광범위한 `prune -a`는 사용하지 않는다.
- destructive한 데이터 삭제 명령은 데이터가 사라짐을 바로 위에서 경고한다.
- Play with Docker session이 끝나면 데이터가 사라질 수 있으므로 중요한 코드는 별도로 복사하도록 안내한다.
- 실패하기 쉬운 `port already allocated`, container name 충돌, platform mismatch, healthcheck 대기 문제를 troubleshooting box로 제공한다.

## 원문 보완 원칙

- Docker 26.1.3에 고정된 설명은 특정 버전 사실로 표시하거나 현재 공식 문서 표현으로 갱신한다.
- Kitematic처럼 종료된 도구와 legacy `docker-compose` 명령은 현재 학습 경로에서 제외한다.
- `1 container = 1 process`는 절대 법칙이 아니라 한 가지 concern과 명확한 lifecycle을 권장하는 원칙으로 설명한다.
- `EXPOSE`는 port를 실제 publish하지 않는 metadata임을 명시한다.
- image가 작다는 이유만으로 안전하다고 말하지 않고 provenance, update cadence, scanner 결과와 runtime hardening을 함께 본다.
- root가 아닌 user 실행만으로 모든 privilege 위험이 사라진다고 설명하지 않는다.

## 우선 출처

- Docker 공식 Get Started, Dockerfile, storage, network, Compose, BuildKit, security 문서
- OCI Image Specification과 Runtime Specification
- Play with Docker 공식 환경 안내
- 원문 교재 저장소 `y-ohgi/introduction-docker`

## 변경 파일

- `_data/courses.yml`: `Software Engineering` 트랙과 Docker course 등록
- `_pages/courses-software-engineering.md`: 독립 트랙 페이지 생성
- `_lessons/docker-fundamentals-and-production/01-understanding-docker.md`
- `_lessons/docker-fundamentals-and-production/02-images-and-dockerfiles.md`
- `_lessons/docker-fundamentals-and-production/03-storage-networking-and-compose.md`
- `_lessons/docker-fundamentals-and-production/04-production-design.md`

모든 새 Markdown 파일에는 YAML frontmatter를 포함한다. 기존 강의와 사용자가 수정 중인 파일은 변경하지 않는다.

## 검증 기준

- Jekyll build가 성공하고 트랙·course·레슨 4개 경로가 생성된다.
- `courses.yml`의 번호·slug와 실제 파일명이 일치한다.
- 모든 외부 링크가 접근 가능한 공식 또는 원문 URL이다.
- 모든 실습에 목적, 명령, 예상 결과, 확인 또는 정리 단계가 있다.
- 사용한 image tag와 Compose syntax가 현재 공식 문서와 호환된다.
- ELI15 설명이 전문 설명과 모순되지 않는다.
