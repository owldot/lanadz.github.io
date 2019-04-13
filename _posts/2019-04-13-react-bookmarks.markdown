---
layout: post
title: "You know that there is calc() in CSS, right?"
date: 2019-04-13
tags:
- css
keywords:
- css
- css calc
- css footer at the bottom
---

`calc()` function allows you to perform calculations when specifying CSS property value.
It's particulary interesting if you need to do some evaluations with different units

{% highlight css %}
width: calc(100% - 80px);
{% endhighlight %}

<!--more-->

| Note `-` an `+` signs should be surrounded by whitespace!

Nice thing it's supported by all major browsers

Now footer might be implemented in easy way, instead of looking for hacks with negative margins
{% highlight css %}
.container
  min-height: calc(100% - 50px)

.footer
    height: 50px
{% endhighlight %}
