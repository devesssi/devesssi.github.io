---
layout: single
title: "Gallery"
permalink: /gallery/
author_profile: true
---

<div class="gallery-grid">
  {% assign gallery_files = site.static_files | where_exp: "file", "file.path contains '/images/gallery/'" %}

  {% for image in gallery_files %}
    <div class="gallery-item" onclick="openModal('{{ image.path }}')">
      <img src="{{ image.path }}" alt="{{ image.name }}">
      <div class="overlay">
        <span>
          {{ image.name | remove: ".jpeg" | remove: ".jpg" | remove: ".png" | replace: "-", " " }}
        </span>
      </div>
    </div>
  {% endfor %}
</div>
