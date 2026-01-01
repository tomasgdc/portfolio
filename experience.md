---
layout: default
title: Experience
permalink: /experience
---

## Experience

{% for exp in site.data.experience %}
  <h3>{{ exp.company }} — {{ exp.title }}</h3>
  <p><em>{{ exp.period }} · {{ exp.location }}</em></p>

  <ul>
    {% for bullet in exp.bullets %}
      <li>
        {{ bullet.text }}
        {% if bullet.sub_bullets %}
          <ul>
            {% for sub in bullet.sub_bullets %}
              <li>{{ sub }}</li>
            {% endfor %}
          </ul>
        {% endif %}
      </li>
    {% endfor %}
  </ul>

  <p><strong>Tech:</strong> {{ exp.tech }}</p>
  <hr>
{% endfor %}