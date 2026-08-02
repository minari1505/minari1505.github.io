---
title: "Production Container Design"
title_ko: "프로덕션 Docker 설계와 운영 원리"
course: docker-fundamentals-and-production
lesson: 4
tags:
  - Docker
  - Container Security
  - OCI
  - Production
---

## 학습 목표

- container 격리와 image 저장 방식을 Linux·OCI 관점에서 설명하기
- 종료 signal, PID 1, resource limit을 고려해 application 실행하기
- 작고 재현 가능한 image와 최소 권한 runtime 구성하기
- 단일 host의 Compose와 여러 host의 orchestrator 경계 판단하기

## ELI15: Container는 ‘독방이 있는 기숙사’다

Virtual machine은 건물 안에 작은 집을 하나 더 짓는 것과 비슷합니다. 각 집이 자기 OS kernel까지 가집니다. Container는 같은 건물의 kernel을 함께 쓰되 방마다 보이는 process·network·file system을 다르게 만든 것에 가깝습니다.

그래서 container는 빠르고 가볍지만 완전히 별개의 computer는 아닙니다. 같은 host kernel을 공유하므로 다음 세 가지가 중요합니다.

1. 방에서 보이는 범위를 줄인다: namespace
2. 방이 쓸 수 있는 양을 제한한다: cgroup
3. 방 열쇠를 최소한으로 준다: capability, seccomp, non-root

Production에서는 image를 잘 만드는 일과 container를 안전하게 실행하는 일을 함께 설계해야 합니다.

## ELIPhD 1: Linux 격리와 OCI

Container는 하나의 단일 기능이 아니라 Linux primitives의 조합입니다.

| Primitive | 하는 일 | 운영 시 질문 |
|---|---|---|
| PID namespace | process ID 공간 분리 | host process가 보이는가? |
| Network namespace | interface·route·port 분리 | 외부에 어떤 port를 열었는가? |
| Mount namespace | mount view 분리 | host path를 과도하게 공유했는가? |
| User namespace | container UID를 host UID와 다르게 mapping | root 권한 범위가 어디까지인가? |
| cgroup | CPU·memory·I/O 사용량 통제 | 한 workload가 host를 고갈시킬 수 있는가? |
| capability | root 권한을 작은 단위로 분해 | 정말 필요한 capability만 남았는가? |
| seccomp | 허용할 system call 제한 | 기본 profile을 무력화하지 않았는가? |

OCI Image Specification은 image manifest·configuration·filesystem layer의 상호 운용 형식을, OCI Runtime Specification은 unpack된 bundle을 실행하는 방법을 정의합니다. Docker image와 runtime이 다른 OCI-compatible 도구와 함께 동작할 수 있는 기반입니다.

Namespace는 강한 경계를 제공하지만 virtual machine과 같은 별도 kernel 경계는 아닙니다. 민감한 multi-tenant 환경에서는 VM, sandboxed runtime, 별도 node 같은 추가 경계를 threat model에 따라 검토합니다.

## ELIPhD 2: Content-addressable layer와 Copy-on-write

Image layer는 content digest로 식별되는 변경 집합입니다. 같은 layer는 여러 image가 공유할 수 있고, pull할 때 digest로 integrity를 확인합니다. Container 실행 시 read-only image layer 위에 writable layer가 놓입니다. 파일을 수정하면 하위 layer를 직접 바꾸는 대신 writable layer로 복사해 변경하는 copy-on-write가 일어납니다.

이 구조에서 다음 결과가 나옵니다.

- 자주 바뀌지 않는 dependency step을 먼저 두면 build cache 재사용률이 높아집니다.
- build 중 생성한 secret을 다음 layer에서 삭제해도 이전 layer에 남을 수 있습니다.
- container writable layer는 영속 data 저장소가 아닙니다.
- tag는 움직일 수 있으나 digest는 특정 content를 가리킵니다.

## 실습 1: Signal을 올바르게 받는 PID 1

Shell form은 application 앞에 shell process를 둘 수 있습니다.

```dockerfile
# 피해야 할 예: signal 전달과 argument 처리가 shell에 좌우된다.
CMD python app.py
```

Exec form은 application을 직접 실행합니다.

```dockerfile
CMD ["python", "app.py"]
```

Container의 PID 1은 종료 signal을 받고 child process를 회수해야 합니다. Application이 이를 제대로 처리하지 못하면 `--init`로 작은 init process를 둘 수 있습니다.

