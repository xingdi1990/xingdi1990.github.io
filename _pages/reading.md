---
layout: single
permalink: /readings/
title: "Readings"
author_profile: true
---

{% include base_path %}

Paper notes and summaries on topics I'm exploring.

{% assign readings = site.posts | where_exp: "post", "post.categories contains 'readings'" %}

{% for post in readings %}
  {% include archive-single.html %}
{% endfor %}
