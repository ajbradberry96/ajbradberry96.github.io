---
layout: page
title: Projects
permalink: /projects/
---

Here you can find a collection of some of my projects over the years. I'm actively adding to this list as I write posts about each project and as I add more projects to my portfolio, so expect more things to show up here soon!

<ul class="listing">
{% for post in site.posts %}
  {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
  {% if year != y %}
    {% assign year = y %}
    <li class="listing-seperator">{{ y }}</li>
  {% endif %}
  <li class="listing-item">
  <a href="{{ post.url | prepend: site.baseurl }}">
	<img src="{{ site.baseurl }}/{{ post.thumbnail }}"  	style="width:200px;height:150px;" alt="thumbnail"></a>    <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
    <a href="{{ post.url | prepend: site.baseurl }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}
</ul>
