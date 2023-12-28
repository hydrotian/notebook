---
layout: page
title: Posts
excerpt: "An archive of posts sorted by time."
search_omit: true
---
<!-- Pinned Posts Section -->
<section class="pinned-posts">
  <h2>Pinned Posts</h2>
  <ul class="post-list">
    {% for post in site.posts %}
      {% if post.pinned %}
        <li><a href="{{ site.url }}{{ post.url | prepend:site.baseurl }}">{{ post.title }}</a></li>
      {% endif %}
    {% endfor %}
  </ul>
</section>

<!-- Line Break -->
<br/>

<!-- Yearly Archive Section -->
<h2>Yearly Archived Posts</h2>

<ul class="taxonomy-index">
  {% assign postsInYear = site.posts | group_by_exp: 'post', 'post.date | date: "%Y"' %}
  {% for year in postsInYear %}
    <li>
      <a href="#{{ year.name }}">
        <strong>{{ year.name }}</strong> <span class="taxonomy-count">{{ year.items | size }}</span>
      </a>
    </li>
  {% endfor %}
</ul>

{% assign postsByYear = site.posts | group_by_exp: 'post', 'post.date | date: "%Y"' %}
<br>
<br>
{% for year in postsByYear %}
  <section id="{{ year.name }}" class="post-list">
    <h2 class="taxonomy-title">{{ year.name }}</h2>	
    <ul class="post-list">
      {% for entry in year.items %}
        <li><a href="{{ site.url }}{{ entry.url | prepend:site.baseurl }}">{{ entry.title }}</a></li>
      {% endfor %}
    </ul>
    <a href="#page-title" class="back-to-top">{{ site.data.text[site.locale].back_to_top | default: 'Back to Top' }} &uarr;</a>
  </section>
{% endfor %}

<!-- Line Break -->
<br/>

<!-- Developing Posts Section -->
<section class="developing-posts">
  <h2>Posts under Development</h2>
  <ul class="post-list">
    {% for post in site.posts %}
      {% if post.developing %}
        <li><a href="{{ site.url }}{{ post.url | prepend:site.baseurl }}">{{ post.title }}</a></li>
      {% endif %}
    {% endfor %}
  </ul>
</section>

