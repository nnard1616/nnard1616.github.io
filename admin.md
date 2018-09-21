---
layout: page
---

{% highlight bash %}
{% for tag in site.tags %}
echo -e '---\nlayout: tag_index\ntag: {{ tag[0] }} \n---' > '{{ tag[0] }}.md' &{% endfor %}
{% endhighlight %}
