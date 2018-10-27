# jsx

jsx is a syntax extension to JavaScript. It transpiles into function calls containing a `nodeName`, `attributes`, and `children`.

## A simple jsx renderer

```js
function h(nodeName, attributes = {}, ...children) {
  return {
    nodeName,
    attributes,
    children: [].concat.apply([], children)
  }
}
```

The above function creates a virtual DOM node, and contains all that is needed to construct a real DOM node.

Using Babel, we can define our own jsx pragmas in a `.babelrc` file.

```json
{
  "presets": ["env"],
  "plugins": [["transform-react-jsx", { "pragma": "h"  }]]

}
```

```js
const vnodes = (
  <div className="header">Hello</div>
);

// Transpiles to:
var vnodes = h(
  'div',
  { className: 'header' },
  'Hello'
);
```

If a jsx element function begins with a capital letter,
Babel will treat is as a function. We can refactor our `h`
function to evaluate these functions. Here we pass the `attributes`
(or `props` if you are used to React) and `children` as the first
to arguments to the function call.

```js
function h(nodeName, attributes = {}, ...children) {
  children = [].concat.apply([], children);

  return typeof nodeName === 'function'
    ? nodeName(attributes, children)
    : {
      nodeName,
      attributes,
      children
    }
}
```

## Creating real DOM elements

```js
function createElement(vnode) {
  const node = typeof vnode === 'string' || typeof vnode === 'number'
    ? document.createTextNode(vnode)
    : document.createElement(vnode.nodeName);

  if (vnode.attributes) {
    for (let name in vnode.attributes) {
      if (/^on/.test(name)) {
        node.addEventListener(
          name.slice(2).toLowerCase(), vnode.attributes[name]
        );
      } else {
        if (name === 'className') {
          node.setAttribute('class', vnode.attributes[name]);
        } else {
          node.setAttribute(name, vnode.attributes[name]);
        }  
      }
    }
  }

  vnode.children
    .map(createElement)
    .forEach(node.appendChild.bind(node));

  return node;
}
```
