---
layout: single
title: "Research"
permalink: /research/
author_profile: true
---

<!-- ## Paper Implementations -->

{% for post in site.publications %}
### [{{ post.title }}]({{ post.url }})

{{ post.content | strip_html | truncate: 220 }}

---

{% endfor %}
