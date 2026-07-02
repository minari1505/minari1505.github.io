---
title: "RAG and Agentic Search"
title_ko: "RAG와 에이전틱 검색"
course: claude-with-the-anthropic-api
lesson: 6
---

## 학습 목표

- RAG가 왜 필요한지, prompt stuffing과 어떤 차이가 있는지 이해하기
- 청킹, 임베딩, 벡터 DB, 유사도 검색으로 이어지는 RAG 흐름 파악하기
- 의미 검색과 BM25 어휘 검색의 강점/약점 구분하기
- 하이브리드 검색과 에이전틱 검색이 필요한 상황 이해하기

## RAG란? (Retrieval Augmented Generation)

Claude는 학습 시점 이후의 정보나 **내 사설 데이터**(사내 문서, 보고서, DB)를 기본적으로 모릅니다. RAG는 사용자의 질문과 관련된 문서를 먼저 검색하고, 검색된 조각을 프롬프트에 넣어 Claude가 근거를 보고 답하게 하는 기법입니다.

```
질문 → 관련 문서 검색 → [질문 + 검색된 문서]로 프롬프트 구성 → Claude가 근거 기반 답변
```

핵심은 "모든 문서를 다 넣자"가 아니라 **지금 질문에 필요한 조각만 찾아 넣자**입니다.

### 큰 문서를 그대로 넣는 방식의 한계

예를 들어 800페이지짜리 재무 문서에서 "이 회사의 리스크 요인은 무엇인가?"를 묻는다고 해봅시다. 가장 단순한 방법은 전체 문서를 프롬프트에 그대로 넣는 것입니다.

```text
Answer the user's question about the financial document.

<user_question>
{user_question}
</user_question>

<financial_document>
{financial_document}
</financial_document>
```

이 방식은 단순하지만 한계가 큽니다.

- 프롬프트 길이 제한에 걸릴 수 있음
- 매우 긴 프롬프트에서는 모델 성능이 떨어질 수 있음
- 입력 토큰이 많아져 비용이 증가함
- 처리 시간이 길어짐

RAG는 이 문제를 전처리와 검색으로 나눠 해결합니다. 먼저 문서를 작은 chunk로 나누고, 사용자가 질문할 때 관련 chunk만 찾아 프롬프트에 넣습니다.

```text
[전처리] 문서 → chunk로 나누기 → 검색 가능한 형태로 저장
[질의]   질문 → 관련 chunk 검색 → 질문 + chunk로 Claude 호출
```

### RAG의 장점과 비용

RAG의 장점은 명확합니다.

- Claude가 관련 내용에 집중할 수 있음
- 매우 큰 문서나 여러 문서를 다룰 수 있음
- 프롬프트가 작아져 비용과 지연이 줄어듦
- 답변에 근거 문서를 붙이기 쉬움

대신 구현 복잡도가 생깁니다.

- 문서를 어떻게 나눌지 결정해야 함
- 관련 chunk를 찾는 검색 시스템이 필요함
- 검색된 chunk가 충분한 맥락을 담고 있는지 평가해야 함
- chunking, embedding, indexing 전략을 계속 튜닝해야 함

즉, RAG는 단순함을 포기하고 확장성과 효율성을 얻는 방식입니다. 문서가 짧고 한 번만 물어볼 거라면 전체 문서를 넣는 편이 나을 수 있지만, 문서가 크거나 반복 질의가 많거나 비용이 중요하면 RAG가 유리합니다.

## 텍스트 청킹 전략

청킹은 RAG 품질을 크게 좌우합니다. 잘못 나눈 chunk는 관련 없는 맥락을 프롬프트에 넣고, 결국 Claude가 엉뚱한 답을 하게 만들 수 있어요.

예를 들어 한 문서에 의학 연구 섹션과 소프트웨어 엔지니어링 섹션이 모두 있다고 해봅시다. 사용자가 "올해 엔지니어가 버그를 얼마나 고쳤나?"라고 물었는데, 청킹이 나쁘면 "bug"라는 단어가 의학 섹션에서 다른 의미로 쓰였다는 이유로 잘못된 chunk가 검색될 수 있습니다.

### 1. 크기 기반 청킹

