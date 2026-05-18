---
layout: default
title: Projects
permalink: /projects/
---

<p class="section-heading">// projects</p>

# Things I've built, broken, and shipped.

{% assign all_projects = site.data.projects %}
{% if all_projects.size > 0 %}
<div class="project-grid">
  {% for project in all_projects %}
  <a class="project-card" href="{{ project.url }}" target="_blank" rel="noopener">
    <p class="project-card__name">{{ project.name }}</p>
    <p class="project-card__desc">{{ project.description }}</p>
    <div class="project-card__footer">
      {% if project.tags %}
        {% for tag in project.tags %}
        <span class="tag">{{ tag }}</span>
        {% endfor %}
      {% endif %}
      {% if project.status %}
      <span class="status-badge status-badge--{{ project.status }}">{{ project.status }}</span>
      {% endif %}
    </div>
  </a>
  {% endfor %}
</div>
{% else %}
Nothing here yet — check back soon.
{% endif %}
