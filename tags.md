---
layout: page
title: Tags
permalink: /tags/
---

<div class="tags"> 
{% for tagitem in site.tags  %} 
  <a href="#{{  tagitem[0] }}">{{  tagitem[0] }}</a>
{% endfor %}
</div>

<!-- iterate through all tags on the site --> 
{% for tagitem in site.tags %} 
  <!-- for each tag, create an anchor by using the tag name as an id --> 
  <div id="{{ tagitem[0] }}">  
    <h3> {{ tagitem[0] }} </h3>  <!-- for create a heading --> 

    <ul> <!-- create the list of posts -->
      <!-- iterate through all the posts on the site --> 
      {% for post in site.posts %} 
        <!-- list only those which contain the current tag --> 
        {% if post.tags contains tagitem[0] %} 
            <li>
              <a href="{{ post.url }}">{{ post.title }}</a>
            </li>
        {% endif %}
      {% endfor %}
    </ul>
  </div>
{% endfor %}

<p style="font-size: 14px; margin-top: 20px;"><a href="{{ site.url }}/posts">View full archive</a></p>
