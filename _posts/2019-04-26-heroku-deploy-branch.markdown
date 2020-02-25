---
layout: post
title: 'Deploy other branch to heroku'
date: 2019-04-26
tags:
  - terminal
keywords:
  - heroku
  - Deploy other branch to heroku
  - SPA and rails on one server
---

I am building an API server and SPA separately.
Since free dynos on Heroku are slow, to improve performance I put everything on one server.

<!--more-->

So, I

- `yarn build` SPA project
- put that built files to the `public` directory of my rails app
- my rails app is deployed to Heroku

Works like a charm on a couple of projects.

I have a situation when I want to have my `master` branch on github clean, without built content from SPA. Since I cannot have different `.gitignore` for different `remotes` (I wish it was possible 🤔), I use different branches: `master`, `development`, `feature` branches for development and `heroku` branch which I deploy to Heroku and don't push to github.

So, basically, each time I need to deploy I merge `master` into `heroku`, copy files from SPA and do the following

`git push heroku heroku:master`

| `heroku:master` means push local `heroku` branch to `master` on Heroku.

Hope it will help with your pet projects
