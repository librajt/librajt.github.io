---
layout: default
title: Lax
---
<!-- Pagination links -->
<div class="pagination">
  {% if paginator.previous_page %}
    {% if paginator.previous_page == 1 %}
      <a href="/" class="previous">&laquo; Previous </a>
    {% else %}
      <a href="/page{{paginator.previous_page}}" class="previous">&laquo; Previous </a>
    {% endif %}
  {% else %}
    <a href="#" class="previous">&laquo; Previous </a>
  {% endif %}
  <span class="page_number ">Page: {{paginator.page}} of {{paginator.total_pages}}</span>
  {% if paginator.next_page %}
    <a href="/page{{paginator.next_page}}" class="next "> Next &raquo; </a>
  {% else %}
    <a href="#" class="next "> Next &raquo; </a>
  {% endif %}
</div>

<div id="post">
<ul>
{% for post in paginator.posts %}
  <li>
  {% for category in post.categories %}
    [<a href="/categories/{{ category }}" title="{{ category }}" rel="category tag">{{ category }}</a>]
  {% endfor %}
    <a href="{{ post.url }}" title="{{ post.title }}" rel="bookmark">{{ post.title }}</a>
    <abbr class="published" title="{{ post.date | date_to_string }}">{{ post.date | date_to_string }}</abbr>
  </li>
{% endfor %}
</ul>
</div>

{% if paginator.total_pages > 1 %}
<div id="pagination">
  {% for page in (1..paginator.total_pages) %}
      <span>
        {% if page > 1 %}
          {% capture page_url %}page{{ page }}{% endcapture%}
        {% endif %}
        {% if page == paginator.page %}
          {{ page }}
        {% else %}
          <a href="/{{ page_url }}">{{ page }}</a>
        {% endif %}
      </span>
  {% endfor %}
</div>
{% endif %}
