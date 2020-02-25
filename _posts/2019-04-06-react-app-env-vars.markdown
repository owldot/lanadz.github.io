---
layout: post
title: 'React App Env Variables Are Undefined'
date: 2019-04-06
tags:
  - javascript
keywords:
  - react env vars undefined
  - env variable is undefined
  - process.env is empty
---

...I added variable to `.env.development` like this `API_URL=http://localhost:3000/`

...I restarted server

...I googled for an hour

...And I still was facing that `process.env.API_URL` is `undefined`

<!--more-->

Somehow I missed this from official [documentation](https://facebook.github.io/create-react-app/docs/adding-custom-environment-variables)

| Note: You must create custom environment variables beginning with REACT*APP*. Any other variables except NODE_ENV will be ignored to avoid accidentally exposing a private key on the machine that could have the same name. Changing any environment variables will require you to restart the development server if it is running.

That's smart, but 🤬

Anyways, don't forget to add `REACT_APP_` prefix to your variables
After renaming my environment variable to REACT_APP_API_URL I could carry on with my tasks 😊

Happy coding!
