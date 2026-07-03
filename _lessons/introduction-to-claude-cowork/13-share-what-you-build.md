---
title: "Share what you build with your team"
title_ko: "만든 것을 팀과 공유하기"
course: introduction-to-claude-cowork
lesson: 13
---

## 학습 목표

- plugin이 Enterprise 조직에 어떻게 배포되는지 설명하기
- 공유 plugin을 시간이 지나도 건강하게 유지하는 습관 적용하기

## 팀 전체로 워크플로 확장

이 시점이면 팀에는 자리를 얻은 skill 몇 개가 있습니다 — 누군가의 개인 방식으로 시작해, eval을 거쳤고, 여러 사람의 사용 사례에서 버팁니다. 이를 팀 전체로 효율적으로 확장하려면 **plugin으로 묶습니다**(Lesson 8). 이 레슨은 그다음 단계 — 필요한 모두에게 그 plugin을 전달하는 것입니다.

## 조직에서 plugin 배포

큰 회사에서 권장 방식은 **조직의 private marketplace** — 관리자가 관리하는, 회사 승인 plugin 카탈로그 — 를 통하는 것입니다.

실제로 배포는 **핸드오프**입니다. marketplace를 소유한 사람(팀 리드, enablement/운영 담당, IT)에게 plugin을 가져가면 그들이 게시합니다. 게시 시 **어떻게 도달할지**를 고릅니다:

- **Available** — Directory에 나타나고 원하는 사람이 설치.
- **Installed by default** — Cowork를 열면 이미 설치돼 있고, 끌 수 있음.
- **Required** — 설치돼 유지됨. 모두가 같게 실행해야 하는 규정 점검 등에 유용.
- **Hidden** — marketplace에 있지만 Directory에 안 보임. 스테이징·제한 롤아웃용.

팀원 입장에서는 plugin이 회사에서 온 것으로 라벨링돼 공개 Anthropic plugin과 나란히 Directory에 나타납니다. 쓰고 끌 수 있지만(required 제외) 편집은 못 하며, 업데이트는 유지 관리자에게서 흐릅니다.

## 유지 습관

공유 plugin이 조용히 낡지 않게 하는 관행:

- **소유자 한 명.** 모든 공유 plugin에는 변경을 검토하고, 수정 후 eval을 돌리고, 업데이트·폐기를 정하는 지정된 사람이 있어야 합니다.
- **게시 전 항상 eval.** eval 루프를 관문으로 삼으세요 — 중요 케이스가 변경 후 버티지 못하면 모두에게 push하지 마세요.
- **skill·plugin 이름을 구체적으로.** "meeting-prep"은 조직 곳곳의 다른 셋과 충돌할 수 있습니다. "sales-customer-renewal-prep"은 아닙니다.
- **검토 주기 설정.** 분기별 정도가 합리적 시작점 — 무엇이 설치됐고, 실제로 쓰이고, 낡았는지 봅니다. 아무도 안 쓰는 것은 폐기하고, 개선 지점은 반영.

## 핵심 정리

- 자리를 얻은 skill을 plugin으로 묶고, 조직 private marketplace 소유자에게 핸드오프해 배포합니다.
- 게시자는 Available/Installed by default/Required/Hidden 중 도달 방식을 고릅니다.
- 소유자 한 명, 게시 전 eval, 구체적 이름, 정기 검토 — 이 습관이 공유 plugin을 건강하게 유지합니다.
