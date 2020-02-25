---
layout: post
title: 'ESLint: Destructure deeply nested objects and return default value for undefined '
date: 2019-06-22
tags:
  - javascript
keywords:
  - eslint destructuring
  - eslint destructuring deeply nested objects
  - js default value for undefined
---

ESLint is awesome, yes it makes you better developer, but this error in my terminal

<div class='errorDiv'>
<div class='content'>
error:  Must use destructuring state assignment
</div>
</div>

drives me crazy sometimes!

<!--more-->

How should I fix that eslint error when I have this deeply nested object and I need to get `ip` field:

```JS
const loggedUserObj = {
    loggedUser: {
        user: {
            name: 'John',
            lastLoggedIn: {
                date: '1970-01-01',
                ip: '0.0.0.0'
            }
        },
        settings: {
            setting1: "1",
            setting2: "2",
        }
    }
};
```

often I ended up with `// eslint-disable-next-line react/destructuring-assignment` and carry on 😅

Recently I found how it can be done in a **right** way. Without further ado, look how we destructure simple object:

```JS
const obj = {
    props: {
        field: "A"
    }
}
```

will become nice:

```JS
const { props } = obj;
const { props: { field } } = obj;

console.log(props); // shows {field: "A"}
console.log(field); // shows "A"
```

See? So, similary, from our previous example with `loggedUserObj` object, to get `ip` field, you need:

```JS
const {
    loggedUser: {
        user: {
            lastLoggedIn: {
                ip
            }
        }
    }
} = loggedUserObj;

console.log(ip); //shows "0.0.0.0"
```

YAY!!🙌

Now, what to do if you have `undefined`?
Say, you want to access `platform` field instead of `ip`:

```JS
const {
    loggedUser: {
        user: {
            lastLoggedIn: {
                platform
            }
        }
    }
} = loggedUserObj;

console.log(platform);// this will print undefined
```

In this case you may want to have default value, and to get it is simple as

```
const {
    loggedUser: {
        user: {
            lastLoggedIn: {
                platform = 'None'
            }
        }
    }
} = loggedUserObj;

console.log(platform);// this will print "None"
```

Yeah, solution is not the prettiest. You moved the real problem from one place to another. Now you have huge assignment statement, but you satisfy eslint. Good solution would be to find how to make that `loggedUserObj` object shallower, but that's another topic..

Hope it will save your time.

Thanks for reading!
