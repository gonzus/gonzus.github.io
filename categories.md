---
layout: page
title: Categories
permalink: /categories/
---


{% for category in site.categories %}
  {% capture cat_name %}{{ category | first }}{% endcapture %}
  <li>{{ site.data.categories[cat_name].label }}
    <ul>
    {% for posts in category %}
      {% for post in posts %}
        {% if post.url %}
          <li><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a> ({{ post.date | date: '%d %B %Y'}})</li>
        {% endif %}
      {% endfor %}
    {% endfor %}
    </ul>
  </li>
{% endfor %}
