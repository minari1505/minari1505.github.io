---
title: "Skills"
title_ko: "Skills로 반복 작업 표준화하기"
course: claude-101
lesson: 6
---

## 학습 목표

- Skills가 무엇이고 Claude가 어떻게 사용하는지 이해하기
- Anthropic의 built-in Skills와 custom Skills를 구분하기
- Settings에서 Skills를 활성화하고 관리하는 방법 익히기
- Projects와 Skills의 차이를 이해하고 함께 쓰는 방법 정리하기

## Skills란?

**Skills**는 Claude가 특정 작업을 더 잘 수행하도록 동적으로 불러오는 instruction, script, resource의 묶음입니다.

쉽게 말하면 Claude에게 특정 업무 방식을 가르치는 **expertise package**입니다. 반복되는 작업을 매번 prompt로 길게 설명하는 대신, Skill 안에 절차와 기준을 넣어두면 Claude가 관련 작업에서 자동으로 활용합니다.

이미 Claude에서 Excel spreadsheet, PowerPoint presentation, Word document, PDF 같은 파일을 만들어본 적이 있다면 Skills를 간접적으로 사용한 것입니다. 이런 document creation 기능은 뒤에서 Skills가 작동해 가능해집니다.

## Skills가 해결하는 문제

반복 업무에는 보통 "우리만의 방식"이 있습니다.

- 분기별 variance analysis를 어떤 순서로 진행할지
- brand voice review에서 무엇을 확인할지
- compliance checklist를 어떤 기준으로 적용할지
- 회의록을 어떤 template으로 정리할지

이런 절차를 매번 Claude에게 설명하면 일관성이 떨어지고 번거롭습니다. Skill은 이런 workflow를 Claude가 재사용할 수 있는 형태로 고정합니다.

```text
반복 업무 방식
  ↓
Skill로 패키징
  ↓
Claude가 관련 작업에서 자동 사용
  ↓
일관된 절차와 결과
```

## Skill의 두 가지 유형

### Anthropic Skills

Anthropic이 만들고 유지관리하는 Skills입니다. 대표적으로 Excel, Word, PowerPoint, PDF 파일 생성과 편집을 강화하는 built-in Skills가 있습니다.

유료 사용자는 별도 설정 없이 관련 요청을 하면 Claude가 자동으로 사용합니다.

예시:

```text
월별 지출을 추적하는 Excel spreadsheet를 만들어줘. 총합 공식도 포함해줘.
```

```text
이 회의록 문서를 PowerPoint presentation으로 바꿔줘.
```

```text
이 데이터를 요약한 PDF report를 만들어줘.
```

### Custom Skills

사용자나 조직이 직접 만드는 Skills입니다. 특정 domain이나 반복 workflow를 Claude에게 가르치는 데 사용합니다.

예시:

- 회사 brand guideline을 presentation에 적용하는 Skill
- 회의록을 사내 template에 맞춰 정리하는 Skill
- 조직의 data analysis workflow를 실행하는 Skill
- 법무 검토 checklist를 적용하는 Skill

Custom Skill의 강점은 Claude가 일반적인 답변이 아니라 **우리 조직의 방식**을 따르게 만들 수 있다는 점입니다.

## Skills 활성화하기

Skills는 현재 Pro, Max, Team, Enterprise plan에서 feature preview로 제공됩니다. Free plan 사용자는 개념을 이해하는 정도로 따라가면 됩니다.

Skills를 사용하려면 Code execution and file creation이 켜져 있어야 합니다. Skills는 Claude의 secure sandboxed computing environment에서 동작하기 때문입니다.

설정 흐름은 다음과 같습니다.

1. `Settings > Capabilities`로 이동합니다.
2. `Code execution and file creation`이 켜져 있는지 확인합니다.
3. `Skills` 섹션으로 이동합니다.
4. 필요한 Skills를 개별적으로 켜거나 끕니다.

Enterprise plan에서는 조직 Owner가 Admin settings에서 Code execution과 Skills를 먼저 활성화해야 individual members가 접근할 수 있습니다. Team plan에서는 feature preview가 조직 수준에서 기본 활성화되어 있습니다.

## 실제로 Skills가 쓰이는 방식

Skills의 장점은 사용자가 보통 Skill을 직접 고르지 않아도 된다는 점입니다. Claude가 요청을 보고 관련 Skill을 자동으로 선택합니다.

예를 들어 다음 요청은 document creation Skills를 호출할 수 있습니다.

```text
월별 expense tracking용 Excel 파일을 만들어줘. category별 합계 공식도 넣어줘.
```

```text
이 meeting notes를 PowerPoint presentation으로 바꿔줘.
```

```text
이 데이터를 요약한 PDF report를 생성해줘.
```

Claude가 Skill을 사용하면 작업 중 chain of thought 영역에서 Skill 사용이 언급될 수 있고, 결과물은 다운로드 가능한 파일로 제공됩니다. Google Drive에 바로 저장할 수도 있습니다.

