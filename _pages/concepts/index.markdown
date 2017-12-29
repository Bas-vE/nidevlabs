---
layout: page
title: "Concepts"
category: navigation
---

 <h1 class="page-heading">Pages</h1>
  
  <ul>
    {% for page in site.pages %}
      {% if page.category == "concepts" %}
        <li><a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a></li>
      {% endif %}
    {% endfor %}
  </ul>