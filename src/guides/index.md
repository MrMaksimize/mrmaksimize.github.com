---
layout: page
title: Guides
---

<div class="message">
  Hello there! The Guides page contains a bunch of updated guides to help you learn stuff!
</div>

<div class="posts">
  {% for guide in site.data.guides %}
  <div class="post">
    <h2 class="post-title">
      <a href="/guides/{{ guide.name | slugify }}/">
        {{ guide.name | capitalize }}
      </a>
    </h2>

    <p> {{ guide.desc}} </p>
  </div>
  {% endfor %}
</div>
