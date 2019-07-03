---
layout: post
title: 'Bookmarks for Typescript'
date: 2019-07-01
tags:
  - typescript
  - js
keywords:
  - typescript
  - ts-node
  - how to execute typescript from terminal or console
---

I've start learning `typescript` for project purposes. Typescript is nice, imho. But amount of boilerplate is killing me 😅. Hope it's benefitial in the end .

<!--more-->

## Helpful tool ts-node

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

## Interfaces

This is because when a class implements an interface, only the instance side of the class is checked. Since the constructor sits in the static side, it is not included in this check.

Instead, you would need to work with the static side of the class directly. In this example, we define two interfaces, ClockConstructor for the constructor and ClockInterface for the instance methods. Then, for convenience, we define a constructor function createClock that creates instances of the type that is passed to it:

```typescript
interface ClockConstructor {
  new (hour: number, minute: number): ClockInterface;
}
interface ClockInterface {
  tick(): void;
}

function createClock(
  ctor: ClockConstructor,
  hour: number,
  minute: number
): ClockInterface {
  return new ctor(hour, minute);
}

class DigitalClock implements ClockInterface {
  constructor(h: number, m: number) {}
  tick() {
    console.log('beep beep');
  }
}
class AnalogClock implements ClockInterface {
  constructor(h: number, m: number) {}
  tick() {
    console.log('tick tock');
  }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
```

Because `createClock`’s first parameter is of type `ClockConstructor`, in `createClock(AnalogClock, 7, 32)`, it checks that `AnalogClock` has the correct constructor signature.

Another simple way is to use class expressions:

```typescript
interface ClockConstructor {
  new (hour: number, minute: number);
}

interface ClockInterface {
  tick();
}

const Clock: ClockConstructor = class Clock implements ClockInterface {
  constructor(h: number, m: number) {}
  tick() {
    console.log('beep beep');
  }
};
```

#### Extending interfaces

```typescript
interface Shape {
  color: string;
}

interface Square extends Shape {
  sideLength: number;
}

let square = {} as Square;
square.color = 'blue';
square.sideLength = 10;
```

---

## Classes

they have `private` and `protected`, and `readonly` modifier

```typescript
class Animal {
  private name: string;
  protected color: string;
  readonly numberOfEyes: number;
  constructor(theName: string, numOfEyes: number = 2) {
    this.name = theName;
    this.color = 'Gray';
    this.numberOfEyes = numOfEyes;
  }
}

new Animal('Cat', 2).name; // Error: 'name' is private;
new Animal('Cat', 2).numberOfEyes = 3; // Cannot assign to 'numberOfEyes' because it is a read-only property.
```

#### get and set

Notic `private _fullName: string;`

```typescript
const fullNameMaxLength = 10;

class Employee {
  private _fullName: string;

  get fullName(): string {
    return this._fullName;
  }

  set fullName(newName: string) {
    if (newName && newName.length > fullNameMaxLength) {
      throw new Error('fullName has a max length of ' + fullNameMaxLength);
    }

    this._fullName = newName;
  }
}
```

##
