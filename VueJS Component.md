# Components Basics

- Accept the same options as a root instance, such as `data`, `computed`, `watch`, `methods`, and `lifecycle hooks`.

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

Components can be reused as many times as you want:

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

- Each one maintains its own, separate count.
- Whenever reuse a component, a new instance of it is created.

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

In a typical app:

```html
<div id="blog-posts-demo">
  <blog-post
    v-for="post in posts"
    :key="post.id"
    :title="post.title"
  ></blog-post>
</div>
```

```javascript
const App = {
  data() {
    return {
      posts: [
        { id: 1, title: 'My journey with Vue' },
        { id: 2, title: 'Blogging with Vue' },
        { id: 3, title: 'Why Vue is so fun' }
      ]
    }
  }
}
const app = Vue.createApp(App)

app.component('blog-post', {
  props: ['title'],
  template: `<h4>{{ title }}</h4>`
})
app.mount('#blog-posts-demo')
```

- use `v-bind` to dynamically pass props.

<div style="page-break-after: always;"></div>

# Component anatomy

```javascript
Vue.component('my-component', {
  components: {
    // Components that can be used in the template
    ProductComponent,
    ReviewComponent
  },
  props: {
    // The parameters the component accepts
    message: String,
    product: Object,
    email: {
      type: String,
      required: true,
      default: "none"
      validator: function (value) {
        // Should return true if value is valid
      }
    }
  },
  data() {
    // `data` must be a function and return object
    return {
      firstName: 'Vue',
      lastName: 'Mastery'
    }
  },
  computed: {
    // Return cached values until dependencies change
    fullName() {
      return this.firstName + ' ' + this.lastName
    }
  },
  watch: {
    // Called when firstName changes value
    firstName(value, oldValue) { ... }
  },
  methods: { ... },
  template: '<span>{{ message }}</span>',
  // Can also use backticks in `template` for multi-line
})
```

<div style="page-break-after: always;"></div>

# Lifecycle hooks

- `beforeCreate`:	After the instance has been initialized.
- `created`:	After the instance is created.
- `beforeMount`:	Before the first render.
- `mounted`:	After the instance has been mounted.
- `beforeUpdate`:	When data changes, before the DOM is patched.
- `updated`:	After a data change.
- `beforeUnmount`:	Before the instance is destroyed.
- `unmounted`:	After a Vue instance has been destroyed.

<div style="page-break-after: always;"></div>

# Custom events

Set listener on component, within its parent

```html
<button-counter v-on:incrementBy="incWithVal">
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
    'incrementBy', // Custom event name
    5 // Data sent up to parent
  )
```

Use `props` to pass data into `child components`, `custom events` to pass data to `parent elements`.

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
  <div>This will be pre-compiled</div>
</template>
<script src="./my-component.js"></script>
<style src="./my-component.css" scoped></style>
```

<div style="page-break-after: always;"></div>
