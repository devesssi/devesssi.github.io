---
layout: single
title: "Notes"
permalink: /notes/
author_profile: true
---

{% for note in site.teaching %}
### [{{ note.title }}]({{ note.url }})

{{ note.excerpt }}

---

{% endfor %}
