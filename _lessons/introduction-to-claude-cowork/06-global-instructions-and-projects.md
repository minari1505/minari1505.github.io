---
title: "Standing context: Global instructions and projects"
title_ko: "상시 맥락: global instructions와 projects"
course: introduction-to-claude-cowork
lesson: 6
---

## 학습 목표

- global instructions로 Claude가 모든 세션을 "내가 어떻게 일하는지" 아는 상태로 시작하게 하기
- 어떤 작업이 project에 속하는지 판단하기
- project를 시작하는 세 가지 방법 고르기

## 새 협업자 온보딩하기

Chat에서 memory는 저절로 쌓입니다 — 켜두면 대화에서 자동 학습. Cowork는 다릅니다. 작업 간에 이어지는 맥락은 대부분 **사용자가 설정한 맥락**입니다: 모든 세션에 적용되는 **global instructions**, 그리고 그 안의 대화에서 Claude가 자동으로 memory를 쌓는 **projects**.

## Global instructions: 모든 세션에 적용되는 브리프

한 번 작성하는 상시 브리프입니다. Settings에서 쓰면 Claude가 모든 세션(모든 chat, 예약 작업, Dispatch)에서 참조합니다.

설정: **Settings → Cowork → Global instructions 옆 Edit → 작성·저장.**

무엇을 넣나:

- 내가 누구이고 무슨 일을 하는가
- 내가 쓰는 약어·줄임말 (Claude가 "QBR 덱"이 뭔지 안 묻도록)
- 출력을 어떻게 받고 싶은가 (형식·길이·톤)

처음부터 완벽할 필요 없습니다. 반복해서 주는 교정("결론부터 말해줘", "옥스퍼드 콤마 쓰지 마")이 곧 global-instruction 후보입니다.

## Projects: 하나의 작업 흐름을 위한 워크스페이스

Global instructions가 "나"를 커버한다면, project는 "내가 지금 하는 일"을 커버합니다. 특정 작업 흐름(고객, 반복 결과물, 런칭)에 묶인 워크스페이스로, 안에 네 가지가 있습니다.

- **Instructions** — global과 같지만 이 프로젝트에 한정. (예: "주간 혁신팀 회의용. 조직 데이터를 모아 매주 슬라이드 덱으로 압축.")
- **Scheduled tasks** — 프로젝트에 속한 반복 실행. 프로젝트 맥락으로 매번 실행됩니다.
- **Context** — Claude가 작업할 폴더나 링크. 프로젝트의 모든 대화가 접근.
- **Memory** — 프로젝트 안 대화에서 Claude가 배우는 것. 시간이 지나며 쌓이고, 직접 쓰지 않습니다.

바로 이 마지막(memory)이 project의 차이입니다. 프로젝트 밖에서는 매 세션이 global instructions만 빼고 새로 시작하지만, 프로젝트 안에서는 모든 대화가 아는 것에 더해져 다음 작업이 이미 고객 상황·지난주 결정·미결 사항을 손에 쥔 채 시작됩니다.

project로 좋은 작업 흐름: **고객/계정, 반복 결과물, 런칭/이니셔티브.**

## Project를 시작하는 세 가지 방법

- **처음부터(from scratch)** — 빈 상태로 시작해 instructions·context를 더해감.
- **기존 폴더에서** — 이미 작업하는 폴더를 가리키면 그것이 프로젝트 작업 디렉터리가 됨.
- **Chat 프로젝트에서** — Chat에서 쓰던 프로젝트의 instructions·knowledge를 Cowork로 이전(단방향 — Cowork 변경은 Chat으로 역동기화되지 않음).

만들기: Cowork 사이드바에서 **Projects → New project**. 작업 폴더·instructions·커넥터는 언제든 변경 가능.

## 핵심 정리

- 두 층을 함께 두세요 — 나를 위한 global instructions, 작업 흐름을 위한 project.
- Global instructions는 모든 세션에 적용되는 상시 브리프이고, project는 memory가 쌓이는 범위 좁힌 워크스페이스입니다.
- project는 처음부터/기존 폴더에서/Chat 프로젝트에서 세 방법으로 시작할 수 있습니다.