## File execution

Skills를 통해 Claude는 slides, spreadsheets, contract redlines 같은 실제 파일 작업을 도울 수 있습니다.

Chat에서는 원본을 직접 덮어쓰기보다 새 버전의 문서를 만드는 방식으로 동작합니다. `.xlsx`, `.pptx`, `.docx`, `.pdf` 등을 업로드하면 Claude가 분석, 수정 제안, 새 파일 생성을 수행할 수 있습니다.

외부 데이터 접근이 필요한 경우 제한적 네트워크 접근을 허용하라는 prompt가 표시될 수 있습니다.

![Skills limited network access prompt](/assets/images/courses/claude-101/skills-limited-network-access.png)

## 보안 고려사항

Skills는 executable code를 포함할 수 있으므로 신중하게 사용해야 합니다.

- Custom Skills는 신뢰할 수 있는 source에서만 설치합니다.
- Anthropic built-in Skills는 Anthropic이 테스트하고 유지관리합니다.
- 직접 업로드한 Custom Skills는 개인 계정에 private하게 저장됩니다.
- 외부에서 받은 Custom Skill은 사용 전에 내용을 검토해 무엇을 하는지 이해해야 합니다.

Skills는 강력한 자동화 단위이므로, 편의성만큼 검토와 신뢰도 중요합니다.

## Custom Skill 만들기

Custom Skill은 직접 코드를 작성하지 않아도 Claude와 대화하면서 만들 수 있습니다.

흐름은 다음과 같습니다.

1. 새 chat을 시작하고 만들고 싶은 Skill을 설명합니다.
2. Claude가 workflow를 이해하기 위해 질문합니다.
3. 좋은 결과물의 기준, 사용 상황, 예시를 알려줍니다.
4. template, style guide, brand asset, 참고 자료를 업로드합니다.
5. Claude가 Skill 구조를 생성합니다.
6. 생성된 파일을 저장하면 Skill로 사용할 수 있습니다.

예시 prompt:

```text
분기별 비즈니스 리뷰를 작성하는 Skill을 만들고 싶어.
```

```text
우리 회사 brand guideline을 presentation에 적용하는 Skill이 필요해.
```

만든 Skill은 left sidebar의 `Customize` 탭에서 확인할 수 있습니다. Claude와 대화하거나 수동으로 내용을 편집해 Skill을 개선할 수도 있습니다.

## Skills vs Projects

Projects와 Skills는 모두 Claude에게 더 많은 context를 주지만 목적이 다릅니다.

간단히 말하면:

```text
Projects store knowledge.
Skills perform tasks.
```

| 구분 | Projects | Skills |
|---|---|---|
| 목적 | Claude가 참조할 지식 저장 | Claude가 실행할 절차 정의 |
| 적합한 경우 | 장기 context, reference material, team collaboration | 반복 workflow, multi-step task, 일관된 methodology |
| 예시 | 고객 hub, research workspace, feedback generator | brand/legal guideline, blog drafting, PDF creation |
| 지속성 | project 안의 모든 chat에서 knowledge 사용 | Skill이 호출될 때 instruction 적용 |

Projects는 "무엇을 알아야 하는가"에 가깝고, Skills는 "어떻게 해야 하는가"에 가깝습니다.

둘은 함께 사용할 수 있습니다. 예를 들어 `customer call prep` Skill은 project knowledge base에 저장된 고객 profile을 참조해 call 준비 workflow를 실행할 수 있습니다.

```text
Project = 고객 정보와 과거 자료
Skill = 고객 미팅 준비 절차
```

## 실습 질문

다음 질문을 생각해보면 Skills의 활용처를 찾기 쉽습니다.

- 내가 정기적으로 만드는 문서 중 Claude built-in Skills가 도움이 될 만한 것은 무엇인가?
- 반복되는 업무 절차 중 custom Skill로 만들면 좋은 것은 무엇인가?
- 문서 생성이나 데이터 분석 업무에서 매번 설명하는 규칙은 무엇인가?
- Projects와 Skills를 함께 쓰면 더 좋아질 workflow는 무엇인가?

## 핵심 정리

Skills는 Claude에게 반복 가능한 전문 workflow를 가르치는 패키지입니다.

- Anthropic Skills는 문서 생성과 파일 작업 같은 built-in 기능을 제공합니다.
- Custom Skills는 조직이나 개인의 특정 workflow를 Claude에게 가르칩니다.
- Skills는 관련 요청에서 Claude가 자동으로 선택해 사용할 수 있습니다.
- executable code를 포함할 수 있으므로 신뢰할 수 있는 source와 내용 검토가 중요합니다.
- Projects는 지식을 저장하고, Skills는 절차를 실행합니다.

다음 레슨부터는 connectors를 통해 Claude의 reach를 확장하는 방법을 살펴봅니다.
