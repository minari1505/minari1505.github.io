---
title: "Sharing skills"
title_ko: "Skill 공유하기"
course: agent-skills
lesson: 5
---

## 학습 목표

- 배포 범위(팀 → 여러 팀 → 조직 전체)에 따른 3가지 공유 방법 구분하기
- 각 방법의 동작 방식과 주의점(신뢰, 네임스페이스) 이해하기
- 외부 skill을 설치할 때의 보안 체크포인트 익히기

## 배포 범위에 따른 3가지 방법

| 방법 | 대상 | 하는 법 |
|---|---|---|
| **Project skills** | 같은 저장소를 쓰는 팀원 | `.claude/skills/`를 git에 커밋 |
| **Plugins** | 여러 팀·여러 저장소 | 플러그인의 `skills/` 디렉토리에 포함해 배포 |
| **Managed (Enterprise)** | 조직 전체 | managed settings로 강제 배포 |

## 1. Project skills — git으로 팀과 공유

가장 간단한 방법입니다. `.claude/skills/`를 버전 관리에 커밋하면, 저장소를 clone한 팀원 모두에게 자동 적용됩니다.

```text
my-repo/
└── .claude/
    └── skills/
        ├── deploy-staging/
        │   └── SKILL.md
        └── api-conventions/
            └── SKILL.md
```

- skill도 코드처럼 **리뷰·버전 관리·롤백**이 가능해집니다.
- 프로젝트 skill의 `allowed-tools`는 **워크스페이스 신뢰(trust) 승인 후**에만 발효돼요. skill이 스스로에게 광범위한 도구 권한을 줄 수 있으니, 저장소를 신뢰하기 전에 skill을 검토하세요.
- 모노레포라면 하위 패키지의 `.claude/skills/`도 인식됩니다. 이름이 겹치면 `apps/web:deploy`처럼 디렉토리 경로로 구분돼요.

## 2. Plugins — 여러 저장소·팀에 배포

저장소 하나를 넘어 배포하려면 **플러그인**에 skill을 담습니다.

- 플러그인의 `skills/` 디렉토리에 skill을 넣고, 마켓플레이스를 통해 설치하게 합니다.
- 플러그인 skill은 `plugin-name:skill-name` **네임스페이스**를 사용하므로 개인/프로젝트 skill과 충돌하지 않아요.
- skill 외에 agents, hooks, MCP 서버까지 한 패키지로 묶을 수 있습니다.

## 3. Managed settings — 조직 전체 강제 배포

Enterprise 환경에서는 관리자가 **managed settings**로 조직 구성원 전체에게 skill을 배포할 수 있습니다.

- 같은 이름의 skill이 있으면 **Enterprise > Personal > Project** 순으로 우선 적용됩니다.
- 관리자는 `disableSkillShellExecution` 같은 정책 설정으로 사용자 skill의 셸 실행(`` !`cmd` ``)을 조직 차원에서 비활성화할 수도 있어요.

## 🔒 보안: 설치 전 체크리스트

Skill은 지시문 + 코드로 에이전트에 새 능력을 부여하는 만큼, 잠재적 취약점이 될 수 있습니다. **신뢰할 수 있는 출처의 skill만 설치**하고, 설치 전에 확인하세요.

1. `SKILL.md`와 부속 파일의 **지시 내용** — 이상한 행동을 유도하지 않는지
2. 번들 **스크립트 코드와 의존성**
3. **외부 네트워크 접속**을 지시하는 부분이 있는지
4. `allowed-tools`가 필요 이상으로 넓지 않은지

## 정리

- 범위 확장 순서: 프로젝트 커밋(`.claude/skills/`) → 플러그인(마켓플레이스) → managed settings(조직).
- 플러그인 skill은 네임스페이스로 충돌 방지, Enterprise skill은 최우선 적용.
- 외부 skill은 코드·의존성·네트워크 접근·도구 권한을 검토한 뒤 설치.