가장 단순한 방법은 텍스트를 일정 길이로 자르는 것입니다. 어떤 문서에도 적용할 수 있고 구현이 쉽습니다.

```python
def chunk_by_char(text, chunk_size=150, chunk_overlap=20):
    chunks = []
    start_idx = 0

    while start_idx < len(text):
        end_idx = min(start_idx + chunk_size, len(text))
        chunk_text = text[start_idx:end_idx]
        chunks.append(chunk_text)

        start_idx = (
            end_idx - chunk_overlap if end_idx < len(text) else len(text)
        )

    return chunks
```

단점도 분명합니다.

- 단어가 중간에서 잘릴 수 있음
- 문장이나 문단의 맥락이 끊길 수 있음
- 제목과 본문이 서로 다른 chunk로 갈라질 수 있음

그래서 보통 **overlap**을 둡니다. 각 chunk가 앞뒤 chunk 일부를 공유하면 경계에서 사라지는 맥락을 줄일 수 있어요.

### 2. 구조 기반 청킹

문서 구조가 명확하다면 제목, 섹션, 문단 단위로 나누는 편이 좋습니다. Markdown 문서처럼 `##` 제목이 안정적으로 들어 있다면 섹션 단위 chunk가 자연스럽습니다.

```python
def chunk_by_section(document_text):
    pattern = r"\n## "
    return re.split(pattern, document_text)
```

장점은 chunk가 의미 있는 단위로 나뉜다는 점입니다. 한 섹션의 제목과 본문이 함께 유지되므로 검색 결과가 더 해석하기 쉽습니다.

단점은 문서 구조를 신뢰할 수 있어야 한다는 점입니다. 현실의 PDF, 복사된 텍스트, OCR 결과는 제목 구조가 깨져 있는 경우가 많습니다.

### 3. 문장 기반 청킹

문장 단위로 나눈 뒤 몇 문장씩 묶는 방식은 좋은 절충안입니다.

```python
def chunk_by_sentence(text, max_sentences_per_chunk=5, overlap_sentences=1):
    sentences = re.split(r"(?<=[.!?])\s+", text)

    chunks = []
    start_idx = 0

    while start_idx < len(sentences):
        end_idx = min(start_idx + max_sentences_per_chunk, len(sentences))
        current_chunk = sentences[start_idx:end_idx]
        chunks.append(" ".join(current_chunk))

        start_idx += max_sentences_per_chunk - overlap_sentences

        if start_idx < 0:
            start_idx = 0

    return chunks
```

문장을 자르지 않으면서 일정한 크기를 유지할 수 있고, overlap 문장으로 맥락도 보존할 수 있습니다.

### 4. 의미 기반 청킹

가장 정교한 방식은 문장들의 의미적 관련성을 보고 chunk를 구성하는 것입니다. 연속된 문장들이 같은 주제를 다루는 동안 하나의 chunk로 묶고, 주제가 바뀌면 분리합니다.

품질은 좋을 수 있지만 비용과 구현 복잡도가 큽니다. 의미 분석을 위해 추가 모델이나 NLP 처리가 필요할 수 있어요.

### 어떤 전략을 고를까?

| 전략 | 적합한 상황 | 주의점 |
|---|---|---|
| 구조 기반 | Markdown, 내부 보고서처럼 구조가 안정적인 문서 | 구조가 깨진 문서에는 약함 |
| 문장 기반 | 일반 텍스트 문서 | 문장 분리 품질에 의존 |
| 크기 기반 + overlap | 다양한 입력을 안정적으로 처리해야 하는 프로덕션 | 의미 단위가 깨질 수 있음 |
| 의미 기반 | 품질이 매우 중요하고 비용을 감당할 수 있는 경우 | 구현과 계산 비용이 큼 |

실전에서는 크기 기반 + overlap을 안정적인 기본값으로 두고, 문서 구조를 신뢰할 수 있을 때 구조 기반을 쓰는 식으로 시작하는 것이 무난합니다.

## 텍스트 임베딩과 의미 검색

청킹 다음 단계는 "사용자의 질문과 가장 관련 있는 chunk가 무엇인가?"를 찾는 것입니다. 이때 많이 쓰는 방식이 **의미 검색(semantic search)**입니다.

