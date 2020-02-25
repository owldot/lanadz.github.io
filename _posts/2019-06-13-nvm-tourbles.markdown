---
layout: post
title: 'nvm is not compatible with the npm config prefix option and other troubles'
date: 2019-06-13
tags:
  - terminal
keywords:
  - different versions of node
  - nvm is not compatible with npm config prefix
  - nvm is forgetting node for new terminal window
---

| nvm is not compatible with the npm config "prefix" option: currently set to "/usr local/Cellar/nvm/0.34.0/versions/node/v10.16.0" <br/>Run `npm config delete prefix` or `nvm use --delete-prefix v10.16.0 --silent` to unset it.

Looks familiar?

<!--more-->

Ok, I need to be able to change `node` versions for different projects. `npm` is handy tool for such cases.

| _ I don't have `node` installed globally<br/> _ `nvm` version 0.34 installed via `brew`</li>

Problem was that my fresh opened console didn't know command `node`, so
I had to run `nvm use xxx`,
then it complained on `config prefix option`,
then running `npm config delete prefix` didn't give any result because `env: node: No such file or directory`,
so I needed to run `nvm use --delete-prefix v10.16.0` which is solution for open tab of terminal only.
And I was supposed to repeat that everytime I open new window in terminal.

### Solution

How I fixed that:

- run `nvm use --delete-prefix v10.16.0` to be able to start `node`
- run `node` to ensure that it's working
- run `npm config delete prefix` to ensure that it's deleted
- run to set it back `npm config set prefix $NVM_DIR/versions/node/v10.16.0`

So, basically I needed to reset nvm prefix.

I hope it will help you too 🤓

Cheers!
