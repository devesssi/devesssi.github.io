---
layout: single
title: "Blog"
permalink: /blog/
author_profile: true
---

{% for post in site.posts %}
### [{{ post.title }}]({{ post.url }})

{{ post.excerpt }}

---

{% endfor %}
