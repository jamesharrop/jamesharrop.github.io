---
layout: page
title: Blog
permalink: /blog_posts/
---
<!-- Recent posts -->
<!-- <ul class="post-list">
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
  </ul> -->

<!-- All posts organised by year -->
<section class="archive-post-list">
   {% for post in site.posts %}
       {% assign currentDate = post.date | date: "%Y" %}
       {% if currentDate != myDate %}
           {% unless forloop.first %}</ul>{% endunless %}
           <h1>{{ currentDate }}</h1>
           <ul>
           {% assign myDate = currentDate %}
       {% endif %}
       <!-- <li><a href="{{ post.url }}"><span>{{ post.date | date: "%B %-d, %Y" }}</span> - {{ post.title }}</a></li>
    <li><a href="{{ post.url }}">{{ post.title }}</a></li> -->
        <li>
      <!-- {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %} -->
      <!-- <span class="post-meta">{{ post.date | date: date_format }}</span> -->
      <p>
        <a class="post-content" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
        <br>{{ post.excerpt | remove: '<p>' | remove: '</p>' }}
      </p>
    </li>
       {% if forloop.last %}</ul>{% endif %}
   {% endfor %}
</section>