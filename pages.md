---
layout: page
title: Pages 
header: Pages
group: misc
---

## All Pages
{% assign pages_list = site.pages %}
{% include helpers/pages_list.html %}


## Pages in group: News
{% assign pages_list = site.pages %}
{% assign group = 'news' %}
{% include helpers/pages_list.html %}

