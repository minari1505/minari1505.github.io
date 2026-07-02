---
title: "Features of Claude"
title_ko: "Claude의 기능들"
course: claude-with-the-anthropic-api
lesson: 7
---

## 학습 목표

- Claude의 주요 고급 기능들을 언제 쓰는지 파악하기
- extended thinking, 이미지·PDF 입력, 인용, 프롬프트 캐싱, 코드 실행을 이해하기

## 1. Extended Thinking (확장 사고)

복잡한 문제에서 Claude가 **답하기 전에 단계적으로 추론**하게 해요. 수학, 다단계 논리, 어려운 코딩에서 정확도가 올라가요.

```python
response = client.messages.create(
    model="claude-opus-4-8", max_tokens=16000,
    thinking={"type": "adaptive"},        # Claude가 사고량을 알아서 조절
    output_config={"effort": "high"},     # low | medium | high | max
    messages=[{"role": "user", "content": "단계별로 풀어줘: ..."}],
)
```

- 최신 모델은 **adaptive thinking**을 써요 — 얼마나 생각할지 모델이 스스로 정해요.
- `effort`로 사고 깊이·토큰 사용량을 조절해요. 응답엔 `thinking` 블록이 먼저 오고 그다음 `text`가 와요.

## 2. 이미지 입력 (Vision)

이미지를 입력으로 넣어 설명·분석·표 추출 등을 시킬 수 있어요. base64 또는 URL로 전달해요.

```python
messages=[{"role": "user", "content": [
    {"type": "image", "source": {"type": "url", "url": "https://.../chart.png"}},
    {"type": "text", "text": "이 차트를 설명해줘"},
]}]
```

## 3. PDF 입력

PDF를 통째로 넣어 요약·질의할 수 있어요. 텍스트와 페이지 이미지를 함께 이해해요.

```python
messages=[{"role": "user", "content": [
    {"type": "document",
     "source": {"type": "base64", "media_type": "application/pdf", "data": pdf_b64}},
    {"type": "text", "text": "이 보고서의 핵심을 요약해줘"},
]}]
```

- 큰 파일은 **Files API**로 한 번 업로드하고 `file_id`로 재사용하면 효율적이에요.

## 4. 인용 (Citations)

문서 블록에 `citations: {"enabled": true}`를 켜면, Claude의 답변이 **어느 부분에서 나왔는지 출처**를 함께 반환해요. 근거를 검증할 수 있어 RAG·문서 QA에 좋아요.

## 5. 프롬프트 캐싱 (Prompt Caching)

같은 큰 컨텍스트(긴 시스템 프롬프트, 문서, 도구 목록)를 반복해서 보낼 때, **캐시**해두면 다음 요청에서 그 부분을 최대 ~90% 저렴하게 처리해요.

### 캐싱의 핵심 규칙

- 캐싱은 **접두사(prefix) 일치**예요. 앞부분이 한 바이트라도 바뀌면 그 뒤 캐시가 전부 무효화돼요.
- 렌더 순서는 `tools` → `system` → `messages`. **안정적인 내용은 앞에, 매번 바뀌는 내용(타임스탬프·랜덤 ID)은 뒤에** 두세요.
- `cache_control: {"type": "ephemeral"}`을 캐시할 블록에 붙여요.
- `usage.cache_read_input_tokens`로 캐시 적중을 확인해요. 계속 0이면 앞부분에 매번 바뀌는 값이 섞여 있는 거예요.

## 6. 코드 실행 & Files API

**코드 실행 도구(code execution)**는 Anthropic의 샌드박스에서 Claude가 **직접 Python 코드를 실행**하게 해요. 데이터 분석, 계산, 차트 생성 등에 유용해요. 서버 측에서 실행되므로 내가 실행 환경을 관리할 필요가 없어요.

```python
response = client.messages.create(
    model="claude-opus-4-8", max_tokens=4096,
    tools=[{"type": "code_execution_20260120", "name": "code_execution"}],
    messages=[{"role": "user", "content": "[1..10]의 평균과 표준편차 계산해줘"}],
)
```

- **Files API**로 CSV·엑셀을 업로드해 코드 실행에 넘기고, 생성된 파일(차트 등)을 다시 다운로드할 수 있어요.

## 정리

- **thinking**: 어려운 문제엔 adaptive thinking + effort로 추론 강화.
- **이미지·PDF**: 멀티모달 입력, 큰 파일은 Files API로 재사용.
- **인용**: 답변의 출처를 함께 받기.
- **캐싱**: 반복되는 큰 컨텍스트를 접두사 캐시로 절약 (안정적인 건 앞에).
- **코드 실행**: 샌드박스에서 Claude가 직접 코드 실행.
