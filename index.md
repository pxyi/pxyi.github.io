---
layout: default
---

{% for post in site.posts %}
  <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
  <p class="author">
    <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
  </p>
  <div class="content">
    {{ post.excerpt | strip_html }}
  </div>
{% endfor %}