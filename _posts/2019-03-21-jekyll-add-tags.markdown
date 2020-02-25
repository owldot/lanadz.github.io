---
layout: post
title: 'How to add tags/categories to jekyll blog'
date: 2019-03-21
tags:
  - terminal
keywords:
  - jekyll add tags or categories
  - jekyll tags
  - jekyll categories
---

Hi,

Today I want to share with you how to add tags/categories to your awesome blog

Basically, I was following this [guide](https://blog.webjeda.com/jekyll-categories/)

<!--more-->

Shortly,

- you need to create permalink in your `_config.yml`

  `permalink: /tags/:title/`

- create a page `tags.html`
  {% highlight html %}
  {% raw %}

---

layout: page
permalink: /tags/
title: Tags

---

<div>
    {% for tag in site.tags %}
    <div>
        {% capture tag_name %}{{ tag | first }}{% endcapture %}
        <div id="#{{ tag_name | slugize }}"></div>
        <p></p>

        <a name="{{ tag_name | slugize }}" href="#{{ tag_name | slugize }}">#</a>
        <span>{{ tag_name }}</span>
        <ul>
        {% for post in site.tags[tag_name] %}
        <li><article>
            <a href="{{ site.baseurl }}{{ post.url }}">{{post.title}}</a>
        </article>
        {% endfor %}
        </ul>
    </div>
    {% endfor %}

</div>
{% endraw %}
{% endhighlight %}

- add `tags` key with array of values to your front matter to each of your post
  {% highlight yaml %}
  tags: - tag1 - tag2
  {% endhighlight %}
