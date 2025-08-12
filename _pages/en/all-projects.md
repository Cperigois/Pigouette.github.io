---
layout: archive
title: "All projects"
permalink: /all-projects/
author_profile: false
classes: wide
---

A list of all projects presented on this website. Projects tagged as "independent" are self-initiated, 
driven by curiosity, and developed as part of my self-learning journey.

<div class="tags-container" aria-label="Compétences et tags">
  <!-- Initiatives / Parcours -->
  <div class="tags-category" role="list" aria-labelledby="cat-initiatives">
    <h3 id="cat-initiatives">Initiatives / Parcours</h3>
    <span class="tag tag-post-doc" role="listitem" title="Post-doctorat">post-doc</span>
    <span class="tag tag-phd" role="listitem" title="Doctorat (PhD)">phd</span>
    <span class="tag tag-independent-study" role="listitem" title="Travail/étude indépendante">independent-study</span>
  </div>

  <!-- Langages & Outils -->
  <div class="tags-category" role="list" aria-labelledby="cat-tools">
    <h3 id="cat-tools">Langages & Outils</h3>
    <span class="tag tag-python" role="listitem" title="Python">python</span>
    <span class="tag tag-pytorch" role="listitem" title="PyTorch">pytorch</span>
    <span class="tag tag-matlab" role="listitem" title="MATLAB">matlab</span>
    <span class="tag tag-simulink" role="listitem" title="Simulink">simulink</span>
  </div>

  <!-- Modélisation, Simulation & IA -->
  <div class="tags-category" role="list" aria-labelledby="cat-modeling">
    <h3 id="cat-modeling">Modélisation, Simulation & IA</h3>
    <span class="tag tag-modeling" role="listitem">modeling</span>
    <span class="tag tag-astrophysical-modeling" role="listitem">astrophysical-modeling</span>
    <span class="tag tag-gravitational-wave-modeling" role="listitem">gravitational-wave-modeling</span>
    <span class="tag tag-detector-simulation" role="listitem">detector-simulation</span>
    <span class="tag tag-population-synthesis" role="listitem">population-synthesis</span>
    <span class="tag tag-binary-population-synthesis" role="listitem">binary-population-synthesis</span>
    <span class="tag tag-semi-analytic-modeling" role="listitem">semi-analytic-modeling</span>
    <span class="tag tag-monte-carlo-simulation" role="listitem">monte-carlo-simulation</span>
    <span class="tag tag-simulation" role="listitem">simulation</span>
    <span class="tag tag-snr-calculation" role="listitem">snr-calculation</span>
    <span class="tag tag-stochastic-background-analysis" role="listitem">stochastic-background-analysis</span>
    <span class="tag tag-autoencoder" role="listitem">autoencoder</span>
    <span class="tag tag-cnn" role="listitem">cnn</span>
    <span class="tag tag-machine-learning" role="listitem">machine-learning</span>
    <span class="tag tag-deep-learning" role="listitem">deep-learning</span>
  </div>

  <!-- Statistiques & Méthodes -->
  <div class="tags-category" role="list" aria-labelledby="cat-stats">
    <h3 id="cat-stats">Statistiques & Méthodes</h3>
    <span class="tag tag-statistics" role="listitem">statistics</span>
    <span class="tag tag-statistical-inference" role="listitem">statistical-inference</span>
    <span class="tag tag-bayesian-analysis" role="listitem">bayesian-analysis</span>
    <span class="tag tag-hierarchical-bayesian-inference" role="listitem">hierarchical-bayesian-inference</span>
    <span class="tag tag-regression" role="listitem">regression</span>
    <span class="tag tag-kernel-density-estimation" role="listitem">kernel-density-estimation</span>
    <span class="tag tag-data-analysis" role="listitem">data-analysis</span>
    <span class="tag tag-signal-processing" role="listitem">signal-processing</span>
  </div>

  <!-- Données & Domaines d'Analyse -->
  <div class="tags-category" role="list" aria-labelledby="cat-data">
    <h3 id="cat-data">Données & Domaines d'Analyse</h3>
    <span class="tag tag-mass-distribution" role="listitem">mass-distribution</span>
    <span class="tag tag-spin-distribution" role="listitem">spin-distribution</span>
    <span class="tag tag-merger-rate" role="listitem">merger-rate</span>
    <span class="tag tag-frequency-spectrum" role="listitem">frequency-spectrum</span>
    <span class="tag tag-time-series" role="listitem">time-series</span>
    <span class="tag tag-redshift-evolution" role="listitem">redshift-evolution</span>
    <span class="tag tag-stochastic-spectrum" role="listitem">stochastic-spectrum</span>
    <span class="tag tag-chirp-mass" role="listitem">chirp-mass</span>
    <span class="tag tag-effective-spin" role="listitem">effective-spin (χ_eff)</span>
    <span class="tag tag-precessing-spin" role="listitem">precessing-spin (χ_p)</span>
  </div>

  <!-- Domaines Scientifiques -->
  <div class="tags-category" role="list" aria-labelledby="cat-domaines">
    <h3 id="cat-domaines">Domaines Scientifiques</h3>
    <span class="tag tag-gravitational-wave-astrophysics" role="listitem">gravitational-wave-astrophysics</span>
    <span class="tag tag-statistical-astrophysics" role="listitem">statistical-astrophysics</span>
  </div>
</div>

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
