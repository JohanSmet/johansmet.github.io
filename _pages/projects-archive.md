---
title: "Portfolio of publicly available projects."
layout: archive
classes: wide
permalink: /projects/
author_profile: true
---

{% assign ordered = site.projects | sort: "order" | reverse %}
{% assign type = "left" %}

{% for project in ordered %}
<div class="feature__wrapper">
<div class="feature__item--{{ type }}">

{% if project.header.teaser %}
	<img src="{{ project.header.teaser | relative_url }}" class="align-{{ type }}" />
{% endif %}

{% if project.title %}
	<h2 class="archive__item-title">{{ project.title }}</h2>
{% endif %}

{% if project.excerpt %}
	<div class="archive__item-excerpt">
	  {{ project.excerpt | markdownify }}
	</div>
{% endif %}

{% if project.url %}
	<p><a href="{{ project.url | relative_url }}" class="btn btn--primary">Read More</a></p>
{% endif %}

 </div>
 </div>

{% if type == "left" %}
	{% assign type = "right" %}
{% else %}
	{% assign type = "left" %}
{% endif %}

{% endfor %}
