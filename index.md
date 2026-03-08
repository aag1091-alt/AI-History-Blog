---
layout: default
title: Home
---

<div class="home-hero">
  <h1>AI History: Milestones That Shaped the Future</h1>
  <p>From Mary Shelley's Frankenstein to ChatGPT — every idea, breakthrough, and cultural moment that built the AI world we live in.</p>
  <div class="home-stats">
    <div class="stat">
      <span class="stat-number">{{ site.posts | size }}</span>
      <span class="stat-label">Milestones</span>
    </div>
    <div class="stat">
      <span class="stat-number">200+</span>
      <span class="stat-label">Years of history</span>
    </div>
    <div class="stat">
      <span class="stat-number">∞</span>
      <span class="stat-label">Growing daily</span>
    </div>
  </div>
</div>

<p class="section-title">Latest Milestones</p>

<ul class="post-list">
  {% for post in site.posts limit: 20 %}
  <li class="post-card">
    <div class="post-card-top">
      {% if post.era %}<span class="era-badge">{{ post.era }}</span>{% endif %}
      {% if post.category %}<span class="category-badge {% if post.category == 'Film & Fiction' %}film{% endif %}">{{ post.category }}</span>{% endif %}
    </div>
    <div class="post-card-title"><a href="{{ post.url | relative_url }}">{{ post.title }}</a></div>
    {% if post.subtitle %}<p class="post-card-excerpt">{{ post.subtitle }}</p>{% endif %}
    <div class="post-card-meta">
      <span class="post-card-date">{{ post.event_date | default: post.date | date: "%Y" }}</span>
      {% if post.location %}<span>📍 {{ post.location }}</span>{% endif %}
    </div>
  </li>
  {% endfor %}
</ul>

<p style="margin-top: 1.5rem; font-size: 0.88rem; color: var(--text-muted);">
  <a href="{{ '/timeline' | relative_url }}">View the full timeline →</a>
</p>
