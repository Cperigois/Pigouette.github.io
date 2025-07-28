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

        <div class="project-meta">
          {% assign method_tags = page.method | default: "" | split: "," %}
          {% assign data_tags = page.data | default: "" | split: "," %}
          {% assign skills_tags = page.skills | default: "" | split: "," %}
          {% assign initiative_tags = page.initiative | default: "" | split: "," %}
          
          {% assign all_tags = method_tags | concat: data_tags | concat: skills_tags | concat: initiative_tags %}
          
          <div class="project-tags-inline">
            {% for tag in all_tags %}
              {% assign cleaned_tag = tag | strip | remove: '"' | remove: '[' | remove: ']' %}
              <span class="tag-badge tag-{{ cleaned_tag | slugify }}">{{ cleaned_tag }}</span>
            {% endfor %}
          </div>
        </div>

        {% if page.description %}
          <p>{{ page.description }}</p>
        {% endif %}
      </li>
    {% endif %}
  {% endfor %}
</ul>
