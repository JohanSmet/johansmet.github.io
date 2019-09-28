---
layout: collection
title: "Projects"
permalink: /projects/
author_profile: false
---

An overview of some personal projects I've worked on.

{% assign ordered = site.projects | sort: "order" %}

{% for project in ordered %}
## [{{ project.title }}]({{ project.url }})
<p class='archive__item-excerpt'>{{ project.excerpt }}</p>
{% endfor %}
