---
layout: page
---

# Initial commit

{% for post in site.posts %}
 {% unless post.categories contains 'obsolete' %}* [{{ post.title }}]({{ post.url }}){% endunless %}
{% endfor %}
