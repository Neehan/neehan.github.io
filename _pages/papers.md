---
layout: archive
title: "Papers"
permalink: /papers/
author_profile: true
---

{% if site.author.googlescholar %}
  <div class="wordwrap">Here is a list of the mediocre research papers I have published so far. You can also find my them on <a href="{{site.author.googlescholar}}">my Google Scholar profile</a>.</div>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
