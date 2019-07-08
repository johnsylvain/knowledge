# transducers

> "Transform" + "Reducer"

Transducers are an efficient way to transform a list of data. Instead of performing multiple iterations over the same set of data, we can perform them all in one go.

To create a transducer, the output of one reducer should be piped through to another.

### Bad

```js
largeDataSet
  .map(doSomething)
  .filter(filterSomething)
  .reduce(reduceSomething)
```

### Good

```js
// a map transducer
function tMap(transform) {
  return function transduce (reducer) {
    return (target, cur) => {
      const processed = transform(cur)
      return reducer(target, processed)
    }
  }
}

const doSomething = tMap(item => item.name)

largeDataSet
  .reduce(doSomething)

```

## rambda

Rambda.js has its own implementation: https://ramdajs.com/docs/#transduce

## reference

https://medium.com/@roman01la/understanding-transducers-in-javascript-3500d3bd9624
https://jrsinclair.com/articles/2019/magical-mystical-js-transducers/
