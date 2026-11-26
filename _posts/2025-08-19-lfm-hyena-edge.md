---
title: "LFM(Liquid Foundation Model)"
date: 2025-08-19
categories:
  - LLM 기법
tags:
  - Model Architecture
  - Hyena
  - Long Convolution
  - Edge AI
toc: true
toc_sticky: true
excerpt: "LFM(Liquid Foundation Model) : attention 대신 장기 convolution, gating, 상태 공간 역학 등을 이용해, 메모리·속도·적응성 측면에서 트랜스포머보다 효율적인 성능을 보여줌"
---

<https://turingpost.co.kr/p/topic-39-lfm-hyena-edge>

<https://arxiv.org/pdf/2302.10866>

## 두 줄 요약

- LFM(Liquid Foundation Model) : attention 대신 장기 convolution, gating, 상태 공간 역학 등을 이용해, 메모리·속도·적응성 측면에서 트랜스포머보다 효율적인 성능을 보여줌
- Hyena Edge : LFM의 최신형 모델로, 경량 엣지 디바이스(예: 스마트폰)에서도 잘 돌아가며, 속도·메모리·전력 효율 측면에서 뛰어난 성능을 갖춤

## LFM이란?

- Liquid Foundation Model
- 출시 시기: 2024년 9월 공개
- **모델 크기**: 1B, 3B, 40B, 최신 7B 버전 제공
- **구성 요소**
  - Long Convolution : 멀리 떨어진 패턴도 포착
  - Gating Function : 수천 스텝 동안 정보 유지
  - 연속 상태 공간(State‑Space) 동역학 → 이 세 가지를 합쳐 “Linear Input‑Varying Systems (LIVs)”라고 불림

- **특징**
  - 모듈형 아키텍처 : 연산의 단위(Computational Unit)를 계층의 깊이를 기준으로 그룹화해 구성

    ![]({{ '/assets/images/papers/lfm/modular.png' | relative_url }})

  - 입력 전 구조화된 Feature 추출 → 모델이 입력 성격에 따라 자동 조정

    ![]({{ '/assets/images/papers/lfm/feature-1.png' | relative_url }})

    ![]({{ '/assets/images/papers/lfm/feature-2.png' | relative_url }})

  - Dual‑Mode Information Mixing
    - Token‑mixing : 시퀀스 내 토큰 관계 포착(어떤 관계에 있고 어떤 영향을 주고받는지 등)
    - Channel‑mixing : 다양한 특성(feature) 사이 관계 이해(특정 단어가 입력될 때, 그 단어의 문법적 역할, 어조, 시제, 위치 인코딩, 의미 벡터 등의 feature들 사이에서 어떤 조합이 더 중요한지, 어떤 의미를 갖는지 이해하도록 섞어줌)
  - 메모리 효율성
    - 정보가 흐르는 과정에서 점점 메모리를 압축해 최대 100만 토큰도 부담 없이 처리 가능
    - ex) LFM‑3B는 16 GB 메모리에서 작동 가능, 비슷한 트랜스포머 모델은 48 GB 필요

    ![]({{ '/assets/images/papers/lfm/memory.png' | relative_url }})

## LFM 성능 및 장점

- **LFM-1B** : 1B급 모델 중 언어 벤치마크 최고 점수
- **LFM-3B** : 3B급 중 1위, 7B~13B 모델 일부보다도 성능 우수. MS의 Phi-3.5와 비슷한 성능, 파라미터 18% 적음

  ![]({{ '/assets/images/papers/lfm/benchmark.png' | relative_url }})

- **LFM-40B (MoE 구조)** : 400억 파라미터지만 추론 시 120억개만 활성화되어, 성능은 유지하면서 처리량을 높이고 배포 비용을 낮춤. MMLU 벤치마크에서 약 78.8점으로 Llama 3.1(70B) 수준
- **LFM-7B :** 다국어 최적화. 영어, 아랍어, 일본어 우수 성능, 스페인어·프랑스어·중국어·한국어 등에서도 매우 높은 품질

  ![]({{ '/assets/images/papers/lfm/lfm-7b.png' | relative_url }})

- **이점**
  - 흐름 중심의 연산
  - 긴 대화에서도 빠르고 안정적
  - 하드웨어 자원 요구 낮고, 딜레이 적음
  - NVIDIA GPU, 스마트폰 Qualcomm 칩, Cerebras의 Wafer-Scale 엔진같은 다양한 환경 적응 가능
  - 자동으로 메모리나 에너지 최적화

## Hyena Edge

- Hyena Edge란?
  - LFM 개념을 기반으로 발전한 모델
    - 2025년 4월 25일 공개된 liquid AI의 LFM
  - 스마트폰, 노트북 등 경량 기기에서도 효율적으로 실행할 수 있는 모델
