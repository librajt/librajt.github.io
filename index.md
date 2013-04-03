---
layout: page
title: librajt
tagline: 
---
{% include JB/setup %}

## Posts List

<ul class="posts">
  {% for post in site.posts %}
    <li>
		<h4><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h4>
		<p>{{ post.description }}</p>
    </li>
  {% endfor %}
</ul>

## To-Do

Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

Complete usage and documentation available at: [Jekyll Bootstrap](http://jekyllbootstrap.com)

This theme is still unfinished. If you'd like to be added as a contributor, [please fork](http://github.com/plusjade/jekyll-bootstrap)!
We need to clean up the themes, make theme usage guides with theme-specific markup examples.


