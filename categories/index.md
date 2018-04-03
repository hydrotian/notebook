---
layout: page
title: Category Index
excerpt: "An archive of posts sorted by category."
search_omit: true
---

{% capture site_categories %}{% for categories in site.categories %}{{ categories | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
{% assign categories_list = site_categories | split:',' | sort %}

<ul class="taxonomy-index">
    {% for item in (0..site.categories.size) %}{% unless forloop.last %}
    {% capture this_word %}{{ categories_list[item] | strip_newlines }}{% endcapture %}
    <li><a href="#{{ this_word }}"><strong>{{ this_word }}</strong> <span>{{ site.categories[this_word].size }}</span></a></li>
  {% endunless %}{% endfor %}
</ul>
<br>
<br>
{% for item in (0..site.categories.size) %}{% unless forloop.last %}
  {% capture this_word %}{{ categories_list[item] | strip_newlines }}{% endcapture %}
  <h2 id="{{ this_word }}">{{ this_word }}</h2>
  <ul class="post-list">
  {% for post in site.categories[this_word] %}{% if post.title != null %}
    <li><a href="{{ site.url }}{{ post.url | prepend:site.baseurl }}">{{ post.title }}</a></li>
  {% endif %}{% endfor %}
   <a href="#page-title" class="back-to-top">{{ site.data.text[site.locale].back_to_top | default: 'Back to Top' }} &uarr;</a>
  </ul>
{% endunless %}{% endfor %}

