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
  {% assign date_format = site.date_format | default: "%d %B %Y" %}
  <p>
    <time datetime="{{ eveniment.date | date_to_xmlschema }}" class="time">{{ eveniment.date | date: date_format }}</time>&nbsp;&middot;&nbsp;{{ eveniment.content | strip_html | truncatewords: 40 }}
    <a href="{{ eveniment.url }}">
      Citește mai mult
    </a>
  </p>
{% endfor %}