의미 검색은 정확히 같은 단어가 있는지보다, 텍스트의 의미와 맥락이 얼마나 가까운지를 봅니다. 이를 위해 텍스트를 embedding으로 바꿉니다.

**텍스트 임베딩**은 텍스트의 의미를 숫자 배열로 표현한 것입니다. 임베딩 모델에 텍스트를 넣으면 긴 실수 리스트가 나오고, 의미가 비슷한 텍스트끼리는 벡터 공간에서 가까운 위치에 놓입니다.

주의할 점은 각 숫자의 의미를 사람이 직접 해석할 수 없다는 것입니다. "첫 번째 숫자는 행복도, 두 번째 숫자는 바다 관련성"처럼 상상할 수는 있지만, 실제 차원별 의미는 모델이 학습한 내부 표현이라 명확히 이름 붙이기 어렵습니다.

### VoyageAI로 임베딩 만들기

강의에서는 임베딩 생성을 위해 VoyageAI를 사용합니다. 별도 API key를 발급받고 환경 변수에 넣습니다.

```text
VOYAGE_API_KEY="your_key_here"
```

설치와 기본 함수는 다음과 같습니다.

```python
%pip install voyageai
```

```python
from dotenv import load_dotenv
import voyageai

load_dotenv()
client = voyageai.Client()

def generate_embedding(text, model="voyage-3-large", input_type="query"):
    result = client.embed([text], model=model, input_type=input_type)
    return result.embeddings[0]
```

이 함수는 하나의 텍스트를 숫자 벡터로 바꿉니다. RAG에서는 문서 chunk와 사용자 질문을 같은 임베딩 공간으로 보내고, 질문 벡터와 가까운 chunk 벡터를 찾습니다.

## 전체 RAG 흐름

```
[준비 단계]  문서 → 청킹 → 임베딩 → 벡터 인덱스에 저장
[질의 단계]  질문 → 임베딩 → 인덱스에서 유사 청크 검색 → 프롬프트에 삽입 → Claude 답변
```

조금 더 구체적으로 보면 여섯 단계입니다.

1. 원본 문서를 chunk로 나눔
2. 각 chunk를 embedding으로 변환
3. embedding과 원문 chunk를 vector database에 저장
4. 사용자 질문을 embedding으로 변환
5. 질문 embedding과 가까운 chunk를 검색
6. 질문과 검색된 chunk를 프롬프트에 넣어 Claude에게 답변 생성 요청

강의에서는 이해를 위해 2차원 임베딩 예시를 듭니다. 첫 번째 숫자가 "의학 관련성", 두 번째 숫자가 "소프트웨어 엔지니어링 관련성"이라고 가정하면, 의학 연구 섹션은 `[0.97, 0.34]`, 소프트웨어 섹션은 `[0.30, 0.97]`처럼 표현될 수 있습니다.

실제 임베딩은 수백, 수천 차원일 수 있고 각 차원을 해석할 수는 없지만, 원리는 같습니다. 질문도 같은 공간의 벡터로 변환한 뒤 가까운 chunk를 찾습니다.

### 코사인 유사도와 코사인 거리

벡터 DB는 보통 **cosine similarity**로 벡터 사이의 유사도를 계산합니다. 두 벡터가 이루는 각도가 작을수록 비슷하다고 봅니다.

- cosine similarity가 `1`에 가까움: 매우 유사
- cosine similarity가 `0`에 가까움: 관련이 약함
- cosine similarity가 `-1`에 가까움: 매우 다름

문서에서는 **cosine distance**를 볼 때도 많습니다.

```text
cosine distance = 1 - cosine similarity
```

이 경우 해석이 반대입니다.

- distance가 `0`에 가까움: 매우 유사
- distance가 클수록 덜 유사

## RAG 구현 예시

강의의 구현은 다섯 단계로 정리됩니다.

### 1. 문서를 섹션별로 나누기

```python
with open("./report.md", "r") as f:
    text = f.read()

chunks = chunk_by_section(text)
chunks[2]
```

`chunk_by_section`으로 문서를 논리적 섹션 단위로 나눕니다.

### 2. 각 chunk의 임베딩 만들기

```python
embeddings = generate_embedding(chunks)
```

