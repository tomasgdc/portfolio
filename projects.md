---
layout: default
title: Projects
permalink: /projects
---

## Projects by Year

{% assign sorted_projects = site.data.projects | sort: 'period' | reverse %}

{% for project in sorted_projects %}
### {{ project.period }}
#### {{ project.title }} ({{ project.company }})
**Tech:** {{ project.tech }}
<ul>
  {% for bullet in project.bullets %}
    <li>{{ bullet }}</li>
  {% endfor %}
</ul>
{% endfor %}
