---
layout: post
title:  "npm and jest on mac"
date:   2019-03-20 
categories: configuration npm js osx
---
Hi,

I started going through some exercises for JS (getting ready for interviews heheh)

And there they asked to have `jest` installed on my computer to run tests from those challenges

Oh, yes, [jest](https://jestjs.io/docs/en/getting-started.html) looks very promising test framework to me. Finally my TDD skills from Ruby world can be applied to JS 😄   

After following installation guide and trying to run tests, I was receiving error `$ command not found: jest` 
Solution was to add `sudo` before command (which wasn't mentioned in guide 😞)
{% highlight bash %}
sudo npm install -g jest
{% endhighlight %}

Thanks