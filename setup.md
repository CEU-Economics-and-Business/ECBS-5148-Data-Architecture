---
title: Setup
---

## Data

<ul>
{% for data in site.data.dataset %}
  <li>
      {{ data.publisher }}, {{ data.date_published }}. <a href="{{ data.documentation_url }}">"{{ data.name }}"</a>. {{ data.description }}. License: {{ data.license }}.
  </li>
{% endfor %}
</ul>

{% include links.md %}
