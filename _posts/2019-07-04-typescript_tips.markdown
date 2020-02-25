---
layout: post
title: 'Miscellaneous for Typescript, VSCode'
date: 2019-07-04
tags:
  - javascript
keywords:
  - typescript tools
  - typescript configurations
  - typescript tips and tricks
  - how to execute typescript from terminal or console
---

## #Miscellaneous

### VSCode

If you are digging through type annotation file `*.d.ts` you might find it helpfull to fold some declarations to level 2 only.

Hit `cmd`+`shift`+`P` to get the command pallete and look for `Fold Level 2`.

After that you will see declarations collapsed:

<div class="image-with-title-dont-change-size">
<img src="{{site.baseurl}}/assets/ts.fold.png"/>
<p>Folded to level 2 and now easily can be seen all declarations</p>
</div>
<!--more-->
### Helpful tool ts-node

`npm install -g ts-node` helps you to test your typescript from console without prior compiling `ts` files. Basically, instead of:

```
$tsc file.ts
$node file.js
```

you will be able to do:

```
$ts-node file.ts
```

Pretty handy.

### Tool to help us run Typescript in the browser

```
npm install -g parcel-bundler
```

### Library to generate fake data

```
npm install faker
```

And because type definitions are not included we also need to install `npm install @types/faker`

### Typescript convention

Try to avoid `default` for exports. The reason for that is we don't need to worry if we need or not to include curly braces in import statement.

```typescript
export const red = 'red';
export const green = 'green';
```

```typescript
import { red, green } from 'file';
```

---
