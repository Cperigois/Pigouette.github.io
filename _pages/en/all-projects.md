---
layout: archive
title: "All projects"
permalink: /all-projects/
author_profile: false
---

A list of all projects presented on this website. Projects tagged as "independent" are self-initiated, 
driven by curiosity, and developed as part of my self-learning journey.


## My projects
<ul>
  {% for page in site.pages %}
    {% if page.tags contains "projets" %}
      <li>
        <h3><a href="{{ page.url | relative_url }}">{{ page.title }}</a></h3>

        {% if page.tags contains "projets" %}
  <div class="project-meta">
    {% if page.method or page.data or page.skills %}
  <div class="project-tags-inline">
    {% assign all_tags = page.method | concat: page.data | concat: page.skills %}
    {% for tag in all_tags %}
      <span class="tag-badge tag-{{ tag | slugify: 'default' }}">{{ tag }}</span>
    {% endfor %}
  </div>
{% endif %}
  </div>
{% endif %}
        
        {% if page.description %}
          <p>{{ page.description }}</p>
        {% endif %}

        
      </li>
    {% endif %}
  {% endfor %}
</ul>
