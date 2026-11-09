---
title: "Deploying ChatGPT Practice Application"
title_ko: "ChatGPT 배포 실전 연습 — COO와의 대화로 첫 롤아웃 결정하기"
course: chatgpt-deployment
lesson: 8
---

## 학습 목표

- 넓은 출시 야망을 **첫 워크플로 진입점·첫 사용자 그룹·성공 신호**로 좁히기
- 여러 이해관계자의 우려 속에서 **가장 중요한 준비도·거버넌스 공백** 하나 고르기
- 첫 코호트를 위한 **도입 지원**과 **확장 조건** 제안하기
- 대화 안에서 간결한 **최종 배포 권고** 말하기
- 레슨 1–7의 흐름을 **하나의 배포 패턴**으로 묶어 보기

## 한눈에 보는 코스 흐름

<ol class="oa-flow">
  <li><strong>활동 1</strong>첫 배포 과제·활성화 우선순위 명확히</li>
  <li><strong>활동 2</strong>가장 중요한 준비도·거버넌스 공백</li>
  <li><strong>활동 3</strong>도입 지원과 확장 조건 → 최종 권고</li>
  <li><strong>디브리프</strong>핵심 배포 패턴</li>
</ol>

## 1. 이 코스는 어떻게 진행되나

이 코스는 기술 구현이 아니라 **가이드된 롤플레이 연습**이에요. 학습자는 **파트너 배포 리드**가 되어, 가상의 고객사 **Kestrel Manufacturing**의 COO **Evan Brooks**(배포 이니셔티브의 임원 스폰서, AI 캐릭터)와 세 번 이어지는 대화를 해요.

| 항목 | 내용 |
|---|---|
| 고객 | 북미·유럽 12개국, 약 18,000명, 여러 사업부를 가진 글로벌 제조·산업 서비스 기업 (가상) |
| 상황 | 지식 근로자 약 **1,000명** 초기 그룹용 ChatGPT Enterprise 라이선스 구매 (강의 기준) |
| 과제 | 구매한 라이선스 → **집중되고, 지원받고, 측정 가능한** 첫 롤아웃 |
| 주 워크플로 | 운영팀의 **내부 지식 검색·절차 요약** |
| 부차 압력 | HR 정책 요약, 고객 대면 응답 초안 — Evan이 분명한 증거를 주지 않는 한 첫 우선순위가 아님 |
| 경계 | 배포 **계획** 수준 유지. 기술 설정·법무 정책·관리자 셋업·전사 변혁 계획을 설계하지 않음 |

<div class="oa-eli">
  <span class="oa-eli__head">🧑‍⚕️ 쉽게 말하면</span>
  <p>좋은 의사는 환자가 증상 10개를 말해도 한 번에 다 고치려 하지 않아요. <b>지금 가장 중요한 것 하나</b>를 골라 처방하고, "다음 진료 때 이걸 확인해요"라고 말하죠. 롤플레이의 요령도 같아요 — <b>매 답변을 짧게, 상대의 최신 우려에 맞춰, 첫 워크플로에 닻을 내리고</b>.</p>
</div>

대화 요령: 설정을 먼저 읽기 · 파트너 배포 리드로 답하기 · 간결하고 고객 중심으로 · 한 번에 모든 문제를 풀려 하지 않기 · Evan의 **최신 우려**를 따라가기 · 첫 워크플로 진입점에 고정.

## 2. 활동 1 — 첫 배포 과제와 활성화 우선순위

Evan은 빨리 가치를 보이고 싶고, 운영 외에도 여러 부서가 관심을 보여요. 목표는 넓은 야망을 **첫 배포 결정**으로 바꾸는 것.

| 준비할 답 | 연습용 답변 방향 (예시) |
|---|---|
| 주 배포 목표 | 운영팀이 필요한 절차 지식을 더 빠르고 일관되게 찾고 쓰도록 |
| 첫 워크플로 진입점 | 승인된 SOP에서 **내부 지식 검색 + 사람이 검토하는 절차 요약** |
| 첫 사용자 그룹 | 1,000명 전체가 아니라 **운영팀의 첫 코호트** |
| 실용적 성공 신호 | 코호트가 승인 소스로 요약을 만들고, 지정 검토자가 사용 가능하다고 확인 |
| 출시 전 열린 검증 질문 | 승인 SOP 세트와 그 소스 오너가 확정됐나? |
| 왜 좁게 시작? | HR·고객 대면은 새 소스·검토·리스크 오너가 필요 — 더 강한 증거가 생기면 다시 검토 |

