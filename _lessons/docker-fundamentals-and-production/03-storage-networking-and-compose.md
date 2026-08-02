---
title: "Storage, Networking, and Compose"
title_ko: "Volume·Network·Docker Compose 실습"
course: docker-fundamentals-and-production
lesson: 3
tags:
  - Docker
  - Docker Compose
  - Container Networking
---

## 학습 목표

- container writable layer와 persistent data를 분리하기
- named volume과 bind mount의 차이를 실습으로 확인하기
- user-defined bridge network에서 container name으로 통신하기
- healthcheck와 dependency가 있는 Compose application 실행하기

## Container 안에 저장하면 왜 사라질까?

Container의 writable layer는 container lifecycle에 묶여 있습니다.

```text
Image layers: application과 기본 파일 — read-only
Container layer: 실행 중 변경한 파일 — container 제거 시 함께 제거
Volume: application lifecycle과 분리한 data — 별도 제거 전까지 유지
```

Application을 새 image로 교체해도 database·upload data는 남아야 합니다. 그래서 state를 container layer에 두지 않고 volume이나 외부 storage로 분리합니다.

## Named volume과 Bind mount

| 구분 | Named volume | Bind mount |
|---|---|---|
| 경로 관리 | Docker가 관리 | 사용자가 host path 지정 |
| 이동성 | Docker 명령으로 다루기 쉬움 | Host directory 구조에 의존 |
| 주 사용처 | database data, application state | local source code, 설정 파일 |
| 권한 문제 | 여전히 UID·GID 확인 필요 | Host OS 권한·Docker Desktop 공유 설정 영향 |
| Backup | volume data를 명시적으로 export | 일반 host file backup 사용 가능 |

과거의 Data Volume Container는 다른 container를 volume holder로 사용하던 패턴입니다. 현재 입문 경로에서는 named volume을 사용합니다.

## 실습 1: Named volume으로 Data 보존하기

### 목적

첫 container가 쓴 file을 완전히 다른 container가 읽도록 합니다.

Volume을 만듭니다.

```bash
docker volume create se-course-data
```

확인합니다.

```bash
docker volume inspect se-course-data
```

첫 container가 file을 씁니다. Container는 `--rm`으로 바로 사라집니다.

```bash
docker run --rm \
  -v se-course-data:/data \
  alpine \
  sh -c 'echo "volume은 container보다 오래 산다" > /data/message.txt'
```

두 번째 container가 같은 volume을 read-only로 mount해 읽습니다.

```bash
docker run --rm \
  -v se-course-data:/data:ro \
  alpine \
  cat /data/message.txt
```

### 예상 결과

```text
volume은 container보다 오래 산다
```

첫 container가 없어도 data가 남았습니다.

### 정확한 정리

이번 실습 volume만 제거합니다. 이 명령은 volume 안의 file을 되돌릴 수 없게 삭제합니다.

```bash
docker volume rm se-course-data
```

확인합니다.

```bash
docker volume ls --filter name=se-course-data
```

## 실습 2: Bind mount로 Local file 공유하기

### 목적

Host에서 편집한 HTML을 nginx가 즉시 읽게 합니다.

```bash
mkdir -p bind-lab/site
cat > bind-lab/site/index.html <<'EOF'
<!doctype html>
<html lang="ko"><body><h1>Bind mount 실습</h1></body></html>
EOF
```

절대 경로를 nginx document root에 read-only로 mount합니다.

```bash
docker run -d \
  --name se-bind-web \
  -p 127.0.0.1:8080:80 \
  -v "$(pwd)/bind-lab/site:/usr/share/nginx/html:ro" \
  nginx:alpine
```

```bash
curl http://localhost:8080
```

`Bind mount 실습`이 보입니다. Host file을 수정한 뒤 다시 요청하면 image를 rebuild하지 않아도 변경이 반영됩니다.

### 정리

```bash
docker stop se-bind-web
docker rm se-bind-web
```

`bind-lab/site/index.html`은 host file이므로 container를 제거해도 남습니다.

## Container Network mental model

Container는 기본적으로 자기 network namespace와 IP를 가집니다. User-defined bridge network에 참여한 container는 Docker의 embedded DNS를 통해 container 이름으로 서로를 찾을 수 있습니다.

```text
Host
└─ se-course-net
   ├─ se-net-web :80
   └─ 일회성 curl container → http://se-net-web
```

Host에서 접근해야 할 때만 `-p`로 publish합니다. Container끼리 같은 network에서 통신하는 데 host port는 필요하지 않습니다.

## 실습 3: 이름으로 Container 찾기

Network를 만듭니다.

```bash
docker network create se-course-net
```

Nginx를 network에 참여시킵니다. Host port는 publish하지 않습니다.

```bash
docker run -d \
  --name se-net-web \
  --network se-course-net \
  nginx:alpine
```

같은 network의 일회성 curl container에서 이름으로 요청합니다.

```bash
docker run --rm \
  --network se-course-net \
  curlimages/curl:8.12.1 \
  -sS http://se-net-web
```

### 예상 결과

Nginx welcome HTML이 출력됩니다. `se-net-web`이라는 이름을 Docker DNS가 container IP로 해석했습니다.

Network 참여 상태를 확인합니다.

```bash
docker network inspect se-course-net
```

### 정리

Network를 제거하려면 연결된 container를 먼저 정리합니다.

```bash
docker stop se-net-web
docker rm se-net-web
docker network rm se-course-net
```

## Docker Compose가 해결하는 문제

