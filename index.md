---
layout: default
---

# spencercmd


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
I'm Spencer.
- Currently working on devops, security, and AI.
- You'll mainly find projects im working on, life updates, opinion pieces here.
- I'm a musician, developer, dog lover, and hobby mycologist. I am heavily involved in dog rescue, volunteer at my local shelter. I am always interested in talking about our canine companions!

---