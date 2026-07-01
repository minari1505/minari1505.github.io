# 🌸 강의 정리 블로그

강의 내용을 귀엽고 깔끔하게 정리하는 GitHub Pages 블로그.
[Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes) 테마 + 파스텔 핑크 커스텀 스타일.

👉 **사이트: https://minari1505.github.io**

## ✏️ 새 강의 정리 글 쓰는 법

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

여기에 강의 내용을 정리하면 돼요! 🌸

## 소제목

- 요점 정리
- 배운 점
```

- `categories`에 적은 이름이 **📚 강의 정리** 메뉴에 묶여요.
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
