---
layout: home
title: "Bio"
tags: [Jekyll, theme, responsive, blog, template]
image:
  feature: typewriter.jpg
---
{% capture about %} {% include about.md %} {% endcapture %}
{{ about | markdownify }}

