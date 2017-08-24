---
layout: page
title: Talks
permalink: /talks/
---

<div class="main-post-list">

  <ol class="post-list">
    {% for talk in site.categories.talks %}
      <li>
        {% if talk.link %}
        <h1 class="post-list__post-title post-title"><a href="{{ site.url }}{{ talk.url }}" title="{{ talk.title }}">{{ talk.title }}</a> <a href="{{ talk.link }}" target="_blank" title="{{ talk.title }}"><i class="icon icon-link"></i></a></h1>
        {% else %}
        <h1 class="post-list__post-title post-title"><a href="{{ site.url }}{{ talk.url }}" title="{{ talk.title }}">{{ talk.title }}</a></h1>
        {% endif %}
        <p>{% if talk.description %}{{ talk.description }}{% else %}{{ talk.content }}{% endif %} <a style="font-size:12px;" href="{{ site.url }}{{ talk.url }}" title="{{ talk.title }}"></a></p>
        <div class="post-list__meta"><time datetime="{{ talk.date | date: "%-d %b %Y" }}" class="post-list__meta--date date">{{ talk.date | date: "%-d %b %Y" }}</time> &#8226; <span class="post-meta__tags">Tags: {% for tag in talk.tags %}<a href="{{ site.url }}/tags/#{{ tag }}">{{ tag }}</a> {% endfor %}</span></div>
        <hr class="post-list__divider">
      </li>
    {% endfor %}
  </ol>

  <hr class="post-list__divider ">
</div>
