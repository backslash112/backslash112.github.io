---
layout: side-projects
title:  "All my Side Projects"
date:  2018-07-17 13:37
description: The things that makes me happy and excited 😄.
img: side-project.png
tags: [side-project]
weight: 100
---
{% assign projects = site.projects | sort: 'date' %}
{% for post in projects reversed %}
<article class="post">
  {% if post.img %}
    <a class="post-thumbnail" style="background-image: url({{"/assets/img/" | prepend: site.baseurl | append : post.img}})" href="{{post.url | prepend: site.baseurl}}"></a>
  {% else %}
  {% endif %}
  <div class="post-content">
    <h2 class="post-title"><a href="{{post.url | prepend: site.baseurl}}">{{post.title}}</a></h2>
    {% if post.description %}
    <p>{{ post.description }}</p>
    {% else %}
    <p>{{ post.content | strip_html | truncatewords: 15 }}</p>
    {% endif %}
    <span class="post-date">{{post.date | date: '%b %d, %Y'}}&nbsp;&nbsp;&nbsp;&nbsp;·&nbsp;&nbsp;</span>
    <span class="post-words">{% capture words %}{{ post.content | number_of_words }}{% endcapture %}{% unless words contains "-" %}{{ words | plus: 250 | divided_by: 250 | append: " minute read" }}{% endunless %}</span>
  </div>
</article>
{% endfor %}
