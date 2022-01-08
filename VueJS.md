# VueJS

# Application & Component Instances

## Creating an Application Instance

Every Vue application starts by creating a new application instance with the `createApp` function:

```javascript
const app = Vue.createApp({
  /* options */
})
```

<div style="page-break-after: always;"></div>

## The Root Component

- The `options` passed to createApp are used to configure the root component.
- The starting point for rendering when we mount the application.

```javascript
const RootComponent = {
  /* options */
}
const app = Vue.createApp(RootComponent)
app.mount('#app')
```

<div style="page-break-after: always;"></div>

## Component Instance Properties

Properties defined in `data` are exposed via the component instance:

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
  - **set up** data observation,
  - **compile** the template,
  - **mount** the instance to the DOM, and
  - **update** the DOM when data changes.
  - also **runs** functions called `lifecycle hooks`, to add their own code at specific stages.

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

<img src="_resources/vue-lifecycle.svg" alt="vue-lifecycle" width="550"/>

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

## Using JavaScript Expressions

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

<div style="page-break-after: always;"></div>

## Directives

- Directives are special attributes with the `v-` prefix.
- A directive's job is to **reactively** apply side effects to the DOM when the value of its expression changes.

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

## Shorthands

v-bind Shorthand

```html
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>
```

## v-on Shorthand

```html
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>
```

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

- To add methods to a component instance we use the *methods* option.
- It should be an **object** containing the desired methods.
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

<div style="page-break-after: always;"></div>

# Conditional Rendering

## v-if, v-else, v-else-if

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

- Serves as an *invisible wrapper*.
- The final rendered result will not include the `<template>` element.

```html
<template v-if="true">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

## v-show

- The usage is same as `v-if`.
- But, `v-show` only toggles the *display CSS property* of the element.
- `v-show` doesn't support the `<template>` element, nor does it work with `v-else`.

<div style="page-break-after: always;"></div>

# List Rendering

## v-for

- Use the `v-for` directive to render a list of items.
- In the form of `item in items`,
- Where `items` is the source data array and `item` is an alias for the array element.
- Have full access to parent scope properties inside `v-for` blocks .
- `v-for` also supports an optional second argument for the `index` of the current item.
- Can also use `of` as the delimiter instead of `in`.
- It is recommended to provide a `key` attribute with `v-for` whenever possible.

```html
<ul id="array-with-index">
  <li v-for="(item, index) in items" :key="item.id">
    {{ index }} - {{ item.message }}
  </li>
</ul>
```

```javascript
Vue.createApp({
  data() {
    return {
      items: [{ id: 1, message: 'Foo' }, { id: 2, message: 'Bar' }]
    }
  }
}).mount('#array-with-index')
```

<div style="page-break-after: always;"></div>

## v-if with v-for

- When `v-if` and `v-for` are both used on the same element, `v-if` will be evaluated first.
- *** Using `v-if` and `v-for` together is not recommended.

<div style="page-break-after: always;"></div>

# Event Handling

## Listening to Events

- Use `v-on` directive, which shorten to the `@` symbol.
- Listen to DOM events and run some JavaScript when they're triggered.

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
    }
  }
}).mount('#event-with-method')
```

<div style="page-break-after: always;"></div>

Benefits:

- Easier to locate the handler function implementations within JS code by skimming the HTML template.
- Don't have to manually attach event listeners in JS.
- When a ViewModel is destroyed, all event listeners are automatically removed. Don't need to worry about cleaning it up.

## Event Modifiers

- The methods can be purely about data logic rather than having to deal with DOM event details.
- Vue provides event modifiers for `v-on`.
  - .stop
  - .prevent, etc.
- Modifiers are special postfixes denoted by a dot.

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

<div style="page-break-after: always;"></div>

# Form Input Bindings

Basic Usage:

- Use the `v-model` directive to create **two-way data bindings** on form elements.
- `v-model` is essentially syntax sugar for updating *data* on user *input* events.

`v-model` internally uses different properties and emits different events for different input elements:

Text:
- *text* and *textarea* elements use **value** property and input event.

```html
<div id="v-model-basic">
  <input v-model="message" placeholder="edit me" />
  <p>Message is: {{ message }}</p>
</div>
```

```javascript
Vue.createApp({
  data() {
    return {
      message: ''
    }
  }
}).mount('#v-model-basic')
```

<div style="page-break-after: always;"></div>

Checkbox:

- *checkboxes* and *radiobuttons* use **checked** property and change event

```html
<div id="v-model-checkbox" class="demo">
  <input type="checkbox" id="checkbox" v-model="checked" />
  <label for="checkbox">{{ checked }}</label>
</div>
```

```javascript
Vue.createApp({
  data() {
    return {
      checked: false
    }
  }
}).mount('#v-model-checkbox')
```

<div style="page-break-after: always;"></div>

Radio:

```html
<div id="v-model-radiobutton">
  <input type="radio" id="one" value="One" v-model="picked" />
  <label for="one">One</label>
  <br />
  <input type="radio" id="two" value="Two" v-model="picked" />
  <label for="two">Two</label>
  <br />
  <span>Picked: {{ picked }}</span>
</div>
```

```javascript
Vue.createApp({
  data() {
    return {
      picked: ''
    }
  }
}).mount('#v-model-radiobutton')
```

<div style="page-break-after: always;"></div>

Select:

- *select fields* use value as a **prop** and change as an event.

```html
<div id="v-model-select">
  <select v-model="selected">
    <option disabled value="">Please select one</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```

```javascript
Vue.createApp({
  data() {
    return {
      selected: ''
    }
  }
}).mount('#v-model-select')
```
