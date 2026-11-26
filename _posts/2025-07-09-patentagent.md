---
title: "PatentAgent: Intelligent Agent for Automated Pharmaceutical Patent Analysis"
date: 2025-07-09
categories:
  - AI for Science
tags:
  - Agent
  - Document AI
  - OCR
  - Patent
toc: true
toc_sticky: true
excerpt: "특허 문서 QA, 구조식 이미지 인식, 핵심 구조 식별을 하나의 에이전트로 묶은 PatentAgent"
---

> 📄 **Paper**: [arXiv:2410.21312](https://arxiv.org/abs/2410.21312)

## 3줄 요약

- PatentAgent는 제약 특허 분석을 자동화하는 시스템으로, **특허 QA · 구조식 이미지 인식 · 핵심 구조 식별** 세 가지를 해요.
- **PA-Img2Mol**은 특허 속 구조식 이미지를 문자열 표현으로 바꾸고, GPT-4o로 변환 결과를 검증해요.
- **PA-CoreId**는 머신러닝으로 특허 속 여러 화합물의 공통 핵심 구조를 찾아요.

## 배경

특허 검토를 주제로 PoC를 진행하면서, 특허 문서에서 필요한 정보를 정리하는 기능을 찾고 있었어요. 그러다 제약 특허 분석을 자동화한 PatentAgent를 발견했고, 그림과 관련 정보를 어떻게 구조화했는지 알고 싶어서 리뷰했어요.

## 구조

![PatentAgent framework]({{ '/assets/images/papers/patentagent/framework.png' | relative_url }})

```text
                 ┌─▶ PA-QA       : 특허 문서 기반 질의응답
특허 문서 ──────▶ ├─▶ PA-Img2Mol  : 구조식 이미지 → 문자열 표현
                 └─▶ PA-CoreId   : 여러 화합물의 공통 구조 식별
```

### 1. PA-QA

- 자연어 질문에 답하는 QA 챗봇이에요.
- 질문이 들어오면 특허의 특정 섹션(초록, 청구항, 상세 설명 등)에 집중해요. 이때 객체 탐지와 OCR로 관련 섹션을 뽑아요.
- 뽑은 섹션 안에서 transformer 기반 retrieval 모델로 semantic search를 해서 답을 만들어요. 구조만 보면 **문서 레이아웃 파싱 + RAG**예요.

### 2. PA-Img2Mol

![PA-Img2Mol 파이프라인]({{ '/assets/images/papers/patentagent/img2mol.png' | relative_url }})

구조식 이미지를 기계가 읽을 수 있는 문자열로 바꾸는 **광학 구조 인식(OCSR)** 모듈이에요. 파이프라인이 흥미로워요.

1. 이미지 탐지 모델(MolDetect)로 특허에서 구조식 그림을 잘라내요.
2. **세 개의 서로 다른 인식 모델**(DECIMER, MolScribe, SwinOCSR)로 각각 변환해요.
3. 결과 문자열을 표준 형식으로 정규화해요.
4. 문자열을 다시 그림으로 렌더링해요.
5. **GPT-4o가 원본 이미지와 렌더링 결과를 비교**해서 가장 잘 맞는 후보를 골라요.

여러 모델의 결과를 **다시 그려서 원본과 대조**하는 방식이라, 단일 OCR 모델보다 오류에 강해요. 표·도면 OCR 같은 다른 문서 AI에도 응용할 수 있는 패턴이에요.

### 3. PA-CoreId

- 여러 화합물에서 공통으로 나타나는 핵심 구조를 찾아요.
- 구조를 고정 길이 fingerprint(ECFP4)와 수치 속성 feature로 바꾸고, 중요한 feature만 골라요.
- XGBoost + Random Forest 앙상블에 Boruta feature selection과 베이지안 하이퍼파라미터 최적화를 썼어요. 공개 데이터셋 일부로 단일 GPU에서 하루 정도 학습했다고 해요.

## 실험

두 가지를 중점적으로 봤어요.

1. **PA-Img2Mol**이 기존 방법보다 이미지 → 문자열 변환을 얼마나 잘하는가
2. **PA-CoreId**가 핵심 구조를 얼마나 정확히 찾는가

![OCSR 비교 결과]({{ '/assets/images/papers/patentagent/ocsr-results.png' | relative_url }})

- **Img2Mol**: OSRA, Imago, MolVec, Img2Mol, SwinOCSR, MolScribe, DECIMER와 5개 데이터셋(CLEF, JPO, UOB, USPTO, ACS)에서 비교했고, **5개 중 4개에서 최고 점수**를 받았어요.
- **CoreId**: 기존 방법들과 Top-1/5/10, Top-5%/10% 정확도(예측 후보 안에 정답이 있는 비율)로 비교했어요.

## 결론

특허 내용을 요약해 주는 기능이 중심일 거라 기대하고 읽었는데, 실제로는 **핵심 구조 찾기, 구조식 이미지 인식, 특허 기반 QA**가 주된 기능이었어요. 핵심 구조 식별은 아직 실무에 쓰기엔 정확도가 아쉬워 보였어요.

기대와는 달랐지만, **여러 인식 모델 결과를 다시 렌더링해서 LLM으로 원본과 대조하는 검증 방식**은 이후 문서 이미지 처리에 활용할 수 있을 것 같아요.
