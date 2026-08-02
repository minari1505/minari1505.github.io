---
title: "Understanding Docker"
title_ko: "Docker를 쉽게 이해하고 첫 컨테이너 실행하기"
course: docker-fundamentals-and-production
lesson: 1
tags:
  - Docker
  - Container
  - Software Engineering
---

## 학습 목표

- image, container, registry, host를 쉬운 말로 구분하기
- container와 virtual machine의 차이를 정확히 이해하기
- Play with Docker에서 첫 container를 실행하고 상태·log를 확인하기
- port publishing과 container 정리 방법 익히기

## ELI15: 같은 이삿짐 상자로 어디서든 시작하기

새 컴퓨터에서 애플리케이션을 실행할 때 이런 일이 자주 생깁니다.

> 내 컴퓨터에서는 되는데요?

프로그램만 복사해서는 충분하지 않기 때문입니다. 실행하려면 특정 language runtime, library, 설정 파일과 filesystem 구조도 필요합니다.

Docker는 이 실행 환경을 **image**라는 표준화된 꾸러미로 만듭니다. 이삿짐에 비유하면 다음과 같습니다.

| Docker 용어 | 이삿짐 비유 | 실제 의미 |
|---|---|---|
| Dockerfile | 포장 설명서 | image를 만드는 명령 모음 |
| Image | 봉인된 이삿짐 상자 | 실행 파일·library·metadata가 담긴 read-only template |
| Container | 상자를 풀어 사용하는 방 | image에서 시작된 격리 process와 writable 상태 |
| Registry | 상자 보관 창고 | image를 저장하고 배포하는 서비스 |
| Host | 건물 | container를 실행하는 OS와 machine |

같은 image를 사용하면 개발자 laptop, CI, server에서 비슷한 filesystem과 실행 명령으로 시작할 수 있습니다.

## Container는 작은 VM인가?

처음에는 “가벼운 virtual machine”이라고 생각해도 모양을 이해하는 데 도움이 됩니다. 하지만 내부 동작은 다릅니다.

```text
Virtual machine
Host hardware
└─ Hypervisor
   ├─ Guest OS ─ App A
   └─ Guest OS ─ App B

Container
Host hardware
└─ Host kernel
   ├─ 격리된 process ─ App A
   └─ 격리된 process ─ App B
```

VM은 일반적으로 각 guest가 자기 kernel을 가집니다. Linux container는 host kernel을 공유하면서 process, network, filesystem view와 resource를 격리합니다. 그래서 보통 시작이 빠르고 overhead가 작지만, VM과 같은 보안 경계라고 단정하면 안 됩니다.

## 실습 환경 준비

