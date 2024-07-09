---
layout: content
title: Tech
permalink: /tech/
pagination:
    enabled: true
    per_page: 10
    category: 기술
---

{% assign posts = paginator.posts | where:"category", pagination.category %}

<section class="c-archives">
  <link rel="shortcut icon" href="">
  {% for post in posts  %}
  {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
  {% capture this_month %}{{ post.date | date: "%m" }}{% endcapture %}
  {% capture next_year %}{{ post.previous.date | date: "%Y" }}{% endcapture %}
  {% capture next_month %}{{ post.previous.date | date: "%m" }}{% endcapture %}
  {% if forloop.first %}
  <h2 class="c-archives__year" id="{{ this_year }}-{{ this_month }}-ref">{{this_year}}.{{ this_month }}</h2>
  <ul class="c-archives__list">
    {% endif %}
    <li class="c-archives__item">
      <h3>
        <a href="{{ post.url | prepend: site.baseurl }}">{{post.title}}</a>
        <br>
        {% for tagObj in post.tags %}
        {% assign tag = tagObj | split: ":" %}
        <small style="background:{{tag[1]}};color:{{tag[2]}}">{{ tag[0] }}</small>
        {% endfor %}
      </h3>
      <p>{{ post.date | date: "%b %-d, %Y" }}</p>
    </li>
    {% if forloop.last %}
  </ul>
  {% else %}
  {% if this_month != next_month %}
</ul>
<h2 class="c-archives__year" id="{{ next_year }}-{{ next_month }}-ref">{{next_year}}.{{ next_month }}</h2>
<ul class="c-archives__list">
  {% endif %}
  {% endif %}
  {% endfor %}
</section>

<div class="pagination">
  {% if paginator.page_trail %}
  <ul>
  {% for trail in paginator.page_trail %}
    <li {% if page.url == trail.path %}class="selected"{% endif %}>
        <a href="{{ trail.path | prepend: site.baseurl }}" title="{{trail.title}}">{{ trail.num }}</a>
    </li>
  {% endfor %}
  </ul>
  {% endif %}
</div>
