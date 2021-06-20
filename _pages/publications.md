---
layout: page
permalink: /publications/
title: publications
description: A collection of my journal publications, conference publications and registered patents.<br/><b>*</b> equal contribution.
years: [2021, 2020, 2019, 2018, 2017]
nav: true
---

<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
