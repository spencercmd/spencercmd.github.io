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

I'm {{ site.author.name }}, and this is where I share my thoughts on technology, innovation, and building the future.

---

*"{{ site.description }}"*