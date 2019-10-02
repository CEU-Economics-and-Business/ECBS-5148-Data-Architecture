---
title: Setup
---

## Data

<ol>
{% for data in site.data.dataset %}
  <li>
      {{ data.publisher }}, {{ data.date_published }}. <a href="{{ data.documentation_url }}">"{{ data.name }}"</a>. {{ data.description }}. License: {{ data.license }}.
  </li>
{% endfor %}
</ol>

## Tools and technologies
<ol>
{% for tool in site.data.tool %}
  <li>
      <a href="{{ tool.documentation_url }}">{{ tool.name }}</a>. {{ tool.purpose }}.
  </li>
{% endfor %}
</ol>

{% include links.md %}
