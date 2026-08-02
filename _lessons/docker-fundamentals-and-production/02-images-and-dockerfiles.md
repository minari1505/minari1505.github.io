---
title: "Images and Dockerfiles"
title_ko: "Image와 Dockerfile 직접 만들기"
course: docker-fundamentals-and-production
lesson: 2
tags:
  - Docker
  - Dockerfile
  - Container Image
---

## 학습 목표

- image, layer, tag, digest, registry의 관계 이해하기
- Dockerfile과 build context로 직접 image 만들기
- build cache와 `.dockerignore`가 build 속도·보안에 미치는 영향 확인하기
- container를 non-root user로 실행하고 port publish하기

## Image는 무엇으로 이루어져 있을까?

Image는 application filesystem을 이루는 **read-only layer**와 실행에 필요한 metadata의 묶음입니다.

```text
Image se-web:1.0
├─ Layer: python:3.13-slim base filesystem
├─ Layer: /app 작업 directory
├─ Layer: index.html
└─ Metadata: USER, EXPOSE, CMD
```

Container를 실행하면 image layer 위에 임시 writable layer가 하나 추가됩니다.

```text
Container
├─ Writable container layer
└─ Read-only image layers
```

Container를 제거하면 writable layer도 함께 사라집니다. 보존할 data는 다음 레슨에서 volume으로 분리합니다.

## Tag와 Digest

`python:3.13-slim`에서 `python`은 repository, `3.13-slim`은 tag입니다. Tag는 사람이 읽기 쉽지만 registry가 새 image를 같은 tag에 연결할 수 있는 **변경 가능한 이름**입니다.

Digest는 image manifest content를 기반으로 한 식별자입니다.

```text
python:3.13-slim@sha256:...
```

- 학습·개발에서는 관리하기 쉬운 tag를 자주 사용합니다.
- 재현성이 중요한 배포에서는 검증된 digest pinning과 정기 update 절차를 함께 고려합니다.
- Digest만 고정하고 update하지 않으면 오래된 취약점도 그대로 고정됩니다.

## 실습 준비

새 directory에서 시작합니다.

```bash
mkdir -p docker-image-lab
cd docker-image-lab
```

현재 위치를 확인합니다.

```bash
pwd
ls -la
```

## 실습 1: Web page 만들기

### 목적

Container에 넣을 가장 작은 정적 web page를 만듭니다.

```bash
cat > index.html <<'EOF'
<!doctype html>
<html lang="ko">
  <head><meta charset="utf-8"><title>Docker Lab</title></head>
  <body>
    <h1>Hello from se-web:1.0</h1>
    <p>이 파일은 container image 안에 들어 있습니다.</p>
  </body>
</html>
EOF
```

확인합니다.

```bash
cat index.html
```

## 실습 2: Dockerfile 작성하기

### 목적

Image를 재현 가능하게 만드는 설명서를 작성합니다.

```bash
cat > Dockerfile <<'EOF'
FROM python:3.13-slim

WORKDIR /app
COPY index.html .

USER 65532:65532
EXPOSE 8000

CMD ["python", "-m", "http.server", "8000"]
EOF
```

Build context에서 제외할 파일도 정의합니다.

```bash
cat > .dockerignore <<'EOF'
.git
.gitignore
.DS_Store
*.log
.env
EOF
```

### 각 instruction의 의미

| Instruction | 실행 시점 | 역할 |
|---|---|---|
| `FROM` | build | base image 선택 |
| `WORKDIR` | build·run metadata | 이후 명령과 process의 기본 directory 설정 |
| `COPY` | build | build context의 파일을 image에 복사 |
| `USER` | run metadata | 이후 build instruction과 기본 process user 지정 |
| `EXPOSE` | metadata | application이 사용할 port를 문서화 |
| `CMD` | run metadata | 기본 process와 argument 지정 |

`EXPOSE 8000`은 host에 port를 열지 않습니다. 실제 publish는 `docker run -p 127.0.0.1:8000:8000`처럼 실행할 때 합니다.

`USER 65532:65532`는 숫자 UID·GID로 application process가 root가 아니게 합니다. 이것만으로 모든 privilege가 사라지는 것은 아닙니다. Capability, mount, daemon 권한과 host 설정도 함께 봐야 합니다.

## 실습 3: Image build하기

### 목적

현재 directory를 build context로 보내 `se-web:1.0` image를 만듭니다.

```bash
docker build -t se-web:1.0 .
```

마지막 `.`은 Dockerfile 위치가 아니라 **build context**입니다. `COPY`는 기본적으로 이 context 안의 파일만 읽을 수 있습니다.

### 예상 결과

BuildKit이 각 step을 실행하고 마지막에 image 이름을 출력합니다. Image 목록을 확인합니다.

```bash
docker image ls se-web
```

`se-web`, tag `1.0`이 보이면 성공입니다.

Layer history를 확인합니다.

```bash
docker history se-web:1.0
```

Dockerfile instruction과 연결되는 여러 항목이 보입니다. History는 layer 이해에 도움을 주지만 secret이 image에 들어갔는지 완벽히 보장하는 security scanner는 아닙니다.

설정 metadata를 확인합니다.

