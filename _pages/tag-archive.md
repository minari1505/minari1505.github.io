---
title: ""
layout: default
permalink: /tags/
author_profile: false
---

{%- capture tags_raw -%}
{%- for post in site.posts -%}
{%- for tag in post.tags -%}{{ tag }}||{%- endfor -%}
{%- endfor -%}
{%- for lesson in site.lessons -%}
{%- for tag in lesson.tags -%}{{ tag }}||{%- endfor -%}
{%- endfor -%}
{%- endcapture -%}
{% assign tag_names = tags_raw | split: "||" | uniq | sort_natural %}
{% assign visible_tag_count = 0 %}
{% for tag in tag_names %}
{% if tag != "" %}{% assign visible_tag_count = visible_tag_count | plus: 1 %}{% endif %}
{% endfor %}

<main class="tag-archive">
  <header class="tag-archive__head">
    <h1>Tags</h1>
    <p>{{ visible_tag_count }}개 태그</p>
  </header>

  <nav class="tag-cloud" aria-label="태그 목록">
    {% for tag in tag_names %}
    {% if tag != "" %}
    {% assign tag_id = tag | downcase | replace: ' ', '-' %}
    {% assign tag_count = 0 %}
    {% for post in site.posts %}
    {% if post.tags contains tag %}{% assign tag_count = tag_count | plus: 1 %}{% endif %}
    {% endfor %}
    {% for lesson in site.lessons %}
    {% if lesson.tags contains tag %}{% assign tag_count = tag_count | plus: 1 %}{% endif %}
    {% endfor %}
    <a href="#{{ tag_id }}">#{{ tag }} <span>{{ tag_count }}</span></a>
    {% endif %}
    {% endfor %}
  </nav>

  {% for tag in tag_names %}
  {% if tag != "" %}
  {% assign tag_id = tag | downcase | replace: ' ', '-' %}
  {% assign tag_count = 0 %}
  {% for post in site.posts %}
  {% if post.tags contains tag %}{% assign tag_count = tag_count | plus: 1 %}{% endif %}
  {% endfor %}
  {% for lesson in site.lessons %}
  {% if lesson.tags contains tag %}{% assign tag_count = tag_count | plus: 1 %}{% endif %}
  {% endfor %}

  <section id="{{ tag_id }}" class="tag-group">
    <h2>#{{ tag }} <span>{{ tag_count }}</span></h2>
    <ul>
      {% for post in site.posts %}
      {% if post.tags contains tag %}
      <li>
        <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%Y.%m.%d" }}</time>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </li>
      {% endif %}
      {% endfor %}

      {% assign lessons_by_path = site.lessons | sort: "path" %}
      {% for lesson in lessons_by_path %}
      {% if lesson.tags contains tag %}
      {% assign course = site.data.courses.courses | where: "slug", lesson.course | first %}
      <li>
        <span>{{ course.title }}</span>
        <a href="{{ lesson.url | relative_url }}">{{ lesson.title }}</a>
      </li>
      {% endif %}
      {% endfor %}
    </ul>
  </section>
  {% endif %}
  {% endfor %}
</main>
