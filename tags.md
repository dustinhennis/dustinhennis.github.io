---
layout: page
title: "Tags"
permalink: /tags
---

{% for tag in site.tags %}

{% assign tag_name = tag | first %}
{% assign tag_name_pretty = tag_name | replace: "_", " " | capitalize %}

### #{{ tag_name }}:

<a name="{{ tag_name | slugize }}"></a>

{% for post in site.tags[tag_name] %}
  {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
**[{{ post.title | escape }}]({{ post.url | relative_url }})**
*{{ post.date | date: date_format }}*
{% endfor %}


{% endfor %}
