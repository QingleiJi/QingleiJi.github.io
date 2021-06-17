---
layout: page
permalink: /publications/
title: publications
description: Journal/conference paper, patents.
years: [2021, 2020, 2019, 2018]
nav: true
scholar:
  last_name: Qinglei
  first_name: Ji
---

<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
