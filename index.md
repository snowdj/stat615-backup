---
layout: page
title: Farm Model
tagline: Farm Model for SK and AB
---
{% include JB/setup %}



This website is designed to host project materials for [A farm model](http://snowdj.github.io/stat615) 

Recent posts for material relevant to the project:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

### Project links

- [Data preprocess](html/load.html)
- [data exploratory analysis](html/clean.html)
- [data simulatoin analysis](html/simulation.html)
- [SK simulatoin analysis](html/sksimulation.html)
- [SK slide](slide/index.html)
- [SK model](slide/skmodel.html)



