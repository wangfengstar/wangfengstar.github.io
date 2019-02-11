---
layout: page
title: About
description: 布衣之旅
keywords: 布衣之旅
comments: true
menu: 关于
permalink: /about/
---

多年大型软件系统研发、团队管理经历，熟悉运营商计费、CRM等领域模型设计，熟悉云化PaaS平台产品研发以及技术团队的管理。

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
