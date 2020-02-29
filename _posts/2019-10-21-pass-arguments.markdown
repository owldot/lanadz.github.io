---
layout: post
title: 'Pass flags/arguments to command for internal command, for example to scripts in npm or yarn'
date: 2019-10-21
tags:
  - terminal
dont_show_excerpt_separator: true
keywords:
  - npm test add watch
  - Run command params inside other command
  - Add params script in package.json
---

So, let's say you need to add new flag `--watch` to your script from `package.json` In this case to "test" script

```json
"scripts": {
  "test": "jest"
}
```

Yes, you can add `"test": "jest --watch"` to your `package.json`, but that might be not very convinient, insted you can add this directly in terminal.
But mind, that

```bash
$ npm test --watch
```

won't work as expected because `--watch` will be considered as a part of npm command, not jest comman as we would like

So, what you should do instead is

```bash
$ npm test -- --watch
```

First pair of `--` associates arguments with `npm` (in our case we don't need any) and second pair of `--` will associate with `test` command (which is in our case is `jest`)

Hope you find this info helpful ;)
