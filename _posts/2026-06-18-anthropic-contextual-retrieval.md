---
title: "Anthropic Contextual Retrieval"
date: 2026-06-18
categories:
  - LLM 기법
tags:
  - RAG
  - Retrieval
  - BM25
  - Embedding
toc: true
toc_sticky: true
excerpt: "임베딩·BM25 인덱싱 전에 각 청크에 LLM이 생성한 짧은 맥락 설명을 덧붙이면, 검색 실패율을 크게 줄일 수 있다"
---

#### **참고 자료**

- 제목: *Introducing Contextual Retrieval*
- 발행: **Anthropic** (엔지니어링 블로그, 2024.09) — Anthropic 공식 엔지니어링 포스트
- 핵심 기법: **Contextual Embeddings** + **Contextual BM25**
- 원문/구현: [Anthropic — Introducing Contextual Retrieval](https://www.engineering.fyi/article/introducing-contextual-retrieval) · [Claude Cookbook(구현 가이드)](https://platform.claude.com/cookbook/capabilities-contextual-embeddings-guide) · [DataCamp 튜토리얼](https://www.datacamp.com/tutorial/contextual-retrieval-anthropic)

### **선정 배경**

- 청크 단위 RAG의 고질적 문제는 "청크를 잘게 쪼개는 순간, 그 청크가 어느 문서·어느 맥락의 일부였는지가 사라진다"는 점입니다. 예를 들어 "영업이익은 461억원"이라는 청크만 떼어 놓으면 *어느 회사·어느 분기·연결인지 별도인지*가 빠져, 임베딩도 BM25도 엉뚱한 문서를 끌어옵니다.
- Anthropic은 이 "맥락 소실(context loss)"을, 검색을 바꾸는 대신 *청크 자체를 자기완결적으로 만들어* 푼다는 발상을 제시합니다.<br>
  우리처럼 방대한 문서에서 근거를 모으는 환경에서 청크 단위 검색 실패를 얼마나 줄여주는지 확인하고자 정리했습니다.

### **한 눈에 보기**

- **출처:** Anthropic 엔지니어링 블로그(2024.09)
- **핵심 주장:** 임베딩·BM25 인덱싱 *전에* 각 청크에 **LLM이 생성한 짧은 맥락 설명을 덧붙이면**, 검색 실패율을 크게 줄일 수 있다
  - <span style="color:gray">2024년에 나온 개념으로 너무 오래된 것 같다고 생각할 수 있겠으나, 현재도 활발히 사용중인 기법</span>
- **핵심 개념:** Contextual Embeddings(맥락 주입 후 임베딩), Contextual BM25(맥락 주입 후 키워드 색인), prompt caching
- **방법론:** 인덱싱 시 청크마다 "이 청크가 문서 전체에서 어떤 위치·역할인지" 50~100 토큰 맥락을 LLM으로 생성 → 청크 앞에 붙여서 *임베딩과 BM25 양쪽*에 색인. 검색은 dense+sparse를 rank fusion으로 합치고, 그 위에 reranking을 얹음
- **포지션:** 검색 알고리즘이 아니라 **인덱싱 시점의 청크 전처리(context augmentation)** 로 품질을 끌어올림

### **문제 정의 — 청크는 왜 맥락을 잃는가**

청크 단위 RAG의 근본 문제는 *"독립 청크는 문서 전역 맥락이 빠져 있다"* 는 것.<br>
같은 문장도 맥락이 없으면 검색이 헛다리를 짚음

| **맥락 소실의 증상** | **영향** |
| --- | --- |
| "461억원" 청크에 회사/기간/연결여부 없음 | 다른 회사·기간 문서와 혼동 |
| 대명사·축약("동사", "전사")만 남음 | 임베딩이 엉뚱한 분포로 매핑 |
| 표 행이 헤더와 분리됨 | 어떤 지표의 값인지 소실 |

Contextual Retrieval의 출발점: *"검색을 고치지 말고, 청크를 먼저 자기완결적으로 만들자."*

### **핵심 아이디어 — Contextualize before you index**

청크를 임베딩/색인하기 *전에*, LLM에게 **"이 청크가 문서 전체에서 무엇인지"**를 짧게 설명하게 하고 그 설명을 청크 앞에 붙임.

| **단계** | **시점** | **구성요소** | **역할** |
| --- | --- | --- | --- |
| 1. Contextualize | **인덱싱 (오프라인, 1회)** | LLM | 청크마다 문서 맥락 50~100토큰 생성 (예: "SK바이오팜 4Q25P 연결 실적 표의 영업이익 행") |
| 2-a. Contextual Embedding | 인덱싱 | 임베딩 인코더 | 맥락+청크를 합쳐 벡터화 → 의미 검색 정확도↑ |
| 2-b. Contextual BM25 | 인덱싱 | 키워드 색인 | 맥락+청크를 BM25 색인 → 정확매칭(회사명·지표·숫자)↑ |
| 3. Search | 쿼리타임 | dense + sparse + rerank | 두 검색 결과를 rank fusion 후 reranking |

핵심 : **dense(의미)와 sparse(어휘)를 둘 다, 그것도 맥락이 주입된 상태로** 쓴다는 점

- **Contextual Embedding** — 의미적 비대칭을 줄임
- **Contextual BM25** — 임베딩이 놓치는 정확 토큰 매칭을 보강
- **prompt caching** — 청크마다 문서 전체를 LLM에 다시 넣는 비용을 캐시로 상쇄해, 인덱싱 LLM 비용을 실용 수준으로 낮춤

### **인덱싱 파이프라인 / prompt**

HyPE가 "이 청크가 답할 질문을 만들어라"였다면, Contextual Retrieval은 "이 청크가 문서에서 *무엇인지* 설명하라"입니다.

```json
📄 문서 전체 + 📖 청크: "영업이익 461억원(+14% YoY, -33% QoQ)"
        │
        ▼  LLM: "이 청크가 문서 전체에서 어떤 맥락인지 50~100토큰으로 설명하라"
        └─ 맥락: "SK바이오팜 4Q25P 연결 기준 실적 리뷰의 영업이익 수치.
                  컨센서스를 16% 하회한 분기 잠정 실적임."
        │
        ▼  [맥락 + 원본 청크] 를 합쳐서
        ├─ 임베딩 → dense 인덱스 저장 💾  (Contextual Embedding)
        └─ BM25 색인 → sparse 인덱스 저장 💾 (Contextual BM25)
```

> 결과적으로 같은 "461억원" 청크라도, 인덱스에는 *회사·기간·연결여부·성격*이 박힌 자기완결 청크로 저장됨

### **실험 결과**

Anthropic 자체 평가 기준, **검색 실패율(top-k 안에 정답 청크가 없을 확률)** 감소

| **구성** | **검색 실패율** | **감소** |
| --- | --- | --- |
| 기준(일반 임베딩) | 5.7% | — |
| Contextual Embedding 단독 | 3.7% | **-35%** |
| + Contextual BM25 (하이브리드) | 2.9% | **-49%** |
| + Reranking | 1.9% | **-67%** |

- **비용 통제:** prompt caching으로 인덱싱 시 문서 재투입 비용을 상쇄 (블로그 기준 100만 문서 토큰당 약 \$1 수준).
- **호환성:** 하이브리드·reranking과 자연스럽게 결합 (오히려 함께 쓸 때 최대 효과)

### **핵심 통찰**

Contextual Retrieval의 효과 한 줄 요약: *"검색 알고리즘이 아니라 색인되는 청크를 자기완결적으로 만든다."*

HyPE가 *문서를 질문으로* 바꿔 question-to-question 매칭을 만들었다면, Contextual Retrieval은 *청크에 문서 맥락을 주입*해 **청크 자체의 표현력**을 끌어올림. 또 하나 주목할 점은 **dense와 sparse를 동시에 맥락화**한다는 것 — 의미 검색의 약점(정확 토큰)과 키워드 검색의 약점(동의·문맥)을 서로 메움. 비용은 prompt caching으로 인덱스 시점에 가두어, 쿼리타임에는 추가 LLM 호출 없이 효과만 누림

### **시사점**

**① 효과는 크지만 "재인덱싱"이 전제**

청크마다 맥락을 새로 만들어 *다시 임베딩·다시 BM25 색인*해야함.<br>
이미 적재가 끝난 우리 인덱스에는 **전체 재인덱싱 비용**으로 돌아옴

② **우리는 이미 상당 부분 확보**

우리 청크 본문에는 heading path와 LLM 태깅 summary가, 구조화 근거 필드에는 회사·세그먼트·기간·지표 성격이 이미 들어 있음. Contextual Retrieval이 *새로 만들어 주입*하려는 "청크 자기완결성"을 **우리는 파싱 단계에서 이미 만들어 둔** 셈

③ **단, 안 쓰는 게 문제**

정작 그 맥락이 검색·합성까지 *전달되지 않고* 있음<br>
(검색 결과 반환 필드에서 잘림 → synthesize는 flat text만 봄)<br>
즉 우리에게 필요한 건 *맥락을 새로 생성*하는 게 아니라 *이미 있는 맥락을 안 버리는 것*.

④ **trade-off 명확화**

- (a) 재인덱싱 비용(전 청크 재작성+재임베딩+재색인).
- (b) BM25 하이브리드 부분만 떼어내면 **재임베딩 없이도** 도입 가능
- (c) 국문 도메인 실측 필요(블로그 수치는 일반 코퍼스 기준)

### **정리**

- Contextual Retrieval은 **인덱싱 시점에 청크마다 LLM 맥락을 붙여** 임베딩·BM25 양쪽을 자기완결화하고, rank fusion + reranking으로 검색 실패율을 줄이는 기법
- 효과: 검색 실패 **35%(임베딩) / -49%(+BM25) / -67%(+reranking)**, 쿼리타임 추가 LLM 호출 없음(비용은 인덱싱+prompt caching으로 흡수)
- 한계: **전체 재인덱싱 필요**, 국문 도메인 실측 필요
- 우리 환경 적용: 맥락 생성(=재인덱싱)은 *이미 summary/구조화 근거 필드로 확보*돼 중복 → **하이브리드(Contextual BM25)의 BM25 부분만 재임베딩 없이 차용**하고, 이미 있는 구조 맥락을 synthesize까지 전달하는 게 핵심
- HyPE ↔ Contextual Retrieval: 둘 다 *인덱스타임에 비용을 가두는* 같은 철학 — HyPE는 *질문을 미리 만들고*, Contextual Retrieval은 *청크에 맥락을 미리 주입*한다는 점이 다름
