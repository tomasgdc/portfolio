---
layout: default
title: Projects
permalink: /projects/
---

## Projects by Year

{% for project in site.data.projects %}
<div class="project">
  <h3>{{ project.period }}</h3>

  {% assign highlight = false %}
  {% if project.title contains "IDU" or project.title contains "Vue" or project.title contains "National Grid" %}
    {% assign highlight = true %}
  {% endif %}

  <h4>
    {% if highlight and project.link %}
      <a href="{{ project.link }}" target="_blank">{{ project.title }}</a>
    {% else %}
      {{ project.title }}
    {% endif %}
  </h4>

  <p>
    <strong>Company:</strong> 
    {% if project.company contains "Sony" %}
      <a href="https://www.sie.com/" target="_blank">{{ project.company }}</a>
    {% elsif project.company contains "Sapiens" %}
      <a href="https://sapiens.com/" target="_blank">{{ project.company }}</a>
    {% elsif project.company contains "Disguise" %}
      <a href="https://www.disguise.one/" target="_blank">{{ project.company }}</a>
    {% elsif project.company contains "AtkinsRealis" %}
      <a href="https://www.atkinsrealis.com/" target="_blank">{{ project.company }}</a>
    {% else %}
      {{ project.company }}
    {% endif %}

    <br>
    <strong>Tech:</strong> {{ project.tech }}

    <br>
    <strong>Platforms:</strong> {{ project.platforms }}
  </p>

  {% if project.bullets %}
  <ul>
    {% for bullet in project.bullets %}
      <li>{{ bullet }}</li>
    {% endfor %}
  </ul>
  {% endif %}
</div>
{% endfor %}
