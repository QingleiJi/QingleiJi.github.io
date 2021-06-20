---
layout: page
permalink: /publications/
title: publications
description: A collection of my journal/conference publications and registered patents.<br/><b>*</b> Equal contribution.
years: [2021, 2020, 2019, 2018, 2017]
nav: true
---

<div class="publications">
{% for y in page.years %}
  <h3 class="year">{{y}}</h3>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}
</div>
