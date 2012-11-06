---
layout: post
category : broken
header: Turning Jekyll into Mr. Hyde
author: fernferret
---

This website you're viewing is generated with [Jekyll](https://github.com/mojombo/jekyll), which, as I've [mentioned before](/news/multiverse-blog.html), is awesome. Jekyll takes o

The other tool that can get a Jekyll site setup very quickly (for those of us who are not creative at all) is [Twitter Bootstrap](http://twitter.github.com/bootstrap/). The base cs

Performing basic tasks is really easy, such as getting the base site architecture setup. The initial layout is [already done for you](https://github.com/mojombo/jekyll/wiki/Usage).

### Getting started
I definetly can't take credit for all of the tips and tricks used here. If you're looking for a good place to start, go to [Jekyll Bootstrap](http://jekyllbootstrap.com/). Go there

### Navigation Rev. A
One of the most important aspects of building this site was to have a good navigation scheme. The bootstrap [navbar](http://twitter.github.com/bootstrap/components.html#navbar) wor

SNIP

This worked great until I started adding in [pagination](). The problem here is that each statically generated page gets the same tags as its parent does, meaning I had 3 'Home' li

### Navigation Rev. B

A crafty individual notes that you can parse hashes quite easily in Jekyll. After knowing this, I searched through the docs to find a way to create a hash. As it turns out, the eas

SNIP

Followed by this liquid template which I called `static_nav.html`:

SNIP

This template worked great! I could now specify which pages I wanted, and since the YAML specification I gave was a **list** of hashes, I even got to pick the ordering! Now, the di

### Navigation Rev. C - Final
There was only one thing my navigation was missing, the ability for posts to show the 'News' link highlighted. This part was a bit uglier than I'd liked, but it works very well. By

SNIP

### Final Navigation code
After seeing this trick, I used it again to allow the 'Home' button to remain highlighted for both `/` and `/index.html` URLs. This is the final result of `static_nav.html`

{% highlight jinja %}
{% raw %}
{% elsif page.url contains 'news/' %}
{% for link_hash in site.navigation %}
  {% for link in link_hash %}
    {% if page.url == link[1] %}
       <li class="active"><a href="{{ link[1] }}">{{ link[0] }}</a></li>
       {% elsif page.url contains 'index.html' %}
       {% if link[0] == "Home" %}
         <li class="active"><a href="{{ link[1] }}">{{ link[0] }}</a></li>
       {% else %}
         <li><a href="{{ link[1] }}">{{ link[0] }}</a></li>
       {% endif %}
       {% elsif page.url contains 'news/' %}
       {% if link[0] == "News" %}
         <li class="active"><a href="{{ link[1] }}">{{ link[0] }}</a></li>
       {% else %}
         <li><a href="{{ link[1] }}">{{ link[0] }}</a></li>
       {% endif %}
       {% else %}
       <li><a href="{{ link[1] }}">{{ link[0] }}</a></li>
       {% endif %}
  {% endfor %}
{% endfor %}
{% endraw %}
{% endhighlight %}