> 레슨 1(기회 → 배포 계획)과 레슨 4(활성화 유스케이스 설계)의 판단을 **대화 속에서** 쓰는 연습이에요.

## 3. 활동 2 — 가장 중요한 준비도·거버넌스 공백

이제 운영 워크플로가 출발점이에요. 첫 활성화는 **Chat**(강의 기준) — 운영 사용자가 승인 SOP를 찾아 대화형으로 요약을 만들고 사람이 검토해요. **Workspace Agent는 나중의 확장 후보**일 뿐 첫 활성화에 포함되지 않아요.

Evan이 두 사람의 우려를 전해요(별도 AI 캐릭터 아님).

<div class="oa-vs">
  <div class="oa-vs__good">
    <span class="oa-vs__label">🛠️ Dana Reyes · Enterprise Technology 디렉터</span>
    <ul>
      <li>워크스페이스 준비도, 오너십</li>
      <li>승인된 지식 소스</li>
      <li>출시 의존성</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">🛡️ Robert King · 최고 리스크 책임자</span>
    <ul>
      <li>거버넌스, 데이터 처리 경계</li>
      <li>검토 기대, 피드백 경로</li>
      <li>이슈 에스컬레이션, 미해결 리스크 오너</li>
    </ul>
  </div>
</div>

### 함정: "Agent도 같이 올리자"

Dana에 따르면 한 운영팀이 이 워크플로용 **Workspace Agent**를 게시하고 싶어 해요. App으로 승인 SOP를 가져오고 초안 요약을 공유 운영 채널에 올릴 수도 있는데 — 누가 쓰는지, App이 **사용자 계정 vs 에이전트 소유 공유 연결** 중 무엇을 쓰는지, 허용 읽기·쓰기 행동, 승인이 필요한 시점, 출력 검토자, 예상 밖 행동·접근 문제 에스컬레이션이 **모두 미합의**예요.

