---
layout: post
title: 'Difference between function(){} and arrow functions'
date: 2019-10-26
tags:
  - javascript
keywords:
  - Difference between function(){} and arrow functions
  - normal functions vs arrow functions
  - javascript arrow functions
---

Here are couple examples of functions and arrow functions

- Definition

  > ```javascript
  > function setName(name) {
  >   this.name = name;
  > }
  > ```

  > ```javascript
  > const setName = name => {
  >   this.name = name;
  > };
  > ```

  <!--more-->

- Objects, (classes)

  > ```javascript
  > const obj = {
  >   getName: function() {}
  > };
  > ```

  > ```javascript
  > const obj = {
  >   getName: () => {}
  > };
  > ```

- Callbacks

  > ```javascript
  > setTimeout(function() {
  >   // ...
  > }, 500);
  > ```

  > ```javascript
  > setTimeout(() => {
  >   // ...
  > }, 500);
  > ```

As you may know we cannot blindly substitude one notation with another. Because _it depends_, as always 😀

So, what's the difference

## Lexical `this` and `arguments`

Arrow functions don't have their own `this` or `arguments` binding.

```javascript
var test = {
  prop: 42,
  func: function() {
    console.log('arguments: ' + arguments[0]);
    return this.prop;
  },
  arrow: () => {
    console.log('arguments: ' + arguments[0]);
    return this.prop;
  }
};

console.log(test.func(1, 2));
// expected output: "arguments: 1", 42
console.log(test.arrow(2, 3));
// expected output: "arguments: undefined", undefined
```

From [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions):

> An arrow function expression is a syntactically compact alternative to a regular function expression, although without its own bindings to the `this`, `arguments`, `super`, or `new.target` keywords. Arrow function expressions are ill suited as methods, and they cannot be used as constructors.

Before arrow functions, every new function defined its own `this` value based on how the function was called:

```javascript
function Person() {
  // The Person() constructor defines `this` as an instance of itself.
  this.age = 0;

  setInterval(function growUp() {
    console.log(this.age); // <--- will print out undefined.
    // this is not Person's `this`, but function's own `this`
  }, 1000);
}

var p = new Person();
// will print out undefined
```

and same, but with arrow function

```javascript
function Person() {
  this.age = 0;

  setInterval(() => {
    console.log(this.age); // <--- will print out 0.
    // this is Person's `this`, function doesn't have its own `this`
  }, 1000);
}

var p = new Person();
```

I encourage to try this in console to see the difference

Arrow function expressions are best suited for **non-method** functions. Let's see what happens when we try to use them as methods

```javascript
'use strict';

var obj = {
  // does not create a new scope
  i: 10,
  b: () => console.log(this.i, this),
  c: function() {
    console.log(this.i, this);
  }
};

obj.b(); // prints undefined, Window {...} (or the global object)
obj.c(); // prints 10, Object {...}
```

Arrow functions cannot be used as constructors and will throw an error when used with `new`.

```javascript
var Foo = () => {};
var foo = new Foo(); // TypeError: Foo is not a constructor
```
