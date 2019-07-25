# Transducers

---

## Agenda

* What is a transducer?
* Where are they useful?
* Transducers are
* Examples
* Summary
* Resources 

---

## What is a transducer?

A transducer is a function that accepts a reducing function and returns reducing function.

Because of this, we can compose any number of transducers using function composition.

This means that we can compose a map, filter and reduce to create a one pass pipeline.

---

## Where are transducers useful?

> The goal of the Transducer Protocol is that all JavaScript transducer implementations 
> interoperate regardless of the surface level API. It calls transducers independently 
> from the context of their input and output sources and specifies only the essence of 
> the transformation in terms of an individual element.

> Because transducers are decoupled from input or output sources, they can be used in 
> many different processes — collections, streams, channels, observables, etc. Transducers 
> compose directly, without awareness of input or creation of intermediate aggregates.

---

## Transducers are:

* Composable using simple function composition
* Efficient for large collections or infinite streams: Only enumerates over the elements 
  once, regardless of the number of operations in the pipeline
* Able to transduce over any enumerable source (e.g., arrays, trees, streams, graphs, etc…)
* Usable for either lazy or eager evaluation with no changes to the transducer pipeline

---

## Why Transducers

When processing data it is useful to break up the processing into small composable stages.

map > filter > reduce

But when you do this via chaining or with immutable structures, you are creating a copy 
of data at each stage of the process. With small data this is fine, but with large data
and live streams, this can be a challenge. Transducers allow you to keep the steps 
in the process loosely coupled, but compose them into one reducer with one pass,
so that you do not have create copies of data structures between each step.


---

## Demo

1. map over the numbers and square each number
2. use filter keep numbers divisible by 8

```js
import { map, filter, reduce, add, compose, into } from 'ramda'

const numbers = [1,2,3,4,5,6,7,8,9,10]
const squareNumbers = map(v => v ** 2)
const keepDiv8s = filter(v => v % 8 === 0)

const transducer = compose(squareNumbers, keepDiv8s)

```

> Use transducers to only have one pass

```js
const result = into([], transducer, numbers)
```

[Example](https://ramdajs.com/repl/?v=0.26.1#?const%20numbers%20%3D%20%5B1%2C2%2C3%2C4%2C5%2C6%2C7%2C8%2C9%2C10%5D%0Aconst%20squareNumbers%20%3D%20map%28v%20%3D%3E%20v%20%2A%2A%202%29%0Aconst%20keepDiv8s%20%3D%20filter%28v%20%3D%3E%20v%20%25%208%20%3D%3D%3D%200%29%0A%0Aconst%20transducer%20%3D%20compose%28squareNumbers%2C%20keepDiv8s%29%0A%0A%2F%2F%20const%20result%20%3D%20into%28%5B%5D%2C%20transducer%2C%20numbers%29)

---

## Break it down

Transducers are a higher order function that takes a reducer and returns a reducer

```
function xMap(fn) {
  return function (step) {
    return function (a, v) { return step(a, fn(v)) }
  }
}


function xFilter(predicate) {
  return function (step) {
    return function (a,v) {
      return predicate(v) ? step(a,v) : a
    }
  }
}

const transducer = compose(xMap(v => v * 2), xFilter(v => v % 2 === 0))

const result = into([], transducer, [2,3,4,5,6,7,9])
```

---

## In Summary

Tranducers are higher order functions that return a reducer and can be composed to 
create single pass pipelines to process large and live data structures while keeping
the process steps isolated.

---


## Resources

* https://medium.com/javascript-scene/transducers-efficient-data-processing-pipelines-in-javascript-7985330fe73d
* https://github.com/getify/Functional-Light-JS/blob/master/manuscript/apA.md/#appendix-a-transducing

 
