---
layout: page
title: "Categories"
date: 2014-05-23 -0700
comments: false
sharing: false
---
<ul>
{% for item in site.categories %}
    <li><a href="/blog/categories/{{ item[0] }}/">{{ item[0] | capitalize }}</a> [ {{ item[1].size }} ]</li>
{% endfor %}
</ul>