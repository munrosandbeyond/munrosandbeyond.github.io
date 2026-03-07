---
layout: default
---

<div class="archive-section">
  <h1 style="font-family: var(--font-heading); font-weight: 800; letter-spacing: -0.03em; margin-bottom: 0.5rem;">Blog Archive</h1>
  <p style="color: var(--color-text-light); margin-bottom: 2rem;">All adventures, organised by tag.</p>

  {% for tag in site.tags %}
  <h2>{{ tag[0] }}</h2>
  <ul class="archive-list">
    {% for post in tag[1] %}
    <li>
      <a href="{{ post.url }}">
        <span class="archive-date">{{ post.date | date: "%B %Y" }}</span>
        {{ post.title }}
      </a>
    </li>
    {% endfor %}
  </ul>
  {% endfor %}
</div>
