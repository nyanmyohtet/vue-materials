# Components Basics

Components are reusable instances with a name: in this case, `<button-counter>`.

Example of a Vue component:

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

```javascript
// Create a Vue application
const app = Vue.createApp({})

// Define a new global component called button-counter
app.component('button-counter', {
  data() {
    return {
      count: 0
    }
  },
  template: `
    <button v-on:click="count++">
      You clicked me {{ count }} times.
    </button>`
})
app.mount('#components-demo')
```
<div style="page-break-after: always;"></div>

# Reusing Components

Components can be reused as many times as you want.

Each one maintains its own **state/date**.

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

<div style="page-break-after: always;"></div>

# Organizing Components

It's common for an app to be organized into a tree of nested components:

<img src="_resources/organizing-vue-components.png" alt="organizing-vue-components.png" width="800"/>

- To use these components in templates, they must be **registered** so that Vue knows about them.

<div style="page-break-after: always;"></div>

# Passing Data to Child Components with Props

- `Props` are custom attributes can register on a component.
- A value is passed to a `prop` attribute, becomes a `property` on that component instance.

```html
<div id="blog-post-demo" class="demo">
  <blog-post title="My journey with Vue"></blog-post>
  <blog-post title="Blogging with Vue"></blog-post>
  <blog-post title="Why Vue is so fun"></blog-post>
</div>
```

```javascript
const app = Vue.createApp({})

app.component('blog-post', {
  props: ['title'],
  template: `<h4>{{ title }}</h4>`
})

app.mount('#blog-post-demo')
```

<div style="page-break-after: always;"></div>

# Component anatomy

```javascript
Vue.component('my-component', {
  components: { // Components that can be used in the template
    ProductComponent,
    ReviewComponent
  },
  props: { // The parameters the component accepts
    message: String,
    product: Object,
    email: {
      type: String,
      required: true,
      default: "none",
      validator (value) { // Should return true if value is valid }
    }
  },
  data() { // `data` must be a function and return object
    return {
      firstName: 'Vue',
      lastName: 'JS'
    }
  },
  computed: { // Return cached values until dependencies change
    fullName() { return this.firstName + ' ' + this.lastName }
  },
  watch: {
    firstName(value, oldValue) { ... } // Called when firstName changes value
  },
  methods: { ... },
  template: '<span>{{ message }}</span>', // Can also use backticks in `template` for multi-line
})
```

<div style="page-break-after: always;"></div>

# Single File Components(*.vue)

## Single File

```html
<template>
  <p>{{ greeting }} World!</p>
</template>

<script>
export default {
  data() {
    return {
      greeting: 'Hello'
    }
  }
}
</script>

<style scoped>
p {
  font-size: 2em;
  text-align: center;
}
</style>
```

<div style="page-break-after: always;"></div>

## Separation

```html
<template>
  <div>Hello World!</div>
</template>
<script src="./my-component.js"></script>
<style src="./my-component.css" scoped></style>
```

<div style="page-break-after: always;"></div>

# Lifecycle hooks

## beforeCreate

- After the instance has been **initialized**, before data observation and event/watcher setup.

## created

- Called after the instance is **created**.
- The instance has finished processing the **options** which means the following have been set up:
  - data observation,
  - computed properties,
  - methods,
  - watch/event callbacks.

## beforeMount

- Called before the mounting begins.

## mounted

- Called after the instance has been mounted, where element, passed to `app.mount` is replaced by the newly created `vm.$el`.
> Note that mounted does not guarantee that all child components have also been mounted.

```javascript
mounted() {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been rendered
  })
}
```

## beforeUpdate

- Called when **data changes**, before the DOM is patched.
- A good place to access the existing DOM before an update.

## updated

- Called after a data change causes the virtual DOM to be **re-rendered** and **patched**.
- The component's DOM will have been updated.
> Note that updated does not guarantee that all child components have also been re-rendered.

```javascript
updated() {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been re-rendered
  })
}
```

## beforeUnmount

- Called before a component instance is unmounted.
- At this stage the instance is still fully functional.

## unmounted

- Called after a component instance has been unmounted.
- All **directives** of the component instance have been unbound,
- All **event listeners** have been removed,
- And all **child component instances** have also been unmounted.

<div style="page-break-after: always;"></div>

## Lifecycle Diagram

<img src="_resources/vue-lifecycle.svg" alt="vue-lifecycle" width="350"/>

<div style="page-break-after: always;"></div>

Sample Code :

```html
<template>
  <button @click="count++">Clicked {{count}} Count</button>
</template>

<script>
export default {
  name: 'App',
  data() {
    return { count: 0 }
  },
  beforeCreate() {
    console.log('this.count: ', this.count) // count: undefined
  },
  created() {
    console.log('this.count: ', this.count) // count: 0
  },
  beforeMount() {
    console.log(this.$el) // null - At this point, this.$el has not been created yet.
  },
  mounted() {
    console.log(this.$el.textContent) // Clicked 0 Count
  },
  beforeUpdate() {
    console.log(this.$el.textContent) // Clicked 0 Count
    console.log('this.count: ', this.count) // 1
  },
  updated() {
    console.log(this.$el.textContent) // Clicked 1 Count
  },
}
</script>
```

<div style="page-break-after: always;"></div>

# Custom events

Set listener on component, within its parent

```html
<button-counter v-on:increment-by="incWithVal">
```

Inside parent component

```javascript
methods: {
  incWithVal(toAdd) { ... }
}
```

Inside button-counter template

```javascript
this.$emit(
    'increment-by', // Custom event name
    5 // Data sent up to parent
  )
```

- Use `props` to pass data into `child components`,
- Use `custom events` to pass data to `parent elements`.

<div style="page-break-after: always;"></div>
