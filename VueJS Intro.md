# What is Vue?

- Frontend JavaScript Framework for building **User Interfaces**.
- Generally used to create **Single Page App** that run on client.
- Can also run on server-side by using a SSR framework like [Nuxt](https://nuxtjs.orgs).

<div style="page-break-after: always;"></div>

# Why use Vue?

- Create dynamic frontend apps
- Easy learning curve
- Easy to integrate with other projects
- Fast and lightweight
- Virtual DOM
- Extremely popular (and rising)
- Great community

<div style="page-break-after: always;"></div>

# What should you known first?

- JavaScript Fundamentals
- Async Programming (Promises)
- Array Methods (forEach, map, filter, etc)
- Fetch API/HTTP Requests
- NPM (Node Package Manager)

<div style="page-break-after: always;"></div>

# Basic Layout of a Vue Component

```html
<template>
  <header>
    <h1>{{ title }}</h1>
  </header>
</template>

<script>
export default {
  props: { title },
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
```

Component includes a **template** for markup, **logic** including state/data/methods as well as the **styling** for that component.

Components can have their own **state** which can determine how a specific component behaves and which data is displayed.

Some state may be `local` to a specific component and some may be `global` state that need to be shared with multiple components.

**Vuex** is a state manager for `global` state in large applications.

<div style="page-break-after: always;"></div>

# Vue CLI

Standard tooling for Vue development

- Command line interface for creating Vue apps
- Dev server and easy production build
- Optional GUI for managing Vue projects
- Integrated testing, TypeScript support, ESLint and more

<div style="page-break-after: always;"></div>

# VSCode Extensions

- [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur)
