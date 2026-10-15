---
layout: page
permalink: /talks/
title: talks
description: talks, tutorials, workshops, posters, panels, podcasts and poetry open mics, newest first.
nav: true
nav_order: 6
# label and colour of the square next to each type; the talks themselves are in _data/talks.yml
talk_types:
  talk: { label: Talk, filter: Talks, color: "#3f51b5" }
  tutorial: { label: Tutorial, filter: Tutorials, color: "#00897b" }
  workshop: { label: Workshop, filter: Workshops, color: "#e0a100" }
  poster: { label: Poster, filter: Posters, color: "#ff66a3" }
  panel: { label: Panel, filter: Panels, color: "#2e7d32" }
  podcast: { label: Podcast, filter: Podcasts, color: "#6d4c41" }
  open-mic: { label: Open Mic, filter: Open Mics, color: "#8e24aa" }
---

<!-- _pages/talks.md
  To add a talk, add an entry to _data/talks.yml (newest at the top); this page sorts by date anyway.
  Entries with the same title are shown as one work with one line per venue, so reuse the exact title.
  Add `month_unknown: true` to an entry to show only its year.
  Leave the year out of `event` (e.g. "ACM SIGKDD", not "ACM SIGKDD 2022"); the date already shows it. -->

<style>
  .talks-intro { margin-bottom: 1rem; }
  .talk-filter { display: flex; flex-wrap: wrap; gap: 0.5rem; margin: 1rem 0 1.5rem; }
  .talk-filter a { border: 1px solid var(--global-divider-color); border-radius: 999px; color: var(--global-text-color); padding: 0.2rem 0.9rem; font-size: 0.85rem; text-decoration: none; }
  .talk-filter a.active { background: var(--global-theme-color); border-color: var(--global-theme-color); color: #fff; }
  .talk-year { font-size: 1.3rem; margin: 1.5rem 0 0.25rem; color: var(--global-text-color-light); }
  .talk-list { list-style: none; padding: 0; margin: 0; }
  .talk-list > li { display: flex; gap: 1.5rem; padding: 0.9rem 0; border-top: 1px solid var(--global-divider-color); }
  .talk-list > li[hidden], .talk-year[hidden], .talk-list[hidden] { display: none; }
  .talk-types { flex: none; width: 6rem; display: flex; flex-direction: column; gap: 0.2rem; }
  .talk-type { flex: none; display: flex; align-items: center; gap: 0.45rem; height: 1.6rem; font-size: 0.8rem; font-weight: 700; color: var(--global-text-color); }
  .talk-type .type-swatch { flex: none; width: 0.8rem; height: 0.8rem; border-radius: 2px; }
  .talk-entry { flex: 1; min-width: 0; }
  .talk-title { font-weight: 600; margin: 0 0 0.2rem; }
  .talk-meta { font-size: 0.9rem; color: var(--global-text-color-light); margin: 0 0 0.3rem; }
  .talk-as { font-weight: 600; color: var(--global-text-color); }
  .talk-links { margin: 0.4rem 0 0; }
  .talk-links a { display: inline-block; margin: 0 0.4rem 0.3rem 0; padding: 0.1rem 0.55rem; border: 1px solid var(--global-text-color); border-radius: 3px; color: var(--global-text-color); font-size: 0.75rem; text-decoration: none; }
  .talk-links a:hover { border-color: var(--global-theme-color); color: var(--global-theme-color); }
  @media (max-width: 576px) {
    .talk-list > li { flex-direction: column; gap: 0.4rem; }
    .talk-types { flex-direction: row; gap: 1rem; width: auto; }
  }
</style>

<p class="talks-intro">
  Slides for most of these are on <a href="https://speakerdeck.com/_themessier" target="_blank" rel="noopener">Speaker Deck</a>;
  code and posters are on <a href="https://github.com/sara-02/pradarshan" target="_blank" rel="noopener">GitHub (pradarshan)</a>.
</p>

{% assign talks = site.data.talks | sort: "date" | reverse %}

<nav class="talk-filter" aria-label="Filter by type">
  <a href="#all" data-filter="all" data-label="All">All</a>
  {% for t in page.talk_types %}
    {% assign n = talks | where: "type", t[0] | size %}
    {% if n > 0 %}<a href="#{{ t[0] }}" data-filter="{{ t[0] }}" data-label="{{ t[1].filter }}">{{ t[1].filter }}</a>{% endif %}
  {% endfor %}
</nav>

{% comment %}
  One list entry per work (same title), shown under the year of its latest presentation.
  Each venue gets its own line; a venue with several presentation types (e.g. talk and poster) shows them as "Talk + Poster".
{% endcomment %}
<div class="talks-all" markdown="0">
{% assign seen = "|" %}
{% assign by_year = talks | group_by_exp: "talk", "talk.date | date: '%Y'" %}
{% for year in by_year %}
  {% capture year_items %}
    {% for talk in year.items %}
      {% assign key = talk.title | prepend: "|" | append: "|" %}
      {% if seen contains key %}{% continue %}{% endif %}
      {% assign seen = seen | append: talk.title | append: "|" %}
      {% assign pres = talks | where: "title", talk.title %}

      {% assign kinds = "" %}
      {% for p in pres %}
        {% assign k = p.type | prepend: " " | append: " " %}
        {% assign padded = kinds | prepend: " " | append: " " %}
        {% unless padded contains k %}{% assign kinds = kinds | append: " " | append: p.type %}{% endunless %}
      {% endfor %}
      {% assign padded = kinds | prepend: " " | append: " " %}
      {% assign kinds = "" %}
      {% for tt in page.talk_types %}
        {% assign k = tt[0] | prepend: " " | append: " " %}
        {% if padded contains k %}{% assign kinds = kinds | append: " " | append: tt[0] %}{% endif %}
      {% endfor %}
      {% assign kinds = kinds | strip %}
      {% assign kind_list = kinds | split: " " %}

      <li data-kind="{{ kinds }}">
        <div class="talk-types">
          {% for k in kind_list %}
            {% assign type = page.talk_types[k] %}
            <div class="talk-type"><span class="type-swatch" style="background-color: {{ type.color }}"></span>{{ type.label }}</div>
          {% endfor %}
        </div>
        <div class="talk-entry">
          <p class="talk-title">{{ talk.title }}</p>
          {% assign done = "|" %}
          {% for p in pres %}
            {% capture ekey %}{{ p.event }}@{{ p.date }}{% endcapture %}
            {% assign ekey_padded = ekey | prepend: "|" | append: "|" %}
            {% if done contains ekey_padded %}{% continue %}{% endif %}
            {% assign done = done | append: ekey | append: "|" %}
            {% assign venue_types = " " %}
            {% for q in pres %}
              {% capture qkey %}{{ q.event }}@{{ q.date }}{% endcapture %}
              {% if qkey == ekey %}{% assign venue_types = venue_types | append: q.type | append: " " %}{% endif %}
            {% endfor %}
            {% assign as = "" %}
            {% for tt in page.talk_types %}
              {% assign k = tt[0] | prepend: " " | append: " " %}
              {% if venue_types contains k %}{% if as != "" %}{% assign as = as | append: " + " %}{% endif %}{% assign as = as | append: tt[1].label %}{% endif %}
            {% endfor %}
            <p class="talk-meta">
              {% if kind_list.size > 1 %}<span class="talk-as">{{ as }}</span> &middot;{% endif %}
              {% if p.event %}
                {% if p.event_url %}<a href="{{ p.event_url }}" target="_blank" rel="noopener">{{ p.event }}</a>{% else %}{{ p.event }}{% endif %}
                &middot;
              {% endif %}
              {% comment %} skip format words that repeat the type or the event name (e.g. "Workshop" for "Pre-ACL Workshop") {% endcomment %}
              {% assign type_label = page.talk_types[p.type].label %}
              {% assign segments = p.format | split: "; " %}
              {% for seg in segments %}
                {% assign seg_down = seg | downcase %}
                {% assign label_down = type_label | downcase %}
                {% if seg_down == label_down %}{% continue %}{% endif %}
                {% if p.event and p.event contains seg %}{% continue %}{% endif %}
                {{ seg }} &middot;
              {% endfor %}
              {% comment %} skip the location when the event name already contains it (e.g. "Ketchup Talk, IIIT Delhi") {% endcomment %}
              {% if p.location %}{% unless p.event and p.event contains p.location %}{{ p.location }} &middot;{% endunless %}{% endif %}
              {% if p.month_unknown %}{{ p.date | date: "%Y" }}{% else %}{{ p.date | date: "%b %Y" }}{% endif %}
            </p>
          {% endfor %}
          {% assign urls = "|" %}
          {% capture link_html %}
            {% for p in pres %}
              {% for l in p.links %}
                {% assign u = l.url | prepend: "|" | append: "|" %}
                {% unless urls contains u %}
                  {% assign urls = urls | append: l.url | append: "|" %}
                  <a href="{{ l.url }}" target="_blank" rel="noopener">{{ l.label }}</a>
                {% endunless %}
              {% endfor %}
            {% endfor %}
          {% endcapture %}
          {% assign link_html = link_html | strip %}
          {% if link_html != "" %}<p class="talk-links">{{ link_html }}</p>{% endif %}
        </div>
      </li>
    {% endfor %}
  {% endcapture %}
  {% assign year_items = year_items | strip %}
  {% if year_items != "" %}
    <h2 class="talk-year">{{ year.name }}</h2>
    <ul class="talk-list">
      {{ year_items }}
    </ul>
  {% endif %}
{% endfor %}
</div>

<script>
  (function () {
    var buttons = document.querySelectorAll(".talk-filter a[data-filter]");
    var items = document.querySelectorAll(".talk-list > li");
    var kinds = Array.prototype.map.call(buttons, function (b) { return b.dataset.filter; });
    var has = function (li, name) { return li.dataset.kind.split(" ").indexOf(name) !== -1; };
    // counts: number of works with each type
    buttons.forEach(function (b) {
      var n = b.dataset.filter === "all" ? items.length : Array.prototype.filter.call(items, function (li) { return has(li, b.dataset.filter); }).length;
      b.textContent = b.dataset.label + " (" + n + ")";
    });
    var show = function (name) {
      if (kinds.indexOf(name) === -1) name = "all";
      items.forEach(function (li) {
        li.hidden = name !== "all" && !has(li, name);
      });
      // hide years with nothing left to show
      document.querySelectorAll(".talk-list").forEach(function (ul) {
        var empty = !ul.querySelector(":scope > li:not([hidden])");
        ul.hidden = empty;
        if (ul.previousElementSibling) ul.previousElementSibling.hidden = empty;
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
