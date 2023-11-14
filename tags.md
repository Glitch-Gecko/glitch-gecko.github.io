---
layout: page
title: Tags
tagline: Tag list
permalink: /tags.html
ref: tags
order: 5
---

## Tags
<div class="tags">
{% assign tags_list = site.tags %}
  {% if tags_list.first[0] == null %}
    {% for tag in tags_list %}
        <a data-scroll href="#{{ tag | slugify }}">{{ tag }}</a>
    {% endfor %}
  {% else %}
    {% for tag in tags_list %}
        - <a data-scroll href="#{{ tag[0] | slugify }}">{{ tag[0] }}</a>
    {% endfor %}
  {% endif %}
{% assign tags_list = nil %}
</div>
<hr>

<div class="tags">
{% for tag in site.tags  %}
    <span class="tag-title" id="{{ tag[0] | slugify }}">{{ tag[0] }}</span>
    <ul class="post-list">
        {% assign pages_list = tag[1] %}
        {% for post in pages_list reversed %}
            {% if post.title != null %}
            {% if group == null or group == post.group %}
            <li>
            <a href="{{ site.url }}{{ post.url }}">{{ post.title }} - <span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}" itemprop="datePublished">{{ post.date | date: "%B %d, %Y" }}</time></span></a></li>
            {% endif %}
            {% endif %}
        {% endfor %}
        {% assign pages_list = nil %}
        {% assign group = nil %}
    </ul>
{% endfor %}
</div>