---
layout: post
title: 'gem install mysql2'
date: 2019-02-08
tags:
  - terminal
  - db
keywords:
  - gem install mysql2
  - configuration
  - gem install mysql2
  - mysql
  - rails
---

Oh, I was so angry.

I had some free time this morning to help out my friend with some UI and it happen to me that I didn't have mysql installed. So I couldn't run his app locally

<!--more-->

So, there I was sitting in Starbucks on my mobile data (don't like to use open wifi you know) and trying to setup env for that projects. The culprit was mysql and mysql2 gem :(

but anyways, following help me:
{% highlight bash %}
gem install mysql2 -v '0.5.2' -- --with-mysql-lib=/usr/local/opt/mysql@5.7/lib --with-mysql-lib=/usr/local/opt/mysql@5.7 --with-mysql-config=/usr/local/opt/mysql@5.7/bin/mysql_config --with-mysql-include=/usr/local/opt/mysql@5.7/include

gem install mysql2 -v '0.5.2' -- --with-ldflags=-L/usr/local/opt/openssl/lib --with-cppflags=-I/usr/local/opt/openssl/include
{% endhighlight %}

So if you are stuck that might help you, just remember to add all paths to your mysql together with `bundle install` command
