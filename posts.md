---
layout: page
title: Posts
tagline: All posts
permalink: /posts.html
ref: posts
order: 4
---

## Latest Posts
<ul class="post-list">
      {% for post in site.posts %}
        <li>
          {% assign date_format = site.cayman-blog.date_format | default: "%b %-d, %Y" %}
          <h2>
            <a class="post-link" href="{{ post.url | absolute_url }}" title="{{ post.title }}">{{ post.title | escape }}</a>
          </h2>
          <span class="post-meta">{{ post.date | date: date_format }}</span>
        </li>
      {% endfor %}
</ul>