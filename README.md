# Fast This Assignments


## Why?
In costructor functions and in the constructor() method in ES6 classes is easily to fall in the following pattern:

```js
F(par1, par2, ..., parN) {
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
This is not DRY.



## What?
The proposal is simple: why do not speed up this situation removing the avoidable repetitions?



## How?
My suggestion is to do so directly in the parameters definition by prefixing a simple dot `.`
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
F(.par1 = defaultValue, .par2) {}
```


### destructured parameters
I think there should not be problems:
```js
F( { .par1, .par2 } ) {}
```
instead of
```js
F( { par1, par2 } ) {
  this.par1 = par1;
  this.par2 = par2;
}
```

and

```js
F( [ .par1, .par2 ] ) {}
```
instead of
```js
F( [ par1, par2 ] ) {
  this.par1 = par1;
  this.par2 = par2;
}
```


### rest operator
I am not sure here, I don't like four dots in row but in case we could trasform this:
```js
F( ...args ) {
  this.args = args; // maybe useless but it is an edge case
}
```

into:
```js
F( .(...args) ) {}
```



## Further ideas
We can go one step further: if not specified `this` could be the default object, but we could also choose to put the arguments into one or more object.

So:
```js
f(.par1, .par2, .parN) {}
```
is equivalent to:
```js
f(this.par1, this.par2, this.parN) {}
```

BUT you could also do something like:
```js
f(obj.par1, obj.par2, obj2.parN) {}
```
that is equivalent to:
```js
f(par1, par2, parN) {
  let or const obj = { par1, par2 };
  let or const obj2 = { parN };
}
```

### going faster
When or more than one parameters should be inserted into an object:
```js
f(.{par1, par2, parN}) {}
```
equivalent to:
```js
f(this.{par1, par2, parN}) {}
```
that is equivaent to:
```js
f(.par1, .par2, .parN) {}
```
and:
```js
f(this.par1, this.par2, this.parN) {}
```

SO we could write that:
```js
f(obj.par1, obj.par2, obj2.parN) {}
```
in this way:
```js
f(obj.{par1, par2}, obj2.parN) {}
```


## Notes
If a class inherited from another, this syntax should be forbidden for the constructor method because `this` is not available before the `super()` call.


### workarounds
```js
F( par1, par2 ) {
  Object.assign(this, {par1, par2});
}
```
Clever one but there are always (theoretically) an "useless" object creation and a function call, as well as the repetition of the parameters.


### I don't like the dot
Ok. We have not to use `.`, only something easy and fast to write but not misunderstoodable.

