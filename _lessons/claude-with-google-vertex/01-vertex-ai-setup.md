---
title: "Vertex AI Setup"
title_ko: "Vertex AI 설정과 인증"
course: claude-with-google-vertex
lesson: 1
---

> 이 정리는 **Google Cloud Vertex AI 고유 부분**(설정·인증·접속·요청)만 다뤄요. 프롬프트 엔지니어링·eval·tool use·RAG·에이전트 등 일반 API 개념은 [Building with the Claude API](/courses/claude-with-the-anthropic-api/01-introduction/) 코스와 겹쳐서 여기서는 생략해요.

## 학습 목표

- Vertex AI에서 **Anthropic 모델을 활성화**하기
- **gcloud CLI로 인증**을 설정해 Anthropic SDK가 Vertex에 접근하게 하기

## 한눈에 보는 흐름

<ol class="oa-flow">
  <li><strong>모델 찾기</strong>Model Garden에서 Anthropic 검색</li>
  <li><strong>Enable</strong>모델 사용 활성화</li>
  <li><strong>gcloud CLI 설치</strong>아직 없다면</li>
  <li><strong>로그인 · 인증</strong>프로젝트와 기본 자격증명 설정</li>
</ol>

이 레슨을 한 문장으로 줄이면: **콘솔에서 모델을 켜고, gcloud로 한 번 로그인해 두면 SDK가 그 자격증명을 알아서 써요.**

## 1. Vertex에서 Anthropic 모델 켜기

<div class="oa-eli">
  <span class="oa-eli__head">🏋️ 쉽게 말하면</span>
  <p>헬스장에 등록했어도 <b>PT는 따로 신청</b>해야 쓸 수 있죠. Google Cloud 계정이 있어도 Claude 모델은 Model Garden에서 따로 켜야 해요.</p>
</div>

**Step 1 — Anthropic 모델 찾기**

<ol class="oa-flow">
  <li><strong>콘솔 열기</strong>console.cloud.google.com/vertex-ai/dashboard</li>
  <li><strong>Model Garden</strong>왼쪽 내비게이션에서 클릭</li>
  <li><strong>검색</strong>Search models 박스에 Anthropic 입력</li>
  <li><strong>모델 선택</strong>사용할 모델 클릭</li>
</ol>

**Step 2 — 모델 Enable**

모델 정보 페이지에서 **Enable** 버튼을 클릭해요.

<div class="oa-vs">
  <div class="oa-vs__bad">
    <span class="oa-vs__label">🔘 Enable 버튼이 보이면</span>
    <ul>
      <li>아직 활성화 전이에요 → 클릭해서 켜요</li>
    </ul>
  </div>
  <div class="oa-vs__good">
    <span class="oa-vs__label">✅ Enable 버튼이 안 보이면</span>
    <ul>
      <li><b>이미 접근 권한이 있는 것</b>이에요 → 그대로 진행</li>
    </ul>
  </div>
</div>

## 2. gcloud CLI로 인증하기

<div class="oa-eli">
  <span class="oa-eli__head">🪪 쉽게 말하면</span>
  <p>건물 로비에서 <b>출입증을 한 번 발급</b>받으면 그 뒤로는 문마다 알아서 열리죠. gcloud 로그인이 그 출입증 발급이고, 이후에는 SDK가 출입증을 알아서 꺼내 써요.</p>
</div>

**Step 3 — gcloud CLI 설치**

gcloud CLI가 없다면 `https://cloud.google.com/sdk/docs/install` 안내를 따라 설치하고 인증해요.

**Step 4 — 로그인과 인증 설정**

아직 gcloud CLI에 로그인하지 않았다면:

```bash
gcloud init
gcloud auth login
```

그다음 프로젝트 ID와 기본 자격증명(application-default)을 설정해요.

```bash
gcloud config set project YOUR_PROJECT_ID
gcloud auth application-default login
```

| 명령 | 하는 일 |
|---|---|
| `gcloud init` | gcloud CLI 초기 설정 |
| `gcloud auth login` | 내 Google 계정으로 로그인 |
| `gcloud config set project YOUR_PROJECT_ID` | 사용할 프로젝트 지정 |
| `gcloud auth application-default login` | 애플리케이션이 쓸 **기본 자격증명** 설정 |

이걸로 끝이에요. **Anthropic SDK가 Vertex에 접근할 때 이 자격증명을 자동으로 사용**해요.

## 확인 문제

<details class="oa-quiz">
  <summary>Q1. Vertex 콘솔에서 Anthropic 모델을 찾는 곳은?</summary>
  <p>① IAM ② <b>Model Garden</b> ③ Cloud Storage ④ BigQuery</p>
  <p><b>정답: ②</b> — Model Garden의 Search models에 "Anthropic"을 입력해요.</p>
</details>

<details class="oa-quiz">
  <summary>Q2. 모델 정보 페이지에 Enable 버튼이 보이지 않는다. 무슨 뜻인가요?</summary>
  <p><b>이미 그 모델에 접근 권한이 있다</b>는 뜻이에요. 따로 할 일이 없어요.</p>
</details>

<details class="oa-quiz">
  <summary>Q3. Anthropic SDK가 자동으로 사용하는 자격증명을 설정하는 명령은?</summary>
  <p>① <code>gcloud init</code> ② <code>gcloud auth login</code> ③ <b><code>gcloud auth application-default login</code></b> ④ <code>gcloud config list</code></p>
  <p><b>정답: ③</b> — 기본 자격증명(application-default)을 설정하면 SDK가 그것을 써요.</p>
</details>

## 정리

- Vertex 콘솔의 **Model Garden**에서 사용할 Anthropic 모델을 검색해 **Enable** 해요.
- Enable 버튼이 없으면 **이미 접근 권한이 있는 것**이에요.
- **gcloud CLI**로 로그인하고 `gcloud config set project`로 프로젝트를 지정해요.
- `gcloud auth application-default login`으로 기본 자격증명을 설정하면, 이후 **Anthropic SDK가 자동으로 사용**해요.
