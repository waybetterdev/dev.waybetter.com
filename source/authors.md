---
layout: default
title: Authors
use:
    - posts

generator: [posts_tag_index, pagination]
pagination:
    provider: page.tag_posts

---

{% for author in post.author %}
  <a href="{{ site.url }}/authors/{{ post.author[loop.index0] }}">{{ post.name[loop.index0] }}</a>
  {% if not loop.last %}, {% endif %}
{% endfor %}

{% for post in page.pagination.items %}
{{ dump(post.author) }}
{% endfor %}



<ul>
  <li>
    <a href="/authors/nikki-stevens">nikki</a>
  </li>
  <li>
    <a href="/authors/mike-klass">mike</a>
  </li>
  <li>
    <a href="/authors/jason-savino">jason</a>
  </li>
</ul>

