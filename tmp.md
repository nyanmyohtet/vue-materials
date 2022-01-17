<div style="page-break-after: always;"></div>


***

# Computed Properties and Watchers (pending)

## Computed Properties

## Watcher

***

# Class and Style Bindings (pending)

## Binding HTML Classes

## Binding Inline Styles

***


# Form Input Bindings

## Basic Usage

## Value Bindings

## Modifiers

***

# Components Basics

## v-for with a component

***

# Listening to Child Components Events

***


<div style="page-break-after: always;"></div>

## Named Views

To display **multiple views** at the same time instead of nesting them.

```html
<router-view class="view left-sidebar" name="LeftSidebar"></router-view>
<!-- A router-view without a name will be given `default` as its name. -->
<router-view class="view main-content"></router-view>
<router-view class="view right-sidebar" name="RightSidebar"></router-view>
```

```javascript
// router/index.js
const route = routes: [
  {
    path: '/',
    components: {
      default: Home,
      // short for LeftSidebar: LeftSidebar
      LeftSidebar,
      // they match the `name` attribute on `<router-view>`
      RightSidebar,
    },
  },
]
```

