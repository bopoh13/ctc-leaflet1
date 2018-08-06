---
layout: categories
title: Categories
description: Мои статьи
keywords: классификация
#comments: false
menu: класс; core.autocrlf false
permalink: /categories/
---

<section class="container posts-content">
  {% assign sorted_categories = site.categories | sort %}
  {% for category in sorted_categories %}
                <h3>{{ category | first }}</h3>
                <ol class="posts-list" id="{{ category[0] }}">
    {% for post in category.last %}
                <li class="posts-list-item">
                  <span class="posts-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
                  <a class="posts-list-name" href="{{ post.url | relative_url }}">{{ post.title }}</a>
                </li>
    {% endfor %}
                </ol>
  {% endfor %}
</section>
<!-- /section.content -->
