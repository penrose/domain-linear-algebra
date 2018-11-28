---
title: Welcome
permalink: /
---

Welcome! This is a domain page for the software 
<a href="https://penrose.github.io" target="_blank">Penrose</a>. Penrose enables 
people to <b>create beautiful diagrams  just by typing mathematical notation in 
plain text.</b> It will make it easy for you to create and explore 
high-quality diagrams without needing to learn challenging graphics programs.

> Where do I go from here?

This is the Penrose {{ site.description }} base. We have several examples to help
get you started:

<ul>{% assign grouped = site.domain | group_by: 'category' %}{% for group in grouped %}<li class="nav-item top-level {% if group.name == page.category %}current{% endif %}">{% assign items = group.items | sort: 'order' %}<a href="{{ site.baseurl }}{{ items.first.url }}">{{ group.name }}</a>
							<ul>{% for item in items %}
							    <li class="nav-item {% if item.url == page.url %}current{% endif %}"><a href="{{ site.baseurl }}{{ item.url }}">{{ item.title }}</a></li>{% endfor %}
</ul>
</li>{% endfor %}
</ul>

> Where can I find an API for this data?

This repository serves a static API (application programming interface) for 
you to find the trios (style, substance, and domain specific language files)
for the {{ site.description }} Domain. You can find it here:

<a target="_blank" href="{{ site.baseurl }}/library.json"><button class="btn btn-primary">JSON</button></a>

<hr>

#### Support

We are here to help! If you have a question, or want to make a contribution, 
please open <a href="{{ site.github.repository_url }}/issues" target="_blank">an issue</a>.
