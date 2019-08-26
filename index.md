---
layout: home
---

# Data Science in Healthcare
I am a medical doctor and a freelance software developer with an interest in data science. Recent projects have been my [iOS apps]({% link iOS_apps.md %}), exploring Python and Pandas, and building this website using Jekyll. 
<br><br> 

# Recent posts
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