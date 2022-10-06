---
title: "git"
permalink: /git/
nav: "main"
---


{% assign posts = site.categories.python %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
