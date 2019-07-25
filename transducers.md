# Transducers

---

## Agenda

* What is a transducer?
* Where are they useful?
* Transducers are
* Example 1
* Example 2
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


## Resources

* https://medium.com/javascript-scene/transducers-efficient-data-processing-pipelines-in-javascript-7985330fe73d
* https://github.com/getify/Functional-Light-JS/blob/master/manuscript/apA.md/#appendix-a-transducing
 
