---
title: ""
layout: default
permalink: /courses/anthropic/
author_profile: false
---

{% assign track = site.data.courses.tracks | where: "slug", "anthropic" | first %}
{% assign tcourses = site.data.courses.courses | where: "track", "anthropic" %}
{% assign tdone = tcourses | where: "done", true %}

<div class="course-shell">
  <header class="course-head">
    <p class="course-head__back"><a href="{{ '/courses/' | relative_url }}">← 강의 정리</a></p>
    <h1>{{ track.title }}</h1>
    <p>Anthropic Academy의 영어 강의(<a href="https://anthropic.skilljar.com" target="_blank" rel="noopener">anthropic.skilljar.com</a>)를 한글로 옮겨 정리한 학습 노트입니다. 정리된 강의를 클릭하면 레슨별로 살펴볼 수 있습니다.</p>
  </header>

  <p class="course-count">전체 {{ tcourses.size }}개 강의 중 <strong>{{ tdone.size }}개</strong> 정리 완료</p>

  <div class="course-grid">
    {% for c in tcourses %}
    {% assign first = c.lessons | first %}
    <div class="course-card course-card--grid">
      <div class="course-card__meta">
        <span class="course-card__label">{{ c.num }}</span>
        {% if c.done %}<span class="course-badge">✓ 정리 완료</span>{% else %}<span class="course-badge course-badge--todo">정리 예정</span>{% endif %}
      </div>
      <h2 class="course-card__title">
        {% if c.done %}<a href="{{ '/courses/' | append: c.slug | append: '/' | append: first.num | append: '-' | append: first.slug | append: '/' | relative_url }}">{{ c.title }}</a>{% else %}{{ c.title }}{% endif %}
      </h2>
      <p class="course-card__desc">{{ c.description }}</p>
      <a class="course-card__source" href="{{ c.source }}" target="_blank" rel="noopener">원문 ↗</a>
    </div>
    {% endfor %}
  </div>
</div>
