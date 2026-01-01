---
layout: default
title: Projects
permalink: /projects/
---

## Projects by Year

{% for project in site.data.projects %}
### {{ project.period }}
#### {{ project.title }} ({{ project.company }})
**Tech:** {{ project.tech }}
<ul>
  {% for bullet in project.bullets %}
    <li>{{ bullet }}</li>
  {% endfor %}
</ul>
{% endfor %}
