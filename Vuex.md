## What is Vuex?

- Vuex is a **state management pattern + library** for Vue.js applications.
- It serves as a **centralized store** for all the components in an application.
- The data `store` in Vuex follows the same rules as the `data` in a Vue instance.
- A `store` is basically a container that holds application **state**.

## When Should I Use It?
- Passing props can be tedious for **deeply nested components**, and simply doesn't work for sibling components.
- Difficult to reach for direct parent/child instance references or to **mutate and synchronize multiple copies of the state via events**.
- Extract the shared state out of the components, and manage it in a **global** singleton.
- With this, component tree becomes a big **view**.
- And any component can **access the state** or **trigger actions**, no matter where they are in the tree!

<div style="page-break-after: always;"></div>

## Setup `Vuex` in Project


```
npm install vuex@next --save
or
yarn add vuex@next --save
```

<div style="page-break-after: always;"></div>

## Setup `vuex` in `main.js`

```javascript
// main.js
import { createApp } from 'vue'
import store from './store' // import `store/index.js` file
import App from './App.vue'

createApp(App)
  .use(store) // use vuex store
  .mount('#app')
```

<div style="page-break-after: always;"></div>

## Create New `store/index.js`.

Then, setup vuex store inside `store/index.js` file.

```javascript
// store/index.js
import { createStore } from 'vuex'

export default createStore({
  state: {
    showForm: true,
    todos: [],
  },
  getters: {
    todoCount(state) { // calculate the count of todos and return it
      return state.todos.length
    }
  },
  mutations: {
    toggleForm(state) { // mutate/change the state of store
      state.showForm = !state.showForm
    }
  },
  actions: {
  },
  modules: {
  }
})
```

<div style="page-break-after: always;"></div>

## Inject `this.$store` into VueJS App

Vuex has a mechanism to **inject** the store into all child components from the root component with the store option(enabled by `Vue.use(Vuex)`):

```javascript
// App Root Component
const app = new Vue({
  // provide the store using the "store" option.
  // this will inject the store instance to all child components.
  store, // shorthand syntax of `store: store`
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
}).mount('#app')
```

By providing the `store` option to the root instance, the `store` will be injected into all child components of the root and will be available on them as `this.$store`.

```javascript
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

<div style="page-break-after: always;"></div>

Now we can **commit** a mutation from component's method:

```javascript
...
methods: {
  increment() {
    this.$store.commit('increment')
    console.log(this.$store.state.count) // 1
  }
}
...
```

<div style="page-break-after: always;"></div>

## Getting Vuex `State` into Vue Components

Vuex stores are **reactive**, the simplest way to r**etrieve state** from it is simply returning some store state from within a **computed property**.

Whenever `this.$store.state.count` changes, it will cause the computed property to **re-evaluate**, and **trigger** associated DOM updates.

```javascript
// let's create a Counter component
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

<div style="page-break-after: always;"></div>

## The `mapState` Helper

- Declaring all these computed properties can get repetitive and verbose.
- We can make use of the `mapState` helper which generates computed getter functions.

```javascript
  computed: {
    // map this.count to store.state.count
    ...mapState(['count'])
  }
```

- The `mapState` returns an object - to use it in combination with other local computed properties, we'd have to use `object spread operator` (`...{...}`) to merge multiple objects into one.

```javascript
computed: {
  localComputed() { /* ... */ },
  // mix this into the outer object with the object spread operator
  ...mapState({
    // ...
  })
}
```

<div style="page-break-after: always;"></div>

## Getters

- Functions used to **return values from the store**. Use these when **comput­ations need to be performed on the state value** before they are passed to the caller.
- Vuex allows us to define `getters` in the store.
- Like computed properties, a `getter`'s result is cached based on its dependencies, and will only re-evaluate when some of its dependencies have changed.

Getters will receive the state as their 1st argument:

```javascript
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

<div style="page-break-after: always;"></div>

## The `mapGetters` Helper

The `mapGetters` helper simply maps store getters to local computed properties:

```javascript
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
    // mix the getters into computed with object spread operator
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```

<div style="page-break-after: always;"></div>

## Mutations

Functions that **commit** changes to the state, and can process the values being passed before saving them.

- The only way to change `state` in a Vuex store is by committing a **mutation**.
- Vuex mutations are very similar to events: each mutation has a **string type** and a **handler**.
- The handler function is where we perform actual `state` modifications:

```javascript
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    // In most cases, the `payload` should be an `object` so that it can contain multiple fields,
    increment(state, payload) {
      state.count += payload
    }
  }
})
```

- To invoke a mutation handler, you need to call `store.commit` with its type:

```javascript
store.commit('increment', 10)
```

<div style="page-break-after: always;"></div>

## Committing Mutations in Components

Can commit mutations in components with `this.$store.commit('xxx')`, or use the `mapMutations` helper which maps component methods to store.commit calls:

```javascript
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // map `this.increment()` to `this.$store.commit('increment')`
      // `mapMutations` also supports payloads:
      'incrementBy' // map `this.incrementBy(amount)` to `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // map `this.add()` to `this.$store.commit('increment')`
    })
  }
}
```

<div style="page-break-after: always;"></div>

## Actions

`Actions` are similar to mutations, the differences being that:

- Instead of **mutating** the state, actions **commit** mutations.
- Actions can contain **arbitrary asynchronous operations** (like API calls).
- Action handlers receive a `context` object which exposes the same set of methods/properties on the store instance.
- So you can call `context.commit` to commit a mutation, or access the state and getters via `context.state` and `context.getters`.

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      // do some async tasks
      context.commit('increment')
    }
  }
})
```

<div style="page-break-after: always;"></div>

In practice, we often use ES2015 argument destructuring (opens new window)to simplify the code

```javascript
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

<div style="page-break-after: always;"></div>

## Dispatching Actions

Actions are triggered with the `store.dispatch` method:

```javascript
store.dispatch('increment')
```

Actions support the **payload format** and **object-style dispatch**:

```javascript
// dispatch with a payload
store.dispatch('incrementAsync', {
  amount: 10
})

// dispatch with an object
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

<div style="page-break-after: always;"></div>

## Dispatching Actions in Components

You can dispatch actions in components with `this.$store.dispatch('xxx')`, or use the `mapActions` helper which maps component methods to `store.dispatch` calls (requires `root` store injection):

```javascript
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // map `this.increment()` to `this.$store.dispatch('increment')`
      // `mapActions` also supports payloads:
      'incrementBy' // map `this.incrementBy(amount)` to `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // map `this.add()` to `this.$store.dispatch('increment')`
    })
  }
}
```

<div style="page-break-after: always;"></div>

## Vuex Diagram

<image src="_resources/vuex-lifecycle.png" alt="vuex-lifecycle">

<div style="page-break-after: always;"></div>
