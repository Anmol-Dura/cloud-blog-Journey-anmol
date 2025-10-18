---
layout: page
title: Tags
permalink: /tags/
---

<ul>
  {% assign sorted_tags = site.tags | sort %}
  {% for tag in sorted_tags %}
    <li>
      <h3 id="{{ tag[0] | slugify }}">{{ tag[0] }} ({{ tag[1].size }})</h3>
      <ul>
        {% for post in tag[1] %}
          <li><a href="{{ post.url | relative_url }}">{{ post.title }}</a> — <small>{{ post.date | date: "%b %d, %Y" }}</small></li>
        {% endfor %}
      </ul>
    </li>
  {% endfor %}
</ul>