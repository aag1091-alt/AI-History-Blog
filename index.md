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
  <div class="view-toggle" id="view-toggle">
    <button class="view-btn" data-view="cards">Cards</button>
    <button class="view-btn" data-view="timeline">Timeline</button>
  </div>
  <input type="search" id="post-search" class="search-input" placeholder="Search milestones… (press /)" aria-label="Search milestones">
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

<!-- ─── Cards view ─── -->
<div id="view-cards">
  <ul class="post-list" id="post-list">
    {% for post in site.posts %}
    <li class="post-card" data-category="{{ post.category }}" data-era="{{ post.era }}" data-post-id="{{ post.id }}" data-search="{{ post.title | downcase }} {{ post.subtitle | downcase }} {{ post.category | downcase }} {{ post.era | downcase }} {{ post.location | downcase }}">
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
          {% if post.era %}{% assign era_page = site.eras | where: "name", post.era | first %}{% if era_page %}<a href="{{ era_page.url | relative_url }}" class="era-badge">{{ post.era | split: '(' | first | strip }}</a>{% else %}<span class="era-badge">{{ post.era | split: '(' | first | strip }}</span>{% endif %}{% endif %}
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
</div>

<!-- ─── Timeline view ─── -->
<div id="view-timeline" style="display:none">
  {% assign sorted_posts = site.posts | sort: "event_date" %}
  {% assign eras_seen = "" | split: "" %}
  {% for post in sorted_posts %}
    {% unless eras_seen contains post.era %}
      {% assign eras_seen = eras_seen | push: post.era %}
    {% endunless %}
  {% endfor %}

  {% for era in eras_seen %}
  {% assign era_items = sorted_posts | where: "era", era %}
  <div class="timeline-era" data-era="{{ era }}">
    <button class="timeline-era-title" aria-expanded="true">
      <span class="timeline-era-name">{% if era %}{{ era }}{% else %}Unknown Era{% endif %}</span>
      <span class="timeline-era-right">
        <span class="timeline-era-count">{{ era_items | size }}</span>
        <span class="era-toggle-icon">▾</span>
      </span>
    </button>
    <div class="timeline-list">
      {% for post in era_items %}
      <a href="{{ post.url | relative_url }}" class="milestone-row" data-category="{{ post.category }}" data-search="{{ post.title | downcase }} {{ post.subtitle | downcase }} {{ post.category | downcase }} {{ post.era | downcase }}">
        <div class="milestone-row-year">{{ post.event_date | default: post.date | date: "%Y" }}</div>
        <div class="milestone-row-body">
          <div class="milestone-row-cat cat-{{ post.category | downcase | replace: ' & ', '-' | replace: ' ', '-' }}">{{ post.category }}</div>
          <div class="milestone-row-title">{{ post.title }}</div>
          {% if post.subtitle %}<p class="milestone-row-sub">{{ post.subtitle }}</p>{% endif %}
        </div>
      </a>
      {% endfor %}
    </div>
  </div>
  {% endfor %}
</div>

