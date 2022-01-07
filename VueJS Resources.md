# VueJS Resources

## What is Vue?

- Frontend JavaScript Framework for building user interfaces
- Generally used to create single-page app that run on client
- Can also run on server-side by using a SSR framework like Nuxt

## Why use Vue?

- Create dynamic frontend apps
- Easy learning curve
- Easy to integrate with other projects
- Fast and lightweight
- Virtual DOM
- Extremely popular (and rising)
- Great community

## What should you known first?

Like with any framework, you should be comfortable with the underlying language first. In this case, that is JavaScript

- JavaScript Fundamentals
- Async Programming (Promises)
- Array Methods (forEach, map, filter, etc)
- Fetch API/HTTP Requests
- NPM (Node Package Manager)

## Basic Layout of a Vue Component

```html
<template>
  <header>
    <h1>{{ title }}</h1>
  </header>
</template>

<script>
export default {
  props: {
  	title: String,
  },
}

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
```

Component includes a template for markup, logic including state/data/methods as well as the styling for that component.

You can also pass "props" into a component.

## Working with State/Date

Components can have their own state which can determine how a specific component behaves and which data is displayed.

Some state may be "local" to a specific component and some may be "global" or "app" level state that need to be shared with multiple components.

Vuex is a state manager for global state in large applications.

## Options API VS Composition API

Vue 3 has the composition API, which aims to address code reusability and readability in Vue 3, especially in large applications.

## Vue CLI

Standard tooling for Vue development

- Command line interface for creating Vue apps
- Dev server and easy production build
- Optional GUI for managing Vue projects
- Integrated testing, TypeScript support, ESLint and more

***

## VSCode Extensions

- Vetur
