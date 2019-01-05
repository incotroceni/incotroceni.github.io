---
layout: page
title: Proiecte Încotroceni
permalink: /proiecte/
---

{% assign items = site.proiecte | sort: 'last_updated' %}
{% for proiect in items %}
  <h3>
    <a href="{{ proiect.url }}">
      {{ proiect.title }}
    </a>
  </h3>
  <p>
    {{ proiect.content | strip_html | truncatewords: 40 }}
    <a href="{{ proiect.url }}">
      Citește mai mult
    </a>
  </p>
{% endfor %}
