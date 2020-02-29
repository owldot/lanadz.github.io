---
layout: post
title: 'ESLint: Ignore file. Ignore console.log()'
date: 2019-06-16
tags:
  - javascript
dont_show_excerpt_separator: true
keywords:
  - eslint ignore file
  - disable eslint for file
---

If you need to mute/disable ESLint for particular file only, put this as a **first** line in that file

```js
/* eslint-disable */
```

Another rule to override (if needed), but for whole project this time:

In your `.eslintrc.js`:

```js
rules: {
        'no-console': 'off',
    },
```
