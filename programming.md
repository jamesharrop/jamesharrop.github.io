---
layout: page
title: Programming
permalink: /programming/
---
## Meet-ups
Attended [NHS HackDay weekend 15th-16th October 2017](http://nhshackday.com) and worked on [What are you waiting for?](https://github.com/what-are-you-waiting-for/what_are_you_waiting_for) using [Opal](http://opal.openhealthcare.org.uk) - a full stack web framework for building healthcare applications.

## Python
  <ul class="post-list">
      {% for post in site.categories.python %}
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


## Swift
  <ul class="post-list">
      {% for post in site.categories.swift %}
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

## Java

  <ul class="post-list">
      {% for post in site.categories.java %}
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

## Open source
[SymPy documentation contribution: Fix for formatting of :noindex: in gotchas.rst and rewriting.rst #12622](https://github.com/sympy/sympy/pull/12622)

## Other

  <ul class="post-list">
      {% for post in site.categories.programming_other %}
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

