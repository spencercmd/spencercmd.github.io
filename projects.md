---
layout: default
title: Projects
permalink: /projects/
---

# Projects

Things I've built, broken, and shipped.

---

{% assign all_projects = site.data.projects %}
{% if all_projects.size > 0 %}
  {% for project in all_projects %}
**[{{ project.name }}]({{ project.url }})**
{% if project.status %}<span style="font-size:0.8em; opacity:0.7;">[{{ project.status }}]</span>{% endif %}

{{ project.description }}

{% if project.tags %}
`{{ project.tags | join: "` `" }}`
{% endif %}

---
  {% endfor %}
{% else %}
Nothing here yet — check back soon.
{% endif %}
