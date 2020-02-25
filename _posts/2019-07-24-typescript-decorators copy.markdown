---
layout: post
title: 'Typescript decorators. Nice error logging example'
date: 2019-07-24
tags:
  - javascript
keywords:
  - error logging for typescript
  - typescript decorators
---

I've recently learnt about typescript decorators and would like to share and also show how you can create loggers easily with decorators

<!--more-->

At the moment of writing Typescript Decorators are in experimental mode, so as it says on official [typescript](https://www.typescriptlang.org/docs/handbook/decorators.html){:target="\_blank"} site

| NOTE  Decorator metadata is an experimental feature and may introduce breaking changes in future releases.

Remember that decorators are run when the class is created for the first time, **not when instance is created**

Ok,

1. In order to enable decorators please uncomment in `tsconfig.json`:

```JSON
{
    "compilerOptions": {
      "experimentalDecorators": true, /* Enables experimental support for ES7 decorators. */,
      "emitDecoratorMetadata": true /* Enables experimental support for emitting type metadata for decorators. */
    }
}
```

2. Now logging:

```javascript
class Car {
  @logError("oops it's broken") // applying decorator
  drive(): void {
    // throwing error to see how decorator logs into console
    throw Error;
    console.log('go go go');
  }
}
// wrapping decorator into function to accept
// arguments(in this case - string)
function logError(message: string) {
  // it needs to return decorator function
  return function(target: any, key: string, desc: PropertyDescriptor) {
    const method = desc.value;
    desc.value = () => {
      try {
        method();
      } catch (e) {
        console.log(message);
      }
    };
  };
}

new Car().drive();
```

first argument is prototype of the object, in our case it'll be `Car`

second the key of the property/method on the object, in this case it'll be `drive`

third - property descriptor

|-----|----|-----|
| writable | whether this property can be changed |
| enumerable |whether this propery can be looped with for..in |
| value | current value |
| configurable | whether property definition can be changed and property definition can be deleted |

Look at this example of descriptors:

<div style="width: 100%; text-align: center; padding: 20px 0;">
<img src="{{site.baseurl}}/assets/obj_descriptors.png"/>
</div>

Now let's take a closer look to:

```javascript
const method = desc.value;
desc.value = () => {
  try {
    method();
  } catch (e) {
    console.log(message);
  }
};
```

`desc.value` will hold the value(body) of the method. so we can call it later in `try` block. After that we reassign body(value) of this method by
`desc.value = () => {}`

So, basically, everytime we call the method, in case if it fails we will have our custom logging applied. Nice!

Thanks for reading!