강의 코드에서는 `generate_embedding`이 문자열 하나뿐 아니라 문자열 리스트도 처리할 수 있게 확장됩니다. 이렇게 하면 chunk를 하나씩 호출하는 것보다 효율적으로 batch 처리할 수 있습니다.

### 3. 벡터 DB에 저장하기

```python
store = VectorIndex()

for embedding, chunk in zip(embeddings, chunks):
    store.add_vector(embedding, {"content": chunk})
```

중요한 점은 embedding만 저장하지 않고 **원문 chunk도 함께 저장**한다는 것입니다. 검색 결과로 숫자 벡터만 받으면 Claude에게 넣을 근거 텍스트가 없습니다. 따라서 벡터와 함께 원문 또는 원문을 찾을 수 있는 참조를 저장해야 합니다.

### 4. 사용자 질문을 임베딩으로 바꾸기

```python
user_embedding = generate_embedding(
    "What did the software engineering dept do last year?"
)
```

질문도 문서 chunk와 같은 임베딩 공간으로 변환합니다.

### 5. 가장 관련 있는 chunk 찾기

```python
results = store.search(user_embedding, 2)

for doc, distance in results:
    print(distance, "\n", doc["content"][0:200], "\n")
```

이 검색은 질문과 가장 가까운 chunk 2개를 반환합니다. 예시에서는 Software Engineering 섹션이 distance `0.71`로 가장 가까웠고, Methodology 섹션이 `0.72`로 두 번째였습니다. distance는 낮을수록 더 유사합니다.

마지막으로 검색된 chunk를 프롬프트에 넣습니다.

```text
Answer the user's question about the financial document.

<user_question>
How many bugs did engineers fix this year?
</user_question>

<report>
## Section 2: Software Engineering
This division dedicated significant effort to studying various infection vectors in our distributed systems
</report>
```

이제 Claude는 전체 문서가 아니라 질문과 관련된 근거만 보고 답할 수 있습니다.

## BM25 어휘 검색

임베딩(의미 검색)만으로는 **정확한 키워드·희귀 용어·코드·ID** 매칭에 약할 수 있어요. **BM25는 단어 빈도 기반의 고전적 어휘 검색**으로, 정확한 단어 일치에 강해요.

예를 들어 사용자가 `INC-2023-Q4-011` 같은 특정 incident ID를 검색한다고 해봅시다. 의미 검색은 문맥적으로 관련 있어 보이는 보안/운영 섹션을 찾을 수 있지만, 정작 그 ID가 들어 있지 않은 섹션도 함께 반환할 수 있습니다. 의미 검색은 "개념적으로 비슷한가"를 보기 때문입니다.

이럴 때는 어휘 검색이 필요합니다. BM25는 검색어를 토큰으로 나누고, 각 단어가 문서들에서 얼마나 자주 등장하는지 계산합니다. 자주 등장하는 일반 단어는 낮게, 드물게 등장하는 중요한 단어는 높게 가중합니다.

BM25의 흐름은 다음과 같습니다.

1. 사용자 질문을 토큰으로 나눔
2. 각 토큰이 문서 전체에서 얼마나 자주 등장하는지 계산
3. 드문 단어에 더 높은 중요도를 부여
4. 중요한 단어가 많이 포함된 문서를 상위 결과로 반환

예를 들어 `"a INC-2023-Q4-011"`이라는 질의에서 `a`는 흔한 단어라 중요도가 낮고, `INC-2023-Q4-011`은 희귀한 식별자라 중요도가 높습니다.

간단한 구현은 다음과 같습니다.

```python
chunks = chunk_by_section(text)

store = BM25Index()
for chunk in chunks:
    store.add_document({"content": chunk})

results = store.search("What happened with INC-2023-Q4-011?", 3)

for doc, distance in results:
    print(distance, "\n", doc["content"][:200], "\n----\n")
```

BM25는 다음 종류의 질의에 특히 강합니다.

- incident ID, ticket ID, 주문 번호
- 함수명, 파일명, 에러 코드
- 제품명, 고유명사, 약어
- 반드시 포함되어야 하는 특정 문구

