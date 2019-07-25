# Currying

## Tom Wilson

---

## Agenda

* Functions as values
* What is currying
* Manual Currying
* Partial Application
* Auto Currying
* Demo
* Resources

---

# Functions as values

```js
function add (a,b) {
  return a + b
}

function sum (...numbers) {
  // passing the add function as a value to 
  // the reduce function
  return numbers.reduce(add, 0)
}
```

---

# What is currying?

Currying is the technique of translating the evaluation of a 
function that takes multiple arguments into a sequence of 
functions that take a single argument.

```js
function add (a) {
  return function (b) {
    return a + b
  }
}

add(1)(2)
```

---

# Manual currying

Manual currying is where the developer manually changes the 
shape of a function using closure.

```js
function add (a,b) {
  return a + b
}

export function addx(a) {
  return function (b) {
    return add(a,b)
  }
}
```

---

# Map Example

```js
function inc(v) { return v + 1 }

export function map(fn) {
  return function (data) {
    return data.map(fn)
  }
}

map(inc)([1,2,3,4])
// > 2,3,4,5

```

---

# Partial Application

Partial Application is similar to currying, it is a function 
that will accept one or more arguments and if all the arguments 
are not supplied, it will return a function looking for the 
rest of the arguments. If all the arguments are supplied it 
will run the function.

---

# Partial Application Example

```js
function add_three(a,b,c) {
  return a + b + c
}

// here is what a partial application implementation looks like
function add_three(a,b,c) {
  if (!a) {
    return function(a,b,c) {
      return a + b + c
    }
  }
  if (!b) {
    return function (b,c) {
      return a + b + c
    }
  }
  if (!c) {
    return function (c) {
      return a + b + c
    }
  }
  return a + b + c
}

```

---

# Auto-Currying

Auto Currying is the best of both curry and partial application, 
auto curry will take arguments one by one or all at the same 
time or any combination.

> This is hard to implement and there are libraries that implement 
this functionality very well. (See Resources)


```js
import { curry } from 'ramda'

export const add = curry(function(a,b) {
  return a + b
})

export const map = curry(function(fn, list) {
  return list.map(fn)
})
```


---

# Demo

https://ramdajs.com/repl/

---

# Resources

* Ramda - https://ramdajs.com
* LoDash FP - https://github.com/lodash/lodash/wiki/FP-Guide


