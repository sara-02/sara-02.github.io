---
layout: page
permalink: /publications/
title: papers
description: publications by color coded venue/type category in chronological order.
nav: true
nav_order: 1
---

<!-- _pages/publications.md -->

<!-- Filter by article type (the abbr field in papers.bib); buttons are built from the badges on the page -->
<style>
  .scholar-link { font-size: 1rem; margin-bottom: 0.75rem; }
  .scholar-link .ai { font-size: 1.3rem; vertical-align: -0.15rem; margin-right: 0.3rem; }
  .type-filter { display: flex; flex-wrap: wrap; gap: 0.5rem; margin-bottom: 1.5rem; }
  .type-filter button { border: 1px solid var(--global-divider-color); border-radius: 999px; background: none; color: var(--global-text-color); padding: 0.2rem 0.9rem; font-size: 0.85rem; cursor: pointer; }
  .type-filter button.active { background: var(--global-theme-color); border-color: var(--global-theme-color); color: #fff; }
  /* type label: small colour square + bold text, instead of text on a coloured badge */
  .publications .abbr abbr.badge.type-label { display: flex; align-items: center; gap: 0.45rem; background: none !important; color: var(--global-text-color) !important; font-weight: 700; font-size: 0.8rem; text-align: left; padding: 0.15rem 0; }
  .publications .abbr .type-swatch { flex: none; width: 0.8rem; height: 0.8rem; border-radius: 2px; }
</style>

<p class="scholar-link"><i class="ai ai-google-scholar"></i> Full list with citations on <a href="https://scholar.google.com/citations?hl=en&amp;user=jxe2vbQAAAAJ&amp;view_op=list_works&amp;sortby=pubdate" target="_blank" rel="noopener">Google Scholar</a></p>

<div class="type-filter" role="group" aria-label="Filter papers by type"></div>

<div class="publications">

{% bibliography %}

</div>

<script>
  (function () {
    // move each badge's colour (from _data/venues.yml) into a small square next to the type name
    document.querySelectorAll(".publications .abbr abbr.badge").forEach(function (badge) {
      var swatch = document.createElement("span");
      swatch.className = "type-swatch";
      swatch.style.backgroundColor = getComputedStyle(badge).backgroundColor;
      badge.insertBefore(swatch, badge.firstChild);
      badge.classList.add("type-label");
    });

    var bar = document.querySelector(".type-filter");
    var entries = Array.prototype.slice.call(document.querySelectorAll(".publications ol.bibliography > li"));
    var typeOf = function (li) {
      var badge = li.querySelector(".abbr abbr");
      return badge ? badge.textContent.trim() : "";
    };
    var types = [];
    entries.forEach(function (li) {
      var t = typeOf(li);
      if (t && types.indexOf(t) === -1) types.push(t);
    });
    types.sort();

    var apply = function (selected) {
      entries.forEach(function (li) {
        li.hidden = selected !== "All" && typeOf(li) !== selected;
      });
      // hide year headings that have no visible papers left
      document.querySelectorAll(".publications ol.bibliography").forEach(function (ol) {
        var empty = !ol.querySelector(":scope > li:not([hidden])");
        ol.hidden = empty;
        var heading = ol.previousElementSibling;
        if (heading && heading.tagName === "H2") heading.hidden = empty;
      });
      bar.querySelectorAll("button").forEach(function (b) {
        var on = b.dataset.type === selected;
        b.classList.toggle("active", on);
        b.setAttribute("aria-pressed", on);
      });
    };

    ["All"].concat(types).forEach(function (t) {
      var b = document.createElement("button");
      b.type = "button";
      b.dataset.type = t;
      b.textContent = t;
      b.addEventListener("click", function () {
        apply(t);
      });
      bar.appendChild(b);
    });
    apply("All");
  })();
</script>