<div class="oa-svg">
<svg viewBox="0 0 720 210" role="img" aria-label="첫 활성화 범위와 나중 확장 후보 구분">
  <defs>
    <style>.g8a{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.w8a{fill:#fff5f3;stroke:#f3cfc6;stroke-width:1.5;stroke-dasharray:5 4}.t8a{font-size:13px;fill:#4a4a4a;font-weight:bold}.s8a{font-size:11px;fill:#8a8a8a}</style>
  </defs>
  <rect x="10" y="20" width="340" height="170" rx="12" class="g8a"/>
  <text x="180" y="46" text-anchor="middle" class="t8a">✅ 첫 활성화 (지금)</text>
  <text x="30" y="76" class="s8a">· Chat에서 승인 SOP 검색·요약</text>
  <text x="30" y="98" class="s8a">· 사람이 검토 후 사용</text>
  <text x="30" y="120" class="s8a">· 출시 전: 승인 소스 세트·소스 오너 확정</text>
  <text x="30" y="142" class="s8a">· 검토 오너·피드백·에스컬레이션 경로</text>
  <text x="30" y="164" class="s8a">· 진행하면서 모니터링할 항목은 따로 표시</text>
  <rect x="370" y="20" width="340" height="170" rx="12" class="w8a"/>
  <text x="540" y="46" text-anchor="middle" class="t8a">⏸️ 나중 확장 후보 (보류)</text>
  <text x="390" y="76" class="s8a">· Workspace Agent + App 연결</text>
  <text x="390" y="98" class="s8a">· 공유 채널에 자동 게시</text>
  <text x="390" y="120" class="s8a">· 미합의: 사용자·연결 방식·읽기/쓰기</text>
  <text x="390" y="142" class="s8a">  승인 시점·검토자·에스컬레이션</text>
  <text x="390" y="164" class="s8a">· 자체 준비 리뷰 후 재검토</text>
</svg>
</div>

목표는 모든 의존성 해결이 아니라, **첫 워크플로가 책임 있게 출시될 수 있는지를 가장 크게 좌우하는 공백**을 찾고 다음 행동을 정하는 것.

| 구분 | 예시 |
|---|---|
| **출시 전 소유·검증·검토·에스컬레이션할 것** | 승인 SOP 세트와 소스 오너, 출력 검토 오너, 데이터 처리 경계, 이슈 에스컬레이션 경로 |
| **롤아웃 중 모니터링할 것** | 소스 공백 신고, 검토 완료율, 반복 질문 패턴 |
| **다음 단계 오너** | 소스·워크스페이스 준비 → 기술 쪽 오너 / 검토·경계·리스크 → 리스크 쪽 오너 / 결정 → 스폰서 |

결론은 셋 중 하나로: **진행 · 좁히기 · 특정 준비 조치 후 출시** (레슨 1·3의 결정 게이트와 같은 사고방식).

## 4. 활동 3 — 도입 지원과 확장 조건

먼저 **출시 범위와 아직 확인이 필요한 요구사항**을 짧게 말한 뒤, 그 범위에 맞는 도입 지원을 계획해요. 새 이해관계자는 등장하지 않고 **하나의 임원 결정 지점**으로 다뤄요.

### 리허설 증거 읽기 (합성 데이터, 강의 기준)

<div class="oa-cards">
  <div class="oa-card"><span class="oa-card__icon">✅</span><span class="oa-card__title">완료 항목</span>지정 검토자가 승인 소스와 대조한 절차 요약 1건 — 파일럿 리드에게 적합</div>
  <div class="oa-card oa-card--warn"><span class="oa-card__icon">⚠️</span><span class="oa-card__title">예외</span>필수 SOP 소스 접근 실패로 두 번째 요약은 <b>미완성</b></div>
  <div class="oa-card"><span class="oa-card__icon">❓</span><span class="oa-card__title">열린 결정</span>검토된 요약을 지역 매니저와 공유 + 정기 후속? 대상·지속 필요성·오너 미합의</div>
</div>

<div class="oa-eli">
  <span class="oa-eli__head">📝 쉽게 말하면</span>
  <p>시험 대비 모의고사 3장 중 1장은 채점까지 끝났고, 1장은 문제지가 찢어져 못 풀었고, 1장은 "반 전체에 돌릴까?" 고민 중이라면 — "3장 다 했다!"가 아니라 <b>"1장은 쓸 만, 1장은 문제지부터 고치기, 1장은 아직 결정 안 함"</b>이라고 정확히 말해야 해요. 이 기록은 출시나 앞선 준비 요건 해결을 <b>증명하지 않아요</b>.</p>
</div>

### 확장 옵션 — 단계가 아니라 서로 다른 선택지 (강의 기준)

| 옵션 | 언제 맞나 |
|---|---|
| **Space**의 공유 출력 | 검토된 결과를 정해진 대상과 공유해야 할 때 |
| **dot** 기반 개인의 지속 책임 | 한 사람이 계속 챙기는 일일 때 |
| **Team Tasks**의 팀 소유 반복 프로세스 | 팀이 소유하는 정기 작업일 때 |
| **Chat 유지 + 확장 보류** | 위의 필요·접근·대상·오너·검토·가용성을 아직 정할 수 없을 때 — **똑같이 타당한 답** |

> 세 가지는 "필수 롤아웃 3단계"가 아니에요. **필요 · 접근 · 대상 · 오너 · 검토 방식 · 가용성**을 정할 수 있을 때만 하나를 권해요.

### 대화에서 다룰 것

첫 사용자 그룹을 어떻게 지원할지(레슨 5) · 주의할 도입·사용 리스크 · 검토할 초기 증거(레슨 6) · 확장 전 참이어야 할 조건(레슨 7) · 모니터링·피드백·에스컬레이션·개선의 오너. 끝에 Evan이 **최종 배포 권고**를 요청하면 별도 문서가 아니라 **대화 안에서** 말해요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">❌ 약한 최종 권고</span>
    <ul>
      <li>"1,000명 전체에 열고, Agent도 만들고, HR과 고객 응대도 같이 시작하죠."</li>
      <li>범위·오너·증거·확장 조건이 없음</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ 강한 최종 권고 (예시 구조)</span>
    <ul>
      <li>운영 첫 코호트가 Chat에서 승인 SOP로 사람 검토 요약을 만든다.</li>
      <li>출시 전: 소스 세트·소스 오너·검토 오너·에스컬레이션 경로 확정. Agent는 보류.</li>
      <li>지원: 역할별 데모, 짧은 가이드, 챔피언 채널, 첫 2주 오피스아워.</li>
      <li>증거: 검토된 사용 가능 결과 vs 미완성·실패 작업을 구분, 접근 실패는 오너에게.</li>
      <li>확장 조건: 공유 출력 접근·예외 담당·확장안의 오너와 검토가 정해질 때 → 범위 내 계속 / 표적 조정 / 확장 전 일시중지.</li>
    </ul>
  </div>
</div>

## 5. 디브리프 — 핵심 배포 패턴

가장 강한 권고는 **가장 넓은 롤아웃 계획이 아니라 가장 명확한 다음 배포 결정**이에요.

<ol class="oa-flow">
  <li><strong>배포 과제</strong>무엇을 이루려 하나</li>
  <li><strong>첫 워크플로 진입점</strong>업무 순간</li>
  <li><strong>첫 사용자 그룹</strong>코호트</li>
  <li><strong>준비도·거버넌스 공백</strong>가장 중요한 하나</li>
  <li><strong>도입 지원</strong>일상 업무 속 지원</li>
  <li><strong>성공 신호</strong>유용하고 책임 있는가</li>
  <li><strong>확장 조건</strong>무엇이 참이어야</li>
  <li><strong>실용적 다음 단계</strong>오너 · 시점</li>
</ol>

새 우려가 나와도 **별도 워크스트림으로 만들지 말고 같은 배포 결정을 다듬는 데** 써요. 확장을 권하기 전에는 검토된 사용 가능 결과와 미완성·실패 작업을 구분하고, 공유 출력 접근·예외 처리 담당·확장안의 오너와 검토 프로세스를 확인한 뒤 **범위 내 계속 · 표적 조정 · 확장 전 일시중지** 중 하나를 권해요.

## 6. 프로그램 총정리 — 레슨 1–7을 한 장으로

<div class="oa-svg">
<svg viewBox="0 0 720 420" role="img" aria-label="ChatGPT Deployment Practitioner 레슨 1부터 7까지의 산출물 흐름">
  <defs>
    <marker id="ar8r" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto"><path d="M0,0 L6,3 L0,6 Z" fill="#c99400"/></marker>
    <style>.p8r{fill:#f6f3ea;stroke:#e5dfcf;stroke-width:1.5}.q8r{fill:#fffdf5;stroke:#f1e4b8;stroke-width:1.5}.g8r{fill:#f4fbf3;stroke:#cfe8c9;stroke-width:1.5}.n8r{font-size:11px;fill:#c99400;font-weight:bold}.t8r{font-size:12px;fill:#4a4a4a;font-weight:bold}.s8r{font-size:11px;fill:#8a8a8a}.h8r{font-size:12px;fill:#8a8a8a;font-weight:bold}</style>
  </defs>
  <text x="10" y="18" class="h8r">계획 (Plan)</text>
  <rect x="10" y="26" width="220" height="78" rx="10" class="p8r"/><text x="22" y="44" class="n8r">레슨 1</text><text x="22" y="62" class="t8r">기회 → 배포 계획</text><text x="22" y="80" class="s8r">준비도 결정 · 롤아웃 구조</text><text x="22" y="96" class="s8r">→ Deployment Plan</text>
  <line x1="230" y1="65" x2="246" y2="65" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar8r)"/>
  <rect x="250" y="26" width="220" height="78" rx="10" class="p8r"/><text x="262" y="44" class="n8r">레슨 2</text><text x="262" y="62" class="t8r">성공 계획</text><text x="262" y="80" class="s8r">목표 · 지표 · 30일 출시 계획</text><text x="262" y="96" class="s8r">→ Success Plan</text>
  <line x1="470" y1="65" x2="486" y2="65" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar8r)"/>
  <rect x="490" y="26" width="220" height="78" rx="10" class="p8r"/><text x="502" y="44" class="n8r">레슨 3</text><text x="502" y="62" class="t8r">워크스페이스 준비</text><text x="502" y="80" class="s8r">접근 · 소스 · 앱 · 권한 · 거버넌스</text><text x="502" y="96" class="s8r">→ Ready / Blocked / Deferred</text>
  <line x1="600" y1="104" x2="600" y2="132" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar8r)"/>
  <text x="10" y="128" class="h8r">활성화·도입 (Activate &amp; Adopt)</text>
  <rect x="490" y="136" width="220" height="78" rx="10" class="q8r"/><text x="502" y="154" class="n8r">레슨 4</text><text x="502" y="172" class="t8r">활성화 유스케이스</text><text x="502" y="190" class="s8r">가치 × 준비도 × 의존성</text><text x="502" y="206" class="s8r">→ Use Case Activation Plan</text>
  <line x1="490" y1="175" x2="474" y2="175" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar8r)"/>
  <rect x="250" y="136" width="220" height="78" rx="10" class="q8r"/><text x="262" y="154" class="n8r">레슨 5</text><text x="262" y="172" class="t8r">사용자 지원·도입</text><text x="262" y="190" class="s8r">교육 · 커뮤니케이션 · 챔피언</text><text x="262" y="206" class="s8r">→ Enablement &amp; Adoption Plan</text>
  <line x1="250" y1="175" x2="234" y2="175" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar8r)"/>
  <rect x="10" y="136" width="220" height="78" rx="10" class="q8r"/><text x="22" y="154" class="n8r">레슨 6</text><text x="22" y="172" class="t8r">활성화·측정·확장</text><text x="22" y="190" class="s8r">도입 · 가치 · 한계 → 4가지 결정</text><text x="22" y="206" class="s8r">→ Activation Review</text>
  <line x1="120" y1="214" x2="120" y2="242" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar8r)"/>
  <text x="10" y="238" class="h8r">확장 (Scale)</text>
  <rect x="10" y="246" width="460" height="64" rx="10" class="g8r"/><text x="22" y="264" class="n8r">레슨 7</text><text x="22" y="282" class="t8r">조직 전체 확장</text><text x="22" y="300" class="s8r">우선순위 · 오너십·거버넌스 · 반복 가능한 지원 · 리스크 → Scaling Strategy</text>
  <line x1="470" y1="278" x2="486" y2="278" stroke="#c99400" stroke-width="1.5" marker-end="url(#ar8r)"/>
  <rect x="490" y="246" width="220" height="64" rx="10" class="g8r"/><text x="502" y="264" class="n8r">레슨 8</text><text x="502" y="282" class="t8r">실전 대화 연습</text><text x="502" y="300" class="s8r">하나의 명확한 다음 결정</text>
  <rect x="10" y="334" width="700" height="72" rx="10" class="p8r"/>
  <text x="360" y="356" text-anchor="middle" class="t8r">모든 레슨을 관통하는 질문</text>
  <text x="360" y="378" text-anchor="middle" class="s8r">어떤 워크플로 · 누가 먼저 · 무엇이 준비돼야 · 누가 소유·검토·에스컬레이션 · 어떤 증거로</text>
  <text x="360" y="396" text-anchor="middle" class="s8r">계속 · 조정 · 확장 · 중단 중 무엇을 고르나</text>
