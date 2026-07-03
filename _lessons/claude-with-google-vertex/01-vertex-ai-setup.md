---
title: "Vertex AI Setup"
title_ko: "Vertex AI 설정과 인증"
course: claude-with-google-vertex
lesson: 1
---

> 이 정리는 **Google Cloud Vertex AI 고유 부분**(설정·인증·접속·요청)만 다룹니다. 프롬프트 엔지니어링·eval·tool use·RAG·에이전트 등 일반 API 개념은 [Building with the Claude API](/courses/claude-with-the-anthropic-api/01-introduction/) 코스와 겹쳐 여기서는 생략합니다.

## 학습 목표

- Vertex AI에서 Anthropic 모델을 활성화하기
- gcloud CLI로 인증을 설정해 Anthropic SDK가 Vertex에 접근하게 하기

## Step 1 — Vertex에서 Anthropic 모델 활성화

1. 브라우저에서 `https://console.cloud.google.com/vertex-ai/dashboard` 로 이동
2. 왼쪽 내비게이션에서 **Model Garden** 클릭
3. **Search models** 박스에 `Anthropic` 입력
4. 사용할 모델 클릭

## Step 2 — 모델 Enable

모델 정보 페이지에서 **Enable** 버튼을 클릭합니다. **Enable 버튼이 안 보이면 이미 접근 권한이 있는 것**입니다.

## Step 3 — gcloud CLI 설치

gcloud CLI가 없다면 `https://cloud.google.com/sdk/docs/install` 안내를 따라 설치·인증합니다.

## Step 4 — 로그인과 인증 설정

```bash
gcloud init
gcloud auth login
```

그다음 프로젝트 ID와 기본 자격증명(application-default)을 설정합니다.

```bash
gcloud config set project YOUR_PROJECT_ID
gcloud auth application-default login
```

이걸로 끝입니다 — **Anthropic SDK가 Vertex에 접근할 때 이 자격증명을 자동으로 사용**합니다.

## 핵심 정리

- Vertex 콘솔의 **Model Garden**에서 사용할 Anthropic 모델을 검색해 **Enable** 합니다.
- **gcloud CLI**로 로그인하고 `gcloud auth application-default login`으로 기본 자격증명을 설정합니다.
- 이후 Anthropic SDK가 이 자격증명을 자동 사용하므로 별도 API key 관리가 필요 없습니다.
