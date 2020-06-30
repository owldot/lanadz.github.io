---
layout: post
title: "You've Got Email"
date: 2019-02-08
tags:
  - terminal
  - backend
keywords:
  - Youve Got Email
  - action_mailer
  - configuration for mailgun rails
  - mailgun rails
  - mailgun
---

Hey guys,

Today I've spent some time setting up emails for my [www.owldot.com](www.owldot.com)

That was a pretty great experience as I never did it before. I mean all configuration for production from scratch.

<!--more-->

The task was to send welcome email to the new users. And also notify me by email that I've got a new user registered to OwlDot. Common task, right? So, what I used:

- Rails Action Mailer is a common library
- Mailgun - popular email service for sending, receiving and tracking email
- OwlDot are hosted on Heroku

Everything started super easy:

`rails g mailer UserMailer` and with help of [official tutorial](https://guides.rubyonrails.org/action_mailer_basics.html) from Rails I managed to start sending welcome emails in less than an hour. That on local env, of course.
But then we need to deploy, right? And a production environment is a completely different story.

First thought was just use Gmail's SMTP configuration and that's it. Something like [rails guide](https://guides.rubyonrails.org/action_mailer_basics.html#action-mailer-configuration-for-gmail) suggests:

{% highlight ruby %} # Add following to config/environments/\$RAILS_ENV.rb

    config.action_mailer.delivery_method = :smtp
    config.action_mailer.smtp_settings = {
      address:              'smtp.gmail.com',
      port:                 587,
      domain:               'example.com',
      user_name:            '<username>',
      password:             '<password>',
      authentication:       'plain',
      enable_starttls_auto: true }

{% endhighlight %}

But then you will face all those issues if you go with Google SMTP conf (you will need to turn off 2-FA, generate app-password, etc.), limit of sent/received emails, custom domains, lack of tracking tools (you will need to build your own solutions).

It didn't look like a reasonable solution for production. So, I started to look for some ready services.

OwlDot is hosted on Heroku and Heroku gives you couple services as add-ons to your application. I picked [Mailgun](https://www.mailgun.com) as they had everything i needed and for free 😍. Such as custom domain, 10K free mails, tracking and dashboard.

Setup with registar was pretty easy. You need to go to your registar (godaddy, name, etc) and update some dns settings. Those settings will be provided for you by mailgun's setup wizard and will look something like this:

![screenshot](/assets/screen-email.png)

Mind it may take up to 48 hours to update. In my case, I was lucky it started to work within 5-10 minutes 😅

Great help was [official gem from mailgun](https://github.com/mailgun/mailgun-ruby) Few lines in initializer and production.rb: And no more changes to my Rails app.

{% highlight ruby %}

# config/initializers/mailgun.rb

    Mailgun.configure do |config|
      config.api_key = 'your-secret-api-key'
    end

{% endhighlight %}

{% highlight ruby %}

# config/environments/production.rb

    config.action_mailer.delivery_method = :mailgun
    config.action_mailer.mailgun_settings = {
        api_key: 'api-myapikey',
        domain: 'mydomain.com',
    }

{% endhighlight %}

Now, I can send/receive emails from my `@owldot.com` to happy new users.

That's it. I hope this comes in handy for you.
