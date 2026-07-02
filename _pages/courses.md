---
title: ""
layout: default
permalink: /courses/
author_profile: false
---

<div class="course-shell">
  <header class="course-head">
    <h1>강의 정리</h1>
    <p>온라인 강의를 학습 노트로 정리한 모음입니다. 강의를 선택해주세요.</p>
  </header>

  {% for track in site.data.courses.tracks %}
  {% assign tcourses = site.data.courses.courses | where: "track", track.slug %}
  {% assign tdone = tcourses | where: "done", true %}
  <div class="course-card">
    <div class="course-card__meta">
      <span class="course-card__label">{{ track.label }}</span>
      <span class="course-badge">{{ tdone.size }} / {{ tcourses.size }} 정리 완료</span>
    </div>
    <h2 class="course-card__title"><a href="{{ track.url | relative_url }}">{{ track.title }}</a></h2>
    <p class="course-card__desc">{{ track.description }}</p>
    <a class="course-card__cta" href="{{ track.url | relative_url }}">강의 목록 보기 →</a>
  </div>
  {% endfor %}
</div>
