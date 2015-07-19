---
layout: default
title: Collections
---

<div class="message">
  Hello there! The collections page is designed to showcase various collections of blog posts that I have set up for your reading pleasure.
</div>

<div class="posts">
  {% for collection in site.data.collections %}
  <div class="post">
    <h2 class="post-title">
      <a href="/collections/{{ collection.category | slugify }}/">
        {{ collection.category | capitalize }}
      </a>
    </h2>

    <p> {{ collection.desc}} </p>

    <a href="/collections/{{ collection.category | slugify }}/">See Collection</a> ||
    <a href="/collections/{{ collection.category | slugify }}/atom.xml">Subscribe!</a>
  </div>
  {% endfor %}
</div>