</svg>
</div>

| 레슨 | 핵심 질문 | 대표 산출물 | 핵심 결정 |
|---|---|---|---|
| 1 기회 → 배포 계획 | 이 기회는 배포할 준비가 됐나? | Deployment Plan | 진행 · 집중 검증 · 재범위 · no-go |
| 2 성공 계획 | 무엇을 성공으로 볼까? | Success Plan · 30일 계획 | 목표·지표·오너·리뷰 시점 |
| 3 워크스페이스 준비 | 사용자·소스·앱·권한이 준비됐나? | 준비도 체크 | Ready · Blocked · Deferred |
| 4 활성화 유스케이스 | 무엇을 먼저 켤까? | Use Case Activation Plan | 활성화 · 좁히기 · 보류 · no-go |
| 5 사용자 지원·도입 | 사람들이 실제 업무에 쓰게 하려면? | Enablement &amp; Adoption Plan | 대상별 활동·메시지·챔피언·지원 |
| 6 활성화·측정·확장 | 증거가 무엇을 말하나? | Activation Review &amp; Expansion Plan | 계속 · 조정 · 확장 · 중단 |
| 7 조직 확장 | 어디로, 어떤 조건에서 넓힐까? | Organizational Scaling Strategy | 단계적 · 반복·확장 · 조정 후 · 보류 |
| 8 실전 연습 | 대화 속에서 무엇을 권할까? | 대화 안의 최종 권고 | 범위 내 계속 · 표적 조정 · 확장 전 일시중지 |

