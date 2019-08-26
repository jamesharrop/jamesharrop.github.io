---
layout: page
title: Machine Learning
permalink: /machinelearning/
---
## R
  <ul class="post-list">
      {% for post in site.categories.r %}
        {% if post.categories contains "machine_learning" %}
            {% if post.url %}
            <li>
            {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
            <span class="post-meta">{{ post.date | date: date_format }}</span>
          <p>
            <a class="post-content" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
          {% if post.subtitle %}
              <br>
              {{ post.subtitle }}
          {% endif %}
          </p>
            </li>
        {% endif %}
      {% endif %}
    {% endfor %}
  </ul>


## Python
  <ul class="post-list">
      {% for post in site.categories.python %}
        {% if post.categories contains "machine_learning" %}
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
      {% endif %}
    {% endfor %}
  </ul>

## Swift
  <ul class="post-list">
      {% for post in site.categories.swift %}
        {% if post.categories contains "machine_learning" %}
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
      {% endif %}
    {% endfor %}
  </ul>


