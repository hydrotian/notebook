---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
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