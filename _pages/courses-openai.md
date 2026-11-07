---
title: ""
layout: default
permalink: /courses/openai/
author_profile: false
---

{% assign track = site.data.courses.tracks | where: "slug", "openai" | first %}
{% assign tcourses = site.data.courses.courses | where: "track", "openai" %}
{% assign tdone = tcourses | where: "done", true %}

<div class="course-shell">
  <header class="course-head">
    <p class="course-head__back"><a href="{{ '/courses/' | relative_url }}">← 강의 정리</a></p>
    <h1>{{ track.title }}</h1>
    <p>OpenAI 파트너 교육 플랫폼(<a href="https://openai-partneru.sana.ai/discover" target="_blank" rel="noopener">OpenAI PartnerU</a>)의 Program을 한글로 옮겨 정리한 학습 노트입니다. Program 하나가 강의 하나이고, 안의 코스들이 레슨이 됩니다.</p>
  </header>

  <p class="course-count">전체 {{ tcourses.size }}개 강의 중 <strong>{{ tdone.size }}개</strong> 정리 완료</p>

  <div class="course-grid">
    {% for c in tcourses %}
    {% assign first = c.lessons | first %}
    <div class="course-card course-card--grid">
      <div class="course-card__meta">
        <span class="course-card__label">{{ c.num }}</span>
        {% if c.done %}<span class="course-badge">✓ 정리 완료</span>{% elsif first %}<span class="course-badge course-badge--todo">정리 중</span>{% else %}<span class="course-badge course-badge--todo">정리 예정</span>{% endif %}
      </div>
      <h2 class="course-card__title">
        {% if first %}<a href="{{ '/courses/' | append: c.slug | append: '/' | append: first.num | append: '-' | append: first.slug | append: '/' | relative_url }}">{{ c.title }}</a>{% else %}{{ c.title }}{% endif %}
      </h2>
      <p class="course-card__desc">{{ c.description }}</p>
      <a class="course-card__source" href="{{ c.source }}" target="_blank" rel="noopener">원문 ↗</a>
    </div>
    {% endfor %}
  </div>
</div>
