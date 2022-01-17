# Vue Router

## Installation

For Vue 2.x:

```
npm install vue-router
or
yarn add vue-router
```

For Vue 3.x:

```
npm install vue-router@4
or
yarn add vue-router@4
```

<div style="page-break-after: always;"></div>

Setup `vue-router` in `main.js`:

```javascript
// main.js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'  // import `router/index.js` file

createApp(App)
  .use(router) // get access to vue-router as `this.$router` as well as the current route as `this.$route` inside of any component
  ...
  .mount('#app')
```

<div style="page-break-after: always;"></div>

Create New `router/index.js`.

Then, setup router and declare routes.

```javascript
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue' // import page component
import About from '../views/About.vue' // import page component

const routes = [
  {
    path: "/",
    name: "home",
    component: Home,
  },
  {
    path: "/about",
    name: "about",
    component: About,
  },
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

<div style="page-break-after: always;"></div>

## A Basic Example

HTML
```html
// App.vue
<template>
  <h1>Vue Router</h1>
  <p>
    <!-- use the router-link component for navigation. -->
    <!-- specify the link by passing the `to` prop. -->
    <!-- `<router-link>` will render an `<a>` tag with the correct `href` attribute -->
    <router-link to="/">Go to Home</router-link>
    <router-link to="/about">Go to About</router-link>
  </p>
  <!-- route outlet -->
  <!-- component matched by the route will render here -->
  <router-view></router-view>
</template>
```

## `router-link`

- Use a custom component `router-link` to create links instead of HTML `<a>` tag.
- `router-link` allows Vue Router to change the URL without reloading the page.

## `router-view`

- `router-view` will display the component that corresponds to the url.

<div style="page-break-after: always;"></div>

## Dynamic Route Matching with Params

```javascript
const routes = [
  // dynamic segments start with a colon (:)
  { path: '/users/:id', component: User },
]
```

Now URLs like `/users/1` and `/users/2` will both map to the same route.

We can render the current user ID by updating User's template to this:

```html
<div>User {{ $route.params.id }}</div>
```

<div style="page-break-after: always;"></div>

## Nested Routes

If components that are nested multiple levels deep. In this case, it is very common that the segments of a URL corresponds to a certain structure of nested components, for example:

```
/user/1/profile                        /user/2/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile       | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

<div style="page-break-after: always;"></div>

In `User.vue`:

```html
<template>
  <div class="user">
    <h2>User {{ $route.params.id }}</h2>
    <router-view></router-view>
  </div>
</template>
```

In `route/index.js`:

```javascript
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      {
        // UserProfile will be rendered inside User's <router-view>
        // when /user/:id/profile is matched
        path: 'profile',
        component: UserProfile,
      },
      {
        // UserPosts will be rendered inside User's <router-view>
        // when /user/:id/posts is matched
        path: 'posts',
        component: UserPosts,
      },
    ],
  },
]
```

<div style="page-break-after: always;"></div>

## Programmatic Navigation

The argument can be a string path, or a location descriptor object. Examples:

```javascript
// literal string path
router.push('/users/eduardo')

// object with path
router.push({ path: '/users/eduardo' })

// named route with params to let the router build the url
router.push({ name: 'user', params: { username: 'eduardo' } })

// with query, resulting in /register?plan=private
router.push({ path: '/register', query: { plan: 'private' } })
```

<div style="page-break-after: always;"></div>

## Redirect

To redirect from `/home` to `/`:

```javascript
// router/index.js
// redirect to url
const routes = [{ path: '/home', redirect: '/' }]

// redirect to a named route
const routes = [{ path: '/home', redirect: { name: 'homepage' } }]
```

<div style="page-break-after: always;"></div>

## Passing `Props` to Route Components

```javascript
const User = {
  // make sure to add a prop named exactly like the route param
  props: ['id'], // can access as `this.id`, instead of `this.$route.params.id
  template: '<div>User {{ id }}</div>'
}
const routes = [{ path: '/user/:id', component: User, props: true }]
```

<div style="page-break-after: always;"></div>

## Navigation Guards

### Global Before Guards

You can register global before guards using `router.beforeEach`:

Every guard function receives two arguments:

- `to`: the target route location being navigated to.
 `from`: the current route location being navigated away from.

```javascript
// router/index.js
router.beforeEach((to, from) => {
  const loggedIn = store.getters.isLoggedIn
  if (!loggedIn && to.name != "login") {
    return '/login'
  }
});
```

<div style="page-break-after: always;"></div>

### Per-Route Guard

You can define `beforeEnter` guards directly on a route's configuration object:

```javascript
// router/index.js
const routes = [
  {
    path: '/users/:id',
    component: UserDetails,
    beforeEnter: (to, from) => {
      // reject the navigation
      return false
    },
  },
]
```

<div style="page-break-after: always;"></div>

## In-Component Guards

You can also directly define route navigation guards inside components.

You can add the following options to route components:

- beforeRouteEnter
  - called before the route that renders this component is confirmed.
  - does NOT have access to `this` component instance,
  - because it has not been created yet when this guard is called!

- beforeRouteUpdate
  - called when the route that renders this component has changed, but this component is reused in the new route.
  - For example, given a route with params `/users/:id`,
  - when we avigate between `/users/1` and `/users/2`,
  - the same `UserDetails` component instance will be reused, and this hook will be called when that happens.

- beforeRouteLeave
  - called when the route that renders this component is about to be navigated away from.
  - it has access to `this` component instance.

<div style="page-break-after: always;"></div>

```javascript
// UserDetail.vue
export default {
  name: 'UserDetail',
  beforeRouteEnter(to, from) {
    // called before the route that renders this component is confirmed.
  },
  beforeRouteUpdate(to, from) {
    // update local state on route params change. eg., `user/1` to `user/2` change
    this.id = to.params.id
  },
  beforeRouteLeave(to, from) {
    const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
    if (!answer) return false
  },
}
```

<div style="page-break-after: always;"></div>
