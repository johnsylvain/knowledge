# transducers

> "Transform" + "Reducer"

Transducers are an efficient way to transform a list of data. Instead of performing multiple iterations over the same set of data, we can perform them all in one go.

### Bad

```js
largeDataSet
  .map(doSomething)
  .filter(filterSomething)
  .reduce(reduceSomething)
```

### Good

```js
function tMap(transform) {
  return function transduce (reducer) {
    return (target, cur) => {
      const processed = transform(cur)
      return reducer(target, processed)
    }
  }
}
```

## reference

https://medium.com/@roman01la/understanding-transducers-in-javascript-3500d3bd9624