- 핵심 요소
  - Long Convolutions
    - 입력 시퀀스 전체에 걸쳐 정보를 혼합해줌
  - Gating Mechanisms
    - 입력값에 따라 어떤 정보를 유지하고 버릴 지 결정
- 구성 방식 : Convolution 기반 ‘멀티 하이브리드’ architecture를 중심으로 만들어짐

  ![]({{ '/assets/images/papers/lfm/hyena-edge.png' | relative_url }})

  - **Dynamic Convolution Filters**
    - Hyena의 핵심은 메모리 처리와 패턴 인식 방식에 있음
    - Convolution layer는 입력 신호를 혼합하고 유지하는 특수한 필터를 사용
    - 이 필터는 고정된 것이 아니라 작은 신경망이 학습을 통해 동적으로 생성함
    - 덕분에 모델은 느리게 변하는 트렌드, 급격한 전환, 미세한 신호 차이 등 다양한 패턴에 적응하며 작용
  - **효율적인 시퀀스 처리**
    - attention layer를 대체하면서도 긴 시퀀스에서 효율적으로 동작
    - 시퀀스 길이가 길어질수록 필요한 시간과 메모리 비용을 크게 줄일 수 있음
  - **Convolutional Multi-Hybrid 구조 (Hyena Edge)**
    - Hyena Edge는 하나의 연산자만 쓰지 않고
      - Hyena 스타일 연산자
      - 트랜스포머 요소
      - 기타 연산 방식

      을 의도적으로 조합했음
    - 각 계층이 가장 잘 작동하는 설정에서 최적 성능을 내도록 설계됨
    - 최신 하드웨어(GPU 등)를 최대한 활용하면서도 효율성과 성능 사이 균형을 맞춤
  - **STAR (Synthesis of Tailored Architectures)** 시스템을 사용해 자동으로 최적 아키텍처 설계
    - STAR : Liquid AI의 아키텍쳐 설계 자동화 시스템으로
    - 시간이 지나며 모델의 구조(attention, convolution, recurrence 등등)를 조합해 수백가지 아키텍쳐를 탐색

      ![]({{ '/assets/images/papers/lfm/star.png' | relative_url }})

    - 모델의 설계
      - 숫자 형태의 “유전체(genome)”로 인코딩됨
      - 성능이 뛰어난 모델을 선택해 이들을 재조합, 수정하며 설계를 발전시킴
      - 속도, 메모리 효율성 같은 특정한 성능 목표를 충족하는 최적의 모델 탐색
    - 실험한 모델
      - **Hyena (full)** : 내부 연산과 Gating 메커니즘 모두에 Convolution을 사용하는 형태
      - **Hyena-X** : 내부 Convolution을 제외한 구조
      - **Hyena-Y** : Gating Feature Group에서 Convolution을 제거한 구조
      - 이 중 Hyena-Y가 속도, 품질, 메모리 사용량 사이에서 가장 좋은 밸런스를 보여주어 최종 Hyena Edge 모델의 전체 attention block 중 2/3을 Hyena-Y Convoution으로 교체함

        **⇒ 현실적인 배포가 가능한 모델을 설계할 수 있음을 증명함**
- Hyena Edge 성능
  - 삼성 Galaxy S24 Ultra에서 실험
  - transformer 기반의 상위 모델들보다 최대 30% 낮은 지연시간
  - Wikitext, Lambada, Hellaswag, Winogrande, Piqa, Arc-easy, Arc-challenge 등의 다양한 벤치마크에서 GQA-Transformer++ baseline 모델을 모두 능가함
  - 짧은 프롬프트, 긴 프롬프트 모두 안정적으로 대응
  - attention의 이차적 메모리 사용 부담이 없음
  - 효율적인 convolution 구조와 연산자 통합, 최적화된 실행경로로 모든 시퀀스 길이에서 더 적은 메모리 사용

    ![]({{ '/assets/images/papers/lfm/hyena-edge-memory.png' | relative_url }})

  - 동일 연상량 투입 시, 하이브리드 모델들이 transformer보다 더 나은 성능을 내는 경향이 있음

    ![]({{ '/assets/images/papers/lfm/hybrid-compute.png' | relative_url }})

## 결론

Liquid AI의 LFM과 Hyena Edge는 단순히 모델 크기로 성능을 높이는 트랜스포머와는 다른 길을 제시함.

특히 Hyena Edge는 **경량 디바이스에서도 빠른 응답과 높은 성능을 동시에 확보**해 현실적인 대안이 될 수 있음.

다만, 이미 강력히 자리잡은 트랜스포머 생태계와 학습 방식의 차이, 아직 API 중심이라는 공개 방식은 널리 확산되기까지 넘어야 할 과제임
