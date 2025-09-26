# render 函数

## 简介

在 Vue 中，.vue 文件里常见的 `<template>` 模板，本质上最终都会被编译成一个 JavaScript 的渲染函数，也就是 render 函数。

比如：

```html
<template>
  <h1>{{ msg }}</h1>
</template>
```

会被编译成类似这样的函数：

```js
render() {
  return h('h1', this.msg)
}
```

这里的 h 是 createVNode 的别名（Vue 内部提供）。

render 返回的不是实际的 DOM，而是一个 虚拟 DOM 树（VNode Tree）。

## setup 和 render

在 Vue 3 中如果 `setup()` 直接返回一个函数，这个函数就会被当作组件的 `render` 函数。

同一个组件的两种写法

模板写法：

```html
<script setup>
import { ref } from 'vue'

const count = ref(0)
function inc() {
  count.value++
}
</script>

<template>
  <button @click="inc">{{ count }}</button>
</template>
```

`render` 函数写法

```js
import { h, ref } from 'vue'

export default {
  setup() {
    const count = ref(0)
    const inc = () => count.value++

    // 直接返回渲染函数
    return () => h('button', { onClick: inc }, count.value)
  }
}
```
