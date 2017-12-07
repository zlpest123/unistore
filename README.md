<p align="center">
  <img src="https://i.imgur.com/o0u6dto.png" width="300" height="300" alt="unistore">
  <br>
  <a href="https://www.npmjs.org/package/unistore"><img src="https://img.shields.io/npm/v/unistore.svg?style=flat" alt="npm"></a> <a href="https://travis-ci.org/developit/unistore"><img src="https://travis-ci.org/developit/unistore.svg?branch=master" alt="travis"></a>
</p>

# unistore

> A tiny 650b centralized state container with component bindings for [Preact].

-   **Small** footprint compliments Preact nicely
-   **Familiar** names and ideas from Redux-like libraries
-   **Useful** data selectors to extract properties from state
-   **Portable** actions can be moved into a common place and imported
-   **Functional** actions are just reducers

## Table of Contents

-   [Install](#install)
-   [Usage](#usage)
-   [Examples](#examples)
-   [API](#api)
-   [License](#license)

## Install

This project uses [node](http://nodejs.org) and [npm](https://npmjs.com). Go check them out if you don't have them locally installed.

```sh
npm install --save unistore
```

Then with a module bundler like [webpack](https://webpack.js.org) or [rollup](http://rollupjs.org), use as you would anything else:

```javascript
// using ES6 modules
import { createStore, Provider, connect } from 'unistore'

// using CommonJS modules
const unistore = require('unistore')
```

The [UMD](https://github.com/umdjs/umd) build is also available on [unpkg](https://unpkg.com):

```html
<script src="//unpkg.com/unistore/dist/unistore.umd.js"></script>
```

You can find the library on `window.unistore`.

### Usage

```js
import { Provider, createStore, connect } from 'unistore'

let store = createStore({ count: 0 })

// If actions is a function, it gets passed the store:
let actions = store => ({
	// Actions can just return a state update:
	increment(state) {
		return { count: state.count+1 }
	},

	// The above example as an Arrow Function:
	increment2: ({ count }) => ({ count: count+1 }),

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

### Examples

[README Example on CodeSandbox](https://codesandbox.io/s/l7y7w5qkz9)

### API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### createStore

Creates a new store, which is a tiny evented state container.

**Parameters**

-   `state` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Optional initial state (optional, default `{}`)

**Examples**

```javascript
let store = createStore();
   store.subscribe( state => console.log(state) );
   store.setState({ a: 'b' });   // logs { a: 'b' }
   store.setState({ c: 'd' });   // logs { c: 'd' }
```

Returns **[store](#store)** 

#### store

An observable state container, returned from [createStore](#createstore)

##### setState

Apply a partial state object to the current state, invoking registered listeners.

**Parameters**

-   `update` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** An object with properties to be merged into state
-   `overwrite` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** If `true`, update will replace state instead of being merged into it (optional, default `false`)

##### subscribe

Register a listener function to be called whenever state is changed.

**Parameters**

-   `listener` **[Function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function)** 

##### unsubscribe

Remove a previously-registered listener function.

**Parameters**

-   `listener` **[Function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function)** 

##### getState

Retreive the current state object.

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** state

#### connect

Wire a component up to the store. Passes state as props, re-renders on change.

**Parameters**

-   `mapStateToProps` **([Function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function) \| [Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array) \| [String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** A function mapping of store state to prop values, or an array/CSV of properties to map.
-   `actions` **([Function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function) \| [Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object))?** Action functions (pure state mappings), or a factory returning them.

**Examples**

```javascript
const Foo = connect('foo,bar')( ({ foo, bar }) => <div /> )
```

Returns **Component** ConnectedComponent

#### Provider

**Extends Component**

Provider exposes a store (passed as `props.store`) into context.
 Generally, an entire application is wrapped in a single `<Provider>` at the root.

**Parameters**

-   `props` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `props.store` **Store** A {Store} instance to expose via context.

### Reporting Issues

Found a problem? Want a new feature? First of all see if your issue or idea has [already been reported](../../issues).
If don't, just open a [new clear and descriptive issue](../../issues/new).

### License

[MIT License](LICENSE.md) © [Jason Miller](https://jasonformat.com/)

[preact]: https://github.com/developit/preact
