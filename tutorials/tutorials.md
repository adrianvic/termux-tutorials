---
permalink: /tutorials/
---

## Listing Tutorials

<ul>
{% for p in site.pages %}
  {% assign path = p.path | default: "" %}
  {% if path != "" and path contains "tutorials/" %}
    {% unless path == "tutorials/index.md" or path == "tutorials/README.md" or path == "tutorials.md" %}
      <li>
        <a href="{{ p.url | relative_url }}">{{ p.title | default: path | replace: "tutorials/", "" }}</a>
        {% if p.date %} — <time datetime="{{ p.date | date_to_xmlschema }}">{{ p.date | date: "%Y-%m-%d" }}</time>{% endif %}
      </li>
    {% endunless %}
  {% endif %}
{% endfor %}
</ul>
