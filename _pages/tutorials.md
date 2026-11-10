---
layout: page
permalink: /tutorials/
title: tutorials
description: Expanding our optimal transport survey, one piece at a time.
nav: true
nav_order: 3
toc:
  sidebar: left
---

{% assign series = site.data.tutorials.series %}
{% assign sections = site.data.tutorials.sections | sort: 'order' %}
{% assign all = site.data.tutorials.tutorials %}
{% assign total = all | size %}
{% assign published = all | where: 'status', 'published' | size %}

<p>{{ series.description }}</p>

<p>
  The code for every figure lives in the companion repository,
  <a href="{{ series.repo }}">examples-ot</a>, as runnable notebooks. Each page
  below links to the notebooks it was built from.
</p>

<p class="text-muted">
  <i class="fa-solid fa-book-open fa-sm"></i>
  {{ published }} of {{ total }} tutorials published.
</p>

<div class="tutorials-series">
  {% for s in sections %}
    {% assign items = all | where: 'section', s.id | sort: 'order' %}
    {% assign done = items | where: 'status', 'published' | size %}

    <h2 id="{{ s.id }}" class="tutorial-section-title">
      {{ s.title }}
      <span class="tutorial-section-count">{{ done }}/{{ items | size }}</span>
    </h2>
    <p class="tutorial-section-blurb">
      {{ s.description }}
      <span class="post-meta">Expands <em>{{ s.paper_section }}</em> in the survey.</span>
    </p>

    {% if items.size == 0 %}
      <p class="tutorial-section-empty">Nothing here yet.</p>
    {% endif %}

    <ol class="tutorial-list">
      {% for t in items %}
        <li class="tutorial-item{% if t.status != 'published' %} tutorial-item-planned{% endif %}">
          {% if t.status == 'published' %}
            {% capture url %}/tutorials/{{ t.section }}/{{ t.slug }}/{% endcapture %}
            <a class="tutorial-item-title" href="{{ url | relative_url }}">{{ t.title }}</a>
          {% else %}
            <span class="tutorial-item-title">{{ t.title }}</span>
            <span class="tutorial-badge">planned</span>
          {% endif %}
          <span class="tutorial-item-desc">{{ t.description }}</span>
        </li>
      {% endfor %}
    </ol>
  {% endfor %}
</div>

<h2>The survey</h2>

<p>
  This series expands the following paper. If the tutorials are useful in your work,
  please cite it.
</p>

<div class="publications">
  {% bibliography --query @*[key={{ series.paper_key }}] %}
</div>
