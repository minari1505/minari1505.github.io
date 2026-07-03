---
title: "Artifacts"
title_ko: "Artifacts 만들고 공유하기"
course: claude-101
lesson: 5
---

## 학습 목표

- Artifacts가 무엇이고 Claude가 언제 artifact를 만드는지 이해하기
- artifact를 동료에게 공유하거나 공개 publish하는 방법 익히기
- artifact가 기대대로 생성되지 않을 때 해결하는 방법 정리하기

## Artifacts란?

**Artifacts**는 Claude가 대화 옆의 전용 창에 만들어주는 독립적인 output입니다. 긴 코드나 문서가 chat 안에 묻히는 대신, 바로 확인하고 수정하고 재사용할 수 있는 형태로 표시됩니다.

예를 들어 다음과 같은 결과물이 artifact로 만들어질 수 있습니다.

- 작동하는 웹페이지
- interactive chart
- 문서 초안
- reusable template
- diagram
- React component

Artifact는 대화의 일부이면서도, 대화에서 분리된 standalone output처럼 다룰 수 있습니다.

## Claude가 artifact를 만드는 기준

Claude는 보통 다음 조건을 만족하면 자동으로 artifact를 만듭니다.

- 내용이 충분히 크고 self-contained함
- 보통 15줄 이상으로 의미 있는 단위임
- 사용자가 편집, 반복 개선, 재사용할 가능성이 높음
- surrounding conversation 없이도 독립적으로 의미가 있음
- 나중에 다시 참조하거나 사용할 만한 결과물임

즉, artifact는 단순 답변보다 **작업물**에 가깝습니다.

## 흔한 artifact 유형

Claude가 만들 수 있는 artifact는 다양합니다.

| 유형 | 용도 |
|---|---|
| **Documents** | 회의록, 보고서, project plan, blog post 같은 text-heavy output |
| **Code snippets** | Python, JavaScript, C++ 등 여러 언어의 작동 코드 |
| **HTML pages** | landing page, form, interactive demo, quick prototype |
| **SVG images** | logo, icon, illustration, vector graphic |
| **Mermaid diagrams** | flowchart, sequence diagram, Gantt chart, org chart |
| **React components** | calculator, dashboard, game, data visualization 같은 interactive UI |

특히 React component artifact는 단순 mockup이 아니라 실제 logic과 user interaction을 포함할 수 있습니다.

## 첫 artifact 만들기

Artifact를 만들기 위해 특별한 명령을 외울 필요는 없습니다. 원하는 결과물을 설명하면 Claude가 artifact로 보여주는 것이 적절한지 판단합니다.

예시:

```text
고객 onboarding process를 보여주는 flowchart를 만들어줘.
```

```text
월별 지출을 입력하면 category별 breakdown을 보여주는 interactive dashboard를 만들어줘.
```

```text
생산성 앱 landing page를 hero section과 feature list가 포함되게 디자인해줘.
```

```text
새 initiative에 재사용할 project brief template을 작성해줘.
```

Claude가 artifact로 만들지 않고 chat에만 답했다면 명시적으로 요청하면 됩니다.

```text
이걸 artifact로 만들어줘.
```

또는:

```text
Show me this in an artifact.
```

## Artifact 창에서 할 수 있는 일

Artifact가 생성되면 대화 오른쪽에 전용 창이 열립니다. 여기서 다음 작업을 할 수 있습니다.

- **Preview 보기**: 결과물이 어떻게 보이는지 확인
- **Code 보기**: Claude가 생성한 underlying code 확인
- **Copy**: 다른 곳에 붙여넣기 위해 내용 복사
- **Download**: 파일로 저장
- **Iterate**: Claude에게 변경 요청해 artifact 개선

Artifact는 "Claude가 만든 결과물을 바로 다루는 작업 공간"입니다.

## Artifact 공유와 publish

유용한 artifact를 만들었다면 여러 방식으로 공유할 수 있습니다.

### Copy 또는 download

개인적으로 사용하거나 다른 채널로 공유하려면 copy 또는 download 버튼을 사용합니다.

### 조직 내부 공유

