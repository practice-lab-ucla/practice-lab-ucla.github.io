---
layout: page
title: People
permalink: /people/
# description: A growing collection of your cool people.
nav: true
nav_order: 3
display_categories: [ , Graduate students, Undergraduate students, Robots, Alumni]
horizontal: false
---

<!-- pages/people.md -->
<div class="projects">  <!-- this need to be projects as it's where the style goes! -->
<style> .post-header { display: none; } </style> <!-- display of the people header is redundant, let's remove it -->
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized people -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category" style="color: var(--global-text-color); border-bottom: none; text-align: left;">{{ category }}</h2>
  </a>
  {% assign categorized_people = site.people | where: "category", category %}
  {% assign sorted_people = categorized_people | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for people in sorted_people %}
      {% include people_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for people in sorted_people %}
      {% include people.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display people without categories -->

{% assign sorted_people = site.people | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for people in sorted_people %}
      {% include people_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for people in sorted_people %}
      {% include people.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>
