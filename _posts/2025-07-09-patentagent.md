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
excerpt: "PatentAgent는 제약 특허 분석을 자동화하기 위한 시스템으로, QA 응답, 화학 구조 이미지 인식, 핵심 구조 식별의 세 가지 역할을 수행"
---

<https://arxiv.org/pdf/2410.21312>

### 3줄 요약

- PatentAgent는 제약 특허 분석을 자동화하기 위한 시스템으로, QA 응답, 화학 구조 이미지 인식, 핵심 구조 식별의 세 가지 역할을 수행
- PA-Img2Mol 모듈은 특허 문서 내 화학 구조 이미지를 SMILES로 변환하고, 이를 GPT-4o를 활용해 유사도 검증까지 진행
- PA-CoreId는 머신러닝 기반으로 특허 내 화합물들 사이의 공통 핵심 구조를 자동으로 찾아냄

### 배경

“신약 개발 특허 검토”라는 주제로 고객사 PoC를 진행하게 되었습니다. 새로 생성할 화합물을 특허로 등록할 수 있는지에 대해 판단하고, 새로 만들 화합물을 특허로 등록 가능할 지 예측하는 업무입니다.

제약 특허 중 필요한 부분을 정리하는 기능에 대해 검색하다, 제약 특허 분석을 자동화하기 위해 만든 PatentAgent 논문을 발견했고, 화합물(사진)과 그에 관련된 정보들을 어떻게 정리했는지 알고 싶어 리뷰하게 되었습니다.

### 핵심 아이디어

**\[Patent framework\]**

![]({{ '/assets/images/papers/patentagent/framework.png' | relative_url }})

**\[Patent Agent 역할 3가지\]**

1. PA-QA
   - 자연어 질의에 대한 QA 챗봇
   - 사용자의 요청을 처리하고 특허 문서에서 정확한 정보를 추출하여 답변 생성
   - 사용자의 요청이 입력되면, PA-QA는 특허의 특정 섹션(초록, 청구항, 상세 설명 등)을 집중적으로 분석. 이 과정에서 객체 탐지 및 광학 문자 인식(OCR) 기술을 사용하여 관련 섹션 추출
   - 특정 섹션에서 semantic search로 사용자의 질문과 가장 관련 있는 내용을 검색.(transformer-based retrieval model 사용)
2. PA-Img2Mol

   ![]({{ '/assets/images/papers/patentagent/img2mol.png' | relative_url }})
   Img → SMILES<br>1. 세가지 OCSR 모델(M1~M3)로 Image to SMILES 진행<br>2. 만들어진 SMILES를 컴퓨터가 더 잘 처리할 수 있도록 표준화<br>3. SMILES를 다시 그림으로 변환 (그림 → SMILES로 제대로 변환됐는지 확인)<br>4. 원본 화학 구조식 이미지와 비교
   {: .text-center}

   - 화학 구조 이미지에서 분자 표현으로의 변환 작업을 수행하는 광학 화학 구조 인식(OCSR) 모듈
   - MolDetect를 사용해 특허에서 화학 이미지를 추출 → 세 가지 OCSR 모델(DECIMER, MolScribe, SwinOCSR)을 통해 SMILES 표현식으로 변환

   ![]({{ '/assets/images/papers/patentagent/gpt4o-similarity.png' | relative_url }})

   - 변환된 SMILES의 유사도 평가를 위해 GPT-4o를 사용, 생성된 SMILES와 원래 이미지 간의 유사도 비교
3. PA-CoreId

   ![]({{ '/assets/images/papers/patentagent/coreid.png' | relative_url }})

   - 다양한 화학 물질에서 공통 구조를 찾는 것으로 핵심 구조 식별
     1. 특징 선택
        - 특징 : 화학 구조 → 컴퓨터가 이해할 수 있는 형태로 표현한 것
        - 화학 물질의 구조적 정보를 담고 있는 ECFP4와 물리화학적 속성 등을 특징으로 사용함
        - 수많은 특징 중 핵심 구조 식별에 가장 중요한 특징들만 골라내기
     2. 모델 훈련
        - XGBoost와 Random Forest 앙상블 사용
        - Boruta 알고리즘을 통한 특성 선택과 베이지안 최적화를 이용, 최적의 파라미터 탐색
        - PatentNetML의 mini dataset2 + 3080ti로 하루동안 훈련시켰다고함.

### 실험

- 중점으로 판단한 것
  1. PA-Img2mol이 기존 방법에 비해 img to SMILES를 얼마나 효과적으로 변환하는가
  2. PA-CoreID는 다른 방법과 비교해 제약 특허 내 핵심 화학구조를 얼마나 정확히 식별하는가

1. OSRA, Imago, MolVec, Img2Mol, SwinOCSR, MolScribe, DECIMER와 비교. 비교 시 사용한 데이터셋은 CLEF, JPO, UOB, USPTO, ACS

   ![]({{ '/assets/images/papers/patentagent/ocsr-results.png' | relative_url }})
   5개 데이터셋 중 4개에서 가장 나은 점수를 획득
   {: .text-center}

2. PatentNetML, Chem-informatics, Molecular Idol, Frequency of Group Analysis를 PatentNetML 데이터셋에서 비교

   ![]({{ '/assets/images/papers/patentagent/coreid-results.png' | relative_url }})
   Top 1,5,10 : 핵심 화학구조일 것이라 예측한 구조 1개,5개,10개 중에 실제로 정답이 있을 확률<br>Top 5%, 10% : 핵심 화학구조일 것이라 예측한 구조 5%,10% 중에 실제로 정답이 있을 확률
   {: .text-center}

### 결론

특허 안의 내용을 정리해주는 기능이 주를 이룰 것이라 예상하고 읽기 시작했으나, 해당 논문과 해당 논문의 서비스는 특허 속 메인 화합물 캐치(하지만 이 부분의 정확도는 실제로 사용하기엔 어려움이 있어보입니다.), 분자 구조를 SMILES로 알려주는 것, 특허 내용을 토대로 QA를 진행하는 것 정도가 메인인 것 같습니다.<br>기대했던 내용과는 달랐지만, 이 중 화합물 구조 그림을 넣었을 때 SMILES로 변경해주는 방법론 정도는 추후 활용할 수 있을 것 같습니다.
