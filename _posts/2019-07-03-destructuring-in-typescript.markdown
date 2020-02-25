---
layout: post
title: 'Destructuring in Typescript'
date: 2019-07-03
tags:
  - javascript
keywords:
  - typescript destructuring
---

Little bit confusing. Bare with me.

Usualy, what you do is

```typescript
let variable: number;
```

Means you want to have a variable of `number` type

<!--more-->

But in case with destructuring you need to specify type of the object you destructuring from. Like this:

```typescript
const profile = {
  name: 'lana',
  age: 30,
  coords: {
    lat: 0,
    lon: 0
  },
  setAge(age: number): void {
    this.age = age;
  }
};

const { age }: { age: number } = profile; // we need to specify { age: number }, not just a number. The thing is, you annotate "profile" object, not "age" variable
const {
  coords: { lat, lon }
}: { coords: { lat: number; lon: number } } = profile; // same here, we need to specify that we expect this kind of structure in "profile" object to get coords
```

The reason behind in this:

```typescript
const { age, name }: ......... = profile; // we cannot say type is number, because "name" is a string, and cannot say string because the "age" is number

```

🧐

Just a little bit more complex example

```typescript
interface Vehicle {
  name: string;
  year: Date;
  broken: boolean;
}

const printVehicle = ({ name, year, broken }: Vehicle): void => {
  //see destructuring and type annotation  ^^^^
  console.log(name);
  console.log(year);
  console.log(broken);
};

const oldCivic = {
  name: 'civic',
  year: new Date(1999),
  broken: true
};

printVehicle(oldCivic);
```
