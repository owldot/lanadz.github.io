---
layout: post
title: 'Ruby Server Oneliner'
date: 2019-03-27
tags:
  - backend
  - terminal
keywords:
  - ruby server
  - webrick
  - run server from the current dir
---

When you need to have a web server there is this command available for you.

This will run webrick and serve current directory on port 9090 (you can change it)

{% highlight bash%}
ruby -rwebrick -e'WEBrick::HTTPServer.new(:Port => 9090, :DocumentRoot => Dir.pwd).start'
{% endhighlight %}

<!--more-->

I like to have this as a alias on my computer. Add this to your `.bash*` file. Then command `fakeserver` will be available for you

{% highlight bash%}
alias fakeserver="ruby -rwebrick -e'WEBrick::HTTPServer.new(:Port => 9090, :DocumentRoot => Dir.pwd).start'"
{% endhighlight %}
