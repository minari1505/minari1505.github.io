---
title: "Skills: Teach Claude Cowork your way"
title_ko: "Skills: Cowork에 내 방식 가르치기"
course: introduction-to-claude-cowork
lesson: 7
---

## 학습 목표

- skill을 정의하고 Claude가 어떻게 사용하는지 설명하기
- skill이 포함할 수 있는 네 가지 building block 알기
- 내 반복 프로세스로 skill 하나 만들기

## skill이란

**Skill**은 재사용 가능한 플레이북 — 파일과 리소스가 든 폴더 — 으로, 특정 종류의 일을 내가 원하는 방식대로 하도록 Claude에게 가르칩니다. 맞는 작업을 시작하면 Claude가 플레이북을 로드해 따릅니다.

skill은 **필요한 순간에 자동으로 사용**됩니다. 이름으로 부를 필요 없이, Claude가 작업이 설치된 skill과 맞는다고 판단하면 자동 로드합니다. 원하면 명시적으로 부를 수도 있습니다("board memo 작성 skill 써줘").

## skill 안의 네 가지

- **Instructions (SKILL.md)** — skill이 무엇을 하고, 언제 쓰며, 어떻게 하는지 알려주는 브리프. 새 동료용 런북처럼, 그들이 실제로 일할 수 있을 만큼 구체적으로.
- **Assets** — 로고, 브랜드 템플릿, 슬라이드 마스터, 폰트. 진짜 같은 출력을 만드는 원자재.
- **References** — 좋은 출력 예시, 스타일 가이드, 조항 라이브러리 등 "좋음"의 기준. Claude가 이 일에서 "좋다"가 무엇인지 배우는 방법.
- **Scripts** — 매번 같게 일어나야 하는 부분을 위한 작은 코드 (분산 계산, 구조화 비교, 차트 포매터 등).

skill은 이 중 어떤 조합이든 될 수 있습니다. SKILL.md 하나뿐인 skill도 충분하고, 브랜드 asset 폴더가 붙기도, 넷 다 있기도 합니다. **필요한 것만 넣고 그 이상은 넣지 않는 것**이 원칙입니다.

## Claude로 skill 만들기

가장 빠른 방법은 Claude와 함께 만드는 것입니다. Cowork에서 새 대화를 열고:

```text
[매번 다시 설명하기 지겨운 반복 프로세스]를 위한 skill을 만들고 싶어. 뭐가 필요한지 안내해줘.
```

Claude가 몇 가지를 묻습니다 — skill이 무엇을 해야 하는지, 언제 발동해야 하는지, 좋은 출력이 무엇인지, 어떤 리소스를 쓸지. 실제 예시·템플릿·과거 출력을 가리키며 구체적으로 답하세요. 결과는 SKILL.md와 필요한 asset·reference·script가 든, 설치 준비된 skill 폴더입니다.

설치 후 **Customize**에서 찾을 수 있고, 수정은 그냥 교정을 주며 업데이트를 요청하면 됩니다("10만 달러 넘고 두 단계 건너뛴 딜을 표시하는 단계 추가해줘"). Claude가 제자리에서 skill을 업데이트합니다.

skill은 프로젝트 안 대화를 포함해 어떤 대화에서든 같은 방식으로 작동합니다.

## 핵심 정리

- skill은 특정 종류의 일을 내 방식대로 하도록 가르치는, 파일이 든 재사용 폴더입니다.
- instructions·assets·references·scripts를 필요에 따라 조합하며, 맞는 작업에서 자동 사용됩니다.
- 가장 빠른 제작법은 Claude와 대화하며 만드는 것 — 반복하는 프로세스 하나가 첫 skill 후보입니다.