```bash
docker run -d --name signal-demo --init nginx:alpine
docker inspect --format '{{.State.Pid}}' signal-demo
time docker stop --time 10 signal-demo
docker rm signal-demo
```

`docker stop`은 먼저 `SIGTERM`을 보내고 제한 시간 뒤에도 종료하지 않으면 `SIGKILL`을 보냅니다. Grace period는 application의 request drain·transaction 종료 시간보다 짧지 않게 정합니다.

## 실습 2: Resource limit 관찰하기

### 목적

Container가 host resource를 무제한 소비하지 않도록 memory와 CPU 상한을 둡니다.

```bash
docker run -d \
  --name limited-nginx \
  --memory 128m \
  --cpus 0.50 \
  -p 8081:80 \
  nginx:alpine

docker stats --no-stream limited-nginx
docker inspect --format '{{.HostConfig.Memory}} {{.HostConfig.NanoCpus}}' limited-nginx
curl http://localhost:8081
docker stop limited-nginx
docker rm limited-nginx
```

예상 결과는 memory limit `134217728` bytes와 NanoCPU `500000000`입니다. Limit은 capacity planning을 대신하지 않으며, memory 초과 시 OOM kill과 application latency를 함께 관찰해야 합니다.

## Multi-stage build로 실행 image 줄이기

Build toolchain과 runtime을 분리하면 공격 표면과 전송량을 줄일 수 있습니다.

```dockerfile
FROM golang:1.24 AS build
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -trimpath -ldflags="-s -w" -o /out/server ./cmd/server

FROM gcr.io/distroless/static-debian12:nonroot
COPY --from=build /out/server /server
USER nonroot:nonroot
ENTRYPOINT ["/server"]
```

각 base image version은 프로젝트 지원 정책에 맞춰 고정하고 정기적으로 갱신합니다. Digest pinning은 동일 artifact 재현에 유리하지만 보안 update가 자동 반영되지 않으므로 update bot이나 정기 rebuild가 함께 필요합니다.

## Build secret은 layer와 log에 남기지 않는다

Secret을 `ARG`, `ENV`, `COPY`로 image에 넣지 않습니다. BuildKit secret mount는 해당 `RUN` 동안만 secret을 노출합니다.

```dockerfile
# syntax=docker/dockerfile:1
FROM alpine:3.21
RUN --mount=type=secret,id=npmrc,target=/root/.npmrc \
    test -s /root/.npmrc
```

```bash
printf '//registry.npmjs.org/:_authToken=example-token\n' > .npmrc.demo
DOCKER_BUILDKIT=1 docker build --secret id=npmrc,src=.npmrc.demo -t secret-demo .
rm .npmrc.demo
docker image rm secret-demo
```

위 token은 실습용 가짜 값입니다. 실제 secret은 shell history, source control, CI log에도 남지 않도록 secret manager에서 주입합니다.

## Runtime 최소 권한

다음 명령은 nginx가 필요한 writable path만 tmpfs로 제공하고 나머지 root filesystem을 read-only로 실행하는 예입니다.

```bash
docker run -d \
  --name hardened-nginx \
  --read-only \
  --tmpfs /var/cache/nginx:rw,noexec,nosuid,size=32m \
  --tmpfs /var/run:rw,noexec,nosuid,size=1m \
  --cap-drop ALL \
  --cap-add CHOWN \
  --cap-add DAC_OVERRIDE \
  --cap-add SETGID \
  --cap-add SETUID \
  --cap-add NET_BIND_SERVICE \
  -p 8082:80 \
  nginx:alpine

curl http://localhost:8082
docker inspect hardened-nginx
docker stop hardened-nginx
docker rm hardened-nginx
```

Capability 목록은 image마다 다릅니다. 먼저 staging에서 동작과 audit log를 확인하고 실제로 필요한 권한만 남깁니다. `--privileged`, Docker socket mount, host namespace 공유는 격리를 크게 약화하므로 편의 목적으로 사용하지 않습니다.

Docker daemon 자체의 root 권한을 줄여야 한다면 rootless mode를 검토합니다. 다만 privileged port, cgroup, storage driver와 운영 환경의 제약을 먼저 확인합니다.

## 공급망: SBOM, Provenance, Scan은 서로 다른 질문이다

| 통제 | 답하는 질문 |
|---|---|
| SBOM | 이 image 안에 어떤 component가 있는가? |
| Provenance | 누가 어떤 source와 build 과정으로 만들었는가? |
| Signature·attestation | 신뢰한 주체가 artifact·metadata를 승인했는가? |
| Vulnerability scan | 알려진 취약점이 어떤 component에 연결되는가? |

