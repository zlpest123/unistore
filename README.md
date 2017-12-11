<p align="center">
  <img src="https://i.imgur.com/o0u6dto.png" width="300" height="300" alt="unistore">
  <br>
  <a href="https://www.npmjs.org/package/unistore"><img src="https://img.shields.io/npm/v/unistore.svg?style=flat" alt="npm"></a> <a href="https://travis-ci.org/developit/unistore"><img src="https://travis-ci.org/developit/unistore.svg?branch=master" alt="travis"></a>
</p>

# unistore

> A tiny ~650b centralized state container with component bindings for [Preact] & [React].

- **Small** footprint compliments Preact nicely
- **Familiar** names and ideas from Redux-like libraries
- **Useful** data selectors to extract properties from state
- **Portable** actions can be moved into a common place and imported
- **Functional** actions are just reducers

## Table of Contents

- [Install](#install)
- [Usage](#usage)
- [Examples](#examples)
- [API](#api)
- [License](#license)

## Install

This project uses [node](http://nodejs.org) and [npm](https://npmjs.com). Go check them out if you don't have them locally installed.

```sh
npm install --save unistore
```

Then with a module bundler like [webpack](https://webpack.js.org) or [rollup](http://rollupjs.org), use as you would anything else:

```js
// The store:
import createStore from 'unistore'

// Preact integration
import { Provider, connect } from 'unistore/preact'

// React integration
import { Provider, connect } from 'unistore/react'
```

Alternatively, you can import the "full" build for each, which includes both `createStore` and the integration for your library of choice:

```js
import { createStore, Provider, connect } from 'unistore/full/preact'
```

The [UMD](https://github.com/umdjs/umd) build is also available on [unpkg](https://unpkg.com):

```html
<!-- just unistore(): -->
<script src="//unpkg.com/unistore/dist/unistore.umd.js"></script>
<!-- for preact -->
<script src="//unpkg.com/unistore/full/preact.umd.js"></script>
<!-- for react -->
<script src="//unpkg.com/unistore/full/react.umd.js"></script>
```

You can find the library on `window.unistore`.

### Usage

```js
import createStore from 'unistore'
import { Provider, connect } from 'unistore/preact'

let store = createStore({ count: 0 })

// If actions is a function, it gets passed the store:
let actions = store => ({
  // Actions can just return a state update:
  increment(state) {
    return { count: state.count+1 }
  },

  // The above example as an Arrow Function:
  increment2: ({ count }) => ({ count: count+1 }),

  //Actions receive current state as first parameter and any other params next
  //check this function as <button onClick={incrementAndLog}>
  incrementAndLog: ({ count }, event) => {
    console.info(event)
    return { count: count+1 }
  },

  // Async actions can be pure async/promise functions:
  async getStuff(state) {
    let res = await fetch('/foo.json')
    return { stuff: await res.json() }
  },

  // ... or just actions that call store.setState() later:
  incrementAsync(state) {
    setTimeout( () => {
      store.setState({ count: state.count+1 })
    }, 100)
  }
})

const App = connect('count', actions)(
  ({ count, increment }) => (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  )
)

export default () => (
  <Provider store={store}>
    <App />
  </Provider>
)
```

### Debug

Make sure to have [Redux devtools extension](https://github.com/zalmoxisus/redux-devtools-extension) previously installed.

```js
import { createStore } from 'unistore'
import devtools from 'unistore/devtools'

let initialState = { count: 0 };
let store = process.env.NODE_ENV === 'production' ?  createStore(initialState) : devtools(createStore(initialState));

// ...
```

### Examples

[README Example on CodeSandbox](https://codesandbox.io/s/l7y7w5qkz9)

### API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Reporting Issues

Found a problem? Want a new feature? First of all see if your issue or idea has [already been reported](../../issues).
If don't, just open a [new clear and descriptive issue](../../issues/new).

### License

[MIT License](LICENSE.md) © [Jason Miller](https://jasonformat.com/)

[preact]: https://github.com/developit/preact

[react]: https://github.com/facebook/react
