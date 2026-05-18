---
layout: default
---

<div class="hero">
  <h1 class="hero__title">spencer<span>cmd</span></h1>
  <p class="hero__sub">devops &middot; security &middot; AI &mdash; building things, writing about them.</p>
</div>

<p class="section-heading">// featured projects</p>

{% assign featured = site.data.projects | where: "featured", true %}
{% if featured.size > 0 %}
<div class="project-grid">
  {% for project in featured %}
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
{% endif %}

<p style="margin-top:0.75rem; font-size:0.875rem;"><a href="/projects/">View all projects &rarr;</a></p>

---

<p class="section-heading">// recent posts</p>

{% if site.posts.size > 0 %}
<ul class="post-list">
  {% for post in site.posts limit:5 %}
  <li>
    <span class="post-list__date">{{ post.date | date: "%Y-%m-%d" }}</span>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
  </li>
  {% endfor %}
</ul>
{% else %}
No posts yet.
{% endif %}

---

<p class="section-heading">// about</p>

I'm Spencer — musician, developer, dog lover, hobby mycologist. Currently working in devops, security, and AI. Heavily involved in dog rescue and volunteer at my local shelter. Always down to talk dogs.
