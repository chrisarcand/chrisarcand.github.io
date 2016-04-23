---
layout: page
title: Archive
permalink: /posts/
---

<div class="main-post-list">
  {% for post in site.posts %}  
    {% unless post.next %}
      <h3 class="year-header">{{ post.date | date: '%Y' }}</h3>
    {% else %}
      {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
      {% if year != nyear %}
        <h3 class="year-header">{{ post.date | date: '%Y' }}</h3>
      {% endif %}
    {% endunless %}
        {% if post.link %}
        <h2 class="post-list__post-title post-title" style="margin-top: 20px;"><a href="{{ site.baseurl }}{{ post.url | remove_first: '/' }}" title="{{ post.title }}">{{ post.title }}</a> <a href="{{ post.link }}" target="_blank" title="{{ post.title }}"><i class="icon icon-link"></i></a></h2>
        {% else %}
        <h2 class="post-list__post-title post-title" style="margin-top: 20px;"><a href="{{ site.baseurl }}{{ post.url | remove_first: '/' }}" title="{{ post.title }}">{{ post.title }}</a></h2>
        {% endif %}
  {% endfor %}
</div>
