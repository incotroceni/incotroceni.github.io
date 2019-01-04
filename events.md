---
layout: page
title: Evenimente Încotroceni
permalink: /evenimente/
---

{% assign items = site.evenimente | sort: 'date' %}
{% for eveniment in items reversed %}
  <h3>
    <a href="{{ eveniment.url }}">
      {{ eveniment.title }}
    </a>
  </h3>
  <p>
    {{ eveniment.content | strip_html | truncatewords: 50 }}
    <a href="{{ eveniment.url }}">
      Citește mai mult
    </a>
  </p>
{% endfor %}
