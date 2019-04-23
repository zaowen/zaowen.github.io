---
layout: default
title: Portfolio
permalink: /portfolio/
---
{% for project in site.projects %}
   <h3><div class="open">{{project.tag}}</div> {{project.name}}
   (
   {{project.languages[0]}}
   {% for lang in project.languages offset:1 %}
   / {{lang}}
   {% endfor %}
   )
   </h3>
   {{project.content | markdownify }}
   <hr>
{% endfor %}