---
layout: page
permalink: /research/
---


<h2>Research Interests</h2>
<p>I'm interested in probability theory, statistical mechanics, and related algorithmic questions in theoretical computer science. Some specific topics I have worked on include: quantum spin systems; efficient sampling algorithms; couplings; cluster expansion methods.</p>


<section>
{%- if site.papers.size > 0 -%}
<h2>{{ page.list_title | default: "Publications and Preprints" }}</h2>
<ul>
{%- for paper in site.papers -%}
<li>
<strong>"{{ paper.title }}"</strong>&nbsp;{%- unless paper.DOI -%}—pending publication&nbsp;{%- endunless -%}
{%- if paper.authors -%} with {{ paper.authors }}&nbsp;{%- endif -%}
{%- if paper.DOI -%}(<a href="{{paper.DOI}}">DOI</a>)&nbsp;{%- endif-%}
{%- if paper.arxiv -%}(<a href="{{paper.arxiv}}">arXiv</a>)&nbsp;{%- endif-%}
{%- if paper.video -%}(<a href="{{paper.video}}">video</a>)&nbsp;{%- endif-%}
<ul>
<li>{{ paper.abstract }}</li>
</ul>
<!--<p>Abstract: {{ paper.abstract }}</p>-->
</li>
{%- endfor -%}
</ul>

{%- endif -%}
</section>