---
layout: page
title: Research
permalink: /research/
redirect_from: /Oscar-Lange/research/  # old address, forwards to this page
description: "Working papers and ongoing research by Oscar Lange on banking supervision, regulatory penalties, bank disclosure and lobbying."
---

{%- comment -%} The papers themselves are in _data/papers.yml {%- endcomment -%}

{% assign working = site.data.papers | where: "section", "working" %}
{% if working.size > 0 %}
## Working Papers

{% for paper in working %}{% include paper.html paper=paper %}
{% endfor %}
{% endif %}

{% assign wip = site.data.papers | where: "section", "wip" %}
{% if wip.size > 0 %}
## Work in Progress

{% for paper in wip %}{% include paper.html paper=paper %}
{% endfor %}
{% endif %}
