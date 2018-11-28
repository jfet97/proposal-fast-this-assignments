# Fast This Assignments

## Why?
In costructor functions and in the constructor() method in ES6 classes is easily to fall in the following pattern:

```js
f(par1, par2, ..., parN) {
  this.par1 = par1;
  this.par2 = par2;
  ...
  this.parN = parN;
}
```

For example:

```js
function Person(firstName, lastName, age) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.age = age;
}
```

or (ES6+)

```js
class Person {
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }
}
```


## What?
The proposal is simple: why do not speed up this situation?


## How?
My suggestion is to do so directly in the parameters definition using a simple dot `.`
```js
function Person(.firstName, .lastName, .age) {}
```

or

```js
class Person {
  constructor(.firstName, .lastName, .age) {}
}
```
So, if a parameter starts with a dot it will be added to `this` **mantaining the same name**.

### default parameters
Why not? 
```js
f(.par1 = defaultValue, .par2) {
}
