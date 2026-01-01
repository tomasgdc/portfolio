---
layout: default
title: Blog
permalink: /blog
---

# Blog Posts

{% for post in site.posts %}
## [{{ post.title }}]({{ post.url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.excerpt | strip_html | truncate: 200 }}

---
{% endfor %}