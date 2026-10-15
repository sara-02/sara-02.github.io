---
layout: default
permalink: /writings/
title: writings
nav: true
nav_order: 7
# colour of the square next to each post's label
post_types:
  essays: { label: Essay, filter: Essays, color: "#3f51b5" }
  notes: { label: Note, filter: Notes, color: "#e0a100" }
  invited: { label: Invited, filter: Invited, color: "#2e7d32" }
  poems: { label: Poem, filter: Poems, color: "#8e24aa" }
---

<!-- _pages/writings.md
  Two kinds of posts, picked by the `categories` field in a post's front matter:
    categories: essays  -> long-form
    categories: notes   -> short
    categories: invited -> article published elsewhere; set `publication:` and `redirect:` (the article's URL)
    categories: poems   -> poem; set `publication:` and `redirect:` (the poem's URL) for poems hosted elsewhere
  Posts without one of these categories are listed as essays.
  This page is only a list (newest first); each post opens on its own page.
  The buttons filter by type (#essays / #notes in the URL). -->

<style>
  .blog-filter { display: flex; flex-wrap: wrap; gap: 0.5rem; margin: 1rem 0 1.5rem; }
  .blog-filter a { border: 1px solid var(--global-divider-color); border-radius: 999px; color: var(--global-text-color); padding: 0.2rem 0.9rem; font-size: 0.85rem; text-decoration: none; }
  .blog-filter a.active { background: var(--global-theme-color); border-color: var(--global-theme-color); color: #fff; }
  .blog-list { list-style: none; padding: 0; margin: 0; }
  .blog-list > li { display: flex; gap: 1.5rem; padding: 0.9rem 0; border-top: 1px solid var(--global-divider-color); }
  .blog-list > li[hidden] { display: none; }
  .post-type { flex: none; width: 5.5rem; display: flex; align-items: center; gap: 0.45rem; height: 1.6rem; font-size: 0.8rem; font-weight: 700; color: var(--global-text-color); }
  .post-type .type-swatch { flex: none; width: 0.8rem; height: 0.8rem; border-radius: 2px; }
  .post-entry { flex: 1; min-width: 0; }
  .entry-title { font-size: 1.15rem; font-weight: 600; margin: 0 0 0.2rem; }
  .entry-subtitle { color: var(--global-text-color-light); margin: 0 0 0.2rem; }
  .blog-meta { font-size: 0.85rem; color: var(--global-text-color-light); margin: 0; }
  .writings-note { color: var(--global-text-color-light); font-size: 0.95rem; margin: 0.5rem 0 0; }
  .blog-empty { color: var(--global-text-color-light); font-style: italic; }
  @media (max-width: 576px) {
    .blog-list > li { flex-direction: column; gap: 0.4rem; }
  }
</style>

{% assign essay_count = 0 %}
{% assign note_count = 0 %}
{% assign invited_count = 0 %}
{% assign poem_count = 0 %}
{% for post in site.posts %}
  {% if post.categories contains "notes" %}{% assign note_count = note_count | plus: 1 %}
  {% elsif post.categories contains "invited" %}{% assign invited_count = invited_count | plus: 1 %}
  {% elsif post.categories contains "poems" %}{% assign poem_count = poem_count | plus: 1 %}
  {% else %}{% assign essay_count = essay_count | plus: 1 %}{% endif %}
{% endfor %}

<div class="post">
  <header class="post-header">
    <h1 class="post-title">writings</h1>
    <p class="post-description">essays, notes, invited articles and poems, newest first.</p>
    <p class="writings-note">
      Essays are long reads and notes are short commentary and lists; both are hosted here. Poems stay on my old blog,
      <a href="https://themessier.wordpress.com/" target="_blank" rel="noopener">The Messier</a>, for legacy and nostalgia.
      Invited articles link to the websites that published them.
    </p>
  </header>

  <nav class="blog-filter" aria-label="Filter posts by type">
    <a href="#all" data-filter="all">All ({{ site.posts.size }})</a>
    <a href="#essays" data-filter="essays">{{ page.post_types.essays.filter }} ({{ essay_count }})</a>
    <a href="#notes" data-filter="notes">{{ page.post_types.notes.filter }} ({{ note_count }})</a>
    <a href="#invited" data-filter="invited">{{ page.post_types.invited.filter }} ({{ invited_count }})</a>
    <a href="#poems" data-filter="poems">{{ page.post_types.poems.filter }} ({{ poem_count }})</a>
  </nav>

  {% if site.posts.size == 0 %}
    <p class="blog-empty">No posts yet.</p>
  {% endif %}

  <ul class="blog-list">
    {% for post in site.posts %}
      {% if post.categories contains "notes" %}{% assign kind = "notes" %}{% elsif post.categories contains "invited" %}{% assign kind = "invited" %}{% elsif post.categories contains "poems" %}{% assign kind = "poems" %}{% else %}{% assign kind = "essays" %}{% endif %}
      {% assign type = page.post_types[kind] %}
      <li data-kind="{{ kind }}">
        <div class="post-type"><span class="type-swatch" style="background-color: {{ type.color }}"></span>{{ type.label }}</div>
        <div class="post-entry">
          {% if post.redirect contains "://" %}
            <h3 class="entry-title"><a href="{{ post.redirect }}" target="_blank" rel="noopener">{{ post.title }} &#8599;</a></h3>
            {% if post.description %}<p class="entry-subtitle">{{ post.description }}</p>{% endif %}
            <p class="blog-meta">{{ post.publication }} &middot; {{ post.date | date: "%Y" }}</p>
          {% else %}
            <h3 class="entry-title"><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
            {% if post.description %}<p class="entry-subtitle">{{ post.description }}</p>{% endif %}
            <p class="blog-meta">{{ post.date | date: "%B %d, %Y" }}</p>
          {% endif %}
        </div>
      </li>
    {% endfor %}
  </ul>
</div>

<script>
  (function () {
    var buttons = document.querySelectorAll(".blog-filter a[data-filter]");
    var items = document.querySelectorAll(".blog-list > li");
    var show = function (name) {
      if (["essays", "notes", "invited", "poems"].indexOf(name) === -1) name = "all";
      items.forEach(function (li) {
        li.hidden = name !== "all" && li.dataset.kind !== name;
      });
      buttons.forEach(function (b) {
        var on = b.dataset.filter === name;
        b.classList.toggle("active", on);
        if (on) b.setAttribute("aria-current", "true");
        else b.removeAttribute("aria-current");
      });
    };
    buttons.forEach(function (b) {
      b.addEventListener("click", function (e) {
        e.preventDefault();
        history.replaceState(null, "", b.dataset.filter === "all" ? location.pathname : "#" + b.dataset.filter);
        show(b.dataset.filter);
      });
    });
    show(location.hash.slice(1));
  })();
</script>
