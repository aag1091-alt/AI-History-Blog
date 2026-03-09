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

<div class="list-controls">
  <p class="section-title" style="margin:0">Latest Milestones</p>
  <input type="search" id="post-search" class="search-input" placeholder="Search milestones…" aria-label="Search milestones">
</div>

<div class="filter-pills" id="category-filters">
  <button class="filter-pill active" data-filter="">All</button>
  <button class="filter-pill" data-filter="Research">Research</button>
  <button class="filter-pill" data-filter="Film &amp; Fiction">Film &amp; Fiction</button>
  <button class="filter-pill" data-filter="Commercial">Commercial</button>
  <button class="filter-pill" data-filter="Culture">Culture</button>
  <button class="filter-pill" data-filter="Policy">Policy</button>
</div>

<p id="no-results" class="no-results" style="display:none">No milestones match your search.</p>

<ul class="post-list" id="post-list">
  {% for post in site.posts limit: 20 %}
  <li class="post-card" data-category="{{ post.category }}" data-era="{{ post.era }}">
    {% if post.image %}
    <a href="{{ post.url | relative_url }}" class="post-card-thumb">
      <img src="{{ post.image | relative_url }}" alt="{{ post.title }}" loading="lazy">
    </a>
    {% else %}
    <div class="post-card-thumb post-card-thumb--empty">
      <span>{{ post.event_date | date: "%Y" }}</span>
    </div>
    {% endif %}
    <div class="post-card-body">
      <div class="post-card-top">
        {% if post.era %}<span class="era-badge">{{ post.era | split: '(' | first | strip }}</span>{% endif %}
        {% if post.category %}<span class="category-badge cat-{{ post.category | downcase | replace: ' & ', '-' | replace: ' ', '-' }}">{{ post.category }}</span>{% endif %}
      </div>
      <div class="post-card-title"><a href="{{ post.url | relative_url }}">{{ post.title }}</a></div>
      {% if post.subtitle %}<p class="post-card-excerpt">{{ post.subtitle }}</p>{% endif %}
      <div class="post-card-meta">
        <span class="post-card-date">{{ post.event_date | default: post.date | date: "%Y" }}</span>
        {% if post.location %}<span>📍 {{ post.location }}</span>{% endif %}
      </div>
    </div>
  </li>
  {% endfor %}
</ul>

<p style="margin-top: 1.5rem; font-size: 0.88rem; color: var(--text-muted);">
  <a href="{{ '/timeline' | relative_url }}">View the full timeline →</a>
</p>

<script>
const search  = document.getElementById('post-search');
const cards   = document.querySelectorAll('#post-list .post-card');
const noRes   = document.getElementById('no-results');
const pills   = document.querySelectorAll('#category-filters .filter-pill');
let activeFilter = '';

function applyFilters() {
  const q = search.value.trim().toLowerCase();
  let visible = 0;
  cards.forEach(card => {
    const text     = card.textContent.toLowerCase();
    const category = card.dataset.category || '';
    const matchQ   = !q || text.includes(q);
    const matchF   = !activeFilter || category === activeFilter;
    const show     = matchQ && matchF;
    card.style.display = show ? '' : 'none';
    if (show) visible++;
  });
  noRes.style.display = visible === 0 ? '' : 'none';
}

search.addEventListener('input', applyFilters);

pills.forEach(pill => {
  pill.addEventListener('click', () => {
    pills.forEach(p => p.classList.remove('active'));
    pill.classList.add('active');
    activeFilter = pill.dataset.filter;
    applyFilters();
  });
});
</script>
