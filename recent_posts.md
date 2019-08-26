---
layout: page
title: Recent Posts
permalink: /recentposts/
---
<ul class="post-list">
{% for post in site.posts limit:4 %}
  {% if post.url %}
    <li>
      {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
      <span class="post-meta">{{ post.date | date: date_format }}</span>
      <p>
        <a class="post-content" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
        <br>{{ post.excerpt | remove: '<p>' | remove: '</p>' }}
      </p>
    </li>
  {% endif %}
{% endfor %}
  </ul>
