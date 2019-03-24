# Currying

> The act of refactoring a function to receive its arguments one at a time.

```js
const add = (a, b) => a + b

const add = a => b => a + b
```

## Arity

> Number of arguments a function receieves

* **Unary:** 1 argument
* **Binary:** 2 arguments
* **Ternary:** 3 arguments
* **Quaternary:** 4 arguments

## Partial application

> The process of passing a number of aruguments to a curried function, resulting in resusable functions of smaller arity.

```js
const request = baseUrl => endpoint => params => 
  fetch(`${baseUrl}${endpoint}`, params)
    .then(res => res.json())

const callAPI = request('https://api.com')

const getWeather = callAPI('/weather')
const getForecast = callAPI('/forecast')

getWeather({ zip: '60601' })
  .then(res => console.log(res))
```
