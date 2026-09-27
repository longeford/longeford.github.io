---
layout: page
title: Blog topics
permalink: /blog/topics/
---

[← All posts]({{ "/blog/" | relative_url }})

{% assign categories = site.categories | sort %}
{% if categories.size > 0 %}
## Categories

{% for category in categories %}
<h3 id="cat-{{ category[0] | slugify }}">{{ category[0] }} <span class="count">({{ category[1].size }})</span></h3>
<ul>
  {% for post in category[1] %}
    <li><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a> <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span></li>
  {% endfor %}
</ul>
{% endfor %}
{% endif %}

{% assign tags = site.tags | sort %}
{% if tags.size > 0 %}
## Tags

{% for tag in tags %}
<h3 id="tag-{{ tag[0] | slugify }}">#{{ tag[0] }} <span class="count">({{ tag[1].size }})</span></h3>
<ul>
  {% for post in tag[1] %}
    <li><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a> <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span></li>
  {% endfor %}
</ul>
{% endfor %}
{% endif %}
