---
layout: post
title: 'Classes in Typescript'
date: 2019-07-03
tags:
  - javascript
keywords:
  - classes in typescript
  - public in constructor
---

Basic structure of the class, modifier usage etc..

<!--more-->

```typescript
class Vehicle {
  public move(): void {
    console.log('gogogogo');
  }
  private stop(): void {
    console.log('stopped');
  }

  public performStop() {
    this.stop();
  }

  constructor(public speed: number) {}
  //public modifier for parameter automaticaly does the same as below
  // speed: number;
  // constructor(speed: number) {
  //   this.speed = speed;
  // }
  static type: string = 'vehicle';
  static printType() {
    console.log(VehicleClass.type);
  }
}
```

```typescript
class Car extends Vehicle {
  constructor(public wheels: number, color: string) {
    // don't add public modifier to color because we don't want to reassign color property of Vehicle class
    super(color);
  }

  public move(): void {
    console.log('vroom');
  }
}

let car = new Car(4, 'black');
```
