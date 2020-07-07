---
layout: default
---

<div class="article-list">
  <ul>
    {% for post in site.posts %}
      <li>
        <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
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