## 스스로 점검 (이 코스에는 객관식 확인 문제가 없어 디브리프 기준으로 구성)

<details class="oa-quiz">
  <summary>Q1. Evan이 "HR과 고객 응대 팀도 빨리 시작하고 싶어 해요"라고 하면?</summary>
  <p><b>방향:</b> 운영 워크플로를 첫 진입점으로 유지하고, HR·고객 대면은 새 소스·검토·리스크 오너가 필요하므로 <b>더 강한 증거가 생기면 별도 준비 리뷰</b>로 다룬다고 말한다. 새 우려를 별도 워크스트림으로 만들지 않는다.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 한 운영팀이 Workspace Agent를 바로 게시하고 싶어 한다. 어떻게 답할까?</summary>
  <p><b>방향:</b> 첫 활성화는 Chat으로 유지하고 Agent는 <b>나중 확장 후보로 보류</b>. 사용 대상, 사용자 계정 vs 공유 연결, 허용 읽기·쓰기, 승인 시점, 검토자, 예외 에스컬레이션이 합의된 뒤 자체 준비 리뷰를 거친다.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. 리허설 기록에 "검토 완료 1건, 소스 접근 실패로 미완성 1건, 공유 여부 미결정 1건"이 있다. 확장해도 될까?</summary>
  <p><b>방향:</b> 아직은 아니다. 사용 가능한 결과와 <b>미완성·실패 작업을 구분</b>하고, 접근 실패는 오너에게 넘기고, 공유 출력의 대상·오너·검토를 정한 뒤 판단한다. 이 기록은 출시나 준비 요건 해결을 증명하지 않는다.</p>
