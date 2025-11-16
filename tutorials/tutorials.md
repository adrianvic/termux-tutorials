---
permalink: /tutorials/
---

## Listing Tutorials

<ul>
{% assign tutorials = site.pages | where_exp: "p", "p.path and p.path contains 'tutorials/'" %}
{% for t in tutorials %}
  {% assign p = t.path | default: "" %}
  {% if p != "tutorials/index.md" and p != "tutorials/README.md" and p != "tutorials.md" %}
    <li>
      <a href="{{ t.url | relative_url }}">{{ t.title | default: p | replace: "tutorials/", "" }}</a>
      {% if t.date %} — <time datetime="{{ t.date | date_to_xmlschema }}">{{ t.date | date: "%Y-%m-%d" }}</time>{% endif %}
    </li>
  {% endif %}
{% endfor %}
</ul>