{% raw %}
```bash
docker image inspect --format '{{json .Config}}' se-web:1.0
```
{% endraw %}

`User`, `ExposedPorts`, `Cmd`를 찾을 수 있습니다.

## 실습 4: 만든 Image 실행하기

```bash
docker run -d --name se-web -p 127.0.0.1:8000:8000 se-web:1.0
```

상태와 process user를 확인합니다.

```bash
docker ps --filter name=se-web
docker exec se-web id
```

`uid=65532 gid=65532`가 포함되면 Dockerfile의 `USER`가 적용된 것입니다.

HTTP 응답을 확인합니다.

```bash
curl http://localhost:8000
```

`Hello from se-web:1.0`이 포함된 HTML이 나와야 합니다.

## 실습 5: Build cache 확인하기

먼저 코드를 한 줄 바꿉니다.

```bash
sed -i 's/se-web:1.0/se-web:1.1/' index.html
```

macOS BSD `sed`를 사용한다면 다음처럼 실행합니다.

```bash
sed -i '' 's/se-web:1.0/se-web:1.1/' index.html
```

새 tag로 다시 build합니다.

```bash
docker build -t se-web:1.1 .
```

### 예상 결과

변경되지 않은 앞 step은 `CACHED`로 표시되고 `COPY index.html .` 이후가 다시 실행됩니다. Dockerfile을 설계할 때 자주 바뀌는 source code보다 dependency manifest를 먼저 복사하면 dependency 설치 layer를 더 오래 재사용할 수 있습니다.

Cache는 이전 build 결과를 재사용합니다. 항상 안전하다는 뜻은 아닙니다. Base image update를 반영하려면 정책에 따라 `--pull`과 cache invalidation을 사용해야 합니다.

```bash
docker build --pull -t se-web:1.1 .
```

## `.dockerignore`가 필요한 이유

Build context에 필요 없는 파일이 많으면 전송과 hash 계산이 느려집니다. 더 중요한 문제는 secret이나 개인 파일을 실수로 `COPY . .`에 포함할 위험입니다.

`.dockerignore`는 다음에 도움이 됩니다.

- `.git`, log, build output 제외
- context 크기 축소
- cache invalidation 감소
- `.env` 같은 민감 파일의 우발적 포함 방지

하지만 `.dockerignore`만 security boundary로 믿으면 안 됩니다. Secret은 BuildKit secret mount나 외부 secret manager로 전달해야 합니다.

## `COPY`와 `ADD`

대부분의 local 파일 복사는 `COPY`가 의도가 명확합니다. `ADD`는 archive 자동 해제와 remote source 같은 추가 동작이 있으므로 그 기능이 실제로 필요할 때 사용합니다. “ADD가 더 강력하니 항상 좋다”가 아닙니다.

## 실습 정리

실행 중인 `se-web` container를 정상 종료하고 제거합니다.

```bash
docker stop se-web
docker rm se-web
```

이번 실습에서 만든 두 image만 제거합니다.

```bash
docker image rm se-web:1.0 se-web:1.1
```

확인합니다.

```bash
docker image ls se-web
```

목록이 비어 있으면 정리가 끝났습니다. 실습 파일은 다음 레슨에서 bind mount·Compose에 재사용하므로 directory는 남겨둡니다.

## 자주 만나는 오류

### COPY failed 또는 file not found

필요한 파일이 build context 밖에 있거나 `.dockerignore`에 포함됐는지 확인합니다.

```bash
pwd
ls -la
cat .dockerignore
```

### exec format error

Host와 image platform 또는 실행 파일 architecture가 맞지 않을 수 있습니다.

{% raw %}
```bash
docker version --format '{{.Server.Arch}}'
docker image inspect --format '{{.Architecture}}/{{.Os}}' se-web:1.0
```
{% endraw %}

Multi-platform build와 emulation은 프로덕션 레슨에서 다룹니다.

### Image 삭제 실패

해당 image를 사용하는 container가 남아 있을 수 있습니다.

```bash
docker ps -a --filter ancestor=se-web:1.0
```

대상을 확인하고 이번 실습 container인 경우에만 먼저 제거합니다.

## 핵심 정리

1. Dockerfile은 image를 재현하는 설명서이고 build context는 build가 읽을 수 있는 입력 범위입니다.
2. Image는 read-only layer와 metadata로 구성되며 tag는 변경될 수 있습니다.
3. `.dockerignore`는 속도와 우발적 파일 포함을 줄이지만 secret 관리 전체를 대신하지 않습니다.
4. `EXPOSE`는 문서화 metadata이고 `-p`가 실제 port publishing을 수행합니다.
5. Cache, digest pinning, base image update는 재현성과 보안 사이에서 함께 설계해야 합니다.

## 참고 자료

- [Dockerfile reference](https://docs.docker.com/reference/dockerfile/)
- [Docker build context](https://docs.docker.com/build/concepts/context/)
- [Build cache](https://docs.docker.com/build/cache/)
- [`.dockerignore` files](https://docs.docker.com/build/concepts/context/#dockerignore-files)
- [Image digests](https://docs.docker.com/dhi/core-concepts/digests/)
