---
layout: post
category: news
header: Turning Jekyll into Mr. Hyde
author: fernferret
---

This website you're viewing is generated with [Jekyll](https://github.com/mojombo/jekyll), which, as I've [mentioned before](/news/multiverse-blog.html), is awesome. Jekyll takes ordinary markdown pages combined with the [liquid templating engine](https://github.com/shopify/liquid/wiki/liquid-for-designers) to produce a complete site that appears as if it has a lot more functionality than it really does. This has several advantages, including security (all pages are static) and speed! It can do a lot of really cool things, but as the name of the project appropriately describes, it can be a bit _unruly at times_.

The other tool that can get a Jekyll site setup very quickly (for those of us who are not creative at all) is [Twitter Bootstrap](http://twitter.github.com/bootstrap/). The base css layout are simply incredible.

Performing basic tasks is really easy, such as getting the base site architecture setup. The initial layout is [already done for you](https://github.com/mojombo/jekyll/wiki/Usage). Beyond that there are a lot of very cool things you can do (most of them without plugins).

### Getting started
I definetly can't take credit for all of the tips and tricks used here. If you're looking for a good place to start, go to [Jekyll Bootstrap](http://jekyllbootstrap.com/). Go there now.

### Navigation Rev. A
One of the most important aspects of building this site was to have a good navigation scheme. The bootstrap [navbar](http://twitter.github.com/bootstrap/components.html#navbar) works perfectly, but for a blog, it doesn't fit well at the top. I started off with a variant of some code provided in Jekyll Bootstrop to list all the pages with a given category:

{% highlight jinja %}
  {% literal %}
  {% for node in pages_list %}
    {% if group == null or group == node.group %}
      {% if page.url == node.url %}
      <li class="active"><a href="{{ node.url }}">{{ page.root }}{{ node.title | capitalize }}</a></li>
      {% else %}
      <li><a href="{{ node.url }}">{{ node.title | capitalize }}</a></li>
      {% endif %}
    {% endif %}
  {% endfor %}
  {% assign pages_list = null %}
  {% assign group = null %}
  {% endliteral %}
{% endhighlight %}

This worked great until I started adding in [pagination](). The problem here is that each statically generated page gets the same tags as its parent does, meaning I had 3 'Home' links appearing in my navbar. This also didn't handle when a user clicked on an entry (like the one you're reading now). The navbar would simply loose focus, which isn't bad, but I'd prefer to have it go to a 'News' section or something.

### Navigation Rev. B

A crafty individual notes that you can parse hashes quite easily in Jekyll. After knowing this, I searched through the docs to find a way to create a hash. As it turns out, the eas

{% highlight yaml %}
    navigation:
      - Home: /
      - News: /news.html
      - About: /about.html
      - Contribute: /contributing.html
      - Download: /download.html
{% endhighlight %}

Followed by this liquid template which I called `static_nav.html`:

{% highlight jinja %}
{% literal %}
{% for link_hash in site.navigation %}
  {% for link in link_hash %}
    {% if page.url == link[1] %}
    <li class="active"><a href="{{ link[1] }}">{{ link[0] }}</a></li>
    {% else %}
    <li><a href="{{ link[1] }}">{{ link[0] }}</a></li>
    {% endif %}
  {% endfor %}
{% endfor %}
{% endliteral %}
{% endhighlight %}

This template worked great! I could now specify which pages I wanted, and since the YAML specification I gave was a **list** of hashes, I even got to pick the ordering! Now, the disadvantage of this is obviously that new content published won't automatically show up in the navigation bar like it would before, but I really don't mind editing some YAML to put a new item in the navigation!

### Navigation Rev. C - Final
There was only one thing my navigation was missing, the ability for posts to show the 'News' link highlighted. This part was a bit uglier than I'd liked, but it works very well. By adding an `elsif` clause, right after checking to see if the current page's url was the link I'd specified in YAML, I could then see if the page's url contained `news/`. If so, I knew it was a news post!

{% highlight jinja %}
{% literal %}
{% elsif page.url contains 'news/' %}
   {% if link[0] == "News" %}
     <li class="active"><a href="{{ link[1] }}">{{ link[0] }}</a></li>
   {% else %}
     <li><a href="{{ link[1] }}">{{ link[0] }}</a></li>
   {% endif %}
{% endliteral %}
{% endhighlight %}

### Final Navigation code
After seeing this trick, I used it again to allow the 'Home' button to remain highlighted for both `/` and `/index.html` URLs. This is the final result of `static_nav.html`.

{% highlight jinja %}
{% literal %}
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
{% endliteral %}
{% endhighlight %}

### Other Remarks

Well, I finally have this page up (only about 15 days later than first written). The main reason for this was my use of the `{% literal %}{% raw %}{% endliteral %}` tag. As it turns out, I was using [liquid](http://liquidmarkup.org/) **2.4.1** with [jekyll](http://jekyllrb.com/) **0.11.2** while [github.com](http://www.github.com) uses **2.2.2** and **0.11.0**, respectively. The `{% literal %}{% raw %}{% endliteral %}` tag was introduced in liquid 2.3.0, whereas 2.2.2 uses the `{% literal %}{% literal %}{% endliteral %}` tag. Subtle difference, but it was causing the site not to build. I ultimately found the answer [hidden in the help pages](https://help.github.com/articles/pages-don-t-build-unable-to-run-jekyll).