<script>
(function () {
  // ─── State ───────────────────────────────────────────────────────────────
  const params       = new URLSearchParams(location.search);
  let currentView    = params.get('view') || 'cards';
  let activeFilter   = params.get('cat')  || '';
  let searchQuery    = params.get('q')    || '';

  // ─── Elements ─────────────────────────────────────────────────────────────
  const searchEl      = document.getElementById('post-search');
  const pills         = document.querySelectorAll('#category-filters .filter-pill');
  const viewBtns      = document.querySelectorAll('#view-toggle .view-btn');
  const viewCards     = document.getElementById('view-cards');
  const viewTimeline  = document.getElementById('view-timeline');
  const noRes         = document.getElementById('no-results');
  const postCards     = document.querySelectorAll('#post-list .post-card');
  const milestoneRows = document.querySelectorAll('#view-timeline .milestone-row');
  const eraBlocks     = document.querySelectorAll('#view-timeline .timeline-era');

  // ─── URL sync ─────────────────────────────────────────────────────────────
  function updateURL() {
    const p = new URLSearchParams();
    if (currentView !== 'cards') p.set('view', currentView);
    if (activeFilter) p.set('cat', activeFilter);
    if (searchQuery)  p.set('q', searchQuery);
    const newURL = location.pathname + (p.toString() ? '?' + p.toString() : '');
    history.pushState(null, '', newURL);
  }

  // ─── Filter logic ─────────────────────────────────────────────────────────
  function applyFilters() {
    const q = searchQuery.toLowerCase();
    let visible = 0;

    if (currentView === 'cards') {
      postCards.forEach(card => {
        const text  = card.dataset.search || card.textContent.toLowerCase();
        const cat   = card.dataset.category || '';
        const show  = (!q || text.includes(q)) && (!activeFilter || cat === activeFilter);
        card.style.display = show ? '' : 'none';
        if (show) visible++;
      });
    } else {
      eraBlocks.forEach(era => {
        const rows = era.querySelectorAll('.milestone-row');
        let eraVisible = 0;
        rows.forEach(row => {
          const text = row.dataset.search || row.textContent.toLowerCase();
          const cat  = row.dataset.category || '';
          const show = (!q || text.includes(q)) && (!activeFilter || cat === activeFilter);
          row.style.display = show ? '' : 'none';
          if (show) { eraVisible++; visible++; }
        });
        era.style.display = eraVisible > 0 ? '' : 'none';
        // Auto-expand era when a search finds results inside it
        if (eraVisible > 0 && q) {
          const list = era.querySelector('.timeline-list');
          const btn  = era.querySelector('.timeline-era-title');
          if (list.style.display === 'none') {
            list.style.display = '';
            btn.setAttribute('aria-expanded', 'true');
          }
        }
      });
    }

    noRes.style.display = visible === 0 ? '' : 'none';
  }

  // ─── View switching ────────────────────────────────────────────────────────
  function switchView(view) {
    currentView = view;
    viewBtns.forEach(b => b.classList.toggle('active', b.dataset.view === view));
    viewCards.style.display    = view === 'cards'    ? '' : 'none';
    viewTimeline.style.display = view === 'timeline' ? '' : 'none';
    updateURL();
    applyFilters();
  }

  // ─── Initialise from URL params ───────────────────────────────────────────
  searchEl.value = searchQuery;
  pills.forEach(p => p.classList.toggle('active', p.dataset.filter === activeFilter));
  if (!activeFilter) document.querySelector('.filter-pill[data-filter=""]').classList.add('active');
  switchView(currentView); // sets display and runs applyFilters

  // ─── Event listeners ──────────────────────────────────────────────────────
  viewBtns.forEach(btn => {
    btn.addEventListener('click', () => switchView(btn.dataset.view));
  });

  searchEl.addEventListener('input', () => {
    searchQuery = searchEl.value.trim();
    updateURL();
    applyFilters();
  });

  pills.forEach(pill => {
    pill.addEventListener('click', () => {
      pills.forEach(p => p.classList.remove('active'));
      pill.classList.add('active');
      activeFilter = pill.dataset.filter;
      updateURL();
      applyFilters();
    });
  });

  // Keyboard shortcut: / to focus, Escape to clear
  document.addEventListener('keydown', function (e) {
    if (e.key === '/' && document.activeElement !== searchEl) {
      e.preventDefault();
      searchEl.focus();
      searchEl.select();
    }
    if (e.key === 'Escape' && document.activeElement === searchEl) {
      searchEl.blur();
      searchEl.value = '';
      searchQuery = '';
      updateURL();
      applyFilters();
    }
  });

  // ─── Timeline era collapse ────────────────────────────────────────────────
  document.querySelectorAll('.timeline-era-title').forEach(btn => {
    btn.addEventListener('click', () => {
      const list     = btn.nextElementSibling;
      const expanded = btn.getAttribute('aria-expanded') === 'true';
      btn.setAttribute('aria-expanded', String(!expanded));
      if (expanded) {
        list.classList.add('collapsing');
        setTimeout(() => { list.style.display = 'none'; list.classList.remove('collapsing'); }, 200);
      } else {
        list.style.display = '';
        list.classList.add('collapsing');
        requestAnimationFrame(() => list.classList.remove('collapsing'));
      }
    });
  });

  // ─── This Day in AI history ───────────────────────────────────────────────
  const allPosts = [{% for post in site.posts %}{"title":{{ post.title | jsonify }},"url":{{ post.url | relative_url | jsonify }},"date":{{ post.event_date | jsonify }}}{% unless forloop.last %},{% endunless %}{% endfor %}];
  const today = new Date();
  const mm    = String(today.getMonth() + 1).padStart(2, '0');
  const dd    = String(today.getDate()).padStart(2, '0');
  const match = allPosts.find(p => p.date && p.date.slice(5, 10) === mm + '-' + dd);
  if (match) {
    document.getElementById('this-day-link').textContent = match.title;
    document.getElementById('this-day-link').href        = match.url;
    document.getElementById('this-day-year').textContent = match.date.slice(0, 4);
    document.getElementById('this-day').style.display    = '';
  }

  // ─── Read tracking ────────────────────────────────────────────────────────
  const read = JSON.parse(localStorage.getItem('ai-history-read') || '[]');
  if (read.length) {
    postCards.forEach(card => {
      if (read.includes(card.dataset.postId)) card.classList.add('post-card--read');
    });
  }
})();
</script>
