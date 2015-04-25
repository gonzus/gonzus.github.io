---
layout: page
title: Categories
permalink: /categories/
---


{% for category in site.categories %}
  {% assign cat_name = category | first %}
  {% assign cat_label = site.data.categories[cat_name].label %}

  <p>
    <hr/>
    <h3>{{ cat_label }}</h3>
    <ul>
    {% for posts in category %}
      {% for post in posts %}
        {% if post.url %}
          <li><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
            ({{ post.date | date: '%d %B %Y'}})
          </li>
        {% endif %}
      {% endfor %}
    {% endfor %}
    </ul>
  </p>
{% endfor %}
