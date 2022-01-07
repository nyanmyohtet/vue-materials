# VueJS

# Introduction

## Declarative Rendering

https://v3.vuejs.org/guide/introduction.html#declarative-rendering

## Handling User Input

## Conditionals and Loops

## Composing with Components

***

# Application & Component Instances

## Creating an Application Instance

Every Vue application starts by creating a new application instance with the createApp function:

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

## Lifecycle Hooks

- Each component instance goes through a series of initialization steps when it's created
  - set up data observation,
  - compile the template,
  - mount the instance to the DOM, and
  - update the DOM when data changes.
  - also runs functions called lifecycle hooks, to add their own code at specific stages.

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

## Lifecycle Diagram

![vue-lifecycle.svg](_resources/vue-lifecycle.svg)

***

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

- Directives are special attributes with the v- prefix.
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

# Data Properties and Methods

## Data Properties

- The data option for a component is a function.
- It should return an object.
- These instance properties are only added when the instance is first created.
- It is possible to add a new property directly to the component instance without including it in data.
- But, it won't automatically be tracked by Vue's reactivity system.

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

## Methods

- To add methods to a component instance we use the methods option.
- It should be an object containing the desired methods.
- Vue automatically binds the this value for methods

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

# Computed Properties and Watchers

## Computed Properties

## Watcher

***

# Class and Style Bindings

## Binding HTML Classes

## Binding Inline Styles

***

# Conditional Rendering

...

***

# List Rendering

## v-for

## v-for on a <template>

## v-for with v-if

***

# Event Handling

...

***

# Form Input Bindings

## Basic Usage

## Value Bindings

## Modifiers

***

# Components Basics

## v-for with a component
