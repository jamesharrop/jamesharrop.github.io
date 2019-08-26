---
layout: home
---

# iOS Apps

## [NEWS2 app]({% link _iosapps/news2.md %})
<a href="{% link _iosapps/news2.md %}"><img src="{{ "/assets/news2icon.png" }}" width="110px" height="110px" title="Screenshot of NEWS2 App" alt="Screenshot of NEWS2 App"></a>
An App to calculate NEWS2 score for clinicans.

## [Doctor's Bag app]({% link _iosapps/doctorsbag.md %})
<a href="{% link _iosapps/doctorsbag.md %}"><img src="{{ "/assets/bagapp_small.png" }}" width="110px" height="110px" title="Screenshot of Bag App" alt="Screenshot of Bag App"></a>
An App to help doctors keep track of medication expiry dates.

## [Resting Pulse app]({% link _iosapps/pulse.md %})
<a href="{% link _iosapps/pulse.md %}"><img src="{{ "/assets/pulseapp_small.png" }}" width="110px" height="110px" title="Screenshot of Pulse App" alt="Screenshot of Pulse App"></a>
Measure your pulse rate using the iPhone camera.

## [Kitchen Store Cupboard app]({% link _iosapps/kitchen.md %})
<a href="{% link _iosapps/kitchen.md %}"><img src="{{ "/assets/kitchenapp_small.png" }}" width="110px" height="110px" title="Screenshot of Store Cupboard App" alt="Screenshot of Store Cupboard App"></a>
Keep track of your kitchen herbs and spices.

<br><br> 

# Popular posts
  <ul class="post-list">
      {% for post in site.categories.for_feed %}
        {% if post.categories contains "top_post" %}
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
