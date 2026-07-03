---
title: "Plugins: Encode your team's expertise"
title_ko: "Plugins: 팀의 전문성 담기"
course: introduction-to-claude-cowork
lesson: 8
---

## 학습 목표

- plugin을 정의하고 무엇을 묶는지 이해하기
- plugin의 두 가지 형태 알기
- 실제 작업에 plugin을 설치하거나 커스터마이징하기

## plugin이란

**Plugin**은 하나의 "일(job)"을 중심으로 묶은 skill 세트입니다. skill이 플레이북 하나라면, plugin은 여러 개 — skill들과 그것들이 의존하는 커넥터·서브에이전트까지. (서브에이전트는 skill이 한 부분을 자체 맥락에서 처리하도록 띄우는 목적형 헬퍼입니다 — 예: 조사 단계용 research 서브에이전트.)

plugin은 **팀의 방식**을 Claude에게 가르칩니다. finance plugin을 설치하면 팀이 주식을 분석하는 방식을, legal plugin을 설치하면 계약 플레이북을 압니다. 전문성이 사람이 아니라 설치를 따라 이동합니다.

Anthropic은 흔한 역할(finance, legal, sales, marketing, customer support, product management 등)용 plugin을 게시합니다. 그대로 설치하거나, 커스터마이징하거나, 직접 만들 수 있습니다.

## 두 종류의 plugin

- **형태 1: 처음부터 끝까지의 프로세스를 묶음.** 순차 단계가 많은 작업을, 각 단계 skill을 묶어 전체가 하나로 돌게 합니다. 예: monthly-close plugin = 실적 수집 + 분산 표 작성 + board memo 초안 skill.
- **형태 2: 팀이 가장 많이 쓰는 skill을 묶음.** 서로 의존하지 않는, 팀이 자주 쓰는 skill 모음. 예: finance plugin = 분산 분석 + 재무 모델링 + 투자 메모 초안 + 분기 보고서. 새 팀원이 하나만 설치하면 팀 도구 세트 전체를 갖습니다.

핵심은 어느 쪽이든 **plugin은 워크플로를 중심으로 묶은 패키지**라는 점입니다.

## marketplace에서 설치

Anthropic은 흔한 역할용 plugin을 게시합니다. Cowork에서 **Customize → Plugins**로 가서, 내 작업에 맞는 것을 찾아 **Install**하고 커넥터를 승인하면 skill이 즉시 사용 가능해집니다.

## 팀에 맞게 커스터마이징

marketplace plugin은 강력한 기본값이지 최종 답은 아닙니다. 안의 skill·커넥터는 일반적인 버전이라, 팀 고유 템플릿·정의·단계에 맞춰 다듬을 수 있습니다.

설치 후 **Customize → Plugins → [Plugin 이름] → Customize.** 새 Cowork 작업이 열려 Claude와 함께 plugin을 손봅니다. 특정 asset을 가리키거나 예시를 업로드하면 Claude가 팀 맥락에 맞게 업데이트합니다.

```text
지난 red-lined NDA 3개야. 이 plugin의 /nda-triage skill을 이 포맷·톤에 맞게 업데이트해줘.
```

## 직접 만들기

기존 plugin에 맞지 않는 워크플로가 있으면 Cowork와 함께 만들 수 있습니다. 대부분 작게 시작합니다 — 가장 반복적인 작업의 skill 하나, 그다음 또 하나. skill 서너 개와 중요한 커넥터가 모이면 공유할 가치가 있는 plugin이 됩니다.

> 관리자가 이미 조직용 plugin을 게시했을 수 있으니, 만들기 전에 Directory(Customize → Plugins)를 확인하세요.

## 지금 해보기

새 Cowork 대화에서:

```text
/setup-cowork
```

짧은 인터뷰가 시작돼, 하는 일을 묻고 가장 맞는 plugin을 제안합니다. 바로 추가해 대화에서 테스트해볼 수 있습니다.

## 핵심 정리

- plugin은 하나의 일을 중심으로 묶은 skill(+커넥터·서브에이전트) 패키지로, 팀의 방식을 설치로 전파합니다.
- 처음부터 끝까지 프로세스형과 자주 쓰는 skill 모음형, 두 형태가 있습니다.
- marketplace에서 설치해 팀에 맞게 커스터마이징하거나 직접 만들 수 있습니다.