Team과 Enterprise 사용자는 artifact를 조직 내부 동료와 공유할 수 있습니다. 이 경우 artifact는 조직 안에 머물고, 접근하려면 team authentication이 필요합니다.

### 공개 publish

Free, Pro, Max 사용자는 artifact를 공개 link로 publish할 수 있습니다.

Publish하면 다음이 적용됩니다.

- 선택한 artifact version만 public이 됩니다.
- 원래 chat은 private으로 유지됩니다.
- Claude account가 없는 사람도 link로 보고 interact할 수 있습니다.
- 다른 사람이 artifact를 remix해서 자신의 Claude conversation에서 수정할 수 있습니다.

Publish하려면 artifact 오른쪽 위의 `Share` 또는 `Publish` 버튼을 사용합니다. 나중에 언제든 unpublish할 수 있습니다.

주의할 점은 publish한 artifact는 link를 가진 누구나 볼 수 있다는 것입니다. 다만 search engine에 indexing되지는 않아 Google 검색 결과에 나타나지는 않습니다.

## Artifact를 잘 만드는 prompt

Artifact 품질은 요청의 구체성에 크게 영향을 받습니다.

### 원하는 기능을 구체적으로 말하기

```text
Budget tracker를 만들어줘.
```

보다:

```text
월별 예산 tracker를 만들어줘.
지출을 category별로 입력할 수 있고,
pie chart로 breakdown을 보여주며,
예산을 초과하면 warning을 보여줘.
```

가 더 좋습니다.

### 사용자를 설명하기

Artifact를 누가 쓸지 알려주면 Claude가 적절한 design choice를 할 수 있습니다.

```text
이 flowchart는 신입 직원 onboarding용이야.
```

와:

```text
이 flowchart는 engineering team의 incident response 절차용이야.
```

는 다른 결과를 만들어야 합니다.

### 점진적으로 개선하기

한 번에 모든 기능을 넣으려 하기보다 한 번에 하나씩 기능을 추가하거나 수정하는 편이 좋습니다.

- 먼저 기본 구조 만들기
- 그다음 chart 추가
- 그다음 warning state 추가
- 마지막으로 styling 조정

이렇게 하면 어떤 변경이 잘 작동하고 어떤 변경이 문제를 만드는지 더 쉽게 확인할 수 있습니다.

## Troubleshooting

Artifact가 기대대로 나오지 않을 때는 다음을 시도합니다.

- Claude가 chat에만 답했다면 "artifact로 만들어줘"라고 명시합니다.
- 결과물이 너무 단순하면 기능, 사용자, format을 더 구체적으로 설명합니다.
- UI가 마음에 들지 않으면 style reference나 tone을 추가합니다.
- 복잡한 artifact는 한 번에 만들지 말고 단계별로 개선합니다.
- artifact가 reuse 목적이라면 export/download 가능한 형태를 요청합니다.

## 실습 질문

다음 질문을 생각해보면 artifact 활용처를 찾기 쉽습니다.

- 반복 업무 중 interactive artifact로 만들면 재사용할 수 있는 것은 무엇인가?
- 내 업무 프로세스 중 flowchart나 diagram으로 보면 더 명확한 것은 무엇인가?
- 빠르게 테스트해보고 싶은 prototype이나 내부 tool이 있는가?
- 문서 template으로 만들어두면 매번 시간을 줄일 수 있는 것은 무엇인가?

## 핵심 정리

Artifacts는 Claude가 만든 결과물을 chat 밖의 독립적인 작업물로 다루게 해줍니다.

- 문서, 코드, HTML page, SVG, diagram, React component를 artifact로 만들 수 있습니다.
- 큰 결과물, 편집 가능한 결과물, 재사용 가능한 결과물이 artifact에 적합합니다.
- artifact 창에서 preview, code, copy, download를 할 수 있습니다.
- 조직 내부 공유나 public publish로 다른 사람과 결과물을 나눌 수 있습니다.
- 좋은 artifact를 만들려면 기능, 사용자, 형식, 제약을 구체적으로 설명해야 합니다.

다음 레슨에서는 Claude에게 반복 가능한 전문 workflow를 가르치는 Skills를 다룹니다.
