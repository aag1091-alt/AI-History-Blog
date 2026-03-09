---
layout: default
title: Home
---


<div id="this-day" class="this-day-box" style="display:none">
  <span class="this-day-label">📅 This day in AI history</span>
  <a id="this-day-link" href="#" class="this-day-title"></a>
  <span id="this-day-year" class="this-day-year"></span>
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
  <li class="post-card" data-category="{{ post.category }}" data-era="{{ post.era }}" data-post-id="{{ post.id }}">
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
(function() {
  const read = JSON.parse(localStorage.getItem('ai-history-read') || '[]');
  if (!read.length) return;
  document.querySelectorAll('#post-list .post-card[data-post-id]').forEach(card => {
    if (read.includes(card.dataset.postId)) card.classList.add('post-card--read');
  });
})();

// This day in AI history
(function() {
  const posts = [{% for post in site.posts %}{"title":{{ post.title | jsonify }},"url":{{ post.url | relative_url | jsonify }},"date":{{ post.event_date | jsonify }}}{% unless forloop.last %},{% endunless %}{% endfor %}];
  const today = new Date();
  const mm = String(today.getMonth() + 1).padStart(2, '0');
  const dd = String(today.getDate()).padStart(2, '0');
  const match = posts.find(p => p.date && p.date.slice(5, 10) === mm + '-' + dd);
  if (match) {
    document.getElementById('this-day-link').textContent = match.title;
    document.getElementById('this-day-link').href = match.url;
    document.getElementById('this-day-year').textContent = match.date.slice(0, 4);
    document.getElementById('this-day').style.display = '';
  }
})();
</script>

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