</details>

<details class="oa-quiz">
  <summary>Q4. Space · dot · Team Tasks 중 무엇을 권해야 할까?</summary>
  <p><b>방향:</b> 셋은 필수 단계가 아니라 다른 선택지. 필요·접근·대상·오너·검토·가용성을 정할 수 있을 때만 하나를 권하고, 그렇지 않으면 <b>Chat 유지 + 확장 보류</b>도 똑같이 타당한 답이다.</p>
</details>

## 정리

- 이 코스는 기술 구현이 아니라 **배포 판단을 대화로 연습**하는 롤플레이다. 계획 수준을 지키고, 짧게, 상대의 최신 우려에 맞춰 답한다.
- 활동 1: 넓은 야망 → **첫 워크플로 · 첫 코호트 · 성공 신호 · 열린 검증 질문**.
- 활동 2: 여러 우려 중 **첫 출시를 가장 좌우하는 공백 하나** + 출시 전 해결 vs 롤아웃 중 모니터링 + 다음 단계 오너. 준비 안 된 Agent는 보류.
- 활동 3: 범위·남은 요구를 먼저 말하고, 지원·리스크·증거·확장 조건·오너를 정리해 **대화 안에서** 최종 권고.
- 가장 강한 권고는 **가장 넓은 계획이 아니라 가장 명확한 다음 결정**. 배포 과제 → 진입점 → 코호트 → 공백 → 지원 → 신호 → 확장 조건 → 다음 단계.
- 레슨 1–7의 모든 산출물은 같은 질문으로 이어진다: **어떤 워크플로, 누가, 무엇이 준비돼야, 누가 소유하고, 어떤 증거로 무엇을 결정하나.**
