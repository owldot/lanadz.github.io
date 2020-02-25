---
layout: post
title: 'SASS and Rails Asset Pipeline. Remove duplicates of imports.'
date: 2019-02-20
tags:
  - backend
  - frontend
keywords:
  - SASS and Rails Asset Pipeline Remove duplicates of imports
  - rails sass
  - Remove duplicates of imports sass
  - Asset Pipeline
---

Hi there,

Another day another problem :)

Let me show what I noticed with my css on the project.

Here is the structure of the project, which is common for most rails project, I think

<!--more-->

In my `application.sass` I had

{% highlight html %}
/\* _= require_tree .
_= require*self
*= require 'bulma.min'
\_/

@import "select2"
@import "values.sass"
@import "overrides.sass"

some general rules
///
{% endhighlight %}

Later I added `file1.sass` where I wanted to use those rules defined in `values.sass`, but guess what, I couldn't use those values, because I got an error

{% highlight bash %}
Showing /app/views/layouts/application.html.haml where line #9 raised:
Undefined variable: "\$gray_dark".
{% endhighlight %}

hmmmm

Easy fix - let's add `@import "values.sass"` to the `file.sass`
Yay! It rendered.

Next, I had to create another file (because we are separating rules for each screen).
And again error about undefined variable, fix - add `@import "values.sass"`

And the process continues...

If you open you compiled file you will see something like:

{% highlight bash %}

# rules from values.css …<------ duplicated

# rules from file.css …

# rules from values.css …<------ duplicated

# rules from another_file.css …

{% endhighlight %}

Do we need this? nooo, it's hard to call a solution, when you have those copied rules multiple times. In the end, our poor users have to download all those long and big files.

So I started to read documentation :) RTFM, right? X))))))

From [sass-rails](https://github.com/rails/sass-rails#important-note)

> Sprockets provides some directives that are placed inside of comments called require, require_tree, and require_self. DO NOT USE THEM IN YOUR SASS/SCSS FILES. They are very primitive and do not work well with Sass files. Instead, use Sass's native @import directive which sass-rails has customized to integrate with the conventions of your Rails projects.

Which basically means we need to @import our files instead of \*=require. So easy and right fix will be to rewrite application.sass to:

{% highlight html %}
// remove \*=require directives
// and add

@import "values
@import "overrides"

// List all your project's file
@import 'file1'
@import 'file2'
@import 'file3'
{% endhighlight %}

After that you will not have any issues with duplications or scope problems.

Thanks for reading!
