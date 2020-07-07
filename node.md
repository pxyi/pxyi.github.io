---
layout: default
---

<div class="article-list">
  <ul>
    {% assign blogs = site.posts | where:"categories","node" %}
    {% for post in blogs %}
      <li>
        <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
        <p class="author">
          <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
        </p>
        <div class="excerpt">
          {{ post.excerpt | strip_html }}
        </div>
      </li>
    {% endfor %}
  </ul>
</div>