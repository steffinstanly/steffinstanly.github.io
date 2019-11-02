---
layout: single
author_profile: true
title: Posts
header:
  overlay_image: blog-cover.jpg
permalink: /posts.html
---

<ul>
{% for post in site.posts %}
  {% assign currentdate = post.date | date: "%Y" %}
  {% if currentdate != date %}
    <h3>{{ currentdate }}</h3>
    {% assign date = currentdate %} 
  {% endif %}
    <a href="{{ post.url }}">{{ post.title }}</a>
    {{ post.excerpt }}
{% endfor %}
</ul>
