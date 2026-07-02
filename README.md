# 정리 노트 블로그

논문 정리와 강의 필기를 깔끔하게 기록하는 GitHub Pages 블로그.
[Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes) 테마 + 파스텔 노랑 커스텀 스타일.

👉 **사이트: https://minari1505.github.io**

## 🎓 강의 정리 (/courses/) 쓰는 법

강의는 포스트가 아니라 **레슨 컬렉션**으로 정리해요. junoflows.github.io/courses/anthropic/ 구조를 참고했어요.

1. `_data/courses.yml`에 강의(course)와 레슨 목록을 등록 (사이드바 커리큘럼 순서가 여기서 나와요)
2. `_lessons/<강의slug>/<NN>-<레슨slug>.md`에 레슨 본문 작성. frontmatter:

   ```yaml
   ---
   title: "What are skills?"      # 영어 원제
   title_ko: "Skill이란 무엇인가?"  # 한글 부제
   course: agent-skills            # courses.yml의 slug
   lesson: 1                       # 레슨 번호 (숫자)
   ---
   ```

3. push하면 `/courses/<강의slug>/<NN>-<레슨slug>/`에 사이드바(진행률+커리큘럼) 딸린 페이지가 생겨요.

## ✏️ 새 정리 글 쓰는 법

`_posts/` 폴더에 `YYYY-MM-DD-제목.md` 형식으로 파일을 만들면 끝이에요.

예) `_posts/2026-07-01-first-lecture.md`

```markdown
---
title: "강의 제목 정리 ✏️"
date: 2026-07-01
categories:
  - 강의이름        # 예: Claude-Code
tags:
  - 키워드1         # 예: 프롬프트
  - 키워드2
---

여기에 강의 내용을 정리하면 돼요!

## 소제목

- 요점 정리
- 배운 점
```

- `categories`에 적은 이름이 **📚 논문 정리** 메뉴에 묶여요.
- `tags`에 적은 키워드가 **🏷️ 태그** 메뉴에 모여요.
- 파일을 저장하고 GitHub에 push하면 1~2분 뒤 자동으로 사이트에 반영돼요.

## 🎨 색/분위기 바꾸기

- 포인트 색상: `assets/css/main.scss` 맨 위의 `$primary-color` 등 변수만 바꾸면 돼요.
- 프로필 그림: `assets/images/avatar.svg`를 원하는 이미지로 교체 (PNG면 `_config.yml`의 `avatar` 경로도 같이 수정).
- 사이트 제목/소개: `_config.yml`의 `title`, `subtitle`, `description`.

## 🖥️ 로컬에서 미리보기 (선택)

```bash
bundle install
bundle exec jekyll serve
# http://localhost:4000 접속
```