| 방식 | 강점 | 약점 |
|---|---|---|
| 임베딩(의미) | 의역·유사 개념 | 정확한 키워드·희귀어 |
| BM25(어휘) | 정확한 단어 일치 | 동의어·의역 |

## 멀티 인덱스 RAG 파이프라인

의미 검색과 어휘 검색은 서로 보완적입니다. 실전에서는 두 검색을 병렬로 실행한 뒤 결과를 합치는 **하이브리드 검색**이 더 견고합니다.

```text
사용자 질문
  ├─ embedding 검색: 의미적으로 가까운 chunk
  └─ BM25 검색: 정확한 단어가 포함된 chunk
        ↓
결과 병합 → 중복 제거 → 점수 정규화/재정렬 → Claude 프롬프트 구성
```

이 구조를 멀티 인덱스 RAG 파이프라인이라고 볼 수 있습니다. 같은 문서 집합을 여러 방식으로 색인해두고, 질의 시점에 여러 검색 결과를 조합합니다.

강의에서는 `VectorIndex`와 `BM25Index`가 거의 같은 API를 갖도록 만듭니다.

- `add_document()`: 문서를 인덱스에 추가
- `search()`: 질의에 맞는 문서를 검색

이렇게 인터페이스를 맞춰두면 두 인덱스를 감싸는 `Retriever` 클래스를 만들 수 있습니다. `Retriever`는 사용자의 질의를 여러 인덱스에 전달하고, 각 인덱스의 결과를 모아 하나의 순위 목록으로 합칩니다.

```python
class Retriever:
    def __init__(self, *indexes: SearchIndex):
        if len(indexes) == 0:
            raise ValueError("At least one index must be provided")
        self._indexes = list(indexes)

    def add_document(self, document):
        for index in self._indexes:
            index.add_document(document)

    def search(self, query_text, k=1, k_rrf=60):
        # 1. 모든 인덱스에서 검색
        # 2. 각 문서의 rank를 수집
        # 3. RRF 점수로 병합
        # 4. 점수순으로 정렬해 반환
        ...
```

핵심은 각 검색 구현을 `Retriever`가 직접 알 필요 없게 만드는 것입니다. 새 검색 방식도 같은 `SearchIndex` 인터페이스만 구현하면 파이프라인에 쉽게 추가할 수 있습니다.

### Reciprocal Rank Fusion (RRF)

검색 결과를 합칠 때 단순히 리스트를 이어 붙이면 안 됩니다. 벡터 검색과 BM25는 점수 체계가 다르기 때문입니다. 한쪽은 cosine distance를 쓰고, 다른 쪽은 BM25 점수를 쓸 수 있어요. 점수값 자체를 바로 비교하면 공정하지 않습니다.

그래서 강의에서는 **Reciprocal Rank Fusion(RRF)**을 사용합니다. RRF는 원래 점수 대신 **각 검색 결과에서의 순위(rank)**를 사용해 결과를 합칩니다.

```text
RRF_score(d) = Σ(1 / (k + rank_i(d)))
```

- `d`: 점수를 계산할 문서
- `rank_i(d)`: i번째 검색 결과에서 문서 d의 순위
- `k`: 순위 영향도를 완화하는 상수. 보통 60을 쓰지만, 예시에서는 이해를 위해 1을 사용

예를 들어 `INC-2023-Q4-011`을 검색했을 때 두 인덱스가 다음 결과를 냈다고 해봅시다.

```text
VectorIndex: Section 2(rank 1), Section 7(rank 2), Section 6(rank 3)
BM25Index:   Section 6(rank 1), Section 2(rank 2), Section 7(rank 3)
```

`k=1`로 계산하면:

```text
Section 2 = 1/(1+1) + 1/(1+2) = 0.833
Section 7 = 1/(1+2) + 1/(1+3) = 0.583
Section 6 = 1/(1+3) + 1/(1+1) = 0.750
```

최종 순위는 다음처럼 됩니다.

```text
1. Section 2: 0.833
2. Section 6: 0.750
3. Section 7: 0.583
```

Section 2는 두 인덱스 모두에서 좋은 순위를 받았기 때문에 1등이 됩니다. 이 방식은 특정 인덱스 하나의 점수 체계에 끌려가지 않고, 여러 검색 방식에서 안정적으로 상위에 나온 문서를 올려줍니다.

