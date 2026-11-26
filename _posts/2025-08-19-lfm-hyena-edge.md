---
title: "LFM(Liquid Foundation Model)과 Hyena Edge"
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
excerpt: "attention 대신 long convolution·gating·상태 공간 역학으로 효율을 높인 LFM과, 스마트폰에서도 돌아가는 Hyena Edge"
---

> 📄 **Paper**: [Hyena Hierarchy (arXiv:2302.10866)](https://arxiv.org/abs/2302.10866) · 참고: [Turing Post – LFM과 Hyena Edge](https://turingpost.co.kr/p/topic-39-lfm-hyena-edge)

## 두 줄 요약

- **LFM(Liquid Foundation Model)**: attention 대신 long convolution, gating, 상태 공간(state-space) 역학을 써서, 메모리·속도·적응성 면에서 transformer보다 효율적이에요.
- **Hyena Edge**: LFM 계열의 최신 모델로, 스마트폰 같은 경량 엣지 기기에서도 잘 돌아가고 속도·메모리·전력 효율이 뛰어나요.

## LFM이란?

- 2024년 9월 공개, 1B · 3B · 40B, 최근 7B 버전까지 있어요.
- **구성 요소**: 세 가지를 합쳐 "Linear Input-Varying Systems(LIVs)"라고 불러요.
  - **Long Convolution**: 멀리 떨어진 패턴도 포착
  - **Gating Function**: 수천 스텝 동안 정보 유지
  - **연속 상태 공간(State-Space) 동역학**

### 특징

**모듈형 아키텍처**: 연산 단위(computational unit)를 계층 깊이 기준으로 묶어 구성해요. 입력 전에 구조화된 feature를 뽑아서, 입력 성격에 따라 모델이 자동으로 조정돼요.

![LFM 모듈형 아키텍처]({{ '/assets/images/papers/lfm/modular.png' | relative_url }})

**Dual-Mode Information Mixing**

| 방식 | 하는 일 |
|---|---|
| Token-mixing | 시퀀스 안 토큰들이 어떤 관계이고 서로 어떤 영향을 주는지 포착 |
| Channel-mixing | 한 단어의 문법적 역할·어조·시제·위치·의미 벡터 같은 feature 중 어떤 조합이 중요한지 섞어서 이해 |

**메모리 효율**: 정보가 흐르면서 메모리를 점점 압축해, 최대 100만 토큰도 부담 없이 처리해요. 예를 들어 LFM-3B는 16GB에서 돌아가지만, 비슷한 transformer는 48GB가 필요해요.

![LFM 메모리 사용량 비교]({{ '/assets/images/papers/lfm/memory.png' | relative_url }})

## LFM 성능

![LFM 벤치마크]({{ '/assets/images/papers/lfm/benchmark.png' | relative_url }})

| 모델 | 성능 |
|---|---|
| LFM-1B | 1B급 중 언어 벤치마크 최고 점수 |
| LFM-3B | 3B급 1위. 일부 7B~13B 모델보다도 좋고, MS Phi-3.5와 비슷한 성능에 파라미터는 18% 적음 |
| LFM-40B (MoE) | 400억 파라미터 중 추론 시 120억만 활성화. MMLU 약 78.8점으로 Llama 3.1 70B 수준 |
| LFM-7B | 다국어 최적화. 영어·아랍어·일본어에서 우수하고, 한국어 등에서도 높은 품질 |

**이점**

- 흐름 중심의 연산이라 긴 대화에서도 빠르고 안정적
- 하드웨어 요구가 낮고 지연이 적음
- NVIDIA GPU, Qualcomm 모바일 칩, Cerebras Wafer-Scale 엔진 등 다양한 환경에 적응
- 메모리와 에너지를 자동으로 최적화

## Hyena Edge

- LFM 개념을 발전시킨 모델로, 2025년 4월 Liquid AI가 공개했어요.
- 스마트폰, 노트북 같은 경량 기기에서도 효율적으로 돌아가요.
- 핵심 요소는 **Long Convolution**(시퀀스 전체에 걸쳐 정보 혼합)과 **Gating**(어떤 정보를 유지하고 버릴지 결정)이에요.

### 구성: Convolution 기반 멀티 하이브리드

![Hyena Edge 구조]({{ '/assets/images/papers/lfm/hyena-edge.png' | relative_url }})

- **Dynamic Convolution Filters**: 필터가 고정돼 있지 않고 작은 신경망이 학습으로 동적으로 만들어요. 그래서 느린 트렌드, 급격한 전환, 미세한 신호 차이 같은 다양한 패턴에 적응해요.
- **효율적인 시퀀스 처리**: attention layer를 대체하면서도 긴 시퀀스에서 시간·메모리 비용을 크게 줄여요.
- **Convolutional Multi-Hybrid**: Hyena 스타일 연산자, transformer 요소, 기타 연산을 의도적으로 섞어서, 각 계층이 가장 잘 작동하는 설정을 쓰게 했어요.

### STAR: 아키텍처 설계 자동화

![STAR 아키텍처 탐색]({{ '/assets/images/papers/lfm/star.png' | relative_url }})

- **STAR(Synthesis of Tailored Architectures)**는 Liquid AI의 아키텍처 설계 자동화 시스템이에요.
- attention, convolution, recurrence 같은 구조를 조합해 수백 가지 아키텍처를 탐색해요.
- 모델 설계를 숫자 형태의 "유전체(genome)"로 인코딩하고, 성능 좋은 모델을 재조합·변형하면서 속도·메모리 같은 목표에 맞는 모델을 찾아요.

| 실험한 변형 | 구조 |
|---|---|
| Hyena (full) | 내부 연산과 gating 모두에 convolution 사용 |
| Hyena-X | 내부 convolution 제외 |
| Hyena-Y | gating feature group에서 convolution 제거 |

Hyena-Y가 속도·품질·메모리 사이 균형이 가장 좋아서, 최종 Hyena Edge는 attention block의 2/3를 Hyena-Y convolution으로 바꿨어요. ⇒ **현실적으로 배포 가능한 모델을 자동 설계할 수 있음**을 보였어요.

### Hyena Edge 성능

- Samsung Galaxy S24 Ultra에서 실험했어요.
- transformer 기반 상위 모델보다 **지연시간이 최대 30% 낮았어요.**
- Wikitext, Lambada, Hellaswag, Winogrande, PIQA, ARC-easy/challenge 등에서 GQA-Transformer++ baseline을 모두 넘었어요.
- 짧은 prompt와 긴 prompt 모두 안정적이고, attention의 이차 메모리 부담이 없어서 모든 시퀀스 길이에서 메모리를 덜 써요.
- 같은 연산량이면 하이브리드 모델이 transformer보다 나은 경향을 보였어요.

## 결론

LFM과 Hyena Edge는 모델 크기를 키워 성능을 올리는 transformer와는 다른 길을 보여줘요. 특히 Hyena Edge는 **경량 기기에서도 빠른 응답과 높은 성능을 함께 확보**해서 현실적인 대안이 될 수 있어요.

다만 이미 자리 잡은 transformer 생태계와 학습 방식이 다르고, 아직 API 중심으로 공개돼 있다는 점은 널리 퍼지기까지 넘어야 할 과제예요.