Scan 결과가 없다고 안전한 것은 아니며, 결과가 있다고 곧바로 exploitable한 것도 아닙니다. 배포 정책은 severity뿐 아니라 실행 가능성, 노출 경로, 수정 버전, 예외 만료일을 함께 다룹니다.

Buildx가 지원되는 환경에서는 metadata를 포함해 build할 수 있습니다.

```bash
docker buildx build \
  --provenance=mode=max \
  --sbom=true \
  --tag example.com/team/app:1.0.0 \
  --load \
  .
```

Builder와 output 방식에 따라 attestation 보존 방식이 다르므로 registry에 push한 뒤 실제 manifest와 attestation을 확인해야 합니다.

## Compose에서 Orchestrator로 넘어갈 때

Compose는 여러 container의 실행 구성을 한 host 중심으로 표현하는 데 탁월합니다. 다음 요구가 생기면 scheduler와 controller가 있는 orchestrator를 검토합니다.

- 여러 node 중 placement를 결정해야 한다.
- replica의 desired state를 지속적으로 맞춰야 한다.
- node failure 때 다른 node에 workload를 재배치해야 한다.
- rolling update·rollback·autoscaling·policy가 필요하다.

| 선택지 | 적합한 상황 | 추가 비용 |
|---|---|---|
| Docker Compose | local 개발, 단일 host의 작은 service | multi-host scheduling·self-healing 제한 |
| Docker Swarm | Docker 중심의 비교적 단순한 cluster | 생태계·운영 선택 폭 검토 필요 |
| Kubernetes | 복잡한 multi-service platform과 확장 정책 | 높은 학습·운영 복잡도 |
| Managed container service | control plane 부담을 줄이고 cloud 통합 활용 | vendor API·비용·제약 고려 |

Orchestrator는 나쁜 image와 불안정한 application을 고쳐주지 않습니다. Health signal, graceful shutdown, idempotency, resource request·limit, observability가 먼저 준비되어야 합니다.

## Production readiness checklist

- [ ] Base image 출처·version·update 주기가 정의되어 있다.
- [ ] Multi-stage build와 `.dockerignore`로 불필요한 파일을 제외했다.
- [ ] Secret이 image layer·environment dump·log에 남지 않는다.
- [ ] Non-root, capability, seccomp, read-only filesystem을 검토했다.
- [ ] CPU·memory limit과 OOM 시 동작을 부하 환경에서 확인했다.
- [ ] `SIGTERM` 수신 후 traffic drain과 정상 종료가 된다.
- [ ] Liveness와 readiness의 의미가 구분되어 있다.
- [ ] Persistent data backup·restore를 실제로 시험했다.
- [ ] SBOM·provenance·scan·patch SLA가 배포 과정에 연결되어 있다.
- [ ] Log·metric·trace와 incident 대응 책임자가 정해져 있다.

## 핵심 정리

1. Container는 별도 kernel이 아니라 namespace·cgroup 등으로 격리된 host process입니다.
2. Image layer, runtime 권한, resource limit, signal 처리는 하나의 production 설계 문제입니다.
3. Non-root 하나만으로 안전해지지 않으며 filesystem·capability·secret·공급망 통제가 함께 필요합니다.
4. Compose와 orchestrator의 차이는 YAML 문법보다 scheduling, desired state reconciliation, failure recovery에 있습니다.
5. 재현성과 보안 update는 상충할 수 있으므로 digest pinning과 자동 갱신 정책을 함께 운영합니다.

## 참고 자료

- [Docker Docs — Security](https://docs.docker.com/engine/security/)
- [Docker Docs — Resource constraints](https://docs.docker.com/engine/containers/resource_constraints/)
- [Docker Docs — Multi-stage builds](https://docs.docker.com/build/building/multi-stage/)
- [Docker Docs — Build secrets](https://docs.docker.com/build/building/secrets/)
- [Docker Docs — Rootless mode](https://docs.docker.com/engine/security/rootless/)
- [Docker Docs — Build attestations](https://docs.docker.com/build/metadata/attestations/)
- [Docker Docs — Containerize production applications](https://docs.docker.com/guides/docker-concepts/building-images/build-tag-and-publish-an-image/)
- [OCI Image Format Specification](https://github.com/opencontainers/image-spec)
- [OCI Runtime Specification](https://github.com/opencontainers/runtime-spec)
- [Kubernetes — Pods and containers](https://kubernetes.io/docs/concepts/workloads/pods/)
