---
layout: default
---

# Welcome to {{ site.title }}

{{ site.description }}

## Recent Posts

{% if site.posts.size > 0 %}
  {% for post in site.posts limit:5 %}
  - [{{ post.title }}]({{ post.url | relative_url }}) - {{ post.date | date: "%B %d, %Y" }}
  {% endfor %}
{% else %}
  No posts yet! Create your first post in the `_posts` folder.
{% endif %}

## About

I'm {{ site.author.name }}, An uber employable catalyst of accelerated technological advancement.
- Currently working on devops, security, and AI at a small data analytics organization.
- You'll mainly find projects im working on, life updates, opinion pieces here.
- I'm a musician, developer, dog lover, and hobby mycologist. I volunteer at my local animal shelter, I've fostered and rescued, and hope to continue that work.

---

*"{{ site.description }}"*