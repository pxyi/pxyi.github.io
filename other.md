---
layout: default
---

{% assign blogs = site.posts | where:"categories","other" %}
{% for post in blogs %}
  <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
  <p class="author">
    <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
  </p>
  <div class="content">
    {{ post.excerpt | strip_html }}
  </div>
{% endfor %}