### 하이브리드 검색 테스트

이전에는 `"what happened with INC-2023-Q4-011?"` 같은 질의에서 벡터 검색만 쓰면 문제가 있었습니다. Cybersecurity 섹션은 잘 찾았지만, 두 번째 결과로 실제 incident ID와 관련 없는 Financial Analysis 섹션이 나올 수 있었어요.

하이브리드 `Retriever`를 쓰면 결과가 더 좋아집니다.

- Section 10: Cybersecurity Analysis - Incident Response Report
- Section 2: Software Engineering - Project Phoenix Stability Enhancements
- Section 5: Legal Developments

의미 검색은 "incident response와 관련된 문맥"을 잡고, BM25는 `INC-2023-Q4-011`이라는 정확한 식별자를 잡습니다. 둘을 합치면 한쪽 방식만 쓸 때보다 안정적인 결과가 나옵니다.

실전에서 고려할 점은 다음과 같습니다.

- 두 검색 결과의 점수 범위가 다르므로 그대로 비교하기 어렵다
- 같은 chunk가 양쪽에서 나올 수 있으므로 중복 제거가 필요하다
- 검색 결과가 많으면 Claude에 넣기 전에 reranker로 다시 정렬할 수 있다
- 최종 프롬프트에는 top-k 전체가 아니라 답변에 필요한 만큼만 넣어야 한다

의미 검색은 "비슷한 말"을 찾고, BM25는 "정확한 말"을 찾습니다. 둘을 합치면 개념 질문과 특정 식별자 검색을 모두 더 안정적으로 처리할 수 있습니다.

이 아키텍처의 장점은 확장성입니다. 키워드 기반 인덱스, 그래프 기반 검색, 도메인 특화 인덱스가 필요해지면 같은 `add_document()`/`search()` 인터페이스만 구현하면 됩니다. 각 검색 방식은 독립적으로 테스트하고, `Retriever`는 결과 병합만 담당하게 만들 수 있습니다.

## 에이전틱 검색

지금까지의 RAG는 고정 파이프라인입니다. 질문이 들어오면 정해진 방식으로 검색하고, 검색 결과를 넣고, Claude가 답합니다.

에이전틱 검색은 여기서 한 단계 더 나아갑니다. Claude에게 검색을 도구로 주고, Claude가 스스로 다음을 판단하게 합니다.

- 어떤 검색어를 만들지
- 의미 검색과 BM25 중 무엇을 쓸지
- 검색을 한 번 더 해야 하는지
- 검색 결과가 충분한지
- 질문을 여러 하위 질문으로 나눠야 하는지

앞에서 배운 tool use와 연결하면, 검색도 하나의 도구가 됩니다. 복잡한 질문은 한 번의 검색으로 충분하지 않을 수 있으므로 Claude가 여러 번 검색하고 중간 결과를 바탕으로 다음 검색을 계획하게 할 수 있습니다.

다만 에이전틱 검색은 더 강력한 만큼 비용과 복잡도가 증가합니다. 단순 FAQ나 문서 Q&A라면 고정 RAG가 충분하고, 여러 문서와 여러 단계의 추론이 필요한 리서치형 질문에서는 에이전틱 검색이 더 적합합니다.

## 정리

- RAG는 긴 문서를 통째로 넣지 않고, 질문과 관련된 chunk만 검색해 프롬프트에 넣는 방식입니다.
- 청킹 품질이 RAG 품질을 크게 좌우합니다. 구조 기반, 문장 기반, 크기 기반 + overlap을 상황에 맞게 선택합니다.
- 임베딩은 텍스트 의미를 숫자 벡터로 바꾸고, 벡터 DB는 질문과 가까운 chunk를 찾습니다.
- 검색 결과에는 embedding뿐 아니라 원문 chunk 또는 참조를 함께 저장해야 합니다.
- BM25는 정확한 키워드, ID, 코드, 고유명사 검색에 강합니다.
- 의미 검색과 BM25를 결합한 하이브리드/멀티 인덱스 RAG가 실전에서 더 견고합니다.
- Claude에게 검색을 도구로 맡기면 고정 파이프라인을 넘어 에이전틱 검색을 만들 수 있습니다.