원문 교재는 [Play with Docker](https://labs.play-with-docker.com/)를 사용합니다. 브라우저에서 일시적인 Linux machine과 Docker Engine을 제공하므로 로컬 설치 없이 실습할 수 있습니다.

1. Docker Hub 계정으로 Play with Docker에 로그인합니다.
2. `Start`를 누릅니다.
3. `ADD NEW INSTANCE`를 눌러 terminal을 엽니다.

> Play with Docker instance는 시간이 지나면 종료되며 파일과 container가 사라질 수 있습니다. 학습용 임시 환경으로 사용하고 중요한 코드는 별도로 보관하세요.

로컬 Docker Desktop을 사용한다면 terminal에서 같은 명령을 실행하면 됩니다.

## 실습 1: Docker가 동작하는지 확인하기

### 목적

CLI가 Docker Engine과 통신할 수 있는지 확인합니다.

```bash
docker version
```

### 예상 결과

`Client`와 `Server` 정보가 함께 나옵니다. Server가 보이지 않고 daemon 연결 오류가 나오면 Docker Engine이 실행 중인지 확인해야 합니다.

간단한 진단 container를 실행합니다.

```bash
docker run --rm hello-world
```

### 무슨 일이 일어났나?

1. CLI가 Docker Engine에 실행을 요청합니다.
2. local에 `hello-world` image가 없으면 registry에서 가져옵니다.
3. image로 container를 만들고 process를 실행합니다.
4. process가 안내문을 출력하고 종료합니다.
5. `--rm` 옵션이 종료된 container를 자동으로 제거합니다.

`Hello from Docker!`가 보이면 정상입니다.

## 실습 2: nginx web server 실행하기

### 목적

background container를 실행하고 host의 8080 port를 container의 80 port에 연결합니다.

```bash
docker run -d --name se-nginx -p 8080:80 nginx:alpine
```

옵션을 하나씩 읽어보겠습니다.

| 옵션 | 의미 |
|---|---|
| `-d` | background(detached)에서 실행 |
| `--name se-nginx` | 사람이 읽을 수 있는 container 이름 지정 |
| `-p 8080:80` | host 8080 → container 80으로 publish |
| `nginx:alpine` | 사용할 image와 tag |

`nginx:alpine` 같은 tag는 시간이 지나며 가리키는 image가 바뀔 수 있습니다. 첫 실습에서는 간단함을 위해 사용하고, 프로덕션에서는 version과 digest 정책을 별도로 정합니다.

### 실행 상태 확인

```bash
docker ps
```

예상 결과에는 `se-nginx`, `Up ...`, `0.0.0.0:8080->80/tcp`가 포함됩니다.

HTTP 응답을 확인합니다.

```bash
curl http://localhost:8080
```

`Welcome to nginx!`가 포함된 HTML이 나오면 성공입니다. Play with Docker에서는 terminal 위의 `8080` 또는 `OPEN PORT` 기능으로 browser 화면을 열 수도 있습니다.

## 실습 3: Log와 상세 상태 보기

nginx가 받은 요청을 확인합니다.

```bash
docker logs se-nginx
```

앞에서 실행한 `curl` 요청의 access log가 보입니다.

Container의 설정을 JSON으로 확인합니다.

```bash
docker inspect se-nginx
```

출력이 길다면 필요한 값만 고를 수 있습니다.

{% raw %}
```bash
docker inspect --format '{{.State.Status}}' se-nginx
docker inspect --format '{{json .NetworkSettings.Ports}}' se-nginx
```
{% endraw %}

예상 결과는 각각 `running`, port mapping JSON입니다.

## 실습 정리

먼저 정상 종료를 요청합니다.

```bash
docker stop se-nginx
```

실행 중인 container 목록에서는 사라지지만 종료된 container는 남아 있습니다.

```bash
docker ps -a
```

이번 실습에서 만든 container만 제거합니다.

```bash
docker rm se-nginx
```

확인합니다.

```bash
docker ps -a --filter name=se-nginx
```

표의 header만 보이면 정리가 끝났습니다. `nginx:alpine` image는 다음 실습에서도 사용할 수 있으므로 남겨둡니다.

## 자주 만나는 오류

### Cannot connect to the Docker daemon

Docker Engine이 실행되지 않았거나 현재 사용자가 daemon socket에 접근할 수 없습니다. Play with Docker에서는 instance를 새로 만들고, 로컬에서는 Docker Desktop 또는 Docker service 상태를 확인합니다.

### port is already allocated

Host의 8080 port를 다른 process 또는 container가 사용 중입니다.

```bash
docker ps --filter publish=8080
```

기존 container를 확인해 멈추거나, 새 실습의 host port만 바꿉니다.

```bash
docker run -d --name se-nginx -p 8081:80 nginx:alpine
```

### container name is already in use

같은 이름의 container가 남아 있습니다.

```bash
docker ps -a --filter name=se-nginx
```

이전 실습에서 만든 것이 맞는지 확인한 뒤 `docker rm se-nginx`로 제거합니다. 정체를 확인하지 않고 강제 삭제하지 않습니다.

## 핵심 정리

1. Image는 실행 환경의 read-only template이고 container는 그 image에서 시작된 격리 process입니다.
2. Linux container는 host kernel을 공유하므로 VM과 구조와 보안 경계가 다릅니다.
3. `-p host:container`는 network port를 외부에 publish하며 Dockerfile의 `EXPOSE`와 다른 동작입니다.
4. 상태는 `ps`, 출력은 `logs`, 설정은 `inspect`로 확인합니다.
5. 실습 resource는 이름을 확인한 뒤 정확한 대상을 지정해 정리합니다.

## 참고 자료

- [입문 Docker — 일본어 원문](https://y-ohgi.com/introduction-docker/)
- [Docker Docs — What is a container?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)
- [Docker Docs — Publishing and exposing ports](https://docs.docker.com/get-started/docker-concepts/running-containers/publishing-ports/)
- [Play with Docker](https://labs.play-with-docker.com/)