Container가 여러 개가 되면 긴 `docker run` 명령을 반복하기 어렵습니다. Compose는 service, network, volume, healthcheck를 `compose.yaml`에 선언하고 하나의 application처럼 다룹니다.

Compose는 기본적으로 현재 project의 service를 한 Docker Engine에서 조정합니다. Multi-host scheduler와 cluster self-healing이 필요한 환경은 별도 orchestrator를 검토합니다.

## 실습 4: Healthcheck가 있는 Compose Application

### 1. 파일 준비

```bash
mkdir -p compose-lab/site
cd compose-lab
```

```bash
cat > site/index.html <<'EOF'
<!doctype html>
<html lang="ko"><body><h1>Compose application is healthy</h1></body></html>
EOF
```

### 2. `compose.yaml` 작성

```bash
cat > compose.yaml <<'EOF'
services:
  web:
    image: nginx:alpine
    ports:
      - "127.0.0.1:8080:80"
    volumes:
      - ./site:/usr/share/nginx/html:ro
    healthcheck:
      test: ["CMD-SHELL", "wget -qO- http://localhost/ >/dev/null 2>&1 || exit 1"]
      interval: 5s
      timeout: 2s
      retries: 5

  checker:
    image: curlimages/curl:8.12.1
    depends_on:
      web:
        condition: service_healthy
    command: ["-sS", "http://web"]
EOF
```

Top-level `version` field은 현재 Compose Specification에서 필요하지 않습니다.

### 3. 설정 해석 결과 확인

```bash
docker compose config
```

환경 변수 치환과 default가 적용된 최종 설정이 출력됩니다. YAML parse 오류도 실제 실행 전에 발견할 수 있습니다.

### 4. 실행

```bash
docker compose up -d
```

상태를 확인합니다.

```bash
docker compose ps --all
```

`web`은 `healthy`, `checker`는 한 번 요청한 뒤 `Exited (0)`가 될 수 있습니다.

```bash
docker compose logs checker
curl http://localhost:8080
```

두 결과에 `Compose application is healthy`가 포함되면 성공입니다.

Checker를 필요할 때 다시 실행할 수도 있습니다.

```bash
docker compose run --rm checker
```

### 5. 정리

현재 `compose.yaml`이 만든 container와 default network만 내립니다.

```bash
docker compose down
```

Bind-mounted `site/index.html`은 host에 남습니다. Compose에 named volume이 있고 data까지 삭제하려면 `down -v`가 필요하지만, data가 사라지는 동작이므로 목적 없이 사용하지 않습니다.

## `depends_on`이 보장하는 것

단순한 `depends_on`은 service 시작 순서를 표현하지만 application이 요청을 받을 준비까지 자동으로 보장하지 않습니다. 위 예시는 `web`에 healthcheck를 정의하고 `condition: service_healthy`를 사용했습니다.

그래도 다음을 고려해야 합니다.

- Application이 실행 중에 다시 unhealthy가 될 수 있음
- Dependency가 일시적으로 끊길 수 있음
- Client에는 timeout·retry·backoff가 필요할 수 있음
- Healthcheck가 실제 readiness를 잘 측정해야 함

## Environment variable과 Secret

Compose의 `environment`와 `.env`는 설정 전달에 편리하지만 자동 암호화 저장소가 아닙니다. Password, token, private key를 image `ENV`, Dockerfile `ARG`, source repository에 넣지 않습니다.

Local demo에서는 별도 sample 값을 사용하고, production에서는 platform secret store, Compose secret 또는 BuildKit secret을 목적에 맞게 사용합니다. Secret이 process environment에 꼭 필요한지, file mount로 전달할 수 있는지도 검토합니다.

## 자주 만나는 오류

### volume is in use

어떤 container가 volume을 mount하고 있는지 확인합니다.

```bash
docker ps -a --filter volume=se-course-data
```

정체를 확인하지 않은 채 container나 volume을 강제 삭제하지 않습니다.

### network has active endpoints

연결된 container를 확인합니다.

```bash
docker network inspect se-course-net
```

해당 실습 container를 stop·remove한 뒤 network를 제거합니다.

### Compose service가 계속 starting

{% raw %}
```bash
docker compose ps
docker compose logs web
docker inspect --format '{{json .State.Health}}' compose-lab-web-1
```
{% endraw %}

Compose가 만든 실제 container 이름은 project 이름에 따라 다를 수 있으므로 먼저 `docker compose ps`에서 확인합니다.

## 핵심 정리

1. Container writable layer는 임시 상태이며 persistent data는 volume이나 외부 storage로 분리합니다.
2. Named volume은 Docker가 경로를 관리하고 bind mount는 host path를 직접 공유합니다.
3. User-defined bridge network에서는 container name 기반 DNS를 사용할 수 있습니다.
4. Compose는 여러 container 설정을 선언적으로 묶지만 service readiness와 runtime 장애 복구까지 자동 보장하지 않습니다.
5. Data와 secret은 lifecycle과 위험도가 다르므로 각각 별도의 저장·전달 정책이 필요합니다.

## 참고 자료

- [Docker Docs — Storage](https://docs.docker.com/engine/storage/)
- [Docker Docs — Volumes](https://docs.docker.com/engine/storage/volumes/)
- [Docker Docs — Networking](https://docs.docker.com/engine/network/)
- [Docker Compose](https://docs.docker.com/compose/)
- [Control startup order](https://docs.docker.com/compose/how-tos/startup-order/)
- [Compose secrets](https://docs.docker.com/compose/how-tos/use-secrets/)
