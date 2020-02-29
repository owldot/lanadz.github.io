---
layout: post
title: 'Change Port For npm start'
date: 2019-04-04
tags:
  - terminal
dont_show_excerpt_separator: true
keywords:
  - default port npm start
  - change port for react app
---

To change port when running `npm start` for your react app you need to add `PORT=XXXX` to your `package.json`
like this

{% highlight javascript%}
...
"scripts": {
"start": "PORT=8000 react-scripts start",
"build": "react-scripts build",
"test": "react-scripts test",
"eject": "react-scripts eject"
},
...
{% endhighlight %}
