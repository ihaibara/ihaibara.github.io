---
layout: page
title: Links
description: 没有链接的博客是孤独的
keywords: 友情链接
comments: true
menu: 链接
permalink: /links/
---

> 主要是小伙伴们的博客链接，写的都挺有意思的

{% for link in site.data.links %}
* [{{ link.name }}]({{ link.url }})
{% endfor %}
