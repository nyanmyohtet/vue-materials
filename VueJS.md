# VueJS

# Introduction

## Declarative Rendering(skiped)

## Handling User Input(skiped)

## Conditionals and Loops(skiped)

## Composing with Components(skiped)

***

<div style="page-break-after: always;"></div>

# Application & Component Instances

## Creating an Application Instance

Every Vue application starts by creating a new application instance with the `createApp` function:

```javascript
const app = Vue.createApp({
  /* options */
})
```

## The Root Component

- The options passed to createApp are used to configure the root component.
- The starting point for rendering when we mount the application.

```javascript
const RootComponent = {
  /* options */
}
const app = Vue.createApp(RootComponent)
const vm = app.mount('#app')
```

## Component Instance Properties

Properties defined in data are exposed via the component instance:

```javascript
const app = Vue.createApp({
  data() {
    return { count: 4 }
  }
})

const vm = app.mount('#app')

console.log(vm.count) // => 4
```

<div style="page-break-after: always;"></div>

## Lifecycle Hooks

- Each component instance goes through a series of initialization steps when it's created
  - set up data observation,
  - compile the template,
  - mount the instance to the DOM, and
  - update the DOM when data changes.
  - also runs functions called `lifecycle hooks`, to add their own code at specific stages.

```javascript
Vue.createApp({
  data() {
    return { count: 1 }
  },
  created() {
    console.log('@created...')
    // `this` points to the vm instance
    console.log('count is: ' + this.count) // => "count is: 1"
  }
})
```

<div style="page-break-after: always;"></div>

## Lifecycle Diagram

<img src="_resources/vue-lifecycle.svg" alt="vue-lifecycle" width="580"/>

<div style="page-break-after: always;"></div>

# Template Syntax

## Interpolations

```html
<span>Message: {{ msg }}</span>
```

## Attributes

```html
<div v-bind:id="dynamicId"></div>

<button v-bind:disabled="isButtonDisabled">Button</button>
```

## Raw HTML (skiped)

## Using JavaScript Expressions

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

## Directives

- Directives are special attributes with the `v-` prefix.
- A directive's job is to reactively apply side effects to the DOM when the value of its expression changes.

```html
<p v-if="seen">Now you see me</p>
```

## Arguments

```html
<!-- the v-bind directive is used to reactively update an HTML attribute: -->
<a v-bind:href="url"> ... </a>

<!-- the v-on directive, which listens to DOM events -->
<a v-on:click="doSomething"> ... </a>
```

<div style="page-break-after: always;"></div>

## Dynamic Arguments (skiped)

## Modifiers

- Modifiers are special postfixes denoted by a dot.

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

## Shorthands

v-bind Shorthand

```html
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>
```

#v-on Shorthand

```html
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>
```

***

<div style="page-break-after: always;"></div>

# Data Properties and Methods

## Data Properties

- The `data` option for a component is a **function**.
- It should return an **object**.
- These instance properties are only added when the instance is **first created**.
- It is possible to add a new property directly to the component instance without including it in data.
- But, it won't automatically be tracked by *Vue's reactivity system*.

```javascript
const app = Vue.createApp({
  data() {
    return { count: 4 }
  }
})

const vm = app.mount('#app')

console.log(vm.$data.count) // => 4
console.log(vm.count)       // => 4

// Assigning a value to vm.count will also update $data.count
vm.count = 5
console.log(vm.$data.count) // => 5

// ... and vice-versa
vm.$data.count = 6
console.log(vm.count) // => 6
```

<div style="page-break-after: always;"></div>

## Methods

- To add methods to a component instance we use the methods option.
- It should be an object containing the desired methods.
- Vue automatically binds the `this` value for methods

```javascript
const app = Vue.createApp({
  data() {
    return { count: 4 }
  },
  methods: {
    increment() {
      // `this` will refer to the component instance
      this.count++
    }
  }
})

const vm = app.mount('#app')

console.log(vm.count) // => 4

vm.increment()

console.log(vm.count) // => 5
```

***

# Computed Properties and Watchers (pending)

## Computed Properties

## Watcher

***

# Class and Style Bindings (pending)

## Binding HTML Classes

## Binding Inline Styles

***

<div style="page-break-after: always;"></div>

# Conditional Rendering

## v-if, v-else, v-else-if

- `v-if` is used to conditionally render a block.
- The block will only be rendered if the directive's expression returns a *truthy* value.

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

## Conditional Groups with v-if on `<template>`

- Serves as an invisible wrapper.
- The final rendered result will not include the `<template>` element.

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

## v-show

- The usage is same as `v-if`.
- The difference is `v-show` only toggles the *display CSS property* of the element.
- v-show doesn't support the `<template>` element, nor does it work with `v-else`.

## v-if with v-for

- Using v-if and v-for together is not recommended.

***

<div style="page-break-after: always;"></div>

# List Rendering

## v-for

- Can use the `v-for` directive to render a list of items based on an array.
- Special syntax in the form of `item in items`, where `items` is the source data array and `item` is an alias for the array element being iterated on.
- Inside `v-for` blocks we have full access to parent scope properties. `v-for` also supports an optional second argument for the `index` of the current item.
- Can also use `of` as the delimiter instead of `in`.
- It is recommended to provide a `key` attribute with `v-for` whenever possible

```html
<ul id="array-with-index">
  <li v-for="(item, index) in items" :key="item.id">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

```javascript
Vue.createApp({
  data() {
    return {
      parentMessage: 'Parent',
      items: [{ message: 'Foo' }, { message: 'Bar' }]
    }
  }
}).mount('#array-with-index')
```

## v-for on a `<template>`(skiped)

## v-for with v-if(skiped, duplicate content)

***

<div style="page-break-after: always;"></div>

# Event Handling

## Listening to Events

- Can use the `v-on` directive, which shorten to the `@` symbol.
- Listen to DOM events and run some JavaScript when they're triggered.

Benefits:

- easier to locate the handler function implementations within JS code by skimming the HTML template.
- Don't have to manually attach event listeners in JS.
- When a ViewModel is destroyed, all event listeners are automatically removed. Don't need to worry about cleaning it up.


```html
<div id="event-with-method">
  <!-- `greet` is the name of a method defined below -->
  <button @click="greet">Greet</button>
</div>
```

```javascript
Vue.createApp({
  data() {
    return {
      name: 'Vue.js'
    }
  },
  methods: {
    greet(event) {
      // `this` inside methods points to the current active instance
      alert('Hello ' + this.name + '!')
      // `event` is the native DOM event
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
}).mount('#event-with-method')
```

## Event Modifiers

- It would be better if the methods can be purely about data logic rather than having to deal with DOM event details.
- Vue provides event modifiers for v-on.
  - .stop
  - .prevent, etc.


***

# Form Input Bindings

## Basic Usage

## Value Bindings

## Modifiers

***

# Components Basics

## v-for with a component
