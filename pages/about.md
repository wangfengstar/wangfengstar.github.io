---
layout: page
title: About
description: 布衣之旅
keywords: 布衣之旅
comments: true
menu: 关于
permalink: /about/
---

一名多年大型软件系统领域穿行的老鸟<br>
坚定认为软件技术可以改变很多领域<br>
坚定的认为软件技术是可以伴随自己一生坚持的兴趣！



## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
